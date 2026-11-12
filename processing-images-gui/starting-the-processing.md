# Bắt đầu quá trình xử lý

Sau khi đã nhập hình ảnh, đánh dấu các mục tiêu hiệu chuẩn và cấu hình cài đặt dự án, bạn đã sẵn sàng bắt đầu quá trình xử lý. Trang này sẽ hướng dẫn bạn cách khởi chạy quy trình xử lý Chloros.

## Danh sách kiểm tra trước khi xử lý

Trước khi nhấp vào nút Bắt đầu, hãy kiểm tra xem mọi thứ đã sẵn sàng chưa:

* [ ] **Tệp đã được nhập** - Tất cả hình ảnh đều xuất hiện trong Trình duyệt tệp
* [ ] **Hình ảnh mục tiêu đã được đánh dấu** - Cột Mục tiêu đã được kiểm tra đối với hình ảnh hiệu chuẩn (hoặc tệp ghi `.daq` đã được nhập cho LATTICE)
* [ ] **Đã phát hiện các mẫu máy ảnh** - Cột "Mẫu máy ảnh" hiển thị đúng các máy ảnh
* [ ] **Đã cấu hình cài đặt** - Đã xem xét và điều chỉnh Cài đặt dự án
* [ ] **Đã chọn các chỉ số** - Đã thêm các chỉ số đa phổ mong muốn (nếu cần)
* [ ] **Định dạng xuất đã được chọn** - Định dạng đầu ra phù hợp với quy trình làm việc của bạn

{% hint style="info" %}
**Mẹo**: Nhấp qua một vài hình ảnh trong Trình duyệt tệp để xác minh rằng chúng đã được tải đúng cách trước khi xử lý.
{% endhint %}

***

## Bắt đầu xử lý

### Tìm nút Bắt đầu

Nút Bắt đầu/Phát nằm ở thanh tiêu đề trên cùng của Chloros:

* Vị trí: Giữa phía trên cửa sổ
* Biểu tượng: **Nút Phát/Bắt đầu** <img src="../.gitbook/assets/image (2) (1) (1).png" alt="" data-size="line">
* Trạng thái: Nút được kích hoạt (sáng) khi sẵn sàng xử lý

### Nhấp để bắt đầu

1. Nhấp vào **nút Phát/Bắt đầu** ở thanh tiêu đề trên cùng
2. Quá trình xử lý bắt đầu ngay lập tức
3. Nút này sẽ chuyển thành nút **Dừng** trong quá trình xử lý
4. Thanh tiến trình được cập nhật, hiển thị trạng thái xử lý

{% hint style="success" %}
**Đã bắt đầu xử lý**: Sau khi nhấp, Chloros sẽ tự động xử lý tất cả các bước - phát hiện mục tiêu, loại bỏ hiệu ứng Bayer, hiệu chỉnh, tính toán chỉ số và xuất dữ liệu. Phần mềm tự động nhận diện dự án của bạn là Survey3, LATTICE hay kết hợp cả hai, và áp dụng quy trình xử lý phù hợp cho từng camera.
{% endhint %}

***

## Hiểu về các chế độ xử lý

Chloros hoạt động ở hai chế độ xử lý khác nhau tùy thuộc vào giấy phép của bạn:

### Chế độ Miễn phí (Xử lý tuần tự)

**Dành cho tất cả người dùng**

**Cách thức hoạt động:**

* Xử lý hình ảnh từng tấm một, theo thứ tự
* Hoạt động trên một luồng duy nhất
* Tiêu thụ bộ nhớ thấp hơn

**Thanh tiến trình hiển thị 2 giai đoạn:**

1.**Phát hiện mục tiêu** - Quét để tìm các mục tiêu hiệu chuẩn
2. **Xử lý** - Áp dụng hiệu chuẩn và xuất hình ảnh**Thời gian xử lý:**

* Chậm hơn nhiều so với chế độ song song củChloros+
* Phù hợp với tập dữ liệu nhỏ đến trung bình (< 200 hình ảnh)

### Chế độ Chloros+ (Xử lý song song)

**Yêu cầu giấy phép Chloros+**

**Cách thức hoạt động:**

* Xử lý nhiều hình ảnh đồng thời bằng [đường ống xử lý 4 luồng](../processing-architecture/processing-pipeline.md)
* [Tự động điều chỉnh tính toán động](../processing-architecture/dynamic-compute-adaptation.md) tự động chọn chiến lược tối ưu cho phần cứng của bạn ngay khi bắt đầu chạy
* Tăng tốc GPU (CUDA) với card đồ họa NVIDIA
* **Số lượng worker thích ứng với phần cứng**: Các chiến lược GPU chạy**2–4 worker đồng thời** (tùy thuộc vào dung lượng VRAM — 3 trên GPU 2–12GB, 2–4 trên GPU 12GB trở lên); Các hệ thống chỉ dùng CPU chạy một worker cho mỗi lõi vật lý, trừ đi một**Thanh tiến trình hiển thị 4 giai đoạn** (tương ứng với 4 luồng trong đường ống xử lý):

1. **Phát hiện** (Luồng 1) - Tìm các mục tiêu hiệu chuẩn
2. **Phân tích** (Luồng 2) - Kiểm tra siêu dữ liệu hình ảnh và tính toán hiệu chuẩn
3. **Hiệu chuẩn** (Luồng 3) - Loại bỏ hiệu ứng bayering, chỉnh sửa hiệu ứng vignette, hiệu chuẩn, tính toán chỉ số
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

* Quét các hình ảnh mà bạn đã chọn trong cột “Mục tiêu” (nếu không chọn hình ảnh nào thì sẽ quét tất cả hình ảnh)
* Xác định các bảng hiệu chuẩn trong mỗi mục tiêu
* Trích xuất các giá trị phản xạ từ các bảng mục tiêu
* Ghi lại dấu thời gian của mục tiêu để lập lịch hiệu chuẩn

**Thời gian thực hiện:** 1–30 giây (với các mục tiêu đã được đánh dấu), 5–30+ phút (không đánh dấu)

### Giai đoạn 2: Loại bỏ hiệu ứng Bayer (Chuyển đổi RAW)

**Chloros thực hiện những gì:**

* Chuyển đổi dữ liệu mẫu Bayer định dạng RAW thành hình ảnh 3 kênh đầy đủ (các mô-đun đơn sắc LATTICE vẫn giữ nguyên dải đơn — quá trình loại bỏ mẫu Bayer sẽ bị bỏ qua đối với chúng và có ghi chú trong nhật ký)
* Áp dụng thuật toán loại bỏ mẫu Bayer đã chọn
* Giữ nguyên chất lượng hình ảnh và chi tiết tối đa

**Thời gian thực hiện:** Thay đổi tùy theo số lượng hình ảnh và tốc độ CPU/GPU

### Giai đoạn 3: Hiệu chuẩn

**Chloros thực hiện những gì:*** **Chỉnh sửa hiện tượng tối góc**: Loại bỏ hiện tượng tối góc do ống kính gây ra
* **Hiệu chỉnh độ phản xạ**: Chuẩn hóa bằng cách sử dụng các giá trị độ phản xạ mục tiêu và/hoặc dữ liệu ánh sáng chiếu xuống từ hệ thống thu thập dữ liệu (DAQ)
* Áp dụng các hiệu chỉnh trên tất cả các dải/kênh
* Sử dụng tham chiếu hiệu chuẩn phù hợp cho từng hình ảnh dựa trên dấu thời gian

**Thời gian thực hiện:** Chiếm phần lớn thời gian xử lý

### Giai đoạn 4: Tính toán chỉ số

**Chloros thực hiện những gì:**

* Tính toán các chỉ số đa phổ đã cấu hình (NDVI, NDRE, v.v.)
* Áp dụng phép toán dải phổ lên các hình ảnh đã hiệu chuẩn
* Tạo hình ảnh chỉ số cho từng chỉ số đã chọn

**Thời gian thực hiện:** Vài giây cho mỗi hình ảnh

### Giai đoạn 5: Xuất dữ liệu

**Chloros thực hiện những gì:**

* Lưu các hình ảnh đã xử lý ở định dạng đã chọn
* **LATTICE fan-out**: mỗi khung hình thô LATTICE được xuất dưới dạng tất cả các sản phẩm đã kích hoạt trong một lần xử lý — đã loại bỏ hiệu ứng Bayer, bản xem trước, độ sáng (luôn là float32), độ phản xạ
* Ghi tệp vào thư mục đầu ra của dự án: `<project>/<camera>/<format>/<Product>_Images/`
* **Giữ nguyên tên tệp nguồn** — thư mục xác định sản phẩm, không thêm hậu tố**Thời gian thực hiện:** Thay đổi tùy theo định dạng xuất và kích thước tệp***

## Hành vi xử lý

### Quy trình xử lý tự động

Sau khi bắt đầu, toàn bộ quy trình sẽ chạy tự động:

* Không cần tương tác từ người dùng
* Tất cả các bước đã cấu hình được thực thi theo thứ tự
* Cập nhật tiến trình được hiển thị theo thời gian thực
* Các tệp xuất ra được ghi vào đĩa ngay khi hoàn thành — bạn có thể mở các tệp đầu ra đã hoàn tất trong khi quy trình vẫn đang chạy

### Sử dụng tài nguyên máy tính trong quá trình xử lý

**Chế độ Miễn phí:**

* Mức sử dụng CPU tương đối thấp (chế độ một luồng)
* Máy tính vẫn phản hồi tốt cho các tác vụ khác
* An toàn khi thu nhỏ cửa sổ Chloros và làm việc trên các ứng dụng khác

**Chế độ song song Chloros+:**

* Mức sử dụng CPU cao trên toàn bộ nhóm công nhân của chiến lược
* Khi có tăng tốc GPU: Mức sử dụng GPU cao
* Máy tính có thể phản hồi chậm hơn trong quá trình xử lý
* Tránh khởi chạy các tác vụ tiêu tốn nhiều CPU khác

{% hint style="warning" %}
**Mẹo tối ưu hiệu suất**: Để đạt hiệu suất tốt nhất cho Chloros+, hãy đóng các ứng dụng khác và để Chloros sử dụng toàn bộ tài nguyên hệ thống.
{% endhint %}

### Quá trình xử lý không thể tạm dừng (nhưng việc dừng lại diễn ra trơn tru)

* Sau khi bắt đầu, quá trình xử lý không thể tạm dừng và tiếp tục sau đó
* Nhấp vào **Dừng** sẽ dừng quá trình chạy một cách triệt để ngay từ lần nhấp đầu tiên
* Các sản phẩm đã được xuất trước khi dừng vẫn được lưu trên đĩa
* Một quá trình đã dừng sẽ báo cáo chính xác những gì đã hoàn thành (xem các dòng `[RUN-SUMMARY]` trong nhật ký)
* Một lần chạy mới sẽ khởi động lại quy trình từ đầu

**Mẹo lập kế hoạch:** Đối với các dự án rất lớn, hãy cân nhắc xử lý theo lô hoặc sử dụng CLI để kiểm soát tốt hơn.

***

## Theo dõi quá trình xử lý

Trong khi quá trình xử lý đang diễn ra, bạn có thể:

* **Theo dõi thanh tiến trình** - Xem tỷ lệ hoàn thành tổng thể
* **Xem giai đoạn hiện tại** - Phát hiện, Phân tích, Hiệu chuẩn hoặc Xuất
* **Kiểm tra tab nhật ký** - Xem các thông báo và cảnh báo chi tiết về quá trình xử lý
* **Xem trước các hình ảnh đã hoàn thành** - Các tệp xuất sẽ xuất hiện trên đĩa trong quá trình xử lý

Để biết thông tin chi tiết về việc theo dõi, hãy xem [Theo dõi quá trình xử lý](monitoring-the-processing.md).

***

## Dừng quá trình xử lý

Nếu bạn cần dừng quá trình xử lý:

### Cách dừng

1. Tìm **nút Dừng** (thay thế nút Bắt đầu trong quá trình xử lý)
2. Nhấp vào nút này một lần — thanh tiến trình sẽ hiển thị **"Đang dừng..."** trong khi hình ảnh đang xử lý hoàn tất
3. Quá trình xử lý kết thúc ở trạng thái dừng hoàn toàn và nhật ký sẽ ghi lại thông tin chi tiết về những gì đã hoàn thành

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

Thời gian xử lý thực tế thay đổi rất nhiều tùy thuộc vào:

* Số lượng hình ảnh
* Độ phân giải hình ảnh
* Định dạng đầu vào RAW so với JPG
* Chế độ xử lý (Miễn phí so với Chloros+)
* Tốc độ CPU và số nhân
* Tình trạng sẵn có của GPU (chỉ áp dụng cho Chloros+)
* Số lượng chỉ số cần tính toán
* Số lượng sản phẩm xuất được kích hoạt (LATTICE)

### Ước tính sơ bộ (Chloros+, hình ảnh 12MP, CPU hiện đại)

| Số lượng ảnh | Chế độ Miễn phí | Chloros+ (CPU) | Chloros+ (GPU) |
| ----------- | --------- | -------------- | -------------- |
| 50 hình ảnh   | 15–20 phút | 5–8 phút        | 3–5 phút        |
| 100 hình ảnh  | 30–40 phút | 10–15 phút      | 5–8 phút        |
| 200 hình ảnh  | 1–1,5 giờ | 20–30 phút      | 10–15 phút      |
| 500 hình ảnh  | 2–3 giờ   | 45–60 phút      | 20–30 phút      |
| 1.000 hình ảnh | 4–6 giờ   | 1,5–2 giờ      | 40–60 phút      |

{% hint style="info" %}
**Lần chạy đầu tiên**: Quá trình xử lý ban đầu có thể mất nhiều thời gian hơn do Chloros đang xây dựng bộ nhớ đệm và hồ sơ. Các lần xử lý tiếp theo đối với các tập dữ liệu tương tự sẽ nhanh hơn.
{% endhint %}

***

## Các sự cố thường gặp khi bắt đầu

### Nút Bắt đầu bị vô hiệu hóa (màu xám)

**Nguyên nhân có thể:**

* Chưa nhập hình ảnh
* Hệ thống nền chưa khởi động hoàn toàn
* Quá trình xử lý trước đó vẫn đang chạy
* Dự án chưa được tải hoàn toàn

**Giải pháp:**

1. Chờ cho đến khi hệ thống nền được khởi tạo hoàn toàn (kiểm tra biểu tượng trên menu chính)
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

### Quá trình chạy kết thúc nhưng không ghi ra hình ảnh nào

Một lần chạy yêu cầu sản phẩm hình ảnh nhưng không ghi ra sản phẩm nào sẽ được coi là **thất bại, không phải thành công** — Chloros sẽ báo lỗi rõ ràng:

* Nhật ký GUI hiển thị các mã lỗi `[RUN-SUMMARY]` gợi ý nguyên nhân có thể — không có hình ảnh nào được nhập, không phát hiện được mục tiêu, hoặc mọi sản phẩm được yêu cầu đều bị bỏ qua vì không áp dụng được (ví dụ: yêu cầu dữ liệu radiance/reflectance từ camera chỉ hỗ trợ RGB)
* Phiên bản tương đương trên giao diện dòng lệnh (CLI) (`chloros-cli process`) sẽ hiển thị thông báo `Processing finished but wrote no image products.` và **thoát với giá trị không bằng 0**, do đó các skript có thể phát hiện được lỗi này
* Một phiên chạy chỉ chứa metadata (tất cả sản phẩm xuất đều bị vô hiệu hóa, không có chỉ mục) vẫn được coi là thành công

Xem [Tài liệu tham khảo CLI](../reference/cli-reference.md#a-run-that-writes-no-images-fails) để biết chi tiết đầy đủ.

### Thư mục dự án quá sâu đối với máy tính này

**Chỉ áp dụng cho Windows, tính năng mới trong phiên bản 1.3.0.** Windows được cài đặt sẵn với tính năng đường dẫn dài bị tắt, do đó đường dẫn tệp không được dài hơn 260 ký tự. Các sản phẩm Chloros nằm cách thư mục dự án khoảng 130 ký tự, do đó thư mục dự án nằm sâu hơn mức đó sẽ không thể nhận được các sản phẩm của nó. Thay vì chạy mà không ghi lại gì, Chloros giờ đây sẽ kiểm tra trước khi bắt đầu xử lý:

* Nhấp vào Start sẽ không khởi chạy: thanh tiến trình hiển thị lý do, và Nhật ký gỡ lỗi hiển thị mã lỗi `[PROCESS] refused: The project folder is too deep for this computer…`. Thông báo này cung cấp độ dài của thư mục dự án và thư mục dự án dài nhất có thể chứa được.
* Việc tạo hoặc mở dự án trong một thư mục quá sâu, ngay cả đối với các tệp của chính dự án đó, cũng bị từ chối. Một hộp thông báo sẽ giải thích lý do, kèm theo tên thư mục dự án dài nhất phù hợp và cách bật tính năng đường dẫn dài, đồng thời không có tệp nào được tạo hoặc mở; Nhật ký gỡ lỗi ghi lại sự cố này với mã lỗi `[PROJECT] refused: …`. Việc sao chép một dự án dưới tên dài hơn cũng có thể vượt quá giới hạn: bản sao được tạo ra nhưng không được mở, và cùng một thông báo sẽ giải thích lý do.

**Giải pháp:**

1. Lưu trữ các dự án trong thư mục có tên ngắn hơn — thay đổi**Thư mục làm việc** trong [Cài đặt Dự án](../project-settings/project-settings.md) và tạo lại dự án, hoặc
2. Bật đường dẫn dài của Windows và khởi động lại Chloros: chạy lại trình cài đặt Chloros với tùy chọn **Enable Windows long paths** được chọn (tùy chọn này được chọn mặc định), hoặc sử dụng Chính sách Nhóm: *Cấu hình Máy tính > Mẫu Quản trị > Hệ thống > Hệ thống Tệp > Bật đường dẫn dài Win32*.

### Cảnh báo “Không phát hiện mục tiêu”

**Nguyên nhân có thể:**

* Quên đánh dấu hình ảnh mục tiêu
* Hình ảnh mục tiêu không chứa các mục tiêu hiển thị
* Cài đặt phát hiện mục tiêu quá nghiêm ngặt

**Giải pháp:**

1. Xem lại [Chọn hình ảnh mục tiêu](choosing-target-images.md)
2. Đánh dấu các hình ảnh phù hợp trong cột "Mục tiêu"
3. Kiểm tra xem các mục tiêu có hiển thị trong các hình ảnh đã đánh dấu hay không
4. Điều chỉnh cài đặt phát hiện mục tiêu nếu cần

***

## Mẹo để xử lý thành công

### Trước khi bắt đầu

1. **Thử nghiệm với một tập con nhỏ trước** - Xử lý 10-20 hình ảnh để xác minh cài đặt
2. **Kiểm tra dung lượng đĩa trống** - Đảm bảo có dung lượng trống gấp 2-3 lần kích thước tập dữ liệu (nhiều hơn nếu tất cả các sản phẩm LATTICE đều được kích hoạt)
3. **Đóng các ứng dụng không cần thiết** - Giải phóng tài nguyên hệ thống
4. **Kiểm tra hình ảnh mục tiêu** - Xem trước các mục tiêu đã đánh dấu để đảm bảo chất lượng
5. **Lưu dự án** - Dự án sẽ tự động lưu, nhưng nên lưu thủ công để đảm bảo an toàn

### Trong quá trình xử lý

1. **Tránh để hệ thống chuyển sang chế độ ngủ** - Tắt các chế độ tiết kiệm năng lượng
2. **Giữ Chloros ở chế độ nền trước** - Hoặc ít nhất là hiển thị trên thanh tác vụ
3. **Theo dõi tiến trình định kỳ** - Kiểm tra các cảnh báo hoặc lỗi
4. **Không mở các ứng dụng nặng khác** - Đặc biệt khi sử dụng chế độ song song của Chloros+

### Tăng tốc GPU củChloros+

Nếu sử dụng tăng tốc GPU của NVIDIA:

1. Cập nhật trình điều khiển NVIDIA lên phiên bản mới nhất
2. Đảm bảo GPU có ít nhất 4GB VRAM (7GB trở lên cho quá trình debayering Texture Aware đồng thời)
3. Đóng các ứng dụng tiêu tốn nhiều tài nguyên GPU (trò chơi, chỉnh sửa video)
4. Theo dõi nhiệt độ GPU (đảm bảo hệ thống làm mát đủ tốt)

***

## Các bước tiếp theo

Khi quá trình xử lý đã bắt đầu:

1. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)
2. **Chờ đến khi hoàn tất** - Quá trình xử lý diễn ra tự động
3. **Xem lại kết quả** - Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md)

Để biết thông tin về những việc cần làm trong quá trình xử lý, hãy xem [Theo dõi quá trình xử lý](monitoring-the-processing.md).
