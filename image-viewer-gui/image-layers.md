# Các lớp hình ảnh

**Menu thả xuống các lớp** ở góc trên bên phải của Trình xem hình ảnh cho phép bạn chuyển đổi giữa các phiên bản khác nhau của hình ảnh đang xem — từ bản chụp nguồn qua từng sản phẩm đã xử lý cho đến các hình ảnh chỉ số được tính toán — mà không cần thoát khỏi trình xem.

## Lớp hình ảnh là gì?

Một “lớp” trong Chloros là một **tệp sản phẩm**được đăng ký dựa trên một hình ảnh nguồn. Quá trình nhập dữ liệu cung cấp cho bạn các tệp nguồn; quá trình xử lý sẽ thêm một lớp cho mỗi sản phẩm được tạo ra trong lần chạy đó. Các tệp đã xuất vẫn giữ tên tệp nguồn — chính**thư mục** là yếu tố xác định sản phẩm, và tên lớp là nhãn mà Chloros gán cho thư mục đó.

<!-- SCREENSHOT-NEEDED: Image Viewer full screen with the layer dropdown open on a processed LATTICE multispectral image, showing the full list: TIFF base, RAW (Original), RAW (Debayered), RAW (Preview), RAW (Radiance), RAW (Reflectance), and one RAW (NDVI Index) entry. -->

***

## Danh sách lớp

### Luôn hiển thị

| Lớp | Mô tả |
| --- | --- |
| **JPG**(hoặc**PNG**/**TIFF**) | Tệp gốc đi kèm với dữ liệu chụp. Survey3 nhập tệp `.JPG` bên cạnh mỗi tệp `.RAW`; dữ liệu chụp từ LATTICE đi kèm với bản xem trước hiển thị định dạng PNG hoặc TIFF. Được gắn nhãn theo nội dung thực tế đã được nhập |
| **RAW (Gốc)** | Khung hình thô nguồn, đã được giải mã màu để hiển thị mà không áp dụng bất kỳ chỉnh sửa nào. Có sẵn ngay từ lúc nhập — không cần xử lý |

Một bản chụp LATTICE mà tệp gốc **chính là** khung hình RAW của nó sẽ không có mục gốc riêng biệt: `RAW (Original)` đã bao gồm nó.

### Các sản phẩm xử lý của Survey3



| Lớp | Được ghi vào | Tồn tại khi |
| --- | --- | --- |
| **RAW (Mục tiêu)** | — | Khung hình được xác định là chứa mục tiêu hiệu chuẩn |
| **RAW (Độ phản xạ)** | `Reflectance_Calibrated_Images/` | Quá trình hiệu chuẩn độ phản xạ đã thực hiện thành công trên khung hình này |
| **Đã hiệu chỉnh hiện tượng tối góc**| `Vignette_Corrected_Images/` | Khung hình không thể hiệu chuẩn phản xạ**và** *chức năng hiệu chỉnh hiện tượng tối góc* đang bật |
| **Phản ứng cảm biến**| `Sensor_Response_Images/` | Không thể hiệu chuẩn độ phản xạ cho khung hình này**và** *chức năng chỉnh hiệu ứng viền tối* đã bị tắt |
| **Đã cân bằng trắng** | `White_Balanced_Images/` | Đã tạo sản phẩm đã cân bằng trắng |

{% hint style="info" %}
**Chức năng "Chỉnh sửa hiệu ứng viền tối" và "Phản hồi cảm biến" là các tùy chọn thay thế cho nhau, không bao giờ được kích hoạt cùng lúc.** Mỗi lần chạy, đối với mỗi mẫu máy ảnh, chỉ tồn tại duy nhất một sản phẩm dự phòng chưa được hiệu chỉnh, và công tắc *Chỉnh sửa hiệu ứng viền tối* sẽ chọn sản phẩm đó. Xem [Cài đặt dự án](../project-settings/project-settings.md).
{% endhint %}

### Các cấp độ LATTICE

LATTICE ghi lại các cấp độ này trong một lần xử lý duy nhất. Các cấp độ nào tồn tại phụ thuộc vào các tùy chọn xuất riêng cho từng sản phẩm trong Cài đặt Dự án và vào những gì áp dụng cho máy ảnh.

| Lớp | Ghi vào | Áp dụng cho |
| --- | --- | --- |
| **RAW (Đã loại bỏ hiệu ứng Bayer)** | `Debayered_Images/` | RGB và đa phổ |
| **RAW (Xem trước)** | `Preview_Images/` | Đa quang phổ (kéo dãn màu giả) |
| **Cân bằng trắng** | `Preview_Images/` | Máy ảnh chính RGB — bản xem trước RGB được đăng ký dưới tên này để khớp với lớp Survey3 cùng tên |
| **RAW (Độ rạng rỡ)** | `Radiance_Images/` | Chỉ đa quang phổ |
| **RAW (Độ phản xạ)** | `Reflectance_Calibrated_Images/` | Chỉ đa quang phổ, và chỉ khi có bản ghi hướng xuống tương ứng `.daq` hoặc mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) bao phủ khung hình |

Các camera chính RGB không có đo quang phổ theo từng dải, do đó các thông số độ rạng rỡ và độ phản xạ sẽ bị bỏ qua đối với chúng với lý do **không áp dụng** — nhật ký hệ thống sẽ ghi rõ điều này thay vì báo lỗi im lặng.

{% hint style="info" %}
**Các lớp dữ liệu thô (Raw) và lớp đã loại bỏ hiệu ứng Bayer (debayered) được căn chỉnh với các sản phẩm (tính năng mới trong phiên bản 1.3.0).**Các tệp dữ liệu thô và đã loại bỏ hiệu ứng Bayer của LATTICE được lưu ở định hướng gốc của cảm biến, và ghi lại định hướng đó trong tệp XMP; các sản phẩm khác được lưu với định hướng đó đã được áp dụng sẵn. Trình xem và lưới hình ảnh sẽ xoay các lớp**RAW (Original)**và**RAW (Debayered)** theo thông tin định hướng đó để hiển thị, do đó khung hình sẽ không còn bị lật khi bạn chuyển đổi giữa một lớp sản phẩm và một lớp RAW. Điều này chỉ áp dụng cho hiển thị — các tệp tin sẽ không bao giờ bị ghi đè. Các tệp không có thông tin định hướng (được chụp trước phiên bản 1.3.0 hoặc được tạo bởi phần mềm khác) sẽ được hiển thị theo định dạng lưu trữ ban đầu. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md#orientation).
{% endhint %}

Các mặt nạ giới hạn (`*_clampmask.tif` trong thư mục `clamp_masks/`) không phải là lớp: Chloros sẽ bỏ qua chúng khi nhập. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md#clamp-masks).

### Các lớp chỉ mục, LUT và sandbox

| Mẫu lớp | Ví dụ | Nguồn gốc |
| --- | --- | --- |
| **RAW (Chỉ mục `<INDEX>`)** | `RAW (NDVI Index)` | Mỗi chỉ mục được cấu hình trong Cài đặt Dự án sẽ có một lớp, được tính toán trong quá trình xử lý |
| **`<INDEX>` LUT** | `NDVI LUT` | Phiên bản được ánh xạ màu của một chỉ mục |
| **Sandbox (`<Name>` `<Index\|LUT>` `<NNN>`)** | `Sandbox (NDVI LUT 003)` | Mỗi [Chạy xuất [Chỉ số/LUT Sandbox]](index-lut-sandbox.md) lần xuất |

Nếu cùng một tên chỉ mục được cấu hình nhiều lần với các thiết lập khác nhau, các mục từ thứ hai trở đi sẽ được thêm số vào tên (`RAW (NDVI2 Index)`) để các lớp vẫn có thể phân biệt được.

***

## Sử dụng trình chọn lớp

1. Mở hình ảnh ở chế độ toàn màn hình bằng cách nhấp vào hình thu nhỏ trong lưới
2. Nhấp vào **menu thả xuống lớp** ở góc trên bên phải của trình xem
3. Chọn một lớp — hình ảnh sẽ được cập nhật ngay lập tức

Menu thả xuống sẽ hiển thị **JPG, RAW (Original), RAW (Target), RAW (Reflectance)** trước tiên, theo thứ tự đó, và liệt kê tất cả các tùy chọn khác sau đó theo thứ tự các sản phẩm đã được đăng ký.

### Ưu tiên lớp khi điều hướng

Nhấn phím **←**/**→** để chuyển sang hình ảnh tiếp theo và cố gắng giữ nguyên lớp hiện tại:

1. **Ưu tiên khớp chính xác** — nếu hình ảnh tiếp theo có lớp cùng tên, hệ thống sẽ chuyển sang lớp đó. Đây là cơ chế giúp bạn duy trì ở lớp `RAW (NDVI Index)` khi duyệt qua toàn bộ bộ dữ liệu
2. **Sau đó là khớp theo loại** — lớp chỉ số sẽ tìm bất kỳ lớp chỉ số nào, lớp LUT tìm bất kỳ lớp LUT nào, lớp phản xạ tìm lớp phản xạ, lớp mục tiêu tìm lớp mục tiêu, lớp gốc tìm lớp gốc, lớp cơ sở tìm lớp cơ sở
3. **Sau đó, chỉ áp dụng cho các lớp xuất** — tên lớp vẫn được giữ nguyên ngay cả khi danh sách lớp chưa cập nhật kịp, vì tệp đã tồn tại trên đĩa. Đây chính là cơ chế cho phép bạn xem trước các sản phẩm trong khi quá trình xử lý vẫn đang ghi chúng
4. **Trong các trường hợp khác** — lớp đầu tiên có sẵn, thường là hình ảnh gốc

Các tệp sidecar `.daq` và `.csv` trong dự án sẽ bị bỏ qua khi điều hướng bằng phím mũi tên, do đó việc di chuyển qua các hình ảnh sẽ không bao giờ dừng lại ở bản ghi cảm biến ánh sáng.

Chức năng thu phóng và di chuyển cũng được áp dụng trên các hình ảnh, giúp việc so sánh trước/sau tại cùng một vị trí trường trở nên đơn giản.

***

## Hiểu giá trị pixel theo từng lớp

[Bảng Giá trị Con trỏ](opening-an-image-full-screen.md#cursor-values) hiển thị giá trị thực theo từng kênh tại vị trí con trỏ, với đơn vị lưu trữ của lớp đó. Các cột trong bảng sẽ thay đổi tùy theo lớp:

| Lớp | Đơn vị hiển thị | Ghi chú |
| --- | --- | --- |
| Cơ sở (xem trước JPG / PNG / TIFF) | DN, 0–255 | Giá trị hiển thị, đã hiệu chỉnh gamma trên RGB. Chỉ dùng để kiểm tra trực quan |
| RAW (Bản gốc) | DN | Số liệu kỹ thuật số thô từ cảm biến. Trục biểu đồ histogram cho biết độ sâu: 255 (8-bit), 4095 (12-bit) hoặc 65535 (16-bit) |
| RAW (Đã loại bỏ hiệu ứng Bayer) | DN | Tuyến tính, không kéo giãn hiển thị |
| RAW (Xem trước) / Đã cân bằng trắng | DN | Sản phẩm hiển thị — đã kéo giãn hoặc hiệu chỉnh gamma. Không dùng để đo lường |
| RAW (Độ rạng rỡ) | **W/m²/sr/nm** | Độ rạng rỡ vật lý dạng Float32. Không có cột DN |
| RAW (Độ phản xạ) | DN **và %** | Phần trăm được tính toán dựa trên thang đo riêng của tệp đó — xem bên dưới |
| Xuất chỉ số / LUT / sandbox | Giá trị chỉ số, hoặc các thành phần RGB | Tệp chỉ số một kênh báo cáo giá trị chỉ số; tệp LUT ánh xạ màu báo cáo các thành phần Red /Green /Blue |

### Độ phản xạ: thang đo là riêng cho từng tệp

{% hint style="warning" %}
**"Chia cho 65.535" chỉ chính xác đối với Survey3.** Độ phản xạ LATTICE được lưu trữ theo thang đo khác, và việc trộn lẫn hai hệ số chia này là nguyên nhân phổ biến nhất dẫn đến các giá trị độ phản xạ chỉ bằng một nửa so với giá trị thực tế.
{% endhint %}

| Nguồn | Giá trị DN tương ứng với độ phản xạ 1,0 | Được xác định bởi |
| --- | --- | --- |
| **LATTICE**(M3C / M3M) |**32768** | Thẻ XMP `Chloros:PixelScale=32768` được gắn vào mọi tệp xuất độ phản xạ LATTICE. Dải dư 2× có nghĩa là ρ lớn hơn 1,0 có thể được biểu diễn thay vì bị cắt bớt |
| **Survey3**| **65535** | Không có thẻ tỷ lệ XMP của Chloros — quá trình hiệu chuẩn Survey3 ghi giá trị ρ × dtype-max và cắt bớt tại 1.0 |

Đối với GIS và lập trình kịch bản: đọc giá trị `Chloros:PixelScale` từ tệp và chia cho giá trị đó. Nếu thẻ này không có, tệp sẽ sử dụng tỷ lệ Survey3 (65535). Trình xem, môi trường thử nghiệm chỉ mục/LUT và quá trình xuất chỉ mục đều xác định tỷ lệ theo cách này, do đó giá trị hiển thị tại con trỏ chính là giá trị được sử dụng trong tính toán chỉ mục.

Lưu trữ theo định dạng cụ thể dựa trên tỷ lệ đó:

* **Tệp TIFF (32-bit, Phần trăm)** lưu trữ DN / 65535 dưới dạng số thực
* **Tệp PNG (8-bit)**và**JPG (8-bit)** lưu trữ DN × 255 / 65535
* Một **giá trị DN 8-bit từ dữ liệu chụp 8-bit**, hoặc tệp phản xạ được ghi trước phiên bản 1.3.0, sẽ bị cắt giới hạn trong khoảng 0–255 và không chứa thẻ tỷ lệ. Bảng điều khiển chỉ hiển thị giá trị DN cho các tệp này, không có cột phần trăm

### Phạm vi giá trị chỉ số

| Nhóm chỉ số | Phạm vi điển hình | Giá trị đọc |
| --- | --- | --- |
| Chênh lệch chuẩn hóa (NDVI, GNDVI, NDRE, ENDVI…) | −1 đến +1 | Thực vật khỏe mạnh thường từ 0,4–0,9; đất trống gần 0; nước có giá trị âm |
| Chỉ số điều chỉnh theo đất (SAVI, OSAVI, MSAVI2…) | khoảng −1 đến +1,5 | Giá trị đọc tương tự NDVI nhưng đã loại bỏ nền đất |
| Tỷ lệ (GRVI, GCI, MSR, CIRE…) | không giới hạn phía trên | Tỷ lệ tăng không giới hạn khi dải số chia về 0 |
| EVI / LAI | từ 0 đến ~1, từ 0 đến ~3,5 | Mây và các pixel bão hòa khác khiến cả hai chỉ số vượt khỏi phạm vi — cần loại bỏ chúng trước |

Xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md) để biết công thức chính xác đằng sau mỗi cài đặt sẵn.

***

## Quy trình làm việc thông thường

### So sánh trước / sau

1. Chọn **RAW (Gốc)** và lưu ý hiện tượng tối góc cùng các giá trị chưa được hiệu chỉnh
2. Chuyển sang **RAW (Phản xạ)**

3. So sánh — hiện tượng tối góc đã được loại bỏ, các giá trị đã được hiệu chỉnh. Tính năng thu phóng và di chuyển được giữ nguyên, do đó bạn đang quan sát cùng một khu vực mặt đất

### Xem xét một chỉ số trên toàn bộ bộ ảnh

1. Mở hình ảnh đã xử lý đầu tiên và chọn lớp chỉ số
2. Nhấn **→** nhiều lần — lớp chỉ số sẽ theo bạn từ hình ảnh này sang hình ảnh khác
3. Theo dõi biểu đồ histogram ở thanh bên trong quá trình xem: khung hình nào có sự thay đổi đột ngột trong phân bố thì cần xem xét kỹ hơn

### Xác minh mục tiêu hiệu chuẩn

1. Chọn **RAW (Mục tiêu)** trên một khung hình mục tiêu
2. Xác nhận mục tiêu hiển thị rõ ràng và đã được phát hiện
3. Chuyển sang khung hình mục tiêu tiếp theo — lớp mục tiêu sẽ theo sau

### Kiểm tra độ chính xác của các giá trị phản xạ

1. Chọn **RAW (Phản xạ)**

2. Đọc cột**%** trong bảng Giá trị Con trỏ — giá trị này đã được quy đổi đúng tỷ lệ cho tệp đó
3. Kiểm tra chéo với các vật liệu đã biết trong khung hình: thảm thực vật khỏe mạnh có giá trị cao ở dải NIR và thấp ở dải đỏ; mục tiêu hiệu chuẩn nên có giá trị phản xạ gần với giá trị đã công bố

***

## Khắc phục sự cố

### Một lớp mà tôi mong đợi không có trong danh sách thả xuống

**Nguyên nhân có thể**

* Hình ảnh chưa bao giờ được xử lý — chỉ có lớp nền và `RAW (Original)` tồn tại
* Công tắc xuất sản phẩm bị bỏ chọn trong Cài đặt Dự án
* Sản phẩm không áp dụng cho máy ảnh đó (độ rạng rỡ và độ phản xạ trên bản gốc RGB; bất kỳ chỉ số nào trên máy ảnh đơn dải M3M mono)
* Quá trình hiệu chuẩn phản xạ không có dữ liệu để xử lý — không có vùng phủ sóng hướng xuống của `.daq` và không có mục tiêu trong khung hình đạt tiêu chuẩn QA — do đó khung hình đã chuyển về chế độ Vignette Corrected hoặc Sensor Response

**Cách khắc phục**

1. Kiểm tra nhật ký chạy: Chloros sẽ thông báo khi không thể xuất sản phẩm được yêu cầu và lý do tại sao
2. Kiểm tra các tùy chọn bật/tắt xuất cho từng sản phẩm trong [Cài đặt Dự án](../project-settings/project-settings.md)
3. Xác nhận thư mục sản phẩm tồn tại trong cây kết quả đầu ra của dự án
4. Xử lý lại với sản phẩm được bật

### Danh sách lớp trông có vẻ lỗi thời

Trong khi quá trình chạy đang diễn ra, Chloros sẽ quét lại các thư mục sản phẩm của dự án và khắc phục các vấn đề đăng ký lớp bị thiếu dựa trên dữ liệu thực tế trên đĩa; do đó, một lớp đã xuất thành công thường tự động xuất hiện trong lần quét tiếp theo. Việc chuyển sang một hình ảnh khác rồi quay lại sẽ buộc hệ thống thực hiện quá trình xác định lại từ đầu.

### Giá trị phản xạ chỉ bằng một nửa so với mức bình thường

Rất có thể bạn đang chia tệp LATTICE cho 65535. Hãy sử dụng `Chloros:PixelScale` (32768), hoặc xem cột **%**, nơi giá trị này đã được áp dụng sẵn.

### Lớp chỉ mục tồn tại nhưng hình ảnh trống

Chỉ mục cần các dải phổ mà lớp của bạn không có — ví dụ: một chỉ mục đọc kênh thứ ba được áp dụng cho tệp một hoặc hai kênh. Chuyển sang lớp đa dải phổ (độ phản xạ hoặc đã loại bỏ hiệu ứng Bayer), hoặc chọn chỉ mục phù hợp với bộ lọc của máy ảnh.

***

## Các bước tiếp theo

* [**Mở hình ảnh ở chế độ toàn màn hình**](opening-an-image-full-screen.md) — hiển thị con trỏ, biểu đồ histogram và điều khiển GSD
* [**Môi trường thử nghiệm chỉ số/LUT**](index-lut-sandbox.md) — hiển thị tương tác chỉ số và xuất dữ liệu
* [**Công thức chỉ số đa phổ**](../project-settings/multispectral-index-formulas.md) — tài liệu tham khảo về chỉ số
* [**Hoàn tất quá trình xử lý**](../processing-images-gui/finishing-the-processing.md) — cấu trúc thư mục đầu ra mà các lớp này trỏ đến
