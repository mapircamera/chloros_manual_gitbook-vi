# Bắt đầu quá trình xử lý

Khi bạn đã nhập hình ảnh của mình, đánh dấu dấu hiệu chỉnh sửa tiêu điểm và định cấu hình cài đặt dự án của mình, bạn đã sẵn sàng bắt đầu quá trình xử lý. Trang này hướng dẫn bạn cách bắt đầu quá trình xử lý Chloros.

## Danh sách kiểm tra trước khi xử lý

Trước khi nhấp vào nút Bắt đầu, hãy chắc chắn rằng mọi thứ đã có sẵn:

* [ ] **Đã nhập các văn bản** - Tất cả hình ảnh được hiển thị trong tệp trình duyệt
* [ ] **Tiêu chuẩn hình ảnh được đánh dấu** - Tiêu chuẩn cột đã được kiểm tra để chỉnh sửa hình ảnh
* [ ] **Đã phát hiện các máy ảnh mẫu** - Cột mẫu máy ảnh hiển thị các máy ảnh chính xác
* [ ] **Cài đặt cấu hình** - Cài đặt dự án được xem xét và điều chỉnh
* [ ] **Các chỉ số đã chọn** - Đã thêm các chỉ số đa mong muốn (nếu cần)
* [ ] **Lựa chọn xuất định dạng** - Định dạng đầu ra phù hợp với quy trình làm việc của bạn

{% hint style="info" %}
**Mẹo**: Bấm qua một số hình ảnh trong trình duyệt tệp tệp để xác minh rằng chúng được tải chính xác trước khi xử lý.
{% endhint %}

***

## Bắt đầu xử lý

### Xác định vị trí bắt đầu của nút

Nút Bắt đầu/Phát nằm ở thanh tiêu đề trên cùng của Chloros:

* Vị trí: Trên cùng chính giữa cửa sổ
* Biểu tượng: **Nút Phát/Bắt đầu** <img src="../.gitbook/assets/image (2) (1).png" alt="" data-size="line">
* Trạng thái: Nút được bật (sáng) khi sẵn sàng xử lý

### Cú để bắt đầu

1. Bấm vào **Nút Phát/Bắt đầu** ở tiêu đề trên cùng
2. Quá trình xử lý bắt đầu ngay lập tức
3. Nút này bị vô hiệu hóa (chuyển sang màu xám) trong quá trình xử lý
4. Cập nhật tiến trình, hiển thị trạng thái

{% hint style="success" %}
**Đã xử lý bắt đầu**: Sau khi nhấp vào, Chloros sẽ tự động xử lý tất cả các bước xử lý - phát hiện mục tiêu, gỡ lỗi, hiệu chỉnh, tính toán chỉ mục và xuất.
{% endhint %}

***

## Tìm hiểu các chế độ xử lý

Chloros hoạt động ở hai chế độ xử lý khác nhau tùy thuộc vào giấy phép của bạn:

### Chế độ miễn phí (Xử lý tuần tự)

**Có sẵn cho tất cả người dùng**

**Hoạt động của thức:**

* Xử lý từng hình ảnh một
* Luồng đơn hoạt động
* Use low more than

**Chương trình tiến trình hiển thị giai đoạn 2:**

1.**Phát các mục tiêu** - Quét hiệu chỉnh các mục tiêu
2. **Đang xử lý**- Áp dụng hiệu chỉnh và xuất hình ảnh**Xử lý thời gian:**

* Chậm hơn nhiều so với chế độ bài hát Chloros+
* Thích hợp cho bộ dữ liệu vừa và nhỏ (< 200 image)

### Chế độ Chloros+ (Xử lý bài hát)

**Yêu cầu giấy phép Chloros+**

**Hoạt động của thức:**

* Xử lý đồng thời nhiều hình ảnh bằng [đường dẫn xử lý 4 luồng](../processing-architecture/processing-pipeline.md)
* [Điều chỉnh điện toán](../processing-architecture/dynamic-compute-adaptation.md) tự động chọn chiến lược tối ưu cho phần cứng của bạn
* Tăng tốc GPU (CUDA) với đồ họa card NVIDIA (máy tính để bàn và Jetson)
* Cân từ Jetson Nano (1 công nhân) đến máy tính để bàn có GPU 12GB+ (3-4 nhân)

**Chương trình thanh tiến trình hiển thị giai đoạn 4** (kết hợp với tiến trình quy trình 4 luồng):

1. **Phát hiện** (Chủ đề 1) - Tìm tiêu chuẩn
2. **Phân tích** (Chủ đề 2) - Kiểm tra hình ảnh siêu dữ liệu và hiệu chỉnh máy tính
3. **Chỉnh sửa** (Chủ đề 3) - Gỡ bỏ lỗi GPU, hiệu chỉnh chi tiết, tính toán chỉ số
4. **Xuất**(Chủ đề 4) - Lưu hình ảnh và xử lý chỉ mục**Tương tác trên thanh tiến trình:*** **Di chuột** qua thanh để xem bảng thả xuống chi tiết 4 giai đoạn
* **Nhấp xuống** thanh tiến trình để cố định thả bảng xuống chỗ trống
* **Nhấp lại**để mở cố định và ẩn điều khiển bảng**Xử lý thời gian:**

* Nhanh hơn đáng kể so với chế độ miễn phí
* Cân bằng số lượng CPU lõi
* Tăng tốc GPU cải thiện tốt hơn nữa tốc độ

{% hint style="info" %}
**Chloros+**: Tốc độ xử lý bài hát có thể nhanh hơn 5-10 lần nên chế độ tuần tự đối với dữ liệu lớn hơn. Dự án 500 hình ảnh mất 2 giờ ở chế độ miễn phí có thể hoàn thành sau 15-20 phút với Chloros+.
{% endhint %}

***

## Điều gì xảy ra trong quá trình xử lý

### Giai đoạn 1: Phát hiện tiêu điểm

**Chloros làm gì:**

* Quét tiêu điểm hình ảnh được đánh dấu (hoặc tất cả hình ảnh nếu không được đánh dấu)
* Xác định 4 chuẩn bảng hiệu trong mỗi mục tiêu
* Trích xuất các giá trị phản xạ từ bảng tiêu điểm
* Ghi lại tiêu điểm thời gian để thiết lập chuẩn kế hoạch

**Thời lượng:** 1-30 giây (với mục tiêu được đánh dấu), 5-30+ phút (không được đánh dấu)

### Giai đoạn 2: Debayering (Chuyển đổi RAW)

**Chloros làm gì:**

* Chuyển đổi dữ liệu RAW Bayer mẫu thành hình ảnh RGB đầy đủ
* Áp dụng chất lượng cao demosaicing thuật toán
* Bảo toàn chất lượng hình ảnh và chi tiết tối đa

**Thời lượng:** Thay đổi tùy chọn về số lượng hình ảnh và tốc độ CPU

### Giai đoạn 3: Hiệu chuẩn

**Chloros làm gì:*** **Chỉnh sửa chi tiết**: Loại bỏ vết tối ở các cạnh của ống kính
* **Chỉnh sửa mức độ phản xạ**: Chuẩn hóa bằng cách sử dụng các tiêu điểm phản xạ giá trị
* Áp dụng hiệu chỉnh trên tất cả các băng tần/kênh
* Sử dụng hợp lý điều chỉnh tiêu chuẩn cho từng hình ảnh dựa trên dấu thời gian

**Thời lượng:** Xử lý phần lớn thời gian

### Giai đoạn 4: Tính chỉ số

**Chloros làm gì:**

* Tính toán các cấu hình đa số phổ thông (NDVI, NDRE, v.v.)
* Áp dụng phạm vi học cho hình ảnh được hiệu chỉnh
* Tạo chỉ mục ảnh cho từng mục đã chọn

**Thời lượng:** Một vài giây cho mỗi hình ảnh

### Giai đoạn 5: Xuất khẩu

**Chloros làm gì:**

* Lưu hình ảnh đã chỉnh sửa ở dạng đã chọn
* Xuất chỉ mục ảnh với LUT màu được định cấu hình
* Ghi tập tin vào thư mục của máy ảnh mẫu
* Giữ nguyên tệp gốc có hậu tố

**Thời lượng:**Thay đổi định dạng xuất và tệp kích thước tùy chọn***

##Hành vi xử lý

### Tự động xử lý đường ống

Sau khi bắt đầu, toàn bộ quy trình sẽ tự động chạy:

* Người dùng không cần phải tương tác
* Tất cả các cấu hình bước thực hiện theo trình tự
* Cập nhật tiến trình được hiển thị trong thời gian thực

### Sử dụng máy tính trong quá trình xử lý

**Chế độ miễn phí:**

* Use CPU analog (đơn luồng)
* Máy tính vẫn đáp ứng các tác vụ khác
* An toàn để giảm thiểu Chloros và hoạt động trong các ứng dụng khác

**Chloros+ Bài hát Chế độ:**

* Mức sử dụng CPU cao (đa luồng, tối đa 16 lõi)
* Với khả năng tăng tốc GPU: Mức độ sử dụng GPU cao
* Máy tính có thể phản hồi chậm hơn trong quá trình xử lý
* Tránh bắt đầu các tác vụ khác của CPU

{% hint style="warning" %}
**Mẹ về hiệu suất**: Để có hiệu suất Chloros+ tốt nhất, hãy đóng các ứng dụng khác và để Chloros sử dụng toàn bộ hệ thống tài nguyên.
{% endhint %}

### Không thể tạm dừng quá trình xử lý

**Quan trọng ở chế độ giới hạn:**

* Sau khi bắt đầu, quá trình xử lý không thể tạm dừng
* Bạn có thể hủy bỏ quá trình xử lý nhưng quá trình xử lý sẽ bị mất
* Kết quả một phần không được lưu
* Phải khởi động lại từ đầu nếu bị hủy bỏ

**Kế hoạch cài đặt chính:**Đối với các dự án rất lớn, hãy cân nhắc xử lý theo khối lượng hoặc sử dụng CLI để kiểm soát tốt hơn***

## Giám sát trong quá trình xử lý của bạn

Trong khi chạy quá trình xử lý, bạn có thể:

* **Xem tiến trình** - Xem phần trăm hoàn thành tổng thể
* **Xem giai đoạn hiện tại** - Phát hiện, Phân tích, Hiệu chỉnh hoặc Xuất
* **Kiểm tra nhật ký tab** - Xem các thông báo và cảnh báo xử lý chi tiết
* **Xem trước ảnh đã hoàn thành** - Một số tệp xuất có thể xuất hiện trong quá trình xử lý

Để biết thông tin chi tiết về giám sát, hãy xem [Giám sát quá trình xử lý](monitoring-the-processing.md).

***

## Đang hủy xử lý

Nếu bạn cần dừng quá trình xử lý:

### Cách hủy bỏ

1. Xác định vị trí **Nút dừng/Hủy** (thay thế nút Bắt đầu trong quá trình xử lý)
2. Nhấn nút Dừng
3. Quá trình xử lý dừng ngay lập tức
4. Kết quả là một phần bị loại bỏ

### Khi nào nên hủy bỏ

**Lý do hợp lệ để hủy bỏ:**

* Đã nhận được cài đặt không được xác định chính xác
* Bỏ qua tiêu điểm hình ảnh
* Bad input image
* Hệ thống chạy quá chậm hoặc không được phản hồi

**Sau khi hủy bỏ:**

* Xem xét và giải quyết mọi vấn đề
* Điều chỉnh cài đặt khi cần thiết
* Khởi động lại quá trình xử lý từ đầu
* Để có trải nghiệm rõ ràng nhất, hãy đóng hoàn toàn Chloros và khởi động lại

{% hint style="warning" %}
**Không có kết quả nào**: Việc hủy bỏ sẽ loại bỏ tất cả tiến trình. Chloros không lưu ảnh được xử lý một phần.
{% endhint %}

***

## Ước tính thời gian xử lý

Thời gian xử lý thực tế thay đổi rất nhiều dựa trên:

* Hình ảnh số lượng
* Độ phân giải hình ảnh
* Định dạng đầu vào RAW và JPG
* Chế độ xử lý (Miễn phí so với Chloros+)
* CPU tốc độ và số lượng
* Tính năng GPU (chỉ Chloros+)
* Các số chỉ cần tính toán
* Độ phức tạp của định dạng đầu ra

### Ước tính sơ bộ (Chloros+, hình ảnh 12MP, CPU hiện đại)

| Đếm Hình Ảnh | Chế độ miễn phí | Chloros+ (CPU) | Chloros+ (GPU) |
| ----------- | --------- | -------------- | -------------- |
| 50 hình ảnh | 15-20 phút | 5-8 phút | 3-5 phút |
| 100 hình ảnh | 30-40 phút | 10-15 phút | 5-8 phút |
| 200 hình ảnh | 1-1,5 giờ | 20-30 phút | 10-15 phút |
| 500 hình ảnh | 2-3 giờ | 45-60 phút | 20-30 phút |
| 1000 hình ảnh | 4-6 giờ | 1,5-2 giờ | 40-60 phút |

{% hint style="info" %}
**Lần chạy đầu tiên**: Quá trình xử lý ban đầu có thể bị mất nhiều thời gian hơn vì Chloros xây dựng bộ đệm và cấu hình bộ nhớ. Việc xử lý các dữ liệu tương tự sau đó sẽ nhanh hơn.
{% endhint %}

***

## Các vấn đề thường gặp khi bắt đầu

### Nút Bắt đầu được vô hiệu hóa (Chuyển sang màu xám)

**Nguyên nhân có thể:**

* Không thể nhập hình ảnh
* Phần hỗ trợ chưa được bắt đầu đầy đủ
* Quá trình xử lý trước đó vẫn đang chạy
* Dự án chưa được tải đầy đủ

**Giải pháp:**

1. Chờ hoàn thành quá trình khởi động phụ trợ (kiểm tra chính menu biểu tượng)
2. Xác minh hình ảnh được nhập vào File Browser
3. Khởi động lại Chloros nếu nút vẫn bị tắt
4. Kiểm tra lỗi gỡ lỗi nhật ký để biết lỗi thông báo

### Quá trình bắt đầu và thất bại ngay lập tức

**Nguyên nhân có thể:**

* Không có hình ảnh hợp lệ trong dự án
* Hình ảnh tệp bị hỏng
* Dung lượng đĩa không đủ
* Không đủ bộ nhớ (RAM)

**Giải pháp:**

1. Kiểm tra lỗi gỡ lỗi nhật ký <img src="../.gitbook/assets/icon_log.JPG" alt="" data-size="line"> để biết thông báo lỗi
2. Xác minh dung lượng trống của đĩa
3. Thử xử lý một hình ảnh tập hợp nhỏ hơn
4. Xác minh hình ảnh không bị hỏng

### Cảnh báo "Không phát hiện được tiêu điểm"

**Nguyên nhân có thể:**

* Bỏ qua tiêu điểm hình ảnh
* Tiêu điểm hình ảnh không chứa màn hình tiêu điểm
* Cài đặt tiêu điểm quá nghiêm ngặt

**Giải pháp:**

1. Xem lại [Chọn mục ảnh](choosing-target-images.md)
2. Đánh dấu hình ảnh phù hợp vào cột Target
3. Xác định tiêu điểm hiển thị trong hình ảnh được đánh dấu
4. Điều chỉnh cài đặt tiêu điểm nếu cần

***

## Mẹo để xử lý thành công

### Trước khi bắt đầu

1. **Thử nghiệm với tập hợp nhỏ trước** - Xử lý 10-20 hình ảnh để xác minh cài đặt
2. **Kiểm tra dung lượng trống của ổ đĩa** - Đảm bảo miễn phí kích thước tệp dữ liệu 2-3x
3. **Đóng các ứng dụng không cần thiết** - Giải thích hệ thống tài nguyên phóng khoáng
4. **Xác minh hình ảnh mục tiêu** - Xem trước mục tiêu được đánh dấu để đảm bảo chất lượng
5. **Lưu dự án** - Dự án tự động lưu nhưng cách tốt nhất là lưu thủ công

### Trong quá trình xử lý

1. **Chế độ ngủ của hệ thống** - Tắt chế độ tiết kiệm năng lượng
2. **Giữ Chloros ở nền trước** - hoặc ít nhất là hiển thị trên thanh tác vụ
3. **Thỉnh thoảng theo dõi tiến trình** - Kiểm tra cảnh báo hoặc lỗi
4. **Không tải các ứng dụng nặng khác** - Đặc biệt với bài hát chế độ Chloros+

### Tăng tốc GPU Chloros+

Nếu sử dụng GPU NVIDIA tăng tốc độ:

1. Cập nhật trình điều khiển NVIDIA lên phiên bản mới nhất
2. Đảm bảo GPU có 4GB+ VRAM
3. Đóng các ứng dụng sử dụng nhiều GPU (game, edit video)
4. Theo dõi nhiệt độ GPU (đảm bảo làm mát đầy đủ)

***

## Các bước tiếp theo

Khi quá trình xử lý đã bắt đầu:

1. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)
2. **Hoàn thành** - Quá trình xử lý diễn đàn tự động
3. **Xem lại kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)

Để biết thông tin về những công việc cần làm trong quá trình xử lý, hãy xem [Giám sát quá trình xử lý](monitoring-the-processing.md).