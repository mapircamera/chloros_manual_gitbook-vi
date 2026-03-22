# Thêm file vào dự án

Sau khi bạn đã tạo hoặc mở dự án trong Chloros, bước tiếp theo là bổ sung thêm hình ảnh đa phổ biến của bạn để bắt đầu xử lý. Tab Trình duyệt tệp<img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> giúp bạn dễ dàng nhập hình ảnh và quản lý dữ liệu của mình.

## Truy cập File browser

1. Mở hoặc tạo dự án trong Chloros
2. Bấm vào biểu tượng **Trình duyệt tệp** <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> ở thanh bên trái
3. Table File Browser sẽ hiển thị danh sách dự án tệp của bạn

{% hint style="info" %}
**Các loại tệp được hỗ trợ**: Chloros hỗ trợ các tệp hình ảnh RAW+JPG và JPG từ máy ảnh MAPIR Survey3W và Survey3N. Chỉ nên sử dụng RAW+JPG.
{% endhint %}

***

##Thêm hình ảnh vào dự án của bạn

Có hai cách chính để thêm hình ảnh vào dự án của bạn:

### Cách 1: Add File

Sử dụng tùy chọn này để nhập một số tệp hình ảnh riêng lẻ hoặc một số tệp nhỏ được chọn.

1. Cúc vào nút **"Thêm tệp"** <img src="../.gitbook/assets/image.png" alt="" data-size="line"> ở đầu bảng trình duyệt tệp
2. Điều hướng đến thư mục chứa hình ảnh của bạn
3. Choose one or more file image (giữ **Ctrl** để chọn nhiều file)
4. Bấm vào **"Mở"** để nhập các tệp đã chọn

### Cách 2: Thêm thư mục

Sử dụng tùy chọn này để nhập tất cả hình ảnh từ một thư mục cùng một lúc.

1. Bấm vào nút **"Thêm thư mục"** <img src="../.gitbook/assets/image (1).png" alt="" data-size="line"> ở đầu bảng trình duyệt tệp
2. Điều hướng đến và chọn thư mục chứa ảnh chụp phiên bản của bạn
3. Click vào **"Chọn thư mục"**để nhập tất cả hình ảnh được hỗ trợ từ thư mục đó***

## Tìm hiểu tệp trình duyệt bảng

Khi hình ảnh được nhập, chúng sẽ xuất hiện trong bảng có các cột sau:

### Tệp tên

* Tên gốc của file ảnh từ máy ảnh
* Duy trì quy trình đặt tên máy ảnh (ví dụ: IMG\_0001.RAW)

###Dấu thời gian

* Ngày và giờ hình ảnh được chụp
* Trích xuất từ ​​hình ảnh EXIF ​​siêu dữ liệu
* Được sử dụng để đồng bộ hóa PPK và phát hiện tiêu chuẩn tín hiệu

### Sample camera

* Tự động phát hiện cấu hình camera và bộ lọc
* Ví dụ: Survey3W\_RGN, Survey3N\_OCN, Survey3W\_RGB
* Được sử dụng để áp dụng chính xác hồ sơ xử lý

### Cột mục tiêu (Hộp kiểm tra)

* Choose this hộp để xem hình ảnh chứa tiêu chuẩn tín hiệu
* Tăng tốc độ đáng kể khi phát hiện mục tiêu trong quá trình xử lý
* Xem [Chọn hình ảnh mục tiêu](choosing-target-images.md) để biết chi tiết

### Xem hình ảnh siêu dữ liệu

Click vào nút chuyển đổi ở góc trên cùng bên phải phía trên bảng sẽ hiển thị siêu dữ liệu của hình ảnh đã chọn trong hình ảnh mạng khu vực.

<figure><img src="../.gitbook/assets/chloros_grid_meta.gif" alt=""><figcaption></figcaption></figure>

***

## Quản lý tệp trong dự án của bạn

### Xóa tập tin

Để xóa hình ảnh không mong muốn khỏi dự án của bạn:

1. Choose one or more pictures in the File Browser của bảng
2. Bấm vào nút **"Xóa đã chọn"** <img src="../.gitbook/assets/image (2).png" alt="" data-size="line"> nút
3. Xác nhận xóa (tệp không bị xóa khỏi đĩa, chỉ xóa khỏi dự án)

### Sắp xếp và lọc

* **Sắp xếp theo cột**: Bấm vào bất kỳ cột tiêu đề nào để sắp xếp hình ảnh
* **Sắp xếp dấu thời gian**: Hữu ích cho việc sắp xếp các chuỗi ảnh chụp theo trình tự thời gian
* **Bộ lọc máy ảnh mẫu**: Nhóm hình ảnh theo loại máy ảnh nếu sử dụng nhiều máy ảnh***

## Xem hình ảnh trước

### Xem đầy đủ hình ảnh

Bấm vào bất kỳ hình ảnh thu nhỏ nào trong Tệp trình duyệt để hiển thị hình ảnh trong khu vực xem trước chính:

1. Hình ảnh hiển thị trong bảng xem trước ở giữa
2. Sử dụng điều khiển thu phóng để kiểm tra hình ảnh chi tiết
3. Điều hướng giữa các hình ảnh bằng phím mũi tên

###Điều hướng nhanh

* **Hình trước**: Bấm vào mũi tên trái hoặc nhấn phím ←
* **Hình ảnh tiếp theo**: Bấm vào mũi tên phải hoặc nhấn phím →
* **Phóng to/Thu nhỏ**: Sử dụng bánh xe chuột hoặc nút thu phóng
* **Pan**: Cẩn và kéo vào hình ảnh khi phóng to***

## Xử lý vòng lặp tệp tin

Chloros tự động phát hiện và bỏ qua vòng lặp lặp tệp:

* Các tệp có tên gốc gốc sẽ bị bỏ qua
* Ngăn chặn quá trình xử lý ngẫu nhiên
* Hiển thị cảnh báo thông báo khi phát hiện vòng lặp trùng lặp

{% hint style="warning" %}
**Quan trọng**: Không đổi tên hoặc sửa đổi gốc hình ảnh tệp của bạn trước khi nhập. Chloros dựa vào tên tệp gốc và siêu dữ liệu để xử lý hợp lý.
{% endhint %}

***

## Bộ dữ liệu máy ảnh vô dụng

Nếu dự án của bạn chứa hình ảnh từ nhiều camera MAPIR:

1. Chloros tự động phát hiện từng loại máy ảnh
2. Mỗi loại camera được xử lý theo cấu hình phù hợp với tiêu chuẩn
3. File Browser hiển thị model camera ở cột Camera Model
4. Xử lý ứng dụng cài đặt chính xác cho từng loại máy ảnh

**Ví dụ tình huống**: Thiết lập máy ảnh kép Survey3W RGN + Survey3N OCN***

## Các phương pháp hay nhất

### Sắp xếp trước khi nhập

* Giữ màn hình chỉnh sửa tiêu điểm ảnh trong cùng thư mục với khảo sát hình ảnh
* Duy trì cấu trúc thư mục gốc từ máy ảnh/thẻ SD của bạn
* Không thể trộn các tệp từ các phiên bản khác nhau trong một dự án

### Đặt tên tệp

* Giữ nguyên tên tệp gốc máy ảnh (IMG\_0001.RAW, v.v.)
* Không đổi tên tệp trước khi nhập
* Tên gốc chứa siêu dữ liệu quan trọng

### Chỉnh sửa tiêu điểm hình ảnh

* Luôn bao gồm 1-2 hình ảnh tiêu điểm điều chỉnh mỗi phiên
* Tạo mục tiêu trước và sau phiên chụp
* Đặt tiêu điểm trong cùng điều kiện ánh sáng cho khu vực chụp
* Đánh dấu mục tiêu bằng hộp kiểm tra mục tiêu để tăng tốc độ xử lý

***

## Các vấn đề thường gặp và giải pháp

### Hình ảnh không hiển thị sau khi nhập

**Nguyên nhân có thể:**

* Tệp định dạng không được hỗ trợ (chỉ RAW+JPG và JPG từ máy ảnh MAPIR)
* Hình ảnh được lấy từ máy ảnh không phải MAPIR (xem [Máy ảnh được hỗ trợ](../supported-cameras.md))
* Tệp bị hỏng hoặc chuyển không đầy đủ từ thẻ SD

**Giải pháp**: Xác định định dạng tệp và khả năng tương thích của máy ảnh mẫu

### Máy ảnh mẫu không được phát hiện

**Nguyên nhân có thể:**

* Siêu dữ liệu EXIF ​​đã được sửa đổi
* Hình ảnh được chỉnh sửa bằng phần mềm bên ngoài
* Truyền tệp không đầy đủ

**Giải pháp**: Nhập lại các tệp gốc, chưa được sửa đổi từ máy ảnh/thẻ SD

### Thiếu dấu thời gian

**Nguyên nhân có thể:**

* Máy ảnh đồng hồ không được xác định chính xác
* EXIF ​​dữ liệu bị bỏ qua phần mềm bên ngoài loại

**Giải pháp**: Xác minh thời gian cài đặt của máy ảnh là chính xác khi chụp***

## Các bước tiếp theo

Khi bạn nhập tệp:

1. **Xem lại danh sách tệp** - Đảm bảo tất cả hình ảnh được tải chính xác
2. **Kiểm tra các máy ảnh mẫu** - Xác minh chính xác camera phát hiện
3. **Đánh dấu hình ảnh tiêu điểm** - Xem [Chọn hình ảnh tiêu điểm](choosing-target-images.md)
4. **Điều chỉnh cài đặt** - Định cấu hình bộ xử lý tùy chọn trong [Cài đặt dự án](adjusting-project-settings.md)
5. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)

Để biết thông tin chi tiết về cấu hình dự án, hãy xem [Điều chỉnh cài đặt dự án](adjusting-project-settings.md).