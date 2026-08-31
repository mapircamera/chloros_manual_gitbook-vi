# Bắt đầu quá trình xử lý

Sau khi đã nhập hình ảnh, đánh dấu các mục tiêu hiệu chuẩn và cấu hình cài đặt dự án, bạn đã sẵn sàng bắt đầu quá trình xử lý. Trang này sẽ hướng dẫn bạn cách khởi chạy quy trình xử lý “Chloros”.

## Danh sách kiểm tra trước khi xử lý

Trước khi nhấp vào nút Bắt đầu, hãy kiểm tra xem mọi thứ đã sẵn sàng chưa:

* [ ] **Tệp đã được nhập** - Tất cả hình ảnh đều xuất hiện trong Trình duyệt tệp
* [ ] **Hình ảnh mục tiêu đã được đánh dấu** - Cột Mục tiêu đã được kiểm tra đối với hình ảnh hiệu chuẩn (hoặc tệp ghi `.daq` đã được nhập cho LATTICE)
* [ ] **Đã phát hiện các mẫu máy ảnh** - Cột &quot;Mẫu máy ảnh&quot; hiển thị đúng các máy ảnh
* [ ] **Đã cấu hình cài đặt** - Đã xem xét và điều chỉnh Cài đặt dự án
* [ ] **Đã chọn các chỉ số** - Đã thêm các chỉ số đa phổ mong muốn (nếu cần)
* [ ] **Định dạng xuất đã được chọn** - Định dạng đầu ra phù hợp với quy trình làm việc của bạn

{% hint style="info" %}
**Mẹo**: Nhấp qua một vài hình ảnh trong Trình duyệt tệp để xác minh rằng chúng đã được tải đúng cách trước khi xử lý.
{% endhint %}

***

## Bắt đầu xử lý

### Tìm nút Bắt đầu

Nút Bắt đầu/Phát nằm ở thanh tiêu đề trên cùng củChloros:

* Vị trí: Giữa phía trên cửa sổ
* Biểu tượng: **Nút Phát/Bắt đầu** <img src="../.gitbook/assets/image (2) (1) (1).png" alt="" data-size="line">
* Trạng thái: Nút được kích hoạt (sáng) khi sẵn sàng xử lý

### Nhấp để bắt đầu

1. Nhấp vào **nút Phát/Bắt đầu** ở thanh tiêu đề trên cùng
2. Quá trình xử lý bắt đầu ngay lập tức
3. Nút này sẽ chuyển thành nút **Dừng** trong quá trình xử lý
4. Thanh tiến trình được cập nhật, hiển thị trạng thái xử lý

{% hint style="success" %}
**Đã bắt đầu xử lý**: Sau khi nhấp vào, Chloros sẽ tự động xử lý tất cả các bước - phát hiện mục tiêu, khử hiệu ứng Bayer, hiệu chuẩn, tính toán chỉ số và xuất dữ liệu. Phần mềm tự động phát hiện dự án của bạn thuộc loại Survey3, LATTICE hay kết hợp cả hai, và áp dụng quy trình xử lý phù hợp cho từng camera.
{% endhint %}

***

## Hiểu về các chế độ xử lý

Chloroshoạt động ở hai chế độ xử lý khác nhau tùy thuộc vào giấy phép của bạn:

### Chế độ Miễn phí (Xử lý tuần tự)

**Dành cho tất cả người dùng**

**Cách thức hoạt động:**

* Xử lý hình ảnh từng cái một, theo thứ tự
* Hoạt động trên một luồng duy nhất
* Tiêu thụ bộ nhớ thấp hơn

**Thanh tiến trình hiển thị 2 giai đoạn:**

1.**Phát hiện mục tiêu** - Quét để tìm các mục tiêu hiệu chuẩn
2. **Xử lý** - Áp dụng hiệu chuẩn và xuất hình ảnh**Thời gian xử lý:**

* Chậm hơn nhiều so với chế độ song song Chloros+
* Phù hợp với các tập dữ liệu nhỏ đến trung bình (&lt; 200 hình ảnh)

### Chế độ Chloros+ (Xử lý song song)

**Yêu cầu giấy phép Chloros+**

**Cách thức hoạt động:**

* Xử lý nhiều hình ảnh đồng thời thông qua [đường ống xử lý 4 luồng](../processing-architecture/processing-pipeline.md)
* [Tính năng Điều chỉnh Tính toán Động](../processing-architecture/dynamic-compute-adaptation.md) tự động chọn chiến lược tối ưu cho phần cứng của bạn ngay khi bắt đầu chạy
* Tăng tốc GPU (CUDA) với card đồ họa NVIDIA (máy tính để bàn và Jetson)
* **Số lượng worker thích ứng với phần cứng**: Các chiến lược GPU chạy**1–4 worker đồng thời** (tùy thuộc vào dung lượng VRAM — một thiết bị Jetson có bộ nhớ thấp chạy 1 worker, trong khi GPU máy tính để bàn có dung lượng 12GB trở lên chạy tối đa 4 worker); các hệ thống chỉ dùng CPU chạy một worker cho mỗi lõi vật lý, trừ đi một**Thanh tiến trình hiển thị 4 giai đoạn** (tương ứng với 4 luồng xử lý trong đường ống):

1. **Phát hiện** (Luồng 1) - Tìm các mục tiêu hiệu chuẩn
2. **Phân tích** (Luồng 2) - Kiểm tra siêu dữ liệu hình ảnh và tính toán hiệu chuẩn
3. **Hiệu chuẩn** (Luồng 3) - Loại bỏ hiệu ứng debayering, chỉnh sửa hiệu ứng vignette, hiệu chuẩn, tính toán chỉ số
4. **Xuất** (Luồng 4) - Lưu hình ảnh đã xử lý và các chỉ số**Tương tác với thanh tiến trình:*** **Di chuột** qua thanh tiến trình để xem bảng thả xuống chi tiết gồm 4 giai đoạn
* **Nhấp chuột** vào thanh tiến trình để cố định bảng thả xuống tại vị trí hiện tại
* **Nhấp chuột lần nữa** để bỏ cố định và ẩn bảng**Thời gian xử lý:**

* Nhanh hơn đáng kể so với chế độ miễn phí
* Tăng tốc GPU giúp cải thiện tốc độ hơn nữa

{% hint style="info" %}
**Tốc độ Chloros+**: Xử lý song song có thể nhanh hơn 5-10 lần so với chế độ tuần tự đối với các tập dữ liệu lớn. Một dự án gồm 500 hình ảnh mất 2 giờ trong chế độ miễn phí có thể hoàn thành trong 15-20 phút với Chloros+.
{% endhint %}

***

## Quá trình xử lý diễn ra như thế nào

### Giai đoạn 1: Phát hiện mục tiêu

**Chloros thực hiện những gì:**

* Quét các hình ảnh bạn đã chọn trong cột Mục tiêu (tất cả hình ảnh nếu không có hình nào được đánh dấu)
* Xác định các bảng hiệu chuẩn trong mỗi mục tiêu
* Trích xuất các giá trị phản xạ từ các bảng mục tiêu
* Ghi lại dấu thời gian của mục tiêu để lập lịch hiệu chuẩn

**Thời gian:** 1–30 giây (với các mục tiêu đã đánh dấu), 5–30+ phút (chưa đánh dấu)

### Giai đoạn 2: Loại bỏ mẫu Bayer (Chuyển đổi RAW)

**Chloros thực hiện những gì:**

* Chuyển đổi dữ liệu mẫu Bayer định dạng RAW thành hình ảnh 3 kênh đầy đủ (các mô-đun LATTICE mono vẫn giữ nguyên dải đơn — quá trình loại bỏ mẫu Bayer sẽ bị bỏ qua đối với chúng và có ghi chú trong nhật ký)
* Áp dụng thuật toán demosaicing đã chọn
* Giữ nguyên chất lượng và chi tiết hình ảnh tối đa

**Thời gian:** Thay đổi tùy theo số lượng hình ảnh và tốc độ CPU/GPU

### Giai đoạn 3: Hiệu chuẩn

**Chloros thực hiện những gì:*** **Chỉnh sửa hiệu ứng tối góc**: Loại bỏ hiện tượng tối góc do ống kính gây ra
* **Hiệu chuẩn độ phản xạ**: Chuẩn hóa bằng cách sử dụng các giá trị độ phản xạ mục tiêu và/hoặc dữ liệu chiếu xuống từ hệ thống thu thập dữ liệu (DAQ)
* Áp dụng các chỉnh sửa trên tất cả các dải/kênh
* Sử dụng tham chiếu hiệu chuẩn phù hợp cho từng hình ảnh dựa trên dấu thời gian

**Thời gian thực hiện:** Chiếm phần lớn thời gian xử lý

### Giai đoạn 4: Tính toán chỉ số

**Chloros thực hiện những gì:**

* Tính toán các chỉ số đa phổ đã cấu hình (NDVI, NDRE, v.v.)
* Áp dụng các phép toán dải phổ lên các hình ảnh đã được hiệu chuẩn
* Tạo hình ảnh chỉ số cho từng chỉ số đã chọn

**Thời gian thực hiện:** Vài giây cho mỗi hình ảnh

### Giai đoạn 5: Xuất dữ liệu

**Chloros thực hiện những gì:**

* Lưu các hình ảnh đã xử lý ở định dạng đã chọn
* **LATTICE fan-out**: mỗi khung hình thô LATTICE được xuất dưới dạng tất cả các sản phẩm đã bật trong một lần xử lý — đã loại bỏ hiệu ứng Bayer, bản xem trước, độ sáng (luôn là float32), độ phản xạ
* Ghi tệp vào thư mục đầu ra của dự án: `<project>/<camera>/<format>/<Product>_Images/`
* **Giữ nguyên tên tệp nguồn** — thư mục xác định sản phẩm, không thêm hậu tố**Thời gian thực hiện:** Thay đổi tùy theo định dạng xuất và kích thước tệp***

## Hành vi xử lý

### Quy trình xử lý tự động

Sau khi bắt đầu, toàn bộ quy trình sẽ chạy tự động:

* Không cần tương tác của người dùng
* Tất cả các bước đã cấu hình được thực thi theo thứ tự
* Cập nhật tiến trình được hiển thị theo thời gian thực
* Các tệp xuất được ghi vào đĩa ngay khi hoàn thành — bạn có thể mở các tệp đầu ra đã hoàn tất trong khi quy trình vẫn đang diễn ra

### Sử dụng tài nguyên máy tính trong quá trình xử lý

**Chế độ Tự do:**

* Mức sử dụng CPU tương đối thấp (chế độ đơn luồng)
* Máy tính vẫn phản hồi tốt cho các tác vụ khác
* An toàn khi thu nhỏ cửa sổ Chloros và làm việc trong các ứng dụng khác

**Chế độ song song Chloros+:**

* Mức sử dụng CPU cao trên toàn bộ nhóm công nhân của chiến lược
* Khi có tăng tốc GPU: Mức sử dụng GPU cao
* Máy tính có thể phản hồi chậm hơn trong quá trình xử lý
* Tránh khởi chạy các tác vụ tiêu tốn nhiều CPU khác

{% hint style="warning" %}
**Mẹo tối ưu hiệu suất**: Để đạt hiệu suất Chloros+ tốt nhất, hãy đóng các ứng dụng khác và cho phép Chloros sử dụng toàn bộ tài nguyên hệ thống.
{% endhint %}

### Quá trình xử lý không thể tạm dừng (nhưng việc dừng lại diễn ra trơn tru)

* Sau khi bắt đầu, quá trình xử lý không thể tạm dừng và tiếp tục sau đó
* Nhấp vào **Dừng** sẽ dừng quá trình một cách triệt để ngay lần nhấp đầu tiên
* Các sản phẩm đã được xuất trước khi dừng vẫn được lưu trên đĩa
* Một quá trình bị dừng sẽ báo cáo chính xác những gì đã hoàn thành (xem các dòng `[RUN-SUMMARY]` trong nhật ký)
* Một lần chạy mới sẽ bắt đầu quy trình từ đầu

**Lời khuyên về lập kế hoạch:** Đối với các dự án rất lớn, hãy cân nhắc xử lý theo lô hoặc sử dụng CLI để kiểm soát tốt hơn.***

## Theo dõi quá trình xử lý

Trong khi quá trình xử lý đang diễn ra, bạn có thể:

* **Theo dõi thanh tiến trình** - Xem tỷ lệ phần trăm hoàn thành tổng thể
* **Xem giai đoạn hiện tại** - Phát hiện, Phân tích, Hiệu chuẩn hoặc Xuất
* **Kiểm tra tab nhật ký** - Xem các thông báo và cảnh báo chi tiết về quá trình xử lý
* **Xem trước các hình ảnh đã hoàn thành** - Các tệp xuất sẽ xuất hiện trên đĩa trong quá trình xử lý

Để biết thông tin chi tiết về việc theo dõi, hãy xem [Theo dõi quá trình xử lý](monitoring-the-processing.md).

***

## Dừng quá trình xử lý

Nếu bạn cần dừng quá trình xử lý:

### Cách dừng

1. Tìm **nút Dừng** (thay thế nút Bắt đầu trong quá trình xử lý)
2. Nhấp vào nút này một lần — thanh tiến trình sẽ hiển thị **&quot;Đang dừng...&quot;** trong khi hình ảnh đang xử lý hoàn tất
3. Quá trình xử lý kết thúc ở trạng thái dừng hoàn toàn và nhật ký sẽ ghi lại thông tin chính xác về những gì đã hoàn thành

### Khi nào nên dừng

**Các lý do hợp lệ để dừng:**

* Nhận ra đã sử dụng cài đặt sai
* Quên đánh dấu các hình ảnh mục tiêu
* Nhập nhầm hình ảnh
* Hệ thống chạy quá chậm hoặc không phản hồi

**Sau khi dừng:**

* Các sản phẩm đã được xuất trước khi dừng vẫn còn trên đĩa
* Kiểm tra và khắc phục các vấn đề, điều chỉnh cài đặt nếu cần
* Khởi động lại quá trình xử lý — quá trình sẽ bắt đầu lại từ đầu

***

## Ước tính thời gian xử lý

Thời gian xử lý thực tế thay đổi đáng kể tùy thuộc vào:

* Số lượng hình ảnh
* Độ phân giải hình ảnh
* Định dạng đầu vào RAW so với JPG
* Chế độ xử lý (Free so với Chloros+)
* Tốc độ CPU và số nhân
* Tình trạng sẵn có của GPU (chỉ dành cho Chloros+)
* Số lượng chỉ số cần tính toán
* Số lượng sản phẩm xuất được kích hoạt (LATTICE)

### Ước tính sơ bộ (Chloros+, hình ảnh 12MP, CPU hiện đại)

| Số lượng hình ảnh | Chế độ Free | Chloros+ (CPU) | Chloros+ (GPU) |
| ----------- | --------- | -------------- | -------------- |
| 50 hình ảnh   | 15–20 phút | 5–8 phút        | 3–5 phút        |
| 100 hình ảnh  | 30–40 phút | 10–15 phút      | 5–8 phút        |
| 200 hình ảnh  | 1–1,5 giờ | 20–30 phút      | 10–15 phút      |
| 500 hình ảnh  | 2–3 giờ   | 45–60 phút      | 20–30 phút      |
| 1.000 hình ảnh | 4–6 giờ   | 1,5–2 giờ      | 40–60 phút      |

{% hint style="info" %}
**Lần chạy đầu tiên**: Quá trình xử lý ban đầu có thể mất nhiều thời gian hơn do Chloros đang xây dựng bộ nhớ đệm và hồ sơ. Các lần xử lý tiếp theo đối với các bộ dữ liệu tương tự sẽ nhanh hơn.
{% endhint %}

***

## Các sự cố thường gặp khi khởi động

### Nút Bắt đầu bị vô hiệu hóa (màu xám)

**Nguyên nhân có thể:**

* Chưa nhập hình ảnh
* Hệ thống nền chưa khởi động hoàn toàn
* Quá trình xử lý trước đó vẫn đang chạy
* Dự án chưa được tải hoàn toàn

**Giải pháp:**

1. Chờ hệ thống nền khởi tạo hoàn toàn (kiểm tra biểu tượng trên menu chính)
2. Kiểm tra xem hình ảnh đã được nhập vào Trình duyệt tệp chưa
3. Khởi động lại Chloros nếu nút vẫn bị vô hiệu hóa
4. Kiểm tra Nhật ký gỡ lỗi để tìm thông báo lỗi

### Quá trình xử lý bắt đầu rồi ngay lập tức thất bại

**Nguyên nhân có thể:**

* Không có hình ảnh hợp lệ trong dự án
* Tệp hình ảnh bị hỏng
* Không đủ dung lượng đĩa
* Không đủ bộ nhớ (RAM)

**Giải pháp:**

1. Kiểm tra Nhật ký gỡ lỗi <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> để tìm thông báo lỗi
2. Kiểm tra dung lượng đĩa trống
3. Thử xử lý một tập con nhỏ hơn các hình ảnh
4. Kiểm tra xem các hình ảnh có bị hỏng hay không

### Quá trình chạy hoàn tất nhưng không ghi ra hình ảnh nào

Một lần chạy yêu cầu các sản phẩm hình ảnh nhưng không ghi ra hình ảnh nào sẽ được coi là **thất bại, không phải thành công** — tệp Chloros sẽ báo cáo rõ ràng điều này:

* Nhật ký GUI hiển thị các mã lỗi `[RUN-SUMMARY]` gợi ý nguyên nhân có thể — không có hình ảnh nào được nhập, không phát hiện được mục tiêu, hoặc mọi sản phẩm được yêu cầu đều bị bỏ qua vì không áp dụng được (ví dụ: yêu cầu độ rạng rỡ/độ phản xạ từ các camera chỉ hỗ trợ chế độ &quot;RGB&quot;)
* Lỗi tương đương với &quot;CLI&quot; (`chloros-cli process`) sẽ hiển thị thông báo `Processing finished but wrote no image products.` và **thoát với giá trị không bằng 0**, do đó các skript có thể phát hiện được lỗi này
* Một lần chạy chỉ xử lý metadata (tất cả sản phẩm xuất đều bị vô hiệu hóa, không có chỉ mục) vẫn được coi là thành công

Xem [Tài liệu tham khảo về CLI](../reference/cli-reference.md#a-run-that-writes-no-images-fails) để biết chi tiết về ngữ nghĩa.

### Cảnh báo &quot;Không phát hiện mục tiêu&quot;

**Nguyên nhân có thể:**

* Quên đánh dấu hình ảnh mục tiêu
* Hình ảnh mục tiêu không chứa mục tiêu có thể nhìn thấy
* Cài đặt phát hiện mục tiêu quá nghiêm ngặt

**Giải pháp:**

1. Xem lại [Cách chọn hình ảnh mục tiêu](choosing-target-images.md)
2. Đánh dấu các hình ảnh phù hợp trong cột &quot;Mục tiêu&quot;
3. Kiểm tra xem các mục tiêu có hiển thị trong các hình ảnh đã đánh dấu hay không
4. Điều chỉnh cài đặt phát hiện mục tiêu nếu cần thiết

***

## Mẹo để xử lý thành công

### Trước khi bắt đầu

1. **Thử nghiệm với một tập con nhỏ trước** - Xử lý 10-20 hình ảnh để kiểm tra cài đặt
2. **Kiểm tra dung lượng đĩa trống** - Đảm bảo có dung lượng trống gấp 2-3 lần kích thước tập dữ liệu (nhiều hơn nếu tất cả các sản phẩm LATTICE được kích hoạt)
3. **Đóng các ứng dụng không cần thiết** - Giải phóng tài nguyên hệ thống
4. **Xác minh hình ảnh mục tiêu** - Xem trước các mục tiêu đã đánh dấu để đảm bảo chất lượng
5. **Lưu dự án** - Dự án sẽ tự động lưu, nhưng nên lưu thủ công để đảm bảo an toàn

### Trong quá trình xử lý

1. **Tránh chế độ ngủ của hệ thống** - Tắt các chế độ tiết kiệm năng lượng
2. **Giữ Chloros ở chế độ nền trước** - Hoặc ít nhất là hiển thị trên thanh tác vụ
3. **Thỉnh thoảng theo dõi tiến trình** - Kiểm tra các cảnh báo hoặc lỗi
4. **Không mở các ứng dụng nặng khác** - Đặc biệt khi sử dụng chế độ song song Chloros+

### Chloros+ Tăng tốc GPU

Nếu sử dụng tăng tốc GPU của NVIDIA:

1. Cập nhật trình điều khiển NVIDIA lên phiên bản mới nhất
2. Đảm bảo GPU có 4GB+ VRAM (7GB+ cho quá trình gỡ nhiễu Texture Aware đồng thời)
3. Đóng các ứng dụng tiêu tốn nhiều tài nguyên GPU (trò chơi, chỉnh sửa video)
4. Theo dõi nhiệt độ GPU (đảm bảo hệ thống làm mát đủ tốt)

***

## Các bước tiếp theo

Khi quá trình xử lý đã bắt đầu:

1. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)
2. **Chờ đến khi hoàn tất** - Quá trình xử lý diễn ra tự động
3. **Kiểm tra kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)

Để biết thông tin về những việc cần làm trong quá trình xử lý, hãy xem [Theo dõi quá trình xử lý](monitoring-the-processing.md).
