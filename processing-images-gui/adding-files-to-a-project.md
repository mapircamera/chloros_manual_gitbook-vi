# Thêm tập tin vào dự án

Sau khi bạn đã tạo hoặc mở dự án trong Chloros, bước tiếp theo là thêm hình ảnh đa quang phổ của bạn để bắt đầu xử lý. Tab Trình duyệt tệp<img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> giúp bạn dễ dàng nhập hình ảnh và quản lý tập dữ liệu của mình.

## Truy cập Trình duyệt Tệp

1. Mở hoặc tạo dự án trong Chloros
2. Nhấp vào biểu tượng **Trình duyệt tệp**<img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng File Browser sẽ hiển thị danh sách tệp dự án của bạn

{% hint style="info" %}**Các loại tệp được hỗ trợ**: Chloros hỗ trợ các tệp hình ảnh RAW+JPG và JPG từ máy ảnh MAPIR Survey3W và Survey3N. Chỉ nên sử dụng RAW+JPG.
{% endhint %}***

## Thêm hình ảnh vào dự án của bạn

Có hai cách chính để thêm hình ảnh vào dự án của bạn:

### Cách 1: Thêm File

Sử dụng tùy chọn này để nhập các tệp hình ảnh riêng lẻ hoặc một số tệp được lựa chọn.

1. Nhấp vào nút**"Thêm tệp"**ở đầu bảng Trình duyệt tệp
2. Điều hướng đến thư mục chứa hình ảnh của bạn
3. Chọn một hoặc nhiều file ảnh (giữ**Ctrl**để chọn nhiều file)
4. Nhấp vào**"Mở"**để nhập các tệp đã chọn

### Cách 2: Thêm thư mục

Sử dụng tùy chọn này để nhập tất cả hình ảnh từ một thư mục cùng một lúc.

1. Nhấp vào nút**"Thêm thư mục"**ở đầu bảng Trình duyệt tệp
2. Điều hướng đến và chọn thư mục chứa hình ảnh phiên chụp của bạn
3. Nhấp vào**"Chọn thư mục"**để nhập tất cả hình ảnh được hỗ trợ từ thư mục đó***

## Tìm hiểu bảng trình duyệt tệp

Khi hình ảnh được nhập, chúng sẽ xuất hiện trong bảng có các cột sau:

### Hình thu nhỏ

* Xem trước nhỏ của mỗi hình ảnh
* Nhấp vào hình thu nhỏ để xem hình ảnh đầy đủ trong khu vực xem trước chính

### Tên tệp

* Tên file gốc từ máy ảnh
* Duy trì quy ước đặt tên máy ảnh (ví dụ: IMG\_0001.RAW)

### Dấu thời gian

* Ngày và giờ hình ảnh được chụp
* Trích xuất từ ​​​​siêu dữ liệu EXIF ​​​​của hình ảnh
* Được sử dụng để đồng bộ hóa PPK và phát hiện mục tiêu hiệu chuẩn

### Mẫu máy ảnh

* Tự động phát hiện cấu hình camera và bộ lọc
* Ví dụ: Survey3W\_RGN, Survey3N\_OCN, Survey3W\_RGB
* Được sử dụng để áp dụng hồ sơ xử lý chính xác

### Cột mục tiêu (Hộp kiểm)

* Chọn hộp này để xem hình ảnh chứa mục tiêu hiệu chuẩn
* Tăng tốc đáng kể việc phát hiện mục tiêu trong quá trình xử lý
* Xem [Chọn ảnh mục tiêu](chọn-target-images.md) để biết chi tiết

***

## Quản lý tệp trong dự án của bạn

### Xóa tập tin

Để xóa hình ảnh không mong muốn khỏi dự án của bạn:

1. Chọn một hoặc nhiều hình ảnh trong bảng File Browser
2. Nhấp vào nút**"Xóa đã chọn"**

3. Xác nhận xóa (tệp không bị xóa khỏi đĩa, chỉ bị xóa khỏi dự án)

### Sắp xếp và lọc

* **Sắp xếp theo cột**: Nhấp vào bất kỳ tiêu đề cột nào để sắp xếp hình ảnh
* **Sắp xếp dấu thời gian**: Hữu ích cho việc sắp xếp các chuỗi chụp theo trình tự thời gian
* **Bộ lọc mẫu máy ảnh**: Nhóm hình ảnh theo loại máy ảnh nếu sử dụng nhiều máy ảnh***

## Xem trước hình ảnh

### Xem hình ảnh đầy đủ

Bấm vào bất kỳ hình thu nhỏ nào của hình ảnh trong Trình duyệt Tệp để hiển thị hình ảnh đó trong khu vực xem trước chính:

1. Hình ảnh xuất hiện trong bảng xem trước ở giữa
2. Sử dụng điều khiển thu phóng để kiểm tra chi tiết hình ảnh
3. Điều hướng giữa các hình ảnh bằng phím mũi tên

### Điều hướng nhanh

* **Hình trước**: Nhấp vào mũi tên trái hoặc nhấn phím ←
* **Hình ảnh tiếp theo**: Nhấp vào mũi tên phải hoặc nhấn phím →
* **Phóng to/Thu nhỏ**: Sử dụng bánh xe chuột hoặc nút thu phóng
* **Pan**: Nhấp và kéo vào hình ảnh khi phóng to***

## Xử lý tệp trùng lặp

Chloros tự động phát hiện và bỏ qua các tập tin trùng lặp:

* Các tệp có tên tệp giống hệt nhau sẽ bị bỏ qua
* Ngăn chặn việc xử lý kép ngẫu nhiên
* Thông báo cảnh báo hiển thị khi phát hiện trùng lặp

{% hint style="warning" %}
**Quan trọng**: Không đổi tên hoặc sửa đổi tệp hình ảnh gốc của bạn trước khi nhập. Cloros dựa vào tên tệp gốc và siêu dữ liệu để xử lý thích hợp.
{% endhint %}***

## Bộ dữ liệu máy ảnh hỗn hợp

Nếu dự án của bạn chứa hình ảnh từ nhiều camera MAPIR:

1. Chloros tự động phát hiện từng mẫu camera
2. Mỗi loại camera được xử lý với cấu hình hiệu chuẩn phù hợp
3. File Browser hiển thị model camera ở cột Camera Model
4. Xử lý áp dụng cài đặt chính xác cho từng loại camera
**Tình huống ví dụ**: Thiết lập máy ảnh kép Survey3W RGN + Survey3N OCN***

## Các phương pháp hay nhất

### Sắp xếp trước khi nhập

* Giữ hình ảnh mục tiêu hiệu chỉnh trong cùng thư mục với hình ảnh khảo sát
* Duy trì cấu trúc thư mục gốc từ máy ảnh/thẻ SD của bạn
* Không trộn lẫn các tập dữ liệu từ các phiên khác nhau trong một dự án

### Đặt tên tệp

* Giữ nguyên tên tệp máy ảnh gốc (IMG\_0001.RAW, v.v.)
* Không đổi tên tập tin trước khi nhập
* Tên gốc chứa siêu dữ liệu quan trọng

### Hình ảnh mục tiêu hiệu chỉnh

* Luôn bao gồm 1-2 hình ảnh mục tiêu hiệu chỉnh mỗi phiên
* Bắt mục tiêu trước và sau phiên chụp
* Đặt mục tiêu trong cùng điều kiện ánh sáng với khu vực chụp
* Đánh dấu ảnh mục tiêu bằng hộp kiểm Mục tiêu để tăng tốc độ xử lý

***

## Các vấn đề thường gặp và giải pháp

### Hình ảnh không xuất hiện sau khi nhập**Nguyên nhân có thể:**

* Định dạng tệp không được hỗ trợ (chỉ RAW+JPG và JPG từ máy ảnh MAPIR)
* Hình ảnh được lấy từ máy ảnh không phải MAPIR (xem [Máy ​​ảnh được hỗ trợ](../supported-Cameras.md))
* Tệp bị hỏng hoặc chuyển không đầy đủ từ thẻ SD

**Giải pháp**: Xác minh định dạng tệp và khả năng tương thích của mẫu máy ảnh

### Mẫu máy ảnh không được phát hiện**Nguyên nhân có thể:**

* Siêu dữ liệu EXIF ​​​​đã sửa đổi
* Hình ảnh được chỉnh sửa bằng phần mềm bên ngoài
* Truyền tập tin không đầy đủ

**Giải pháp**: Nhập lại các tập tin gốc, chưa sửa đổi từ máy ảnh/thẻ SD

### Thiếu dấu thời gian**Nguyên nhân có thể:**

* Đồng hồ máy ảnh không được đặt chính xác
* Dữ liệu EXIF ​​​​bị phần mềm bên ngoài loại bỏ

**Giải pháp**: Xác minh cài đặt thời gian của máy ảnh là chính xác trong khi chụp***

## Các bước tiếp theo

Khi tệp của bạn được nhập:

1.**Xem lại danh sách tệp**- Đảm bảo tất cả hình ảnh được tải chính xác
2.**Kiểm tra các mẫu máy ảnh**- Xác minh việc phát hiện camera chính xác
3.**Đánh dấu hình ảnh mục tiêu**- Xem [Chọn hình ảnh mục tiêu](chọn-target-images.md)
4.**Điều chỉnh cài đặt**- Định cấu hình các tùy chọn xử lý trong [Cài đặt dự án]( adjustment-project-settings.md)
5.**Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)

Để biết thông tin chi tiết về cấu hình dự án, hãy xem [Điều chỉnh cài đặt dự án]( adjustment-project-settings.md).
