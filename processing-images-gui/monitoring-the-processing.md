# Theo dõi quá trình xử lý

Khi quá trình xử lý đã bắt đầu,Chloros

cung cấp một số cách để theo dõi tiến độ, kiểm tra các sự cố và hiểu rõ những gì đang diễn ra với tập dữ liệu của bạn. Trang này giải thích cách theo dõi quá trình xử lý và giải thích thông tin màChloros

cung cấp.

## Tổng quan về thanh tiến trình

Thanh tiến trình ở phần đầu trang hiển thị trạng thái xử lý theo thời gian thực và tỷ lệ phần trăm hoàn thành. Tiến độ được truyền trực tiếp từ hệ thống backend thông qua Server-Sent Events (SSE), do đó thanh tiến độ phản ánh chính xác những gì quy trình xử lý đang thực hiện.

### Thanh tiến độ ở Chế độ Miễn phí

Đối với người dùng không có giấy phépChloros

+:

**Hiển thị tiến độ 2 giai đoạn:**

1.**Phát hiện mục tiêu** - Tìm các mục tiêu hiệu chuẩn trong hình ảnh
2. **Xử lý** - Áp dụng các chỉnh sửa và xuất dữ liệu**Thanh tiến trình hiển thị:**

* Tỷ lệ hoàn thành tổng thể (0-100%)
* Tên giai đoạn hiện tại
* Biểu diễn đơn giản dưới dạng thanh ngang

### Thanh tiến trìnhChloros



Dành cho người dùng có giấy phépChloros

+:

**Hiển thị tiến trình 4 giai đoạn:**

1.**Phát hiện** - Tìm các mục tiêu hiệu chuẩn
2. **Phân tích** - Kiểm tra hình ảnh và chuẩn bị quy trình xử lý
3. **Hiệu chuẩn** - Áp dụng các điều chỉnh hiệu ứng viền tối và độ phản xạ
4. **Xuất** - Lưu các tệp đã xử lý**Các tính năng tương tác:*** **Di chuột qua** thanh tiến trình để xem bảng 4 giai đoạn mở rộng
* **Nhấp vào** thanh tiến trình để đóng băng/ghim bảng mở rộng
* **Nhấp lại** để bỏ đóng băng và ẩn tự động khi di chuột ra khỏi vùng
* Mỗi giai đoạn hiển thị tiến trình riêng biệt (0-100%)

{% hint style="info" %}
**Tính nhất quán củaCLI**: trong quá trình chạy `chloros-cli process`, bốn luồng (thread) này đều báo trạng thái đang “Phát hiện”, “Phân tích”, “Xử lý” và “Xuất”, trong khi `chloros-cli export-status` hiển thị tiến trình xuất của Luồng 4 theo thời gian thực từ một thiết bị đầu cuối khác. Xem [Tài liệu tham khảoCLI

](../reference/cli-reference.md).
{% endhint %}

***

## Hiểu về từng giai đoạn xử lý

{% hint style="info" %}
**Kiến trúc đường ống**: 4 giai đoạn trên giao diện người dùng (GUI) này tương ứng với [đường ống xử lý 4 luồng](../processing-architecture/processing-pipeline.md). Trên các hệ thống có khả năng tăng tốc GPU, luồng 3 (Hiệu chuẩn) được hưởng lợi từ [Tính năng điều chỉnh tính toán động](../processing-architecture/dynamic-compute-adaptation.md), giúp tối ưu hóa quá trình xử lý cho phần cứng cụ thể của bạn.
{% endhint %}

### Giai đoạn 1: Phát hiện (Phát hiện mục tiêu)

**Quá trình diễn ra như sau:**

* Chương trình quét hình ảnh (Chloros

) quét các hình ảnh mà bạn đã chọn bằng hộp kiểm &quot;Target&quot; (tất cả hình ảnh nếu không có hình ảnh nào được chọn)
* Các thuật toán thị giác máy tính xác định các bảng hiệu chuẩn
* Giá trị phản xạ được trích xuất từ mỗi bảng
* Dấu thời gian của mục tiêu được ghi lại để lập lịch hiệu chuẩn chính xác

**Thời gian thực hiện:**

* Có mục tiêu được đánh dấu: 10–60 giây
* Không có mục tiêu được đánh dấu: 5–30+ phút (quét tất cả hình ảnh)

**Chỉ báo tiến trình:**

* Đang phát hiện: 0% → 100%
* Số lượng hình ảnh đã quét (chỉ tính các hình ảnh thực sự đang được quét)
* Số lượng mục tiêu đã tìm thấy

**Cần lưu ý:**

* Quá trình sẽ hoàn tất nhanh chóng nếu các mục tiêu được đánh dấu đúng cách
* Nếu mất quá nhiều thời gian, có thể các mục tiêu chưa được đánh dấu
* Kiểm tra Nhật ký gỡ lỗi để tìm các thông báo &quot;Đã tìm thấy mục tiêu&quot;

### Giai đoạn 2: Phân tích

**Điều gì đang xảy ra:**

* Đọc siêu dữ liệu EXIF của hình ảnh (dấu thời gian, cài đặt phơi sáng)
* Xác định chiến lược hiệu chuẩn dựa trên dấu thời gian của mục tiêu và dữ liệu DAQ hướng xuống có sẵn
* Sắp xếp hàng đợi xử lý hình ảnh
* Chuẩn bị các công cụ xử lý song song (chỉ dành choChloros

trở lên)

**Thời gian thực hiện:** 5–30 giây**Chỉ báo tiến trình:**

* Đang phân tích: 0% → 100%
* Giai đoạn diễn ra nhanh, thường hoàn thành sớm

**Cần lưu ý:**

* Quá trình nên diễn ra đều đặn mà không bị gián đoạn
* Các cảnh báo về metadata thiếu sẽ xuất hiện trong Nhật ký gỡ lỗi

### Giai đoạn 3: Hiệu chuẩn

**Điều gì đang diễn ra:*** **Debayering**: Chuyển đổi mẫu Bayer RAW thành 3 kênh (bị bỏ qua đối với các mô-đun LATTICE mono, kèm chú thích)
* **Chỉnh sửa hiệu ứng tối góc**: Loại bỏ hiện tượng tối ở viền ống kính
* **Hiệu chuẩn độ phản xạ**: Chuẩn hóa với các giá trị mục tiêu và/hoặc tín hiệu xuống từ hệ thống thu thập dữ liệu (DAQ)
* **Tính toán chỉ số**: Tính toán các chỉ số đa phổ
* Xử lý từng hình ảnh qua toàn bộ quy trình

**Thời gian:** Chiếm phần lớn tổng thời gian xử lý (60–80%)**Chỉ báo tiến trình:**

* Đang hiệu chuẩn: 0% → 100%
* Đang xử lý hình ảnh hiện tại
* Số hình ảnh đã hoàn thành / Tổng số hình ảnh

**Hành vi xử lý:*** **Chế độ Free**: Xử lý từng hình ảnh một theo thứ tự
* **Chế độChloros

+**: Chạy nhóm công nhân thích ứng với phần cứng — 1–4 công nhân đồng thời trên hệ thống GPU (tùy theo VRAM), một công nhân cho mỗi lõi vật lý (trừ đi một) trên hệ thống chỉ dùng CPU. Xem [Dynamic Compute Adaptation](../processing-architecture/dynamic-compute-adaptation.md)
* **Tăng tốc bằng GPU**: Tăng tốc đáng kể giai đoạn này**Cần lưu ý:**

* Tiến trình ổn định theo số lượng hình ảnh
* Kiểm tra Nhật ký gỡ lỗi để xem thông báo hoàn thành từng hình ảnh
* Cảnh báo về chất lượng hình ảnh hoặc các vấn đề hiệu chuẩn

### Giai đoạn 4: Xuất dữ liệu

**Quá trình diễn ra:**

* Ghi các hình ảnh đã xử lý lên đĩa theo định dạng đã chọn ngay khi chúng hoàn thành
* **LATTICE**: mỗi khung hình được phân tán ra tất cả các sản phẩm đã kích hoạt (debayered / preview / radiance / reflectance)
* Xuất các hình ảnh chỉ số đa quang phổ với màu LUT
* Tạo cây thư mục đầu ra `<project>/<camera>/<format>/<Product>_Images/` — các tệp đã xuất giữ nguyên tên tệp nguồn; thư mục xác định sản phẩm

**Thời gian thực hiện:** 10–20% tổng thời gian xử lý**Chỉ báo tiến trình:**

* Đang xuất: 0% → 100%
* Đang ghi tệp
* Định dạng xuất và đích đến

**Cần lưu ý:**

* Cảnh báo dung lượng đĩa
* Lỗi ghi tệp
* Hoàn tất tất cả các đầu ra đã cấu hình

***

## Tab Nhật ký gỡ lỗi

Nhật ký gỡ lỗi cung cấp thông tin chi tiết về tiến trình xử lý và bất kỳ sự cố nào gặp phải. Các thông báo khởi động của hệ thống nền cũng được ghi lại vào bảng điều khiển nhật ký, do đó nhật ký sẽ phản ánh toàn bộ quá trình ngay cả khi bạn mở nó muộn.

### Truy cập Nhật ký gỡ lỗi

1. Nhấp vào biểu tượng **Nhật ký gỡ lỗi**<img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line">

ở thanh bên trái
2. Bảng nhật ký sẽ mở ra, hiển thị các thông báo xử lý theo thời gian thực
3. Tự động cuộn để hiển thị các thông báo mới nhất

<!-- SCREENSHOT-NEEDED: Debug Log tab open at the end of a completed run, showing real backend log lines including the [RUN-SUMMARY] lines (images / camera groups / targets / calibrated / files written) -->

### Hiểu các thông báo nhật ký Các dòng nhật ký

Chloros

được đánh dấu bằng các thẻ trong ngoặc vuông chỉ tên hệ thống con — ví dụ: `[PROCESSING]`, `[RUN-SUMMARY]`, `[LATTICE-EXPORT]`, `[EXPORT-CHECK]`, `[IMPORT-LEVEL]`. Thông tin quan trọng nhất cần biết là **tóm tắt quá trình chạy**, được hiển thị ở cuối mỗi lần chạy (bao gồm cả các lần chạy đã dừng):

```
[RUN-SUMMARY] 49 image(s) in 2 camera group(s); 4 target(s) detected; 45 image(s) calibrated; 180 file(s) written.
```

Các dòng gợi ý bổ sung `[RUN-SUMMARY]` sẽ xuất hiện bất cứ khi nào cần giải thích điều gì đó — ví dụ như một lần chạy không tạo ra kết quả nào, hoặc một camera mà sản phẩm được yêu cầu đã bị bỏ qua vì không áp dụng được. Các dòng `[EXPORT-CHECK]` giải thích các trường hợp bỏ qua theo từng camera (ví dụ: lý do tại sao cameraRGB

không nhận được sản phẩm radiance).

Các mức độ nghiêm trọng chung của thông báo (các ví dụ dưới đây chỉ mang tính minh họa, không phải nguyên văn):

#### Thông báo Thông tin (Màu trắng/Xám)

Cập nhật xử lý thông thường: quá trình xử lý đã bắt đầu, phát hiện mục tiêu (kèm số lượng ô), tiến trình hiệu chuẩn từng hình ảnh, tệp đã được xuất, quá trình xử lý hoàn tất.

#### Thông báo Cảnh báo (Màu vàng)

Các vấn đề không nghiêm trọng không làm dừng quá trình xử lý — ví dụ: thiếu dữ liệu GPS trong một khung hình, khoảng cách thời gian lớn giữa các hình ảnh mục tiêu, hoặc độ tương phản thấp trong bảng hiệu chuẩn.

**Hành động:** Kiểm tra các cảnh báo sau khi xử lý, nhưng không ngắt quá trình

#### Thông báo lỗi (Red

)

Các vấn đề nghiêm trọng có thể khiến quá trình xử lý thất bại — ví dụ: đĩa đầy, tệp hình ảnh bị hỏng, hoặc không phát hiện được mục tiêu trong khi yêu cầu hiệu chuẩn phản xạ.

**Hành động:** Dừng xử lý, khắc phục lỗi, khởi động lại

### Các tình huống thường gặp trong nhật ký

| Tình huống                             | Ý nghĩa                                       | Hành động cần thực hiện                                         |
| ------------------------------------- | --------------------------------------------- | ----------------------------------------------------- |
| Phát hiện mục tiêu trong \[tên tệp]        | Đã tìm thấy mục tiêu hiệu chuẩn thành công         | Không có - bình thường                                         |
| Dòng tiến trình cho từng hình ảnh              | Cập nhật tiến trình hiện tại                       | Không có - bình thường                                         |
| Không tìm thấy mục tiêu nào                      | Không phát hiện mục tiêu hiệu chuẩn               | Đánh dấu hình ảnh mục tiêu hoặc tắt hiệu chuẩn phản xạ |
| Dung lượng đĩa không đủ               | Không đủ dung lượng lưu trữ cho kết quả                 | Giải phóng dung lượng đĩa                                    |
| Bỏ qua tệp bị hỏng               | Tệp hình ảnh bị hỏng                         | Sao chép lại tệp từ thẻ SD                             |
| `[IMPORT-LEVEL] Skipping ... no raw source` | Không thể xử lý dữ liệu chụp không có khung hình thô | Chụp lại với khung hình thô hoặc sử dụngCLI

`--input-level`  |
| `[RUN-SUMMARY] ... 0 file(s) written` | Quá trình chạy không tạo ra sản phẩm hình ảnh — được báo cáo là lỗi kèm gợi ý | Đọc các dòng gợi ý; kiểm tra những gì đã bị bỏ qua và lý do |

### Sao chép dữ liệu nhật ký

Để sao chép nhật ký nhằm khắc phục sự cố hoặc hỗ trợ:

1. Mở bảng Nhật ký gỡ lỗi
2. Nhấp vào nút **&quot;Sao chép nhật ký&quot;** (hoặc nhấp chuột phải → Chọn tất cả)
3. Dán vào tệp văn bản hoặc email
4. Gửi đến bộ phận hỗ trợMAPIR

nếu cần

***

## Giám sát tài nguyên hệ thống

### Tỷ lệ sử dụng CPU

**Chế độ Tự do:**

* 1 nhân CPU ở mức ~100%
* Các nhân còn lại ở trạng thái nhàn rỗi hoặc sẵn sàng
* Hệ thống vẫn phản hồi bình thường

**Chế độ song songChloros

+:**

* Nhiều nhân hoạt động ở mức sử dụng cao — số lượng cụ thể phụ thuộc vào chiến lược được chọn bởi [Dynamic Compute Adaptation](../processing-architecture/dynamic-compute-adaptation.md)
* Hệ thống có thể cảm thấy chậm hơn

**Để theo dõi:**

* Trình quản lý tác vụWindows

(Ctrl+Shift+Esc)
* Tab Hiệu suất → Phần CPU
* Tìm các tiến trình “Chloros

” hoặc “chloros-backend”

### Mức sử dụng bộ nhớ (RAM)

**Mức sử dụng thông thường:**

* Dự án nhỏ (&lt; 100 hình ảnh): 2–4 GB
* Dự án trung bình (100–500 hình ảnh): 4–8 GB
* Dự án lớn (500+ hình ảnh): 8–16 GB
* Chế độ song songChloros

+ tiêu tốn nhiều RAM hơn

**Nếu bộ nhớ thấp:**

* Xử lý các lô nhỏ hơn
* Đóng các ứng dụng khác
* Nâng cấp RAM nếu thường xuyên xử lý các tập dữ liệu lớn

### Sử dụng GPU (từChloros

trở lên với CUDA)

Khi bật tính năng tăng tốc GPU:

* GPU NVIDIA hiển thị mức sử dụng cao (60–90%)
* Sử dụng VRAM tăng lên (yêu cầu 4GB VRAM trở lên; 7GB trở lên cho quá trình debayering Texture Aware đồng thời)
* Giai đoạn hiệu chuẩn diễn ra nhanh hơn đáng kể

**Để theo dõi:**

* Biểu tượng NVIDIA trên khay hệ thống
* Trình quản lý tác vụ → Hiệu suất → GPU
* GPU-Z hoặc công cụ theo dõi tương tự

### I/O đĩa

**Điều cần lưu ý:**

* Tốc độ đọc đĩa cao trong giai đoạn Phân tích
* Tốc độ ghi đĩa cao trong giai đoạn Xuất
* SSD nhanh hơn đáng kể so với HDD

**Mẹo tối ưu hiệu suất:**

* Sử dụng SSD cho thư mục dự án khi có thể
* Tránh sử dụng ổ đĩa mạng cho tập dữ liệu lớn
* Đảm bảo ổ đĩa không gần đầy (ảnh hưởng đến tốc độ ghi)

***

## Phát hiện sự cố trong quá trình xử lý

### Dấu hiệu cảnh báo

**Tiến trình bị đình trệ (không thay đổi trong hơn 5 phút):**

* Kiểm tra Nhật ký gỡ lỗi để tìm lỗi
* Kiểm tra dung lượng ổ đĩa còn trống
* Kiểm tra Trình quản lý tác vụ (Task Manager) để đảm bảo quá trình &quot;Chloros

&quot; đang chạy

**Thông báo lỗi xuất hiện thường xuyên:**

* Dừng xử lý và kiểm tra các lỗi
* Nguyên nhân phổ biến: dung lượng đĩa, tệp bị hỏng, vấn đề về bộ nhớ
* Xem phần Khắc phục sự cố bên dưới

**Hệ thống không phản hồi:**

* Chế độ song song &quot;Chloros

+&quot; đang sử dụng quá nhiều tài nguyên
* Cân nhắc giảm số tác vụ đồng thời hoặc nâng cấp phần cứng
* Chế độ miễn phí (Free mode) tiêu tốn ít tài nguyên hơn

### Khi nào nên dừng xử lý

Dừng xử lý nếu bạn thấy:

* ❌ Lỗi &quot;Đĩa đầy&quot; hoặc &quot;Không thể ghi tệp&quot;
* ❌ Lỗi tệp hình ảnh bị hỏng lặp đi lặp lại
* ❌ Hệ thống bị đóng băng hoàn toàn (không phản hồi)
* ❌ Nhận ra đã cấu hình sai các thiết lập
* ❌ Nhập nhầm hình ảnh

**Cách dừng:**

1. Nhấp vào**nút Dừng** (thay thế cho nút Bắt đầu) — chỉ cần nhấp một lần là đủ
2. Thanh tiến trình hiển thị &quot;Đang dừng...&quot; trong khi hình ảnh đang xử lý hoàn tất, sau đó quá trình kết thúc ở trạng thái đã dừng
3. Các sản phẩm đã xuất vẫn còn trên đĩa; nhật ký ghi lại thông tin chi tiết về những gì đã hoàn thành với mã lỗi `[RUN-SUMMARY]`
4. Khắc phục sự cố và khởi động lại — quá trình sẽ bắt đầu lại từ đầu

***

## Khắc phục sự cố trong quá trình xử lý

### Quá trình xử lý rất chậm

**Nguyên nhân có thể:**

* Hình ảnh mục tiêu chưa được đánh dấu (quét tất cả hình ảnh)
* Sử dụng ổ cứng HDD thay vì ổ SSD
* Tài nguyên hệ thống không đủ
* Cấu hình quá nhiều chỉ mục
* Truy cập ổ đĩa mạng

**Giải pháp:**

1. Nếu mới bắt đầu và đang ở giai đoạn Phát hiện: Dừng lại, đánh dấu mục tiêu, khởi động lại
2. Đối với lần sau: Sử dụng SSD, giảm số lượng chỉ mục, nâng cấp phần cứng
3. Xem xét sử dụngCLI

để xử lý hàng loạt tập dữ liệu lớn

### Cảnh báo &quot;Dung lượng đĩa&quot;

**Giải pháp:**

1. Giải phóng dung lượng đĩa ngay lập tức
2. Chuyển dự án sang ổ đĩa có dung lượng lớn hơn
3. Giảm số lượng chỉ mục cần xuất
4. Tắt các sản phẩm xuất LATTICE không cần thiết (Cài đặt Dự án → Xử lý)
5. Sử dụng định dạng JPG thay vì &quot;TIFF

&quot; (tệp nhỏ hơn)

### Thông báo &quot;Tệp bị hỏng&quot; xuất hiện thường xuyên

**Giải pháp:**

1. Sao chép lại hình ảnh từ thẻ SD để đảm bảo tính toàn vẹn
2. Kiểm tra lỗi trên thẻ SD
3. Loại bỏ các tệp bị hỏng khỏi dự án
4. Tiếp tục xử lý các hình ảnh còn lại

### Hệ thống quá nhiệt / Giảm hiệu suất

**Giải pháp:**

1. Đảm bảo thông gió đủ
2. Làm sạch bụi khỏi các khe thông gió của máy tính
3. Giảm tải xử lý (sử dụng chế độ &quot;Free&quot; thay vì &quot;Chloros

&quot;)
4. Xử lý vào những thời điểm mát mẻ trong ngày

***

## Thông báo hoàn tất xử lý

Khi quá trình xử lý kết thúc:

* Thanh tiến trình đạt 100%
* Các dòng `[RUN-SUMMARY]` xuất hiện trong Nhật ký gỡ lỗi kèm theo số liệu cuối cùng
* Nút Bắt đầu lại có thể sử dụng được
* Tất cả tệp đầu ra đều nằm trong thư mục đầu ra theo từng camera của dự án: `<project>/<camera>/<format>/<Product>_Images/`

***

## Các bước tiếp theo

Sau khi quá trình xử lý hoàn tất:

1. **Kiểm tra kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)
2. **Kiểm tra thư mục đầu ra** - Xác minh tất cả các tệp đã được xuất chính xác
3. **Xem lại nhật ký gỡ lỗi** - Kiểm tra xem có cảnh báo hoặc lỗi nào không
4. **Xem trước hình ảnh đã xử lý** - Sử dụng Trình xem hình ảnh hoặc phần mềm bên ngoài

Để biết thông tin về cách xem lại và sử dụng kết quả đã xử lý, hãy xem [Hoàn tất quá trình xử lý](finishing-the-processing.md).
