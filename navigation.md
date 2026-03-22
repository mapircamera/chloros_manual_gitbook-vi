#GUI: Điều hướng

Khi bạn khởi chạy Chloros và Chloros (Trình duyệt) lần đầu tiên, nó sẽ khởi động phần phụ hỗ trợ của nó. Khi nó đã sẵn sàng, menu biểu tượng chính ở trên cùng bên trái sẽ hiển thị <img src=".gitbook/assets/image (1) (1) (1).png" alt="" data-size="line"> .

<figure><img src=".gitbook/assets/header.JPG" alt=""><figcaption></figcaption></figure>

Từ trái sang phải tiêu đề trên cùng chứa:

### <img src=".gitbook/assets/image (1) (1) (1) (1).png" alt="" data-size="line"> Menu chính

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

Menu từ chính bạn có thể:

* **Dự án mới** — tạo một dự án mới
* **Dự án mở** — mở một dự án hiện có
* **Mở dự án thư mục** — mở dự án thư mục trong quá trình khám phá tệp của bạn
* **Thêm tệp** — thêm các tệp hình ảnh riêng lẻ vào hiện tại dự án _(hiển thị sau khi mở dự án)_
* **Thêm thư mục** — thêm hình ảnh thư mục vào hiện tại dự án _(hiển thị sau khi mở dự án)_
* **Bắt đầu xử lý / Dừng xử lý** — bắt đầu hoặc dừng quá trình xử lý hình ảnh _(được bật sau khi thêm tệp)_

{% hint style="info" %}
**Chỉ Windows**: GUI máy tính để bàn Chloros có sẵn trên Windows. Người dùng Linux nên xem tài liệu [CLI](CLI.md) và [Python SDK](api-python-sdk.md) để xử lý đầu ra.
{% endhint %}

### <img src=".gitbook/assets/image (2) (1).png" alt="" data-size="line"> Nút Phát/Bắt đầu

Khi được bật, nút bắt đầu xử lý sẽ khởi động quá trình xử lý hình ảnh.

### <img src=".gitbook/assets/image (4).png" alt="" data-size="line"> Thanh tiến trình <img src=".gitbook/assets/image (5).png" alt="" data-size="line">

Ở chế độ Chloros miễn phí, xử lý tất cả các tệp theo cách tự động, thanh tiến trình sẽ hiển thị 2 giai đoạn: Phát hiện và Xử lý Mục tiêu.

Ở chế độ được cấp phép Chloros+ trả phí, xử lý đồng thời tất cả các tệp, thanh tiến trình hiển thị 4 giai đoạn: Phát hiện, Phân tích, Hiệu chỉnh, Xuất. Nếu bạn di chuyển con trỏ chuột qua tiến trình Chloros+, nó sẽ thả xuống phần mở rộng tiến trình điều khiển 4 để bạn có thể theo dõi. Nhấn vào tiến trình trên cùng sẽ đóng băng thả xuống, nhấp lại sẽ mở khóa.

<figure><img src=".gitbook/assets/plus_prog.JPG" alt=""><figcaption></figcaption></figure>

## Bên cạnh Menu

Bên trái menu chứa nhiều biểu tượng khác nhau để tương tác:

#### <img src=".gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> [Cài đặt dự án](project-settings/project-settings.md)

Tab Cài đặt dự án cho phép bạn điều chỉnh cài đặt dự án và toàn bộ dự án. Điều chỉnh những điều này trước khi bạn bắt đầu xử lý tệp.

#### <img src=".gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> Trình duyệt tệp

Thêm tệp/thư mục và xóa tệp khỏi dự án. Vòng lặp tập tin sẽ bị bỏ qua. Chọn cột mục tiêu để tìm bất kỳ hình ảnh mục tiêu nào và quá trình xử lý sẽ chỉ xem xét các hình ảnh đã chọn để tìm mục tiêu, giúp tăng tốc đáng kể trong quá trình xử lý thời gian của bạn. Sử dụng nút chuyển đổi Hình ảnh/Siêu dữ liệu để chuyển giữa hình ảnh thu nhỏ của hình ảnh đã chọn và siêu dữ liệu chi tiết của bảng.

#### <img src=".gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> [ Trình hình xem ảnh](image-viewer-gui/opening-an-image-full-screen.md)

Khi một hình ảnh được nhấp vào trong quá trình xem hình ảnh chính, nó sẽ được mở toàn màn hình trong tab Trình xem hình ảnh.

#### <img src=".gitbook/assets/image (7).png" alt="" data-size="line"> [Bản đồ](image-viewer-gui/map-markers.md)

Xem hình ảnh của bạn trên bản đồ 2D tương tác dựa trên chế độ GPS của chúng. Hỗ trợ các nhà cung cấp Google Maps và ESRI, tự động chọn dịch vụ tốt nhất cho vị trí của bạn. Di chuột qua điểm đánh dấu để xem bản xem trước của hình ảnh thu nhỏ của hình ảnh.

#### <img src=".gitbook/assets/icon_log.JPG" alt="" data-size="line"> Nhật ký gỡ lỗi

Xem lại nhật ký để tìm các bản gỡ lỗi khi xảy ra sự cố. Sao chép/tải xuống nhật ký và gửi đến [MAPIR Support](https://www.mapir.camera/community/contact) để được hỗ trợ.

#### <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> [Đăng nhập người dùng](chloros+-login.md)

Người dùng đăng nhập vào thành cho phép bạn đăng nhập vào tài khoản Chloros+ của mình để mở khóa các tính năng nâng cao. Bạn cũng có thể xem phiên bản hiện tại của ứng dụng cũng như điều chỉnh ngôn ngữ của bản văn được hiển thị trong GUI Chloros và CLI.