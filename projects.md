# Giao diện người dùng (GUI): Dự án

Chloroscho phép bạn tạo các dự án có thể mở lại trong tương lai. Một dự án là một thư mục thông thường (nằm trong Thư mục Dự án của bạn) chứa:

* `project.json` — cài đặt dự án, danh sách tệp và tùy chọn hiển thị
* `cameras.json` — các camera và mảng cảm biến được kết nối trong khi dự án đang mở, kèm theo cài đặt của chúng
* `sensors.json` — các cảm biến ánh sáng DAQ được kết nối khi dự án đang mở, cùng với các liên kết camera↔cảm biến
* các bản chụp, bản ghi `.daq` và các thư mục đầu ra đã xử lý của bạn

Không có định dạng tệp dự án độc quyền — thư mục và các tệp JSON trong đó chính là dự án, điều này cũng giúp việc sao chép, lưu trữ và truy cập dự án từ [CLI](CLI.md) hoặc [Python SDK](api-python-sdk.md) trở nên dễ dàng.

## Dự án mới

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>Chọn &quot;Dự án mới&quot; từ menu chính và nhập tên duy nhất cho dự án của bạn.

Nếu bạn đã lưu bất kỳ mẫu dự án nào, một menu thả xuống **Chọn mẫu** sẽ xuất hiện bên dưới trường tên — việc chọn một mẫu sẽ khởi tạo dự án mới dựa trên cài đặt của mẫu đó. Các mẫu dự án được lưu từ [Cài đặt dự án](project-settings/project-settings.md): nhập tên vào trường &quot;Tên mẫu dự án&quot; và nhấp vào biểu tượng lưu.

## Mở dự án

<figure><img src=".gitbook/assets/v120-open-project.jpg" alt=""><figcaption><p>Danh sách &quot;Mở Dự án&quot; hiển thị tất cả các dự án trong thư mục dự án của bạn, với tùy chọn <strong>&quot;Mở Thư mục Dự án&quot;</strong> ở phía dưới</p></figcaption></figure>

Chọn &quot;Mở Dự án&quot; để xem danh sách các dự án hiện có trong Thư mục Dự án. Nếu không có dự án nào, menu phụ bên cạnh sẽ không mở ra. Bạn có thể thấy một số dự án được tạo bởi GUI (t1, t2, t3) được liệt kê trong hình ảnh trên. Các dự án có định dạng DATE\_TIME được tạo bởi Trình tạo dự án tự động (CLI) theo quy ước đặt tên dự án mặc định. Nhấp vào bất kỳ tên dự án nào sẽ mở dự án đó.

Nhấp vào nút &quot;Mở thư mục dự án&quot; sẽ mở Trình quản lý tệp (File Explorer) trên máy tính của bạn tại đường dẫn của dự án. Bạn có thể điều chỉnh đường dẫn dự án trong [Cài đặt Dự án](project-settings/project-settings.md).

Nếu bất kỳ tệp hình ảnh nguồn nào của dự án đã bị di chuyển hoặc xóa kể từ lần mở cuối cùng, Chloros sẽ hiển thị một hộp thoại liệt kê chính xác các tệp nào đang thiếu thay vì mở một lưới trống.

## Sao chép dự án

Tính năng này có sẵn khi dự án đã được mở. Chọn &quot;Sao chép dự án&quot; để sao chép dự án hiện tại dưới một tên mới — Chloros sẽ đề xuất tên trống tiếp theo (ví dụ: &quot;MyProject (2)&quot;) — và bản sao sẽ được mở ngay lập tức.

## Thêm tệp

Sau khi dự án được mở, chọn &quot;Thêm tệp&quot; từ menu chính để thêm các tệp hình ảnh riêng lẻ vào dự án hiện tại. Tính năng này tương tự như chức năng thêm tệp trong trình duyệt tệp nhưng có thể truy cập trực tiếp từ menu chính để thuận tiện hơn.

## Thêm thư mục

Sau khi dự án được mở, hãy chọn &quot;Thêm thư mục&quot; từ menu chính để thêm các thư mục chứa hình ảnh vào dự án hiện tại. Bạn có thể chọn nhiều thư mục cùng một lúc. Các tệp trùng lặp sẽ bị bỏ qua.

## Bắt đầu / Dừng xử lý

Sau khi các tệp được thêm vào dự án, tùy chọn &quot;Bắt đầu xử lý&quot; sẽ xuất hiện trong menu chính. Đây là thao tác tương tự như nhấp vào nút Phát/Bắt đầu ở thanh tiêu đề trên cùng. Trong quá trình xử lý, mục menu này sẽ chuyển thành &quot;Dừng xử lý&quot; để cho phép bạn dừng quy trình xử lý.

## Kết nối với máy ảnh / Kết nối với cảm biến ánh sáng

Phần dưới cùng của menu chính có hai phím tắt phần cứng, có thể sử dụng dù có hay không có dự án đang mở:

* **Kết nối với máy ảnh** — mở [tab Máy ảnh](lattice/) để kết nối máy ảnh hoặc mảng máy ảnh LATTICE.
* **Kết nối với cảm biến ánh sáng** — mở [tab Cảm biến ánh sáng](daq/) để kết nối cảm biến ánh sáng DAQ.

Việc kết nối phần cứng khi dự án đang mở sẽ lưu kết nối đó vào dự án (xem bên dưới). Nếu không có dự án, các kết nối chỉ tồn tại trong phiên làm việc.

{% hint style="info" %}
Các mục menu Thêm tệp, Thêm thư mục và Bắt đầu/Dừng xử lý chỉ hiển thị hoặc được kích hoạt khi dự án đang mở và các tệp đã được thêm vào. Chúng cung cấp quyền truy cập nhanh vào các thao tác cũng có sẵn thông qua thanh bên Trình duyệt tệp và các nút trên thanh tiêu đề.
{% endhint %}

## Dự án ghi nhớ phần cứng của bạn

Mới trong phiên bản 1.2.0: dự án sẽ lưu lại phần cứng bạn kết nối trong suốt thời gian dự án đang mở. Các camera và mảng camera (cùng với cài đặt riêng cho từng camera, tên, màu sắc và bố cục lưới) được lưu lại dưới dạng ảnh chụp nhanh vào tệp `cameras.json`, còn các cảm biến ánh sáng (cùng với tên, màu sắc và liên kết với camera) được lưu vào tệp `sensors.json` — một cách tự động trong quá trình bạn làm việc.

Khi bạn **mở lại** một dự án, Chloros sẽ không kết nối ngay với bất kỳ thiết bị phần cứng nào. Mỗi phần sẽ kết nối lại lần đầu tiên khi bạn truy cập vào tab sở hữu nó:

* Mở tab **Camera** sẽ kết nối lại các camera và mảng đã lưu, đồng thời áp dụng lại các cài đặt đã lưu của chúng.
* Mở tab **Cảm biến ánh sáng** sẽ kết nối lại các cảm biến DAQ đã lưu.

Nhờ đó, việc mở dự án chỉ để duyệt hoặc xuất hình ảnh sẽ không bao giờ kích hoạt chế độ truyền dữ liệu liên tục cho các camera. Nếu không tìm thấy thiết bị đã lưu khi mở tab tương ứng, một hộp thoại sẽ thông báo cho bạn biết những thiết bị nào không khả dụng để bạn có thể kết nối lại hoặc xóa chúng.

## Bản ghi DAQ và tệp .daq trong dự án

* Các bản ghi `.daq` được tạo ra khi dự án đang mở (từ tab Cảm biến ánh sáng hoặc trong quá trình ghi hình) sẽ được **tự động thêm vào dự án**.
* Các tệp `.daq` được nhập vào và tất cả các bản ghi của dự án đều được liệt kê trong phần **Cảm biến Ánh sáng DAQ** của [Cài đặt Dự án](project-settings/project-settings.md), mỗi bản ghi đi kèm với hồ sơ hiệu chỉnh độ chói riêng.
* Trong quá trình xử lý, các tệp `.daq` của dự án cung cấp thông tin chiếu sáng từ trên xuống cho các sản phẩm phản xạ — xem [Định dạng hình ảnh đầu ra](output-image-formats.md).

## Chạy dự án đã lưu mà không cần giao diện người dùng (headless)

Một dự án đã lưu có thể được chạy mà không cần giao diện người dùng (GUI):

* **CLI**: `chloros-cli project open / connect / capture / sensor / align / run` hoạt động dựa trên đường dẫn thư mục dự án — xem [Tài liệu tham khảo CLI](reference/cli-reference.md).
* **SDK**: `chloros_sdk.open_project(path)` trả về một handle dự án; `connect_all()` kích hoạt tất cả các camera và cảm biến đã lưu cùng với các cài đặt đã lưu của chúng — xem [SDK Tài liệu tham khảo](reference/sdk-reference.md).
