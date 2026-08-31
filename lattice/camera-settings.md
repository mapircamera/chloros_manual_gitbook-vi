# Cài đặt máy quay

Tab **Máy quay** là giao diện điều khiển trực tiếp củChloros

dành cho các máy quay LATTICE: một khu vực nguồn cấp chính hiển thị mọi máy quay được kết nối dưới dạng ô trực tiếp, cùng một thanh bên có thể trượt qua lại giữa ba trang — **danh sách máy quay**,**bảng cài đặt**(cài đặt cho từng máy quay, mảng máy quay hoặc cài đặt ghi hình — chỉ một mục tại một thời điểm), và**Trình tính toán chỉ số**. Trang này mô tả chi tiết mọi điều khiển trong danh sách camera, bảng cài đặt cho từng camera và bảng cài đặt mảng camera. Các chế độ ghi hình, lựa chọn loại xuất và quy trình “Capture All” được trình bày trên trang liên quan [Cài đặt &amp; Chế độ ghi hình](capture.md).

Tab “Camera” sẽ xuất hiện trong thanh bên khi hệ thống nền “Chloros

” đã sẵn sàng. Tất cả các điều khiển dưới đây đều tương tác với hệ thống nền cục bộ qua `127.0.0.1:5000`; các thay đổi sẽ được áp dụng ngay lập tức cho camera đang phát trực tiếp trừ khi có ghi chú khác.

## Các loại camera được sử dụng trên trang này

Các điều khiển sẽ hiển thị hoặc ẩn tùy thuộc vào loại camera được chọn. Tài liệu hướng dẫn sử dụng các thuật ngữ sau đây xuyên suốt:

| Thuật ngữ | Ý nghĩa | Kênh bộ lọc |
| --- | --- | --- |
| **CameraRGB** | LATTICE M3C với bộ lọc FRGB (mã sản phẩm chứa `-FRGB`) |Red

/Green

/Blue

|
| **Bayer đa quang phổ** | LATTICE M3C với FRGN, FOCN hoặc FNGB | FRGN:Red

/Green

/NIR

· FOCN:Orange

/Cyan

/NIR

· FNGB:NIR

/Green

/Blue

|
| **Mono (M3M)** | LATTICE M3M — một bộ lọc dải hẹp, một dải đã hiệu chuẩn | Dải đơn |
| **Thành viên mảng** | Một camera được kết nối như một phần của mảng đồng bộ (hiển thị kết hợp hoặc riêng biệt) | Theo bộ lọc của nó |

RGB

các camera được xử lý quang đo (cân bằng trắng, cấu hình màu, gamma); camera đa quang phổ và camera đơn sắc được xử lý theo chuỗi đo xạ và bỏ qua các điều khiển quang học. Các thành viên mảng chuyển các cài đặt cấp luồng (định dạng pixel, độ phân giải, ghép pixel, kích hoạt, tốc độ khung hình) sang mảng — các hàng đó sẽ trở thành chỉ đọc trong ngăn dành cho từng camera và được chuyển sang ngăn cài đặt mảng.

## Khu vực nguồn cấp dữ liệu chính



<!-- SCREENSHOT-NEEDED: Cameras tab with 2+ cameras connected in grid view — live tiles visible with name and fps overlays, sidebar camera list open on the right. -->

Khi không có camera nào được kết nối, khu vực nguồn cấp dữ liệu hiển thị thông báo **&quot;Kết nối camera để bắt đầu&quot;**kèm theo hai nút:**Kết nối Camera**(màu xanh lá, mở hộp thoại kết nối camera đơn) và**Kết nối Mảng** (màu xanh dương, mở hộp thoại kết nối mảng). Các hộp thoại kết nối này được mô tả chi tiết trong [Kết nối Camera](connecting.md); các khái niệm về mảng (đồng bộ hóa, cấp độ, băng thông) được trình bày trong [Mảng nhiều camera](arrays.md). Khi bạn mở một dự án đã lưu có chứa camera, thông báo khởi động sẽ hiển thị một biểu tượng quay vòng kèm theo &quot;Đang mở lại N camera đã lưu…&quot; trong khiChloros

khôi phục các luồng video từ phiên làm việc trước đó.

<!-- SCREENSHOT-NEEDED: Cameras tab empty state — the "Connect a camera to get started" splash with the green Connect Camera and blue Connect Array buttons. -->

### Thanh trên cùng

| Điều khiển | Chức năng |
| --- | --- |
| **Chuyển đổi chế độ xem**| Chuyển đổi giữa**chế độ xem dạng lưới**(tất cả các ô đều là ô) và**chế độ xem dạng danh sách** (mảng chiếm toàn bộ chiều rộng ở trên cùng, MỘT camera đang hoạt động ở bên dưới). Chú thích: &quot;Chuyển sang chế độ lưới&quot; / &quot;Chuyển sang chế độ danh sách&quot;. |
| **Khóa lưới**(ổ khóa) | Mặc định là**khóa** — các ô được cố định tại chỗ. Mở khóa để kéo và sắp xếp lại các ô vào bất kỳ vị trí nào (khoảng trống được giữ nguyên). Lưới sẽ tự động khóa lại mỗi khi có camera mới kết nối. Chú thích công cụ: &quot;Mở khóa lưới (cho phép kéo ô)&quot; / &quot;Khóa lưới (đóng băng các ô tại chỗ)&quot;. |
| Thanh trượt **Zoom nguồn cấp dữ liệu** | Kích thước ô, từ 60 px đến chiều rộng đầy đủ của khung chứa. Các ô duy trì tỷ lệ khung hình 4:3. Khi chiều rộng ô dưới 200 px, các lớp phủ tên và fps sẽ ẩn đi để giữ cho ô gọn gàng. |

### Các ô nguồn cấp dữ liệu

Mỗi camera hiển thị một ô trực tiếp tổng hợp; một camera có thể hiển thị thêm ba ô **chia theo kênh** màu xám (xem [Chia kênh](#display-overlays-drawn-over-the-live-feed)), và các mảng hiển thị một ô kết hợp. Ô đang hoạt động có vòng chọn màu tương ứng với màu của camera (hoặc của mảng) màu.

Khi di chuột qua ô, nút đóng **X** sẽ xuất hiện:

* Đóng ô **tổng hợp** trong khi các ô tách kênh của nó vẫn còn chỉ ẩn ô tổng hợp.
* Đóng **ô hiển thị cuối cùng của một camera độc lập** sẽ ngắt kết nối camera đó.
* **Các ô chia kênh thuộc mảng kết hợp** **không bao giờ ngắt kết nối** camera — chúng chỉ ẩn đi.

Khi lưới được mở khóa, hãy kéo bất kỳ ô nào đến bất kỳ ô trống nào; bố cục sẽ được lưu cùng với dự án.

## Thanh bên — danh sách

<!-- SCREENSHOT-NEEDED: sidebar camera list pane showing a standalone camera row and an ARRAY group with indented member rows, the DAQ on/off pill visible on the array row, plus the Connect Camera / Connect Array / Capture All buttons at the top. -->

camera Trang đầu tiên của thanh bên liệt kê mọi camera và mảng đã kết nối:

* **Kết nối Camera**(màu xanh lá) /**Kết nối Mảng** (màu xanh dương, hiển thị &quot;Đang phát hiện...&quot; trong khi quét). Cả hai đều bị vô hiệu hóa khi hộp thoại kết nối đang mở.
* **Chụp tất cả** (màu đỏ) — chụp tất cả các camera được liệt kê với các loại xuất được chọn trong Cài đặt chụp. Yêu cầu phải có dự án đang mở. Được ghi chi tiết đầy đủ trong [Cài đặt &amp; Chế độ chụp](capture.md).
* **Biểu tượng bánh răng Cài đặt chụp** (bên cạnh Chụp tất cả) — mở [Cửa sổ Cài đặt chụp](capture.md#the-capture-settings-pane). Bị vô hiệu hóa nếu không có dự án hoặc trong khi đang ghi hình.

### Các hàng camera

Mỗi hàng camera hiển thị một đường viền được mã hóa màu (màu tùy chỉnh của camera), nhãn &quot;CAM&quot; — kèm theo chữ **M**màu xanh dương (chính) hoặc chữ**S** (slave) cho các thành viên trong mảng — và tên hiển thị. Tên mặc định là `LATTICE-MODEL (serial)`; bạn có thể đổi tên từ bảng cài đặt riêng cho từng camera. Các nút trên hàng:

| Nút | Tác dụng |
| --- | --- |
| **Mắt**| Bật/tắt hiển thị. Các camera bị ẩn sẽ biến mất khỏi lưới và**không được tính vào tính năng Capture All**. |
| **Bánh răng** | Mở bảng cài đặt riêng cho từng camera (phần tiếp theo). |
| **Tạm dừng / Phát**| Đóng băng bản xem trước trực tiếp**chỉ ở phía hiển thị** — quá trình ghi hình ở phía máy chủ vẫn tiếp tục chạy. Các camera đang tạm dừng không thể ghi hình. |
| **X** | Ngắt kết nối. Giao diện người dùng cập nhật ngay lập tức (dự kiến); quá trình ngắt kết nối ở phía máy chủ có thể mất 10–30 giây để hoàn tất. |

### Hàng mảng

Một hàng mảng hiển thị biểu tượng &quot;ARRAY&quot; với màu của mảng, tên mảng (có thể đổi tên trong cài đặt mảng) và nút **DAQ · bật/tắt**—**bật** khi Cảm biến Ánh sáng cấp mảng được kích hoạt *hoặc* bất kỳ thành viên nào có cảm biến riêng cho từng camera; chú thích của nó liệt kê chính xác cảm biến nào cung cấp dữ liệu cho cái gì. Các camera thành viên được liệt kê lùi vào bên dưới với các hàng riêng của chúng. Các nút trên hàng mảng: **mắt**(ẩn/hiển thị TẤT CẢ các thành viên cùng lúc),**bánh răng**(cửa sổ cài đặt mảng),**X**(ngắt kết nối toàn bộ mảng).

Trạng thái cảm biến ánh sáng (DLS) được sử dụng trong các hàng mảng và bảng cài đặt mảng có bốn trạng thái:**tắt**,**đang chờ**(chưa có phổ),**hoạt động**(có phổ đến trong vòng 3 giây qua), và**lỗi thời** — không có phổ mới trong 3 giây, nhưng giá trị đọc cuối cùng *vẫn được sử dụng* (các giá trị đọc DAQ không bao giờ hết hạn trên đường dẫn thu thập).

Bạn có thể kéo các camera độc lập và toàn bộ nhóm mảng qua nhau trong thanh bên để sắp xếp lại danh sách; các thành viên trong mảng không thể kéo riêng lẻ.

## Bảng cài đặt từng camera

Mở bằng cách nhấp vào biểu tượng **bánh răng** trên hàng camera. Bảng cài đặt sẽ trượt qua danh sách camera.

<!-- SCREENSHOT-NEEDED: per-camera settings pane, top portion — header with color swatch, camera name, rename pencil and close X; live histogram with the orange dashed AE-target line and green mean-luma line; the RGB per-band toggle button visible top-right of the histogram. -->



**Tiêu đề**:**mẫu màu**của camera (nhấp để mở công cụ chọn màu gốc — dùng để thiết lập màu viền thanh bên và màu vòng chọn ô),**tên**kèm nút bút chì**Đổi tên**(nếu lưu tên trống, hệ thống sẽ khôi phục về tên mặc định `MODEL (serial)`), và**×** để đóng.

### Biểu đồ histogram thời gian thực

Ở đầu bảng điều khiển là biểu đồ histogram độ sáng (luma) thời gian thực được tính toán từ bản xem trướcJPEG

với tần suất khoảng 8 Hz. Giá trị trung bình được tính theo trọng số Bayer — (R+2G+B)/4 — để phù hợp với chế độ đo sáng tự động (AE) của chính máy ảnh.

* **Đường đứt nétOrange**= mục tiêu AE. **Kéo ngang để điều chỉnh lại mục tiêu** — một lệnh sẽ được gửi khi thả chuột, và việc kéo sẽ chuyển chế độ mục tiêu AE sang Chế độ Thủ công.
* **Green

đường liền** = giá trị luma trung bình thực tế (kết quả mà hệ thống đo sáng tự động đang cung cấp).
* **RGB

nút** (góc trên bên phải): chuyển đổi hiển thị biểu đồ histogram chồng lớp theo từng dải màu, được tô màu theo bộ lọc của máy ảnh (ví dụ: trên chế độ FRGN: xámNIR

, xanh lá, đỏ). Trên các máy ảnh đơn sắc (M3M), nút này hiển thị &quot;MONO&quot; và bị vô hiệu hóa — chế độ đơn sắc luôn hiển thị biểu đồ histogram độ sáng của dải đơn.
* Nhãn trục X tuân theo độ sâu bit của cảm biến theo định dạng pixel hiện tại: 0..255, 0..1023, 0..4095 hoặc 0..65535.

### Các hàng

<!-- SCREENSHOT-NEEDED: per-camera settings info rows — Model, Radiometric Calibration "Active" badge with the tier/sha/date caption, Calibration Report Download button, Serial, Firmware row showing the "Up to date" state, IP, Temperature readout, Calibration Target checkbox, Light Sensor dropdown. -->

thông tin camera

| Hàng | Hành vi |
| --- | --- |
| **Mẫu** | Chỉ đọc (ví dụ: `LATT-M3C-L87-FRGN`). |
| **Hiệu chuẩn quang phổ** |Green

Biểu tượng **&quot;Hoạt động&quot;**kèm chú thích hiển thị cấp độ hiệu chuẩn, mã băm, ngày hiệu chuẩn và danh sách dải phổ, được tải từ gói hiệu chuẩn của máy ảnh (xem [Hiệu chuẩn quang phổ tại nhà máy](https://mapir.gitbook.io/lattice-camera/calibration/factory-radiometric-calibration)).**Ẩn đối với các máy ảnhRGB

** — các máy ảnh này sử dụng hiệu chuẩn cân bằng trắng quang học, không phải hiệu chuẩn độ rạng rỡ theo từng dải. |
| **Báo cáo hiệu chuẩn**| Nút**Tải xuống** — mở tệp PDF chứng chỉ hiệu chuẩn NIST theo số sê-ri của máy ảnh trong trình xem của hệ điều hành. Nếu chứng chỉ chưađược lưu vào bộ nhớ đệm,Chloros

sẽ hiển thị một thông báo thay thế. |
| **Số sê-ri** | Chỉ đọc. |
| **Phần mềm**| Hiển thị phiên bản hiện tại, sau đó xác định phiên bản có sẵn cho mẫu máy này (được lưu vào bộ nhớ đệm theo từng mẫu — một mảng gồm N máy ảnh sẽ kiểm tra máy chủ một lần). Các trạng thái: &quot;Đang kiểm tra…&quot; → Nút**&quot;Cập nhật lên X&quot;**→ &quot;Đang flash…&quot; → &quot;Đã cập nhật từ A → B&quot; / &quot;Thất bại: …&quot; / &quot;Bỏ qua: …&quot; / màu xanh lá cây**&quot;Đã cập nhật&quot;**. Chú thích của nút cập nhật: &quot;Khôi phục cài đặt gốc + cập nhật firmware + lập trình lại UserSet1. Khoảng 2–3 phút; không ngắt kết nối.&quot; |
| **IP** | Chỉ đọc. |
| **Nhiệt độ** | Chỉ đọc, làm mới mỗi 3 giây. Chuyển sang màu cam khi ≥65 °C và màu đỏ kèm biểu tượng ⚠ khi ≥75 °C. |
| Hộp kiểm **Mục tiêu hiệu chuẩn** | Kích hoạt tính năng phát hiện mục tiêu phản xạ ArUco với bảng xác thực theo từng tấmNDVI

bên dưới nguồn cấp dữ liệu trực tiếp (chế độ xem danh sách). Chỉ dành cho phiên — luôn mở ở trạng thái tắt. |
| Menu thả xuống **Cảm biến ánh sáng** | Kết nối một cảm biến ánh sáng DAQ (DAQ-E/M/U, từ danh sách trong tab Cảm biến ánh sáng) với camera này để hiệu chỉnh ánh sáng chiếu xuống (DLS) và tự động phơi sáng dự đoán. Chọn &quot;None&quot; để hủy liên kết. Nếu không có cảm biến nào được kết nối, menu thả xuống sẽ hiển thị &quot;(không có cảm biến nào được kết nối — mở tab DAQ)&quot;. Liên kết này được lưu cùng với dự án. |

### Phơi sáng &amp; Độ khuếch đại

<!-- SCREENSHOT-NEEDED: per-camera Exposure & Gain section — Exposure (us) and Gain (dB) rows with Auto/Manual toggles, AE Target Brightness, AE Smoothing slider, AE Region of Interest row with the Aim button, and (on an array camera) AE Tune Speed and Highlight Protection rows. -->

Tất cả các trường nhập số ở đây đều sử dụng thanh trượt tăng tốc khi giữ: chạm = ±1, giữ &gt;1,5 giây = ±10, giữ &gt;3 giây = ±100. Giá trị sẽ được gửi đến camera khi bạn thả tay.

| Điều khiển | Phạm vi / tùy chọn | Mặc định | Áp dụng cho | Chức năng |
| --- | --- | --- | --- | --- |
| **Thời gian phơi sáng (us)**| Giá trị tối thiểu/tối đa trực tiếp của máy ảnh | Tự động | Tất cả | Thời gian phơi sáng tính bằng microgiây, kèm công tắc**Tự động/Thủ công**. Tự động = chế độ phơi sáng tự động liên tục do máy ảnh điều khiển. |
| **Độ khuếch đại (dB)**| Giá trị tối thiểu/tối đa trực tiếp của máy ảnh (ví dụ: tối đa 48 dB) | Thủ công (tắt) | Tất cả | Độ khuếch đại tương tự/kỹ thuật số với công tắc**Tự động/Thủ công** riêng. |
| **Độ sáng mục tiêu AE**| 0–255 | 80, chế độ**Tự động**| Tất cả (có thể chỉnh sửa khi AE hoặc độ khuếch đại tự động được bật) | Độ sáng mà AE hướng tới. Ở chế độ**Tự động**(mặc định), bộ điều khiển nền dựa trên biểu đồ histogram sẽ tự chọn mục tiêu, giữ mức phơi sáng ở 60–75 % so với mức tối đa của cảm biến. Nhập một giá trị hoặc kéo đường màu cam trên biểu đồ histogram sẽ chuyển sang chế độ**Thủ công**. |
| **Làm mịn AE** | 0,5–40, bước 0,1 | 8,0 | Tất cả | Độ giảm chấn AE. Chú thích: &quot;Giá trị thấp hơn = AE phản ứng nhanh hơn (có thể nhấp nháy ở tốc độ khung hình cao). Giá trị cao hơn = mượt mà hơn / chậm hơn.&quot; Các giá trị thấp hơn nhiều so với mặc định có thể khiến AE nhấp nháy và làm mất ổn định luồng phát ở tốc độ khung hình cao; 8.0 là giá trị mặc định ổn định. |
| **Vùng quan tâm của AE**| Hộp kiểm Bật + Nút**Aim**| Tắt | Tất cả | Khi bật, AE chỉ đo vùng màu xanh lá cây đứt nét thay vì toàn bộ khung hình. Nút**Aim**kích hoạt tính năng nhấp để đặt trên luồng trực tiếp: một cú nhấp sẽ căn giữa vùng tại 30 % khung hình; nhấp và kéo sẽ vẽ một hình chữ nhật tùy chỉnh (tối thiểu 5% × 5%). Chức năng**Aim** sẽ tự động tắt sau khi đặt một lần. Vùng này được ánh xạ trở lại tọa độ gốc của máy quay theo bất kỳ thiết lập xoay/phản chiếu nào bạn đã đặt, và được lưu cùng với dự án. |
| **Tốc độ điều chỉnh AE** | 0,1–5, bước 0,1 | 1,0 | Chỉ dành cho thành viên mảng | Tốc độ mà mục tiêu AE tự động theo dõi sự thay đổi độ sáng của cảnh; 1.0× kiểm tra lại mỗi 2,5 giây. |
| **Bảo vệ vùng sáng** | Nghiêm ngặt (1%) / Bình thường (5%) / Nới lỏng (15%) | Nghiêm ngặt | Các máy ảnh hỗ trợ cài đặt này | Phần khung hình nào có thể bị cắt sáng thành màu trắng trước khi AE làm tối hình ảnh. |

{% hint style="info" %}
**Yêu cầu về ánh sáng cho máy ảnh đa quang phổ Bayer (RGN

/OCN

/NGB

):** Cảnh phải có đủ ánh sáng trên cả ba kênh, nếu không quá trình hiệu chuẩn sẽ không hoạt động chính xác — một lần phơi sáng cảm biến duy nhất bao phủ cả ba dải quang phổ. Sử dụng cảm biến ánh sáng DAQ để đo ánh sáng, hoặc chuyển sang chế độ toàn đơn sắc (M3M) để mỗi dải có thời gian phơi sáng riêng. Nếu một lần chụp vi phạm yêu cầu này,Chloros

sẽ phát hiện và cảnh báo bạn (thông báo unmix-clamp).
{% endhint %}

### Định dạng pixel &amp; Độ phân giải

<!-- SCREENSHOT-NEEDED: per-camera Pixel Format & Resolution section on a STANDALONE camera — Pixel Format, Resolution, and Binning dropdowns plus the Current WxH readout. A second capture on an array member showing the read-only "Set in array settings" state would also be useful. -->

**Các thành viên mảng** hiển thị các hàng chỉ đọc “Hiện tại” (định dạng + WxH) và “Binning” kèm chú thích “Được thiết lập trong cài đặt mảng” — việc khởi động lại luồng trên một thành viên sẽ làm mất đồng bộ, do đó các thiết lập này được quản lý trong [bảng cài đặt mảng](#array-settings-pane).**Camera độc lập** có các tùy chọn sau:

| Điều khiển | Lựa chọn | Chức năng |
| --- | --- | --- |
| **Định dạng pixel** | BayerRG8 / BayerRG10 / BayerRG12 / BayerRG16 / Mono8 | Định dạng pixel cảm biến (độ sâu bit). |
| **Độ phân giải** | Đầy đủ / Nửa / Một phần tư | Tương đối so với chế độ gộp pixel hiện tại: Đầy đủ = 2048/N × 1536/N đối với gộp pixel N×N. |
| **Gộp điểm ảnh** | 1x1 (không gộp) / 2x2 / 4x4 | Gộp điểm ảnh N×N bằng phần cứng — giá trị lớn hơn sẽ làm giảm độ phân giải nhưng tăng tỷ lệ tín hiệu trên nhiễu (SNR) và tốc độ khung hình. Thay đổi thông số này sẽ khởi động lại luồng dữ liệu và đặt lại bất kỳ vùng quan tâm (ROI) nào về trường nhìn đầy đủ mới. |
| **Hiện tại** | chỉ đọc | Kích thước WxH thực tế và độ lệch (x, y) đang có hiệu lực. |

### Xem trước trực tiếp

Mọi thứ trong phần này đều **chỉ áp dụng cho màn hình**— nó thay đổi những gì bạn thấy trong luồng trực tiếp, trong khi các bản chụp đã lưu vẫn giữ nguyên và không bị sửa đổi — với một ngoại lệ:**Vignette** là tính năng đo quang và cũng ảnh hưởng đến các tệp xuất (được nêu rõ bên dưới).

<!-- SCREENSHOT-NEEDED: per-camera Live Preview section on an RGB (FRGB) camera — Render resolution, White Balance mode, Gamma, Denoise, Sharpness, Vignette, Color Profile dropdown open showing Raw/Linear/Natural/Enhanced/Custom Temperature, Saturation, Contrast, Mirror H/V and Rotation. -->

<!-- SCREENSHOT-NEEDED: per-camera Live Preview section on a Bayer multispectral (e.g. FRGN) camera — showing the Index row with its gear button (and the absence of the RGB-only White Balance / Gamma / Color Profile / Saturation / Contrast rows). -->



| Điều khiển | Phạm vi / tùy chọn | Mặc định | Áp dụng cho | Chức năng |
| --- | --- | --- | --- | --- |
| **Độ phân giải hiển thị** | 360p (nhanh nhất) / 480p / 720p / 1080p / Độ phân giải cảm biến gốc (chậm nhất) | 720p | Tất cả | Độ cao mà hệ thống nền chạy chuỗi xem trước theo hệ đo quang. Giảm giá trị này sẽ tăng tốc độ khung hình mà không thay đổi trường nhìn. |
| **Chỉ số**| Hộp kiểm &quot;Bật&quot; + biểu tượng bánh răng | Tắt | Chỉ dành cho cảm biến đa phổ Bayer,**không** áp dụng cho các cảm biến mảng kết hợp | Xem trước chỉ số thực vật theo thời gian thực. Biểu tượng bánh răng mở cửa sổ [Trình tính toán chỉ số](#index-calculator-pane) đã được tải sẵn với các dải phổ tự nhiên của bộ lọc máy ảnh (ví dụ: `Red_660_RGN`, `Green_550_RGN`, `NIR_850_RGN`). Biểu thức tùy chỉnh kết hợp với LUT (bật/tắt, mức mặc định 3, tối thiểu mặc định 0,2, tối đa mặc định 1) được tính toán trên mỗi khung hình xem trước. Các thành viên mảng kết hợp ẩn hàng này — mảng sở hữu một chỉ số chung. |
| **Cân bằng trắng** | Tắt / Một lần / Liên tục + nút chụp lại | Liên tục | Chỉ dành choRGB

| Cân bằng trắng thời gian thực. Nút làm mới sẽ chụp lại cân bằng trắng từ phổ DLS hiện tại (bị vô hiệu hóa khi chế độ ở trạng thái Tắt). |
| **Gamma** | Bật / Tắt | Bật | Chỉ dành choRGB

| Hiển thị gamma (γ = 2.2 LUT) trên bản xem trước trực tiếp. Các bản chụp đã lưu vẫn giữ tính tuyến tính. |
| **Giảm nhiễu** | Hộp chọn + mức độ 0–100 | Tắt / 50 | Tất cả (theo từng camera, kể cả bên trong mảng) | Bộ lọc hai chiều trên bản xem trước trực tiếp. Mức cao hơn = hình ảnh mịn hơn nhưng chi tiết mờ hơn. |
| **Độ sắc nét** | Hộp chọn + mức độ 0–100 | Tắt / 30 | Tất cả | Mặt nạ làm mờ trên bản xem trước trực tiếp, được áp dụng cuối cùng. Có thể làm tăng nhiễu. Chỉ áp dụng cho bản xem trước. |
| **Vignette**| Hộp chọn + mức độ 0–100 | Tắt / 0 | Tất cả | Loại bỏ hiệu ứng vignette thủ công (làm sáng các góc), được chồng lên ước tính Smart Vignette của mảng.**Radiometric — ảnh hưởng đến chế độ xem trực tiếp VÀ các tệp xuất**, không giống như Denoise/Sharpness. |
| **Color Profile** | Raw / Linear / Natural / Enhanced / Custom Temperature | Natural | Chỉ dành choRGB

| Xem bên dưới. |
| **Nhiệt độ màu** | 2000–10000 K, bước 100 | 5500 K | Chỉ dành choRGB

, hồ sơ Nhiệt độ tùy chỉnh | Gắn cân bằng trắng vào một nhiệt độ màu tương quan cố định (bỏ qua đầu vào DLS). Giá trị Kelvin được chọn gần nhất sẽ được lưu lại khi chuyển đổi giữa các cấu hình. |
| **Độ bão hòa** | 0–200 (100 = trung tính) | 100 | Chỉ dành choRGB

| Độ bão hòa HSV trên bản xem trước trực tiếp. |
| **Độ tương phản** | 0–200 (100 = trung tính) | 100 | Chỉ dành choRGB

| Độ tương phản tuyến tính quanh mức xám trung bình trên bản xem trước trực tiếp. |
| **Lật ngang / Lật dọc** | Hộp chọn | Tắt | Tất cả | Lật bản xem trước theo chiều ngang / chiều dọc. |
| **Xoay**| 0° / 90° / 180° / 270° | 0° | Tất cả | Xoay bản xem trước. Hướng được áp dụng ở cuối chuỗi xử lý xem trước phía sau —**các ảnh đã lưu vẫn giữ hướng gốc của máy ảnh**, và các chế độ xem tổng hợp mảng sẽ bỏ qua điều này. |**Ngữ nghĩa hồ sơ màu** (máy ảnhRGB

):

* **Raw** — bỏ qua hoàn toàn chuỗi xử lý.
* **Linear** — tín hiệu tối + hiệu chỉnh trường phẳng + cân bằng trắng; không có ma trận màu, không có gamma.
* **Natural** *(mặc định)* — chế độ Linear cộng với ma trận hiệu chỉnh màu đã đo và đường cong tông màu thích ứng với cảnh.
* **Enhanced**— Natural cộng với độ sống động (vibrance) và độ tương phản cục bộ CLAHE. Chi phí bổ sung chỉ áp dụng cho**xem trước trực tiếp** — các ảnh đã lưu luôn được xử lý hoàn chỉnh bất kể cấu hình nào.
* **Custom Temperature** — Natural với cân bằng trắng được cố định ở giá trị Kelvin do bạn chọn.

{% hint style="warning" %}
Đối với các chế độ Tự nhiên, Nâng cao và Nhiệt độ tùy chỉnh, khung điều khiển hiển thị một lưu ý về tông màu: các khung hình được làm sáng theo bối cảnh riêng của chúng, do đó các hình ảnh *hiển thị* đã lưu không thể so sánh trực tiếp giữa các khung hình. **Xuất dữ liệu độ rạng rỡ hoặc độ phản xạ để đo lường.**
{% endhint %}

### Lớp phủ hiển thị (vẽ lên luồng trực tiếp)

Đây là các tính năng chỉ dành cho giao diện người dùng — được vẽ lên video, không bao giờ tác động đến luồng trực tiếp hoặc các ảnh chụp.

<!-- SCREENSHOT-NEEDED: a live feed tile with overlays active — zebra stripes on clipped sky, 3x3 grid, focus peaking in the default orange, and the on-feed histogram strip; the overlays section of the settings pane visible alongside. -->

| Lớp phủ | Điều khiển | Mặc định | Chức năng |
| --- | --- | --- | --- |
| **Zebra** | Hộp chọn + ngưỡng 200–255 | Tắt / 250 | Các sọc chéo màu magenta trên các pixel bị cắt. |
| **Crosshair** | Hộp chọn | Tắt | Dấu trung tâm khung hình. |
| **Grid** | Tắt / 3 × 3 / 9 × 9 | Tắt | Lưới bố cục. |
| **Histogram** | Hộp chọn + chiều rộng 0,10–0,90 khung hình | Tắt / 0,25 | Dải biểu đồ histogram hiển thị trên luồng video. |
| **Đỉnh lấy nét** | Hộp chọn + ngưỡng 20–200 + mẫu màu | Tắt / 80 / `#ff5722` | Tô sáng cạnh theo thuật toán Sobel để lấy nét. |
| **Tách kênh** | Nút &quot;Hiển thị tách kênh (Red

/Green

/NIR

)&quot; / &quot;Ẩn tách kênh&quot; | Ẩn | Thêm ba ô xám độc lập cho từng kênh bên cạnh hình ảnh tổng hợp (nhãn nút tuân theo các kênh bộ lọc của máy ảnh). Mỗi ô chia màn hình có thể kéo thả và có cùng màu viền với camera. Tính năng này không khả dụng trên camera đơn sắc. Được lưu cùng với dự án. |

### Máy đo điểm

* **Hộp kiểm**Click to Sample**: nhấp vào luồng trực tiếp để lấy mẫu một pixel duy nhất (một thước ngắm hình chữ thập sẽ đánh dấu nó), hoặc nhấp và kéo một vùng để tính trung bình pixel.**Clear**xóa mẫu và thước ngắm. Tính năng này không thể sử dụng đồng thời với chế độ**Aim** của AE-ROI.
* Menu thả xuống **Show**:**Thô (độ sâu bit)**— các số liệu kỹ thuật số gốc ở độ sâu bit của cảm biến (ví dụ: 12-bit → 0..4095) — hoặc**Hiển thị (8-bit)**(mặc định). Khi chỉ số thời gian thực đang hoạt động, mục**Display** sẽ hiển thị giá trị chỉ số đã tính toán (ví dụ:NDVI

) thay thế.
* Bảng hiển thị liệt kê tọa độ pixel, kích thước khung hình, định dạng pixel, độ sâu bit và bảng kênh (Chan / Value / %) kèm nhãn dải và bước sóng; các cặp màu xanh lá cây theo mô hình Bayer được tính trung bình; các mẫu vùng hiển thị &quot;N px avg&quot;.

Trạng thái máy đo điểm chỉ áp dụng cho phiên làm việc.

<!-- SCREENSHOT-NEEDED: Spot Meter in use — reticle placed on the live feed, readout panel showing the per-channel value table with band wavelength labels. -->

### Tự động phơi sáng dự đoán (điều khiển bằng DLS)

Phần này chỉ xuất hiện khi **có ít nhất một cảm biến ánh sáng DAQ được kết nối** — bộ giải cần phổ ánh sáng chiếu xuống trực tiếp để điều khiển tính năng này.

<!-- SCREENSHOT-NEEDED: Predictive Auto-Exposure (DLS-driven) section with a DAQ connected — Enable checkbox, Smoothing (α) slider at 0.30, and the "Recalibrate ρ" button. -->



| Điều khiển | Phạm vi | Mặc định | Chức năng |
| --- | --- | --- | --- |
| **Bật** | Hộp chọn | Bật (máy ảnh độc lập) | Bộ giải phương trình dạng đóng sử dụng phổ DLS cùng với cáccác giá trị tỷ số trong gói hiệu chuẩn của máy ảnh để đưa dải sáng nhất gần mức bão hòa trong khi giữ dải tối nhất trên ngưỡng SNR tối thiểu — mỗi lần giải thuật ghi một lần phơi sáng, không có vòng lặp ổn định. Được thiết kế cho chế độ chụp time-lapse sử dụng năng lượng mặt trời, nơi mỗi khung hình phải được phơi sáng chính xác. Hệ thống sẽ tự động chuyển sang chế độ phơi sáng phản ứng (Reactive AE) một cách im lặng khi dữ liệu DLS đã cũ/thiếu hoặc gói hiệu chuẩn chưa được tải. |
| **Làm mịn (α)** | 0,05–1,0, bước 0,05 | 0,3 | Làm mịn các giải pháp dự đoán liên tiếp (giá trị càng thấp thì càng mịn). |
| **Độ phản xạ cảnh**| Nút**Điều chỉnh lại ρ** | — | Ước tính lạimà bộ giải sử dụng. |

{% hint style="info" %}
**Chế độ kết nối mảng (Array connect) tắt tính năng AE dự đoán theo mặc định** — đối với mảng, tính năng AE thông minh củaChloros

cùng với tính năng tự động phơi sáng (auto-exposure) của máy ảnh sẽ xử lý phơi sáng (kèm bảo vệ chống bão hòa), và ước lượng độ phản xạ cảnh dự đoán duy nhất của AE dự đoánkhông an toàn khi áp dụng cho các cảnh hỗn hợp. Bạn có thể bật lại tính năng này cho từng camera tại đây nếu muốn sử dụng phơi sáng phóng xạ do DLS điều khiển.
{% endhint %}

**Giới hạn phơi sáng do DAQ điều khiển và chế độ AE cố định theo ánh sáng tới.** Bất kể tùy chọn hộp kiểm ở trên, khi cảm biến ánh sáng DAQ được gán cho máy ảnhRGB

,Chloros

sẽ tính toán — từ cường độ bức xạ tuyệt đối hướng xuống được đo — mức phơi sáng × độ khuếch đại tối đa mà tại đó bề mặt có độ phản xạ 100% vẫn nằm dưới ngưỡng cắt, và áp dụng nó như một **giới hạn trên**cho chế độ phơi sáng tự động. Trong khi giới hạn trên còn hoạt động, máy ảnh sẽ ở chế độ**gắn với cường độ ánh sáng tới**: máy ảnh hoạt động ở chế độ vòng hở với mức phơi sáng được đo từ cường độ ánh sáng tới và độ khuếch đại ở 0 dB — mức phơi sáng sẽ theo dõi cường độ ánh sáng đo được, chứ không phải nội dung cảnh. Vì giới hạn trên chỉ có thể làm giảm thời gian phơi sáng, nên bản thân nó không thể gây ra hiện tượng cắt sáng. Giới hạn trên sẽ tự động ngắt hoạt động — và chế độ phơi sáng tự động (AE) bình thường sẽ tiếp tục — bất cứ khi nào giá trị đọc từ DAQ bị thiếu, đã cũ (&gt;30 giây), hoặc tối, hoặc nếu ≥15% khung hình bị cắt sáng ở mức phơi sáng cố định (có nghĩa là cảm biến và máy ảnh đang nhận được mức chiếu sáng khác nhau). Không có công tắc trên giao diện người dùng (GUI); đây là hành vi tiêu chuẩn mỗi khi máy ảnhRGB

được liên kết với DAQ.

### Thu thập dữ liệu &amp; Kích hoạt

<!-- SCREENSHOT-NEEDED: Acquisition & Trigger section on a standalone camera — Trigger Mode, Trigger Source, and the Frame Rate row in Auto mode showing live fps; ideally a second capture on an array member showing the read-only Role/Sync Line/Peers rows. -->

Các thành viên trong mảng còn hiển thị các hàng chỉ đọc **Vai trò**(Master màu xanh dương / Slave màu xanh lá),**Dòng đồng bộ**và**Các thiết bị cùng mạng**.

| Điều khiển | Lựa chọn | Mặc định | Ghi chú |
| --- | --- | --- | --- |
| **Chế độ kích hoạt** | Tắt / Bật | Bật | Bị vô hiệu hóa đối với các thành viên mảng (mảng quản lý việc kích hoạt). |
| **Nguồn kích hoạt** | Phần mềm / Dòng 0 (M8) / Dòng 1 / Dòng 2 | Dòng 0 | Ẩn khi Chế độ kích hoạt ở trạng thái Tắt; bị vô hiệu hóa đối với các thành viên trong mảng. Dòng 0 là đầu vào kích hoạt bên ngoài cách ly quang học M8. |
| **Tốc độ khung hình**| Tự động / Thủ công + giá trị | Tự động |**Tự động**: giới hạn tốc độ khung hình của máy ảnh bị tắt — thời gian phơi sáng quyết định fps, và hộp hiển thị tốc độ thực tế trực tiếp.**Thủ công**: bạn giới hạn fps bằng thanh trượt (từ 1 đến giá trị tối đa bị giới hạn bởi băng thông), lấy giá trị ban đầu từ tốc độ thực tế hiện tại. Các thành viên mảng sẽ thấy thông tin chỉ đọc &quot;N fps (trực tiếp)&quot; kèm theo &quot;Đặt trong cài đặt mảng&quot;. |

### Mạng / Truyền tải

| Cột | Hành vi |
| --- | --- |
| **Kích thước gói tin**| 1500 (Tiêu chuẩn) / 9000 (Jumbo) — mặc định là**Jumbo**. |
| **Thông lượng** | Giới hạn thông lượng liên kết chỉ đọc tính bằng MB/s. Hệ thống backend sẽ cân bằng lại giá trị này trên tất cả các camera đã kết nối mỗi khi kết nối/ngắt kết nối. |
| **Xử lý bộ đệm** | Chế độ xử lý bộ đệm chỉ đọc. |

### Ghi hình

Cửa sổ này kết thúc bằng nút **&quot;Mở Cài đặt Ghi hình…&quot;**, cho phép chuyển đến [khung Cài đặt Ghi hình](capture.md#the-capture-settings-pane) (bị vô hiệu hóa cho đến khi mở dự án — &quot;Tạo hoặc mở dự án để lưu các bản ghi hình&quot;). Nếu camera bị ẩn hoặc tạm dừng, một lời nhắc sẽ thông báo bạn cần hiển thị lại/tiếp tục trước khi ghi hình.

## Khung cài đặt mảng

Mở bằng cách nhấp vào biểu tượng **bánh răng**trên một hàng ARRAY. Tiêu đề: tên mảng kèm biểu tượng bút chì để đổi tên và biểu tượng**×** để đóng. Các phần bên dưới được đánh dấu *chỉ kết hợp* chỉ xuất hiện đối với các mảng được kết nối ở chế độ hiển thị kết hợp.

<!-- SCREENSHOT-NEEDED: array settings pane, top portion — array name header, Sync section (Master/Slaves/Sync Line), and Ambient Light Sensor section with the Light Sensor dropdown and the green "Active — all cameras in the array are illumination-corrected" status line. -->



### Đồng bộ

Các hàng **Master**,**Slaves**và**Sync Line** chỉ đọc.

### Cảm biến ánh sáng môi trường

Hiển thị cho cả mảng kết hợp và mảng riêng lẻ:

* Hộp kiểm **Mục tiêu hiệu chuẩn** — &quot;Phát hiện mục tiêu ArUco tạiMAPIR

và xác thựcNDVI

so với bảng tra cứu phản xạ (LUT) của bảng điều khiển&quot;; điều khiển lớp phủ mục tiêu và bảng xác thực của ô kết hợp.
* Menu thả xuống **Cảm biến ánh sáng**— liên kết một thiết bị thu thập dữ liệu (DAQ) với toàn bộ mảng. Lựa chọn này có hiệu lực ngay lập tức, được truyền đến menu thả xuống**Cảm biến ánh sáng** của từng camera thành viên (bạn vẫn có thể ghi đè cài đặt cho từng camera) và bắt đầu chuyển tiếp phổ đến mảng.
* Dòng **Trạng thái** trực tiếp: Tắt · &quot;Đang chờ phổ đầu tiên…&quot; · &quot;Hoạt động — tất cả camera trong mảng đã được hiệu chỉnh ánh sáng&quot; · &quot;Không có phổ mới trong 3 giây qua — vẫn đang sử dụng giá trị đọc cuối cùng (không hết thời gian chờ do dữ liệu cũ)…&quot;.
* Lưu ý trong khung: &quot;Điều chỉnh phóng xạ cho toàn bộ mảng. Cài đặt riêng cho từng camera sẽ ghi đè lên cài đặt này.&quot;

### Chụp — cài đặt cảm biến đồng nhất *(chỉ áp dụng kết hợp)*

Các cài đặt này được áp dụng đồng nhất cho mọi thành viên (việc thay đổi riêng lẻ cho từng thành viên sẽ làm mất đồng bộ). Các chỉnh sửa được lưu tạm và áp dụng cùng lúc.

<!-- SCREENSHOT-NEEDED: array settings Capture section — Pixel Format, Binning, Resolution preset, the ROI crop W/H/X/Y fields with the "max WxH" hint and Reset button, Trigger Rate row in Auto showing the derived fps, and the Apply/Cancel buttons; ideally with the live orange crop-preview box visible on the array tile. -->

| Điều khiển | Lựa chọn / phạm vi | Chức năng |
| --- | --- | --- |
| **Định dạng pixel** | BayerRG8 / BayerRG10 / BayerRG12 / BayerRG16 / Mono8 | Định dạng cảm biến đồng nhất cho tất cả các thành viên. |
| **Binning** | 1x1 / 2x2 / 4x4 | Binning phần cứng — giữ nguyên toàn bộ trường nhìn đồng thời tăng tỷ lệ tín hiệu trên nhiễu (SNR) và tốc độ khung hình. Thay đổi thông số này sẽ đặt lại các trường ROI về trường nhìn đầy đủ mới. |
| Cài đặt sẵn **Độ phân giải** | Đầy đủ / Nửa | Một phần tư | Tương đối với cài đặt gộp pixel; lấp đầy các trường ROI bằng vùng cắt trung tâm. |
| **Vùng cắt ROI (px)**| Các trường số W / H / X / Y | Vùng cắt cảm biến. Chiều rộng/chiều cao tự động căn chỉnh theo bội số của 16 (tối thiểu 64); các giá trị dịch chuyển phải là bội số của 4. Gợi ý “max WxH” hiển thị giới hạn trên và**Reset** sẽ quay lại trường nhìn đầy đủ. Trong khi chỉnh sửa, một khung xem trước vùng cắt màu cam sẽ được hiển thị trực tiếp trên ô mảng (bao gồm sơ đồ cảm biến đầy đủ khi mở rộng vùng cắt ra ngoài). |
| **Tần suất kích hoạt**| Chuyển đổi Tự động / Thủ công + fps 0,5–10, bước 0,5 |**Tự động**(mặc định): hệ thống phía sau tính toán tần suất kích hoạt dựa trên độ phân giải và băng thông — trường nhập bị vô hiệu hóa và hiển thị giá trị tính toán được.**Thủ công**: cố định giá trị của bạn khi nhấn Áp dụng. |

Lưu ý trong khung: &quot;Thay đổi định dạng/độ phân giải sẽ khởi động lại tất cả camera trong thời gian ngắn. Tần suất kích hoạt được áp dụng ngay lập tức.&quot; Các nút **Áp dụng / Hủy** nằm ở phía dưới cùng của khung.

### Căn chỉnh (đồng bộ hóa) *(chỉ kết hợp)*

<!-- SCREENSHOT-NEEDED: array settings Alignment section after a successful calibration — green "RMS x.xx px" residual pill, "✓ All cameras aligned (N)" summary, the per-camera table with px error / match count / NCC columns, the Recalibrate alignment button and the "Auto-expose cameras for alignment" checkbox. -->



* Khung **Dư**: &quot;RMS x.xx px&quot; — màu xanh lá khi dưới 1 px, màu vàng khi dưới 3 px, màu đỏ trong các trường hợp khác hoặc nếu có camera nào đó không thành công; &quot;không có cấu hình&quot; trước lần giải quyết đầu tiên.
* Dòng tóm tắt: &quot;✓ Tất cả camera đã được căn chỉnh (N)&quot; / &quot;⚠ p/N camera đã được căn chỉnh —  <serial (filter)="">thất bại&quot; / &quot;Chế độ cắt xén đang hoạt động — Hiệu chỉnh lại để căn chỉnh (sử dụng toàn bộ cảm biến)&quot; / &quot;Đang chờ phơi sáng ổn định…&quot;.
* Bảng thông tin từng camera: camera (4 ký tự cuối của số sê-ri + bộ lọc), sai số tái chiếu tính bằng px kèm số lần khớp (&quot;ref&quot; cho camera chính), và điểm tương quan chéo chuẩn hóa của vùng chồng lấn so với ngưỡng tối thiểu 0,35.
* Nút **&quot;Hiệu chỉnh lại căn chỉnh&quot;** (hiển thị &quot;Hiệu chỉnh căn chỉnh&quot; trước khi tạo hồ sơ đầu tiên) — chạy lại quá trình ghép ảnh trên các khung hình mới.
* **Hộp kiểm &quot;Tự động điều chỉnh phơi sáng máy ảnh để căn chỉnh&quot;** (được chọn mặc định) — tạm thời làm sáng các máy ảnh tối hoặc phẳng (điều chỉnh phơi sáng trước, sau đó là độ khuếch đại) để chúng có kết cấu phù hợp, sau đó khôi phục cài đặt phơi sáng tự động.

Bản xem trước kết hợp sẽ tự động căn chỉnh khi mở; hãy hiệu chỉnh lại nếu tiêu điểm hoặc độ sâu cảnh thay đổi. Việc căn chỉnh là **chỉ dành cho phiên làm việc-chỉ trong phiên làm việc** — nó không bao giờ được lưu vào hồ sơ, vì nó phụ thuộc vào khoảng cách cảnh tại thời điểm đó. Các bản chụp vẫn có thể được xuất với độ đăng ký pixel (xem [Xuất đã căn chỉnh](capture.md#per-array-controls)).

### Vignette thông minh

* Hộp kiểm **Bật hiệu chỉnh**— áp dụng ước tính hiệu ứng vignette cho từng máy ảnh vào chuỗi đo quang (trực tiếp**và** khi xuất).
* **Hiệu chuẩn từ khung nhìn hiện tại**— hướng mảng máy ảnh vào một mục tiêu đồng nhất (màn hình phẳng, tường hoặc bầu trời); mỗi camera sẽ được làm phẳng riêng biệt và trạng thái báo cáo mức độ phẳng &quot;n/N camera · −x.x %&quot;. Nhấn**Xóa** để loại bỏ ước tính.
* Tinh chỉnh từng camera bằng thanh trượt **Vignette** riêng cho từng camera trong [Xem trước trực tiếp](#live-preview).

### Xem trước trực tiếp *(chỉ kết hợp)** **Chỉ số**: kích hoạt ô chọn + biểu tượng bánh răng — mở [Trình tính toán chỉ số](#index-calculator-pane) chung với các dải dữ liệu được lấy từ**tất cả** các camera thành viên. Dòng xem trước biểu thức bên dưới hiển thị biểu thức hiện tại (&quot;Chưa thiết lập biểu thức — mở công cụ tính toán để tạo một biểu thức&quot;), được làm mới mỗi giây.
* Menu thả xuống **Độ phân giải kết xuất**(các cài đặt sẵn giống như cho từng camera, mặc định 720p): chiều cao luồng xem trực tiếp**và** kích thước xuất hình ảnh tổng hợp đã lưu. Lưu ý trong khung: &quot;Xem trước + kích thước hình ảnh tổng hợp đã lưu. Hình ảnh từng camera luôn được xuất ở độ phân giải đầy đủ.&quot;

### Hiển thị lớp *(chỉ kết hợp)** Hộp kiểm **Bật** (mặc định tắt — camera chính được hiển thị trực tiếp; bật = hình ảnh tổng hợp theo lớp).
* Menu thả xuống **Tiền cảnh**/**Hậu cảnh**: từng camera thành viên (theo tên) hoặc**Chỉ số**. Khi Tiền cảnh là Chỉ số, các pixel nằm ngoài phạm vi LUT Min/Max sẽ hiển thị lớp Hậu cảnh.

### Chế độ chia màn hình *(chỉ áp dụng khi kết hợp)*

**&quot;Hiển thị camera thành viên&quot;**— nút**Tách / Ẩn camera thành viên** cho phép thêm luồng trực tiếp riêng của từng camera thành viên dưới dạng các ô lưới riêng biệt bên cạnh hình ảnh ghép. Các ô lưới này đọc từ bộ đệm khung hình hiện có của mảng (không cần kết nối camera bổ sung). Chỉ hiển thị ở chế độ lưới; được lưu theo từng mảng cùng với dự án.

### Tính năng

Một bảng điều khiển chỉ đọc được làm mới mỗi 5 giây:

* **Nhãn cấp độ**: &quot;Ghi hình đồng thời&quot; (xanh lục) · &quot;Ghi hình đồng thời (phát tín hiệu FTD xen kẽ)&quot; (xanh lục) · &quot;Ghi hình xen kẽ (chênh lệch 100 ms)&quot; (vàng) · &quot;Cấu hình quá lớn&quot; (đỏ).
* **Tình trạng khung hình**: &quot;x.xx % chưa hoàn thành&quot; — màu xanh lá cây dưới 1 %, màu vàng cam dưới 5 %, màu đỏ từ 5 % trở lên.
* **Dòng liên kết**: &quot;NIC {mbps} Mbps - tốc độ ổn định {MB/s} MB/s&quot;.

Đây là ngân sách băng thông thời gian thực của mảng camera. Để biết thông tin chi tiết về fps và mô hình mạng cơ bản — cũng như những điều cần thay đổi khi cấp độ chuyển sang màu vàng cam hoặc đỏ — hãy tham khảo [Mảng nhiều camera](arrays.md) và [Tài liệu tham khảo CLI](../reference/cli-reference.md).



<!-- SCREENSHOT-NEEDED: array settings Capabilities panel showing a green "Simultaneous capture" tier, the frame-health percentage, and the NIC/sustained-throughput line. -->## Khung &quot;Index Calculator&quot;

Trang thanh bên thứ ba, được chia sẻ giữa công cụ Index cho từng camera và công cụ Index cho mảng kết hợp (chỉ hiển thị một trong hai tại một thời điểm — tiêu đề sẽ hiển thị &quot;Index Calculator — <camera name="">&quot; hoặc &quot;Trình tính chỉ số — <array name="">&quot;). Trang này nhận danh sách dải tần (các dải tần tự nhiên của bộ lọc camera, hoặc tất cả các dải tần trên các thành viên của mảng), biểu thức hiện tại và cấu hình LUT (bật/tắt, mức — mặc định 3, tối thiểu — mặc định 0,2, tối đa — mặc định 1), cùng với biểu đồ histogram chỉ số trực tiếp. **Áp dụng** sẽ xác nhận biểu thức; các thay đổi LUT được áp dụng trực tiếp lên bản xem trước.

<!-- SCREENSHOT-NEEDED: Index Calculator pane open for a combined array — band buttons for all member cameras, an NDVI-style expression in the editor, LUT controls, and the live index histogram. -->

## Cài đặt riêng cho từng máy ảnh so với cài đặt do mảng quản lý

Tham khảo nhanh về vị trí của các cài đặt khi một máy ảnh là thành viên của mảng:

| Do mảng quản lý (chỉ đọc trong khung máy ảnh) | Vẫn là cài đặt riêng cho từng máy ảnh bên trong mảng |
| --- | --- |
| Định dạng pixel, độ phân giải, gộp pixel | Phơi sáng tự động (phơi sáng, độ khuếch đại, mục tiêu, làm mịn, vùng quan tâm) |
| Chế độ/nguồn kích hoạt, tốc độ khung hình | Giảm nhiễu, độ sắc nét, hiệu ứng viền tối |
| | Hướng (phản chiếu/xoay), lớp phủ hiển thị, đo sáng điểm |
| | Chỉ số (mảng hiển thị riêng biệt), liên kết cảm biến ánh sáng |

Các hành vi chung khác:

* **Hiển thị kết hợp so với riêng biệt** được chọn khi kết nối mảng: kết hợp = một ô tổng hợp được căn chỉnh (các thành viên chỉ truyền tín hiệu qua Chế độ xem chia nhỏ); riêng biệt = mỗi thành viên hiển thị ô đồng bộ của riêng mình. Một camera không bao giờ hiển thị cả luồng độc lập và ô mảng cùng lúc.
* **Tự động kết nối lại**: mở một dự án đã lưu sẽ khôi phục các camera và mảng của dự án đó, đồng thời áp dụng lại mọi cài đặt đã lưu vào hệ thống nền trước khi các luồng phát tiếp tục.
* **Kiểm soát ghi hình**: các camera bị ẩn hoặc tạm dừng sẽ bị loại trừ khỏi tính năng Ghi hình Tất cả; một mảng chỉ bị chặn hoàn toàn khi TẤT CẢ các thành viên đều bị ẩn/tạm dừng. Xem [Cài đặt &amp; Chế độ Ghi hình](capture.md).

## Cách các cài đặt được lưu trữ

Trạng thái của tab camera được lưu **cùng với dự án**, không phải trong trình duyệt:

* Mỗi thay đổi tương tác sẽ chụp nhanh trạng thái của các camera và mảng vào dự án theo định dạng `cameras.json` (được lọc nhiễu trong 500 ms). Điều này bao gồm tên và màu sắc camera, cài đặt phơi sáng/độ khuếch đại/AE, định dạng pixel/độ phân giải/binning, tốc độ kích hoạt, cài đặt xem trước (độ phân giải hiển thị, khử nhiễu, độ sắc nét, hiệu ứng viền tối, cấu hình màu, độ bão hòa/độ tương phản), hướng, lớp phủ, phân chia kênh, cấu hình chỉ mục, cài đặt AE dự đoán, vùng quan tâm (ROI) của AE, tên mảng, chế độ hiển thị, cài đặt chụp mảng (bao gồm vị trí cắt ROI) và khối lưới (phóng to nguồn cấp, chế độ xem, khóa lưới, thứ tự ô thủ công, camera ẩn, ô đóng, camera đang hoạt động).
* Các thiết lập liên kết cảm biến ánh sáng được lưu trong tệp `sensors.json` của dự án.
* Khi mở lại dự án, hệ thống sẽ kết nối lại phần cứng và áp dụng lại toàn bộ các thiết lập này.
* **Không mở dự án = chỉ phiên làm việc**: nếu không có dự án, mọi thiết lập sẽ bị xóa khi đóng Chloros.
* Chỉ phiên làm việcdù có dự án hay không: trạng thái tạm dừng, mẫu đo điểm sáng, hộp kiểm “Mục tiêu hiệu chuẩn” cho từng camera (luôn mở ở trạng thái tắt) và hồ sơ căn chỉnh mảng (được tính toán lại cho mỗi phiên theo thiết kế).
* Một ngoại lệ: các lựa chọn xuất trong **Cài đặt chụp** và chế độ chụp được lưu giữ theo từng dự án trong bộ nhớ ứng dụng cục bộ thay vì trong `cameras.json` — xem [Cài đặt &amp; Chế độ chụp](capture.md).</array></camera></serial>
