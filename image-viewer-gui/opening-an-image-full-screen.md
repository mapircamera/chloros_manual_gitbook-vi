# Mở một hình ảnh toàn màn hình

Trình xem ảnh Chloros cung cấp giao diện toàn màn hình chuyên dụng để xem, phân tích và thao tác với các hình ảnh phổ biến của bạn. Cho dù xem hình ảnh gốc hay kết quả đầu ra đã được xử lý, Trình xem Hình ảnh đều cung cấp các công cụ mạnh mẽ để kiểm tra và phân tích.

## Truy cập xem ảnh

### Từ trình duyệt File

Cách phổ biến nhất để mở hình ảnh trong Trình xem ảnh:

1. Đảm bảo bạn đang ở trong tab **Trình duyệt tệp** <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line">
2. Tự động bất kỳ **hình thu nhỏ** nào trong ảnh mạng
3. Hình ảnh mở ra trong **khu vực xem trước chính** (giữa màn hình)
4. Hình ảnh đã được tải xuống và sẵn sàng để xem toàn màn hình

### Mở tab Trình xem ảnh

Sau khi tải hình ảnh vào khu vực xem trước:

1. Bấm vào biểu tượng **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> ở thanh bên trái
2. Tab Trình xem Hình ảnh mở ra, hiển thị toàn màn hình đã chọn ảnh
3. Các công cụ xem và nâng cấp phân tích có sẵn ở bên trái

***

## Tổng quan về giao diện xem ảnh

### Khu vực hiển thị chính

Phần lớn nhất của màn hình hiển thị hình ảnh của bạn:

* **Đủ độ phân giải**: Hình ảnh được hiển thị ở độ phân giải gốc
* **Có thể thu phóng**: Sử dụng điều khiển hoặc con chuột để thu phóng
* **Có thể xoay**: Cẩn và kéo để di chuyển xung quanh khi được phóng to
* **Tỷ lệ khung hình được duy trì**: Tỷ lệ hình ảnh theo tỷ lệ***

## Xem tùy chọn

### Cơ sở hình ảnh điều hướng

#### Duyệt qua hình ảnh

Điều hướng bộ hình ảnh của bạn bằng phím tắt hoặc nút:

* **Hình ảnh tiếp theo**: Bấm vào nút → hoặc nhấn phím**→** (Mũi tên phải)
* **Hình trước**: Bấm vào nút ← hoặc nhấn phím**←** (Mũi tên trái)
* **Chuyển đến hình ảnh cụ thể**: Quay lại Trình duyệt tệp và nhấp vào hình thu nhỏ mong muốn

#### Zoom control

Điều chỉnh độ phóng đại để kiểm tra chi tiết hình ảnh:

**Phóng to:***Bấm vào nút****** (Cộng)
* Nhấn phím **++ hoặc**=***Cuộn con cuộn** lên**

**Thu nhỏ:***Click vào nút**−** (Trừ)
* Nhấn phím **−** (Trừ)
* Cuộn con lăn chuột **xuống**

#### Xoay khi phóng to

Khi phóng to ra ngoài màn hình kích thước:

1. Di chuyển con trỏ chuột lên hình ảnh
2. Cún và **giữ chuột trái**

3.**Kéo** để chuyển hình ảnh xung quanh
4. Thả ra để dừng máy

**Cách khác**: Sử dụng mũi tên để xoay theo từng bước nhỏ***

## Kiểm tra pixel giá trị

### Xem pixel giá trị tại con trỏ

Khi bạn di chuyển con trỏ chuột lên hình ảnh, các pixel giá trị sẽ hiển thị theo thời gian thực:**Vị trí hiển thị giá trị:*** **Số nổi và đường màu đỏ ở mục mục bên phải Chú giải độ dốc LUT*****Khi phóng to hơn nữa, giá trị nổi gần con trỏ và pixel được đánh dấu***Hiển thị giá trị cho pixel**dưới con trỏ hoặc được đánh dấu*** Cập nhật khi bạn chuyển chuột***

## Loại hình ảnh bạn có thể xem

### JPG

**Hình ảnh JPG từ máy ảnh:**

* Hiển thị JPG dữ liệu như đã xem trước đó
* Show the root value, not edit
* Hữu ích cho việc kiểm tra chất lượng hình ảnh trước khi xử lý

### NGUYÊN (Bản gốc)

### RAW (Phản xạ)

**Sau khi xử lý:**

* Đã chỉnh sửa chi tiết
* Kích hoạt phản xạ
* Băng tần TIFF (Red, Green, NIR, v.v.)
* Dữ liệu học có sẵn để phân tích

### RAW (Chỉ mục)

**NDVI, NDRE, GNDVI, v.v. (tệp \_NDVI.tif):**

* Hình ảnh thang độ xám đơn băng tần
* Pixel giá trị biểu thị số chỉ số tính toán kết quả
* Phạm vi thường là -1 đến +1 đối với các số được chuẩn hóa
* Có thể áp dụng màu LUT để trực quan hóa

***

## Chỉ mục ứng dụng và LUT

Áp dụng các bảng màu đa phổ và bảng màu:

1. Xác định vị trí **Hộp cát Index/LUT**trong**Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> thanh bên
2. Chọn chỉ số thực vật (NDVI, NDRE, v.v.)
3. Chọn công thức đa phổ hoặc tạo tùy chọn điều chỉnh công thức của riêng bạn (chỉ Chloros+)
4. Áp dụng gradient LUT color để trực quan hóa
5. Điều chỉnh phạm vi giá trị và ngưỡng

Xem [Hộp cát Index/LUT](index-lut-sandbox.md) để biết hướng dẫn chi tiết.

***

## Tắt phím

###Điều hướng

* **→** (Mũi tên phải): Hình ảnh tiếp theo
* **←** (Mũi tên trái): Hình ảnh trước đó
* **Trang chủ**: Hình ảnh đầu tiên trong danh sách
* **Kết thúc**: Hình ảnh cuối cùng trong danh sách

###Phóng

* *******hoặc**=**: Phóng to
* **−**: Thu nhỏ
* **Con lăn chuột**: Phóng to/thu nhỏ***

### Xác minh số chỉ số tính toán

Kiểm tra xem các số được tính toán có độ chính xác không:

1. Open NDVI hoặc các chỉ mục ảnh khác
2. Kiểm tra vật liệu thực vật:
   * **NDVI**: Nên hiển thị 0,4-0,9 đối với cây khỏe mạnh
   * **NDRE**: Giá trị cao hơn cho sự tăng trưởng mạnh mẽ
   * **GNDVI**: NDVI tương tự nhưng nhạy cảm với văn bản
3. Kiểm tra vật thực hành:
   * **Đất**: Gần 0 hoặc hơi âm
   * **Nước**: Giá trị âm (-0,5 đến 0)***

##Khắc phục sự cố khi xem

### Ảnh không thể mở được

**Nguyên nhân có thể:**

* Tệp bị hỏng trong quá trình xử lý
* File Format không được hỗ trợ
* Không đủ bộ nhớ cho hình ảnh lớn

**Giải pháp:**

1. Thử mở trình xem bên ngoài để xác định tính toàn vẹn của tệp
2. Kiểm tra sự phù hợp của định dạng tệp phù hợp với loại dự án
3. Đóng các ứng dụng khác để giải phóng bộ nhớ
4. Thử nghiệm hình ảnh nhỏ hơn/khác

### Show image đen hoặc trắng

**Nguyên nhân có thể:**

* Phạm vi giá trị ngoài khả năng hiển thị
* Hình ảnh nổi 32-bit có giá trị bất ngờ
* Chỉ số tính toán lỗi

**Giải pháp:**

1. Kiểm tra pixel giá trị - nếu ở mức rất thấp hoặc rất cao, hãy điều chỉnh phạm vi hiển thị
2. Thử mở trong QGIS hoặc tương tự với việc điều chỉnh phạm vi tự động
3. Kiểm tra lỗi nhật ký trong quá trình xử lý để tìm lỗi

### Pixel giá trị có vẻ sai

**Nguyên nhân có thể:**

* Xem sai hình ảnh (bản gốc đã được xử lý)
* Chuẩn kích hoạt không được áp dụng chính xác
* Ánh sáng biến dữ liệu không được đưa vào đầu vào
* Không xác định chính xác phần trăm chuyển đổi chế độ

**Giải pháp:**

1. Xác minh rằng bạn đang xem đầu ra được xử lý (kiểm tra tệp hậu tố tên)
2. Kiểm tra phần trăm chế độ nút trạng thái
3. So sánh các hình ảnh nổi tiếng từ cùng một dữ liệu

***

## Các bước tiếp theo

Bây giờ bạn có thể xem hình ảnh toàn màn hình:

* [**Lớp hình ảnh**](image-layers.md) - Tìm hiểu về hiển thị băng tần tối đa
* [**Index/LUT Sandbox**](index-lut-sandbox.md) - Áp dụng các tùy chỉnh chỉ mục và màu ánh xạ
* [**Công thức đa mục mục phổ quát**](../project-settings/multispectral-index-formulas.md) - Tìm hiểu các số chỉ có sẵn

Để biết quy trình xử lý, hãy xem:

* [**Xử lý hình ảnh (GUI)**](../processing-images-gui/adding-files-to-a-project.md) - Hoàn thiện hướng dẫn xử lý