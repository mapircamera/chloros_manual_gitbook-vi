# Hoàn tất quá trình xử lý

Khi Chloros hoàn tất quá trình xử lý, đã đến lúc xem lại kết quả, kiểm tra chất lượng đầu ra và chuẩn bị các hình ảnh đã được xử lý để sử dụng trong quy trình làm việc của bạn. Trang này sẽ hướng dẫn bạn thực hiện các bước cuối cùng và các thao tác tiếp theo.

## Dấu hiệu cho biết quá trình xử lý đã hoàn tất

Khi quá trình xử lý kết thúc thành công, bạn sẽ thấy một số dấu hiệu sau:

* ✅ **Thanh tiến trình**: Đạt 100% hoàn tất
* ✅ **Nhật ký gỡ lỗi**: Hiển thị các dòng `[RUN-SUMMARY]` cuối cùng kèm theo số lượng (hình ảnh, nhóm camera, mục tiêu, hình ảnh đã hiệu chuẩn, tệp đã ghi)
* ✅ **Nút Bắt đầu**: Lại được kích hoạt (sẵn sàng cho lần xử lý tiếp theo)
* ✅ **Tệp đầu ra**: Tất cả hình ảnh đã xử lý được lưu vào thư mục đầu ra của dự án (bên dưới)

{% hint style="warning" %}
**Một lần chạy không ghi ra bất kỳ hình ảnh nào được coi là thất bại.** Nếu bạn yêu cầu các sản phẩm hình ảnh nhưng lần chạy không ghi lại bất kỳ hình ảnh nào, Chloros sẽ báo cáo đó là một lần chạy thất bại — tên nhật ký `[RUN-SUMMARY]` gợi ý nguyên nhân có thể (không có dữ liệu nào được nhập, không phát hiện mục tiêu, hoặc mọi sản phẩm được yêu cầu đều bị bỏ qua vì không áp dụng được). Phiên bản CLI tương ứng sẽ thoát với giá trị không bằng 0. Một lần chạy chỉ metadata theo ý định (tắt tất cả sản phẩm xuất, không có chỉ mục) vẫn được coi là thành công. Xem [Tài liệu tham khảo CLI](../reference/cli-reference.md#a-run-that-writes-no-images-fails).
{% endhint %}

***

## Tìm kiếm hình ảnh đã xử lý

### Mở thư mục đầu ra

1. Nhấp vào biểu tượng **Menu chính** <img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt="" data-size="line">
(góc trên bên trái)
2. Chọn **"Mở thư mục dự án"**

3. Trình quản lý tệp của bạn sẽ mở ra tại thư mục dự án
4. Tìm dự án của bạn theo tên

### Cấu trúc thư mục đầu ra

Các sản phẩm được lưu **trong thư mục dự án, được nhóm theo máy ảnh và sau đó theo định dạng tệp**:

```
<project>/
└── LATT-M3M-L41-F550/                  # one folder per camera
    ├── tiff16/
    │   ├── Reflectance_Calibrated_Images/
    │   ├── Debayered_Images/
    │   ├── Preview_Images/
    │   └── NDVI_Index_Images/           # one folder per selected index
    └── tiff32/
        └── Radiance_Images/             # float32 radiance always lands here
```

* **Thư mục máy ảnh**: `LATT-<sensor>-<lens>-F<filter>` cho LATTICE (phù hợp với EXIF của ảnh chụp `Model`), `<model>_<filter>` dành cho Survey3 (ví dụ: `Survey3N_RGN`). Hai máy ảnh dùng chung cảm biến và bộ lọc nhưng có ống kính khác nhau sẽ có các cây thư mục riêng biệt — hiệu ứng viền tối, trường nhìn và độ méo khác nhau.
* **Thư mục định dạng**: tuân theo cài đặt định dạng xuất của bạn — `tiff16`, `tiff8`, `png8`, `jpg8`, hoặc `tiff32` cho định dạng TIFF (32-bit, Phần trăm). Độ sáng (Radiance) luôn là float32 và luôn được lưu trong thư mục `tiff32`.
* **Thư mục sản phẩm**:
  * `Reflectance_Calibrated_Images/` — độ phản xạ đã hiệu chuẩn
  * `Debayered_Images/` — đã loại bỏ hiệu ứng bayering tuyến tính (LATTICE): đã loại bỏ độ lệch tối và khôi phục pixel, không sử dụng hình ảnh phẳng hoặc tách kênh, theo hướng gốc của cảm biến
  * `Preview_Images/` — bản xem trước trên màn hình (LATTICE)
  * `Radiance_Images/` — độ rạng quang phổ float32, W/m²/sr/nm (đa quang phổ LATTICE)
  * `Vignette_Corrected_Images/` **hoặc** `Sensor_Response_Images/` — tùy chọn dự phòng chưa hiệu chuẩn dành cho các khung hình không có tham chiếu phản xạ; chỉ có đúng một trong hai tùy chọn này tồn tại cho mỗi lần chạy, được chọn dựa trên cài đặt chỉnh sửa hiệu ứng viền tối
  * `<INDEX>_Index_Images/` — một thư mục cho mỗi chỉ số đã chọn (ví dụ: `NDVI_Index_Images`)
* **`clamp_masks/`** (mới trong phiên bản 1.3.0, chỉ dành cho máy ảnh đa phổ LATTICE M3C): xuất hiện trong thư mục định dạng, bên cạnh các thư mục sản phẩm, khi quá trình tách dải phổ cắt các giá trị âm xuống 0 trong sản phẩm độ rạng rỡ hoặc độ phản xạ. Thư mục này chứa một mặt nạ cho mỗi pixel trong mỗi sản phẩm bị ảnh hưởng, ví dụ: `tiff16/clamp_masks/Reflectance_Calibrated_Images/<name>_clampmask.tif`. Các mặt nạ không bao giờ nằm trong thư mục sản phẩm, do đó thư mục sản phẩm có thể được chuyển trực tiếp sang phần mềm quang trắc học. Xem [Mặt nạ Clamp](../output-image-formats.md#clamp-masks).

{% hint style="info" %}
**Mỗi sản phẩm được xuất đều giữ nguyên tên tệp NGUỒN.**Một sản phẩm radiance được xuất từ `capture_..._raw.tif` vẫn được gọi là `capture_..._raw.tif` — nó chỉ nằm trong thư mục `tiff32/Radiance_Images/`.**Thư mục xác định sản phẩm, không phải tên tệp**, do đó việc tìm kiếm `*radiance*.tif` sẽ không cho kết quả nào; hãy tìm kiếm dựa trên thư mục thay vào đó.
{% endhint %}

<!-- SCREENSHOT-NEEDED: Windows Explorer open on a processed project folder showing the tree: a LATT-M3C multispectral camera folder expanded with tiff16 (Reflectance_Calibrated_Images, Debayered_Images, Preview_Images, NDVI_Index_Images, clamp_masks) and tiff32 (Radiance_Images, clamp_masks) subfolders visible -->


### Nên có bao nhiêu tệp?

Đừng tính theo công thức — số lượng đầu ra phụ thuộc vào sản phẩm nào đã được kích hoạt và sản phẩm nào áp dụng cho từng camera (ví dụ: camera RGB không có radiance/reflectance). Số lượng chính xác được ghi trong nhật ký: dòng cuối cùng `[RUN-SUMMARY]` báo cáo chính xác số lượng tệp đã được ghi, và các dòng gợi ý giải thích bất kỳ nội dung nào bị bỏ qua.

***

## Xem lại hình ảnh đã xử lý

### Xem trước nhanh trong File Explorer

**Tính năng xem trước tích hợp sẵn của Windows:**

1. Truy cập vào thư mục sản phẩm (ví dụ: `tiff16/Reflectance_Calibrated_Images/`)
2. Chọn một tệp hình ảnh
3. Hình xem trước sẽ xuất hiện trong khung xem trước củWindows Explorer
4. Sử dụng các phím mũi tên để duyệt qua các hình ảnh

### Xem trước trong các trình xem hình ảnh bên ngoài

**Các trình xem được khuyến nghị:**

* **QGIS** - Phần mềm GIS miễn phí (phù hợp nhất cho phân tích đa phổ có tham chiếu địa lý)
* **IrfanView** - Trình xem hình ảnh nhanh, nhẹ (hỗ trợ định dạng TIFF)
* **Adobe Photoshop** - Chỉnh sửa chuyên nghiệp (hỗ trợ định dạng TIFF)
* **GIMP** - Phần mềm thay thế miễn phí cho Photoshop
* **Windows Photos** - Xem cơ bản (có thể không hỗ trợ định dạng TIFF 16-bit)

### Xem trước trong Trình xem hình ảnh Chloros


Sử dụng Trình xem hình ảnh tích hợp sẵn của Chloros để hiển thị nâng cao:

1. Nhấp vào hình thu nhỏ của hình ảnh trong Trình duyệt tệp
2. Hình ảnh sẽ mở ra trong khu vực xem trước chính
3. Nhấp vào tab **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
ở thanh bên trái
4. Sử dụng [Index/LUT Sandbox](../image-viewer-gui/index-lut-sandbox.md) để phân tích tương tác

Xem [Trình xem hình ảnh](../image-viewer-gui/opening-an-image-full-screen.md) để biết hướng dẫn chi tiết.

***

## Đọc giá trị phản xạ của pixel (GIS / Pix4D / Tập lệnh)

Độ phản xạ được lưu trữ dưới dạng số nguyên DN, và **giá trị DN tương ứng với ρ = 1.0 phụ thuộc vào máy ảnh nguồn**:

| Nguồn          | ρ = 1.0 là | Cách xác định                                        |
| --------------- | ---------- | -------------------------------------------------- |
| LATTICE (M3C/M3M) | **32768** (dải giá trị lên đến ρ 2.0) | Thẻ XMP `Chloros:PixelScale=32768` được gắn vào tệp |
| Survey3          | **65535** (bị cắt tại ρ 1.0)     | Không có thẻ `Chloros:PixelScale` — sự vắng mặt này chính là dấu hiệu (các sản phẩm Survey3 vẫn chứa các thẻ xử lý `Chloros:*` khác) |

**Đọc thẻ `Chloros:PixelScale` và chia cho giá trị đó** thay vì giả định một giá trị cố định là 65535 — việc chia độ phản xạ LATTICE cho 65535 sẽ âm thầm giảm một nửa mọi giá trị. Một tệp độ phản xạ từ phiên bản trước 1.3.0 được ghi dưới dạng 8-bit đã bị cắt bớt, không được điều chỉnh tỷ lệ, và không có thẻ tỷ lệ: hãy xuất lại tệp đó ở định dạng 16-bit hoặc 32-bit thay vì chia nó. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md) để biết chi tiết đầy đủ.

***

## Thông tin siêu dữ liệu được giữ lại trong các tệp xuất

Mỗi sản phẩm đều giữ lại **khối GPS**và**phần tử con EXIF** của dữ liệu nguồn, do đó,
tệp xuất sẽ chứa `FocalLength`, `FNumber`, `ExposureTime`, `ISO`, `DateTimeOriginal` và
`CameraSerialNumber` cũng như thông tin tham chiếu địa lý.

{% hint style="warning" %}
**Nếu bản đồ ghép chính xác (orthomosaic) hiển thị ở tỷ lệ không hợp lý, hãy kiểm tra `FocalLength` trước tiên.**
Pix4D tính toán khoảng cách mẫu mặt đất (GSD) dựa trên tiêu cự cộng với độ cao. Nếu thiếu thẻ này,
phần mềm sẽ tự động điều chỉnh về tỷ lệ sai lệch nghiêm trọng — trong một chuyến bay đo đạc gồm 49 khung hình, một vườn cam có kích thước 411 m × 160 m
được tái tạo thành 47,8 km × 13 km, tạo ra một ảnh chính quy 455 megapixel chủ yếu là
không gian trống. Quá trình ghép ảnh chậm và tệp tin có dung lượng lớn bất ngờ là các triệu chứng của vấn đề này, chứ không phải là các
vấn đề riêng biệt.

```bash
exiftool -FocalLength -GPSLatitude "YourProject/.../some_export.tif"
```
{% endhint %}

Không phải *tất cả* các thẻ đều được sao chép. Các thẻ cấu trúc của IFD0 được cố ý bỏ lại (việc sao chép
chúng sẽ làm hỏng đầu ra của LATTICE), và `ExifImageWidth` / `ExifImageHeight` bị loại trừ
vì chúng mô tả bản chụp gốc — nếu không, một tệp xuất đã được thay đổi kích thước sẽ
yêu cầu các kích thước mâu thuẫn với raster của chính nó.

Chloros cũng ghi các thẻ XMP `Chloros:*` riêng của mình để mô tả những thao tác đã thực hiện trên từng sản phẩm
(ví dụ: `Chloros:ProcessingLevel`, `Chloros:CalibrationApplied`,
`Chloros:VignetteCorrected`). Kể từ phiên bản 1.3.0, các sản phẩm Survey3 đều chứa các thẻ này trong mọi chế độ xử lý, các sản phẩm chỉ mục mô tả chính chỉ mục đó (`Chloros:ProcessingLevel=index`,
công thức, và liệu nó được tính toán từ độ phản xạ hay độ sáng) thay vì lặp lại các
thẻ của dữ liệu thô nguồn, và các sản phẩm trong các thư mục có tên dài hơn 260 ký tự cũng giữ nguyên siêu dữ liệu
của chúng. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md#index-products).

***

## Kiểm tra nhật ký gỡ lỗi

### Kiểm tra cảnh báo hoặc lỗi

1. Mở tab **Nhật ký gỡ lỗi** <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line">

2. Cuộn qua các thông báo
3. Tìm các cảnh báo màu vàng hoặc lỗi màu đỏ
4. Đọc các dòng `[RUN-SUMMARY]` và bất kỳ gợi ý nào
5. Liên hệ với bộ phận hỗ trợ MAPIR để được trợ giúp

### Lưu nhật ký

Để lưu lại bản ghi quá trình xử lý hoặc gửi cho bộ phận hỗ trợ MAPIR:

1. Nhấp vào nút **"Sao chép"**hoặc**"Tải xuống"**

2. Lưu dưới dạng tệp văn bản trong thư mục dự án
3. Đính kèm vào tài liệu dự án
4. Gửi cho bộ phận hỗ trợ MAPIR nếu gặp sự cố

Để gửi vấn đề trực tiếp đến MAPIR, hãy sử dụng biểu tượng lỗi (**Báo cáo lỗi**) ở đầu tab Nhật ký gỡ lỗi — xem [Báo cáo lỗi từ tab nhật ký](monitoring-the-processing.md#reporting-a-bug-from-the-log-tab).

***

## Các sự cố đầu ra thường gặp và cách khắc phục

### Sự cố: Thiếu tệp đầu ra

**Nguyên nhân có thể:**

* Sản phẩm không áp dụng cho loại camera đó (ví dụ: độ rạng rỡ/độ phản xạ cho camera RGB — nhật ký đã ghi rõ điều này)
* Thiếu tham chiếu bắt buộc (ví dụ: phản xạ không có mục tiêu và không có `.daq` chiếu xuống)
* Hộp kiểm xuất sản phẩm bị vô hiệu hóa trong Cài đặt Dự án
* Hết dung lượng đĩa trong quá trình xuất

**Giải pháp:**

1. Kiểm tra các gợi ý `[RUN-SUMMARY]` và các dòng `[EXPORT-CHECK]` trong Nhật ký gỡ lỗi — chúng giải thích các trường hợp bỏ qua theo từng camera
2. Kiểm tra lại các ô chọn xuất sản phẩm trong [Cài đặt Dự án](adjusting-project-settings.md)
3. Kiểm tra xem dung lượng đĩa có đủ không
4. Xử lý lại sau khi khắc phục nguyên nhân

### Vấn đề: Viền tối hoặc sáng (hiệu ứng vignetting vẫn còn)

**Nguyên nhân có thể:**

* Chức năng chỉnh sửa hiện tượng tối viền bị tắt
* Camera/ống kính không có trong cơ sở dữ liệu cấu hình Chloros

* Hiện tượng tối viền quá nghiêm trọng, vượt quá khả năng chỉnh sửa

**Giải pháp:**

1. Kiểm tra xem chức năng chỉnh sửa hiện tượng tối viền đã được bật trong Cài đặt Dự án chưa
2. Kiểm tra xem mẫu camera đã được nhận diện chính xác chưa
3. Liên hệ bộ phận hỗ trợ MAPIR nếu hiện tượng tối viền vẫn còn

### Vấn đề: Màu sắc hoặc giá trị không chính xác

**Nguyên nhân có thể:**

* Không phát hiện được mục tiêu hiệu chuẩn
* Chọn sai mẫu mục tiêu hiệu chuẩn
* Tính năng hiệu chuẩn phản xạ bị tắt
* Hình ảnh mục tiêu có chất lượng kém

**Giải pháp:**

1. Kiểm tra xem tính năng hiệu chuẩn phản xạ đã được bật chưa
2. Kiểm tra các thông báo “Đã tìm thấy mục tiêu” trong Nhật ký gỡ lỗi
3. Kiểm tra chất lượng hình ảnh mục tiêu
4. Xử lý lại với các mục tiêu được đánh dấu chính xác

### Vấn đề: Giá trị NDVI có vẻ không chính xác

**Phạm vi NDVI dự kiến:**

* **Nước, đá, đất**: -0,1 đến 0,2
* **Thực vật thưa thớt/không khỏe mạnh**: 0,2 đến 0,4
* **Thực vật vừa phải**: 0,4 đến 0,6
* **Thực vật khỏe mạnh, rậm rạp**: 0,6 đến 0,9**Nếu các giá trị nằm ngoài các khoảng này:**

1. Kiểm tra xem đã áp dụng hiệu chuẩn phản xạ chưa
2. Kiểm tra xem nhật ký cảm biến ánh sáng đã được bao gồm chưa
3. Kiểm tra xem các mục tiêu hiệu chuẩn đã được phát hiện chưa
4. Đảm bảo đã phát hiện đúng mẫu máy ảnh
5. Kiểm tra thời điểm và điều kiện chụp ảnh mục tiêu
6. Nếu tự tính toán các chỉ số từ tệp phản xạ, hãy xác nhận rằng bạn đã chia cho giá trị `Chloros:PixelScale` của tệp (xem ở trên)

***

## Sử dụng hình ảnh đã xử lý

### Dành cho Photogrammetry / Tạo bản đồ chính xác

**Quy trình làm việc được khuyến nghị:**

1.**Nhập hình ảnh phản xạ đã hiệu chuẩn** vào phần mềm photogrammetry:
   * Pix4Dmapper
   * Agisoft Metashape
   * DroneDeploy
   * WebODM
2. **Giữ nguyên siêu dữ liệu EXIF**: Đảm bảo dữ liệu GPS được bảo toàn để gắn thẻ địa lý
3. **Quy trình làm việc đã hiệu chỉnh**: Sử dụng hình ảnh phản xạ để đảm bảo độ chính xác khoa học — Hình ảnh phản xạ LATTICE chứa các thẻ hiệu chỉnh XMP mà Pix4D có thể đọc
4. **Xử lý ảnh ghép chỉ số**: Tạo ảnh ghép chính quy NDVI từ các hình ảnh chỉ số riêng lẻ
5. **Xuất tệp GeoTIFF có tham chiếu địa lý**: Để sử dụng trong các ứng dụng GIS

### Đối với phân tích GIS

**Quy trình làm việc được khuyến nghị:**

1.**Tải vào QGIS, ArcGIS hoặc các phần mềm tương tự**

2.**Sử dụng hình ảnh phản xạ định dạng TIFF 16-bit** cho phân tích đa dải (chia cho giá trị `Chloros:PixelScale` trong tệp)
3. **Sử dụng hình ảnh chỉ số** (NDVI, NDRE) làm các lớp thảm thực vật sẵn sàng sử dụng
4. **Công cụ tính toán raster**: Kết hợp các dải phổ để phân tích tùy chỉnh
5. **Xuất dữ liệu**: Tạo bản đồ phân loại, phát hiện thay đổi, bản đồ sức khỏe thảm thực vật

### Dành cho phân tích trực tiếp / Báo cáo

**Quy trình làm việc được khuyến nghị:**

1.**Sử dụng hình ảnh chỉ số với màu sắc LUT** để tạo báo cáo trực quan
2. **Trích xuất số liệu thống kê**: Giá trị trung bình NDVI theo từng cánh đồng/ô thí nghiệm
3. **Dãy thời gian**: So sánh các chỉ số qua nhiều đợt quan trắc
4. **Tạo báo cáo**: Bao gồm bản đồ, số liệu thống kê và các biểu đồ trực quan***

## Lưu trữ và sao lưu

### Chiến lược sao lưu được khuyến nghị

**Cần lưu gì:**

* ✅ **Hình ảnh RAW/JPG gốc hoặc dữ liệu thô LATTICE** - Lưu trữ trên ổ đĩa riêng biệt hoặc đám mây; dữ liệu thô là nguồn đầu vào của quy trình xử lý và mọi thứ khác đều có thể được tái tạo từ đó
* ✅ **Tệp cảm biến ánh sáng `.daq` / `.csv`** - Cần thiết để tính lại hệ số phản xạ sau này
* ✅ **Kết quả đã xử lý** - Giữ lại hình ảnh và chỉ số đã hiệu chuẩn
* ✅ **Thư mục dự án** (`project.json` và các tệp liên quan) - Chứa tất cả cài đặt để xử lý lại nếu cần
* ✅ **Nhật ký gỡ lỗi** - Ghi lại chi tiết quá trình xử lý
* ✅ **Hình ảnh mục tiêu hiệu chuẩn** - Dùng để xác minh và xử lý lại

**Khuyến nghị về lưu trữ:**

* **Sao lưu ngay lập tức**: Ổ cứng ngoài
* **Lưu trữ dài hạn**: Lưu trữ đám mây (Google Drive, Dropbox, v.v.)
* **Dữ liệu quan trọng**: Giữ 2-3 bản sao tại các vị trí khác nhau***

## Các lần xử lý tiếp theo

### Tái sử dụng cài đặt dự án

Nếu xử lý các bộ dữ liệu tương tự trong tương lai:

1. **Lưu mẫu dự án** (nếu chưa thực hiện)
2. **Tạo dự án mới** bằng mẫu đã lưu
3. **Nhập hình ảnh mới**

4.**Xử lý**với các cài đặt giống hệt nhau để đảm bảo tính nhất quán

### Xử lý hàng loạt nhiều phiên

Đối với nhiều phiên/bộ dữ liệu:**Tùy chọn 1: Giao diện người dùng (GUI) - Nhiều dự án**

* Tạo dự án riêng biệt cho từng phiên
* Sử dụng cài đặt mẫu nhất quán
* Xử lý từng dự án một

**Tùy chọn 2: Chloros CLI (chỉ dành cho Chloros+)**

* Tự động hóa xử lý hàng loạt
* Xử lý nhiều thư mục bằng tập lệnh
* Xem [Tài liệu CLI](../CLI.md) và [Tham chiếu CLI](../reference/cli-reference.md)

**Tùy chọn 3: Python SDK (chỉ dành cho Chloros+)**

* Điều khiển bằng lập trình
* Tích hợp với các quy trình phân tích
* Xem [Tài liệu API](../api-python-sdk.md) và [Tham chiếu SDK](../reference/sdk-reference.md)

***

## Khắc phục sự cố sau xử lý

### Xử lý lại với các thiết lập khác

Nếu kết quả không như mong đợi:

1. Giữ lại hình ảnh gốc (không bao giờ xóa)
2. Mở cùng một dự án trong Chloros
3. Điều chỉnh thiết lập trong bảng Cài đặt Dự án
4. Xử lý lại — kết quả sẽ được lưu vào cùng các thư mục sản phẩm, do đó các tệp có cùng tên từ lần chạy trước sẽ bị thay thế

### Xử lý một tập con hình ảnh

Để xử lý lại chỉ các hình ảnh cụ thể:

1. Tạo dự án mới
2. Chỉ nhập các hình ảnh cần xử lý lại
3. Sử dụng cùng mẫu cài đặt
4. Xử lý tập dữ liệu nhỏ hơn

### Nhận hỗ trợ

Nếu bạn gặp sự cố:

* 📧 **Email**: info@mapir.camera (đính kèm Nhật ký gỡ lỗi)
* 🌐 **Hỗ trợ**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Câu hỏi thường gặp**: [Câu hỏi thường gặp](../faq.md)
* 📖 **Tài liệu hướng dẫn**: [Hướng dẫn sử dụng Chloros](../)

***

## Tóm tắt: Quy trình làm việc hoàn chỉnh

Bạn đã hoàn thành toàn bộ quy trình xử lý Chloros:

1. ✅ **Đã tạo dự án** - Xem [Dự án](../projects.md)
2. ✅ **Thêm tệp** - Xem [Thêm tệp](adding-files-to-a-project.md)
3. ✅ **Điều chỉnh cài đặt** - Xem [Điều chỉnh cài đặt dự án](adjusting-project-settings.md)
4. ✅ **Đánh dấu mục tiêu** - Xem [Chọn hình ảnh mục tiêu](choosing-target-images.md)
5. ✅ **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
6. ✅ **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)
7. ✅ **Xem lại kết quả** - Trang này**Các hình ảnh đa phổ đã được hiệu chuẩn và hiệu chỉnh độ phản xạ của bạn đã sẵn sàng để phân tích!**

***

## Tài nguyên bổ sung

### Tính năng nâng cao

* [**Trình xem hình ảnh**](../image-viewer-gui/opening-an-image-full-screen.md) - Trực quan hóa và phân tích tương tác
* [**Môi trường thử nghiệm chỉ số/LUT**](../image-viewer-gui/index-lut-sandbox.md) - Thử nghiệm chỉ số tùy chỉnh
* [**Công thức chỉ số đa phổ**](../project-settings/multispectral-index-formulas.md) - Tài liệu tham khảo đầy đủ về chỉ số

### Tự động hóa & Tích hợp

* [**Tài liệu CLI**](../CLI.md) - Xử lý hàng loạt qua dòng lệnh
* [**Python SDK**](../api-python-sdk.md) - Tự động hóa bằng lập trình
* [**Các tính năng của Chloros+**](../#chloros) - Khả năng xử lý nâng cao

### Hỗ trợ & Học tập

* [**Câu hỏi thường gặp (FAQ)**](../faq.md) - Trả lời các câu hỏi phổ biến
* [**Mục tiêu hiệu chuẩn**](../calibration-targets.md) - Hiểu về hiệu chuẩn độ phản xạ
* [**Máy ảnh được hỗ trợ**](../supported-cameras.md) - Phần cứng tương thích
