# Điều chỉnh cài đặt dự án

Trước khi xử lý hình ảnh, điều quan trọng là phải định cấu hình cài đặt dự án để phù hợp với quy trình làm việc yêu cầu của bạn. Bảng cài đặt dự án <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> cung cấp khả năng kiểm soát toàn diện đối với chuẩn hiệu, bộ xử lý tùy chọn, đầu ra đa dạng và đa dạng chỉ mục.

## Truy cập dự án cài đặt

1. Mở dự án của bạn trong Chloros
2. Cắt vào biểu tượng **Cài đặt dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng cài đặt hiển thị tất cả các cấu hình tùy chọn

{% hint style="info" %}
**Cài đặt tự động lưu** vào dự án của bạn. Khi bạn mở lại một dự án, tất cả các cài đặt sẽ được khôi phục.
{% endhint %}

***

## Thiết lập nhanh chóng cho công việc chung

### Cài đặt mặc định ( Khuyến khích cho hầu hết người dùng)

Đối với hoạt động quy trình của máy ảnh MAPIR Survey3 điển hình, cài đặt mặc định hoạt động tốt:

* ✅ **Chỉ chi tiết**: Đã bật
* ✅ **Phản xạ chỉnh sửa**: Đã bật (yêu cầu hình ảnh của mục tiêu MAPIR)
* ✅ **Phương pháp Debayer**: Tiêu chuẩn (Nhanh, Chất lượng trung bình)
* ✅ **Xuất dạng định dạng**: TIFF (16-bit)

Bạn chỉ cần nhập hình ảnh của mình và bắt đầu xử lý các giá trị mặc định này.

***

## Tổng quan về cài đặt dự án

Dự án cài đặt bảng được tổ chức thành nhiều danh mục. Dưới đây là phần tóm tắt của từng phần. Để có đầy đủ tài liệu, hãy xem [Cài đặt dự án](../project-settings/project-settings.md).

### Phát hiện tiêu điểm

Kiểm soát cách Chloros chỉnh sửa tiêu điểm xác định trong hình ảnh của bạn.

**Cài đặt chính:*** **Sử dụng mức tối thiểu chuẩn mực mẫu**: Kích thước ngưỡng để hiển thị tiêu chuẩn (mặc định: 25 pixel)
* **Tối thiểu phân cụm tiêu chuẩn**: Ngưỡng tương thích để nhóm các tiêu phân vùng (mặc định: 60)**Điều chỉnh thời điểm:**

* Mẫu tích lũy nếu phát hiện sai
* Giảm nếu mục tiêu không được phát hiện
* Điều chỉnh phân cụm nếu mục tiêu được chia thành nhiều lần

### Xử lý

Các tùy chọn chỉnh sửa và xử lý hình ảnh chính.

**Cài đặt chính:*** **Chỉ họa tiết**: Độ tối của ống kính ở các cạnh ✅ Khuyến nghị
* **Chỉnh sửa phản xạ**: Chuẩn hóa các giá trị bằng cách sử dụng tiêu chuẩn ✅ Khuyến nghị
* **Phương pháp Debayer**: Thuật toán chuyển đổi RAW sang đa phổ 3 kênh
* **Khoảng thời gian điều chỉnh lại thời gian tối thiểu**: Khoảng thời gian giữa các lần sử dụng tiêu chuẩn hiệu chuẩn (0 = sử dụng tất cả)**Cài đặt nâng cao:*** **Độ lệch giờ của cảm biến ánh sáng**: Để đồng bộ hóa PPK thời gian (mặc định: 0)
* **Áp dụng PPK hiệu chỉnh**: Sử dụng GPS/pin phơi sáng dữ liệu từ tệp .daq
* **Chân phơi sáng 1/2**: Gán camera cho các chi tiết phơi sáng để thiết lập máy ảnh kép

### Debayer phương pháp

Chúng tôi cung cấp 2 phương pháp gỡ lỗi trong Chloros:

#### Tiêu chuẩn (Nhanh, Chất lượng trung bình)

Trình xử lý tiêu chuẩn lỗi nhanh hơn nhưng hiển thị nhiễu màu gần hơn, dẫn đến độ chính xác và nhiễu hơn của hình ảnh.

#### Nhận cấu hình (Chậm, Chất lượng cao nhất) \[Chỉ Chloros+]

Text Aware sử dụng công cụ gỡ lỗi nhận biết chất lượng cao kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ tiếng ồn gây nhiễu. Mô hình Nhận biết GPU bộ nhớ (VRAM) yêu cầu cấu hình để chạy. Chúng tôi khuyên bạn nên sử dụng nó khi bạn có sẵn >4GB VRAM để xử lý nhanh hơn.

### Chỉ mục (Chỉ số đa phổ)

Định cấu hình các thực thể chỉ số để tính toán và xuất.

**Thêm chỉ số:**

1. Click vào nút**"Thêm chỉ mục"**

2. Chọn một mục từ menu thả xuống (NDVI, NDRE, GNDVI, v.v.)
3. Màn hình cài đặt định cấu hình (màu LUT, phạm vi giá trị)
4. Thêm nhiều số nếu cần

**Chỉ phổ biến:*** **NDVI**: Tình trạng chung của thực vật (phổ biến nhất)
* **NDRE**: Phát hiện căng thẳng sớm với RedEdge
* **GNDVI**: Nhạy cảm với nồng độ chất diệp lục
* **OSAVI**: Hoạt động tốt với trái đất được nhìn thấy
* **EVI**: Các vùng có chỉ số tích phân lá cao (LAI)**Tùy chỉnh công thức (chỉ Chloros+):**

* Tạo công thức điều chỉnh đa phổ biến chỉ số
* Sử dụng băng tần học toán cho tất cả các kênh hình ảnh
* Lưu tùy chỉnh công thức để sử dụng lại

Để biết tất cả các chỉ số và công thức có sẵn, hãy xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md).

### Xuất khẩu

Kiểm tra đầu ra tệp định dạng và chất lượng.

**Các định dạng có sẵn:*** **TIFF (16-bit)**: Được khuyến nghị cho GIS và phân tích khoa học (phạm vi 0-65.535)
* **TIFF (32-bit, Phần trăm)**: Giá trị phản xạ xạ xạ (phạm vi 0,0-1,0)
* **PNG (8-bit)**: Nén không mất dữ liệu để hiển thị (phạm vi 0-255)
* **JPG (8-bit)**: Tệp nhỏ nhất, nén bị mất dữ liệu (phạm vi 0-255)***

## Cài đặt và tải xuống

### Lưu dự án mẫu

Tạo các mẫu có thể sử dụng lại cho công việc quản lý tốt nhất:

1. Cấu hình tất cả các cài đặt mong muốn trong cài đặt dự án
2. Cuộn đến phần **"Lưu dự án mẫu"** ở cuối
3. Nhập mô tả mẫu tên (ví dụ: "Survey3N\_RGN\_Agriculture")
4. Click vào biểu tượng lưu

**Những lợi ích:**

* Áp dụng các cài đặt gốc trên nhiều dự án
* Chia sẻ cấu hình với các thành viên trong nhóm
* Duy trì tính toán tối thiểu cho các khảo sát lặp lại

### Tải mẫu vào dự án mới

Khi tạo một dự án mới:

1. Choose **"Dự án mới"** từ menu chính
2. Choose option **"Tải từ mẫu"**

3. Chọn mẫu đã lưu của bạn
4. Tất cả cài đặt sẽ được tự động áp dụng

### Working Directory

Cài đặt **"Lưu dự án thư mục"** chỉ định nơi các dự án mới được tạo theo mặc định:

* **Mặc định vị trí**: `C:\Users\[Username]\Chloros Projects`
* **Thay đổi vị trí**: Click vào chỉnh sửa biểu tượng và chọn thư mục mới
* **Khi nào cần thay đổi**:
  * Mạng ổ đĩa để cộng tác với nhóm
  * Ổ đĩa khác có nhiều dung lượng lưu trữ hơn
  * Cấu hình thư mục được sắp xếp theo năm/khách hàng

***

## PPK Setting (Kinematic after handler)

Nếu sử dụng máy ghi MAPIR DAQ có GPS để định vị chính xác:

### Điều kiện tiên quyết

* XPROTX000026MôXPROTX DAQ với module GPS (GNSS)
* Tệp nhật ký .daq với các mục phơi sáng
* Máy ảnh được kết nối với DAQ phơi sáng trong phiên bản chụp

### Cấu hình bước

1. Đặt nhật ký tệp .daq vào dự án thư mục của bạn
2. Trong Cài đặt dự án, bật kiểm tra **"Áp dụng chỉnh sửa PPK"**

3. Đặt**"Độ lệch múi giờ của ánh sáng biến"** nếu cần (mặc định: 0 cho UTC)
4. Gán camera cho các chi tiết phơi sáng:
   * **Camera đơn**: Tự động được cấp cho Chân 1
   * **Camera kép**: Chỉ định thủ công từng camera cho mã pin chính xác**Gán chốt phơi sáng:*** **Pin phơi sáng 1**: Chọn loại máy ảnh từ danh sách thư giãn xuống
* **Pin phơi sáng 2**: Chọn máy ảnh thứ hai hoặc "Không sử dụng"
* Không thể gán cùng một camera cho cả hai chân

{% hint style="warning" %}
**Quan trọng**: Các mức phơi sáng phải được chỉ định chính xác cho các máy ảnh tương ứng. Việc phân công không chính xác sẽ dẫn đến sai sót địa chỉ định vị dữ liệu.
{% endhint %}

***

## nâng cao bản nâng cấp

### Dự án nhiều camera

Khi xử lý hình ảnh từ nhiều camera MAPIR trong một dự án:

1. Chloros tự động phát hiện từng loại máy ảnh
2. Mỗi máy ảnh đều có hồ sơ xử lý phù hợp
3. PPK: Chỉ định thủ công từng camera cho chốt phơi sáng chính xác
4. Tất cả các máy ảnh đều sử dụng cùng định dạng và đầu ra chỉ mục

**Ví dụ**: Survey3W RGN + Survey3N OCN Giàn ảnh kép

### Khảo sát theo thời gian hoặc nhiều ngày

Đối với các khảo sát lặp đi lặp lại trên cùng một khu vực theo thời gian:

1. Tạo mẫu cho tiêu chuẩn cài đặt của bạn
2. Sử dụng thiết lập chuẩn mực tiêu chuẩn nhất cho mỗi phiên bản
3. Xử lý mỗi ngày như một dự án riêng biệt
4. Sử dụng các cài đặt tương tự để có kết quả tương thích
5. Xuất ở cùng định dạng để phân tích theo thời gian

### Big Data

Đối với các dự án có nhiều hình ảnh (500+):

* Xem xét chia thành các dự án nhỏ hơn theo ngày hoặc khu vực
* Sử dụng bộ xử lý bài hát Chloros+ để có kết quả nhanh hơn
* Xem xét CLI hoặc API để tự động hóa hàng loạt
* Điều chỉnh tối thiểu hóa hiệu chỉnh khoảng thời gian để giảm thời gian phát hiện mục tiêu

***

## Xác minh cài đặt của bạn

Trước khi bắt đầu xử lý, hãy xem lại các cài đặt chính sau:

* [ ] Đã phát hiện máy ảnh mẫu chính xác trong tệp trình duyệt
* [ ] Đã bật tính năng chỉnh sửa chi tiết
* [ ] Đã bật chế độ phản xạ
* [ ] Đã nhập ít nhất một chỉnh sửa tiêu điểm ảnh
* [] Đã thêm các chỉ số phổ mong muốn
* [ ] Xuất định dạng phù hợp với quy trình làm việc của bạn
* [ ] Đã định cấu hình PPK cài đặt (nếu sử dụng .daq với các điều kiện hiển thị)

***

## Các bước tiếp theo

Khi cài đặt, cấu hình của bạn đã được định cấu hình:

1. **Danh dấu hiệu chỉnh tiêu điểm ảnh** - Xem [Chọn hình ảnh tiêu điểm](choosing-target-images.md)
2. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
3. **Theo dõi quá trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết chi tiết đầy đủ về tất cả các cài đặt có sẵn, hãy xem tài liệu tham khảo [Cài đặt dự án](../project-settings/project-settings.md).