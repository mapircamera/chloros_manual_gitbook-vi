# Các lớp hình ảnh

**Menu thả xuống &quot;Lớp&quot;** ở góc trên bên phải của Trình xem hình ảnh cho phép bạn chuyển đổi giữa các phiên bản khác nhau của hình ảnh đang xem — từ hình ảnh gốc cho đến từng sản phẩm đã qua xử lý và các hình ảnh chỉ mục được tính toán — mà không cần thoát khỏi trình xem.

## Lớp hình ảnh là gì?

Một &quot;lớp&quot; trongChloros

là một **tệp sản phẩm**được đăng ký dựa trên một hình ảnh nguồn. Quá trình nhập dữ liệu cung cấp cho bạn các tệp nguồn; quá trình xử lý sẽ thêm một lớp cho mỗi sản phẩm được tạo ra trong lần chạy đó. Các tệp được xuất vẫn giữ tên tệp nguồn — chính**thư mục** là yếu tố xác định sản phẩm, và tên lớp chính là nhãn màChloros

gán cho thư mục đó.

<!-- SCREENSHOT-NEEDED: Image Viewer full screen with the layer dropdown open on a processed LATTICE multispectral image, showing the full list: TIFF base, RAW (Original), RAW (Debayered), RAW (Preview), RAW (Radiance), RAW (Reflectance), and one RAW (NDVI Index) entry. -->

***

## Danh sách lớp

### Luôn hiển thị

| Lớp | Là gì |
| --- | --- |
| **JPG**(hoặc**PNG

**/**TIFF

**) | Tệp gốc được nhập cùng với dữ liệu chụp.Survey3

nhập tệp `.JPG` bên cạnh mỗi tệp `.RAW`; dữ liệu chụp từ LATTICE mang theo bản xem trước hiển thịPNG

hoặcTIFF

. Được gắn nhãn theo những gì thực sự được nhập |
| **RAW (Gốc)** | Khung hình thô nguồn, đã được khử hiệu ứng Bayer để hiển thị mà không áp dụng bất kỳ chỉnh sửa nào. Có sẵn ngay từ khi nhập — không cần xử lý |

Một bản chụp LATTICE có tệp gốc **chính là** khung hình RAW của nó sẽ không có mục gốc riêng biệt: `RAW (Original)` đã bao gồm nó.

### Sản phẩm xử lý từ dữ liệu tham chiếu (Survey3

)

| Lớp | Được ghi vào | Tồn tại khi |
| --- | --- | --- |
| **RAW (Mục tiêu)** | — | Khung hình được xác định là chứa mục tiêu hiệu chuẩn |
| **RAW (Độ phản xạ)** | `Reflectance_Calibrated_Images/` | Quá trình hiệu chuẩn độ phản xạ đã diễn ra thành công trên khung hình này |
| **Đã hiệu chỉnh hiện tượng tối góc**| `Vignette_Corrected_Images/` | Khung hình không thể hiệu chuẩn phản xạ**và** *chức năng hiệu chỉnh hiện tượng tối góc* đang bật |
| **Phản ứng cảm biến**| `Sensor_Response_Images/` | Không thể hiệu chuẩn độ phản xạ cho khung hình này**và** *chức năng chỉnh hiệu ứng viền tối* đã bị tắt |
| **Đã cân bằng trắng** | `White_Balanced_Images/` | Đã tạo sản phẩm đã cân bằng trắng |

{% hint style="info" %}
**Chức năng &quot;Chỉnh sửa hiện tượng tối góc&quot; và &quot;Phản hồi cảm biến&quot; là hai tùy chọn thay thế cho nhau, không bao giờ sử dụng cả hai cùng lúc.** Chỉ có đúng một sản phẩm dự phòng chưa được hiệu chỉnh cho mỗi lần chạy, đối với mỗi mẫu máy ảnh, và công tắc *Chỉnh sửa hiện tượng tối góc* sẽ chọn sản phẩm đó. Xem [Cài đặt Dự án](../project-settings/project-settings.md).
{% endhint %}

### Các cấp độ LATTICE

LATTICE ghi lại các cấp độ này trong một lần xử lý duy nhất. Các cấp độ nào tồn tại phụ thuộc vào các tùy chọn xuất riêng cho từng sản phẩm trong Cài đặt Dự án và vào những gì áp dụng cho máy ảnh.

| Lớp | Ghi vào | Áp dụng cho |
| --- | --- | --- |
| **RAW (Đã loại bỏ hiệu ứng Bayer)** | `Debayered_Images/` | Máy ảnhRGB

và đa quang phổ |
| **RAW (Xem trước)** | `Preview_Images/` | Đa quang phổ (kéo dãn màu giả) |
| **Cân bằng trắng** | `Preview_Images/` | Máy ảnh chính củaRGB

— bản xem trước củaRGB

được đăng ký dưới tên này để khớp với lớpSurvey3

cùng tên |
| **RAW (Bức xạ)** | `Radiance_Images/` | Chỉ đa quang phổ |
| **RAW (Độ phản xạ)** | `Reflectance_Calibrated_Images/` | Chỉ đa quang phổ, và chỉ khi có bản ghi hướng xuống tương ứng `.daq` hoặc mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) bao phủ khung hình |

RGB

các camera chính không có đo quang phổ theo dải, do đó độ rạng rỡ và độ phản xạ sẽ bị bỏ qua đối với chúng với lý do **không áp dụng** — nhật ký sẽ ghi rõ điều này thay vì báo lỗi im lặng.

### Các lớp chỉ số, LUT và sandbox

| Mẫu lớp | Ví dụ | Nguồn gốc |
| --- | --- | --- |
| **RAW (Chỉ số `<INDEX>`)** | `RAW (NDVI Index)` | Mỗi chỉ số được cấu hình trong Cài đặt Dự án, được tính toán trong quá trình xử lý |
| **`<INDEX>` LUT** | `NDVI LUT` | Phiên bản được ánh xạ màu của một chỉ mục |
| **Sandbox (`<Name>` `<Index\|LUT>` `<NNN>`)** | `Sandbox (NDVI LUT 003)` | Mỗi lần xuất [Chỉ mục/LUT Sandbox](index-lut-sandbox.md) sẽ có một bản |

Nếu cùng một tên chỉ mục được cấu hình nhiều lần với các thiết lập khác nhau, các bản cấu hình từ lần thứ hai trở đi sẽ được thêm số vào tên (`RAW (NDVI2 Index)`) để các lớp vẫn có thể phân biệt được.

***

## Sử dụng trình chọn lớp

1. Mở hình ảnh ở chế độ toàn màn hình bằng cách nhấp vào hình thu nhỏ trong lưới
2. Nhấp vào **menu thả xuống lớp** ở góc trên bên phải của trình xem
3. Chọn một lớp — hình ảnh sẽ được cập nhật ngay lập tức

Menu thả xuống sẽ hiển thị **JPG, RAW (Original), RAW (Target), RAW (Reflectance)** trước tiên, theo thứ tự đó, và liệt kê tất cả các tùy chọn khác sau đó theo thứ tự các sản phẩm được đăng ký.

### Ưu tiên lớp khi điều hướng

Nhấn phím **←**/**→** để chuyển sang hình ảnh tiếp theo và cố gắng giữ nguyên lớp hiện tại:

1. **Ưu tiên khớp chính xác** — nếu hình ảnh tiếp theo có lớp cùng tên, hệ thống sẽ chuyển sang lớp đó. Đây là cơ chế giúp bạn duy trì ở lớp `RAW (NDVI Index)` khi duyệt qua toàn bộ bộ dữ liệu
2. **Sau đó là khớp theo loại** — lớp chỉ mục sẽ tìm bất kỳ lớp chỉ mục nào, lớp LUT tìm bất kỳ lớp LUT nào, lớp phản xạ tìm lớp phản xạ, lớp mục tiêu tìm lớp mục tiêu, lớp gốc tìm lớp gốc, lớp cơ sở tìm lớp cơ sở
3. **Tiếp theo, chỉ áp dụng cho các lớp xuất** — tên lớp vẫn được giữ nguyên ngay cả khi danh sách lớp chưa cập nhật kịp, vì tệp đã tồn tại trên đĩa. Đây chính là cơ chế cho phép bạn xem trước sản phẩm trong khi quá trình chạy vẫn đang ghi chúng
4. **Trong các trường hợp khác** — lớp đầu tiên có sẵn, thường là hình ảnh gốc

Các tệp sidecar `.daq` và `.csv` trong dự án sẽ bị bỏ qua khi điều hướng bằng phím mũi tên, do đó việc di chuyển qua các hình ảnh sẽ không bao giờ dừng lại ở bản ghi cảm biến ánh sáng.

Chức năng thu phóng và di chuyển cũng được áp dụng trên các hình ảnh, giúp việc so sánh trước/sau tại cùng một vị trí trường trở nên đơn giản.

***

## Hiểu giá trị pixel theo từng lớp

[Bảng Giá trị Con trỏ](opening-an-image-full-screen.md#cursor-values) hiển thị giá trị thực theo từng kênh tại vị trí con trỏ của bạn, với đơn vị mà lớp đó được lưu trữ. Các cột của bảng này thay đổi tùy theo lớp:

| Lớp | Đơn vị hiển thị | Ghi chú |
| --- | --- | --- |
| Base (JPG /PNG

/TIFF

preview) | DN, 0–255 | Giá trị hiển thị, đã được hiệu chỉnh gamma trênRGB

. Chỉ dành cho kiểm tra trực quan |
| RAW (Bản gốc) | DN | Số liệu kỹ thuật số thô từ cảm biến. Trục biểu đồ tần suất cho biết độ sâu: 255 (8-bit), 4095 (12-bit) hoặc 65535 (16-bit) |
| RAW (Đã loại bỏ hiệu ứng Bayer) | DN | Tuyến tính, không kéo giãn hiển thị |
| RAW (Xem trước) / Đã cân bằng trắng | DN | Sản phẩm hiển thị — đã được kéo giãn hoặc hiệu chỉnh gamma. Không dùng để đo lường |
| RAW (Độ rạng rỡ) | **W/m²/sr/nm** | Độ rạng rỡ vật lý Float32. Không có cột DN |
| RAW (Độ phản xạ) | DN **và %** | Phần trăm được tính toán dựa trên thang đo riêng của tệp đó — xem bên dưới |
| Xuất chỉ số / LUT / sandbox | Giá trị chỉ số, hoặc các thành phầnRGB

| Tệp chỉ số kênh đơn báo cáo giá trị chỉ số; tệp LUT ánh xạ màu báo cáo các thành phầnRed

/Green

/Blue

|

### Độ phản xạ: thang đo được xác định theo từng tệp

{% hint style="warning" %}
**&quot;Chia cho 65.535&quot; chỉ chính xác đối vớiSurvey3

.** Độ phản xạ LATTICE được lưu trữ theo một thang đo khác, và việc trộn lẫn hai hệ số chia này là cách phổ biến nhất dẫn đến các giá trị độ phản xạ chỉ bằng một nửa so với giá trị thực tế.
{% endhint %}

| Nguồn | Giá trị DN tương ứng với hệ số phản xạ 1.0 | Được xác định bởi |
| --- | --- | --- |
| **LATTICE**(M3C / M3M) |**32768** | Thẻ XMP `Chloros:PixelScale=32768` được gắn vào mọi tệp xuất độ phản xạ LATTICE. Dải động 2× có nghĩa là ρ lớn hơn 1.0 có thể được biểu diễn thay vì bị cắt bớt |
| **Survey3**| **65535** | Không có thẻ tỷ lệ XMP &quot;Chloros

&quot; — quá trình hiệu chuẩnSurvey3

ghi giá trị ρ × dtype-max và cắt bớt tại 1.0 |

Đối với GIS và lập trình kịch bản: đọc giá trị `Chloros:PixelScale` từ tệp và chia cho giá trị đó. Nếu thẻ này không có, tệp sẽ sử dụng tỷ lệ quy mô mặc định củaSurvey3

(65535). Trình xem, môi trường thử nghiệm chỉ mục/LUT và quá trình xuất chỉ mục đều xác định tỷ lệ này theo cách tương tự, do đó con số hiển thị tại vị trí con trỏ chính là giá trị được sử dụng trong các phép tính của chỉ mục.

Lưu trữ theo định dạng cụ thể dựa trên tỷ lệ quy mô đó:

* **TIFF

(32-bit, Phần trăm)** lưu trữ DN / 65535 dưới dạng số thực
* **PNG

(8-bit)**và**JPG (8-bit)** lưu trữ DN × 255 / 65535
* Một **tệp xuất 8-bitTIFF

từ nguồn 8-bit** sẽ bị cắt giới hạn trong khoảng 0–255 thay vì được điều chỉnh tỷ lệ, và cố ý không chứa thẻ tỷ lệ. Bảng điều khiển chỉ hiển thị giá trị DN cho các tệp này, không có cột phần trăm

### Phạm vi giá trị chỉ số

| Nhóm chỉ số | Phạm vi điển hình | Giá trị |
| --- | --- | --- |
| Chênh lệch chuẩn hóa (NDVI

,GNDVI

,NDRE

, ENDVI…) | −1 đến +1 | Thực vật khỏe mạnh thường từ 0,4–0,9; đất trống gần 0; nước có giá trị âm |
| Chỉ số điều chỉnh theo đất (SAVI

,OSAVI

, MSAVI2…) | khoảng −1 đến +1,5 | Giá trị tương tự nhưNDVI

nhưng đã loại bỏ nền đất |
| Tỷ lệ (GRVI

,GCI

, MSR, CIRE…) | không giới hạn phía trên | Tỷ lệ tăng không giới hạn khi dải mẫu số tiến tới 0 |
|EVI

/LAI

| 0 đến ~1, 0 đến ~3,5 | Mây và các pixel bão hòa khác đẩy cả hai giá trị ra khỏi phạm vi — hãy che chúng lại trước |

Xem [Công thức chỉ số đa quang phổ](../project-settings/multispectral-index-formulas.md) để biết công thức chính xác đằng sau mỗi cài đặt sẵn.

***

## Quy trình làm việc thông thường

### So sánh trước / sau

1. Chọn **RAW (Original)** và lưu ý hiện tượng tối góc và các giá trị chưa được hiệu chỉnh
2. Chuyển sang **RAW (Reflectance)**

3. So sánh — hiện tượng tối góc đã được loại bỏ, các giá trị đã được hiệu chuẩn. Tính năng thu phóng và di chuyển được giữ nguyên, do đó bạn đang xem cùng một khu vực mặt đất

### Xem xét một chỉ số trên toàn bộ bộ ảnh

1. Mở hình ảnh đã xử lý đầu tiên và chọn lớp chỉ số
2. Nhấn **→** nhiều lần — lớp chỉ số sẽ theo bạn từ hình ảnh này sang hình ảnh khác
3. Theo dõi biểu đồ histogram ở thanh bên trong quá trình xem: khung hình nào có sự thay đổi đột ngột trong phân bố giá trị thì cần xem xét kỹ hơn

### Xác minh mục tiêu hiệu chuẩn

1. Chọn **RAW (Mục tiêu)** trên một khung hình mục tiêu
2. Xác nhận mục tiêu hiển thị rõ ràng và được phát hiện
3. Chuyển sang khung hình mục tiêu tiếp theo — lớp mục tiêu sẽ theo sau

### Kiểm tra độ chính xác của các giá trị phản xạ

1. Chọn **RAW (Phản xạ)**

2. Đọc cột**%** trong bảng Giá trị Con trỏ — giá trị này đã được chuẩn hóa chính xác cho tệp đó
3. Kiểm tra chéo với các vật liệu đã biết trong khung hình: thực vật khỏe mạnh có giá trị phản xạ xanh lá cây (NIR

) cao và giá trị đỏ thấp; mục tiêu hiệu chuẩn nên có giá trị phản xạ gần với giá trị đã công bố

***

## Khắc phục sự cố

### Một lớp mà tôi mong đợi không có trong danh sách thả xuống

**Nguyên nhân có thể**

* Hình ảnh chưa bao giờ được xử lý — chỉ có lớp nền và `RAW (Original)` tồn tại
* Công tắc xuất sản phẩm chưa được chọn trong Cài đặt Dự án
* Sản phẩm không áp dụng cho máy ảnh đó (độ rạng rỡ và độ phản xạ trên máy ảnh chínhRGB

; bất kỳ chỉ số nào trên máy ảnh đơn dải M3M mono)
* Quá trình hiệu chuẩn phản xạ không có dữ liệu để xử lý — không có vùng phủ sóng hướng xuống của `.daq` và không có mục tiêu trong khung hình đạt tiêu chuẩn QA — do đó khung hình đã chuyển về chế độ “Vignette Corrected” hoặc “Sensor Response”

**Cách xử lý**

1. Kiểm tra nhật ký chạy:Chloros

ghi lại thời điểm sản phẩm xuất yêu cầu không thể thực hiện được và lý do tại sao
2. Kiểm tra các công tắc xuất theo từng sản phẩm trong [Cài đặt Dự án](../project-settings/project-settings.md)
3. Xác nhận thư mục sản phẩm tồn tại trong cây kết quả đầu ra của dự án
4. Xử lý lại với sản phẩm được bật

### Danh sách lớp trông có vẻ lỗi thời

Chloros

sẽ quét lại các thư mục sản phẩm của dự án trong khi quá trình chạy đang diễn ra và khắc phục các đăng ký lớp bị thiếu dựa trên dữ liệu thực tế trên đĩa, do đó một lớp đã xuất thành công thường sẽ tự động xuất hiện trong lần quét tiếp theo. Việc chuyển sang cửa sổ khác rồi quay lại sẽ buộc hệ thống thực hiện quá trình xác định lại từ đầu.

### Giá trị phản xạ chỉ bằng một nửa so với mức bình thường

Rất có thể bạn đang chia tệp LATTICE cho 65535. Hãy sử dụng `Chloros:PixelScale` (32768), hoặc xem cột **%**, nơi giá trị này đã được áp dụng sẵn.

### Lớp chỉ mục tồn tại nhưng hình ảnh trống

Chỉ mục yêu cầu các dải phổ mà lớp của bạn không có — ví dụ: một chỉ mục đọc kênh thứ ba được áp dụng cho tệp một hoặc hai kênh. Chuyển sang lớp đa dải phổ (phản xạ hoặc đã loại bỏ hiệu ứng Bayer), hoặc chọn chỉ mục phù hợp với bộ lọc của máy ảnh.

***

## Các bước tiếp theo

* [**Mở hình ảnh ở chế độ toàn màn hình**](opening-an-image-full-screen.md) — hiển thị con trỏ, biểu đồ histogram và điều khiển GSD
* [**Hộp cát chỉ số/LUT**](index-lut-sandbox.md) — trực quan hóa chỉ số tương tác và xuất
* [**Công thức chỉ số đa phổ**](../project-settings/multispectral-index-formulas.md) — tài liệu tham khảo về chỉ số
* [**Hoàn tất quá trình xử lý**](../processing-images-gui/finishing-the-processing.md) — Cấu trúc thư mục đầu ra mà các lớp này trỏ đến
