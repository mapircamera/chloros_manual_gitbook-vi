# Mở một hình ảnh toàn màn hình

Trình xem ảnh Chloros cung cấp giao diện toàn màn hình chuyên dụng để xem, phân tích và thao tác với các hình ảnh đa phổ của bạn. Cho dù xem hình ảnh gốc hay kết quả đầu ra đã được xử lý, Trình xem Hình ảnh đều cung cấp các công cụ mạnh mẽ để kiểm tra và phân tích.

## Truy cập Trình xem ảnh

### Từ Trình duyệt Tệp

Cách phổ biến nhất để mở hình ảnh trong Trình xem ảnh:

1. Đảm bảo bạn đang ở tab **Trình duyệt tệp** <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line">
2. Nhấp vào bất kỳ **hình thu nhỏ** nào trong lưới hình ảnh
3. Hình ảnh mở ra trong **khu vực xem trước chính** (giữa màn hình)
4. Hình ảnh hiện đã được tải và sẵn sàng để xem toàn màn hình

### Mở tab Trình xem ảnh

Sau khi tải hình ảnh vào khu vực xem trước:

1. Nhấp vào biểu tượng **Trình xem ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> ở thanh bên trái
2. Tab Trình xem Hình ảnh mở ra, hiển thị toàn màn hình hình ảnh đã chọn
3. Các công cụ xem và phân tích nâng cao có sẵn ở thanh bên trái

***

## Tổng quan về giao diện trình xem hình ảnh

### Khu vực hiển thị chính

Phần lớn nhất của màn hình hiển thị hình ảnh của bạn:

* **Độ phân giải đầy đủ**: Hình ảnh được hiển thị ở độ phân giải gốc
* **Có thể thu phóng**: Sử dụng điều khiển hoặc con lăn chuột để thu phóng
* **Có thể xoay**: Nhấp và kéo để di chuyển xung quanh khi được phóng to
* **Tỷ lệ khung hình được duy trì**: Tỷ lệ hình ảnh theo tỷ lệ

***

## Tùy chọn xem

### Điều hướng hình ảnh cơ bản

#### Duyệt qua hình ảnh

Điều hướng qua bộ hình ảnh của bạn bằng phím tắt hoặc nút:

* **Hình ảnh tiếp theo**: Nhấp vào nút → hoặc nhấn phím **→** (Mũi tên phải)
* **Hình trước**: Bấm vào nút ← hoặc nhấn phím **←** (Mũi tên trái)
* **Chuyển đến hình ảnh cụ thể**: Quay lại Trình duyệt Tệp và nhấp vào hình thu nhỏ mong muốn

#### Điều khiển thu phóng

Điều chỉnh độ phóng đại để kiểm tra chi tiết hình ảnh:

**Phóng to:**

* Nhấp vào nút ****** (Cộng)
* Nhấn phím **++ hoặc **=**
* Cuộn con lăn chuột **lên**

**Thu nhỏ:**

* Nhấp vào nút **−** (Trừ)
* Nhấn phím **−** (Dấu trừ)
* Cuộn con lăn chuột **xuống**

#### Xoay khi phóng to

Khi phóng to ra ngoài kích thước màn hình:

1. Di chuyển con trỏ chuột lên hình ảnh
2. Nhấp và **giữ chuột trái**
3. **Kéo** để di chuyển hình ảnh xung quanh
4. Thả ra để dừng lia máy

**Cách khác**: Sử dụng phím mũi tên để xoay theo từng bước nhỏ

***

## Kiểm tra giá trị pixel

### Xem giá trị pixel tại con trỏ

Khi bạn di chuyển con trỏ chuột lên hình ảnh, các giá trị pixel sẽ hiển thị theo thời gian thực:

**Vị trí hiển thị giá trị:**

* **Số nổi và đường màu đỏ ở chỉ mục bên phải Chú giải độ dốc LUT**
* **Khi phóng to hơn nữa, giá trị nổi gần con trỏ và pixel được đánh dấu**
* Hiển thị giá trị cho pixel **dưới con trỏ hoặc được đánh dấu**
* Cập nhật khi bạn di chuyển chuột

***

## Loại hình ảnh bạn có thể xem

### JPG

**Hình ảnh JPG từ máy ảnh:**

* Hiển thị dữ liệu JPG như được xem trước
* Hiển thị giá trị gốc, chưa được sửa
* Hữu ích cho việc kiểm tra chất lượng hình ảnh trước khi xử lý

### NGUYÊN (Bản gốc)

### RAW (Phản xạ)

**Sau khi xử lý:**

* Đã sửa họa tiết
* Hiệu chỉnh phản xạ
* Đa băng tần TIFF (Red, Green, NIR, v.v.)
* Dữ liệu khoa học sẵn sàng để phân tích

### RAW (Chỉ mục)

**NDVI, NDRE, GNDVI, v.v. (tệp \_NDVI.tif):**

* Hình ảnh thang độ xám đơn băng tần
* Giá trị pixel biểu thị kết quả tính toán chỉ số
* Phạm vi thường là -1 đến +1 đối với các chỉ số được chuẩn hóa
* Có thể áp dụng LUT màu để trực quan hóa

***

## Ứng dụng chỉ mục và LUT

Áp dụng các chỉ số đa phổ và Bảng tra cứu màu sắc:

1. Xác định vị trí **Hộp cát Index/LUT** trong **Trình xem hình ảnh** thanh bên <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
2. Chọn chỉ số thực vật (NDVI, NDRE, v.v.)
3. Chọn công thức đa phổ hoặc tạo công thức tùy chỉnh của riêng bạn (chỉ Chloros+)
4. Áp dụng gradient LUT màu để trực quan hóa
5. Điều chỉnh phạm vi giá trị và ngưỡng

Xem [Hộp cát Index/LUT](index-lut-sandbox.md) để biết hướng dẫn chi tiết.

***

## Phím tắt

### Điều hướng

* **→** (Mũi tên phải): Hình ảnh tiếp theo
* **←** (Mũi tên trái): Hình ảnh trước đó
* **Trang chủ**: Hình ảnh đầu tiên trong danh sách
* **Kết thúc**: Hình ảnh cuối cùng trong danh sách

### Phóng

* ******* hoặc **=**: Phóng to
* **−**: Thu nhỏ
* **Con lăn chuột**: Phóng to/thu nhỏ

***

### Xác minh tính toán chỉ số

Kiểm tra xem các chỉ số được tính toán có chính xác không:

1. Mở NDVI hoặc hình ảnh chỉ mục khác
2. Kiểm tra diện tích thảm thực vật:
   * **NDVI**: Nên hiển thị 0,4-0,9 đối với cây khỏe mạnh
   * **NDRE**: Giá trị cao hơn cho sự tăng trưởng mạnh mẽ
   * **GNDVI**: Tương tự NDVI nhưng nhạy cảm với diệp lục
3. Kiểm tra thảm thực vật:
   * **Đất**: Gần 0 hoặc hơi âm
   * **Nước**: Giá trị âm (-0,5 đến 0)

***

## Khắc phục sự cố khi xem

### Ảnh không mở được

**Nguyên nhân có thể:**

* Tệp bị hỏng trong quá trình xử lý
* Định dạng tệp không được hỗ trợ
* Không đủ bộ nhớ cho hình ảnh lớn

**Giải pháp:**

1. Thử mở bằng trình xem bên ngoài để xác minh tính toàn vẹn của tệp
2. Kiểm tra định dạng tệp phù hợp với loại dự kiến
3. Đóng các ứng dụng khác để giải phóng bộ nhớ
4. Thử hình ảnh nhỏ hơn/khác

### Hiển thị hình ảnh đen hoặc trắng

**Nguyên nhân có thể:**

* Phạm vi giá trị ngoài khả năng hiển thị
* Hình ảnh nổi 32-bit có giá trị bất thường
* Lỗi tính chỉ số

**Giải pháp:**

1. Kiểm tra giá trị pixel - nếu tất cả rất thấp hoặc rất cao, hãy điều chỉnh phạm vi hiển thị
2. Thử mở trong QGIS hoặc tương tự với điều chỉnh phạm vi tự động
3. Kiểm tra nhật ký gỡ lỗi trong quá trình xử lý để tìm lỗi

### Giá trị pixel có vẻ sai

**Nguyên nhân có thể:**

* Xem hình ảnh sai (bản gốc so với đã xử lý)
* Hiệu chuẩn không được áp dụng chính xác
* Dữ liệu cảm biến ánh sáng không được đưa vào đầu vào
* Chế độ phần trăm chuyển đổi không chính xác

**Giải pháp:**

1. Xác minh rằng bạn đang xem đầu ra được xử lý (kiểm tra hậu tố tên tệp)
2. Kiểm tra trạng thái nút chế độ phần trăm
3. So sánh với các hình ảnh nổi tiếng từ cùng một tập dữ liệu

***

## Các bước tiếp theo

Bây giờ bạn có thể xem hình ảnh toàn màn hình:

* [**Lớp hình ảnh**](image-layers.md) - Tìm hiểu về hiển thị đa băng tần
* [**Index/LUT Sandbox**](index-lut-sandbox.md) - Áp dụng các chỉ mục tùy chỉnh và ánh xạ màu
* [**Công thức chỉ mục đa phổ**](../project-settings/multispectral-index-formulas.md) - Hiểu các chỉ số có sẵn

Để biết quy trình xử lý, hãy xem:

* [**Xử lý hình ảnh (GUI)**](../processing-images-gui/adding-files-to-a-project.md) - Hướng dẫn xử lý hoàn chỉnh