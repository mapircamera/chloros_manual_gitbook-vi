# Theo dõi quá trình xử lý

Khi quá trình xử lý đã bắt đầu, Chloros cung cấp nhiều cách để theo dõi tiến độ, kiểm tra các sự cố và nắm bắt tình hình của tập dữ liệu của bạn. Trang này giải thích cách theo dõi quá trình xử lý và giải thích thông tin mà Chloros cung cấp.

## Tổng quan về thanh tiến trình

Thanh tiến trình ở đầu trang hiển thị trạng thái xử lý theo thời gian thực và tỷ lệ hoàn thành. Tiến trình được truyền trực tiếp từ hệ thống nền thông qua Server-Sent Events (SSE), do đó thanh tiến trình phản ánh chính xác những gì quy trình xử lý đang thực hiện.

### Thanh tiến trình ở Chế độ Miễn phí

Dành cho người dùng không có giấy phép Chloros+:

**Hiển thị tiến trình 2 giai đoạn:**

1.**Phát hiện mục tiêu** - Tìm các mục tiêu hiệu chuẩn trong hình ảnh
2. **Xử lý** - Áp dụng các chỉnh sửa và xuất dữ liệu**Thanh tiến trình hiển thị:**

* Tỷ lệ hoàn thành tổng thể (0-100%)
* Tên giai đoạn hiện tại
* Hình ảnh thanh ngang đơn giản

### Thanh tiến trình Chloros+

Dành cho người dùng có giấy phép Chloros+:

**Hiển thị tiến trình 4 giai đoạn:**

1.**Phát hiện** - Tìm các mục tiêu hiệu chuẩn
2. **Phân tích** - Kiểm tra hình ảnh và chuẩn bị quy trình xử lý
3. **Hiệu chuẩn** - Áp dụng hiệu chỉnh hiệu ứng viền tối và độ phản xạ
4. **Xuất** - Lưu các tệp đã xử lý**Tính năng tương tác:*** **Di chuột qua** thanh tiến trình để xem bảng điều khiển 4 giai đoạn mở rộng
* **Nhấp** vào thanh tiến trình để đóng băng/ghim bảng điều khiển mở rộng
* **Nhấp lại** để bỏ khóa và ẩn tự động khi di chuột ra khỏi
* Mỗi giai đoạn hiển thị tiến trình riêng biệt (0-100%)

{% hint style="info" %}
**Tính tương thích với CLI**: trong quá trình chạy `chloros-cli process`, bốn luồng (thread) tương tự sẽ hiển thị trạng thái "Đang phát hiện", "Đang phân tích", "Đang xử lý", "Đang xuất", và `chloros-cli export-status` hiển thị tiến trình xuất của Luồng 4 theo thời gian thực từ một terminal khác. Xem [Tài liệu tham khảo CLI](../reference/cli-reference.md).
{% endhint %}

***

## Hiểu rõ từng giai đoạn xử lý

{% hint style="info" %}
**Kiến trúc đường ống**: 4 giai đoạn GUI này tương ứng với [đường ống xử lý 4 luồng](../processing-architecture/processing-pipeline.md). Trên các hệ thống có khả năng tăng tốc GPU, Luồng 3 (Hiệu chuẩn) được hưởng lợi từ [Tính năng điều chỉnh tính toán động](../processing-architecture/dynamic-compute-adaptation.md), giúp tối ưu hóa quá trình xử lý cho phần cứng cụ thể của bạn.
{% endhint %}

### Giai đoạn 1: Phát hiện (Phát hiện mục tiêu)

**Quá trình diễn ra như sau:**

* Chloros quét các hình ảnh mà bạn đã chọn bằng hộp kiểm “Mục tiêu” (tất cả hình ảnh nếu không có hình ảnh nào được chọn)
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
* Kiểm tra Nhật ký gỡ lỗi để tìm các thông báo "Đã tìm thấy mục tiêu"

### Giai đoạn 2: Phân tích

**Quá trình diễn ra:**

* Đọc siêu dữ liệu EXIF của hình ảnh (dấu thời gian, cài đặt phơi sáng)
* Xác định chiến lược hiệu chuẩn dựa trên dấu thời gian của mục tiêu và dữ liệu DAQ hướng xuống có sẵn
* Sắp xếp hàng đợi xử lý hình ảnh
* Chuẩn bị các công cụ xử lý song song (Chỉ áp dụng cho Chloros+)

**Thời gian thực hiện:** 5–30 giây**Chỉ báo tiến trình:**

* Đang phân tích: 0% → 100%
* Giai đoạn diễn ra nhanh, thường hoàn thành trong thời gian ngắn

**Cần lưu ý:**

* Quá trình nên diễn ra đều đặn mà không bị gián đoạn
* Các cảnh báo về metadata thiếu sẽ xuất hiện trong Nhật ký gỡ lỗi

### Giai đoạn 3: Hiệu chuẩn

**Quá trình diễn ra:*** **Debayering**: Chuyển đổi mẫu Bayer RAW thành 3 kênh (bước này bị bỏ qua đối với các mô-đun đơn sắc LATTICE, kèm chú thích)
* **Chỉnh sửa hiệu ứng tối góc**: Loại bỏ hiện tượng tối ở viền ống kính
* **Hiệu chuẩn độ phản xạ**: Chuẩn hóa theo các giá trị mục tiêu và/hoặc tín hiệu xuống từ hệ thống thu thập dữ liệu (DAQ)
* **Tính toán chỉ số**: Tính toán các chỉ số đa phổ
* Xử lý từng hình ảnh qua toàn bộ quy trình

**Thời gian thực hiện:** Chiếm phần lớn tổng thời gian xử lý (60–80%)**Chỉ báo tiến trình:**

* Đang hiệu chuẩn: 0% → 100%
* Đang xử lý hình ảnh hiện tại
* Số hình ảnh đã hoàn thành / Tổng số hình ảnh

**Hành vi xử lý:*** **Chế độ tự do**: Xử lý từng hình ảnh một theo thứ tự
* **Chế độ Chloros+**: Chạy nhóm công nhân thích ứng với phần cứng — 1–4 công nhân đồng thời trên hệ thống GPU (tùy thuộc vào VRAM), một công nhân cho mỗi lõi vật lý (trừ đi một) trên hệ thống chỉ dùng CPU. Xem [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md)
* **Tăng tốc GPU**: Tăng tốc đáng kể giai đoạn này**Cần lưu ý:**

* Tiến trình ổn định theo số lượng hình ảnh
* Kiểm tra Nhật ký gỡ lỗi để xem thông báo hoàn thành từng hình ảnh
* Các cảnh báo về chất lượng hình ảnh hoặc vấn đề hiệu chuẩn

### Giai đoạn 4: Xuất dữ liệu

**Quá trình diễn ra như thế nào:**

* Ghi các hình ảnh đã xử lý vào đĩa theo định dạng đã chọn ngay khi chúng hoàn thành
* **LATTICE**: mỗi khung hình được phân tán ra tất cả các sản phẩm đã kích hoạt (debayered / preview / radiance / reflectance)
* Xuất các hình ảnh chỉ số đa phổ với màu LUT
* Tạo cấu trúc thư mục đầu ra `<project>/<camera>/<format>/<Product>_Images/` — các tệp xuất giữ nguyên tên tệp nguồn; thư mục xác định sản phẩm

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

Nhật ký gỡ lỗi cung cấp thông tin chi tiết về tiến trình xử lý và bất kỳ sự cố nào gặp phải. Các thông báo khởi động của hệ thống nền cũng được ghi lại vào bảng điều khiển nhật ký, do đó nhật ký sẽ ghi lại toàn bộ quá trình ngay cả khi bạn mở nó muộn.

### Truy cập Nhật ký gỡ lỗi

1. Nhấp vào biểu tượng **Nhật ký gỡ lỗi** <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line">

ở thanh bên trái
2. Bảng nhật ký sẽ mở ra, hiển thị các thông báo xử lý theo thời gian thực
3. Tự động cuộn để hiển thị các thông báo mới nhất

<!-- SCREENSHOT-NEEDED: Debug Log tab open at the end of a completed run, showing real backend log lines including the [RUN-SUMMARY] lines (images / camera groups / targets / calibrated / files written), with the 1.3.0 bug icon (Report a bug) visible at the top of the tab -->

### Hiểu các thông báo nhật ký Các dòng nhật ký của

Chloros được đánh dấu bằng các thẻ trong ngoặc vuông chỉ tên hệ thống con — ví dụ: `[PROCESSING]`, `[RUN-SUMMARY]`, `[LATTICE-EXPORT]`, `[EXPORT-CHECK]`, `[IMPORT-LEVEL]`. Thông tin quan trọng nhất cần biết là **tóm tắt quá trình chạy**, được hiển thị ở cuối mỗi lần chạy (bao gồm cả các lần chạy đã dừng):

```
[RUN-SUMMARY] 49 image(s) in 2 camera group(s); 4 target(s) detected; 45 image(s) calibrated; 180 file(s) written.
```

Các dòng gợi ý bổ sung `[RUN-SUMMARY]` sẽ xuất hiện bất cứ khi nào cần giải thích điều gì đó — ví dụ như một lần chạy không tạo ra kết quả nào, hoặc một camera mà sản phẩm được yêu cầu đã bị bỏ qua vì không áp dụng được. Các dòng `[EXPORT-CHECK]` giải thích các trường hợp bỏ qua theo từng camera (ví dụ: lý do tại sao camera RGB không nhận được sản phẩm radiance).

Các mức độ nghiêm trọng chung của thông báo (các ví dụ dưới đây chỉ mang tính minh họa, không phải nguyên văn):

#### Thông báo Thông tin (Màu trắng/Xám)

Cập nhật xử lý thông thường: quá trình xử lý đã bắt đầu, phát hiện mục tiêu (kèm số lượng bảng hiệu chuẩn), tiến trình hiệu chuẩn từng hình ảnh, tệp đã được xuất, quá trình xử lý hoàn tất.

#### Thông báo Cảnh báo (Màu vàng)

Các vấn đề không nghiêm trọng không làm dừng quá trình xử lý — ví dụ: thiếu dữ liệu GPS trong một khung hình, khoảng cách thời gian lớn giữa các hình ảnh mục tiêu, hoặc độ tương phản thấp trong bảng hiệu chuẩn.

**Hành động:** Kiểm tra các cảnh báo sau khi xử lý, nhưng không ngắt quá trình

#### Thông báo lỗi (màu Red)

Các vấn đề nghiêm trọng có thể khiến quá trình xử lý thất bại — ví dụ: đĩa đầy, tệp hình ảnh bị hỏng hoặc không phát hiện được mục tiêu trong khi yêu cầu hiệu chuẩn phản xạ.

**Hành động:** Dừng xử lý, khắc phục lỗi, khởi động lại

### Các tình huống thường gặp trong nhật ký

| Tình huống                             | Ý nghĩa                                       | Hành động cần thực hiện                                         |
| ------------------------------------- | --------------------------------------------- | ----------------------------------------------------- |
| Phát hiện mục tiêu trong \[tên tệp\]        | Đã tìm thấy mục tiêu hiệu chuẩn thành công         | Không có - bình thường                                         |
| Thanh tiến trình cho từng hình ảnh              | Cập nhật tiến trình hiện tại                       | Không có - bình thường                                         |
| Không tìm thấy mục tiêu nào                      | Không phát hiện mục tiêu hiệu chuẩn               | Đánh dấu hình ảnh mục tiêu hoặc tắt hiệu chuẩn phản xạ |
| Không đủ dung lượng đĩa               | Không đủ dung lượng lưu trữ cho kết quả                 | Giải phóng dung lượng đĩa                                    |
| Bỏ qua tệp bị hỏng               | Tệp hình ảnh bị hỏng                         | Sao chép lại tệp từ thẻ SD                             |
| `[IMPORT-LEVEL] Skipping ... no raw source` | Không thể xử lý dữ liệu chụp không có khung hình thô | Chụp lại với định dạng thô, hoặc sử dụng CLI `--input-level`  |
| `[RUN-SUMMARY] ... 0 file(s) written` | Quá trình chạy không tạo ra sản phẩm hình ảnh — được báo cáo là lỗi kèm gợi ý | Đọc các dòng gợi ý; kiểm tra những gì đã bị bỏ qua và lý do |
| `[PROCESS] refused: The project folder is too deep for this computer…` | Tính năng đường dẫn dài của Windows bị tắt và thư mục dự án nằm quá sâu so với giới hạn đường dẫn 260 ký tự; không có dữ liệu nào được xử lý | Sử dụng thư mục dự án ngắn hơn (thông báo sẽ hiển thị đường dẫn dài nhất vẫn phù hợp), hoặc bật tính năng đường dẫn dài củWindows và khởi động lại Chloros — xem [Khởi động quá trình xử lý](starting-the-processing.md#the-project-folder-is-too-deep-for-this-computer) |
| `[unmix-clamp-mask] …` | Không thể ghi mặt nạ kẹp (clamp mask) của sản phẩm LATTICE; bản thân sản phẩm vẫn hoạt động bình thường và hiển thị thông báo `Chloros:UnmixClampMask=unavailable` | Kiểm tra dung lượng đĩa và quyền truy cập thư mục, sau đó xử lý lại nếu cần mặt nạ |

### Sao chép dữ liệu nhật ký

Để sao chép nhật ký nhằm khắc phục sự cố hoặc hỗ trợ kỹ thuật:

1. Mở bảng điều khiển Nhật ký gỡ lỗi
2. Nhấp vào nút **Sao chép**(sao chép nhật ký vào khay nhớ tạm) hoặc**Tải xuống** để lưu dưới dạng tệp văn bản
3. Dán vào tệp văn bản hoặc email
4. Gửi đến bộ phận hỗ trợ của MAPIR nếu cần

### Báo cáo lỗi từ tab Nhật ký gỡ lỗi

Tính năng mới trong phiên bản 1.3.0: biểu tượng lỗi (**Báo cáo lỗi**) ở đầu tab Nhật ký gỡ lỗi sẽ gửi báo cáo trực tiếp đến MAPIR.

1. Nhấp vào biểu tượng lỗi ở đầu tab Nhật ký gỡ lỗi
2. Mô tả vấn đề: bạn đang làm gì, kết quả mong đợi và điều gì đã xảy ra thay vào đó (bắt buộc)
3. Tùy chọn: đánh dấu vào **Thêm một vài hình ảnh (tối đa 10)**và chọn hình ảnh từ dự án đang mở, hoặc sử dụng**Duyệt...** để chọn bất kỳ tệp hình ảnh nào khác
4. Nhấp vào **Gửi**

Báo cáo bao gồm mô tả của bạn, các hình ảnh bạn đính kèm, nhật ký Chloros và thông tin hệ thống (phiên bản Chloros, hệ điều hành, GPU và cài đặt dự án). Bạn phải đăng nhập vào Chloros (tab Người dùng) để gửi báo cáo. Bạn có thể đóng cửa sổ trong khi quá trình gửi đang diễn ra; khi quá trình hoàn tất, một mã tham chiếu như `Bug report sent. Reference: BR-…` sẽ được ghi vào nhật ký — hãy trích dẫn mã này khi liên hệ với MAPIR. Nếu báo cáo không thể được tải lên (ví dụ: không có kết nối internet), Chloros sẽ lưu báo cáo trên máy tính của bạn và hiển thị đường dẫn để bạn có thể gửi email báo cáo đó đến support@mapir.camera.

***

## Giám sát tài nguyên hệ thống

### Mức sử dụng CPU

**Chế độ Tự do:**

* 1 nhân CPU hoạt động ở mức ~100%
* Các nhân còn lại ở trạng thái nhàn rỗi hoặc sẵn sàng
* Hệ thống vẫn phản hồi bình thường

**Chế độ song song Chloros+:**

* Nhiều nhân CPU hoạt động ở mức cao — số lượng nhân phụ thuộc vào chiến lược được chọn bởi [Dynamic Compute Adaptation](../processing-architecture/dynamic-compute-adaptation.md)
* Hệ thống có thể cảm thấy chậm phản hồi hơn

**Để theo dõi:**

* Trình quản lý tác vụ Windows (Ctrl+Shift+Esc)
* Tab Hiệu suất → Phần CPU
* Tìm các tiến trình "Chloros" hoặc "chloros-backend"

### Sử dụng bộ nhớ (RAM)

**Mức sử dụng thông thường:**

* Dự án nhỏ (< 100 hình ảnh): 2–4 GB
* Dự án trung bình (100–500 hình ảnh): 4–8 GB
* Dự án lớn (500+ hình ảnh): 8–16 GB
* Chế độ song song của Chloros+ tiêu tốn nhiều RAM hơn

**Nếu bộ nhớ thấp:**

* Xử lý các lô nhỏ hơn
* Đóng các ứng dụng khác
* Nâng cấp RAM nếu thường xuyên xử lý các tập dữ liệu lớn

### Sử dụng GPU (ChChloros

với CUDA)

Khi bật tính năng tăng tốc GPU:

* GPU NVIDIA hiển thị mức sử dụng cao (60–90%)
* Mức sử dụng VRAM tăng lên (yêu cầu 4 GB VRAM trở lên; 7 GB trở lên cho quá trình loại bỏ hiệu ứng bayering đồng thời với Texture Aware)
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
* Tránh sử dụng ổ đĩa mạng cho các tập dữ liệu lớn
* Đảm bảo ổ đĩa không gần đầy (ảnh hưởng đến tốc độ ghi)

***

## Phát hiện sự cố trong quá trình xử lý

### Dấu hiệu cảnh báo

**Quá trình xử lý bị đình trệ (không có thay đổi trong 5 phút trở lên):**

* Kiểm tra Nhật ký gỡ lỗi để tìm lỗi
* Kiểm tra dung lượng đĩa trống
* Kiểm tra Trình quản lý tác vụ để đảm bảo Chloros đang chạy

**Thông báo lỗi xuất hiện thường xuyên:**

* Dừng xử lý và kiểm tra các lỗi
* Nguyên nhân phổ biến: dung lượng đĩa, tệp bị hỏng, vấn đề về bộ nhớ
* Xem phần Khắc phục sự cố bên dưới

**Hệ thống không phản hồi:**

* Chế độ song song của Chloros+ đang tiêu tốn quá nhiều tài nguyên
* Cân nhắc giảm số tác vụ đồng thời hoặc nâng cấp phần cứng
* Chế độ miễn phí tiêu tốn ít tài nguyên hơn

### Khi nào nên dừng xử lý

Dừng xử lý nếu bạn thấy:

* ❌ Lỗi "Đĩa đầy" hoặc "Không thể ghi tệp"
* ❌ Lỗi tệp hình ảnh bị hỏng lặp đi lặp lại
* ❌ Hệ thống bị treo hoàn toàn (không phản hồi)
* ❌ Phát hiện cài đặt sai
* ❌ Nhập nhầm hình ảnh

**Cách dừng:**

1. Nhấp vào**nút Dừng** (thay thế cho nút Bắt đầu) — chỉ cần nhấp một lần là đủ
2. Thanh tiến trình sẽ hiển thị "Đang dừng..." trong khi hình ảnh đang xử lý hoàn tất, sau đó quá trình chạy kết thúc ở trạng thái đã dừng
3. Các sản phẩm đã được xuất vẫn còn trên đĩa; nhật ký sẽ hiển thị mã lỗi `[RUN-SUMMARY]` mô tả chính xác những gì đã hoàn thành
4. Khắc phục sự cố và khởi động lại — quá trình xử lý sẽ bắt đầu lại từ đầu

***

## Khắc phục sự cố trong quá trình xử lý

### Quá trình xử lý diễn ra rất chậm

**Nguyên nhân có thể:**

* Hình ảnh mục tiêu chưa được đánh dấu (quét tất cả hình ảnh)
* Sử dụng ổ cứng HDD thay vì ổ SSD
* Tài nguyên hệ thống không đủ
* Cấu hình quá nhiều chỉ mục
* Truy cập ổ đĩa mạng

**Giải pháp:**

1. Nếu mới bắt đầu và đang ở giai đoạn Phát hiện: Dừng lại, đánh dấu mục tiêu, khởi động lại
2. Cho lần sau: Sử dụng SSD, giảm số lượng chỉ mục, nâng cấp phần cứng
3. Cân nhắc sử dụng giao diện dòng lệnh (CLI) để xử lý hàng loạt tập dữ liệu lớn

### Cảnh báo "Dung lượng đĩa"

**Giải pháp:**

1. Giải phóng dung lượng đĩa ngay lập tức
2. Chuyển dự án sang ổ đĩa có dung lượng lớn hơn
3. Giảm số lượng chỉ mục cần xuất
4. Tắt các sản phẩm xuất LATTICE không cần thiết (Cài đặt Dự án → Xử lý)
5. Sử dụng định dạng JPG thay vì TIFF (tệp nhỏ hơn)

### Thông báo "Tệp bị hỏng" xuất hiện thường xuyên

**Giải pháp:**

1. Sao chép lại hình ảnh từ thẻ SD để đảm bảo tính toàn vẹn
2. Kiểm tra lỗi trên thẻ SD
3. Loại bỏ các tệp bị hỏng khỏi dự án
4. Tiếp tục xử lý các hình ảnh còn lại

### Hệ thống quá nhiệt / Giảm tốc độ

**Giải pháp:**

1. Đảm bảo thông gió đủ
2. Làm sạch bụi khỏi các khe thông gió của máy tính
3. Giảm tải xử lý (sử dụng chế độ Free thay vì Chloros+)
4. Xử lý vào những thời điểm mát mẻ trong ngày

***

## Thông báo hoàn tất xử lý

Khi quá trình xử lý kết thúc:

* Thanh tiến trình đạt 100%
* Các dòng `[RUN-SUMMARY]` xuất hiện trong Nhật ký gỡ lỗi kèm theo số liệu cuối cùng
* Nút Bắt đầu lại được kích hoạt
* Tất cả tệp đầu ra đều nằm trong thư mục đầu ra theo từng camera của dự án: `<project>/<camera>/<format>/<Product>_Images/`

***

## Các bước tiếp theo

Sau khi quá trình xử lý hoàn tất:

1. **Kiểm tra kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)
2. **Kiểm tra thư mục đầu ra** - Xác minh tất cả các tệp đã được xuất chính xác
3. **Xem lại nhật ký gỡ lỗi** - Kiểm tra xem có cảnh báo hoặc lỗi nào không
4. **Xem trước hình ảnh đã xử lý** - Sử dụng Trình xem hình ảnh hoặc phần mềm bên ngoài

Để biết thông tin về cách xem lại và sử dụng kết quả đã xử lý, hãy xem [Hoàn tất quá trình xử lý](finishing-the-processing.md).
