# Giao diện người dùng (GUI): Điều hướng

Khi bạn khởi chạy Chloros lần đầu tiên, phần xử lý nền của ứng dụng sẽ được khởi động. Ngay khi phần xử lý nền sẵn sàng, biểu tượng menu chính ở góc trên bên trái sẽ xuất hiện <img src=".gitbook/assets/image (1) (1) (1) (1).png" alt="" data-size="line"> và các tab “Camera” và “Cảm biến ánh sáng” trong thanh bên trái sẽ được mở khóa (trước đó, chúng bị mờ đi).

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Từ trái sang phải, thanh tiêu đề trên cùng bao gồm:

### <img src=".gitbook/assets/image (1) (1) (1) (1).png" alt="" data-size="line"> Menu chính

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>Từ menu chính, bạn có thể:

* **Dự án mới**— tạo một dự án mới. Nếu bạn đã lưu các mẫu dự án, một menu thả xuống**Chọn mẫu** sẽ xuất hiện để dự án mới bắt đầu từ cài đặt của mẫu đó.
* **Mở dự án**— mở một dự án hiện có. Danh sách này bao gồm nút**Mở Thư mục Dự án** để mở thư mục dự án trong trình quản lý tệp của bạn.
* **Sao chép Dự án** — sao chép dự án đang mở dưới một tên mới (được đề xuất là tên tự do như &quot;MyProject (2)&quot;) và mở bản sao đó. _(hiển thị sau khi dự án được mở)_
* **Thêm tệp** — thêm các tệp hình ảnh riêng lẻ vào dự án hiện tại _(hiển thị sau khi dự án được mở)_
* **Thêm thư mục** — thêm một hoặc nhiều thư mục chứa hình ảnh vào dự án hiện tại _(hiển thị sau khi dự án được mở)_
* **Bắt đầu xử lý / Dừng xử lý** — bắt đầu hoặc dừng quy trình xử lý hình ảnh _(chức năng này được kích hoạt sau khi đã thêm tệp)_
* **Kết nối với máy ảnh** — chuyển đến [tab Máy ảnh](lattice/) để kết nối máy ảnh hoặc mảng máy ảnh LATTICE. Chức năng này hoạt động ngay cả khi không có dự án nào đang mở.
* **Kết nối với cảm biến ánh sáng** — chuyển đến [tab Cảm biến ánh sáng](daq/) để kết nối cảm biến ánh sáng DAQ. Hoạt động mà không cần mở dự án.

{% hint style="info" %}
**Chỉ dành cho Windows**: Giao diện người dùng Chloros

Desktop có sẵn trên Windows

. Người dùng Linux

nên tham khảo tài liệu về [CLI

](CLI.md) và [Python

SDK

](api-python-sdk.md) để xử lý không giao diện đồ họa.
{% endhint %}

### <img src=".gitbook/assets/image (2) (1) (1).png" alt="" data-size="line">

Nút Phát/Bắt đầu

Khi được bật, nút bắt đầu xử lý sẽ khởi động quy trình xử lý hình ảnh.

### <img src=".gitbook/assets/image (4).png" alt="" data-size="line">

Thanh tiến trình <img src=".gitbook/assets/image (5).png" alt="" data-size="line">



Trong chế độ Chloros

miễn phí, nơi xử lý tất cả tệp theo thứ tự, thanh tiến trình sẽ hiển thị 2 giai đoạn: Phát hiện mục tiêu và Xử lý.

Trong chế độ Chloros

+ có bản quyền, nơi xử lý tất cả tệp đồng thời, thanh tiến trình hiển thị 4 giai đoạn: Phát hiện, Phân tích, Hiệu chỉnh, Xuất. Nếu bạn di chuột qua thanh tiến trình Chloros

+, một bảng điều khiển mở rộng gồm 4 thanh tiến trình sẽ hiện ra để bạn có thể theo dõi quá trình. Nhấp vào thanh tiến trình trên cùng sẽ khóa bảng điều khiển mở rộng; nhấp lại sẽ mở khóa nó.

<figure><img src=".gitbook/assets/plus_prog.JPG" alt=""><figcaption></figcaption></figure>

## Menu bên

Menu thanh bên trái chứa các biểu tượng khác nhau để tương tác, theo thứ tự từ trên xuống dưới như sau:

#### <img src=".gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> [Cài đặt Dự án](project-settings/project-settings.md)

Tab Cài đặt Dự án cho phép bạn điều chỉnh các thiết lập chung của dự án và thiết lập xử lý dự án. Hãy điều chỉnh các thiết lập này trước khi bắt đầu xử lý tệp của bạn.

#### <img src=".gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> Trình duyệt tệp

Thêm tệp/thư mục và xóa tệp khỏi dự án. Các tệp trùng lặp sẽ bị bỏ qua. Đánh dấu vào ô cột mục tiêu cho bất kỳ hình ảnh mục tiêu nào, và quá trình xử lý sẽ chỉ xem xét các hình ảnh đã được đánh dấu làm mục tiêu, giúp tăng tốc đáng kể thời gian xử lý của bạn. Sử dụng nút chuyển đổi Hình ảnh/Siêu dữ liệu để chuyển đổi giữa chế độ xem lưới hình thu nhỏ của hình ảnh đã chọn và bảng siêu dữ liệu chi tiết.

#### <img src=".gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> [Trình xem hình ảnh](image-viewer-gui/opening-an-image-full-screen.md)

Khi nhấp vào một hình ảnh trong trình xem hình ảnh chính, hình ảnh đó sẽ được mở toàn màn hình trong tab Trình xem hình ảnh.

#### <img src=".gitbook/assets/image (3) (1).png" alt="" data-size="line"> [Trình xem bản đồ](image-viewer-gui/map-markers.md)

Xem hình ảnh của bạn trên bản đồ 2D tương tác dựa trên tọa độ GPS của chúng. Hỗ trợ các nhà cung cấp bản đồ Google Maps và ESRI, tự động chọn dịch vụ phù hợp nhất cho vị trí của bạn. Di chuột qua các điểm đánh dấu để xem bản xem trước thu nhỏ của hình ảnh.

#### <img src=".gitbook/assets/image (17).png" alt="" data-size="line"> [Camera](lattice/)

Kết nối và điều khiển camera LATTICE trực tiếp — từng chiếc một hoặc dưới dạng mảng camera đa kênh đồng bộ. Tab này hiển thị các ô xem trước trực tiếp kèm lớp phủ và biểu đồ histogram, cài đặt cho từng camera và từng mảng camera, cùng với Cài đặt Ghi hình để chọn các camera và loại xuất dữ liệu mà tùy chọn &quot;Ghi lại Tất cả&quot; sẽ tạo ra. Tính năng này sẽ khả dụng khi hệ thống nền tảng đã sẵn sàng; xem [phần LATTICE](lattice/) để biết hướng dẫn chi tiết.

#### <img src=".gitbook/assets/image (23).png" alt="" data-size="line"> [Cảm biến ánh sáng](daq/)

Kết nối các cảm biến ánh sáng DAQ — DAQ-U (USB), DAQ-M (Bluetooth) và DAQ-E (Ethernet) — và xem biểu đồ phổ đã hiệu chuẩn trực tiếp của chúng với đơn vị W/m²/nm. Từ đây, bạn có thể ghi các tệp `.daq` vào dự án đang mở, đổi tên cảm biến, chọn cấu hình hiệu chỉnh cap và cập nhật phần mềm hệ thống (firmware) cho DAQ-E. Tính năng này sẽ khả dụng khi hệ thống nền tảng đã sẵn sàng; xem [phần DAQ](daq/) để biết hướng dẫn chi tiết.

#### <img src=".gitbook/assets/icon_log.JPG" alt="" data-size="line"> Nhật ký gỡ lỗi

Kiểm tra nhật ký để xem các thông báo gỡ lỗi khi xảy ra sự cố. Sao chép/tải xuống nhật ký và gửi đến [Bộ phận Hỗ trợ MAPIR](https://www.mapir.camera/community/contact) để được hỗ trợ.

Mới trong phiên bản 1.3.0: biểu tượng lỗi ở đầu tab nhật ký (**Báo cáo lỗi**) sẽ gửi cho MAPIR một báo cáo kèm theo mô tả của bạn, tối đa 10 hình ảnh, nhật ký Chloros và thông tin hệ thống. Bạn phải đăng nhập để gửi báo cáo. Xem [Cách báo cáo lỗi từ tab nhật ký](processing-images-gui/monitoring-the-processing.md#reporting-a-bug-from-the-log-tab).

#### <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> [Đăng nhập người dùng](chloros+-login.md)

Thanh bên đăng nhập người dùng cho phép bạn đăng nhập vào tài khoản Chloros+ của mình để mở khóa các tính năng nâng cao. Bạn cũng có thể xem phiên bản ứng dụng hiện tại, cũng như điều chỉnh ngôn ngữ của văn bản hiển thị trong giao diện người dùng đồ họa (GUI) và giao diện dòng lệnh (CLI) của Chloros.
