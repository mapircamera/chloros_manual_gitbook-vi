# Điều chỉnh cài đặt dự án

Trước khi xử lý hình ảnh, điều quan trọng là phải định cấu hình cài đặt dự án để phù hợp với yêu cầu quy trình làm việc của bạn. Bảng Cài đặt dự án <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> cung cấp khả năng kiểm soát toàn diện đối với việc hiệu chỉnh, tùy chọn xử lý, chỉ mục đa phổ và định dạng xuất.

## Truy cập cài đặt dự án

1. Mở dự án của bạn bằng Chloros
2. Nhấp vào biểu tượng **Cài đặt dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng Cài đặt dự án hiển thị tất cả các tùy chọn cấu hình

{% gợi ý style="info" %}
**Cài đặt được lưu tự động** với dự án của bạn. Khi bạn mở lại một dự án, tất cả các cài đặt sẽ được khôi phục.
{% endhint %}

***

## Thiết lập nhanh cho quy trình công việc chung

### Cài đặt mặc định (Được khuyến nghị cho hầu hết người dùng)

Đối với quy trình làm việc của camera MAPIR Survey3 điển hình, cài đặt mặc định hoạt động tốt:

* ✅ **Chỉnh họa tiết**: Đã bật
* ✅ **Hiệu chỉnh phản xạ**: Đã bật (yêu cầu hình ảnh của mục tiêu MAPIR)
* ✅ **Phương pháp Debayer**: Chất lượng cao (Nhanh hơn)
* ✅ **Định dạng xuất**: TIFF (16-bit)

Chỉ cần nhập hình ảnh của bạn và bắt đầu xử lý với các giá trị mặc định này.

***

## Tổng quan về cài đặt dự án

Bảng Cài đặt dự án được tổ chức thành nhiều danh mục. Dưới đây là tóm tắt của từng phần. Để có tài liệu đầy đủ, hãy xem [Cài đặt dự án](../project-settings/project-settings.md).

### Phát hiện mục tiêu

Kiểm soát cách Chloros xác định mục tiêu hiệu chuẩn trong hình ảnh của bạn.

**Cài đặt chính:**

* **Vùng mẫu hiệu chuẩn tối thiểu**: Ngưỡng kích thước để phát hiện mục tiêu (mặc định: 25 pixel)
* **Phân cụm mục tiêu tối thiểu**: Ngưỡng tương tự để nhóm các vùng mục tiêu (mặc định: 60)

**Thời điểm điều chỉnh:**

* Tăng diện tích mẫu nếu phát hiện sai
* Giảm nếu mục tiêu không được phát hiện
* Điều chỉnh phân cụm nếu mục tiêu đang được chia thành nhiều lần phát hiện

### Xử lý

Các tùy chọn hiệu chỉnh và xử lý hình ảnh chính.

**Cài đặt chính:**

* **Chỉnh họa tiết**: Bù độ tối của ống kính ở các cạnh ✅ Khuyến nghị
* **Hiệu chỉnh phản xạ**: Chuẩn hóa các giá trị bằng cách sử dụng mục tiêu hiệu chuẩn ✅ Được khuyến nghị
* **Phương pháp Debayer**: Thuật toán chuyển đổi RAW sang đa phổ 3 kênh
* **Khoảng thời gian hiệu chuẩn lại tối thiểu**: Thời gian giữa các lần sử dụng mục tiêu hiệu chuẩn (0 = sử dụng tất cả)

**Cài đặt nâng cao:**

* **Độ lệch múi giờ của cảm biến ánh sáng**: Để đồng bộ hóa thời gian PPK (mặc định: 0)
* **Áp dụng hiệu chỉnh PPK**: Sử dụng dữ liệu GPS/pin phơi sáng từ tệp .daq
* **Chân phơi sáng 1/2**: Gán camera cho các chân phơi sáng để thiết lập máy ảnh kép

### Chỉ mục (Chỉ số đa phổ)

Định cấu hình các chỉ số thực vật nào để tính toán và xuất.

**Cách thêm chỉ số:**

1. Nhấp vào nút **"Thêm chỉ mục"**
2. Chọn chỉ mục từ menu thả xuống (NDVI, NDRE, GNDVI, v.v.)
3. Định cấu hình cài đặt hiển thị (màu LUT, phạm vi giá trị)
4. Thêm nhiều chỉ số nếu cần

**Chỉ số phổ biến:**

* **NDVI**: Tình trạng chung của thực vật (phổ biến nhất)
* **NDRE**: Phát hiện căng thẳng sớm với RedEdge
* **GNDVI**: Nhạy cảm với nồng độ chất diệp lục
* **OSAVI**: Hoạt động tốt với đất nhìn thấy được
* **EVI**: Vùng có chỉ số diện tích lá cao (LAI)

**Công thức tùy chỉnh (chỉ Cloros+):**

* Tạo công thức chỉ số đa phổ tùy chỉnh
* Sử dụng toán học băng tần với tất cả các kênh hình ảnh
* Lưu công thức tùy chỉnh để sử dụng lại

Để biết tất cả các chỉ số và công thức có sẵn, hãy xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md).

### Xuất khẩu

Kiểm soát định dạng và chất lượng tập tin đầu ra.

**Các định dạng có sẵn:**

* **TIFF (16-bit)**: Được khuyến nghị cho GIS và phân tích khoa học (phạm vi 0-65.535)
* **TIFF (32-bit, Phần trăm)**: Giá trị phản xạ dấu phẩy động (phạm vi 0,0-1,0)
* **PNG (8-bit)**: Nén không mất dữ liệu để hiển thị (phạm vi 0-255)
* **JPG (8-bit)**: Tệp nhỏ nhất, nén bị mất dữ liệu (phạm vi 0-255)

***

## Cài đặt lưu và tải

### Lưu mẫu dự án

Tạo các mẫu có thể sử dụng lại cho quy trình công việc nhất quán:

1. Định cấu hình tất cả các cài đặt mong muốn trong bảng Cài đặt dự án
2. Cuộn đến phần **"Lưu mẫu dự án"** ở cuối
3. Nhập tên mẫu mô tả (ví dụ: "Survey3N\_RGN\_Agriculture")
4. Nhấp vào biểu tượng lưu

**Những lợi ích:**

* Áp dụng các cài đặt giống hệt nhau trên nhiều dự án
* Chia sẻ cấu hình với các thành viên trong nhóm
* Duy trì tính nhất quán cho các cuộc khảo sát lặp đi lặp lại

### Tải mẫu vào dự án mới

Khi tạo một dự án mới:

1. Chọn **"Dự án mới"** từ menu chính
2. Chọn tùy chọn **"Tải từ mẫu"**
3. Chọn mẫu đã lưu của bạn
4. Tất cả cài đặt sẽ được tự động áp dụng

### Thư mục làm việc

Cài đặt **"Lưu thư mục dự án"** chỉ định nơi các dự án mới được tạo theo mặc định:

* **Vị trí mặc định**: `C:\Users\[Tên người dùng]\Chloros Project`
* **Thay đổi vị trí**: Nhấp vào biểu tượng chỉnh sửa và chọn thư mục mới
* **Khi nào cần thay đổi**:
  * Ổ đĩa mạng để cộng tác nhóm
  * Ổ đĩa khác có nhiều dung lượng lưu trữ hơn
  * Cấu trúc thư mục được sắp xếp theo năm/khách hàng

***

## Thiết lập PPK (Kinematic sau xử lý)

Nếu sử dụng máy ghi MAPIR DAQ có GPS để định vị chính xác:

### Điều kiện tiên quyết

* MAPIR DAQ với mô-đun GPS (GNSS)
* Tệp nhật ký .daq với các mục ghim phơi sáng
* Camera được kết nối với các chân phơi sáng DAQ trong phiên chụp

### Các bước cấu hình

1. Đặt tệp nhật ký .daq vào thư mục dự án của bạn
2. Trong Cài đặt dự án, bật hộp kiểm **"Áp dụng chỉnh sửa PPK"**
3. Đặt **"Độ lệch múi giờ của cảm biến ánh sáng"** nếu cần (mặc định: 0 cho UTC)
4. Gán camera cho các chân phơi sáng:
   * **Camera đơn**: Tự động được gán cho Chân 1
   * **Camera kép**: Chỉ định thủ công từng camera cho mã pin chính xác

**Gán chốt phơi sáng:**

* **Pin phơi sáng 1**: Chọn kiểu máy ảnh từ danh sách thả xuống
* **Pin phơi sáng 2**: Chọn máy ảnh thứ hai hoặc "Không sử dụng"
* Không thể gán cùng một camera cho cả hai chân

{% gợi ý style="warning" %}
**Quan trọng**: Các chốt phơi sáng phải được gán chính xác cho các máy ảnh tương ứng. Việc gán không chính xác sẽ dẫn đến dữ liệu định vị địa lý sai.
{% endhint %}

***

## Kịch bản nâng cao

### Dự án nhiều camera

Khi xử lý hình ảnh từ nhiều camera MAPIR trong một dự án:

1. Chloros tự động phát hiện từng mẫu camera
2. Mỗi camera có hồ sơ xử lý phù hợp
3. PPK: Chỉ định thủ công từng camera cho chốt phơi sáng chính xác
4. Tất cả các máy ảnh đều sử dụng cùng định dạng và chỉ mục xuất

**Ví dụ**: Giàn máy ảnh kép Survey3W RGN + Survey3N OCN

### Khảo sát theo thời gian hoặc nhiều ngày

Đối với các khảo sát lặp đi lặp lại trên cùng một khu vực theo thời gian:

1. Tạo mẫu với cài đặt tiêu chuẩn của bạn
2. Sử dụng thiết lập mục tiêu hiệu chuẩn nhất quán mỗi phiên
3. Xử lý mỗi ngày như một dự án riêng biệt
4. Sử dụng các cài đặt giống nhau để có kết quả tương đương
5. Xuất ở cùng định dạng để phân tích theo thời gian

### Bộ dữ liệu lớn

Đối với các dự án có nhiều hình ảnh (500+):

* Xem xét chia thành các dự án nhỏ hơn theo ngày hoặc khu vực
* Sử dụng xử lý song song Chloros+ để có kết quả nhanh hơn
* Xem xét CLI hoặc API để tự động hóa hàng loạt
* Điều chỉnh khoảng thời gian hiệu chỉnh lại tối thiểu để giảm thời gian phát hiện mục tiêu

***

## Xác minh cài đặt của bạn

Trước khi bắt đầu xử lý, hãy xem lại các cài đặt chính sau:

* [ ] Đã phát hiện chính xác mẫu máy ảnh trong Trình duyệt tệp
* [ ] Đã bật tính năng chỉnh sửa họa tiết
* [ ] Đã bật hiệu chỉnh độ phản xạ
* [ ] Đã nhập ít nhất một hình ảnh mục tiêu hiệu chỉnh
* [] Đã thêm các chỉ số đa phổ mong muốn
* [ ] Định dạng xuất phù hợp với quy trình làm việc của bạn
* [ ] Đã định cấu hình cài đặt PPK (nếu sử dụng .daq với các sự kiện phơi sáng)

***

## Các bước tiếp theo

Khi cài đặt của bạn được định cấu hình:

1. **Đánh dấu hình ảnh mục tiêu hiệu chỉnh** - Xem [Chọn hình ảnh mục tiêu](chọn-target-images.md)
2. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
3. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết chi tiết đầy đủ về tất cả các cài đặt có sẵn, hãy xem tài liệu tham khảo [Cài đặt dự án](../project-settings/project-settings.md).
