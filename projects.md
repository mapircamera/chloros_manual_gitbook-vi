# Giao diện người dùng (GUI): Dự án

Chloros cho phép bạn tạo các dự án có thể mở lại trong tương lai. Một dự án là một thư mục thông thường (nằm trong Thư mục Dự án của bạn) chứa:

* `project.json` — cài đặt dự án, danh sách tệp và tùy chọn hiển thị
* `cameras.json` — các camera và mảng cảm biến được kết nối trong khi dự án đang mở, kèm theo cài đặt của chúng
* `sensors.json` — các cảm biến ánh sáng DAQ được kết nối trong khi dự án đang mở, cùng với các liên kết camera↔cảm biến
* các tệp ghi hình của bạn, các bản ghi `.daq` và các thư mục đầu ra đã xử lý

Không có định dạng tệp dự án độc quyền — thư mục và các tệp JSON bên trong chính là dự án, điều này cũng giúp việc sao chép, lưu trữ và điều khiển dự án từ [CLI](CLI.md) hoặc [Python SDK](api-python-sdk.md) trở nên dễ dàng.

Trừ khi bạn thay đổi trong [Cài đặt Dự án](project-settings/project-settings.md), Thư mục Dự án mặc định là `Documents\MAPIR\Chloros Projects` trong thư mục Documents (`~/Documents/MAPIR/Chloros Projects` trên Linux).

{% hint style="warning" %}
**Windows: hãy giữ cho thư mục dự án ngắn gọn trừ khi tính năng đường dẫn dài được bật.**Windows được cài đặt sẵn với tính năng đường dẫn dài bị tắt, do đó không có đường dẫn tệp nào được phép dài hơn 260 ký tự. Các sản phẩm đã xử lý được lưu trữ ở vị trí cách thư mục dự án khoảng 130 ký tự về phía dưới; do đó, kể từ phiên bản 1.3.0, Chloros sẽ từ chối thư mục dự án quá sâu — khi bạn tạo hoặc mở nó, hoặc khi bắt đầu xử lý — thay vì chạy mà không ghi lại bất kỳ dữ liệu nào. Thông báo sẽ hiển thị thư mục dự án dài nhất có thể chấp nhận được. Hãy sử dụng một thư mục dự án ngắn hơn, hoặc bật đường dẫn dài của Windows (tùy chọn**Bật đường dẫn dài của Windows** trong trình cài đặt, được chọn mặc định) và khởi động lại Chloros. Xem [Bắt đầu xử lý](processing-images-gui/starting-the-processing.md#the-project-folder-is-too-deep-for-this-computer).
{% endhint %}

## Dự án mới

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>Chọn &quot;Dự án mới&quot; từ menu chính và nhập tên duy nhất cho dự án của bạn.

Nếu bạn đã lưu bất kỳ mẫu dự án nào, một menu thả xuống **Chọn mẫu** sẽ xuất hiện bên dưới trường tên — việc chọn một mẫu sẽ bắt đầu dự án mới dựa trên cài đặt của mẫu đó. Các mẫu dự án được lưu từ [Cài đặt dự án](project-settings/project-settings.md): nhập tên vào trường &quot;Tên mẫu dự án&quot; và nhấp vào biểu tượng lưu.

## Mở dự án

<figure><img src=".gitbook/assets/v120-open-project.jpg" alt=""><figcaption><p>&quot;Mở Dự án&quot; liệt kê tất cả các dự án trong thư mục dự án của bạn, với tùy chọn <strong>Mở Thư mục Dự án</strong> ở phía dưới</p></figcaption></figure>

Chọn &quot;Mở Dự án&quot; để xem danh sách các dự án hiện có trong Thư mục Dự án. Nếu không có dự án nào, menu phụ bên cạnh sẽ không mở ra. Bạn có thể thấy một số dự án được tạo bởi giao diện người dùng (t1, t2, t3) được liệt kê trong hình ảnh trên. Các dự án có định dạng DATE\_TIME được tạo bởi giao diện dòng lệnh (CLI) theo quy ước đặt tên dự án mặc định. Nhấp vào bất kỳ tên dự án nào sẽ mở dự án đó.

Nhấp vào nút &quot;Mở thư mục dự án&quot; sẽ mở Trình quản lý tệp trên máy tính của bạn tại đường dẫn của dự án. Bạn có thể điều chỉnh đường dẫn dự án trong [Cài đặt Dự án](project-settings/project-settings.md).

Nếu bất kỳ tệp hình ảnh nguồn nào của dự án đã bị di chuyển hoặc xóa kể từ lần mở gần nhất, Chloros sẽ hiển thị một hộp thoại liệt kê chính xác các tệp bị thiếu thay vì mở một lưới trống.

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

Mới trong phiên bản 1.2.0: dự án sẽ lưu lại phần cứng mà bạn kết nối trong suốt thời gian dự án đang mở. Các camera và mảng camera (cùng với cài đặt riêng cho từng camera, tên, màu sắc và bố cục lưới) được chụp nhanh vào tệp `cameras.json`, còn các cảm biến ánh sáng (cùng với tên, màu sắc và liên kết với camera) được lưu vào tệp `sensors.json` — một cách tự động trong khi bạn làm việc.

Khi bạn **mở lại** một dự án, Chloros sẽ không kết nối ngay với bất kỳ phần cứng nào. Mỗi phần sẽ kết nối lại khi bạn truy cập tab sở hữu phần đó lần đầu tiên:

* Mở tab **Camera** sẽ kết nối lại các camera và mảng đã lưu và áp dụng lại các cài đặt đã lưu của chúng.
* Mở tab **Cảm biến ánh sáng**sẽ kết nối lại các cảm biến DAQ đã lưu và áp dụng lại các cài đặt đã lưu của chúng, bao gồm cả nắp bảo vệ. Một cảm biến được đặt thành**None (cảm biến trần)** trong menu Cap sẽ vẫn ở trạng thái trần. Một dự án được lưu bởi phiên bản Chloros trước đó đã ghi lại cảm biến trần như một tùy chọn không có, do đó nó sẽ khôi phục lại với hiệu chỉnh cosin ánh nắng; hãy đặt lại thành None một lần nữa và lưu dự án.

Bằng cách này, việc mở dự án chỉ để duyệt hoặc xuất hình ảnh sẽ không bao giờ kích hoạt chế độ truyền dữ liệu trực tiếp của camera. Nếu không tìm thấy thiết bị đã lưu khi mở tab tương ứng, một hộp thoại sẽ thông báo các thiết bị không khả dụng để bạn có thể kết nối lại hoặc loại bỏ chúng.

## Bản ghi DAQ và tệp .daq trong dự án

* Các bản ghi `.daq` được tạo ra khi dự án đang mở (từ tab Cảm biến Ánh sáng hoặc trong quá trình ghi) sẽ được **tự động thêm vào dự án**.
* Các tệp `.daq` được nhập vào và tất cả các bản ghi của dự án đều được liệt kê trong phần **Cảm biến ánh sáng DAQ** của [Cài đặt dự án](project-settings/project-settings.md), mỗi bản ghi đi kèm với hồ sơ hiệu chỉnh độ chói riêng.
* Trong quá trình xử lý, các tệp `.daq` của dự án cung cấp thông tin chiếu sáng từ trên xuống cho các sản phẩm phản xạ — xem [Định dạng hình ảnh đầu ra](output-image-formats.md).

## Chạy dự án đã lưu mà không cần giao diện người dùng

Một dự án đã lưu có thể được chạy mà không cần giao diện người dùng:

* **CLI**: `chloros-cli project open / connect / capture / sensor / align / run` hoạt động trên đường dẫn thư mục dự án — xem [Tham chiếu CLI](reference/cli-reference.md).
* **Giao diện lập trình ứng dụng (SDK)**: `chloros_sdk.open_project(path)` trả về một handle dự án; `connect_all()` kích hoạt tất cả các camera, mảng cảm biến và cảm biến ánh sáng đã lưu với các cài đặt đã lưu (cảm biến ánh sáng thông qua backend Chloros đang chạy) — xem [Tài liệu tham khảo SDK](reference/sdk-reference.md).
