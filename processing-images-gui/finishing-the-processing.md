#Hoàn tất quá trình xử lý

Sau khi Chloros hoàn tất quá trình xử lý, lúc này bạn sẽ xem lại kết quả, xác định chất lượng đầu ra và chuẩn hình ảnh đã được xử lý để sử dụng trong quá trình làm việc của bạn. Trang này hướng dẫn bạn qua các bước cuối cùng và hành động tiếp theo.

## Đang xử lý thành công thông báo

Khi quá trình xử lý kết thúc thành công, bạn sẽ thấy một số chỉ báo:

* ✅ **Thanh tiến trình**: Hoàn thành 100%
* ✅ **Gỡ lỗi nhật ký**: Hiển thị thông báo "Đang xử lý hoàn tất"
* ✅ **Nút bắt đầu**: Được bật lại (sẵn sàng cho quá trình xử lý tiếp theo)
* ✅ **Tệp đầu ra**: Tất cả hình ảnh đã được xử lý được lưu vào thư mục của máy ảnh mẫu***

## Bạn đã xử lý hình ảnh định vị

### Mở đầu thư mục

1. Click vào biểu tượng **Main Menu** <img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt="" data-size="line"> biểu tượng (ở cùng bên trái)
2. Choose **"Mở dự án thư mục"**

3. File explorer của bạn sẽ mở ra dự án thư mục
4. Xác định dự án của bạn theo tên

***

## Xem lại hình ảnh đã được xử lý

### Xem trước nhanh trong File Explorer

**Bản xem trước hợp lý Windows:**

1. Điều hướng đến máy ảnh mẫu thư mục
2. Choose file image
3. Bản xem trước xuất hiện trong khung xem trước Windows Explorer
4. Sử dụng mũi tên để duyệt hình ảnh

### Xem trước trong Trình xem ảnh bên ngoài

**Người xem được đề xuất:*** **QGIS** - Phần mềm GIS miễn phí (tốt nhất cho phân tích đa tham chiếu địa lý)
* **IrfanView** - Trình xem hình ảnh nhanh, nhẹ nhàng (hỗ trợ TIFF)
* **Adobe Photoshop** - Chỉnh sửa chuyên nghiệp (hỗ trợ TIFF)
* **GIMP** - Thay thế miễn phí cho Photoshop
* **Ảnh Windows** - Xem cơ bản (TIFF 16 bit có thể không được hỗ trợ)

### Xem trước trong Trình xem ảnh Chloros

Sử dụng Trình xem hình ảnh hợp lý của Chloros để hiển thị nâng cao:

1. Click vào hình thu nhỏ của hình ảnh trong File trình duyệt
2. Hình ảnh mở ra trong khu vực xem trước chính
3. Cú vào tab **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> ở thanh bên trái
4. Sử dụng [Index/LUT Sandbox](../image-viewer-gui/index-lut-sandbox.md) để phân tích tương tác

Xem [ Hướng dẫn xem hình ảnh](../image-viewer-gui/opening-an-image-full-screen.md) để biết hướng dẫn chi tiết.

***

## Xem lại lỗi gỡ lỗi Nhật ký

### Kiểm tra cảnh báo hoặc lỗi

1. Mở **Nhật ký gỡ lỗi** <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> tab
2. Cuộn qua tin nhắn
3. Tìm cảnh báo màu vàng hoặc lỗi màu đỏ
4. Xem lại mọi vấn đề đã được ghi
5. Liên hệ bộ phận hỗ trợ MAPIR để được hỗ trợ

### Lưu nhật ký

Để lưu hồ sơ xử lý hoặc gửi tới bộ phận hỗ trợ MAPIR:

1. Cẩn vào nút **"Sao chép"**hoặc**"Tải xuống"**

2. Lưu dưới dạng bản văn tệp trong dự án thư mục
3. Đính kèm dự án tài liệu
4. Gửi tới bộ phận hỗ trợ MAPIR nếu gặp sự cố

***

## Các vấn đề và giải pháp thường gặp

### Vấn đề: Thiếu tệp đầu ra

**Nguyên nhân có thể:**

* Tệp xử lý tiêu chuẩn không đáp ứng
* Dành riêng cho mục tiêu hình ảnh (không thể xuất ra)
* Dung lượng ổ đĩa đã hết trong khi xuất
* Tệp bị hỏng trong quá trình xử lý

**Giải pháp:**

1. Kiểm tra lỗi gỡ lỗi nhật ký để biết lỗi thông báo/bỏ qua
2. Xác minh đủ dung lượng ổ đĩa
3. Đếm tệp: Phải khớp (số lượng ban đầu - số lượng mục tiêu) × (chỉ số + 1)
4. Nhập lại và xử lý lại mọi tệp bị thiếu

### Vấn đề: Các cạnh tối hoặc sáng (Xem chi tiết)

**Nguyên nhân có thể:**

* Đã tắt tính năng chỉnh sửa chi tiết
* Camera/ống kính không có trong cấu hình cơ sở dữ liệu Chloros
* Họa tiết cực độ vượt quá khả năng chỉnh sửa

**Giải pháp:**

1. Xác minh tính năng chỉnh sửa chi tiết đã được bật trong Cài đặt dự án
2. Kiểm tra độ chính xác của máy ảnh mẫu
3. Liên hệ với bộ phận hỗ trợ MAPIR nếu đường viền mờ biểu tượng vẫn tiếp tục diễn ra

### Vấn đề: Màu sắc hoặc giá trị không chính xác

**Nguyên nhân có thể:**

* Không thể phát hiện được tiêu chuẩn đầu mục
* Đã chọn sai tiêu chuẩn mô hình
* Kích hoạt phản xạ vô hiệu hóa
* Hình ảnh sản phẩm chất lượng giá trị

**Giải pháp:**

1. Xác minh đã bật phản xạ chỉnh sửa
2. Kiểm tra thông báo "Đã tìm thấy mục tiêu" trong lỗi gỡ lỗi nhật ký
3. Xem lại mục hình ảnh chất lượng
4. Tái xử lý các mục tiêu thích hợp được đánh dấu

### Vấn đề: Giá trị NDVI có vẻ sai

**Phạm vi NDVI dự kiến:*** **Nước, đá, đất**: -0,1 đến 0,2
* **Thảm thực vật thưa thớt/không tốt**: 0,2 đến 0,4
* **Sản phẩm thực sự phải**: 0,4 đến 0,6
* **Thảm thực nước suối, khỏe mạnh**: 0,6 đến 0,9**Nếu các giá trị nằm ngoài phạm vi này:**

1. Xác minh mã phản xạ chuẩn đã được áp dụng
2. Xác minh nhật ký ký hiệu ánh sáng đã được đưa vào
3. Kiểm tra tiêu chuẩn tín hiệu đã được phát hiện
4. Đảm bảo phát hiện đúng máy ảnh mẫu
5. Xem lại thời gian và điều kiện chụp ảnh mục tiêu

***

## Sử dụng hình ảnh đã được xử lý của bạn

### Phép đo hình ảnh được phép / Tạo cấu hình

**Quy trình làm việc được xuất ra:**

1.**Nhập ảnh đã được chỉnh sửa phản xạ** vào ảnh đo phần mềm:
   * Pix4Dmapper
   * Metashape Agisoft
   * Triển khai Drone
   * WebODM
2. **Giữ EXIF ​​siêu dữ liệu**: Đảm bảo GPS dữ liệu được bảo đảm an toàn để gắn thẻ địa chỉ
3. **Quy trình làm việc đã được hiệu chỉnh**: Sử dụng hình ảnh phản chiếu để có độ chính xác khoa học
4. **Xử lý chỉ mục**: Tạo cấu hình NDVI từ một lẻ chỉ mục hình ảnh
5. **Xuất GeoTIFF được tham chiếu địa lý**: Để sử dụng trong các ứng dụng GIS

### Để phân tích GIS

**Quy trình làm việc được xuất ra:**

1.**Tải vào QGIS, ArcGIS hoặc tương tự**

2.**Sử dụng hình ảnh phản xạ 16-bit TIFF** để phân tích tối đa băng tần
3. **Sử dụng chỉ mục hình ảnh** (NDVI, NDRE) để sẵn sàng sử dụng các lớp thực vật
4. **Máy tính raster**: Kết hợp các phạm vi để phân tích tùy chỉnh
5. **Xuất**: Tạo bản đồ phân loại, phát hiện thay đổi, bản đồ trạng thái thảm thực vật

### Để phân tích / báo cáo trực tiếp

**Quy trình làm việc được xuất ra:**

1.**Sử dụng chỉ mục hình ảnh có màu LUT** cho báo cáo trực tiếp
2. **Trích xuất số liệu thống kê**: NDVI trung bình trên mỗi trường/ô
3. **Chuỗi thời gian**: So sánh các số trên nhiều phiên bản
4. **Tạo báo cáo**: Bao gồm bản đồ, số liệu thống kê và hình ảnh trực quan***

## Lưu trữ và sao lưu

### Chiến lược sao lưu được xuất đề

**Những thứ cần lưu:***✅**Hình ảnh RAW/JPG gốc** - Lưu trữ trên ổ đĩa/đám mây riêng
* ✅ **Đầu ra đã được xử lý** - Giữ hình ảnh và chỉnh sửa chỉ số
* ✅ **Tệp dự án** - Chứa tất cả các cài đặt để xử lý lại nếu cần
* ✅ **Nhật ký gỡ lỗi** - Chi tiết xử lý tài liệu
* ✅ **Hình ảnh mục tiêu hiệu chỉnh**- Để xác minh và xử lý lại**Khuyến đề nghị lưu trữ:*** **Sao lưu ngay lập tức**: Ổ cứng ngoài
* **Lưu trữ dài hạn**: Lưu trữ đám mây (Google Drive, Dropbox, v.v.)
* **Dữ liệu quan trọng**: Giữ 2-3 bản sao ở các vị trí khác***

## Xử lý lần tiếp theo

### Sử dụng lại dự án cài đặt

Nếu xử lý các dữ liệu tương tự trong tương lai:

1. **Lưu dự án mẫu** (nếu chưa được thực hiện)
2. **Tạo dự án mới** sử dụng mẫu đã lưu
3. **Nhập hình ảnh mới**

4.**Xử lý**với các cài đặt gốc phun để đảm bảo tính chất nhất quán

### Xử lý hàng loạt nhiều phiên bản

Đối với nhiều phiên bản/dữ liệu:**Tùy chọn 1: GUI - Nhiều dự án**

* Tạo dự án riêng cho mỗi phiên
* Sử dụng cài đặt quán mẫu nhất
* Xử lý từng cái một

**Tùy chọn 2: Chloros CLI (chỉ Chloros+)**

* Tự động xử lý hàng loạt
* Xử lý nhiều thư mục bằng lệnh
* Xem [Tài liệu CLI](../CLI.md)

**Tùy chọn 3: Python SDK (chỉ Chloros+)**

* Điều khiển chương trình
* Tích hợp với phân tích đường ống
* Xem [Tài liệu API](../api-python-sdk.md)

***

##Khắc phục sự cố sau quá trình xử lý

### Xử lý lại các cài đặt khác

Nếu kết quả không đạt được yêu cầu:

1. Giữ nguyên hình ảnh (không bao giờ xóa)
2. Mở cùng một dự án trong Chloros
3. Điều chỉnh cài đặt trong cài đặt dự án
4. Xử lý lại - đầu ra của kết quả sẽ ghi đè kết quả trước đó

### Đang xử lý tập hợp hình ảnh

Để xử lý lại các hình ảnh cụ thể:

1. Tạo dự án mới
2. Chỉ nhập những hình ảnh cần xử lý lại
3. Sử dụng cùng một cài đặt mẫu
4. Xử lý dữ liệu nhỏ hơn

### Nhận trợ giúp

Nếu bạn gặp phải vấn đề:

* 📧 **Email**: info@mapir.camera (bao gồm cả nhật ký gỡ lỗi)
* 🌐 **Hỗ trợ**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Câu hỏi thường gặp**: [Câu hỏi thường gặp](../faq.md)
* 📖 **Tài liệu**: [ Hướng dẫn sử dụng Chloros](../)***

## Tóm tắt: Công việc hoàn thành quy trình

Bây giờ bạn đã hoàn thành xong bộ xử lý Chloros:

1. ✅ **Dự án đã tạo** - Xem [Dự án](../projects.md)
2. ✅ **Đã thêm tệp** - Xem [Thêm tệp](adding-files-to-a-project.md)
3. ✅ **Cài đặt điều chỉnh** - Xem [Điều chỉnh cài đặt dự án](adjusting-project-settings.md)
4. ✅ **Mục tiêu được đánh dấu** - Xem [Chọn hình ảnh tiêu điểm](choosing-target-images.md)
5. ✅ **Đã xử lý bắt đầu** - Xem [Bắt xử lý bắt đầu](starting-the-processing.md)
6. ✅ **Tiến trình giám sát** - Xem [Giám sát quá trình xử lý](monitoring-the-processing.md)
7. ✅ **Kết quả đã được đánh giá**- Trang này**Hình ảnh phổ biến đã được chỉnh sửa, hiệu chỉnh mức độ phản xạ của bạn đã sẵn sàng để phân tích!**

***

##Tài nguyên bổ sung

### Tính năng nâng cao

* [**Image Viewer**](../image-viewer-gui/opening-an-image-full-screen.md) - Tương tác trực quan hóa và phân tích
* [**Index/LUT Sandbox**](../image-viewer-gui/index-lut-sandbox.md) - Kiểm tra tùy chỉnh chỉ mục
* [**Đa phổ chỉ mục công thức**](../project-settings/multispectral-index-formulas.md) - Tham chiếu đầy đủ chỉ mục

###Tự động hóa & Tích hợp

* [**Tài liệu CLI**](../CLI.md) - Xử lý lệnh dòng hàng
* [**Python SDK**](../api-python-sdk.md) - Tự động hóa theo chương trình
* [**Chloros+ Tính năng**](../#chloros) - Khả năng xử lý nâng cao

### Hỗ trợ & Tập tin học tập

* [**FAQ**](../faq.md) - Đã trả lời các câu hỏi thường gặp
* [**Mục tiêu chuẩn**](../calibration-targets.md) - Tìm hiểu về phản xạ chuẩn
* [**Máy ảnh được hỗ trợ**](../supported-cameras.md) - Phần cứng tương thích