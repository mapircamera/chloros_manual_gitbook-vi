# Chọn ảnh mục tiêu

Đánh dấu hình ảnh nào chứa mục tiêu hiệu chuẩn là một bước quan trọng giúp tăng tốc đáng kể quy trình xử lý Chloros. Bằng cách chọn trước hình ảnh mục tiêu, bạn loại bỏ nhu cầu sử dụng Chloros để quét mọi hình ảnh trong tập dữ liệu của mình để tìm mục tiêu hiệu chỉnh.

## Tại sao lại đánh dấu hình ảnh mục tiêu?

### Tốc độ xử lý

Nếu không đánh dấu hình ảnh mục tiêu, Chloros phải:

* Quét từng hình ảnh trong dự án của bạn
* Chạy thuật toán phát hiện mục tiêu trên mỗi hình ảnh
* Kiểm tra hàng trăm, hàng nghìn hình ảnh không cần thiết

**Kết quả**: Quá trình xử lý có thể mất nhiều thời gian hơn, đặc biệt đối với các tập dữ liệu lớn.

### Với hình ảnh mục tiêu được đánh dấu

Khi bạn kiểm tra cột Mục tiêu để tìm hình ảnh cụ thể:

* Chloros chỉ quét các hình ảnh đã kiểm tra để tìm mục tiêu
* Phát hiện mục tiêu hoàn thành nhanh hơn nhiều
* Thời gian xử lý tổng thể giảm đáng kể

{% hint style="success" %}
**Cải thiện tốc độ**: Đánh dấu 2-3 hình ảnh mục tiêu trong tập dữ liệu 500 hình ảnh có thể giảm thời gian phát hiện mục tiêu từ hơn 30 phút xuống dưới 1 phút.
{% endhint %}***

## Cách đánh dấu ảnh mục tiêu

### Bước 1: Xác định hình ảnh mục tiêu của bạn

Xem qua các hình ảnh đã nhập của bạn trong Trình duyệt Tệp và xác định hình ảnh nào chứa mục tiêu hiệu chỉnh.
**Các tình huống thường gặp:***
**Mục tiêu bắt trước**: Bị bắt trước khi bắt đầu phiên
* **Mục tiêu sau khi bắt**: Bị bắt sau khi hoàn thành phiên
* **Mục tiêu tại hiện trường**: Mục tiêu được đặt trong khu vực đánh chiếm
* **Nhiều mục tiêu**: 2-3 hình ảnh mục tiêu mỗi phiên (được khuyến nghị)

### Bước 2: Kiểm tra cột mục tiêu

Đối với mỗi hình ảnh chứa mục tiêu hiệu chuẩn:

1. Xác định vị trí ảnh trong bảng File Browser
2. Tìm cột**Target** (cột ngoài cùng bên phải)
3. Nhấn vào ô đánh dấu ở cột Target cho hình ảnh đó
4. Lặp lại cho tất cả các hình ảnh có chứa mục tiêu

### Bước 3: Xác minh lựa chọn của bạn

Trước khi xử lý, hãy kiểm tra kỹ:

* [ ] Tất cả hình ảnh có mục tiêu hiệu chỉnh đều được kiểm tra
* [ ] Không có hình ảnh không phải mục tiêu nào được kiểm tra ngẫu nhiên
* [ ] Mục tiêu được hiển thị rõ ràng trong hình ảnh được kiểm tra

***

## Các phương pháp hay nhất cho hình ảnh mục tiêu

### Nguyên tắc nắm bắt mục tiêu**Thời gian:**

* Chụp ảnh mục tiêu ngay trước và trong suốt phiên chụp của bạn
* Trong cùng điều kiện ánh sáng với cảm biến ánh sáng DAQ của bạn
* Lý tưởng nhất là chụp ảnh mục tiêu thường xuyên nhất có thể để có kết quả tốt nhất. Nếu không, dữ liệu cảm biến ánh sáng sẽ được sử dụng để điều chỉnh hiệu chuẩn theo thời gian.

**Vị trí máy ảnh:**

* Giữ máy ảnh phía trên mục tiêu sao cho chính giữa và chiếm khoảng 40-60% tâm ảnh.
* Giữ camera song song/điểm thấp nhất với bề mặt mục tiêu

**Ánh sáng:**

* Ánh sáng xung quanh giống như cảm biến ánh sáng DAQ của bạn
* Tránh bóng trên bề mặt mục tiêu
* Đừng chặn nguồn sáng của bạn bằng cơ thể, xe cộ hoặc thảm thực vật
* Điều kiện u ám mang lại kết quả nhất quán

**Điều kiện mục tiêu:**

* Giữ tấm mục tiêu sạch sẽ và khô ráo
* Tất cả 4 tấm phải được nhìn thấy rõ ràng và không bị cản trở
* Nhắm mục tiêu vuông góc/điểm thấp nhất với nguồn sáng nếu có thể

### Có bao nhiêu hình ảnh mục tiêu?

**Tối thiểu:**1 hình ảnh mục tiêu mỗi phiên.**Khuyến nghị:**3-5 hình ảnh mục tiêu mỗi phiên.**Lịch tập luyện tốt nhất:**

* 3-5 hình ảnh được chụp ngay sau khi cảm biến ánh sáng ghi lại
* Xoay camera giữa các lần chụp để có kết quả tốt nhất
* Tùy chọn: định kỳ giữa buổi nếu điều kiện ánh sáng thay đổi liên tục

***

## Làm việc với nhiều máy ảnh

### Cài đặt máy ảnh kép

Nếu sử dụng đồng thời hai camera MAPIR (ví dụ: Survey3W RGN + Survey3N OCN):

1. Chụp ảnh mục tiêu bằng**cả hai camera**cùng lúc
2. Sử dụng**cùng một mục tiêu vật lý**cho cả hai máy ảnh
3. Đánh dấu hình ảnh mục tiêu cho**cả hai loại máy ảnh**trong Trình duyệt Tệp
4. Chloros sẽ sử dụng mục tiêu phù hợp cho việc hiệu chuẩn của từng camera

### Cột Model máy ảnh

Cột**Mẫu máy ảnh** giúp xác định hình ảnh nào đến từ máy ảnh nào:

* Survey3W\_RGN
* Khảo sát3N\_OCN
* Khảo sát3W\_RGB
* vân vân.

Sử dụng cột này để xác minh rằng bạn đã đánh dấu mục tiêu cho từng loại camera trong dự án của mình.

***

## Cài đặt phát hiện mục tiêu

### Điều chỉnh độ nhạy phát hiện

Nếu Chloros không phát hiện chính xác mục tiêu của bạn, hãy điều chỉnh các cài đặt này trong [Cài đặt dự án]( adjustment-project-settings.md):**Diện tích mẫu hiệu chuẩn tối thiểu:***
**Mặc định**: 25 pixel
* **Tăng** nếu phát hiện sai trên các đồ tạo tác nhỏ
* **Giảm** nếu không phát hiện được mục tiêu**Phân cụm mục tiêu tối thiểu:*** ** Mặc định**: 60
* **Tăng** nếu mục tiêu được chia thành nhiều lần phát hiện
* **Giảm** nếu mục tiêu có biến thể màu không được phát hiện đầy đủ***

## Các vấn đề phổ biến về hình ảnh mục tiêu

### Vấn đề: Không phát hiện thấy mục tiêu**Nguyên nhân có thể:**

* Nhắm mục tiêu hình ảnh không được đánh dấu trong File Browser
* Mục tiêu trong khung hình quá nhỏ (< 30% hình ảnh)
* Ánh sáng kém (bóng, chói)
* Cài đặt phát hiện mục tiêu quá nghiêm ngặt

**Giải pháp:**

1. Xác minh cột Target đã được kiểm tra hình ảnh chính xác chưa
2. Xem lại chất lượng hình ảnh mục tiêu trong bản xem trước
3. Giành lại mục tiêu nếu chất lượng kém
4. Điều chỉnh cài đặt phát hiện mục tiêu nếu cần

### Vấn đề: Phát hiện mục tiêu sai**Nguyên nhân có thể:**

* Các tòa nhà, xe cộ hoặc lớp phủ mặt đất màu trắng bị nhầm lẫn với mục tiêu
* Những mảng sáng trong thảm thực vật
* Độ nhạy phát hiện quá thấp

**Giải pháp:**

1. Chỉ đánh dấu ảnh mục tiêu thực tế để hạn chế phạm vi phát hiện
2. Tăng diện tích mẫu hiệu chuẩn tối thiểu
3. Tăng giá trị phân cụm mục tiêu tối thiểu
4. Đảm bảo hình ảnh mục tiêu chỉ hiển thị mục tiêu (nền tối thiểu bị lộn xộn)***

## Danh sách kiểm tra xác minh

Trước khi bắt đầu xử lý, hãy xác minh lựa chọn hình ảnh mục tiêu của bạn:

* [ ] Ít nhất 1 hình ảnh mục tiêu được đánh dấu mỗi phiên
* [ ] Các hộp kiểm cột mục tiêu được chọn cho tất cả các hình ảnh mục tiêu
* [ ] Hình ảnh mục tiêu được chụp trong cùng khung thời gian với khảo sát
* [ ] Mục tiêu hiển thị rõ ràng trong bản xem trước khi được nhấp vào
* [ ] Tất cả 4 bảng hiệu chuẩn hiển thị trong mỗi ảnh mục tiêu
* [ ] Không có bóng hoặc vật cản trên mục tiêu
* [ ] Đối với máy ảnh kép: Mục tiêu được đánh dấu cho cả hai loại máy ảnh

***

## Xử lý không có mục tiêu

### Xử lý không có mục tiêu hiệu chỉnh

Mặc dù không được khuyến nghị cho công việc khoa học nhưng bạn có thể xử lý mà không cần mục tiêu:

1. Bỏ chọn tất cả các hộp kiểm cột Mục tiêu
2.**Tắt**"Hiệu chỉnh phản xạ" trong Cài đặt dự án
3. Hiệu chỉnh họa tiết vẫn sẽ được áp dụng
4. Đầu ra sẽ không được hiệu chỉnh để có độ phản xạ tuyệt đối

{% hint style="warning" %}**Không được đề xuất**: Nếu không hiệu chỉnh độ phản xạ, giá trị pixel chỉ biểu thị độ sáng tương đối, không phải phép đo phản xạ khoa học. Sử dụng mục tiêu hiệu chuẩn để có kết quả chính xác, có thể lặp lại.
{% endhint %}***

## Các bước tiếp theo

Khi bạn đã đánh dấu hình ảnh mục tiêu của mình:

1.**Xem lại cài đặt của bạn**- Xem [Điều chỉnh cài đặt dự án]( adjustment-project-settings.md)
2.**Bắt đầu xử lý**- Xem [Bắt đầu xử lý](starting-the-processing.md)
3.**Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết thêm thông tin về chính các mục tiêu hiệu chuẩn, hãy xem [Mục tiêu hiệu chuẩn](../calibration-targets.md).
