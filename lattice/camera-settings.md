# Cài đặt máy quay

Tab **Máy quay** là giao diện điều khiển trực tiếp củChloros

dành cho các máy quay LATTICE: một khu vực nguồn chính hiển thị mọi máy quay được kết nối dưới dạng ô trực tiếp, cùng một thanh bên có thể trượt qua lại giữa ba trang — **danh sách máy quay**,**bảng cài đặt**(cài đặt cho từng camera, mảng camera hoặc cài đặt ghi hình — chỉ hiển thị một loại tại một thời điểm), và**Trình tính toán chỉ số**. Trang này mô tả chi tiết mọi điều khiển trong danh sách camera, bảng cài đặt cho từng camera và bảng cài đặt mảng camera. Các chế độ ghi hình, lựa chọn loại xuất và quy trình “Capture All” được trình bày trên trang liên quan [Cài đặt &amp; Chế độ Ghi hình](capture.md).

Tab “Camera” sẽ xuất hiện trong thanh bên khi hệ thống nền Chloros

đã sẵn sàng. Tất cả các điều khiển dưới đây giao tiếp với hệ thống nền cục bộ qua `127.0.0.1:5000`; các thay đổi sẽ được áp dụng ngay lập tức cho camera trực tiếp trừ khi có ghi chú khác.

## Các loại camera được sử dụng trên trang này

Các điều khiển sẽ hiển thị hoặc ẩn tùy thuộc vào loại camera được chọn. Tài liệu hướng dẫn sử dụng các thuật ngữ sau đây xuyên suốt:

| Thuật ngữ | Ý nghĩa | Kênh bộ lọc |
| --- | --- | --- |
| **Camera RGB** | LATTICE M3C với bộ lọc FRGB (mã sản phẩm chứa `-FRGB`) | Red

/ Green

/ Blue

|
| **Đa quang phổ Bayer** | LATTICE M3C với FRGN, FOCN hoặc FNGB | FRGN: Red

/ Green

/ NIR

· FOCN: Orange

/ Cyan

/ NIR

· FNGB: NIR

/ Green

/ Blue

|
| **Mono (M3M)** | LATTICE M3M — một bộ lọc dải hẹp, một dải đã hiệu chuẩn | Dải đơn |
| **Thành viên mảng** | Một camera được kết nối như một phần của mảng đồng bộ (hiển thị kết hợp hoặc riêng biệt) | Theo bộ lọc của nó |

Camera RGB

được xử lý quang học (cân bằng trắng, cấu hình màu, gamma); camera đa quang phổ và đơn quang phổ được xử lý theo chuỗi bức xạ và bỏ qua các điều khiển quang học. Các thành viên mảng chuyển các cài đặt cấp luồng (định dạng pixel, độ phân giải, binning, kích hoạt, tốc độ khung hình) cho mảng — các hàng đó sẽ chuyển sang chế độ chỉ đọc trong khung điều khiển từng camera và được chuyển sang khung cài đặt mảng thay thế.

## Khu vực nguồn cấp dữ liệu chính



<!-- SCREENSHOT-NEEDED: Cameras tab with 2+ cameras connected in grid view — live tiles visible with name and fps overlays, sidebar camera list open on the right. -->

Khi không có camera nào được kết nối, khu vực nguồn cấp dữ liệu sẽ hiển thị thông báo **&quot;Kết nối camera để bắt đầu&quot;**kèm theo hai nút:**Kết nối Camera**(màu xanh lá, mở hộp thoại kết nối camera đơn) và**Kết nối Mảng** (màu xanh dương, mở hộp thoại kết nối mảng). Các hộp thoại kết nối được mô tả chi tiết trong [Kết nối Camera](connecting.md); các khái niệm về mảng (đồng bộ hóa, cấp độ, băng thông) được trình bày trong [Mảng nhiều camera](arrays.md). Khi bạn mở một dự án đã lưu có chứa camera, màn hình chào mừng sẽ hiển thị biểu tượng quay tròn kèm thông báo &quot;Đang mở lại N camera đã lưu…&quot; trong khi Chloros

khôi phục các luồng video từ phiên làm việc trước.

<!-- SCREENSHOT-NEEDED: Cameras tab empty state — the "Connect a camera to get started" splash with the green Connect Camera and blue Connect Array buttons. -->

### Thanh trên cùng

| Điều khiển | Chức năng |
| --- | --- |
| **Chuyển đổi chế độ xem**| Chuyển đổi giữa**chế độ xem lưới**(tất cả ô hiển thị dưới dạng ô lưới) và**chế độ xem danh sách**(các hàng toàn chiều rộng: mỗi mảng kết hợp được hiển thị dưới dạng một hàng tổng hợp, mỗi thành viên của một**mảng riêng biệt** được hiển thị dưới dạng một hàng riêng với luồng video và bảng Mục tiêu hiệu chuẩn riêng, sau đó là MỘT camera độc lập — camera đang hoạt động — ở phía dưới). Chú thích: &quot;Chuyển sang chế độ lưới&quot; / &quot;Chuyển sang chế độ danh sách&quot;. |
| **Khóa lưới**(ổ khóa) | Mặc định**được khóa** — các ô được cố định tại vị trí. Mở khóa để kéo và sắp xếp lại các ô vào bất kỳ vị trí nào (khoảng trống được giữ nguyên). Lưới sẽ tự động khóa lại mỗi khi có camera mới kết nối. Chú thích: &quot;Mở khóa lưới (cho phép kéo ô)&quot; / &quot;Khóa lưới (đóng băng các ô tại vị trí)&quot;. |
| Thanh trượt **Zoom nguồn cấp dữ liệu** | Kích thước ô, từ 60 px đến toàn bộ chiều rộng khung chứa. Các ô duy trì tỷ lệ khung hình 4:3. Khi chiều rộng ô nhỏ hơn 200 px, tên và lớp phủ fps sẽ ẩn đi để giữ cho ô gọn gàng. |

### Các ô nguồn cấp dữ liệu

Mỗi camera hiển thị một ô trực tiếp tổng hợp; một camera có thể hiển thị thêm ba ô **chia theo kênh** màu xám (xem [Chia theo kênh](#display-overlays-drawn-over-the-live-feed)), còn các mảng camera hiển thị một ô tổng hợp. Ô đang hoạt động có vòng chọn màu theo màu của camera (hoặc mảng camera).

Khi di chuột qua ô, nút đóng **X** sẽ xuất hiện:

* Đóng một ô **tổng hợp** trong khi các ô chia kênh của nó vẫn còn chỉ ẩn ô tổng hợp đó.
* Đóng **ô hiển thị cuối cùng của một camera độc lập** sẽ ngắt kết nối camera đó.
* **Các ô chia kênh của thành viên mảng kết hợp không bao giờ ngắt kết nối** camera — chúng chỉ ẩn đi.

Khi lưới được mở khóa, hãy kéo bất kỳ ô nào đến bất kỳ vị trí nào; bố cục sẽ được lưu cùng với dự án.

## Thanh bên — danh sách

<!-- SCREENSHOT-NEEDED: sidebar camera list pane showing a standalone camera row and an ARRAY group with indented member rows, the DAQ on/off pill visible on the array row, plus the Connect Camera / Connect Array / Capture All buttons at the top. -->

camera Trang đầu tiên của thanh bên liệt kê tất cả các camera và mảng đã kết nối:

* **Kết nối máy ảnh**(màu xanh lá) /**Kết nối mảng** (màu xanh dương, hiển thị &quot;Đang phát hiện...&quot; trong khi quét). Cả hai đều bị vô hiệu hóa khi hộp thoại kết nối đang mở.
* **Ghi lại tất cả** (màu đỏ) — ghi lại tất cả các máy ảnh được liệt kê với các loại xuất được chọn trong Cài đặt Ghi lại. Yêu cầu có dự án đang mở. Được ghi chép đầy đủ trong [Cài đặt &amp; Chế độ chụp](capture.md).
* **Biểu tượng bánh răng Cài đặt chụp** (bên cạnh Chụp tất cả) — mở [cửa sổ Cài đặt chụp](capture.md#the-capture-settings-pane). Tính năng này bị vô hiệu hóa nếu không có dự án hoặc trong khi đang ghi hình.

### Các hàng camera

Mỗi hàng camera hiển thị một đường viền được mã hóa màu (màu tùy chỉnh của camera), nhãn &quot;CAM&quot; — kèm theo ký tự vai trò màu xanh dương **M**(chính) hoặc chữ cái vai trò**S** màu xanh lá (phụ) cho các thành viên trong mảng — và tên hiển thị. Tên mặc định là `LATTICE-MODEL (serial)`; bạn có thể đổi tên từ bảng cài đặt riêng cho từng camera. Các nút trên hàng:

| Nút | Tác dụng |
| --- | --- |
| **Mắt**| Bật/tắt hiển thị. Các camera bị ẩn sẽ biến mất khỏi lưới và**không được tính vào tính năng &quot;Capture All&quot;**. |
| ****Bánh răng** | Mở bảng cài đặt cho từng camera (phần tiếp theo). |
| **Tạm dừng / Phát**| Đóng băng bản xem trước trực tiếp**chỉ ở phía hiển thị** — quá trình ghi hình ở phía máy chủ vẫn tiếp tục chạy. Các camera đang tạm dừng không thể ghi hình. |
| **X** | Ngắt kết nối. Giao diện người dùng cập nhật ngay lập tức (trong trường hợp thuận lợi); quá trình ngắt kết nối ở phía máy chủ có thể mất 10–30 giây để hoàn tất. |

### Các hàng trong mảng

Một hàng trong mảng hiển thị biểu tượng &quot;ARRAY&quot; với màu của mảng, tên mảng (có thể đổi tên trong cài đặt mảng) và nút **DAQ · bật/tắt**—**bật** khi Cảm biến ánh sáng cấp mảng được bật *hoặc* bất kỳ camera thành viên nào có cảm biến riêng; chú thích của nó liệt kê chính xác cảm biến nào cung cấp dữ liệu cho cái gì. Các camera thành viên được liệt kê lùi vào bên dưới với các hàng riêng của chúng. Các nút trên hàng mảng: **mắt**(ẩn/hiển thị TẤT CẢ các thành viên cùng lúc),**bánh răng**(cửa sổ cài đặt mảng),**X**(ngắt kết nối toàn bộ mảng).

Trạng thái cảm biến ánh sáng (DLS) được sử dụng trong các hàng mảng và cửa sổ cài đặt mảng có bốn trạng thái:**tắt**,**đang chờ**(chưa có phổ),**hoạt động**(có phổ đến trong vòng 3 giây qua), và**lỗi thời** — không có phổ mới trong 3 giây, nhưng giá trị đọc cuối cùng *vẫn được sử dụng* (các giá trị đọc từ DAQ không bao giờ hết hạn trên đường dẫn thu thập).

Cho đến khi bạn sắp xếp lại, danh sách sẽ hiển thị các mảng trước, sau đó là các máy ảnh độc lập, với các máy ảnh (và các thành viên của từng mảng) theo **thứ tự máy ảnh**: M3C (Bayer) trước M3M (mono); trong số M3C, FRGB, FRGN, FOCN, rồi FNGB; M3M theo bước sóng bộ lọc, từ thấp nhất đến cao nhất. Cách bố trí mặc định của lưới cũng tuân theo thứ tự này. Bạn có thể kéo các máy ảnh độc lập và toàn bộ nhóm mảng qua nhau trong thanh bên để sắp xếp lại danh sách; các thành viên của mảng không thể kéo riêng lẻ.

## Cửa sổ cài đặt riêng cho từng camera

Mở bằng cách nhấp vào biểu tượng **bánh răng** trên hàng camera. Cửa sổ này sẽ trượt qua danh sách camera.

<!-- SCREENSHOT-NEEDED: per-camera settings pane, top portion — header with color swatch, camera name, rename pencil and close X; live histogram with the orange dashed AE-target line and green mean-luma line; the RGB per-band toggle button visible top-right of the histogram. -->



**Tiêu đề**:**mẫu màu**của camera (nhấp để mở công cụ chọn màu gốc — dùng để thiết lập màu viền thanh bên và màu vòng chọn ô),**tên**kèm nút bút chì**Đổi tên**(nếu lưu tên trống, hệ thống sẽ khôi phục về tên mặc định `MODEL (serial)`), và**×** để đóng.

### Biểu đồ histogram thời gian thực

Ở đầu khung là biểu đồ histogram độ sáng (luma) thời gian thực được tính toán từ bản xem trước JPEG

với tần suất khoảng 8 Hz. Giá trị trung bình được tính theo trọng số Bayer — (R+2G+B)/4 — để khớp với chế độ đo sáng tự động (AE) của chính máy ảnh.

* **Đường đứt nét màu Orange**= mục tiêu AE. **Kéo ngang để điều chỉnh lại mục tiêu** — một lệnh sẽ được gửi khi thả chuột, và việc kéo sẽ chuyển chế độ mục tiêu AE sang Chế độ Thủ công.
* **Đường liền màu Green** = giá trị luma trung bình thực tế (kết quả mà hệ thống AE đang cung cấp).
* **Nút RGB** (góc trên bên phải): chuyển đổi giữa các biểu đồ histogram chồng lớp theo từng dải màu, được tô màu theo bộ lọc của máy ảnh (ví dụ: trên FRGN: NIR

màu xám, xanh lá, đỏ). Trên các máy ảnh đơn sắc (M3M), nút này hiển thị &quot;MONO&quot; và bị vô hiệu hóa — chế độ đơn sắc luôn hiển thị biểu đồ histogram độ sáng một dải.
* Nhãn trục X tuân theo độ sâu bit cảm biến của định dạng pixel hiện tại: 0..255, 0..1023, 0..4095 hoặc 0..65535.

### Các hàng

<!-- SCREENSHOT-NEEDED: per-camera settings info rows — Model, Radiometric Calibration "Active" badge with the tier/sha/date caption, Calibration Report Download button, Serial, Firmware row showing the "Up to date" state, IP, Temperature readout, Calibration Target checkbox, Light Sensor dropdown. -->

thông tin máy ảnh

| Dòng | Hành vi |
| --- | --- |
| **Mẫu** | Chỉ đọc (ví dụ: `LATT-M3C-L87-FRGN`). |
| **Hiệu chuẩn bức xạ** | Biểu tượng màu Green**&quot;Hoạt động&quot; kèm chú thích hiển thị cấp độ hiệu chuẩn, mã băm, ngày hiệu chuẩn và danh sách dải phổ, được tải từ gói hiệu chuẩn của máy ảnh (xem [Hiệu chuẩn bức xạ tại nhà máy](https://mapir.gitbook.io/lattice-camera/calibration/factory-radiometric-calibration)). **Ẩn đối với máy ảnh RGB** — các máy ảnh này sử dụng hiệu chuẩn cân bằng trắng quang học, không phải hiệu chuẩn bức xạ theo từng dải. |
| **Báo cáo hiệu chuẩn**| Nút**Tải xuống** — mở tệp PDF chứng chỉ hiệu chuẩn NIST theo số sê-ri của camera trong trình xem của hệ điều hành. Nếu chứng chỉ chưa được lưu trong bộ nhớ đệm, Chloros

sẽ hiển thị một gợi ý thay thế. |
| **Sê-ri** | Chỉ đọc. |
| **Phần mềm cơ sở**| Hiển thị phiên bản hiện tại, sau đó xác định phiên bản có sẵn cho mẫu máy này (được lưu trong bộ nhớ đệm theo từng mẫu — một mảng gồm N camera sẽ kiểm tra máy chủ một lần). Các trạng thái: &quot;Đang kiểm tra…&quot; → Nút**&quot;Cập nhật lên X&quot;**→ &quot;Đang ghi firmware…&quot; → &quot;Đã cập nhật từ A sang B&quot; / &quot;Thất bại: …&quot; / &quot;Bỏ qua: …&quot; / màu xanh lá cây**&quot;Đã cập nhật&quot;**. Chú thích của nút cập nhật: &quot;Khôi phục cài đặt gốc + cập nhật firmware + lập trình lại UserSet1. Khoảng 2–3 phút; không ngắt kết nối.&quot; |
| **IP** | Chỉ đọc. |
| **Nhiệt độ** | Chỉ đọc, làm mới mỗi 3 giây. Chuyển sang màu cam khi ≥65 °C và màu đỏ kèm biểu tượng ⚠ khi ≥75 °C. |
| **Mục tiêu hiệu chuẩn** hộp chọn | Kích hoạt tính năng phát hiện mục tiêu phản xạ ArUco với bảng xác thực NDVI

cho từng tấm panel bên dưới luồng trực tiếp (chế độ xem danh sách) — xem [Bảng Mục tiêu hiệu chuẩn](#the-calibration-target-table). Chỉ áp dụng cho phiên — luôn mở ở trạng thái tắt. |
| Menu thả xuống **Cảm biến ánh sáng** | Kết nối cảm biến ánh sáng DAQ (DAQ-E/M/U, từ danh sách trên tab Cảm biến ánh sáng) với camera này để hiệu chỉnh ánh sáng chiếu xuống (DLS) và tự động phơi sáng dự đoán. &quot;&quot;Không&quot; sẽ xóa liên kết. Nếu không có cảm biến nào được kết nối, menu thả xuống sẽ hiển thị &quot;(không có cảm biến nào được kết nối — mở tab DAQ)&quot;. Việc gán này được lưu cùng với dự án. |

#### Bảng Mục tiêu hiệu chuẩn

Bảng này chỉ **hiển thị kết quả xác thực** (&quot;chỉ xác thực — không sửa đổi hiệu chuẩn&quot;). Tiêu đề của bảng liệt kê số sê-ri máy ảnh, bộ lọc, số mục tiêu và kích thước điểm đánh dấu tính bằng pixel, và mỗi bảng có một hàng tương ứng với mỗi tấm:

* **Bảng**, sau đó là một cột cho mỗi dải với giá trị của bảng — máy ảnh Bayer hiển thị ba dải của nó theo tên và bước sóng (ví dụ: FRGN: Red

660 nm / Green

550 nm / NIR

850 nm); một **máy ảnh đơn sắc (M3M) hiển thị dải đơn của nó** (ví dụ: `F750`) và không có cột NDVI

.
* **Sat** — tỷ lệ pixel của bảng bị bão hòa (màu hổ phách từ 1 %, màu đỏ từ 5 %).
* **NDVI

Meas**,**NDVI

Theo** (NDVI

phản xạ tham chiếu của bảng cảm biến) và **Δ**— chỉ áp dụng cho camera đa phổ Bayer. Dấu nhỏ sau &quot;NDVI

Meas&quot; cho biết NDVI

đo được được tính toán từ đâu:**ρ**(phản xạ — có dữ liệu từ cảm biến ánh sáng),**L**(cường độ bức xạ — không có giá trị cảm biến ánh sáng) hoặc**DN** (giá trị thô từ camera). Chỉ có NDVI

dựa trên **ρ** mới có thể so sánh với NDVI

Theo: đối với L và DN, cột Δ sẽ bị mờ đi và có chú thích như sau (&quot;Không có giá trị cảm biến ánh sáng: NDVI

được tính từ độ rạng, do đó Δ không phải là kiểm tra hiệu chuẩn&quot; / &quot;NDVI

được tính từ giá trị thô của máy ảnh, do đó Δ không phải là kiểm tra hiệu chuẩn&quot;). Với ρ, Δ sẽ hiển thị màu xanh lá khi dưới 0,02, màu vàng cam khi dưới 0,05, và màu đỏ trong các trường hợp khác.

<!-- SCREENSHOT-NEEDED: a 1.3.0 Calibration Target table under a live feed in list view — a Bayer camera's table showing the band columns, Sat, and "NDVI Meas ρ" / NDVI Theo / Δ; ideally next to a mono camera's single-band table with no NDVI columns. -->

### Phơi sáng &amp; Độ khuếch đại

<!-- SCREENSHOT-NEEDED: per-camera Exposure & Gain section — Exposure (us) and Gain (dB) rows with Auto/Manual toggles, AE Target Brightness, AE Smoothing slider, AE Region of Interest row with the Aim button, and (on an array camera) AE Tune Speed and Highlight Protection rows. -->

Tất cả các trường nhập số ở đây đều sử dụng thanh trượt tăng tốc khi giữ: chạm = ±1, giữ &gt;1,5 giây = ±10, giữ &gt;3 giây = ±100. Giá trị sẽ được gửi đến camera khi bạn thả tay.

| Điều khiển | Phạm vi / tùy chọn | Mặc định | Áp dụng cho | Chức năng |
| --- | --- | --- | --- | --- |
| **Độ phơi sáng (us)**| Giá trị tối thiểu/tối đa trực tiếp của camera | Tự động | Tất cả | Thời gian phơi sáng tính bằng microgiây, kèm theo nút chuyển đổi**Tự động/Thủ công**. Tự động = phơi sáng tự động liên tục từ phía camera. Khi thiết lập phơi sáng thủ công,**Độ khuếch đại** sẽ vẫn ở chế độ Tự động (nếu trước đó đã ở chế độ này), cho phép sử dụng toàn bộ dải độ khuếch đại của camera, cho đến khi bạn chỉnh sửa độ khuếch đại hoặc chuyển sang chế độ Thủ công. |
| **Độ khuếch đại (dB)**| Giá trị tối thiểu/tối đa trực tiếp của máy ảnh (ví dụ: lên đến 48 dB) | Thủ công (tắt) | Tất cả | Độ khuếch đại analog/kỹ thuật số với công tắc**Tự động/Thủ công** riêng. |
| **Độ sáng mục tiêu AE**| 0–255 | 80, chế độ**Tự động**| Tất cả (có thể chỉnh sửa khi AE hoặc độ khuếch đại tự động đang bật) | Độ sáng mà AE hướng tới. Trong chế độ**Tự động**(mặc định), bộ điều khiển nền dựa trên biểu đồ histogram sẽ tự chọn mức mục tiêu, giữ mức phơi sáng ở 60–75% mức tối đa của cảm biến. Nhập giá trị hoặc kéo đường màu cam trên biểu đồ histogram sẽ chuyển sang chế độ**Thủ công**. |
| **Làm mịn AE** | 0,5–40, bước 0,1 | 8,0 | Tất cả | Độ giảm chấn AE. Chú thích: &quot;Giá trị thấp hơn = AE phản ứng nhanh hơn (có thể nhấp nháy ở tốc độ khung hình cao). Giá trị cao hơn = mượt mà hơn / chậm hơn.&quot; Các giá trị thấp hơn nhiều so với mặc định có thể khiến AE nhấp nháy và làm mất ổn định luồng phát ở tốc độ khung hình cao; 8,0 là giá trị mặc định ổn định. |
| **Vùng quan tâm của AE**| Hộp kiểm Bật + nút**Nhắm**| Tắt | Tất cả | Khi bật, AE chỉ đo vùng màu xanh lá cây đứt nét thay vì toàn bộ khung hình.**Chức năng **Nhắm**kích hoạt tính năng nhấp để đặt trên luồng trực tiếp: nhấp chuột sẽ căn giữa vùng tại vị trí 30% khung hình; nhấp và kéo sẽ vẽ một hình chữ nhật tùy chỉnh (tối thiểu 5% × 5%). Chức năng**Aim** sẽ tự động tắt sau một lần đặt. Vùng này được ánh xạ trở lại tọa độ gốc của máy ảnh theo bất kỳ thiết lập xoay/phản chiếu nào bạn đã đặt, và được lưu cùng với dự án. |
| **Tốc độ điều chỉnh AE** | 0,1–5, bước 0,1 | 1,0 | Chỉ dành cho thành viên mảng | Tốc độ mà mục tiêu AE tự động theo dõi sự thay đổi độ sáng cảnh; 1,0× kiểm tra lại mỗi 2,5 giây. |
| **Bảo vệ vùng sáng** | Nghiêm ngặt (1 %) / Bình thường (5 %) / Nới lỏng (15 %) | Nghiêm ngặt | Các máy ảnh hỗ trợ cài đặt này | Mức độ mà khung hình có thể bị cắt sáng thành màu trắng trước khi AE làm tối hình ảnh. |

{% hint style="info" %}
**Yêu cầu về ánh sáng cho máy ảnh đa quang phổ Bayer (RGN

/ OCN

/ NGB

):** cảnh phải có đủ ánh sáng ở cả ba kênh, nếu không quá trình hiệu chuẩn sẽ không hoạt động chính xác — một lần phơi sáng cảm biến duy nhất bao phủ cả ba dải quang phổ. Sử dụng cảm biến ánh sáng DAQ để đo ánh sáng, hoặc chuyển sang chế độ toàn đơn sắc (M3M) để mỗi dải có thời gian phơi sáng riêng. Nếu một lần chụp vi phạm điều này, Chloros

sẽ phát hiện và cảnh báo bạn (thông báo giới hạn tách dải). Trong chế độ trực tiếp, ô chia kênh sẽ hiển thị **&quot;Dữ liệu dải có độ tin cậy thấp · N%&quot;** khi quá trình tách kênh bị giới hạn ở dải đó trên 25% hoặc nhiều hơn số pixel của dải đó.
{% endhint %}

{% hint style="info" %}
**Cách chế độ phơi sáng tự động phân bổ thời gian phơi sáng và độ khuếch đại (1.3.0).** Khi Gain ở chế độ Tự động, Chloros

duy trì mức tăng (gain) ở 0 dB cho đến khi thời gian phơi sáng đạt đến giá trị dài nhất được phép (giới hạn trên của AE, khoảng thời gian kích hoạt của mảng, hoặc giới hạn phơi sáng của DAQ); chỉ khi đó mức tăng mới tăng lên. Trong một mảng, tần suất kích hoạt được ưu tiên hàng đầu: chế độ phơi sáng tự động lấp đầy khoảng thời gian do tần suất đó quy định, sau đó mới tăng độ khuếch đại; do đó, cảnh tối đi sẽ không còn làm giảm tần suất kích hoạt tự động (trong phiên bản 1.2.0 thì có). Việc thiết lập phơi sáng **thủ công**trên một thành viên mảng với thời gian dài hơn chu kỳ kích hoạt sẽ làm chậm tần suất kích hoạt của mảng để phù hợp với thời gian đó; tốc độ tự động sẽ được khôi phục lại khi tất cả các thành viên đã làm chậm nó đều quay trở lại chế độ phơi sáng tự động. Khi cả**Phơi sáng**và**Độ sáng mục tiêu AE** đều ở chế độ Tự động và cảm biến ánh sáng DAQ cung cấp dữ liệu đo, thời gian phơi sáng của mỗi camera sẽ được giữ nguyên sau khi phơi sáng tự động đã ổn định, miễn là ánh sáng đo được vẫn nằm trong khoảng ±25 %; một sự thay đổi ánh sáng lớn và kéo dài, việc khởi động lại cài đặt (ví dụ: thay đổi vùng quan tâm (ROI)) hoặc việc bạn tự điều chỉnh phơi sáng sẽ kích hoạt lại chế độ này.
{% endhint %}

{% hint style="info" %}
**Cách hoạt động của chế độ phơi sáng tự động (1.3.0).**

* Khi thiết lập phơi sáng thủ công, mức khuếch đại vẫn ở chế độ Tự động cho đến khi bạn thay đổi mức khuếch đại.
* Các vùng sáng nhỏ không làm tối cảnh: vùng phản xạ lên đến 1% khung hình sẽ bị bỏ qua, và trong các tấm của mục tiêu hiệu chuẩn đã biết, lên đến 0,5% diện tích tấm.
* Giới hạn phơi sáng DAQ chỉ áp dụng khi mục tiêu hiệu chuẩn nằm trong khung hình; khi không có tấm nào được nhìn thấy, máy ảnh sẽ phơi sáng theo cảnh.
* Độ phơi sáng duy trì ổn định trong điều kiện ánh sáng không đổi và ổn định lại trong vòng vài giây sau khi có sự thay đổi lớn. Hiện tượng cắt sáng (clip) không còn giới hạn vĩnh viễn độ phơi sáng nữa, và việc thay đổi vùng quan tâm (ROI) sẽ giúp máy ảnh tìm lại mức phơi sáng phù hợp.
* Mục tiêu độ sáng không vượt quá phạm vi của máy ảnh, do đó ánh sáng quay trở lại sẽ không gây lóa khung hình.
* Một camera có chế độ phơi sáng tự động ngừng phản hồi dưới ánh nắng chói chang sẽ được giữ ở mức phơi sáng phù hợp với điều kiện ánh sáng hiện tại — được đo bởi cảm biến ánh sáng hoặc, nếu không có cảm biến nào được kết nối, dựa trên các khung hình của chính camera — và sẽ tự động điều chỉnh lại khi ánh sáng thay đổi. Cảm biến ánh sáng bị che khuất hoặc trong điều kiện tối sẽ không ảnh hưởng đến quá trình này.
{% endhint %}

### Định dạng pixel &amp; Độ phân giải

<!-- SCREENSHOT-NEEDED: per-camera Pixel Format & Resolution section on a STANDALONE camera — Pixel Format, Resolution, and Binning dropdowns plus the Current WxH readout. A second capture on an array member showing the read-only "Set in array settings" state would also be useful. -->

**Các thành viên trong mảng** hiển thị các hàng chỉ đọc “Hiện tại” (định dạng + WxH) và “Binning” kèm chú thích “Được thiết lập trong cài đặt mảng” — việc khởi động lại luồng trên một thành viên sẽ làm mất đồng bộ, do đó các thiết lập này được quản lý trong [bảng cài đặt mảng](#array-settings-pane).**Camera độc lập** có:

| Điều khiển | Lựa chọn | Chức năng |
| --- | --- | --- |
| **Định dạng pixel** | 8-bit / 10-bit / 12-bit / 16-bit | Độ sâu bit cảm biến; nhóm định dạng phụ thuộc vào cảm biến. Giá trị mặc định là 12-bit, và dự án sẽ lưu lại thiết lập của bạn (bao gồm cả 8-bit) và kết nối lại theo thiết lập đó. |
| **Độ phân giải** | Đầy đủ / Nửa / Một phần tư | Tương đối so với chế độ gộp pixel hiện tại: Toàn phần = 2048/N × 1536/N đối với ghép pixel N×N. |
| **Binning** | 1x1 (không) / 2x2 / 4x4 | Ghép pixel N×N bằng phần cứng — các giá trị lớn hơn làm giảm độ phân giải nhưng tăng tỷ lệ tín hiệu trên nhiễu (SNR) và tốc độ khung hình. Thay đổi thông số này sẽ khởi động lại luồng dữ liệu và đặt lại bất kỳ vùng quan tâm (ROI) nào về trường nhìn đầy đủ mới. |
| **Hiện tại** | chỉ đọc | Kích thước WxH thực tế và độ lệch (x, y) đang được áp dụng. |

### Xem trước trực tiếp

Mọi thứ trong phần này **chỉ áp dụng cho màn hình**— nó thay đổi những gì bạn thấy trong luồng trực tiếp, trong khi các bản chụp đã lưu vẫn giữ nguyên và không bị sửa đổi — với một ngoại lệ:**Vignette** là tính năng đo quang và cũng ảnh hưởng đến các tệp xuất (được nêu rõ bên dưới).

<!-- SCREENSHOT-NEEDED: per-camera Live Preview section on an RGB (FRGB) camera — Render resolution, White Balance mode, Gamma, Denoise, Sharpness, Vignette, Color Profile dropdown open showing Raw/Linear/Natural/Enhanced/Custom Temperature, Saturation, Contrast, Mirror H/V and Rotation. -->

<!-- SCREENSHOT-NEEDED: per-camera Live Preview section on a Bayer multispectral (e.g. FRGN) camera — showing the Index row with its gear button (and the absence of the RGB-only White Balance / Gamma / Color Profile / Saturation / Contrast rows). -->



| Điều khiển | Phạm vi / tùy chọn | Mặc định | Áp dụng cho | Chức năng |
| --- | --- | --- | --- | --- |
| **Độ phân giải hiển thị** | 360p (nhanh nhất) / 480p / 720p / 1080p / Độ phân giải cảm biến gốc (chậm nhất) | 720p | Tất cả | Độ cao mà hệ thống nền chạy chuỗi xem trước phóng xạ. Giảm độ phân giải sẽ tăng tốc độ khung hình mà không thay đổi trường nhìn. |
| **Chỉ số**| Hộp kiểm Bật + biểu tượng bánh răng | Tắt | Chỉ dành cho cảm biến đa phổ Bayer,**không** áp dụng cho các cảm biến mảng kết hợp | Xem trước chỉ số thực vật trực tiếp. Biểu tượng bánh răng mở cửa sổ [Trình tính toán chỉ số](#index-calculator-pane) được tải sẵn với các dải phổ tự nhiên của bộ lọc máy ảnh (ví dụ: `Red_660_RGN`, `Green_550_RGN`, `NIR_850_RGN`). Biểu thức tùy chỉnh cùng với LUT (bật/tắt, mức mặc định 7-stop, tối thiểu mặc định 0,2, tối đa mặc định 1) được tính toán trên mỗi khung hình xem trước. Các thành viên mảng kết hợp ẩn hàng này — mảng sở hữu một chỉ số chung. |
| **Cân bằng trắng** | Tắt / Một lần / Liên tục + nút chụp lại | Liên tục | Chỉ RGB

| Cân bằng trắng trực tiếp. Nút làm mới sẽ chụp lại cân bằng trắng từ phổ DLS hiện tại (bị vô hiệu hóa khi chế độ ở trạng thái Tắt). |
| **Gamma** | Bật / Tắt | Bật | Chỉ RGB

| Hiển thị gamma (γ = 2.2 LUT) trên bản xem trước trực tiếp. Các ảnh đã lưu vẫn giữ tính tuyến tính. |
| **Giảm nhiễu** | Hộp chọn + mức độ 0–100 | Tắt / 50 | Tất cả (theo từng camera, kể cả bên trong mảng) | Bộ lọc hai chiều trên bản xem trước trực tiếp. Mức cao hơn = hình ảnh mịn hơn nhưng chi tiết mờ hơn. |
| **Độ sắc nét** | Hộp chọn + mức độ 0–100 | Tắt / 30 | Tất cả | Mặt nạ làm mờ trên bản xem trước trực tiếp, được áp dụng cuối cùng. Có thể làm tăng nhiễu. Chỉ dành cho bản xem trước. |
| **Vignette**| Hộp chọn + mức độ 0–100 | Tắt / 0 | Tất cả | Loại bỏ hiệu ứng vignette thủ công (làm sáng các góc), được chồng lên ước tính Smart Vignette của mảng.**Radiometric — ảnh hưởng đến chế độ xem trực tiếp VÀ các tệp xuất**, khác với Denoise/Sharpness. |
| **Color Profile** | Raw / Linear / Natural / Enhanced / Custom Temperature | Natural | Chỉ RGB

| Xem bên dưới. |
| **Nhiệt độ màu** | 2000–10000 K, bước 100 | 5500 K | Chỉ RGB

, chỉ hồ sơ Nhiệt độ tùy chỉnh | Gắn cân bằng trắng vào một nhiệt độ màu tương quan cố định (bỏ qua đầu vào DLS). Giá trị Kelvin được chọn gần nhất sẽ được ghi nhớ khi chuyển đổi giữa các hồ sơ. |
| **Điểm sáng** | −100 đến 0, bước 1 | 0 (tắt) | Chỉ RGB

| Mới trong phiên bản 1.3.0. Làm tối các vùng sáng nhất, trắng nhất của hình ảnh (0 = không thay đổi, −100 = mức mạnh nhất). Áp dụng cho bản xem trước trực tiếp **và các ảnh chụp ở mức hiển thị (Preview)**, ghi lại cài đặt này (`Chloros:DisplayHighlights`); các tệp xuất độ sáng và độ phản xạ không bao giờ bị ảnh hưởng. Tính năng này không thể khôi phục dữ liệu cảm biến bị cắt — hãy giảm độ phơi sáng để khắc phục điều đó. Lớp phủ zebra sẽ theo hình ảnh đã được làm tối. Tính năng này bị vô hiệu hóa khi sử dụng cấu hình màu Raw (&quot;Không áp dụng khi sử dụng cấu hình màu Raw.&quot;). |
| **Độ bão hòa** | 0–200 (100 = trung tính) | 100 | Chỉ RGB

| Độ bão hòa HSV trên chế độ xem trước trực tiếp. |
| **Độ tương phản** | 0–200 (100 = trung tính) | 100 | Chỉ RGB

| Độ tương phản tuyến tính quanh mức xám trung bình trên bản xem trước trực tiếp. |
| **Lật ngang / Lật dọc** | Hộp chọn | Tắt | Tất cả | Lật bản xem trước theo chiều ngang / chiều dọc. |
| **Xoay**| 0° / 90° / 180° / 270° | 0° | Tất cả | Xoay bản xem trước. Hướng được áp dụng ở cuối chuỗi xem trước phía máy chủ.**Ảnh chụp đã lưu (1.3.0):**các sản phẩm Xem trước, Độ rạng rỡ, Độ phản xạ và Chỉ số của máy ảnh độc lập sẽ được xoay bằng tính năng Gương/Xoay, trong khi**Raw và Debayered vẫn giữ hướng gốc của máy ảnh**; mỗi tệp ảnh ghi lại cài đặt này trong XMP của nó, do đó khi xử lý tệp raw sau này, cài đặt này sẽ được áp dụng theo cách tương tự.**Các tệp thành viên trong mảng vẫn giữ hướng gốc** (cài đặt được ghi lại, không được áp dụng), và các chế độ xem tổng hợp mảng sẽ bỏ qua cài đặt này. |**Ngữ nghĩa của hồ sơ màu** (máy ảnh RGB

):

* **Raw** — bỏ qua hoàn toàn chuỗi xử lý.
* **Linear** — tín hiệu tối + trường phẳng + cân bằng trắng; không có ma trận màu, không có gamma.
* **Tự nhiên** *(mặc định)* — tuyến tính cộng với ma trận hiệu chỉnh màu đã đo và đường cong tông màu thích ứng với cảnh.
* **Nâng cao**— Tự nhiên cộng với độ sống động và độ tương phản cục bộ CLAHE. Chi phí bổ sung chỉ áp dụng cho**xem trước trực tiếp** — các ảnh đã lưu luôn được hoàn thiện đầy đủ bất kể cấu hình nào.
* **Nhiệt độ tùy chỉnh** — Natural với cân bằng trắng được cố định ở giá trị Kelvin do bạn chọn.

{% hint style="warning" %}
Đối với các chế độ Tự nhiên, Nâng cao và Nhiệt độ tùy chỉnh, khung điều khiển hiển thị lưu ý về tông màu: các khung hình được làm sáng theo bối cảnh riêng của chúng, do đó các hình ảnh *hiển thị* đã lưu không thể so sánh trực tiếp giữa các khung hình. **Xuất dữ liệu độ rạng rỡ hoặc độ phản xạ để đo lường.**
{% endhint %}

### Lớp phủ hiển thị (vẽ lên luồng video trực tiếp)

Các tính năng này chỉ hoạt động ở giao diện người dùng — được vẽ lên video, không bao giờ can thiệp vào luồng video hoặc các bản ghi.

<!-- SCREENSHOT-NEEDED: a live feed tile with overlays active — zebra stripes on clipped sky, 3x3 grid, focus peaking in the default orange, and the on-feed histogram strip; the overlays section of the settings pane visible alongside. -->

| Lớp phủ | Điều khiển | Mặc định | Chức năng |
| --- | --- | --- | --- |
| **Zebra** | Hộp chọn + ngưỡng 200–255 | Tắt / 250 | Các sọc chéo màu magenta trên các pixel bị cắt. |
| **Thước ngắm** | Hộp chọn | Tắt | Dấu trung tâm khung hình. |
| **Grid** | Tắt / 3 × 3 / 9 × 9 | Tắt | Lưới bố cục. |
| **Histogram** | Hộp chọn + chiều rộng 0,10–0,90 khung hình | Tắt / 0,25 | Dải biểu đồ histogram hiển thị trên luồng video. |
| **Đỉnh lấy nét** | Hộp chọn + ngưỡng 20–200 + mẫu màu | Tắt / 80 / `#ff5722` | Tô sáng cạnh Sobel để lấy nét. |
| **Tách kênh** | &quot;Hiển thị tách kênh (Red

/ Green

/ NIR

)&quot; / Nút &quot;Ẩn phân tách&quot; | Ẩn | Thêm ba ô thang xám độc lập cho từng kênh bên cạnh hình ảnh tổng hợp (nhãn nút tuân theo các kênh bộ lọc của máy ảnh). Mỗi ô phân tách có thể kéo thả và có cùng màu viền với máy ảnh. Không khả dụng trên máy ảnh đơn sắc. Được lưu cùng với dự án. |

### Máy đo điểm

* **Hộp kiểm &quot;Nhấp để lấy mẫu&quot;**: nhấp vào hình ảnh trực tiếp để lấy mẫu một pixel duy nhất (một thước ngắm hình chữ thập sẽ đánh dấu vị trí đó), hoặc nhấp và kéo một vùng để tính trung bình pixel.**Xóa**sẽ xóa mẫu và thước ngắm. Tính năng này không thể sử dụng đồng thời với chế độ**Nhắm** của AE-ROI.
* **Menu thả xuống**Hiển thị**:**Thô (độ sâu bit)**— các số kỹ thuật số gốc ở độ sâu bit của cảm biến (ví dụ: 12-bit → 0..4095) — hoặc**Hiển thị (8-bit)** (mặc định). Khi chỉ số trực tiếp đang hoạt động, tùy chọn Hiển thị sẽ hiển thị giá trị chỉ số đã tính toán (ví dụ: NDVI

) thay thế.
* Bảng hiển thị liệt kê tọa độ pixel, kích thước khung hình, định dạng pixel, độ sâu bit và bảng kênh (Chan / Giá trị / %) kèm nhãn dải và bước sóng; các cặp màu xanh lá cây theo mô hình Bayer được tính trung bình; các mẫu vùng hiển thị &quot;N px avg&quot;.

Trạng thái máy đo điểm chỉ tồn tại trong phiên làm việc.

<!-- SCREENSHOT-NEEDED: Spot Meter in use — reticle placed on the live feed, readout panel showing the per-channel value table with band wavelength labels. -->

### Tự động phơi sáng dự đoán (điều khiển bởi DLS)

Phần này chỉ xuất hiện khi **có ít nhất một cảm biến ánh sáng DAQ được kết nối** — bộ giải cần có phổ ánh sáng chiếu xuống thời gian thực để điều khiển tính năng này.

<!-- SCREENSHOT-NEEDED: Predictive Auto-Exposure (DLS-driven) section with a DAQ connected — Enable checkbox, Smoothing (α) slider at 0.30, and the "Recalibrate ρ" button. -->



| Điều khiển | Phạm vi | Mặc định | Chức năng |
| --- | --- | --- | --- |
| **Bật** | Hộp chọn | Bật (máy ảnh độc lập) | Bộ giải dạng đóng sử dụng phổ DLS cùng các hằng số trong gói hiệu chuẩn của máy ảnh để đưa dải sáng nhất gần mức bão hòa trong khi giữ dải tối nhất trên ngưỡng SNR tối thiểu — chỉ ghi một lần phơi sáng cho mỗi lần giải, không có vòng lặp ổn định. Được thiết kế cho chế độ chụp time-lapse sử dụng năng lượng mặt trời, nơi mỗi khung hình phải được phơi sáng chính xác. Hệ thống nền sẽ tự động chuyển sang chế độ phơi sáng tự động phản ứng (Reactive AE) mỗi khi dữ liệu DLS đã cũ/thiếu hoặc gói hiệu chuẩn chưa được tải. |
| **Làm mịn (α)** | 0,05–1,0, bước 0,05 | 0,3 | Làm mịn các giải pháp dự đoán liên tiếp (giá trị càng thấp thì càng mịn). |
| **Độ phản xạ cảnh**| Nút**Điều chỉnh lại ρ** | — | Ước tính lại hệ số độ phản xạ cảnh mà bộ giải sử dụng. |

{% hint style="info" %}
**Chế độ kết nối mảng (Array connect) tắt tính năng AE dự đoán theo mặc định** — đối với mảng, tính năng AE thông minh của Chloros

cùng với tính năng tự động phơi sáng (AE) của máy ảnh sẽ xử lý phơi sáng (kèm bảo vệ chống bão hòa), trong khi ước lượng độ phản xạ cảnh đơn lẻkhông an toàn khi xử lý các cảnh hỗn hợp. Bạn có thể bật lại tính năng này cho từng camera tại đây nếu muốn sử dụng phơi sáng phóng xạ dựa trên DLS.
{% endhint %}

**Giới hạn phơi sáng dựa trên DAQ và chế độ AE cố định theo ánh sáng tới.** Bất kể tùy chọn hộp kiểm ở trên, khi cảm biến ánh sáng DAQ được gán cho camera RGB

, Chloros

sẽ tính toán — từ cường độ bức xạ tuyệt đối hướng xuống được đo — mức phơi sáng × độ khuếch đại tối đa mà tại đó bề mặt có độ phản xạ 100% vẫn nằm dưới ngưỡng cắt, và áp dụng nó làm **giới hạn**cho chế độ phơi sáng tự động. Kể từ phiên bản 1.3.0, giới hạn này chỉ áp dụng khi mục tiêu hiệu chuẩn nằm trong tầm nhìn của camera đó (xuất hiện trong khung hình của nó trong vòng 10 giây gần nhất); khi không có mục tiêu trong khung hình, chế độ phơi sáng tự động thông thường của camera — kèm theo cơ chế bảo vệ vùng sáng riêng — sẽ được kích hoạt thay thế, để phần còn lại của cảnh không bị làm tối hơn mức cần thiết. Trong thời gian giới hạn này hoạt động, camera hoạt động ở chế độ**gắn với ánh sáng tới**: nó hoạt động ở chế độ vòng hở với mức phơi sáng được đo theo ánh sáng tới, độ khuếch đại ở 0 dB — mức phơi sáng tuân theo lượng ánh sáng đo được, chứ không phải nội dung cảnh. Vì giới hạn trên chỉ có thể rút ngắn thời gian phơi sáng, nên bản thân nó không thể gây ra hiện tượng cắt sáng. Kể từ phiên bản 1.3.0, ngưỡng trần được tính toán dựa trên hằng số phơi sáng riêng của máy ảnh từ gói hiệu chuẩn (giá trị trung bình của toàn bộ đội chỉ được áp dụng khi gói hiệu chuẩn không chứa giá trị cụ thể). Chế độ ngưỡng trần sẽ tự động ngắt — và chế độ phơi sáng tự động (AE) thông thường sẽ được khôi phục — mỗi khi không có mục tiêu hiệu chuẩn trong khung hình, dữ liệu DAQ bị thiếu, đã cũ (&gt;30 giây), hoặc tối, hoặc nếu ≥15% khung hình bị cắt tại mức phơi sáng cố định (nghĩa là cảm biến và máy ảnh nhận được mức chiếu sáng khác nhau). Không có công tắc trên giao diện người dùng; đây là hành vi tiêu chuẩn mỗi khi máy ảnh RGB

được kết nối với DAQ.

### Thu thập dữ liệu &amp; Kích hoạt

Các thành viên trong<!-- SCREENSHOT-NEEDED: Acquisition & Trigger section on a standalone camera — Trigger Mode, Trigger Source, and the Frame Rate row in Auto mode showing live fps; ideally a second capture on an array member showing the read-only Role/Sync Line/Peers rows. -->

mảng còn hiển thị các hàng chỉ đọc **Vai trò**(Master màu xanh dương / Slave màu xanh lá),**Dòng đồng bộ**và**Các thiết bị cùng cấp**.

| Điều khiển | Lựa chọn | Mặc định | Ghi chú |
| --- | --- | --- | --- |
| **Chế độ kích hoạt** | Tắt / Bật | Bật | Bị vô hiệu hóa đối với các thành viên mảng (mảng quản lý việc kích hoạt). |
| **Nguồn kích hoạt** | Phần mềm / Dòng 0 (M8) / Dòng 1 / Dòng 2 | Dòng 0 | Ẩn khi Chế độ kích hoạt ở trạng thái Tắt; bị vô hiệu hóa đối với các thành viên trong mảng. Dòng 0 là đầu vào kích hoạt bên ngoài cách ly quang học M8. |
| **Tốc độ khung hình**| Tự động / Thủ công + giá trị | Tự động |**Tự động**: giới hạn tốc độ khung hình của máy ảnh bị tắt — thời gian phơi sáng quyết định fps, và hộp hiển thị tốc độ thực tế trực tiếp.**Thủ công**: bạn giới hạn fps bằng thanh trượt (từ 1 đến mức tối đa bị giới hạn bởi băng thông), lấy giá trị từ tốc độ thực tế hiện tại. Các thành viên trong mảng sẽ thấy thông tin chỉ đọc &quot;N fps (trực tiếp)&quot; kèm theo &quot;Đặt trong cài đặt mảng&quot;. |

### Mạng / Truyền tải

| Cột | Hành vi |
| --- | --- |
| **Kích thước gói tin**| 1500 (Tiêu chuẩn) / 9000 (Jumbo) — mặc định là**Jumbo**. |
| **Thông lượng** | Giới hạn thông lượng liên kết chỉ đọc tính bằng MB/s. Hệ thống phía sau sẽ tự động cân bằng lại giá trị này trên tất cả các camera kết nối mỗi khi kết nối/ngắt kết nối. |
| **Xử lý bộ đệm** | Chế độ xử lý bộ đệm chỉ đọc. |

### Ghi hình

Cửa sổ này kết thúc bằng nút **&quot;Mở Cài đặt Ghi hình…&quot;**, giúp chuyển đến [Cửa sổ Cài đặt Ghi hình](capture.md#the-capture-settings-pane) (bị vô hiệu hóa cho đến khi mở dự án — &quot;Tạo hoặc mở một dự án để lưu các bản ghi&quot;). Nếu camera bị ẩn hoặc đang tạm dừng, một lời nhắc sẽ thông báo bạn cần hiển thị lại/tiếp tục trước khi ghi hình.

## Cửa sổ cài đặt mảng

Mở bằng cách nhấp vào biểu tượng **bánh răng**trên một hàng MẠNG. Tiêu đề: tên mạng kèm biểu tượng bút chì đổi tên và**×** để đóng. Các phần bên dưới được đánh dấu *chỉ dành cho chế độ kết hợp* chỉ xuất hiện đối với các mạng được kết nối ở chế độ hiển thị kết hợp.

<!-- SCREENSHOT-NEEDED: array settings pane, top portion — array name header, Sync section (Master/Slaves/Sync Line), and Ambient Light Sensor section with the Light Sensor dropdown and the green "Active — all cameras in the array are illumination-corrected" status line. -->



### Đồng bộ

Các hàng **Master**,**Slaves**và**Sync Line** chỉ đọc.

### Cảm biến ánh sáng môi trường

Hiển thị cho cả mảng kết hợp và mảng riêng biệt:

* Menu thả xuống **Light Sensor**— liên kết một DAQ với toàn bộ mảng. Lựa chọn này có hiệu lực ngay lập tức, lan truyền đến menu thả xuống**Cảm biến ánh sáng** của từng camera thành viên (bạn vẫn có thể ghi đè cài đặt cho từng camera), và bắt đầu chuyển tiếp quang phổ đến mảng.
* Dòng **Trạng thái** trực tiếp: Tắt · &quot;Đang chờ quang phổ đầu tiên…&quot; · &quot;Hoạt động — tất cả các camera trong mảng đều đã được hiệu chỉnh độ sáng&quot; · &quot;Không có phổ mới trong 3 giây qua — vẫn đang sử dụng giá trị đọc cuối cùng (không hết thời gian chờ do dữ liệu cũ)…&quot;.
* Lưu ý trong khung: &quot;Điều chỉnh phóng xạ trên toàn mảng. Cài đặt riêng cho từng camera sẽ ghi đè lên cài đặt này.&quot;

### Mục tiêu hiệu chuẩn

Nhóm riêng ngay sau Cảm biến ánh sáng môi trường, dành cho cả mảng kết hợp và mảng riêng biệt:

* Hộp kiểm **Bật** — &quot;Phát hiện mục tiêu MAPIR

ArUco và xác thực NDVI

so với bảng tra cứu phản xạ của bảng&quot;; điều khiển lớp phủ mục tiêu và bảng xác thực của ô ghép. Trên mảng **riêng biệt**, tùy chọn này sẽ kích hoạt [bảng Mục tiêu hiệu chuẩn](#the-calibration-target-table) (mỗi hàng camera trong chế độ xem danh sách có một bảng).

### Ghi hình — cài đặt cảm biến đồng nhất

Hiển thị cho cả mảng kết hợp và mảng riêng biệt (tiêu đề nhóm ghi là &quot;Ghi hình (đồng nhất trên các camera)&quot;). Các cài đặt này được áp dụng đồng nhất cho mọi thành viên (việc thay đổi riêng lẻ cho từng thành viên sẽ làm mất đồng bộ). Các chỉnh sửa được lưu tạm và áp dụng cùng lúc khi nhấn **Áp dụng**— ngoại trừ công tắc**Bật/Tắt ROI**, công tắc này sẽ áp dụng ngay lập tức.

<!-- SCREENSHOT-NEEDED: array settings Capture section (1.3.0) — Pixel Format, Binning, Resolution preset, the ROI crop row with the "ROI on" toggle, the "Select" button, the "max WxH" hint and Reset button, the W/H/X/Y fields, the full-sensor ROI map below them with the orange crop box and its "W×H @ X,Y" label above the box, Trigger Rate row in Auto showing the derived fps, and the Apply/Cancel buttons. -->

| Điều khiển | Lựa chọn / phạm vi | Chức năng |
| --- | --- | --- |
| **Định dạng pixel** | 8-bit / 10-bit / 12-bit / 16-bit | Độ sâu bit đồng nhất cho tất cả các thành viên; mỗi camera sẽ sử dụng định dạng độ sâu đó cho cảm biến của riêng mình. |
| **Binning** | 1x1 / 2x2 / 4x4 | Ghép pixel bằng phần cứng — giữ nguyên toàn bộ trường nhìn đồng thời tăng tỷ lệ tín hiệu trên nhiễu (SNR) và tốc độ khung hình. Việc thay đổi cài đặt này sẽ đặt lại các trường ROI về trường nhìn đầy đủ mới. |
| Cài đặt sẵn **Độ phân giải** | Đầy đủ / Nửa / Một phần tư | Tương ứng với chế độ gộp pixel; điền các trường ROI bằng vùng cắt trung tâm. |
| **Cắt ROI (px)**| Công tắc**Bật ROI / Tắt ROI**, nút**Chọn**, các trường W / H / X / Y, bản đồ cảm biến đầy đủ | Cắt cảm biến — xem [Trình chỉnh sửa ROI](#the-roi-editor). Chiều rộng/chiều cao tự động căn chỉnh theo bội số của 16 (tối thiểu 64); độ lệch khớp với bội số của 4. Gợi ý &quot;max WxH&quot; hiển thị giới hạn trên và**Reset** đưa các trường trở về trường nhìn đầy đủ. Trong khi chỉnh sửa, hộp cắt màu cam theo dõi mọi thay đổi trường trên bản đồ và trên ô mảng. |
| **Tốc độ kích hoạt**| Chuyển đổi Tự động / Thủ công + fps 0,5–10, bước 0,5 |**Tự động**(mặc định): hệ thống phía sau tính toán tốc độ kích hoạt dựa trên độ phân giải và băng thông — trường nhập bị vô hiệu hóa và hiển thị giá trị đã tính toán.**Thủ công**: cố định giá trị của bạn khi nhấn Áp dụng. Tốc độ đã cố định mà kích thước khung hình mới không thể hỗ trợ sẽ được giữ ở tốc độ nhanh nhất mà kích thước đó cho phép, và sẽ quay lại khi kích thước khung hình cho phép lại. |

Lưu ý trong khung: &quot;Thay đổi định dạng/độ phân giải sẽ khởi động lại tất cả camera trong thời gian ngắn. Tần suất kích hoạt được áp dụng ngay lập tức.&quot; Các nút **Áp dụng / Hủy** nằm ở cuối khung.

#### Trình chỉnh sửa ROI

Mới trong phiên bản 1.3.0. Dòng cắt ROI và bản đồ bên dưới hoạt động cùng nhau:

* **Bật ROI / Tắt ROI**— bật hoặc tắt vùng cắt cảm biến**ngay lập tức**(các camera sẽ khởi động lại trong thời gian ngắn; ô mảng hiển thị &quot;Đang áp dụng cài đặt…&quot;).**Tắt**sẽ đọc toàn bộ cảm biến và**ghi nhớ vùng cắt của bạn**— các trường vẫn hiển thị thông tin này;**Bật** sẽ áp dụng lại vùng cắt. Bật ROI mà không thiết lập vùng cắt sẽ áp dụng vùng cắt kích thước bằng một nửa và được căn giữa (&quot;Chưa thiết lập vùng cắt, nên vùng cắt kích thước bằng một nửa và được căn giữa đã được áp dụng. Điều chỉnh bằng các trường.&quot;).
* **Chọn**— vẽ vùng cắt trên bản đồ: kéo qua vùng đó hoặc nhấp vào hai góc đối diện (nhấn Esc để hủy). Hình chữ nhật sẽ được hiển thị trong các trường W/H/X/Y, được căn chỉnh theo các bước trên; không có gì được áp dụng cho đến khi bạn nhấn**Áp dụng**, vì vậy bạn có thể tinh chỉnh nó bằng các mũi tên trường trước. Mỗi lần nhấn Chọn sẽ tạo ra một hình chữ nhật.
* **Bản đồ**hiển thị vùng cắt trên toàn bộ cảm biến, vì vậy bạn có thể mở rộng nó trở lại: &quot;Toàn bộ cảm biến, trực tiếp&quot; trong khi các camera đọc toàn bộ cảm biến, hoặc “Ảnh chụp toàn khung hình từ {thời gian}. Máy ảnh chỉ đọc vùng cắt.” khi chúng đang ở chế độ cắt. Nếu mảng này trước đây chỉ truyền dữ liệu ở chế độ cắt, bản đồ sẽ hiển thị “Chưa có chế độ xem toàn khung hình: mảng này chỉ truyền dữ liệu ở chế độ cắt.&quot; kèm nút**Hiển thị toàn khung**, giúp tắt vùng quan tâm (ROI). Nhãn vùng cắt `W×H @ X,Y` nằm bên ngoài khung (phía trên hoặc phía dưới khi khung chạm đến mép trên), trên bản đồ và trên ô trực tiếp.
* **Áp dụng**sẽ lưu các thiết lập: một vùng cắt sẽ bật ROI; chế độ toàn khung hình (sau khi**Đặt lại**) sẽ tắt ROI nhưng vẫn ghi nhớ vùng cắt trước đó; một ROI đang tắt và các thiết lập vẫn hiển thị vùng cắt đã ghi nhớ sẽ tiếp tục tắt khi bạn áp dụng thay đổi khác (định dạng, gộp pixel, tốc độ).
* **Trên một mảng đã được căn chỉnh, cửa sổ cắt của mỗi thành viên được đặt theo kết quả căn chỉnh**, do đó mọi camera đều lấy khung cùng một vùng của cảnh. Một mảng đã được cắt trước khi được căn chỉnh sẽ được căn chỉnh trên toàn bộ cảm biến trước tiên (vùng cắt được gỡ bỏ, vấn đề căn chỉnh được giải quyết, và vùng cắt được khôi phục với các cửa sổ riêng cho từng camera).
* Sau khi thay đổi vùng cắt, tính năng phơi sáng tự động sẽ tự động điều chỉnh lại cho khung hình mới. Trong vùng cắt, thời gian phơi sáng của một thành viên vẫn có thể kéo dài đến hết khoảng thời gian kích hoạt.
* Trạng thái bật/tắt vùng quan tâm (ROI) và khung cắt đã lưu sẽ được lưu cùng với dự án.

### Căn chỉnh (đồng bộ hóa)

Hiển thị cho **cả mảng kết hợp và mảng riêng biệt** (phiên bản 1.2.0 chỉ hiển thị cho mảng kết hợp); tiêu đề nhóm ghi là &quot;Căn chỉnh&quot;.

<!-- SCREENSHOT-NEEDED: array settings Alignment section after a successful calibration — green "RMS x.xx px" residual pill, "✓ All cameras aligned (N)" summary, the per-camera table with px error / match count / NCC columns, the Recalibrate alignment button and the "Auto-expose cameras for alignment" checkbox. -->



* Khung **dư**: &quot;RMS x.xx px&quot; — màu xanh lá khi dưới 1 px, màu vàng khi dưới 3 px, màu đỏ trong các trường hợp khác hoặc nếu có camera nào đó không hoạt động; &quot;không có cấu hình&quot; trước lần giải quyết đầu tiên.
* Dòng tóm tắt: &quot;✓ Tất cả camera đã được căn chỉnh (N)&quot; / &quot;⚠ p/N camera đã được căn chỉnh —  <serial (filter)="">thất bại&quot; / &quot;Chế độ cắt xén đang hoạt động — Hiệu chỉnh lại để căn chỉnh (sử dụng toàn bộ cảm biến)&quot; / &quot;Đang chờ phơi sáng ổn định…&quot; / &quot;Đang áp dụng cài đặt…&quot; (các camera đang khởi động lại) / &quot;Mảng camera ngừng cung cấp khung hình — kết nối lại mảng&quot;.
* Bảng theo từng camera: camera (4 ký tự cuối của số sê-ri + bộ lọc), sai số tái chiếu tính bằng px kèm theo số lần khớp (&quot;ref&quot; cho camera chính), và điểm số tương quan chéo chuẩn hóa của vùng chồng chéo so với ngưỡng 0,35.
* **Nắn lại căn chỉnh** Nút (hiển thị &quot;Hiệu chỉnh căn chỉnh&quot; trước hồ sơ đầu tiên) — chạy lại quá trình đăng ký chung trên các khung hình mới.
* **Hộp kiểm &quot;Tự động phơi sáng camera để căn chỉnh&quot;** (được chọn mặc định) — tạm thời làm sáng các camera tối hoặc phẳng (phơi sáng trước, sau đó là độ khuếch đại) để chúng có kết cấu phù hợp, sau đó khôi phục AE.

Chloros tự động căn chỉnh mọi mảng sau khi kết nối, trong cả hai chế độ hiển thị, ngay khi độ phơi sáng tự động của các thành viên đã ổn định (nó sẽ tiếp tục thử trong tối đa 5 phút); bản xem trước kết hợp cũng được căn chỉnh khi mở. Một mảng riêng lẻ không có ô ghép tổng hợp, do đó phần này kiểm tra trạng thái căn chỉnh và tự động chạy chức năng Tái hiệu chuẩn. Tái hiệu chuẩn nếu tiêu điểm hoặc độ sâu cảnh thay đổi. Căn chỉnh **chỉ áp dụng trong phiên làm việc theo thiết kế** — nó không bao giờ được lưu vào hồ sơ, vì nó phụ thuộc vào khoảng cách cảnh tại thời điểm đó. Các bản chụp vẫn có thể được xuất với tính năng căn chỉnh pixel (xem [Xuất hình ảnh đã căn chỉnh](capture.md#per-array-controls)).

### Hiệu ứng Vignette Thông minh

* Hộp kiểm **Bật hiệu chỉnh**— áp dụng ước tính hiệu ứng vignette cho từng camera vào chuỗi đo lường bức xạ (cả chế độ trực tiếp**và** khi xuất).
* ****Hiệu chuẩn từ khung nhìn hiện tại**— trước tiên hãy hướng mảng camera vào một mục tiêu đồng nhất (màn hình phẳng, tường hoặc bầu trời); mỗi camera sẽ được hiệu chỉnh độ phẳng riêng lẻ và trạng thái báo cáo mức tăng độ phẳng &quot;n/N camera · −x.x %&quot;. Nhấn**Xóa** để loại bỏ ước tính.
* Trạng thái: &quot;Đã hiệu chuẩn&quot;, &quot;Chưa hiệu chuẩn&quot; hoặc &quot;Đã hiệu chuẩn một phần — chạy lại&quot; khi chỉ một số ước tính của các camera vẫn áp dụng được ở khung hình hiện tại (ví dụ: sau khi thao tác **Hiệu chuẩn lại**làm thay đổi cửa sổ cắt của một camera) — hãy chạy lại lệnh**Hiệu chuẩn**.
* Tinh chỉnh từng máy ảnh bằng thanh trượt **Vignette** dành riêng cho từng máy ảnh trong [Xem trước trực tiếp](#live-preview).

### Xem trước trực tiếp

* Hộp kiểm **Biểu đồ histogram**+ thanh trượt độ rộng (0,10–0,90 khung hình) — dải biểu đồ histogram trên luồng trực tiếp hiển thị đồng thời trên từng ô hình của mỗi thành viên (bao gồm cả ô hình kênh tách biệt), cùng với [Lớp phủ biểu đồ histogram](#display-overlays-drawn-over-the-live-feed) được thiết lập cho toàn bộ mảng. Tính năng này hiển thị trạng thái “bật” khi mọi thành viên đều đã có sẵn. Trên một mảng**riêng biệt**, đây là hàng duy nhất trong nhóm.
* *Chỉ áp dụng cho chế độ kết hợp:* **Chỉ số**: hộp kiểm kích hoạt + biểu tượng bánh răng — mở [Trình tính toán chỉ số](#index-calculator-pane) chung với các dải được vẽ từ**tất cả** camera thành viên. Dòng xem trước biểu thức bên dưới hiển thị biểu thức hiện tại (&quot;Chưa thiết lập biểu thức — mở trình tính toán để tạo một biểu thức&quot;), được làm mới mỗi giây.
* *Chỉ áp dụng khi kết hợp:* Menu thả xuống **Độ phân giải hiển thị**(các cài đặt sẵn giống như từng camera, mặc định 720p): chiều cao luồng xem trực tiếp**và** kích thước xuất hình ảnh tổng hợp đã lưu. Lưu ý trong khung: &quot;Kích thước xem trước + kích thước hình ảnh tổng hợp đã lưu. Hình ảnh từng camera luôn được xuất ở độ phân giải đầy đủ.&quot;

### Lớp hiển thị *(chỉ áp dụng khi kết hợp)** Hộp kiểm **Bật** (mặc định tắt — camera chính được hiển thị trực tiếp; bật = hình ảnh ghép lớp).
* **Menu thả xuống**Tiền cảnh**/**Hậu cảnh**: từng camera thành viên (theo tên) hoặc**Chỉ số**. Khi Tiền cảnh là Chỉ số, các pixel nằm ngoài phạm vi LUT Min/Max sẽ hiển thị lớp Hậu cảnh.

### Chế độ chia màn hình *(chỉ áp dụng khi kết hợp)*

**&quot;Hiển thị camera thành viên&quot;**— nút**Chia tách / Ẩn camera thành viên** cho phép thêm luồng trực tiếp riêng của từng camera thành viên dưới dạng các ô lưới riêng biệt bên cạnh hình ảnh ghép. Các ô lưới này đọc từ bộ đệm khung hình hiện có của mảng (không cần kết nối camera bổ sung). Chỉ hiển thị ở chế độ lưới; được lưu theo từng mảng cùng với dự án.

### Tính năng

Một bảng điều khiển chỉ đọc được làm mới mỗi 5 giây:

* **Nhãn cấp độ**: &quot;Ghi hình đồng thời&quot; (xanh lục) · &quot;Ghi hình đồng thời (phát tín hiệu xen kẽ FTD)&quot; (xanh lục) · &quot;Ghi hình xen kẽ (chênh lệch 100 ms)&quot; (vàng) · &quot;Cấu hình quá lớn&quot; (màu đỏ).
* **Tình trạng khung hình**: &quot;x.xx % chưa hoàn thành&quot; — màu xanh lá khi dưới 1 %, màu vàng khi dưới 5 %, màu đỏ khi từ 5 % trở lên.
* ****Dòng kết nối**: &quot;NIC {mbps} Mbps - tốc độ ổn định {MB/s} MB/s&quot;.

Đây là ngân sách băng thông thời gian thực của mảng. Để biết thông tin về fps cơ bản và mô hình mạng — cũng như những thay đổi cần thực hiện khi cấp độ chuyển sang màu vàng cam hoặc đỏ — hãy tham khảo [Mảng nhiều camera](arrays.md) và [Tài liệu tham khảo CLI](../reference/cli-reference.md).

<!-- SCREENSHOT-NEEDED: array settings Capabilities panel showing a green "Simultaneous capture" tier, the frame-health percentage, and the NIC/sustained-throughput line. -->

## Khung &quot;Index Calculator&quot;

Trang thanh bên thứ ba, được chia sẻ giữa công cụ Index cho từng camera và công cụ Index cho mảng kết hợp (mỗi lần chỉ hiển thị một — tiêu đề hiển thị là &quot;Index Calculator — <camera name="">&quot; hoặc &quot;Index Calculator — <array name="">&quot;). Trang này nhận danh sách dải tần (các dải tần tự nhiên của bộ lọc camera- dải tự nhiên của camera, hoặc tất cả các dải trên các thành viên của mảng), biểu thức hiện tại và cấu hình LUT (bật/tắt, mức — mặc định 7 stop, tối thiểu — mặc định 0,2, tối đa — mặc định 1), cùng với biểu đồ histogram chỉ số thời gian thực. Nút **Áp dụng** sẽ xác nhận biểu thức; các thay đổi LUT được áp dụng trực tiếp lên bản xem trước.

<!-- SCREENSHOT-NEEDED: Index Calculator pane open for a combined array — band buttons for all member cameras, an NDVI-style expression in the editor, LUT controls, and the live index histogram. -->

## Cài đặt riêng cho từng máy ảnh so với cài đặt do mảng quản lý

Tham khảo nhanh về vị trí của các cài đặt khi một máy ảnh là thành viên của mảng:

| Do mảng quản lý (chỉ đọc trong bảng điều khiển máy ảnh) | Vẫn là cài đặt riêng cho từng máy ảnh bên trong mảng |
| --- | --- |
| Định dạng pixel, độ phân giải, gộp pixel | Phơi sáng tự động (phơi sáng, độ khuếch đại, mục tiêu, làm mịn, vùng quan tâm) |
| Chế độ/nguồn kích hoạt, tốc độ khung hình | Giảm nhiễu, độ sắc nét, hiệu ứng viền tối |
| | Hướng (phản chiếu/xoay), lớp phủ hiển thị (cài đặt mảng → Chế độ Xem trước Trực tiếp thiết lập biểu đồ histogram cho tất cả các thành viên cùng lúc), đo sáng điểm |
| | Chỉ số (mảng hiển thị riêng biệt), liên kết cảm biến ánh sáng |

Các hành vi chung khác:

* **Hiển thị kết hợp so với hiển thị riêng biệt** được chọn khi kết nối mảng: kết hợp = một ô tổng hợp được căn chỉnh (dòng dữ liệu từ các thành viên chỉ được truyền qua Chế độ xem chia đôi); riêng biệt = mỗi thành viên hiển thị ô đồng bộ của riêng mình. Một camera không bao giờ hiển thị cả dòng dữ liệu độc lập và ô mảng cùng lúc.
* ****Tự động kết nối lại**: mở một dự án đã lưu sẽ khôi phục các máy ảnh và mảng của dự án đó, đồng thời áp dụng lại mọi cài đặt đã lưu vào hệ thống nền trước khi các luồng dữ liệu tiếp tục.
* **Kiểm soát ghi hình**: các máy ảnh bị ẩn hoặc tạm dừng sẽ bị loại trừ khỏi chế độ Ghi tất cả; một mảng chỉ bị chặn hoàn toàn khi TẤT CẢ các thành viên đều bị ẩn/tạm dừng. Xem [Cài đặt &amp; Chế độ ghi hình](capture.md).

## Cách các cài đặt được lưu giữ

Trạng thái của tab camera được lưu **cùng với dự án**, không phải trong trình duyệt:

* Mỗi thay đổi phản ứng sẽ chụp nhanh các camera và mảng vào tệp `cameras.json` của dự án (được lọc nhiễu sau 500 ms). Điều này bao gồm tên và màu sắc của camera, cài đặt phơi sáng/độ khuếch đại/AE, định dạng pixel/độ phân giải/binning, tốc độ kích hoạt, cài đặt xem trước (độ phân giải hiển thị, khử nhiễu, độ sắc nét, hiệu ứng viền tối, hồ sơ màu, vùng sáng nổi bật, độ bão hòa/độ tương phản), hướng, lớp phủ, chia kênh, cấu hình chỉ mục, cài đặt AE dự đoán, vùng quan tâm (ROI) cho AE, tên mảng, chế độ hiển thị, cài đặt chụp mảng (bao gồm vùng cắt ROI, trạng thái bật/tắt ROI và vùng cắt được lưu khi tắt), ngân sách dây (Wire Budget) của mảng, và khối lưới (zoom nguồn cấp, chế độ xem, khóa lưới, thứ tự ô thủ công, camera ẩn, ô đóng, camera hoạt động).
* Các thiết lập liên kết cảm biến ánh sáng được lưu trong tệp `sensors.json` của dự án.
* Khi mở lại dự án, hệ thống sẽ kết nối lại phần cứng và áp dụng lại toàn bộ các thiết lập này.
* **Không mở dự án = chỉ áp dụng cho phiên làm việc**: nếu không có dự án, mọi thiết lập sẽ bị xóa khi đóng Chloros.
* Chỉ áp dụng cho phiên làm việc bất kể có dự án hay không: trạng thái tạm dừng, mẫu đo sáng điểm, hộp kiểm “Mục tiêu hiệu chuẩn” cho từng camera (luôn mở ở trạng thái tắt) và cấu hình căn chỉnh mảng (được tính toán lại cho mỗi phiên theo thiết kế).
* Một ngoại lệ: các lựa chọn xuất **Cài đặt chụp** và chế độ chụp được lưu giữ theo từng dự án trong bộ nhớ ứng dụng cục bộ thay vì `cameras.json` — xem [Cài đặt &amp; Chế độ chụp](capture.md).</array></camera></serial>
