# Giám sát trong quá trình xử lý

Sau khi quá trình xử lý bắt đầu, Chloros cung cấp một số cách để theo dõi quá trình, kiểm tra sự cố và hiểu điều gì đang xảy ra với dữ liệu của bạn. Đây là giải pháp theo dõi quá trình xử lý của bạn và cung cấp giải pháp Chloros thông tin.

## Tổng quan về tiến trình

Thanh tiến trình ở tiêu đề trên cùng hiển thị trạng thái xử lý theo thời gian thực và phần trăm hoàn thành.

### Thanh tiến trình miễn phí

Đối với người dùng không có giấy phép Chloros+:

**Hiển thị giai đoạn 2 cấp độ:**

1.**Phát mục tiêu** - Tìm kiếm chỉnh sửa tiêu điểm trong hình ảnh
2. **Đang xử lý**- Áp dụng chỉnh sửa và xuất**Thanh tiến trình hiển thị:**

* Tỷ lệ hoàn thành có thể (0-100%)
* Current Art
* Đơn giản hóa thanh ngang trực quan

### Chloros+ Tiến trình

Đối với người dùng có giấy phép Chloros+:

**Hiển thị tiến trình 4 giai đoạn:**

1.**Phát hiện** - Tìm kiếm tiêu chuẩn
2. **Phân tích** - Kiểm tra hình ảnh và đường chuẩn
3. **Chỉnh sửa** - Áp dụng hiệu chỉnh chi tiết và độ phản chiếu
4. **Xuất**- Lưu các tập tin đã xử lý**Tính năng tương tác:*** **Di chuột qua** thanh tiến trình để xem bảng 4 mở rộng giai đoạn
* **Nhấp vào** thanh tiến trình để cố gắng mở rộng bảng định nghĩa/ghim
* **Nhấp xuống** để giải nén và tự động ẩn khi rời chuột
* Mỗi giai đoạn cho thấy sự tiến bộ của từng cá nhân (0-100%)

***

## Tìm hiểu từng giai đoạn xử lý

{% hint style="info" %}
**Đường ống kiến trúc**: GUI 4 giai đoạn này tương ứng với [trình xử lý đường dẫn 4 luồng](../processing-architecture/processing-pipeline.md). Trên các hệ thống có khả năng tăng tốc GPU, Thread 3 (Hiệu chỉnh) có thể được hưởng lợi từ [Thích ứng điện động](../processing-architecture/dynamic-compute-adaptation.md) giúp tối ưu hóa quá trình xử lý cho phần cứng công cụ của bạn.
{% endhint %}

### Giai đoạn 1: Phát hiện (Phát mục tiêu)

**Chuyện gì đang xảy ra vậy:**

* Chloros quét các hình ảnh được đánh dấu bằng hộp kiểm Target
* Thuật toán giác giác máy tính xác định 4 chuẩn bảng
* Giá trị phản xạ được trích xuất từ ​​mỗi bảng
* Ghi lại tiêu điểm thời gian để lập lịch phù hợp với tiêu chuẩn

**Khoảng thời gian:**

* Với dấu mục tiêu được đánh dấu: 10-60 giây
* Không có mục tiêu nào được đánh dấu: 5-30+ phút (quét tất cả hình ảnh)

**Chỉ báo tiến độ:**

* Hiện tại: 0% → 100%
* Số lượng hình ảnh được quét
* Số mục tiêu được tìm thấy

**Những thứ cần xem:**

* Nên hoàn thành nhanh chóng nếu mục tiêu được đánh dấu đúng
* Nếu bị mất quá nhiều thời gian, mục tiêu có thể không được đánh dấu
* Kiểm tra lỗi gỡ lỗi nhật ký để biết thông báo "Đã tìm thấy tiêu điểm"

### Giai đoạn 2: Phân tích

**Chuyện gì đang xảy ra vậy:**

* Đọc hình ảnh EXIF ​​siêu dữ liệu (dấu thời gian, cài đặt ánh sáng)
* Xác định cơ sở điều chỉnh chiến lược dựa trên tiêu điểm thời gian
* Tổ chức xử lý ảnh hàng đợi
* Chuẩn bị bài hát xử lý công nhân (chỉ Chloros+)

**Thời lượng:**5-30 giây**Chỉ báo tiến độ:**

* Phân tích: 0% → 100%
* Giai đoạn nhanh, thường kết thúc nhanh

**Những thứ cần xem:**

* Nên tiến triển đều đặn không ngừng nghỉ
* Cảnh báo thiếu siêu dữ liệu sẽ xuất hiện trong lỗi gỡ lỗi nhật ký

### Giai đoạn 3: Hiệu chỉnh

**Chuyện gì đang xảy ra vậy:*** **Debayering**: Chuyển đổi mẫu RAW Bayer thành 3 kênh
* **Chỉ họa tiết**: Loại bỏ vết đen ở cạnh ống kính
* **Phản xạ chỉnh sửa**: Chuẩn hóa với tiêu điểm giá trị
* **Tính toán chỉ số**: Tính toán các chỉ số đa phổ
* Xử lý từng hình ảnh thông qua đường ống đầy đủ

**Thời lượng:**Xử lý thời gian phần lớn (60-80%)**Chỉ báo tiến độ:**

* Hiệu chuẩn: 0% → 100%
* Đang xử lý hiện tại hình ảnh
* Hình ảnh hoàn thành / Tổng số hình ảnh

**Hành vi xử lý:*** **Chế độ miễn phí**: Xử lý tuần tự từng hình ảnh
* **Chế độ Chloros+**: Xử lý đồng thời tối đa 16 hình ảnh
* **Tăng tốc GPU**: Tăng tốc đáng kể giai đoạn này**Những gì cần xem:**

* Tiến bộ ổn định thông tin về số lượng hình ảnh
* Kiểm tra lỗi gỡ lỗi nhật ký để biết thông báo hoàn thành trên mỗi hình ảnh
* Cảnh báo về vấn đề chất lượng hình ảnh hoặc tiêu chuẩn

### Giai đoạn 4: Xuất khẩu

**Chuyện gì đang xảy ra vậy:**

* Ghi hình ảnh đã được chỉnh sửa vào đĩa ở dạng đã chọn
* Xuất đa chỉ mục hình ảnh với LUT màu
* Tạo máy ảnh thư mục
* Giữ nguyên tên tập tin với điều chỉnh hậu tố thích hợp

**Thời lượng:**Xử lý tổng thời gian 10-20%**Chỉ báo tiến độ:**

* Xuất: 0% → 100%
* File đang được ghi
* Xuất và định dạng

**Những thứ cần xem:**

* Cảnh báo dung lượng ổ đĩa
* File write error
* Hoàn thành tất cả các cấu hình đầu ra

***

## Gỡ lỗi tab

Nhật ký gỡ lỗi cung cấp thông tin chi tiết về quá trình xử lý và mọi vấn đề phải gặp.

### Lỗi gỡ bỏ bản cập nhật

1. Bấm vào biểu tượng **Nhật ký gỡ lỗi** <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> ở thanh bên trái
2. Nhật ký mở bảng hiển thị quá trình xử lý thông báo theo thời gian
3. Tự động cuộn để hiển thị tin nhắn mới nhất

###Tìm hiểu thông điệp thông báo

#### Tin nhắn thông tin (Trắng/Xám)

Cập nhật thông tin xử lý thông thường:

```
[INFO] Processing started
[INFO] Target detected in IMG_0015.RAW - 4 panels found
[INFO] Calibrating IMG_0234.RAW
[INFO] Exported NDVI image: IMG_0234_NDVI.tif
[INFO] Processing complete
```

#### Thông báo cảnh báo (Vàng)

Sự cố không nghiêm trọng không được xử lý:

```
[WARN] No GPS data found in IMG_0145.RAW
[WARN] Target image timestamp gap > 30 minutes
[WARN] Low contrast in calibration panel - results may vary
```

**Hành động:** Xem lại cảnh báo sau khi xử lý nhưng không làm gián đoạn

#### Lỗi thông báo (Red)

Các vấn đề nghiêm trọng có thể khiến quá trình xử lý không thành công:

```
[ERROR] Cannot write file - disk full
[ERROR] Corrupted image file: IMG_0299.RAW
[ERROR] No targets detected - enable reflectance calibration or mark target images
```

**Hành động:** Dừng xử lý, giải quyết lỗi, khởi động lại

###Nhật ký chung thông báo

| Tin nhắn | Ý nghĩa | Cần hành động |
| -------------------------------- | -------------------------------------- | ----------------------------------------------------- |
| "Đã phát hiện mục tiêu trong \[tên tệp]" | Đã tìm thấy công cụ chỉnh sửa tiêu điểm thành công | Không - bình thường |
| "Xử lý ảnh X của Y" | Cập nhật hiện tại tiến trình | Không - bình thường |
| "Không tìm thấy mục tiêu" | Không thể phát hiện được tiêu chuẩn đầu mục | Đánh dấu tiêu điểm hình ảnh hoặc tắt chế độ phản xạ chỉnh sửa |
| "Không đủ dung lượng đĩa" | Không đủ dung lượng lưu trữ cho đầu ra | Giải thích dung lượng ổ đĩa phóng to |
| "Bỏ qua file bị hỏng" | Ảnh tập tin bị hỏng | Sao chép tập tin từ thẻ SD |
| "Dữ liệu PPK được ứng dụng" | Đã áp dụng chỉnh sửa GPS từ tệp .daq | Không - bình thường |

### Sao chép nhật ký dữ liệu

Để sao chép bản cập nhật nhằm giải quyết vấn đề hoặc hỗ trợ:

1. Mở gỡ lỗi nhật ký bảng
2. Bấm vào nút **"Sao chép nhật ký"** (hoặc nhấp chuột phải → Chọn tất cả)
3. Dán vào tập tin văn bản hoặc email
4. Gửi tới bộ phận hỗ trợ MAPIR nếu cần

***

##Giám sát hệ thống tài nguyên

### Cách sử dụng CPU

**Chế độ miễn phí:**

* 1 CPU lõi ở mức \~100%
* Các nền tảng khác hoặc có sẵn
* System is not reply

**Bài hát Chloros+ Chế độ:**

* Nhiều cốt lõi ở mức 80-100% (tối đa 16 cốt lõi)
* Tổng mức sử dụng CPU có thể cao
* Hệ thống có thể cảm nhận được phản hồi tốt hơn

**Để theo dõi:**

* Trình quản lý tác vụ Windows (Ctrl+Shift+Esc)
* Hiệu suất tab → CPU phần
* Tìm kiếm quy trình "Chloros" hoặc "chloros-backend"

### Cách sử dụng bộ nhớ (RAM)

**Cách sử dụng hình điển:**

* Dự án nhỏ (< 100 hình ảnh): 2-4 GB
* Dự án trung bình (100-500 hình ảnh): 4-8 GB
* Dự án lớn (500+ hình ảnh): 8-16 GB
* Chế độ bài hát Chloros+ dùng nhiều RAM hơn

**Nếu bộ nhớ sắp hết:**

* Xử lý các lô nhỏ hơn
* Đóng các ứng dụng khác
* Nâng cấp RAM nếu thường xuyên Xử lý dữ liệu lớn

### Cách sử dụng GPU (Chloros+ với CUDA)

Khi tính năng tăng tốc GPU được bật:

* GPU NVIDIA cho thấy mức sử dụng cao (60-90%)
* Tăng mức sử dụng VRAM (yêu cầu 4GB+ VRAM)
* Giai đoạn chỉnh sửa nhanh hơn đáng kể

**Để theo dõi:**

* Biểu tượng Khay hệ thống NVIDIA
* Trình quản lý tác vụ → Hiệu suất → GPU
* GPU-Z hoặc công cụ giám sát tương tự

### Đĩa I/O

**Điều gì sẽ xảy ra:**

* Tốc độ đọc đĩa cao trong giai đoạn phân tích
* Ghi cao đĩa trong giai đoạn Xuất
* SSD nhanh hơn đáng kể so với HDD

**Mao performance:**

* Sử dụng SSD cho dự án thư mục khi có thể
* Tránh mạng ổ đĩa cho dữ liệu lớn
* Đảm bảo đĩa không hết dung lượng (ảnh hưởng đến tốc độ ghi)

***

## Cố gắng phát triển trong quá trình xử lý

###Dấu cảnh báo

**Tiến trình được di chuyển (không thay đổi trong hơn 5 phút):**

* Kiểm tra nhật ký gỡ lỗi để tìm lỗi
* Xác minh dung lượng trống của đĩa
* Kiểm tra tác vụ trình quản lý để đảm bảo Chloros đang chạy

**Thông báo lỗi xuất hiện thường xuyên:**

* Dừng xử lý và xem xét lỗi
* Nguyên nhân thường gặp: ổ đĩa dung lượng lớn, tệp bị hỏng, vấn đề về bộ nhớ
* Xem phần khắc phục sự cố bên dưới

**Hệ thống không phản hồi:**

* Bài hát chế độ Chloros+ sử dụng quá nhiều tài nguyên
* Cân nhắc giảm bớt các tác vụ đồng thời hoặc nâng cấp phần cứng
* Chế độ miễn phí ít tài nguyên hơn

### Khi nào nên ngừng xử lý

Dừng quá trình xử lý nếu bạn thấy:

* ❌ Error "Đĩa đầy" hoặc "Không thể ghi tệp"
* ❌ Lỗi làm hỏng vòng lặp ảnh của tệp lặp lại
* ❌ Hệ thống được đóng băng hoàn toàn (không phản hồi)
* ❌ Đã nhận được lỗi cài đặt đã được định cấu hình
* ❌ Import bad image

**Điểm dừng:**

1. Kích vào**Nút Dừng/Hủy** (thay thế nút Bắt đầu)
2. Quá trình xử lý bị dừng, quá trình xử lý bị mất
3. Khắc phục sự cố và khởi động lại từ đầu

***

##Khắc phục sự cố trong quá trình xử lý

### Quá trình xử lý rất chậm

**Nguyên nhân có thể:**

* Tiêu điểm hình ảnh không được đánh dấu (quét tất cả hình ảnh)
* HDD thay thế SSD
* Hệ thống tài nguyên không đủ
* Nhiều cấu hình được chỉ định
* Truy cập mạng ổ đĩa

**Giải pháp:**

1. Nếu mới bắt đầu và đang trong giai đoạn Phát triển: Hủy, đánh dấu mục tiêu, khởi động lại
2. Tương lai: Sử dụng SSD, giảm chỉ số, nâng cấp phần cứng
3. Cân nhắc CLI để xử lý hàng loạt dữ liệu

### Cảnh báo "Dung lượng ổ đĩa"

**Giải pháp:**

1. Giải thích dung lượng ổ đĩa ngay lập tức
2. Chuyển dự án sang ổ đĩa có nhiều không gian hơn
3. Giảm số lượng chỉ cần xuất
4. Sử dụng định dạng JPG thay vì TIFF (tệp nhỏ hơn)

### Thông báo "Tep bị hỏng" thường xuyên

**Giải pháp:**

1. Sao chép lại hình ảnh từ thẻ SD để đảm bảo an toàn
2. Kiểm tra thẻ SD lỗi
3. Xóa các tập tin bị hỏng khỏi dự án
4. Tiếp tục xử lý lại các ảnh còn lại

### System too hot / Giảm ga

**Giải pháp:**

1. Đảm bảo thông gió đầy đủ
2. Dọn dẹp bụi bẩn trên máy tính
3. Giảm tải xử lý (sử dụng chế độ Miễn phí thay vì Chloros+)
4. Xử lý khoảnh mát mẻ trong ngày

***

## Đang xử lý hoàn chỉnh thông báo

Khi quá trình xử lý kết thúc:

* Thanh tiến độ đạt 100%
* **Thông báo "Đang xử lý hoàn tất"** xuất hiện trong lỗi Nhật ký gỡ lỗi
* Nút bắt đầu được bật lại
* Tất cả các đầu tập tin đều nằm trong thư mục của máy ảnh mẫu

***

## Các bước tiếp theo

Sau khi quá trình xử lý hoàn tất:

1. **Xem lại kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)
2. **Kiểm tra đầu thư mục** - Xác minh tất cả các tệp được xuất chính xác
3. **Xem lại nhật ký gỡ lỗi** - Kiểm tra mọi cảnh báo hoặc lỗi
4. **Xem trước hình ảnh đã được xử lý** - Use Image Viewer hoặc phần mềm bên ngoài

Để biết thông tin về cách xem xét và sử dụng kết quả đã xử lý của bạn, hãy xem [Hoàn tất quá trình xử lý](finishing-the-processing.md).