#Hoàn tất quá trình xử lý

Sau khi Chloros hoàn tất quá trình xử lý, đã đến lúc xem lại kết quả của bạn, xác minh chất lượng đầu ra và chuẩn bị hình ảnh đã xử lý để sử dụng trong quy trình làm việc của bạn. Trang này hướng dẫn bạn qua các bước cuối cùng và hành động tiếp theo.

## Đang xử lý chỉ báo hoàn thành

Khi quá trình xử lý kết thúc thành công, bạn sẽ thấy một số chỉ báo:

* ✅ **Thanh tiến trình**: Hoàn thành 100%
* ✅ **Nhật ký gỡ lỗi**: Hiển thị thông báo "Đang xử lý hoàn tất"
* ✅ **Nút bắt đầu**: Được bật lại (sẵn sàng cho lần xử lý tiếp theo)
* ✅ **Tệp đầu ra**: Tất cả hình ảnh đã xử lý được lưu vào thư mục con của mẫu máy ảnh***## Định vị hình ảnh đã xử lý của bạn

### Mở thư mục đầu ra

1. Nhấp vào biểu tượng**Main Menu**<img src="../.gitbook/assets/image (1) (1).png" alt="" data-size="line"> (trên cùng bên trái)
2. Chọn**"Mở thư mục dự án"**3. File explorer của bạn sẽ mở ra thư mục dự án
4. Xác định vị trí dự án của bạn theo tên***

## Xem lại hình ảnh đã xử lý

### Xem trước nhanh trong File Explorer**Bản xem trước tích hợp sẵn của Windows:**1. Điều hướng đến thư mục con mẫu máy ảnh
2. Chọn file hình ảnh
3. Bản xem trước xuất hiện trong khung xem trước của Windows Explorer
4. Sử dụng phím mũi tên để duyệt qua hình ảnh

### Xem trước trong Trình xem ảnh bên ngoài**Người xem được đề xuất:***
**QGIS** - Phần mềm GIS miễn phí (tốt nhất cho phân tích đa phổ tham chiếu địa lý)
* **IrfanView** - Trình xem ảnh nhanh, nhẹ (hỗ trợ TIFF)
* **Adobe Photoshop** - Chỉnh sửa chuyên nghiệp (hỗ trợ TIFF)
* **GIMP** - Thay thế miễn phí cho Photoshop
* **Windows Photos**- Xem cơ bản (có thể không hỗ trợ TIFF 16-bit)

### Xem trước trong Trình xem ảnh Chloros

Sử dụng Trình xem hình ảnh tích hợp của Chloros để hiển thị nâng cao:

1. Nhấp vào hình thu nhỏ của hình ảnh trong Trình duyệt Tệp
2. Hình ảnh mở ra trong khu vực xem trước chính
3. Nhấp vào tab**Trình xem hình ảnh**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> ở thanh bên trái
4. Sử dụng [Index/LUT Sandbox](../image-viewer-gui/index-lut-sandbox.md) để phân tích tương tác

Xem [Trình xem hình ảnh](../image-viewer-gui/opening-an-image-full-screen.md) để biết hướng dẫn chi tiết.***## Xem lại Nhật ký gỡ lỗi

### Kiểm tra cảnh báo hoặc lỗi

1. Mở**Nhật ký gỡ lỗi**<img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> tab
2. Cuộn qua tin nhắn
3. Tìm cảnh báo màu vàng hoặc lỗi màu đỏ
4. Xem lại mọi vấn đề được ghi nhận
5. Liên hệ với bộ phận hỗ trợ của MAPIR để được hỗ trợ

### Lưu nhật ký

Để lưu giữ hồ sơ xử lý hoặc gửi tới bộ phận Hỗ trợ MAPIR:

1. Nhấp vào nút**"Sao chép"**hoặc**"Tải xuống"**2. Lưu dưới dạng tệp văn bản trong thư mục dự án
3. Kèm theo tài liệu dự án
4. Gửi tới bộ phận hỗ trợ MAPIR nếu gặp sự cố***

## Các vấn đề và giải pháp đầu ra thường gặp

### Vấn đề: Thiếu tệp đầu ra**Nguyên nhân có thể:**

* Tệp không đáp ứng tiêu chí xử lý
* Hình ảnh chỉ dành cho mục tiêu (không được xuất)
* Dung lượng ổ đĩa hết trong khi xuất
* Tệp bị hỏng trong quá trình xử lý

**Giải pháp:**1. Kiểm tra Nhật ký gỡ lỗi để biết thông báo lỗi/bỏ qua
2. Xác minh dung lượng ổ đĩa đủ
3. Đếm tệp: Phải khớp (số lượng ban đầu - số lượng mục tiêu) × (chỉ số + 1)
4. Nhập lại và xử lý lại mọi tệp bị thiếu

### Vấn đề: Các cạnh tối hoặc sáng (Vẫn hiển thị họa tiết)**Nguyên nhân có thể:**

* Đã tắt tính năng chỉnh sửa họa tiết
* Máy ảnh/ống kính không có trong cơ sở dữ liệu hồ sơ Chloros
* Họa tiết cực độ vượt quá khả năng chỉnh sửa

**Giải pháp:**1. Xác minh tính năng chỉnh sửa họa tiết đã được bật trong Cài đặt dự án
2. Kiểm tra mẫu máy ảnh được phát hiện chính xác
3. Liên hệ với bộ phận hỗ trợ của MAPIR nếu hiện tượng mờ viền vẫn tiếp diễn

### Vấn đề: Màu sắc hoặc giá trị không chính xác**Nguyên nhân có thể:**

* Không phát hiện thấy mục tiêu hiệu chuẩn
* Đã chọn sai mô hình mục tiêu hiệu chuẩn
* Hiệu chỉnh phản xạ bị vô hiệu hóa
* Hình ảnh mục tiêu chất lượng kém

**Giải pháp:**1. Xác minh hiệu chỉnh độ phản xạ đã được bật
2. Kiểm tra thông báo "Đã tìm thấy mục tiêu" trong Nhật ký gỡ lỗi
3. Xem lại chất lượng hình ảnh mục tiêu
4. Tái xử lý với các mục tiêu thích hợp được đánh dấu

### Vấn đề: Giá trị NDVI có vẻ sai**Phạm vi NDVI dự kiến:***
**Nước, đá, đất**: -0,1 đến 0,2
* **Thảm thực vật thưa thớt/không tốt**: 0,2 đến 0,4
* **Thảm thực vật vừa phải**: 0,4 đến 0,6
* **Thảm thực vật rậm rạp, khỏe mạnh**: 0,6 đến 0,9**Nếu các giá trị nằm ngoài phạm vi này:**1. Xác minh hiệu chuẩn phản xạ đã được áp dụng
2. Xác minh nhật ký cảm biến ánh sáng đã được đưa vào
3. Kiểm tra mục tiêu hiệu chuẩn đã được phát hiện
4. Đảm bảo phát hiện đúng mẫu máy ảnh
5. Xem lại thời gian và điều kiện chụp ảnh mục tiêu***

## Sử dụng hình ảnh đã xử lý của bạn

### Dành cho phép đo ảnh / Tạo chỉnh hình**Quy trình làm việc được đề xuất:**1.**Nhập hình ảnh phản xạ đã hiệu chỉnh** vào phần mềm đo ảnh:
   * Pix4Dmapper
   * Metashape Agisoft
   * Triển khai Drone
   * WebODM
2. **Giữ siêu dữ liệu EXIF**: Đảm bảo dữ liệu GPS được bảo toàn để gắn thẻ địa lý
3.**Quy trình làm việc đã được hiệu chỉnh**: Sử dụng hình ảnh phản chiếu để có độ chính xác khoa học
4.**Xử lý khảm chỉ mục**: Tạo trực giao NDVI từ các hình ảnh chỉ mục riêng lẻ
5.**Xuất GeoTIFF tham chiếu địa lý**: Để sử dụng trong các ứng dụng GIS

### Để phân tích GIS**Quy trình làm việc được đề xuất:**1.**Tải vào QGIS, ArcGIS hoặc tương tự**2.**Sử dụng hình ảnh phản xạ TIFF 16-bit**để phân tích đa băng tần
3.**Sử dụng hình ảnh chỉ mục**(NDVI, NDRE) làm lớp thực vật sẵn sàng sử dụng
4.**Máy tính raster**: Kết hợp các dải để phân tích tùy chỉnh
5.**Xuất**: Tạo bản đồ phân loại, phát hiện thay đổi, bản đồ tình trạng thảm thực vật

### Để phân tích / báo cáo trực tiếp**Quy trình làm việc được đề xuất:**1.**Sử dụng hình ảnh chỉ mục có màu LUT**cho báo cáo trực quan
2.**Trích xuất số liệu thống kê**: NDVI trung bình trên mỗi trường/ô
3.**Chuỗi thời gian**: So sánh các chỉ số trên nhiều phiên
4.**Tạo báo cáo**: Bao gồm bản đồ, số liệu thống kê và hình ảnh trực quan***## Lưu trữ và sao lưu

### Chiến lược sao lưu được đề xuất**Những gì cần lưu:***✅**Hình ảnh RAW/JPG gốc** - Lưu trữ trên ổ đĩa/đám mây riêng
* ✅ **Đầu ra đã xử lý** - Giữ hình ảnh và chỉ số đã hiệu chỉnh
* ✅ **Tệp dự án** - Chứa tất cả các cài đặt để xử lý lại nếu cần
* ✅ **Nhật ký gỡ lỗi** - Chi tiết xử lý tài liệu
* ✅ **Hình ảnh mục tiêu hiệu chuẩn**- Để xác minh và xử lý lại**Khuyến nghị lưu trữ:***
**Sao lưu ngay lập tức**: Ổ cứng ngoài
* **Lưu trữ dài hạn**: Lưu trữ đám mây (Google Drive, Dropbox, v.v.)
* **Dữ liệu quan trọng**: Giữ 2-3 bản sao ở các vị trí khác nhau***## Lần xử lý tiếp theo

### Sử dụng lại cài đặt dự án

Nếu xử lý các tập dữ liệu tương tự trong tương lai:

1.**Lưu mẫu dự án**(nếu chưa được thực hiện)
2.**Tạo dự án mới**sử dụng mẫu đã lưu
3.**Nhập hình ảnh mới**4.**Xử lý**với các cài đặt giống hệt nhau để đảm bảo tính nhất quán

### Xử lý hàng loạt nhiều phiên

Đối với nhiều phiên/tập dữ liệu:**Tùy chọn 1: GUI - Nhiều dự án**

* Tạo dự án riêng cho mỗi phiên
* Sử dụng cài đặt mẫu nhất quán
* Xử lý từng cái một

**Tùy chọn 2: Chloros CLI (chỉ Cloros+)**

* Tự động xử lý hàng loạt
* Xử lý nhiều thư mục bằng tập lệnh
* Xem [Tài liệu CLI](../CLI.md)

**Tùy chọn 3: SDK Python (chỉ Cloros+)**

* Điều khiển theo chương trình
* Tích hợp với các đường ống phân tích
* Xem [Tài liệu API](../api-python-sdk.md)

***

## Khắc phục sự cố sau xử lý

### Xử lý lại với các cài đặt khác

Nếu kết quả không đạt yêu cầu:

1. Giữ nguyên hình ảnh (không bao giờ xóa)
2. Mở dự án tương tự ở Chloros
3. Điều chỉnh cài đặt trong bảng Cài đặt dự án
4. Xử lý lại - kết quả đầu ra sẽ ghi đè kết quả trước đó

### Đang xử lý tập hợp con hình ảnh

Để chỉ xử lý lại những hình ảnh cụ thể:

1. Tạo dự án mới
2. Chỉ nhập những hình ảnh cần xử lý lại
3. Sử dụng cùng một mẫu cài đặt
4. Xử lý tập dữ liệu nhỏ hơn

### Nhận trợ giúp

Nếu bạn gặp phải vấn đề:

* 📧 **Email**: info@mapir.camera (bao gồm Nhật ký gỡ lỗi)
* 🌐 **Hỗ trợ**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Câu hỏi thường gặp**: [Câu hỏi thường gặp](../faq.md)
* 📖 **Tài liệu**: [Hướng dẫn sử dụng Cloros](../)***## Tóm tắt: Hoàn thành quy trình làm việc

Bây giờ bạn đã hoàn thành toàn bộ quy trình xử lý Chloros:

1. ✅**Đã tạo dự án**- Xem [Dự án](../projects.md)
2. ✅**Đã thêm tệp**- Xem [Thêm tệp](adding-files-to-a-project.md)
3. ✅**Cài đặt đã điều chỉnh**- Xem [Điều chỉnh cài đặt dự án]( adjustment-project-settings.md)
4. ✅**Mục tiêu được đánh dấu**- Xem [Chọn hình ảnh mục tiêu](chọn-target-images.md)
5. ✅**Đã bắt đầu xử lý**- Xem [Bắt đầu xử lý](starting-the-processing.md)
6. ✅**Tiến trình được giám sát**- Xem [Giám sát quá trình xử lý](monitoring-the-processing.md)
7. ✅**Kết quả đã đánh giá**- Trang này**Hình ảnh đa phổ đã được hiệu chỉnh, hiệu chỉnh độ phản xạ của bạn đã sẵn sàng để phân tích!**
***

## Tài nguyên bổ sung

### Tính năng nâng cao

* [ **Image Viewer**](../image-viewer-gui/opening-an-image-full-screen.md) - Trực quan hóa và phân tích tương tác
* [ **Index/LUT Sandbox**](../image-viewer-gui/index-lut-sandbox.md) - Kiểm tra chỉ mục tùy chỉnh
* [ **Công thức chỉ mục đa phổ**](../project-settings/multispectral-index-formulas.md) - Tham chiếu chỉ mục đầy đủ

### Tự động hóa & Tích hợp

* [ **CLI Documentation**](../CLI.md) - Xử lý hàng loạt dòng lệnh
* [ **Python SDK**](../api-python-sdk.md) - Tự động hóa theo chương trình
* [ **Chloros+ Tính năng**](../#chloros) - Khả năng xử lý nâng cao

### Hỗ trợ & Học tập

* [ **FAQ**](../faq.md) - Các câu hỏi thường gặp đã được trả lời
* [ **Calibration Targets**](../calibration-targets.md) - Tìm hiểu về hiệu chuẩn phản xạ
* [ **Máy ảnh được hỗ trợ**](../supported-Cameras.md) - Phần cứng tương thích
