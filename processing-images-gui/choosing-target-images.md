# Chọn hình ảnh mục tiêu

Việc đánh dấu những hình ảnh nào chứa mục tiêu hiệu chuẩn sẽ giúp Chloros xác định chính xác vị trí cần tìm kiếm chúng. Khi có ít nhất một hình ảnh được chọn trong cột “Target”, Chloros sẽ quét **chỉ những hình ảnh đã được chọn** — do đó, việc đánh dấu mục tiêu vừa giúp tăng tốc quá trình xử lý, vừa ngăn chặn việc hình ảnh khảo sát bị nhầm lẫn với mục tiêu.

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

## Tại sao nên đánh dấu hình ảnh mục tiêu?

### Việc đánh dấu kiểm soát quá trình quét

Khi bạn đánh dấu cột “Mục tiêu” cho các hình ảnh cụ thể:

* Chloros chỉ quét các hình ảnh đã được đánh dấu để tìm mục tiêu
* Quá trình phát hiện mục tiêu hoàn thành nhanh hơn nhiều
* Hình ảnh khảo sát không thể gây ra các phát hiện mục tiêu sai

Nếu **không** có hình ảnh nào được đánh dấu, Chloros sẽ quay lại quét mọi hình ảnh trong dự án:

* Các thuật toán phát hiện mục tiêu sẽ chạy trên từng hình ảnh
* Hàng trăm hoặc hàng nghìn hình ảnh bị kiểm tra một cách không cần thiết
* Quá trình xử lý mất nhiều thời gian hơn đáng kể, đặc biệt đối với các tập dữ liệu lớn

{% hint style="success" %}
**Cải thiện tốc độ**: Đánh dấu 2-3 hình ảnh mục tiêu trong bộ dữ liệu gồm 500 hình ảnh có thể giảm thời gian phát hiện mục tiêu từ hơn 30 phút xuống dưới 1 phút.
{% endhint %}

***

## Cách đánh dấu hình ảnh mục tiêu

### Bước 1: Xác định hình ảnh mục tiêu của bạn

Xem qua các hình ảnh đã nhập trong Trình duyệt tệp và xác định những hình ảnh nào chứa mục tiêu hiệu chuẩn.

**Các tình huống phổ biến:*** **Mục tiêu trước khi chụp**: Được chụp trước khi bắt đầu phiên chụp
* **Mục tiêu sau khi chụp**: Được chụp sau khi hoàn thành phiên chụp
* **Mục tiêu tại hiện trường**: Các mục tiêu được đặt trong khu vực chụp
* **Nhiều mục tiêu**: 2-3 hình ảnh mục tiêu mỗi phiên (được khuyến nghị)

### Bước 2: Kiểm tra cột **Target** <img src="../.gitbook/assets/image (33).png" alt="" data-size="original">

Đối với mỗi hình ảnh chứa mục tiêu hiệu chuẩn:

1. Tìm hình ảnh trong bảng Trình duyệt tệp
2. Tìm cột **Target** (cột ngoài cùng bên phải)
3. Nhấp vào ô chọn trong cột **Target** cho hình ảnh đó
4. Lặp lại cho tất cả các hình ảnh chứa mục tiêu

### Bước 3: Xác minh lựa chọn của bạn

Trước khi xử lý, hãy kiểm tra lại:

* [ ] Tất cả các hình ảnh có mục tiêu hiệu chuẩn đều đã được chọn
* [ ] Không có hình ảnh không phải mục tiêu nào bị chọn nhầm
* [ ] Các mục tiêu hiển thị rõ ràng trong các hình ảnh đã chọn

***

## LATTICE: Mục tiêu là tùy chọn khi DAQ đang ghi hình

Đối với camera đa quang phổ LATTICE, mục tiêu hiệu chuẩn trong khung hình là **một trong hai** tham chiếu phản xạ khả thi:

* **Mục tiêu trong khung hình**: khi một hình ảnh mục tiêu được đánh dấu vượt qua các tiêu chuẩn chất lượng (QA) củChloros, mục tiêu đó sẽ trở thành**mức tham chiếu phản xạ tuyệt đối** cho các hình ảnh xung quanh nó.
* **Tia sáng chiếu xuống từ DAQ**: khi không có mục tiêu (hoặc không đạt tiêu chuẩn QA), Chloros sẽ tính toán độ phản xạ dựa trên cường độ bức xạ chiếu xuống từ cảm biến ánh sáng của DAQ (ρ = π·L/E). Nếu bản ghi `.daq` hoặc DAQ-M `.csv` bao phủ các hình ảnh bạn thu được, bạn sẽ nhận được độ phản xạ đã hiệu chuẩn**mà không cần bất kỳ hình ảnh mục tiêu nào cả**.

Hành vi tự động này là mặc định. Trong tệp tham chiếu &quot;CLI&quot; / &quot;SDK&quot;, điều này tương ứng với `--reflectance-source auto`; bạn cũng có thể ép sử dụng `target` (chế độ nghiêm ngặt — không thay thế bằng DAQ) hoặc `daq` (chế độ ưu tiên DAQ). Xem [CLI Tài liệu tham khảo](../reference/cli-reference.md#per-product-export-toggles-lattice-multispectral).

**Hình dạng mục tiêu LATTICE**: ngoài phương pháp phát hiện bảng truyền thống được sử dụng cho Survey3, quy trình xử lý LATTICE hỗ trợ**mục tiêu được đánh dấu ArUco**,**mục tiêu vùng quan tâm cố định (ROI)**và**mục tiêu dạng dải**, được cấu hình theo từng dự án. Các bản quét độ phản xạ**đo được** của mục tiêu cho từng đơn vị có thể được cung cấp theo số sê-ri (CLI: `--target-reflectance-dir`, một `<serial>.csv` cho mỗi đơn vị mục tiêu), với phổ T3/T4P danh nghĩa làm phương án dự phòng.

{% hint style="info" %}
**Mô-đun F988**: Độ phản xạ của F988 được hiệu chuẩn bằng bảng phản xạ tại hiện trường: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros sẽ áp dụng dữ liệu chụp bảng phản xạ mới nhất của bạn và giữ nguyên giá trị đó giữa các lần quan sát bảng. Nếu mô-đun F988 được xử lý chỉ bằng DAQ, Chloros sẽ từ chối độ phản xạ dựa trên DAQ cho dải đó (lý do bỏ qua: `dls-uncalibrated-band-988`) — quy trình làm việc với bảng đo là phương án được hỗ trợ.
{% endhint %}

***

## Các thực hành tốt nhất cho hình ảnh mục tiêu

### Hướng dẫn chụp hình ảnh mục tiêu

**Thời điểm:**

* Chụp hình ảnh mục tiêu ngay trước và trong suốt phiên chụp
* Trong cùng điều kiện ánh sáng với cảm biến ánh sáng DAQ
* Tốt nhất nên chụp hình ảnh mục tiêu thường xuyên nhất có thể để đạt kết quả tốt nhất. Nếu không, dữ liệu từ cảm biến ánh sáng sẽ được sử dụng để điều chỉnh hiệu chuẩn theo thời gian.

**Vị trí máy ảnh:**

* Giữ máy ảnh ở vị trí cao hơn mục tiêu sao cho mục tiêu nằm chính giữa và chiếm khoảng 40–60% diện tích trung tâm của hình ảnh.
* Giữ máy ảnh song song hoặc vuông góc với bề mặt mục tiêu

**Ánh sáng:**

* Ánh sáng môi trường xung quanh phải giống với cảm biến ánh sáng DAQ của bạn
* Tránh tạo bóng trên bề mặt mục tiêu
* Không che khuất nguồn sáng bằng cơ thể, phương tiện hoặc cây cối
* Điều kiện trời nhiều mây mang lại kết quả ổn định nhất

**Tình trạng mục tiêu:**

* Giữ các tấm mục tiêu sạch sẽ và khô ráo
* Tất cả các tấm của mục tiêu (ví dụ: cả 4 tấm trên T4) phải nhìn rõ và không bị che khuất
* Đặt mục tiêu vuông góc/hướng thẳng xuống nguồn sáng nếu có thể

### Cần bao nhiêu hình ảnh mục tiêu?

**Tối thiểu:**1 hình ảnh mục tiêu mỗi phiên.**Được khuyến nghị:** 3–5 hình ảnh mục tiêu mỗi phiên.**Lịch trình thực hiện tốt nhất:**

* Chụp 3–5 hình ảnh ngay sau khi cảm biến ánh sáng bắt đầu ghi hình
* Xoay camera giữa các lần chụp để đạt kết quả tốt nhất
* Tùy chọn: định kỳ giữa phiên nếu điều kiện ánh sáng thay đổi liên tục

***

## Làm việc với nhiều máy ảnh

### Cấu hình hai máy ảnh

Nếu sử dụng đồng thời hai máy ảnh MAPIR (ví dụ: Survey3W RGN + Survey3N OCN):

1. Chụp hình ảnh mục tiêu bằng **cả hai máy ảnh** cùng lúc
2. Sử dụng **cùng một mục tiêu vật lý** cho cả hai camera
3. Đánh dấu hình ảnh mục tiêu cho **cả hai loại camera** trong Trình duyệt tệp
4. Chloros sẽ sử dụng các mục tiêu phù hợp để hiệu chuẩn từng camera

### Cột Mẫu camera

Cột **Mẫu camera** giúp xác định hình ảnh nào đến từ camera nào:

* Survey3W\_RGN
* Survey3N\_OCN
* LATT-M3M-L41-F550
* LATT-M3C-L87-FRGN
* v.v.

Sử dụng cột này để xác minh rằng bạn đã đánh dấu mục tiêu cho từng loại camera trong dự án của mình.

***

## Cài đặt phát hiện mục tiêu

### Điều chỉnh độ nhạy phát hiện

Nếu Chloros không phát hiện chính xác các mục tiêu của bạn, hãy điều chỉnh các cài đặt này trong [Cài đặt dự án](adjusting-project-settings.md):**Diện tích mẫu hiệu chuẩn tối thiểu (px):*** **Mặc định**: 25 pixel
* **Tăng** nếu xuất hiện các phát hiện sai trên các chi tiết nhỏ
* **Giảm** nếu mục tiêu không được phát hiện**Mức nhóm mục tiêu tối thiểu (0-100):*** **Mặc định**: 60
* **Tăng** nếu các mục tiêu bị chia thành nhiều phát hiện riêng biệt
* **Giảm** nếu các mục tiêu có sự biến đổi màu sắc không được phát hiện đầy đủ

{% hint style="info" %}
**Mẹo về CLI**: `chloros-cli process` hỗ trợ các thông số điều chỉnh tương tự (`--min-target-size`, `--target-clustering`), và cờ `--target`/`--targets` của nó đánh dấu toàn bộ thư mục đầu vào là chỉ dành cho bảng mục tiêu. Xem [Tài liệu tham khảo CLI](../reference/cli-reference.md).
{% endhint %}

***

## Các vấn đề thường gặp với hình ảnh mục tiêu

### Vấn đề: Không phát hiện được mục tiêu

**Nguyên nhân có thể:**

* Hình ảnh mục tiêu không được đánh dấu trong Trình duyệt tệp
* Mục tiêu quá nhỏ trong khung hình (&lt; 30% hình ảnh)
* Ánh sáng kém (bóng tối, lóa sáng)
* Cài đặt phát hiện mục tiêu quá nghiêm ngặt

**Giải pháp:**

1. Kiểm tra xem cột Mục tiêu đã được chọn cho các hình ảnh chính xác chưa
2. Kiểm tra chất lượng hình ảnh mục tiêu trong chế độ xem trước
3. Chụp lại các mục tiêu nếu chất lượng kém
4. Điều chỉnh cài đặt phát hiện mục tiêu nếu cần thiết

### Vấn đề: Phát hiện mục tiêu sai

**Nguyên nhân có thể:**

* Các tòa nhà, phương tiện hoặc lớp phủ mặt đất màu trắng bị nhầm lẫn với mục tiêu
* Các vùng sáng trong thảm thực vật
* Độ nhạy phát hiện quá thấp

**Giải pháp:**

1. Chỉ đánh dấu các hình ảnh mục tiêu thực sự — chỉ các hình ảnh được đánh dấu mới được quét
2. Tăng diện tích mẫu hiệu chuẩn tối thiểu
3. Tăng giá trị nhóm mục tiêu tối thiểu
4. Đảm bảo hình ảnh mục tiêu chỉ hiển thị mục tiêu (ít nhiễu nền nhất có thể)

***

## Danh sách kiểm tra xác minh

Trước khi bắt đầu xử lý, hãy xác minh việc lựa chọn hình ảnh mục tiêu của bạn:

* [ ] Ít nhất 1 hình ảnh mục tiêu được đánh dấu cho mỗi phiên (hoặc, đối với LATTICE, một bản ghi `.daq`/`.csv` bao phủ toàn bộ phiên)
* [ ] Các ô chọn cột mục tiêu đã được đánh dấu cho tất cả các hình ảnh mục tiêu
* [ ] Các hình ảnh mục tiêu được chụp trong cùng khoảng thời gian với cuộc khảo sát
* [ ] Các mục tiêu hiển thị rõ ràng trong chế độ xem trước khi nhấp vào
* [ ] Tất cả các bảng hiệu chuẩn đều hiển thị trong mỗi hình ảnh mục tiêu
* [ ] Không có bóng hoặc vật cản trên các mục tiêu
* [ ] Đối với hệ thống hai camera: Các mục tiêu đã được đánh dấu cho cả hai loại camera

***

## Xử lý không cần mục tiêu

### LATTICE: Sử dụng bản ghi DAQ

Nếu cảm biến ánh sáng DAQ đã ghi lại cường độ bức xạ xuống trong quá trình thu thập dữ liệu LATTICE của bạn, thì không cần mục tiêu:

1. Nhập tệp `.daq` (hoặc DAQ-M `.csv`) chứa hình ảnh
2. Để trống cột Mục tiêu
3. Hệ số phản xạ được tính toán tự động từ giá trị tham chiếu bức xạ xuống của DAQ
4. Độ rạng rỡ (Radiance) không bao giờ cần mục tiêu hay DAQ — nó được xác định hoàn toàn dựa trên hiệu chuẩn phóng xạ gốc của camera

### Xử lý mà không cần bất kỳ tham chiếu nào

Bạn cũng có thể xử lý mà không cần mục tiêu và không cần DAQ:

1. Để tất cả các ô chọn trong cột &quot;Mục tiêu&quot; ở trạng thái không được chọn
2. **Tắt** tùy chọn &quot;Hiệu chuẩn độ phản xạ / cân bằng trắng&quot; trong Cài đặt Dự án — quá trình phát hiện mục tiêu sẽ bị bỏ qua hoàn toàn
3. Chức năng chỉnh sửa hiệu ứng viền tối vẫn được áp dụng
4. Kết quả đầu ra sẽ không được hiệu chuẩn cho độ phản xạ tuyệt đối (LATTICE đa phổ vẫn xuất các sản phẩm đã loại bỏ hiệu ứng Bayer, bản xem trước và sản phẩm độ rạng rỡ)

{% hint style="warning" %}
**Không được khuyến nghị cho công việc khoa học Survey3**: Nếu không có hiệu chuẩn độ phản xạ, các giá trị pixel Survey3 chỉ đại diện cho độ sáng tương đối, không phải là các phép đo độ phản xạ khoa học. Hãy sử dụng các mục tiêu hiệu chuẩn (hoặc, đối với LATTICE, cảm biến ánh sáng DAQ) để có kết quả chính xác và lặp lại được.
{% endhint %}

***

## Các bước tiếp theo

Sau khi bạn đã đánh dấu các hình ảnh mục tiêu:

1. **Kiểm tra lại cài đặt** - Xem [Điều chỉnh cài đặt dự án](adjusting-project-settings.md)
2. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
3. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết thêm thông tin về các mục tiêu hiệu chuẩn, xem [Mục tiêu hiệu chuẩn](../calibration-targets.md).
