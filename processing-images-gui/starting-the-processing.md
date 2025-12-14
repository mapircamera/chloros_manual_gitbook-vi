# Bắt đầu xử lý

Khi bạn đã nhập hình ảnh của mình, đánh dấu các mục tiêu hiệu chỉnh và định cấu hình cài đặt dự án của mình, bạn đã sẵn sàng bắt đầu xử lý. Trang này hướng dẫn bạn cách bắt đầu quy trình xử lý Chloros.

## Danh sách kiểm tra trước khi xử lý

Trước khi nhấp vào nút Bắt đầu, hãy xác minh rằng mọi thứ đã sẵn sàng:

* [ ] **Tệp đã nhập** - Tất cả hình ảnh xuất hiện trong Trình duyệt tệp
* [ ] **Hình ảnh mục tiêu được đánh dấu** - Cột mục tiêu đã được kiểm tra để hiệu chỉnh hình ảnh
* [ ] **Đã phát hiện các mẫu máy ảnh** - Cột Mẫu máy ảnh hiển thị các máy ảnh chính xác
* [ ] **Cài đặt được định cấu hình** - Cài đặt dự án được xem xét và điều chỉnh
* [ ] **Các chỉ số đã chọn** - Đã thêm các chỉ số đa phổ mong muốn (nếu cần)
* [ ] **Định dạng xuất đã chọn** - Định dạng đầu ra phù hợp với quy trình làm việc của bạn

{% gợi ý style="info" %}
**Mẹo**: Nhấp qua một số hình ảnh trong Trình duyệt tệp để xác minh rằng chúng được tải chính xác trước khi xử lý.
{% endhint %}

***

## Bắt đầu xử lý

### Xác định vị trí nút Bắt đầu

Nút Bắt đầu/Phát nằm ở thanh tiêu đề trên cùng của Chloros:

* Vị trí: Trên cùng chính giữa cửa sổ
* Biểu tượng: **Nút Phát/Bắt đầu** <img src="../.gitbook/assets/image (2).png" alt="" data-size="line">
* Trạng thái: Nút được bật (sáng) khi sẵn sàng xử lý

### Nhấp để bắt đầu

1. Nhấp vào **Nút Phát/Bắt đầu** ở tiêu đề trên cùng
2. Quá trình xử lý bắt đầu ngay lập tức
3. Nút này sẽ bị vô hiệu hóa (chuyển sang màu xám) trong quá trình xử lý
4. Cập nhật thanh tiến trình, hiển thị trạng thái xử lý

{% gợi ý style="thành công" %}
**Đã bắt đầu xử lý**: Sau khi nhấp vào, Chloros sẽ tự động xử lý tất cả các bước xử lý - phát hiện mục tiêu, gỡ lỗi, hiệu chỉnh, tính toán chỉ mục và xuất.
{% endhint %}

***

## Tìm hiểu các chế độ xử lý

Chloros hoạt động ở hai chế độ xử lý khác nhau tùy thuộc vào giấy phép của bạn:

### Chế độ miễn phí (Xử lý tuần tự)

**Có sẵn cho tất cả người dùng**

**Cách thức hoạt động:**

* Xử lý tuần tự từng hình ảnh một
* Hoạt động đơn luồng
* Sử dụng bộ nhớ thấp hơn

**Thanh tiến trình hiển thị 2 giai đoạn:**

1. **Phát hiện mục tiêu** - Quét các mục tiêu hiệu chỉnh
2. **Đang xử lý** - Áp dụng hiệu chỉnh và xuất hình ảnh

**Thời gian xử lý:**

* Chậm hơn nhiều so với chế độ song song của Chloros+
* Thích hợp cho bộ dữ liệu vừa và nhỏ (< 200 hình ảnh)

### Chế độ Chloros+ (Xử lý song song)

**Yêu cầu giấy phép Chloros+**

**Cách thức hoạt động:**

* Xử lý nhiều hình ảnh cùng một lúc
* Hoạt động đa luồng (tối đa 16 công nhân song song)
* Sử dụng nhiều lõi CPU
* Tăng tốc GPU (CUDA) tùy chọn với card đồ họa NVIDIA

**Thanh tiến trình hiển thị 4 giai đoạn:**

1. **Phát hiện** - Tìm mục tiêu hiệu chuẩn
2. **Phân tích** - Kiểm tra siêu dữ liệu hình ảnh và chuẩn bị quy trình
3. **Hiệu chỉnh** - Áp dụng hiệu chỉnh và hiệu chuẩn
4. **Xuất** - Lưu hình ảnh và chỉ mục đã xử lý

**Tương tác trên thanh tiến trình:**

* **Di chuột** qua thanh để xem bảng thả xuống chi tiết 4 giai đoạn
* **Nhấp vào** thanh tiến trình để cố định bảng thả xuống tại chỗ
* **Nhấp lại** để mở cố định và ẩn bảng điều khiển

**Thời gian xử lý:**

* Nhanh hơn đáng kể so với chế độ miễn phí
* Cân với số lượng lõi CPU
* Tăng tốc GPU cải thiện hơn nữa tốc độ

{% gợi ý style="info" %}
**Chloros+ Speed**: Xử lý song song có thể nhanh hơn 5-10 lần so với chế độ tuần tự đối với các tập dữ liệu lớn. Dự án 500 hình ảnh mất 2 giờ ở chế độ miễn phí có thể hoàn thành sau 15-20 phút với Chloros+.
{% endhint %}

***

## Điều gì xảy ra trong quá trình xử lý

### Giai đoạn 1: Phát hiện mục tiêu

**Chloros làm gì:**

* Quét hình ảnh mục tiêu được đánh dấu (hoặc tất cả hình ảnh nếu không được đánh dấu)
* Xác định 4 bảng hiệu chuẩn trong mỗi mục tiêu
* Trích xuất các giá trị phản xạ từ bảng mục tiêu
* Ghi lại dấu thời gian mục tiêu để lập kế hoạch hiệu chuẩn

**Thời lượng:** 1-30 giây (với mục tiêu được đánh dấu), 5-30+ phút (không được đánh dấu)

### Giai đoạn 2: Debayering (Chuyển đổi RAW)

**Chloros làm gì:**

* Chuyển đổi dữ liệu mẫu RAW Bayer thành hình ảnh RGB đầy đủ
* Áp dụng thuật toán demosaicing chất lượng cao
* Bảo toàn chất lượng hình ảnh và chi tiết tối đa

**Thời lượng:** Thay đổi tùy theo số lượng hình ảnh và tốc độ CPU

### Giai đoạn 3: Hiệu chỉnh

**Chloros làm gì:**

* **Hiệu chỉnh họa tiết**: Loại bỏ vết tối ở các cạnh của ống kính
* **Hiệu chỉnh độ phản xạ**: Chuẩn hóa bằng cách sử dụng các giá trị phản xạ mục tiêu
* Áp dụng hiệu chỉnh trên tất cả các băng tần/kênh
* Sử dụng mục tiêu hiệu chỉnh phù hợp cho từng hình ảnh dựa trên dấu thời gian

**Thời lượng:** Phần lớn thời gian xử lý

### Giai đoạn 4: Tính chỉ số

**Chloros làm gì:**

* Tính toán các chỉ số đa phổ được cấu hình (NDVI, NDRE, v.v.)
* Áp dụng toán học dải cho hình ảnh được hiệu chỉnh
* Tạo hình ảnh chỉ mục cho từng chỉ mục đã chọn

**Thời lượng:** Một vài giây cho mỗi hình ảnh

### Giai đoạn 5: Xuất khẩu

**Chloros làm gì:**

* Lưu hình ảnh đã hiệu chỉnh ở định dạng đã chọn
* Xuất hình ảnh chỉ mục với màu LUT được định cấu hình
* Ghi tập tin vào thư mục con của mẫu máy ảnh
* Giữ nguyên tên tập tin gốc có hậu tố

**Thời lượng:** Thay đổi tùy theo định dạng xuất và kích thước tệp

***

## Hành vi xử lý

### Đường ống xử lý tự động

Sau khi bắt đầu, toàn bộ quy trình sẽ tự động chạy:

* Không cần sự tương tác của người dùng
* Tất cả các bước cấu hình thực hiện theo trình tự
* Cập nhật tiến độ được hiển thị trong thời gian thực

### Sử dụng máy tính trong quá trình xử lý

**Chế độ miễn phí:**

* Sử dụng CPU tương đối thấp (đơn luồng)
* Máy tính vẫn đáp ứng các tác vụ khác
* An toàn để giảm thiểu Cloros và hoạt động trong các ứng dụng khác

**Chloros+ Chế độ song song:**

* Mức sử dụng CPU cao (đa luồng, tối đa 16 lõi)
* Với khả năng tăng tốc GPU: Mức sử dụng GPU cao
* Máy tính có thể phản hồi kém hơn trong quá trình xử lý
* Tránh bắt đầu các tác vụ ngốn CPU khác

{% gợi ý style="warning" %}
**Mẹo về hiệu suất**: Để có hiệu suất Chloros+ tốt nhất, hãy đóng các ứng dụng khác và để Chloros sử dụng toàn bộ tài nguyên hệ thống.
{% endhint %}

### Không thể tạm dừng quá trình xử lý

**Những hạn chế quan trọng:**

* Sau khi bắt đầu, quá trình xử lý không thể bị tạm dừng
* Bạn có thể hủy quá trình xử lý nhưng tiến trình sẽ bị mất
* Kết quả một phần không được lưu
* Phải khởi động lại từ đầu nếu bị hủy

**Mẹo lập kế hoạch:** Đối với các dự án rất lớn, hãy cân nhắc xử lý theo đợt hoặc sử dụng CLI để kiểm soát tốt hơn.

***

## Giám sát quá trình xử lý của bạn

Trong khi chạy quá trình xử lý, bạn có thể:

* **Xem thanh tiến trình** - Xem phần trăm hoàn thành tổng thể
* **Xem giai đoạn hiện tại** - Phát hiện, Phân tích, Hiệu chỉnh hoặc Xuất
* **Kiểm tra tab nhật ký** - Xem các thông báo và cảnh báo xử lý chi tiết
* **Xem trước hình ảnh đã hoàn thành** - Một số tệp xuất có thể xuất hiện trong quá trình xử lý

Để biết thông tin chi tiết về giám sát, hãy xem [Giám sát quá trình xử lý](monitoring-the-processing.md).

***

## Đang hủy xử lý

Nếu bạn cần dừng xử lý:

### Cách hủy

1. Xác định vị trí **Nút Dừng/Hủy** (thay thế nút Bắt đầu trong quá trình xử lý)
2. Nhấp vào nút Dừng
3. Quá trình xử lý dừng ngay lập tức
4. Kết quả một phần bị loại bỏ

### Khi nào nên hủy

**Lý do hợp lệ để hủy:**

* Nhận thấy cài đặt không chính xác đã được sử dụng
* Quên đánh dấu hình ảnh mục tiêu
* Hình ảnh được nhập sai
* Hệ thống chạy quá chậm hoặc không phản hồi

**Sau khi hủy:**

* Xem xét và khắc phục mọi sự cố
* Điều chỉnh cài đặt khi cần thiết
* Khởi động lại quá trình xử lý từ đầu
* Để có trải nghiệm sạch nhất, hãy đóng hoàn toàn Chloros và khởi động lại

{% gợi ý style="warning" %}
**Không có kết quả một phần**: Việc hủy sẽ loại bỏ tất cả tiến trình. Cloros không lưu hình ảnh được xử lý một phần.
{% endhint %}

***

## Ước tính thời gian xử lý

Thời gian xử lý thực tế thay đổi rất nhiều dựa trên:

* Số lượng hình ảnh
* Độ phân giải hình ảnh
* Định dạng đầu vào RAW và JPG
* Chế độ xử lý (Miễn phí so với Chloros+)
* Tốc độ CPU và số lượng lõi
* Tính khả dụng của GPU (chỉ Cloros+)
* Số chỉ số cần tính
* Độ phức tạp của định dạng xuất

### Ước tính sơ bộ (Chloros+, hình ảnh 12MP, CPU hiện đại)

| Đếm Hình Ảnh | Chế độ miễn phí | Cloros+ (CPU) | Cloros+ (GPU) |
| ----------- | --------- | -------------- | -------------- |
| 50 hình ảnh | 15-20 phút | 5-8 phút | 3-5 phút |
| 100 hình ảnh | 30-40 phút | 10-15 phút | 5-8 phút |
| 200 hình ảnh | 1-1,5 giờ | 20-30 phút | 10-15 phút |
| 500 hình ảnh | 2-3 giờ | 45-60 phút | 20-30 phút |
| 1000 hình ảnh | 4-6 giờ | 1,5-2 giờ | 40-60 phút |

{% gợi ý style="info" %}
**Lần đầu tiên**: Quá trình xử lý ban đầu có thể mất nhiều thời gian hơn do Chloros xây dựng bộ nhớ đệm và cấu hình. Việc xử lý các tập dữ liệu tương tự sau đó sẽ nhanh hơn.
{% endhint %}

***

## Các vấn đề thường gặp khi bắt đầu

### Nút Bắt đầu bị vô hiệu hóa (Chuyển sang màu xám)

**Nguyên nhân có thể:**

* Không có hình ảnh được nhập
* Phần phụ trợ chưa bắt đầu đầy đủ
* Quá trình xử lý trước đó vẫn đang chạy
* Dự án chưa được tải đầy đủ

**Giải pháp:**

1. Đợi phần phụ trợ khởi chạy hoàn toàn (kiểm tra biểu tượng menu chính)
2. Xác minh hình ảnh được nhập vào File Browser
3. Khởi động lại Chloros nếu nút vẫn bị tắt
4. Kiểm tra Nhật ký gỡ lỗi để biết thông báo lỗi

### Quá trình bắt đầu rồi thất bại ngay lập tức

**Nguyên nhân có thể:**

* Không có hình ảnh hợp lệ trong dự án
* Tệp hình ảnh bị hỏng
* Dung lượng đĩa không đủ
* Không đủ bộ nhớ (RAM)

**Giải pháp:**

1. Kiểm tra Nhật ký gỡ lỗi <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> để biết thông báo lỗi
2. Xác minh dung lượng đĩa trống
3. Thử xử lý một tập hợp con hình ảnh nhỏ hơn
4. Xác minh hình ảnh không bị hỏng

### Cảnh báo "Không phát hiện mục tiêu"

**Nguyên nhân có thể:**

* Quên đánh dấu hình ảnh mục tiêu
* Hình ảnh mục tiêu không chứa mục tiêu hiển thị
* Cài đặt phát hiện mục tiêu quá nghiêm ngặt

**Giải pháp:**

1. Xem lại [Chọn ảnh mục tiêu](chọn-target-images.md)
2. Đánh dấu hình ảnh phù hợp vào cột Target
3. Xác minh mục tiêu hiển thị trong hình ảnh được đánh dấu
4. Điều chỉnh cài đặt phát hiện mục tiêu nếu cần

***

## Mẹo để xử lý thành công

### Trước khi bắt đầu

1. **Thử nghiệm với tập hợp con nhỏ trước** - Xử lý 10-20 hình ảnh để xác minh cài đặt
2. **Kiểm tra dung lượng đĩa trống** - Đảm bảo kích thước tập dữ liệu miễn phí 2-3x
3. **Đóng các ứng dụng không cần thiết** - Giải phóng tài nguyên hệ thống
4. **Xác minh hình ảnh mục tiêu** - Xem trước mục tiêu được đánh dấu để đảm bảo chất lượng
5. **Lưu dự án** - Dự án tự động lưu nhưng cách tốt nhất là lưu thủ công

### Trong quá trình xử lý

1. **Tránh chế độ ngủ của hệ thống** - Tắt chế độ tiết kiệm năng lượng
2. **Giữ Chloros ở nền trước** - Hoặc ít nhất là hiển thị trên thanh tác vụ
3. **Thỉnh thoảng theo dõi tiến trình** - Kiểm tra các cảnh báo hoặc lỗi
4. **Không tải các ứng dụng nặng khác** - Đặc biệt có chế độ song song Chloros+

### Tăng tốc GPU + Chloros

Nếu sử dụng khả năng tăng tốc GPU NVIDIA:

1. Cập nhật trình điều khiển NVIDIA lên phiên bản mới nhất
2. Đảm bảo GPU có 4GB+ VRAM
3. Đóng các ứng dụng sử dụng nhiều GPU (game, chỉnh sửa video)
4. Theo dõi nhiệt độ GPU (đảm bảo làm mát đầy đủ)

***

## Các bước tiếp theo

Khi quá trình xử lý đã bắt đầu:

1. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)
2. **Chờ hoàn thành** - Quá trình xử lý diễn ra tự động
3. **Xem lại kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)

Để biết thông tin về những việc cần làm trong quá trình xử lý, hãy xem [Giám sát quá trình xử lý](monitoring-the-processing.md).
