# Lớp hình ảnh

Trình đơn thả xuống Lớp hình ảnh trong Trình xem hình ảnh Chloros cho phép bạn chuyển đổi nhanh chóng giữa các phiên bản khác nhau của cùng một hình ảnh - từ ảnh chụp ban đầu đến đầu ra phản xạ được xử lý và hình ảnh chỉ mục được tính toán.

## Lớp hình ảnh là gì?

Trong Chloros, **lớp** đề cập đến các hình ảnh đầu khác nhau có sẵn cho một hình ảnh nguồn duy nhất. Khi bạn xử lý hình ảnh, Chloros sẽ tạo nhiều phiên bản:

* **Hình ảnh gốc** (tệp JPG và RAW từ máy ảnh của bạn)
* **Đầu ra được điều chỉnh bằng cách phản xạ** (nếu điều chỉnh bằng cách kích hoạt phản xạ)
* **Tiêu điểm hình ảnh** (nếu hình ảnh chứa hiệu chỉnh tiêu điểm mục)
* **Chỉ mục hình ảnh**(NDVI, NDRE, GNDVI, v.v. if chỉ mục đã được định cấu hình)** Trình đơn thả xuống Bộ chọn**ở phía trên bên phải của Trình xem hình ảnh cho phép bạn chuyển đổi cài đặt ngay lập tức giữa các phiên bản này mà không cần phải rời khỏi trình xem.***

## Các loại lớp có sẵn

### JPG

* Hình ảnh xem trước gốc JPG từ máy ảnh của bạn
* Luôn có sẵn cho tất cả các hình ảnh
* Chưa được xử lý, như được chụp bởi máy ảnh
* Tải và hiển thị nhanh nhất

**Thời điểm xem:**

* Xem trước ảnh chụp nhanh gốc
* Kiểm tra bố cục và hình ảnh khung
* Xác minh chất lượng đã chụp trước khi xử lý

### NGUYÊN (Bản gốc)

* Gốc dữ liệu gốc của biến cảm biến RAW từ máy ảnh của bạn
* Đã gỡ bỏ mà không áp dụng quá trình xử lý bài đăng
* Độ sâu bit cao hơn JPG (thường là biến dữ liệu 12 bit hoặc 14 bit)

**Thời điểm xem:**

* Kiểm tra gốc biến thể dữ liệu chất lượng
* Kiểm tra các vấn đề về biến hoặc hiện tượng
* So sánh kết quả trước/sau quá trình xử lý

### RAW (Mục tiêu)

* Chỉ xuất hiện đối với các hình ảnh được xác định có chứa tiêu chuẩn tín hiệu
* Hiển thị ảnh gốc RAW với mục tiêu được phát hiện
* Được sử dụng để xác định công việc phát hiện mục tiêu đã thành công

**Thời điểm xem:**

* Xác định tiêu chuẩn nhận dạng đã được xác định chính xác
* Kiểm tra chất lượng hình ảnh tiêu điểm
* Khắc phục tiêu chuẩn cố định

{% hint style="info" %}
**Lớp mục**: Lớp này chỉ hiển thị trong danh sách thả xuống dành cho các hình ảnh có chứa hiệu chỉnh tiêu điểm. Ảnh chụp thông thường sẽ không có tùy chọn này.
{% endhint %}

### RAW (Phản xạ)

* Hình ảnh đầu ra phản xạ được điều chỉnh
* Đã chỉnh sửa chi tiết (nếu được bật trong quá trình xử lý)
* Độ phản xạ được chỉnh sửa bằng mục tiêu dữ liệu (nếu được bật)
* Băng tần TIFF với tất cả các kênh camera
* Pixel giá trị biểu thị tỷ lệ phần trăm phóng xạ (khi sử dụng phần trăm chế độ)
* Các tiện ích sẵn sàng hoạt động với [Index/LUT Sandbox](index-lut-sandbox.md)

**Thời điểm xem:**

* Kiểm tra tiêu chuẩn kết quả
* Kiểm tra tiêu chuẩn chất lượng
* Kiểm tra pixel giá trị để có khoa học chính xác
* So sánh với bản gốc để xem hiệu chỉnh hiệu ứng

{% hint style="success" %}
**Khuyến nghị**: Sử dụng lớp RAW (Phản xạ) khi kiểm tra pixel giá trị cho phép đo và phân tích khoa học.
{% endhint %}

### RAW (Chỉ số NDVI)... và tương tự

* Hình ảnh chỉ số thực vật được tính toán (NDVI trong ví dụ này)
* Thay đổi chỉ mục tên dựa trên bất kỳ mục nào được cấu hình trong quá trình xử lý
* Ví dụ: RAW (Chỉ số NDVI), RAW (Chỉ số NDRE), RAW (Chỉ số GNDVI), v.v.
* Hình ảnh thang độ xám đơn hiển thị kết quả tính toán chỉ số
* Một lớp xuất hiện cho mỗi mục được định cấu hình trong cài đặt dự án

**Mục chỉ tên có thể có:**

* RAW (Chỉ số NDVI)
* RAW (Chỉ số NDRE)
* RAW (Chỉ số GNDVI)
* RAW (Chỉ số OSAVI)
* RAW (Chỉ số EVI)
* RAW (Chỉ số SAVI)
* Và nhiều hơn nữa... (xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md))

**Thời điểm xem:**

* Kiểm tra chỉ số tính toán kết quả
* Kiểm tra số chỉ giá trị phạm vi
* Xác định lĩnh vực quan tâm
* Xác minh chỉ mục hình ảnh trước khi sử dụng trong GIS hoặc phân tích

***

## Use Use Layer Selector

### Mở menu thả xuống

1. Mở hình ảnh ở chế độ toàn màn hình (nhấp vào bất kỳ hình ảnh thu nhỏ nào trong Trình xem Hình ảnh)
2. Xác định vị trí **menu thả xuống lớp** ở góc trên bên phải của trình xem
3. Trình đơn thả xuống hiển thị lớp đã chọn (ví dụ: "JPG")
4. Click vào menu thả xuống để xem tất cả các lớp có sẵn

### Chuyển lớp

1. Click vào menu thả xuống lớp để mở danh sách
2. Tất cả các lớp có sẵn cho hình ảnh hiện tại đều được hiển thị
3. Bấm vào lớp bất kỳ để chuyển sang phiên bản đó
4. Cập nhật hình ảnh ngay lập tức để hiển thị lớp đã chọn

**Chuyển đổi nhanh:**

* Danh sách thả xuống ghi nhớ lựa chọn cuối cùng của bạn
* Khi điều chỉnh đến hình ảnh tiếp theo, Chloros cố gắng trình diễn cùng loại lớp
* If lớp đó không tồn tại trên hình ảnh tiếp theo, nó sẽ mặc định là JPG

### Tính khả năng của lớp

Không phải tất cả các lớp đều có sẵn cho mọi hình ảnh:

**Luôn có sẵn:*** ✅ JPG (mỗi hình ảnh đều có bản xem trước JPG)**Có điều kiện:**

* ⚠️ RAW (Bản gốc) - Chỉ khi ảnh được chụp ở chế độ RAW hoặc RAW+JPG
* ⚠️ RAW (Mục tiêu) - Chỉ khi hình ảnh chứa hiệu chỉnh tiêu điểm được phát hiện
* ⚠️ RAW (Phản xạ) - Chỉ sau khi xử lý với tính năng chỉnh sửa chế độ phản xạ được bật
* ⚠️ RAW (\[Index] Index) - Chỉ sau khi xử lý các mục được cấu hình chỉ mục

***

##Độ bền của lớp

### Điều hướng giữa các hình ảnh

Khi bạn điều chỉnh một hình ảnh khác (sử dụng mũi tên hoặc nhấp vào hình thu nhỏ):**Ưu tiên lớp được giữ nguyên:**

* Nếu xem "RAW (Phản xạ)", hình ảnh tiếp theo hiển thị "RAW (Phản xạ)" (nếu có)
* Nếu xem "RAW (Chỉ mục NDVI)", hình ảnh tiếp theo hiển thị "RAW (Chỉ mục NDVI)" (nếu có)
* If không tồn tại cùng một lớp, mặc định là JPG

**Mẫu công việc của quy trình:**

1. Mở Ảnh 1, chuyển sang RAW (Chỉ số NDVI)
2. Nhấn → để xem Hình 2
3. Hình ảnh 2 tự động hiển thị lớp RAW (NDVI Index)
4. Tiếp tục điều hướng - tất cả hình ảnh hiển thị lớp NDVI
5. Rất hiệu quả để xem chỉ mục kết quả nhận xét trên nhiều hình ảnh

***

## Quy trình công việc chung

### Quy trình 1: Trước/Sau so sánh

**Mục tiêu**: So sánh ảnh gốc và ảnh đã chỉnh sửa

1. Mở hình ảnh đã được xử lý trong Image Viewer
2. Choose **RAW (Bản gốc)** từ danh sách thả xuống
3. Lưu ý các chi tiết giá trị và chưa được hiệu chỉnh
4. Chuyển sang **RAW (Phản xạ)** từ danh sách thả xuống
5. So sánh - loại bỏ chi tiết, hiệu chỉnh giá trị

### Quy trình 2: Đánh giá chỉ mục

**Mục tiêu**: Xem xét kết quả nhanh NDVI trên dữ liệu

1. Mở hình ảnh được xử lý đầu tiên
2. Chọn **RAW (Chỉ mục NDVI)** từ danh sách thả xuống
3. Sử dụng mũi tên → để chuyển sang hình ảnh tiếp theo
4. Lớp NDVI tự động tồn tại
5. Tiếp tục xem tất cả các hình ảnh, kiểm tra các mẫu NDVI
6. Chuyển sang **RAW (Chỉ số NDRE)** để so sánh

### Quy trình 3: Xác định mục tiêu

**Mục tiêu**: Xác minh tất cả các mục tiêu hình ảnh được phát hiện chính xác

1. Điều hướng đến hình ảnh tiêu điểm
2. Choose **RAW (Mục tiêu)** từ danh sách thả xuống
3. Xác minh rằng tiêu chuẩn các mục tiêu được nhìn thấy và rõ ràng
4. Điều hướng đến hình ảnh tiêu điểm tiếp theo
5. Lặp lại xác thực cho tất cả các mục tiêu

### Quy trình 4: Kiểm tra pixel giá trị

**Mục tiêu**: Kiểm tra các xạ trị giá trị để đảm bảo độ chính xác về mặt khoa học

1. Mở trình xử lý ảnh
2. Chọn lớp **RAW (Phản xạ)**

3. Kích hoạt chế độ**Phần trăm pixel** (nút ở thanh công cụ bên phải)
4. Di chuyển con trỏ qua các thực vật
5. Xác minh pixel giá trị nằm trong phạm vi dự kiến (30-70% đối với NIR, 5-15% đối với Red)
6. Kiểm tra các vùng đất, nước để có giá trị phù hợp

***

## Tìm hiểu pixel giá trị theo lớp

Các lớp khác nhau hiển thị các pixel giá trị phạm vi khác nhau:

### Lớp JPG

* **Phạm vi**: 0-255 (8-bit)
* **Ý nghĩa**: Hiển thị giá trị, hiệu chỉnh gamma
* **Sử dụng**: Chỉ kiểm tra bằng mắt, không dùng để đo khoa học

### NGUYÊN (Bản gốc)

* **Phạm vi**: 0-65535 (16-bit)
* **Ý nghĩa**: Số liệu cảm biến thô
* **Sử dụng**: Kiểm tra hoạt động của biến cảm ứng, chưa chỉnh sửa

### RAW (Phản xạ)

* **Phạm vi**: 0-65.535 (16-bit TIFF) hoặc 0,0-1,0 (Phần trăm 32-bit)
* **Ý nghĩa**: Tỉ lệ phản xạ được chỉnh sửa
* **Sử dụng**: Đo lường và phân tích khoa học**Đối với TIFF 16 bit:**Chia cho 65.535 để nhận phần trăm phản xạ**Đối đầu với phần trăm 32 bit:** Giá trị biểu thị trực tiếp phần trăm (độ phản xạ 0,5 = 50%)

### RAW (Chỉ mục hình ảnh)

* **Phạm vi**: Thay đổi chỉ mục (Thường là -1,0 đến +1,0 đối với các chỉ số được chuẩn hóa)
* **Ý nghĩa**: Kết quả tính toán chỉ số
* **Ví dụ**:
  * NDVI: -1 đến +1 (thực tế là 0,4 đến 0,9)
  * NDRE: -1 đến +1 (phát hiện căng thẳng)
  * EVI: 0 đến 1 (thảm thực vật được tăng cường)

***

## Mẹo và cách thực hiện tốt nhất

### Chuyển đổi hiệu quả lớp

* **Nhận thức về các phím tắt**: Mặc dù không có phím tắt cho các lớp nhưng mũi tên điều hướng (←/→) hoạt động trên tất cả các lớp
* **Quy trình làm việc nhất quán**: Chọn một lớp (ví dụ: NDVI) và xem lại toàn bộ tập dữ liệu trước khi chuyển sang lớp khác
* **So sánh nhanh**: Chuyển đổi giữa Bản gốc và Phản chiếu để xử lý chất lượng cụ thể

### Cân nhắc về hiệu ứng

* **JPG tải nhanh nhất**: Sử dụng để điều hướng nhanh qua nhiều hình ảnh
* **Các lớp RAW tải xuống chậm hơn**: Độ phân giải và độ sâu bit cao hơn
* **Lớp chỉ mục**: Tốc độ tương tự như lớp Phản chiếu
* **Lần tải xuống chậm nhất**: Các lượt xem tiếp theo của cùng một lớp được lưu vào bộ nhớ đệm và nhanh hơn

### Xác minh chất lượng

* **Luôn kiểm tra RAW (Bản gốc)**: Xác minh nguồn dữ liệu chất lượng trước khi tin cậy các đầu ra được xử lý
* **So sánh các lớp**: Sử dụng chuyển đổi lớp để xác thực trong quá trình xử lý hoạt động chính xác
* **Kiểm tra chỉ mục vi phạm**: Sử dụng Pixel Percent chế độ với các chỉ mục lớp để xác định giá trị hợp lý***

##Khắc phục sự cố

### Lớp không có sẵn

**Cố gắng**: Lớp dự kiến ​​​​không xuất hiện trong danh sách thả xuống**Nguyên nhân có thể:**

* Hình ảnh chưa được xử lý (chỉ có JPG và RAW (Bản gốc))
* Kích hoạt phản xạ đã bị vô hiệu hóa trong quá trình xử lý
* Chỉ mục cụ thể chưa được định cấu hình trong Cài đặt dự án
* Hình ảnh chỉ là tiêu điểm hình ảnh (không có chỉ mục nào được tạo cho tiêu điểm)

**Giải pháp:**

1. Xác minh hình ảnh đã được xử lý (kiểm tra đầu thư mục để tìm các tệp đã được xử lý)
2. Kiểm tra cài đặt dự án để xác định các mục đã được định cấu hình
3. Xử lý lại các mục mong muốn được kích hoạt

### Hiển thị sai lớp

**Sự cố**: Hình ảnh mở ở lớp không mong muốn**Lý do**: Ưu tiên lớp từ hình ảnh trước đó được chuyển tiếp nhưng lớp đó không tồn tại trên hình ảnh hiện tại**Giải pháp**: Chloros tự động quay lại JPG khi không có lớp thích hợp - đây là hành vi bình thường

### Không thể nhìn thấy hiệu chỉnh tiêu điểm được tìm thấy

**Cố gắng**: Lớp RAW (Mục tiêu) không hiển thị tính năng phát hiện mục**Nguyên nhân có thể:**

* Mục tiêu không được phát hiện trong quá trình xử lý
* Hình ảnh thực tế không chứa mục tiêu
* Cài đặt tiêu điểm quá nghiêm ngặt

**Giải pháp:**

1. Kiểm tra lỗi gỡ lỗi nhật ký để biết thông báo "Đã tìm thấy mục tiêu"
2. Xác minh hình ảnh thực sự chứa màn hình tiêu chuẩn mục tiêu
3. Điều chỉnh cài đặt tiêu điểm trong Cài đặt dự án
4. Xem [Chọn mục ảnh](../processing-images-gui/choosing-target-images.md)

***

## Tính năng liên quan

### Công cụ xem ảnh

Khi xem bất kỳ lớp nào, bạn có thể sử dụng:

* **Điều khiển thu phóng**: Phóng to để kiểm tra chi tiết
* **Pan**: Tinh và kéo để chuyển đổi xung quanh hình ảnh được phóng to
* **Kiểm tra pixel giá trị**: Xem giá trị tại vị trí con trỏ
* **Mũi điều hướng tên**: Di chuyển giữa các hình ảnh trong khi duy trì lớp
* **Phần trăm pixel ở chế độ**: Chuyển đổi giữa màn hình DN và phần trăm

Xem [Mở toàn màn hình hình ảnh](opening-an-image-full-screen.md) để biết đầy đủ tài liệu về Trình xem hình ảnh.

### Hộp cát chỉ mục/LUT

Để kiểm tra và trực tiếp hóa các chỉ mục tương tác:

* **Tính chỉ số theo thời gian**: Kiểm tra các chỉ số khác nhau của công thức
* **Ánh xạ màu LUT**: Áp dụng chuyển màu cho các chỉ số thang độ xám
* **Xuất trực quan hóa**: Lưu hình ảnh màu chỉ mục

Xem [Hộp cát Index/LUT](index-lut-sandbox.md) để biết chi tiết.

***

## Các bước tiếp theo

Bây giờ bạn đã hiểu các hình ảnh lớp:

* [**Mở hình ảnh toàn màn hình**](opening-an-image-full-screen.md) - Hướng dẫn hoàn chỉnh về Trình xem hình ảnh
* [**Index/LUT Sandbox**](index-lut-sandbox.md) - Tương tác chỉ số trực quan
* [**Đa phổ chỉ mục công thức**](../project-settings/multispectral-index-formulas.md) - Có sẵn chỉ số tham chiếu
* [**Hoàn tất quá trình xử lý**](../processing-images-gui/finishing-the-processing.md) - Tìm hiểu kết quả xử lý đầu ra