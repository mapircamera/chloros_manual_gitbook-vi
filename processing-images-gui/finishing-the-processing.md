# Hoàn tất quá trình xử lý

KhiChloros
hoàn tất quá trình xử lý, đã đến lúc bạn xem lại kết quả, kiểm tra chất lượng đầu ra và chuẩn bị các hình ảnh đã xử lý để sử dụng trong quy trình làm việc của mình. Trang này sẽ hướng dẫn bạn thực hiện các bước cuối cùng và các thao tác tiếp theo.

## Dấu hiệu cho biết quá trình xử lý đã hoàn tất

Khi quá trình xử lý kết thúc thành công, bạn sẽ thấy một số dấu hiệu sau:

* ✅ **Thanh tiến trình**: Đạt 100% hoàn thành
* ✅ **Nhật ký gỡ lỗi**: Hiển thị các dòng `[RUN-SUMMARY]` cuối cùng kèm theo số lượng (hình ảnh, nhóm camera, mục tiêu, hình ảnh đã hiệu chuẩn, tệp đã ghi)
* ✅ **Nút Bắt đầu**: Lại được kích hoạt (sẵn sàng cho lần xử lý tiếp theo)
* ✅ **Tệp đầu ra**: Tất cả hình ảnh đã xử lý được lưu vào thư mục đầu ra của dự án (bên dưới)

{% hint style="warning" %}
**Một lần chạy không ghi ra bất kỳ hình ảnh nào được coi là thất bại.** Nếu bạn yêu cầu các sản phẩm hình ảnh nhưng lần chạy không ghi ra bất kỳ sản phẩm nào,Chloros
sẽ báo cáo đó là thất bại — dòng `[RUN-SUMMARY]` trong tên nhật ký gợi ý nguyên nhân có thể (không có dữ liệu nào được nhập vào, không phát hiện được mục tiêu, hoặc mọi sản phẩm được yêu cầu đều bị bỏ qua vì không áp dụng được). Chương trình tương đươngCLI
sẽ thoát với giá trị không bằng 0. Một lần chạy chỉ chứa siêu dữ liệu (tắt tất cả các sản phẩm xuất, không có chỉ mục) vẫn được coi là thành công. Xem [Tài liệu tham khảoCLI
](../reference/cli-reference.md#a-run-that-writes-no-images-fails).
{% endhint %}

***

## Tìm hình ảnh đã xử lý

### Mở thư mục đầu ra

1. Nhấp vào biểu tượng **Menu chính**<img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt="" data-size="line">
(góc trên bên trái)
2. Chọn **&quot;Mở thư mục dự án&quot;**

3. Trình quản lý tệp của bạn sẽ mở ra tại thư mục dự án
4. Tìm dự án của bạn theo tên

### Cấu trúc thư mục đầu ra

Các tệp đầu ra được lưu trữ **trong thư mục dự án, được nhóm theo máy ảnh và sau đó theo định dạng tệp**:

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

* **Thư mục máy ảnh**: `LATT-<sensor>-<lens>-F<filter>` cho LATTICE (trùng khớp với thông tin EXIF của ảnh chụp `Model`), `<model>_<filter>` dành choSurvey3
(ví dụ: `Survey3N_RGN`). Hai máy ảnh cùng sử dụng cảm biến và bộ lọc nhưng khác nhau về ống kính sẽ có các cây thư mục riêng biệt — hiệu ứng viền tối, trường nhìn và độ méo khác nhau.
* **Thư mục định dạng**: tuân theo cài đặt định dạng xuất của bạn — `tiff16`, `tiff8`, `png8`, `jpg8`, hoặc `tiff32` cho dữ liệu phản xạ (TIFF
) (32-bit, Phần trăm). Dữ liệu Radiance luôn có định dạng float32 và luôn được lưu trong thư mục `tiff32`.
* **Thư mục sản phẩm**:
  * `Reflectance_Calibrated_Images/` — độ phản xạ đã hiệu chuẩn
  * `Debayered_Images/` — đã loại bỏ hiệu ứng Bayer tuyến tính (LATTICE)
  * `Preview_Images/` — bản xem trước trên màn hình (LATTICE)
  * `Radiance_Images/` — độ rạng quang phổ float32, W/m²/sr/nm (LATTICE đa quang phổ)
  * `Vignette_Corrected_Images/` **hoặc** `Sensor_Response_Images/` — giá trị dự phòng chưa hiệu chuẩn cho các khung hình không có tham chiếu độ phản xạ; chỉ có đúng một trong hai giá trị này tồn tại cho mỗi lần chạy, được chọn dựa trên cài đặt hiệu chỉnh Vignette
  * `<INDEX>_Index_Images/` — một thư mục cho mỗi chỉ số được chọn (ví dụ: `NDVI_Index_Images`)

{% hint style="info" %}
**Mọi sản phẩm được xuất đều giữ nguyên tên tệp NGUỒN.**Một tệp radiance được xuất từ `capture_..._raw.tif` vẫn được gọi là `capture_..._raw.tif` — chỉ là nó nằm trong thư mục `tiff32/Radiance_Images/`.**Thư mục xác định sản phẩm, không phải tên tệp**, do đó việc tìm kiếm `*radiance*.tif` sẽ không cho kết quả nào; hãy tìm kiếm dựa trên thư mục thay vào đó.
{% endhint %}



<!-- SCREENSHOT-NEEDED: Windows Explorer open on a processed project folder showing the tree: a LATT-… camera folder expanded with tiff16 (Reflectance_Calibrated_Images, Debayered_Images, Preview_Images, NDVI_Index_Images) and tiff32 (Radiance_Images) subfolders visible -->
### Nên có bao nhiêu tệp?

Đừng tính theo công thức — số lượng tệp đầu ra phụ thuộc vào các sản phẩm nào đã được kích hoạt và sản phẩm nào áp dụng cho từng camera (ví dụ: các cameraRGB
không có dữ liệu radiance/reflectance). Số lượng chính xác được ghi trong nhật ký: dòng cuối cùng `[RUN-SUMMARY]` báo cáo chính xác số tệp đã được ghi, và các dòng gợi ý giải thích những gì đã bị bỏ qua.

***

## Xem lại hình ảnh đã xử lý

### Xem trước nhanh trong File Explorer

**Chức năng xem trước tích hợp sẵn trongWindows
:**

1. Truy cập vào thư mục sản phẩm (ví dụ: `tiff16/Reflectance_Calibrated_Images/`)
2. Chọn một tệp hình ảnh
3. Hình ảnh xem trước sẽ hiển thị trong khung xem trước củWindows
Explorer
4. Sử dụng các phím mũi tên để duyệt qua các hình ảnh

### Xem trước trong các trình xem hình ảnh bên ngoài

**Các trình xem được khuyến nghị:*** **QGIS** - Phần mềm GIS miễn phí (phù hợp nhất cho phân tích đa phổ có tham chiếu địa lý)
* **IrfanView** - Trình xem hình ảnh nhanh, nhẹ (hỗ trợ định dạngTIFF
)
* **Adobe Photoshop** - Chỉnh sửa chuyên nghiệp (hỗ trợ định dạngTIFF
)
* **GIMP** - Phần mềm thay thế miễn phí cho Photoshop
* **Windows
Photos** - Xem cơ bản (có thể không hỗ trợ định dạng 16-bitTIFF
)

### Xem trước trong Trình xem hình ảnh củaChloros


Sử dụng Trình xem hình ảnh tích hợp sẵn củaChloros
để xem hình ảnh nâng cao:

1. Nhấp vào hình thu nhỏ của ảnh trong Trình duyệt tệp
2. Ảnh sẽ mở ra trong khu vực xem trước chính
3. Nhấp vào tab **Trình xem ảnh**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
ở thanh bên trái
4. Sử dụng [Index/LUT Sandbox](../image-viewer-gui/index-lut-sandbox.md) để phân tích tương tác

Xem [Trình xem hình ảnh](../image-viewer-gui/opening-an-image-full-screen.md) để biết hướng dẫn chi tiết.

***

## Đọc giá trị phản xạ của pixel (GIS / Pix4D / Tập lệnh)

Độ phản xạ được lưu trữ dưới dạng số nguyên DN, và **giá trị DN tương ứng với ρ = 1.0 phụ thuộc vào máy ảnh nguồn**:

| Nguồn          | ρ = 1.0 là | Cách xác định                                        |
| --------------- | ---------- | -------------------------------------------------- |
| LATTICE (M3C/M3M) | **32768** (dải giá trị lên đến ρ 2.0) | Thẻ XMP `Chloros:PixelScale=32768` được gắn vào tệp |
|Survey3
         | **65535** (bị cắt ở ρ 1.0)     | Không có thẻ XMP `Chloros:*` — sự vắng mặt này chính là tín hiệu |

**Đọc thẻ `Chloros:PixelScale` và chia cho giá trị đó** thay vì giả định một giá trị chung là 65535 — việc chia độ phản xạ LATTICE cho 65535 sẽ âm thầm giảm một nửa mọi giá trị. Một trường hợp ngoại lệ không mang tỷ lệ theo thiết kế: một bản ghi nguồn 8-bit được ghi ra dưới dạng đầu ra 8-bit sẽ bị cắt bớt, không được điều chỉnh tỷ lệ, và cố ý không có thẻ tỷ lệ — hãy xuất lại ở định dạng 16-bit hoặc 32-bit thay vì chia nó. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md) để biết chi tiết đầy đủ.***

## Thông tin siêu dữ liệu được giữ lại trong các tệp xuất

Mỗi sản phẩm đều giữ lại **khối GPS**và**phân vùng EXIF con** của tệp nguồn, do đó, một
tệp xuất sẽ chứa `FocalLength`, `FNumber`, `ExposureTime`, `ISO`, `DateTimeOriginal` và
`CameraSerialNumber` cũng như thông tin tham chiếu địa lý.

{% hint style="warning" %}
**Nếu bản đồ ghép chính xác (orthomosaic) hiển thị ở tỷ lệ không hợp lý, hãy kiểm tra `FocalLength` trước tiên.**
Pix4D tính toán khoảng cách mẫu mặt đất (GSD) dựa trên tiêu cự cộng với độ cao. Nếu thiếu thẻ này,
hệ thống sẽ tự động chuyển về tỷ lệ sai lệch nghiêm trọng — trong một chuyến bay đo đạc gồm 49 ảnh, một vườn cam có kích thước 411 m × 160 m
đã được tái tạo thành 47,8 km × 13 km, tạo ra một ảnh chính quy 455 megapixel chủ yếu là
không gian trống. Quá trình ghép ảnh chậm và tệp tin có dung lượng lớn bất thường là các triệu chứng của vấn đề này, chứ không phải là các
vấn đề riêng biệt.

```bash
exiftool -FocalLength -GPSLatitude "YourProject/.../some_export.tif"
```
{% endhint %}

Không phải *tất cả* các thẻ đều được sao chép. Các thẻ cấu trúc của IFD0 được cố ý bỏ qua (việc sao chép
chúng sẽ làm hỏng đầu ra của LATTICE), và `ExifImageWidth` / `ExifImageHeight` bị loại trừ
vì chúng mô tả bản chụp gốc — nếu không, một tệp xuất đã được thay đổi kích thước sẽ
yêu cầu các kích thước mâu thuẫn với chính raster của nó.

***

## Kiểm tra Nhật ký Gỡ lỗi

### Kiểm tra Cảnh báo hoặc Lỗi

1. Mở tab **Nhật ký gỡ lỗi** (<img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line">
)
2. Cuộn qua các thông báo
3. Tìm các cảnh báo màu vàng hoặc lỗi màu đỏ
4. Đọc các dòng `[RUN-SUMMARY]` và bất kỳ gợi ý nào
5. Liên hệ với bộ phận hỗ trợMAPIR
để được trợ giúp

### Lưu nhật ký

Để lưu lại bản ghi quá trình xử lý hoặc gửi cho bộ phận hỗ trợMAPIR
:

1. Nhấp vào nút **&quot;Sao chép&quot;**hoặc**&quot;Tải xuống&quot;**

2. Lưu dưới dạng tệp văn bản trong thư mục dự án
3. Đính kèm vào tài liệu dự án
4. Gửi cho bộ phận hỗ trợMAPIR
nếu gặp sự cố

***

## Các vấn đề thường gặp về đầu ra và cách khắc phục

### Vấn đề: Thiếu tệp đầu ra

**Nguyên nhân có thể:**

* Sản phẩm không áp dụng cho loại camera đó (ví dụ: độ sáng/độ phản xạ cho cameraRGB
— nhật ký sẽ ghi rõ điều này)
* Thiếu tham chiếu bắt buộc (ví dụ: phản xạ mà không có mục tiêu và không có `.daq` chiếu xuống)
* Hộp kiểm xuất sản phẩm đã bị vô hiệu hóa trong Cài đặt Dự án
* Hết dung lượng đĩa trong quá trình xuất

**Giải pháp:**

1. Kiểm tra các gợi ý `[RUN-SUMMARY]` và các dòng `[EXPORT-CHECK]` trong Nhật ký Gỡ lỗi — chúng giải thích các trường hợp bỏ qua theo từng camera
2. Kiểm tra các ô chọn xuất sản phẩm trong [Cài đặt Dự án](adjusting-project-settings.md)
3. Kiểm tra xem dung lượng đĩa có đủ không
4. Xử lý lại sau khi khắc phục nguyên nhân

### Vấn đề: Viền tối hoặc sáng (hiệu ứng vignetting vẫn còn)

**Nguyên nhân có thể:**

* Tính năng chỉnh sửa hiện tượng tối góc bị tắt
* Camera/ống kính không có trong cơ sở dữ liệu hồ sơ củaChloros

* Hiện tượng tối góc quá mức, vượt quá khả năng chỉnh sửa

**Giải pháp:**

1. Kiểm tra xem tính năng chỉnh sửa hiện tượng tối góc đã được bật trong Cài đặt Dự án chưa
2. Kiểm tra xem mẫu máy ảnh đã được phát hiện chính xác chưa
3. Liên hệ với bộ phận hỗ trợMAPIR
nếu hiện tượng tối viền vẫn tiếp diễn

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

### Vấn đề: Giá trị của Chỉ số Độ tương phản Điểm ảnh (NDVI
) có vẻ không chính xác

**Phạm vi giá trịNDVI
dự kiến:*** **Nước, đá, đất**: -0,1 đến 0,2
* **Thảm thực vật thưa thớt/không khỏe mạnh**: 0,2 đến 0,4
* **Thảm thực vật vừa phải**: 0,4 đến 0,6
* **Thực vật khỏe mạnh, rậm rạp**: 0,6 đến 0,9**Nếu các giá trị nằm ngoài các khoảng này:**

1. Kiểm tra xem đã áp dụng hiệu chuẩn độ phản xạ chưa
2. Kiểm tra xem nhật ký cảm biến ánh sáng đã được bao gồm chưa
3. Kiểm tra xem các mục tiêu hiệu chuẩn đã được phát hiện chưa
4. Đảm bảo đã phát hiện đúng mẫu máy ảnh
5. Xem lại thời điểm và điều kiện chụp ảnh mục tiêu
6. Nếu tự tính toán các chỉ số từ tệp phản xạ, hãy xác nhận rằng bạn đã chia cho giá trị `Chloros:PixelScale` của tệp (xem ở trên)

***

## Sử dụng hình ảnh đã xử lý

### Dành cho Photogrammetry / Tạo bản đồ chính quy

**Quy trình làm việc được khuyến nghị:**

1.**Nhập hình ảnh phản xạ đã hiệu chuẩn** vào phần mềm photogrammetry:
   * Pix4Dmapper
   * Agisoft Metashape
   * DroneDeploy
   * WebODM
2. **Giữ lại siêu dữ liệu EXIF**: Đảm bảo dữ liệu GPS được bảo toàn để gắn thẻ địa lý
3. **Quy trình làm việc đã hiệu chỉnh**: Sử dụng hình ảnh phản xạ để đảm bảo độ chính xác khoa học — phản xạ LATTICE chứa các thẻ hiệu chỉnh XMP mà Pix4D có thể đọc được
4. **Xử lý ảnh ghép chỉ số**: Tạo ảnh ghép trực giao (orthomosaics) từ các hình ảnh chỉ số riêng lẻNDVI

5. **Xuất ảnh ghép đã được định vị địa lý (GeoTIFF
)**: Để sử dụng trong các ứng dụng GIS

### Đối với phân tích GIS

**Quy trình làm việc được khuyến nghị:**

1.**Tải vào QGIS, ArcGIS hoặc các phần mềm tương tự**

2.**Sử dụng hình ảnh phản xạ 16-bitTIFF
** cho phân tích đa dải (chia cho giá trị `Chloros:PixelScale` trong tệp)
3. **Sử dụng hình ảnh chỉ số** (NDVI
,NDRE
) làm các lớp thảm thực vật sẵn sàng sử dụng
4. **Công cụ tính toán raster**: Kết hợp các dải phổ để phân tích tùy chỉnh
5. **Xuất dữ liệu**: Tạo bản đồ phân loại, phát hiện thay đổi, bản đồ sức khỏe thảm thực vật

### Dành cho phân tích trực tiếp / Báo cáo

**Quy trình làm việc được khuyến nghị:**

1.**Sử dụng hình ảnh chỉ số với màu LUT** cho báo cáo trực quan
2. **Trích xuất số liệu thống kê**: Giá trị trung bình của Chỉ số Phân loại Thực vật (NDVI
) cho từng cánh đồng/ô thí nghiệm
3. **Dãy thời gian**: So sánh các chỉ số qua nhiều đợt quan trắc
4. **Tạo báo cáo**: Bao gồm bản đồ, số liệu thống kê và các biểu đồ trực quan***

## Lưu trữ và sao lưu

### Chiến lược sao lưu được khuyến nghị

**Cần lưu gì:*** ✅ **Hình ảnh RAW/JPG gốc hoặc các bản chụp thô LATTICE** - Lưu trữ trên ổ đĩa riêng biệt hoặc đám mây; dữ liệu thô là nguồn đầu vào cho quy trình xử lý và mọi thứ khác có thể được tái tạo từ đó
* ✅ **Tệp cảm biến ánh sáng `.daq` / `.csv`** - Cần thiết để tái tính toán độ phản xạ sau này
* ✅ **Kết quả đã xử lý** - Giữ lại hình ảnh đã hiệu chuẩn và các chỉ số
* ✅ **Thư mục dự án** (`project.json` và các tệp liên quan) - Chứa tất cả cài đặt để xử lý lại nếu cần
* ✅ **Nhật ký gỡ lỗi** - Ghi lại chi tiết quá trình xử lý
* ✅ **Hình ảnh mục tiêu hiệu chuẩn** - Dùng để xác minh và xử lý lại**Khuyến nghị lưu trữ:*** **Sao lưu ngay lập tức**: Ổ cứng ngoài
* **Lưu trữ dài hạn**: Lưu trữ đám mây (Google Drive, Dropbox, v.v.)
* **Dữ liệu quan trọng**: Lưu giữ 2-3 bản sao tại các vị trí khác nhau***

## Các lần xử lý tiếp theo

### Tái sử dụng cài đặt dự án

Nếu xử lý các tập dữ liệu tương tự trong tương lai:

1. **Lưu mẫu dự án** (nếu chưa thực hiện)
2. **Tạo dự án mới** bằng mẫu đã lưu
3. **Nhập hình ảnh mới**

4.**Xử lý**với các cài đặt giống hệt nhau để đảm bảo tính nhất quán

### Xử lý hàng loạt nhiều phiên

Đối với nhiều phiên/bộ dữ liệu:**Tùy chọn 1: Giao diện người dùng đồ họa (GUI) - Nhiều dự án**

* Tạo dự án riêng biệt cho từng phiên
* Sử dụng các cài đặt mẫu nhất quán
* Xử lý từng dự án một

**Tùy chọn 2:Chloros
CLI
(chỉ dành choChloros
+)**

* Tự động hóa xử lý hàng loạt
* Xử lý nhiều thư mục bằng tập lệnh
* Xem [CLI
Tài liệu](../CLI.md) và [CLI
Tài liệu tham khảo](../reference/cli-reference.md)

**Tùy chọn 3:Python
SDK
(chỉ dành choChloros
+)**

* Điều khiển bằng lập trình
* Tích hợp với các quy trình phân tích
* Xem [API
Tài liệu](../api-python-sdk.md) và [SDK
Tài liệu tham khảo](../reference/sdk-reference.md)

***

## Khắc phục sự cố trong quá trình xử lý sau

### Xử lý lại với các thiết lập khác

Nếu kết quả không như mong đợi:

1. Giữ lại hình ảnh gốc (không bao giờ xóa)
2. Mở cùng một dự án trongChloros
3. Điều chỉnh cài đặt trong bảng Cài đặt dự án
4. Xử lý lại — kết quả sẽ được lưu vào cùng các thư mục sản phẩm, do đó các tệp có cùng tên từ lần chạy trước sẽ bị thay thế

### Xử lý một tập con hình ảnh

Để xử lý lại chỉ các hình ảnh cụ thể:

1. Tạo dự án mới
2. Chỉ nhập các hình ảnh cần xử lý lại
3. Sử dụng cùng mẫu cài đặt
4. Xử lý tập dữ liệu nhỏ hơn

### Nhận hỗ trợ

Nếu bạn gặp sự cố:

* 📧 **Email**: info@mapir.camera (kèm theo Nhật ký gỡ lỗi)
* 🌐 **Hỗ trợ**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Câu hỏi thường gặp**: [Câu hỏi thường gặp](../faq.md)
* 📖 **Tài liệu**: [Hướng dẫn sử dụngChloros
](../)

***

## Tóm tắt: Quy trình làm việc hoàn chỉnh

Bây giờ bạn đã hoàn tất toàn bộ quy trình xử lýChloros
:

1. ✅ **Tạo dự án** - Xem [Dự án](../projects.md)
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

### Tự động hóa &amp; Tích hợp

* [**Tài liệuCLI**](../CLI.md) - Xử lý hàng loạt qua dòng lệnh
* [**SDK
Python
**](../api-python-sdk.md) - Tự động hóa lập trình
* [**Chloros
+ Tính năng**](../#chloros) - Khả năng xử lý nâng cao

### Hỗ trợ &amp; Học tập

* [**Câu hỏi thường gặp (FAQ)**](../faq.md) - Trả lời các câu hỏi thường gặp
* [**Mục tiêu hiệu chuẩn**](../calibration-targets.md) - Hiểu về hiệu chuẩn độ phản xạ
* [**Máy ảnh được hỗ trợ**](../supported-cameras.md) - Phần cứng tương thích
