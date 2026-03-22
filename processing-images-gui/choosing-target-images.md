# Select image item

Đánh dấu hình ảnh nào chứa tiêu chuẩn mục tiêu là một bước quan trọng giúp tăng tốc đáng kể quy trình xử lý Chloros. Bằng cách chọn trước hình ảnh mục tiêu, bạn loại bỏ nhu cầu sử dụng Chloros để quét mọi hình ảnh trong dữ liệu của mình để tìm hiệu chỉnh tiêu điểm mục tiêu.

## Tại sao đánh dấu tiêu điểm hình ảnh?

### Xử lý tốc độ

Không đánh dấu tiêu điểm hình ảnh, Chloros phải:

* Quét từng hình ảnh trong dự án của bạn
* Chạy tiêu điểm phân tích thuật toán trên mỗi hình ảnh
* Kiểm tra hàng trăm, hàng hình ảnh không cần thiết

**Kết quả**: Quá trình xử lý có thể bị mất nhiều thời gian hơn, đặc biệt đối với các dữ liệu lớn.

### Với tiêu điểm hình ảnh được đánh dấu

Khi bạn kiểm tra cột Mục tiêu để tìm cụ thể hình ảnh:

* Chloros chỉ kiểm tra các ảnh đã được kiểm tra để tìm mục tiêu
* Hoàn thành mục tiêu nhanh hơn nhiều
* Xử lý tổng hợp thời gian có thể giảm đáng kể

{% hint style="success" %}
**Cải thiện tốc độ**: Đánh dấu 2-3 hình ảnh mục tiêu trong tệp dữ liệu 500 hình ảnh có thể giảm thời gian phát hiện mục tiêu từ hơn 30 phút xuống dưới 1 phút.
{% endhint %}

***

##Cách đánh dấu tiêu điểm hình ảnh

### Bước 1: Xác định mục tiêu hình ảnh của bạn

Xem qua các hình ảnh bạn đã nhập trong Tệp trình duyệt và xác định hình ảnh nào chứa chỉnh sửa tiêu điểm.

**Vấn đề thường gặp:*** **Mục tiêu bắt trước**: Bị bắt trước khi bắt đầu phiên
* **Mục tiêu sau khi bắt**: Bị bắt sau khi hoàn thành phiên
* **Mục tiêu tại trường hiện tại**: Mục tiêu được đặt trong lĩnh vực sử dụng chiến đấu
* **Nhiều mục tiêu**: 2-3 hình ảnh mục tiêu mỗi phiên (được khuyến nghị)

### Bước 2: Kiểm tra mục tiêu

Đối với mỗi hình ảnh chứa tiêu chuẩn mục tiêu:

1. Xác định vị trí ảnh trong File Browser của bảng
2. Tìm cột **Target** (cột ngoài cùng bên phải)
3. Nhấn vào ô đánh dấu ở cột Target để chọn hình ảnh đó
4. Lặp lại cho tất cả các hình ảnh chứa mục tiêu

### Bước 3: Xác định lựa chọn của bạn

Trước khi xử lý, hãy kiểm tra kỹ năng:

* [ ] Tất cả các hình ảnh có hiệu chỉnh tiêu điểm đều được kiểm tra
* [ ] No image do not a focus nào được kiểm tra ngẫu nhiên
* [ ] Mục tiêu được hiển thị rõ ràng trong hình ảnh được kiểm tra

***

## Các phương pháp hay nhất cho hình ảnh tiêu điểm

### Nguyên tắc bắt tiêu điểm

**Thời gian:**

* Chụp ảnh tiêu điểm ngay trước đó và trong phiên bản chụp thông minh của bạn
* Trong cùng điều kiện ánh sáng với DAQ ánh sáng đa dạng của bạn
* Lý tưởng nhất là chụp ảnh mục tiêu thường xuyên nhất có thể để có kết quả tốt nhất. Nếu không, độ sáng của dữ liệu sẽ được sử dụng để điều chỉnh hiệu chuẩn theo thời gian.

**Máy ảnh định vị:**

* Giữ máy ảnh ở phía trên mục tiêu sao cho chính giữa và sử dụng khoảng 40-60% tâm ảnh.
* Giữ bài hát/điểm thấp nhất của bài hát camera ở bề mặt mục tiêu

**Ánh sáng:**

* Ánh sáng xung quanh tương tự như ánh sáng DAQ của bạn
* Tránh bóng trên bề mặt mục
* Đừng chặn nguồn sáng của bạn bằng cơ sở, xe cột hoặc thảm thực vật
* Điều kiện mang lại kết quả tốt nhất

**Mục điều kiện:**

* Giữ các mục tiêu sạch sẽ và khô ráo
* Tất cả 4 tấm phải được nhìn thấy rõ ràng và không bị cản trở
* Im lặng góc vuông/điểm thấp nhất với nguồn sáng nếu có thể

### Có bao nhiêu mục tiêu hình ảnh?

**Tối thiểu:**1 hình ảnh tiêu chuẩn mỗi phiên.**Khuyến nghị:**3-5 hình ảnh tiêu điểm mỗi phiên.**Lịch tập luyện tốt nhất:**

* 3-5 ảnh được chụp ngay sau khi viết lại biến ánh sáng
* Xoay camera giữa các lần chụp để có kết quả tốt nhất
* Tùy chọn: định nghĩa giữa phiên nếu điều kiện ánh sáng thay đổi liên tục

***

##Làm việc với nhiều máy ảnh

### Thiết lập máy ảnh kép

Nếu sử dụng máy ảnh đồng thời MAPIR (ví dụ: Survey3W RGN + Survey3N OCN):

1. Chụp ảnh tiêu điểm bằng **cả hai camera** cùng lúc
2. Sử dụng **cùng một mục tiêu** cho cả hai máy ảnh
3. Đánh dấu hình ảnh mục tiêu cho **tất cả các loại máy ảnh** trong Trình duyệt tệp
4. Chloros sẽ sử dụng các mục tiêu thích hợp để hiệu chỉnh từng máy ảnh

### Máy ảnh Model Model

Cột **Mẫu ảnh** giúp xác định hình ảnh nào đến từ bất kỳ máy ảnh nào:

* Survey3W\_RGN
* Survey3N\_OCN
* Survey3W\_RGB
* v.v.

Hãy sử dụng cột này để xác định rằng bạn đã đánh dấu được mục tiêu cho từng loại máy ảnh trong dự án của mình.

***

## Cài đặt tiêu điểm

### Phát hiện độ nhạy điều chỉnh

Nếu Chloros không phát hiện được mục tiêu chính xác của bạn, hãy điều chỉnh cài đặt này trong [Cài đặt dự án](adjusting-project-settings.md):**Dung tích tiêu chuẩn mẫu tối thiểu:*** **Mặc định**: 25 pixel
* **Tăng** nếu phát hiện sai trên các tác nhân tạo đồ
* **Giảm**nếu không phát hiện được các mục tiêu**Tối thiểu phân cụm mục tiêu:*** **Mặc định**: 60
* **Tăng** nếu mục tiêu được phát hiện thành nhiều lần
* **Giảm** nếu mục tiêu có biến màu không thể được phát hiện đầy đủ***

## Các vấn đề phổ biến về hình ảnh tiêu điểm

### Vấn đề: Không phát hiện được mục tiêu

**Nguyên nhân có thể:**

* Hình ảnh tiêu điểm ngay lập tức không được đánh dấu trong File Browser
* Mục tiêu trong khung hình quá nhỏ (< 30% hình ảnh)
* Ánh sáng thân thiện (bóng, sơn)
* Cài đặt tiêu điểm quá nghiêm ngặt

**Giải pháp:**

1. Xác minh cột Mục tiêu đã được kiểm tra hình ảnh chính xác chưa
2. Xem lại mục tiêu hình ảnh chất lượng trong bản xem trước
3. Giành lại mục tiêu nếu chất lượng kém
4. Điều chỉnh cài đặt tiêu điểm nếu cần

### Vấn đề: Sai tiêu điểm phát hiện

**Nguyên nhân có thể:**

* Các công trình nhà, phương tiện hoặc lớp phủ màu trắng của mặt đất được trộn lẫn với các mục tiêu
* Các mảng sáng trong thảm thực vật
* Phát hiện độ nhạy quá thấp

**Giải pháp:**

1. Chỉ đánh dấu hình ảnh thực tế mục tiêu để hạn chế phát hiện vi phạm
2. Tối thiểu tiêu chuẩn mẫu tăng cường
3. Tối thiểu tăng phân cụm giá trị
4. Bảo đảm hình ảnh mục tiêu chỉ hiển thị mục tiêu (tối thiểu khi quay vòng)

***

## Danh sách kiểm tra xác thực

Trước khi bắt đầu xử lý, hãy xác định lựa chọn hình ảnh mục tiêu của bạn:

* [ ] Ít nhất 1 hình ảnh được đánh dấu mỗi phiên
* [ ] Các mục tiêu trong cột kiểm tra được chọn cho tất cả các mục tiêu hình ảnh
* [ ] Hình ảnh tiêu điểm được chụp trong cùng khung thời gian khảo sát
* [ ] Hiển thị mục tiêu rõ ràng trong bản xem trước khi nhấp vào
* [ ] Tất cả 4 bảng hiệu hiển thị trong mỗi mục tiêu ảnh
* [ ] Không có bóng hoặc vật cản trên tiêu điểm
* [ ] Đối với máy ảnh kép: Mục tiêu được đánh dấu cho cả hai loại máy ảnh

***

## Xử lý không có mục tiêu

### Xử lý không có hiệu chỉnh tiêu điểm

Mặc dù không được khuyến khích cho công việc khoa học nhưng bạn có thể xử lý mà không cần tiêu điểm:

1. Bỏ chọn tất cả các mục tiêu của cột kiểm tra
2. **Tắt** "Hiệu chỉnh xạ" trong Cài đặt dự án
3. Hiệu chỉnh chi tiết vẫn sẽ được áp dụng
4. Đầu ra sẽ không được chỉnh sửa để có chế độ phản xạ tuyệt đối

{% hint style="warning" %}
**Không được xuất đề**: Nếu không điều chỉnh độ phản xạ, pixel giá trị chỉ biểu thị độ sáng tương phản, không được phép đo phản xạ khoa học. Sử dụng tiêu chuẩn mục tiêu để có kết quả chính xác, có thể lặp lại.
{% endhint %}

***

## Các bước tiếp theo

Khi bạn đã đánh dấu tiêu điểm hình ảnh của mình:

1. **Xem lại cài đặt của bạn** - Xem [Điều chỉnh dự án cài đặt](adjusting-project-settings.md)
2. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
3. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết thêm thông tin về tiêu chuẩn mục tiêu, hãy xem [Tiêu chuẩn mục tiêu](../calibration-targets.md).