# Lớp hình ảnh

Trình đơn thả xuống Lớp hình ảnh trong Trình xem ảnh Chloros cho phép bạn nhanh chóng chuyển đổi giữa các phiên bản khác nhau của cùng một hình ảnh - từ ảnh chụp ban đầu đến đầu ra phản xạ đã xử lý và hình ảnh chỉ mục được tính toán.

## Lớp hình ảnh là gì?

Trong Chloros, **lớp** đề cập đến các đầu ra hình ảnh khác nhau có sẵn cho một hình ảnh nguồn. Khi bạn xử lý hình ảnh, Chloros sẽ tạo ra nhiều phiên bản:

* **Hình ảnh gốc** (tệp JPG và RAW từ máy ảnh của bạn)
* **Các đầu ra được hiệu chỉnh phản xạ** (nếu hiệu chỉnh phản xạ được bật)
* **Hình ảnh mục tiêu** (nếu hình ảnh chứa mục tiêu hiệu chỉnh)
* **Hình ảnh chỉ mục**(NDVI, NDRE, GNDVI, v.v. nếu chỉ mục đã được định cấu hình)**Trình đơn thả xuống Bộ chọn lớp**ở phía trên bên phải của Trình xem hình ảnh cho phép bạn chuyển đổi ngay lập tức giữa các phiên bản này mà không cần rời khỏi trình xem.***

## Các loại lớp có sẵn

### JPG

* Hình ảnh xem trước JPG gốc từ máy ảnh của bạn
* Luôn có sẵn cho tất cả các hình ảnh
* Chưa được xử lý, như được chụp bởi camera
* Tải và hiển thị nhanh nhất

**Thời điểm xem:**

* Xem trước nhanh bản chụp gốc
* Kiểm tra bố cục và khung hình ảnh
* Xác minh chất lượng chụp trước khi xử lý

### NGUYÊN (Bản gốc)

* Dữ liệu cảm biến RAW gốc từ máy ảnh của bạn
* Đã gỡ bỏ mà không áp dụng xử lý bài đăng
* Độ sâu bit cao hơn JPG (thường là dữ liệu cảm biến 12 bit hoặc 14 bit)

**Thời điểm xem:**

* Kiểm tra chất lượng dữ liệu cảm biến gốc
* Kiểm tra các vấn đề về cảm biến hoặc hiện vật
* So sánh kết quả trước/sau xử lý

### RAW (Mục tiêu)

* Chỉ xuất hiện đối với các hình ảnh được xác định là chứa mục tiêu hiệu chuẩn
* Hiển thị ảnh RAW gốc với mục tiêu được phát hiện
* Được sử dụng để xác minh việc phát hiện mục tiêu đã thành công

**Thời điểm xem:**

* Xác nhận mục tiêu hiệu chuẩn đã được phát hiện chính xác
* Kiểm tra chất lượng hình ảnh mục tiêu
* Khắc phục sự cố hiệu chuẩn

{% hint style="info" %}
**Lớp mục tiêu**: Lớp này chỉ xuất hiện trong danh sách thả xuống dành cho các hình ảnh có chứa mục tiêu hiệu chỉnh. Ảnh chụp thông thường sẽ không có tùy chọn này.
{% endhint %}

### RAW (Phản xạ)

* Hình ảnh đầu ra phản xạ được hiệu chỉnh
* Đã sửa họa tiết (nếu được bật trong quá trình xử lý)
* Độ phản xạ được hiệu chỉnh bằng dữ liệu mục tiêu (nếu được bật)
* TIFF đa băng tần với tất cả các kênh camera
* Giá trị pixel biểu thị độ phản xạ phần trăm (khi sử dụng chế độ phần trăm)
* Sẵn sàng thao tác với [Index/LUT Sandbox](index-lut-sandbox.md)

**Thời điểm xem:**

* Kiểm tra kết quả hiệu chuẩn
* Kiểm tra chất lượng hiệu chuẩn
* Kiểm tra giá trị pixel để có độ chính xác khoa học
* So sánh với bản gốc để xem hiệu ứng hiệu chỉnh

{% hint style="success" %}
**Khuyến nghị**: Sử dụng lớp RAW (Phản xạ) khi kiểm tra giá trị pixel cho các phép đo và phân tích khoa học.
{% endhint %}

### RAW (Chỉ số NDVI)... và tương tự

* Hình ảnh chỉ số thực vật được tính toán (NDVI trong ví dụ này)
* Tên chỉ mục thay đổi dựa trên chỉ mục nào được cấu hình trong quá trình xử lý
* Ví dụ: RAW (Chỉ số NDVI), RAW (Chỉ số NDRE), RAW (Chỉ số GNDVI), v.v.
* Hình ảnh thang độ xám đơn dải hiển thị kết quả tính toán chỉ số
* Một lớp xuất hiện cho mỗi chỉ mục được định cấu hình trong Cài đặt dự án

**Tên chỉ mục có thể có:**

* RAW (Chỉ số NDVI)
* RAW (Chỉ số NDRE)
* RAW (Chỉ số GNDVI)
* RAW (Chỉ số OSAVI)
* RAW (Chỉ số EVI)
* RAW (Chỉ số SAVI)
* Và nhiều hơn nữa... (xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md))

**Thời điểm xem:**

* Kiểm tra kết quả tính chỉ số
* Kiểm tra phạm vi giá trị chỉ số
* Xác định lĩnh vực quan tâm
* Xác minh hình ảnh chỉ mục trước khi sử dụng trong GIS hoặc phân tích

***## Sử dụng Bộ chọn lớp

### Mở menu thả xuống

1. Mở hình ảnh ở chế độ toàn màn hình (nhấp vào bất kỳ hình thu nhỏ nào trong Trình xem Hình ảnh)
2. Xác định vị trí**trình đơn thả xuống lớp**ở góc trên bên phải của trình xem
3. Trình đơn thả xuống hiển thị lớp hiện được chọn (ví dụ: "JPG")
4. Nhấp vào menu thả xuống để xem tất cả các lớp có sẵn

### Chuyển lớp

1. Nhấp vào menu thả xuống lớp để mở danh sách
2. Tất cả các lớp có sẵn cho hình ảnh hiện tại đều được hiển thị
3. Nhấp vào tên lớp bất kỳ để chuyển sang phiên bản đó
4. Hình ảnh cập nhật ngay lập tức để hiển thị lớp đã chọn**Chuyển đổi nhanh:**

* Danh sách thả xuống ghi nhớ lựa chọn cuối cùng của bạn
* Khi điều hướng đến hình ảnh tiếp theo, Chloros cố gắng hiển thị cùng loại lớp
* Nếu lớp đó không tồn tại trên hình ảnh tiếp theo, nó sẽ mặc định là JPG

### Tính khả dụng của lớp

Không phải tất cả các lớp đều có sẵn cho mọi hình ảnh:

**Luôn có sẵn:***✅ JPG (mỗi hình ảnh đều có bản xem trước JPG)**Có điều kiện:**

* ⚠️ RAW (Bản gốc) - Chỉ khi ảnh được chụp ở chế độ RAW hoặc RAW+JPG
* ⚠️ RAW (Mục tiêu) - Chỉ khi hình ảnh chứa mục tiêu hiệu chỉnh được phát hiện
* ⚠️ RAW (Phản xạ) - Chỉ sau khi xử lý với tính năng hiệu chỉnh độ phản xạ được bật
* ⚠️ RAW (\[Index] Index) - Chỉ sau khi xử lý với các chỉ mục được định cấu hình

***## Độ bền của lớp

### Điều hướng giữa các hình ảnh

Khi bạn điều hướng đến một hình ảnh khác (sử dụng phím mũi tên hoặc nhấp vào hình thu nhỏ):**Ưu tiên lớp được giữ nguyên:**

* Nếu xem "RAW (Phản xạ)", hình ảnh tiếp theo hiển thị "RAW (Phản xạ)" (nếu có)
* Nếu xem "RAW (Chỉ số NDVI)", hình ảnh tiếp theo hiển thị "RAW (Chỉ mục NDVI)" (nếu có)
* Nếu cùng một lớp không tồn tại, mặc định là JPG

**Quy trình làm việc mẫu:**1. Mở Ảnh 1, chuyển sang RAW (Chỉ số NDVI)
2. Nhấn → để xem Hình 2
3. Hình 2 tự động hiển thị lớp RAW (NDVI Index)
4. Tiếp tục điều hướng - tất cả hình ảnh đều hiển thị lớp NDVI
5. Rất hiệu quả để xem xét kết quả chỉ mục trên nhiều hình ảnh***

## Quy trình làm việc chung

### Quy trình 1: Trước/Sau so sánh**Mục tiêu**: So sánh hình ảnh gốc và hình ảnh đã hiệu chỉnh

1. Mở hình ảnh đã xử lý trong Image Viewer
2. Chọn**RAW (Bản gốc)**từ danh sách thả xuống
3. Lưu ý các giá trị họa tiết và chưa được hiệu chỉnh
4. Chuyển sang**RAW (Phản xạ)**từ danh sách thả xuống
5. So sánh - loại bỏ họa tiết, hiệu chỉnh các giá trị

### Quy trình 2: Đánh giá chỉ mục**Mục tiêu**: Xem xét nhanh kết quả NDVI trên tập dữ liệu

1. Mở hình ảnh được xử lý đầu tiên
2. Chọn**RAW (Chỉ số NDVI)**từ danh sách thả xuống
3. Sử dụng phím mũi tên → để chuyển sang hình ảnh tiếp theo
4. Lớp NDVI tự động tồn tại
5. Tiếp tục xem qua tất cả các hình ảnh, kiểm tra các mẫu NDVI
6. Chuyển sang**RAW (Chỉ số NDRE)**để so sánh

### Quy trình 3: Xác minh mục tiêu**Mục tiêu**: Xác minh tất cả hình ảnh mục tiêu được phát hiện chính xác

1. Điều hướng đến hình ảnh mục tiêu
2. Chọn**RAW (Mục tiêu)**từ danh sách thả xuống
3. Xác minh các mục tiêu hiệu chuẩn được nhìn thấy và phát hiện rõ ràng
4. Điều hướng đến hình ảnh mục tiêu tiếp theo
5. Lặp lại xác minh cho tất cả các mục tiêu

### Quy trình 4: Kiểm tra giá trị pixel**Mục tiêu**: Kiểm tra các giá trị phản xạ để đảm bảo độ chính xác về mặt khoa học

1. Mở ảnh đã xử lý
2. Chọn lớp**RAW (Phản xạ)**3. Bật chế độ**Phần trăm pixel**(nút ở thanh công cụ trên cùng bên phải)
4. Di chuyển con trỏ qua vùng thực vật
5. Xác minh giá trị pixel nằm trong phạm vi mong đợi (30-70% đối với NIR, 5-15% đối với Đỏ)
6. Kiểm tra các vùng đất, nước để có giá trị phù hợp***

## Tìm hiểu giá trị pixel theo lớp

Các lớp khác nhau hiển thị các phạm vi giá trị pixel khác nhau:

### Lớp JPG

* **Phạm vi**: 0-255 (8-bit)
* **Ý nghĩa**: Hiển thị giá trị, đã hiệu chỉnh gamma
* **Sử dụng**: Chỉ kiểm tra bằng mắt, không dùng để đo lường khoa học

### NGUYÊN (Bản gốc)

* **Phạm vi**: 0-65535 (16-bit)
* **Ý nghĩa**: Số liệu cảm biến thô
* **Sử dụng**: Kiểm tra hoạt động của cảm biến, chưa hiệu chỉnh

### RAW (Phản xạ)

* **Phạm vi**: 0-65.535 (TIFF 16 bit) hoặc 0,0-1,0 (Phần trăm 32 bit)
* **Ý nghĩa**: Phần trăm phản xạ đã được hiệu chỉnh
* **Sử dụng**: Đo lường và phân tích khoa học**Đối với TIFF 16 bit:**Chia cho 65.535 để có phần trăm phản xạ**Đối với Phần trăm 32 bit:** Các giá trị biểu thị trực tiếp phần trăm (0,5 = 50% phản xạ)

### RAW (Hình ảnh chỉ mục)

* **Phạm vi**: Thay đổi theo chỉ mục (thường là -1,0 đến +1,0 đối với các chỉ số được chuẩn hóa)
* **Ý nghĩa**: Kết quả tính chỉ số
* **Ví dụ**:
  * NDVI: -1 đến +1 (thực vật thường là 0,4 đến 0,9)
  * NDRE: -1 đến +1 (phát hiện ứng suất)
  * EVI: 0 đến 1 (thảm thực vật được tăng cường)

***

## Mẹo và cách thực hành tốt nhất

### Chuyển lớp hiệu quả

* **Nhận thức về phím tắt**: Mặc dù không có phím tắt cho các lớp nhưng mũi tên điều hướng (←/→) hoạt động trên tất cả các lớp
* **Quy trình làm việc nhất quán**: Chọn một lớp (ví dụ: NDVI) và xem lại toàn bộ tập dữ liệu trước khi chuyển sang lớp khác
* **So sánh nhanh**: Chuyển đổi giữa Bản gốc và Phản chiếu để xác minh chất lượng xử lý

### Cân nhắc về hiệu suất

* **JPG tải nhanh nhất**: Sử dụng để điều hướng nhanh qua nhiều hình ảnh
* **Các lớp RAW tải chậm hơn**: Độ phân giải và độ sâu bit cao hơn
* **Lớp chỉ mục**: Tốc độ tương tự như lớp Phản chiếu
* **Lần tải đầu tiên chậm nhất**: Các lượt xem tiếp theo của cùng một lớp được lưu vào bộ nhớ đệm và nhanh hơn

### Xác minh chất lượng

* **Luôn kiểm tra RAW (Bản gốc)**: Xác minh chất lượng dữ liệu nguồn trước khi tin cậy các đầu ra được xử lý
* **So sánh các lớp**: Sử dụng chuyển đổi lớp để xác thực quá trình xử lý hoạt động chính xác
* **Kiểm tra phạm vi chỉ mục**: Sử dụng chế độ Pixel Percent với các lớp chỉ mục để xác minh giá trị là hợp lý***## Khắc phục sự cố

### Lớp không có sẵn**Sự cố**: Lớp dự kiến ​​không xuất hiện trong danh sách thả xuống**Nguyên nhân có thể:**

* Hình ảnh chưa được xử lý (chỉ có JPG và RAW (Bản gốc))
* Hiệu chỉnh phản xạ đã bị vô hiệu hóa trong quá trình xử lý
* Chỉ mục cụ thể chưa được định cấu hình trong Cài đặt dự án
* Hình ảnh chỉ là hình ảnh mục tiêu (không có chỉ mục nào được tạo cho mục tiêu)

**Giải pháp:**1. Xác minh hình ảnh đã được xử lý (kiểm tra thư mục đầu ra để tìm các tệp đã được xử lý)
2. Kiểm tra Cài đặt dự án để xác nhận các chỉ mục đã được định cấu hình
3. Xử lý lại với các chỉ mục mong muốn được kích hoạt

### Hiển thị sai lớp**Sự cố**: Hình ảnh mở ở lớp không mong muốn**Lý do**: Ưu tiên lớp từ hình ảnh trước đó được chuyển tiếp nhưng lớp đó không tồn tại trên hình ảnh hiện tại**Giải pháp**: Chloros tự động chuyển về JPG khi không có lớp ưa thích - đây là hiện tượng bình thường

### Không thể nhìn thấy mục tiêu hiệu chỉnh**Sự cố**: Lớp RAW (Mục tiêu) không hiển thị tính năng phát hiện mục tiêu**Nguyên nhân có thể:**

* Mục tiêu không được phát hiện trong quá trình xử lý
* Hình ảnh thực tế không chứa mục tiêu
* Cài đặt phát hiện mục tiêu quá nghiêm ngặt

**Giải pháp:**1. Kiểm tra Nhật ký gỡ lỗi để biết thông báo "Đã tìm thấy mục tiêu"
2. Xác minh hình ảnh thực sự chứa các mục tiêu hiệu chuẩn hiển thị
3. Điều chỉnh cài đặt phát hiện mục tiêu trong Cài đặt dự án
4. Xem [Chọn hình ảnh mục tiêu](../processing-images-gui/choosing-target-images.md)***

## Tính năng liên quan

### Công cụ xem ảnh

Khi xem bất kỳ lớp nào, bạn có thể sử dụng:

* **Điều khiển thu phóng**: Phóng to để kiểm tra chi tiết
* **Pan**: Nhấp và kéo để di chuyển xung quanh hình ảnh được phóng to
* **Kiểm tra giá trị pixel**: Xem giá trị tại vị trí con trỏ
* **Mũi tên điều hướng**: Di chuyển giữa các hình ảnh trong khi duy trì lớp
* **Chế độ Phần trăm pixel**: Chuyển đổi giữa hiển thị DN và phần trăm

Xem [Mở toàn màn hình hình ảnh](opening-an-image-full-screen.md) để biết tài liệu đầy đủ về Trình xem hình ảnh.

### Hộp cát chỉ mục/LUT

Để kiểm tra và trực quan hóa chỉ mục tương tác:

* **Tính chỉ số theo thời gian thực**: Kiểm tra các công thức chỉ số khác nhau
* **Ánh xạ màu LUT**: Áp dụng chuyển màu cho các chỉ số thang độ xám
* **Xuất trực quan hóa**: Lưu hình ảnh chỉ mục màu

Xem [Index/LUT Sandbox](index-lut-sandbox.md) để biết chi tiết.***

## Các bước tiếp theo

Bây giờ bạn đã hiểu các lớp hình ảnh:

* [ **Mở hình ảnh toàn màn hình**](opening-an-image-full-screen.md) - Hướng dẫn hoàn chỉnh về Trình xem hình ảnh
* [ **Index/LUT Sandbox**](index-lut-sandbox.md) - Trực quan hóa chỉ mục tương tác
* [ **Công thức chỉ mục đa phổ**](../project-settings/multispectral-index-formulas.md) - Tham chiếu các chỉ số có sẵn
* [ **Hoàn tất quá trình xử lý**](../processing-images-gui/finishing-the-processing.md) - Tìm hiểu kết quả đầu ra được xử lý
