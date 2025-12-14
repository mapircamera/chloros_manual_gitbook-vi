# Giám sát quá trình xử lý

Sau khi quá trình xử lý bắt đầu, Chloros cung cấp một số cách để theo dõi tiến trình, kiểm tra sự cố và hiểu điều gì đang xảy ra với tập dữ liệu của bạn. Trang này giải thích cách theo dõi quá trình xử lý của bạn và diễn giải thông tin mà Chloros cung cấp.

## Tổng quan về thanh tiến trình

Thanh tiến trình ở tiêu đề trên cùng hiển thị trạng thái xử lý theo thời gian thực và phần trăm hoàn thành.

### Thanh tiến trình chế độ miễn phí

Đối với người dùng không có giấy phép Chloros+:

**Hiển thị tiến độ 2 giai đoạn:**1.** Phát hiện mục tiêu**- Tìm mục tiêu hiệu chuẩn trong hình ảnh
2.**Đang xử lý**- Áp dụng chỉnh sửa và xuất**Thanh tiến trình hiển thị:**

* Tỷ lệ hoàn thành tổng thể (0-100%)
* Nghệ danh hiện tại
* Trực quan hóa thanh ngang đơn giản

### Chloros+ Thanh tiến trình

Đối với người dùng có giấy phép Chloros+:

**Hiển thị tiến độ 4 giai đoạn:**1.** Phát hiện**- Tìm mục tiêu hiệu chuẩn
2.**Phân tích**- Kiểm tra hình ảnh và chuẩn bị đường dẫn
3.**Hiệu chỉnh**- Áp dụng hiệu chỉnh họa tiết và độ phản chiếu
4.**Xuất**- Lưu các tập tin đã xử lý**Tính năng tương tác:***
**Di chuột qua** thanh tiến trình để xem bảng 4 giai đoạn mở rộng
* **Nhấp vào** thanh tiến trình để cố định/ghim bảng mở rộng
* **Nhấp lại** để giải phóng và tự động ẩn khi rời chuột
* Mỗi giai đoạn cho thấy sự tiến bộ của từng cá nhân (0-100%)

***

## Tìm hiểu từng giai đoạn xử lý

### Giai đoạn 1: Phát hiện (Phát hiện mục tiêu)**Chuyện gì đang xảy ra vậy:**

* Chloros quét hình ảnh được đánh dấu bằng hộp kiểm Target
* Thuật toán thị giác máy tính xác định 4 bảng hiệu chuẩn
* Giá trị phản xạ được trích xuất từ ​​​​mỗi bảng
* Dấu thời gian mục tiêu được ghi lại để lập lịch hiệu chuẩn phù hợp

**Khoảng thời gian:**

* Với mục tiêu được đánh dấu: 10-60 giây
* Không có mục tiêu được đánh dấu: 5-30+ phút (quét tất cả hình ảnh)

**Chỉ báo tiến độ:**

* Phát hiện: 0% → 100%
* Số lượng hình ảnh được quét
* Số mục tiêu được tìm thấy

**Những gì cần xem:**

* Nên hoàn thành nhanh nếu mục tiêu được đánh dấu đúng
* Nếu mất quá nhiều thời gian, mục tiêu có thể không được đánh dấu
* Kiểm tra Nhật ký gỡ lỗi để biết thông báo "Đã tìm thấy mục tiêu"

### Giai đoạn 2: Phân tích

**Chuyện gì đang xảy ra vậy:**

* Đọc siêu dữ liệu EXIF ​​​​của hình ảnh (dấu thời gian, cài đặt phơi sáng)
* Xác định chiến lược hiệu chỉnh dựa trên dấu thời gian mục tiêu
* Tổ chức hàng đợi xử lý ảnh
* Chuẩn bị công nhân xử lý song song (chỉ Cloros+)

**Thời lượng:**5-30 giây**Chỉ báo tiến độ:**

* Phân tích: 0% → 100%
* Giai đoạn nhanh, thường kết thúc nhanh

**Những gì cần xem:**

* Nên tiến triển đều đặn không ngừng nghỉ
* Cảnh báo về siêu dữ liệu bị thiếu sẽ xuất hiện trong Nhật ký gỡ lỗi

### Giai đoạn 3: Hiệu chỉnh

**Chuyện gì đang xảy ra vậy:*** ** Debayering**: Chuyển đổi mẫu RAW Bayer thành 3 kênh
* **Chỉnh họa tiết**: Loại bỏ vết tối ở cạnh ống kính
* **Hiệu chỉnh phản xạ**: Chuẩn hóa với giá trị mục tiêu
* **Tính toán chỉ số**: Tính toán các chỉ số đa phổ
* Xử lý từng hình ảnh thông qua đường ống đầy đủ

**Thời lượng:**Phần lớn thời gian xử lý (60-80%)** Chỉ báo tiến độ:**

* Hiệu chuẩn: 0% → 100%
* Hình ảnh hiện tại đang được xử lý
* Hình ảnh hoàn thành / Tổng số hình ảnh

**Hành vi xử lý:*** ** Chế độ miễn phí**: Xử lý tuần tự từng hình ảnh
* **Chế độ Cloros+**: Xử lý đồng thời tối đa 16 hình ảnh
* **Tăng tốc GPU**: Tăng tốc đáng kể giai đoạn này**Những gì cần xem:**

* Tiến bộ ổn định thông qua số lượng hình ảnh
* Kiểm tra Nhật ký gỡ lỗi để biết thông báo hoàn thành trên mỗi hình ảnh
* Cảnh báo về vấn đề chất lượng hình ảnh hoặc hiệu chuẩn

### Giai đoạn 4: Xuất khẩu

**Chuyện gì đang xảy ra vậy:**

* Ghi hình ảnh đã hiệu chỉnh vào đĩa ở định dạng đã chọn
* Xuất hình ảnh chỉ mục đa phổ với màu LUT
* Tạo thư mục con mô hình máy ảnh
* Giữ nguyên tên tập tin gốc với hậu tố thích hợp

**Thời lượng:**10-20% tổng thời gian xử lý**Chỉ báo tiến độ:**

* Xuất: 0% → 100%
* Tập tin đang được ghi
* Định dạng xuất và đích

**Những gì cần xem:**

* Cảnh báo dung lượng ổ đĩa
* Lỗi ghi tập tin
* Hoàn thành tất cả các đầu ra được cấu hình

***

## Tab nhật ký gỡ lỗi

Nhật ký gỡ lỗi cung cấp thông tin chi tiết về tiến trình xử lý và mọi vấn đề gặp phải.

### Truy cập nhật ký gỡ lỗi

1. Nhấp vào biểu tượng**Nhật ký gỡ lỗi**<img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> ở thanh bên trái
2. Bảng nhật ký mở ra hiển thị các thông báo xử lý theo thời gian thực
3. Tự động cuộn để hiển thị tin nhắn mới nhất

### Hiểu thông điệp tường trình

#### Tin nhắn thông tin (Trắng/Xám)

Cập nhật xử lý thông thường:

```
[INFO] Đã bắt đầu xử lý
[INFO] Đã phát hiện mục tiêu trong IMG_0015.RAW - tìm thấy 4 bảng
[INFO] Đang hiệu chỉnh IMG_0234.RAW
[INFO] Hình ảnh NDVI đã xuất: IMG_0234_NDVI.tif
[THÔNG TIN] Quá trình xử lý hoàn tất
```

#### Thông báo cảnh báo (Vàng)

Các sự cố không nghiêm trọng không ngừng xử lý:

```
[CẢNH BÁO] Không tìm thấy dữ liệu GPS trong IMG_0145.RAW
[CẢNH BÁO] Khoảng cách dấu thời gian của hình ảnh mục tiêu > 30 phút
[CẢNH BÁO] Độ tương phản thấp trong bảng hiệu chuẩn - kết quả có thể thay đổi
```**Hành động:** Xem lại cảnh báo sau khi xử lý nhưng không làm gián đoạn

#### Thông Báo Lỗi (Đỏ)

Các vấn đề nghiêm trọng có thể khiến quá trình xử lý không thành công:

```
[ERROR] Không thể ghi tập tin - đĩa đầy
[ERROR] Tệp hình ảnh bị hỏng: IMG_0299.RAW
[LỖI] Không phát hiện thấy mục tiêu nào - bật hiệu chỉnh độ phản xạ hoặc đánh dấu hình ảnh mục tiêu
```**Hành động:** Dừng xử lý, giải quyết lỗi, khởi động lại

### Thông báo nhật ký chung

| Tin nhắn | Ý nghĩa | Cần hành động |
| -------------------------------- | -------------------------------------- | ----------------------------------------------------- |
| "Đã phát hiện mục tiêu trong \[tên tệp]" | Đã tìm thấy mục tiêu hiệu chỉnh thành công | Không - bình thường |
| "Xử lý ảnh X của Y" | Cập nhật tiến độ hiện tại | Không - bình thường |
| "Không tìm thấy mục tiêu" | Không phát hiện thấy mục tiêu hiệu chuẩn | Đánh dấu hình ảnh mục tiêu hoặc tắt hiệu chỉnh độ phản xạ |
| "Không đủ dung lượng đĩa" | Không đủ dung lượng lưu trữ cho đầu ra | Giải phóng dung lượng ổ đĩa |
| "Bỏ qua tập tin bị hỏng" | File ảnh bị hỏng | Sao chép lại tập tin từ thẻ SD |
| "Dữ liệu PPK được áp dụng" | Đã áp dụng chỉnh sửa GPS từ tệp .daq | Không - bình thường |

### Sao chép dữ liệu nhật ký

Để sao chép nhật ký nhằm khắc phục sự cố hoặc hỗ trợ:

1. Mở bảng Nhật ký gỡ lỗi
2. Nhấp vào nút**"Sao chép nhật ký"**(hoặc nhấp chuột phải → Chọn tất cả)
3. Dán vào file văn bản hoặc email
4. Gửi tới bộ phận hỗ trợ MAPIR nếu cần***

## Giám sát tài nguyên hệ thống

### Cách sử dụng CPU
**Chế độ miễn phí:**

* 1 lõi CPU ở mức \~100%
* Các lõi khác nhàn rỗi hoặc có sẵn
* Hệ thống vẫn đáp ứng

**Chloros+ Chế độ song song:**

* Nhiều lõi ở mức 80-100% (tối đa 16 lõi)
* Mức sử dụng CPU tổng thể cao
* Hệ thống có thể cảm thấy kém phản hồi hơn

**Để theo dõi:**

* Trình quản lý tác vụ Windows (Ctrl+Shift+Esc)
* Tab hiệu suất → phần CPU
* Tìm kiếm các quy trình "Chloros" hoặc "chloros-backend"

### Cách sử dụng bộ nhớ (RAM)

**Cách sử dụng điển hình:**

* Dự án nhỏ (< 100 hình ảnh): 2-4 GB
* Dự án trung bình (100-500 hình ảnh): 4-8 GB
* Dự án lớn (500+ hình ảnh): 8-16 GB
* Chế độ song song Chloros+ sử dụng nhiều RAM hơn

**Nếu bộ nhớ sắp hết:**

* Xử lý các lô nhỏ hơn
* Đóng các ứng dụng khác
* Nâng cấp RAM nếu thường xuyên xử lý tập dữ liệu lớn

### Cách sử dụng GPU (Chloros+ với CUDA)

Khi tính năng tăng tốc GPU được bật:

* GPU NVIDIA cho thấy mức sử dụng cao (60-90%)
* Mức sử dụng VRAM tăng lên (yêu cầu 4GB+ VRAM)
* Giai đoạn hiệu chỉnh nhanh hơn đáng kể

**Để theo dõi:**

* Biểu tượng Khay hệ thống NVIDIA
* Trình quản lý tác vụ → Hiệu suất → GPU
* GPU-Z hoặc công cụ giám sát tương tự

### Đĩa I/O

**Điều gì sẽ xảy ra:**

* Tốc độ đọc đĩa cao trong giai đoạn Phân tích
* Ghi đĩa cao trong giai đoạn Xuất
* SSD nhanh hơn đáng kể so với HDD

**Mẹo về hiệu suất:**

* Sử dụng SSD cho thư mục dự án khi có thể
* Tránh các ổ đĩa mạng cho các tập dữ liệu lớn
* Đảm bảo đĩa không quá dung lượng (ảnh hưởng đến tốc độ ghi)

***

## Phát hiện sự cố trong quá trình xử lý

### Dấu hiệu cảnh báo**Tiến trình bị đình trệ (không thay đổi trong hơn 5 phút):**

* Kiểm tra nhật ký gỡ lỗi để tìm lỗi
* Xác minh dung lượng đĩa trống
* Kiểm tra Trình quản lý tác vụ để đảm bảo Chloros đang chạy

**Thông báo lỗi xuất hiện thường xuyên:**

* Dừng xử lý và xem xét lỗi
* Nguyên nhân thường gặp: dung lượng ổ đĩa, file bị hỏng, vấn đề về bộ nhớ
* Xem phần Khắc phục sự cố bên dưới

**Hệ thống không phản hồi:**

* Chế độ song song Chloros+ sử dụng quá nhiều tài nguyên
* Cân nhắc giảm bớt các tác vụ đồng thời hoặc nâng cấp phần cứng
* Chế độ miễn phí ít tốn tài nguyên hơn

### Khi nào nên ngừng xử lý

Dừng xử lý nếu bạn thấy:

* ❌ Lỗi "Đĩa đầy" hoặc "Không thể ghi tệp"
* ❌ Lỗi hỏng file ảnh lặp đi lặp lại
* ❌ Hệ thống bị đóng băng hoàn toàn (không phản hồi)
* ❌ Nhận ra cài đặt sai đã được định cấu hình
* ❌ Nhập sai hình ảnh

**Cách dừng:**1. Nhấp vào** Nút Dừng/Hủy**(thay thế nút Bắt đầu)
2. Quá trình xử lý bị dừng, tiến trình bị mất
3. Khắc phục sự cố và khởi động lại từ đầu***

## Khắc phục sự cố trong quá trình xử lý

### Quá trình xử lý rất chậm
**Nguyên nhân có thể:**

* Hình ảnh mục tiêu không được đánh dấu (quét tất cả hình ảnh)
* HDD thay vì lưu trữ SSD
* Tài nguyên hệ thống không đủ
* Nhiều chỉ số được cấu hình
* Truy cập ổ đĩa mạng

**Giải pháp:**

1. Nếu mới bắt đầu và đang trong giai đoạn Phát hiện: Hủy, đánh dấu mục tiêu, khởi động lại
2. Tương lai: Sử dụng SSD, giảm chỉ số, nâng cấp phần cứng
3. Cân nhắc CLI để xử lý hàng loạt tập dữ liệu lớn

### Cảnh báo "Dung lượng ổ đĩa"**Giải pháp:**

1. Giải phóng dung lượng ổ đĩa ngay lập tức
2. Di chuyển dự án sang ổ đĩa có nhiều không gian hơn
3. Giảm số lượng chỉ số cần xuất
4. Sử dụng định dạng JPG thay vì TIFF (tệp nhỏ hơn)

### Thông báo "Tệp bị hỏng" thường xuyên**Giải pháp:**

1. Sao chép lại hình ảnh từ thẻ SD để đảm bảo tính toàn vẹn
2. Kiểm tra lỗi thẻ SD
3. Xóa các tập tin bị hỏng khỏi dự án
4. Tiếp tục xử lý các ảnh còn lại

### Hệ thống quá nóng / Giảm ga**Giải pháp:**

1. Đảm bảo thông gió đầy đủ
2. Làm sạch bụi ở lỗ thông hơi máy tính
3. Giảm tải xử lý (sử dụng chế độ Free thay vì Chloros+)
4. Xử lý vào thời điểm mát mẻ trong ngày***

## Đang xử lý thông báo hoàn chỉnh

Khi quá trình xử lý kết thúc:

* Thanh tiến độ đạt 100%
* **Thông báo "Đang xử lý hoàn tất"** xuất hiện trong Nhật ký gỡ lỗi
* Nút bắt đầu được bật lại
* Tất cả các tập tin đầu ra đều nằm trong thư mục con của mẫu máy ảnh

***

## Các bước tiếp theo

Sau khi quá trình xử lý hoàn tất:

1.**Xem lại kết quả**- Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)
2.**Kiểm tra thư mục đầu ra**- Xác minh tất cả các tệp được xuất chính xác
3.**Xem lại nhật ký gỡ lỗi**- Kiểm tra mọi cảnh báo hoặc lỗi
4.**Xem trước hình ảnh đã xử lý** - Sử dụng Image Viewer hoặc phần mềm bên ngoài

Để biết thông tin về việc xem xét và sử dụng kết quả đã xử lý của bạn, hãy xem [Hoàn tất quá trình xử lý](finishing-the-processing.md).
