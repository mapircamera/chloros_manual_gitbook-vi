# Điều chỉnh cài đặt dự án

Trước khi xử lý hình ảnh, việc cấu hình cài đặt dự án sao cho phù hợp với yêu cầu quy trình làm việc của bạn là rất quan trọng. Bảng điều khiển Cài đặt dự án <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> cung cấp khả năng kiểm soát toàn diện về hiệu chuẩn, các tùy chọn xử lý, chỉ số đa quang phổ và định dạng xuất.

## Truy cập Cài đặt dự án

1. Mở dự án của bạn trong Chloros
2. Nhấp vào biểu tượng **Cài đặt dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng Cài đặt dự án sẽ hiển thị tất cả các tùy chọn cấu hình

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption><p>Bảng điều khiển Cài đặt dự án — Hiển thị, Phát hiện mục tiêu và Xử lý</p></figcaption></figure>

{% hint style="info" %}
**Các cài đặt được lưu tự động** cùng với dự án của bạn. Khi bạn mở lại dự án, tất cả cài đặt sẽ được khôi phục.
{% endhint %}

***

## Cài đặt nhanh cho các quy trình làm việc thông dụng

### Cài đặt mặc định (Được khuyến nghị cho hầu hết người dùng)

Các cài đặt mặc định hoạt động tốt cho các quy trình làm việc tiêu biểu củaSurvey3
và LATTICE:

* ✅ **Chỉnh sửa hiệu ứng tối góc**: Bật
* ✅ **Hiệu chuẩn độ phản xạ / cân bằng trắng**: Bật (sử dụng các mục tiêuMAPIR
và/hoặc dữ liệu cảm biến ánh sáng DAQ)
* ✅ **Phương pháp Debayer**: Tiêu chuẩn (Nhanh, Chất lượng trung bình)
* ✅ **Định dạng xuất**:TIFF
(16-bit)
* ✅ **Tất cả sản phẩm xuất**: Đã bật (LATTICE tự động chia tách thành các tệp đã khử hiệu ứng Vignette, xem trước, độ sáng và độ phản xạ)

Chỉ cần nhập hình ảnh của bạn và bắt đầu xử lý với các thiết lập mặc định này.

***

## Tổng quan về Cài đặt Dự án

Bảng Cài đặt Dự án được tổ chức thành các phần dưới đây. Hai phần bổ sung — **Cảm biến ánh sáng DAQ**và**Căn chỉnh mảng** — sẽ tự động xuất hiện khi dự án của bạn chứa các tệp liên quan. Để xem tài liệu đầy đủ, hãy tham khảo [Cài đặt Dự án](../project-settings/project-settings.md).

### Hiển thị

* **Độ phân giải hình thu nhỏ**: Độ phân giải của các hình thu nhỏ trong lưới hình ảnh. Các tùy chọn:**Mặc định (512 px)**,**1024 px**,**2048 px**,**Độ phân giải đầy đủ**. Chỉ dành cho hiển thị — không bao giờ ảnh hưởng đến quá trình xử lý. Các giá trị cao hơn sẽ hiển thị sắc nét hơn khi phóng to nhưng tải chậm hơn.

### Phát hiện mục tiêu

Kiểm soát cáchChloros
xác định các mục tiêu hiệu chuẩn trong hình ảnh của bạn.

**Các cài đặt chính:*** **Diện tích mẫu hiệu chuẩn tối thiểu (px)**: Ngưỡng kích thước để phát hiện mục tiêu (mặc định:**25**, phạm vi 0–10000)
* **Độ tương đồng tối thiểu để nhóm mục tiêu (0–100)**: Ngưỡng độ tương đồng để nhóm các vùng mục tiêu (mặc định:**60**)**Khi nào cần điều chỉnh:**

* Tăng diện tích mẫu nếu xuất hiện các phát hiện sai
* Giảm nếu các mục tiêu không được phát hiện
* Điều chỉnh độ tập trung nếu các mục tiêu bị chia thành nhiều phát hiện

{% hint style="info" %}
Các cài đặt này sẽ bị mờ đi khi tính năng **Hiệu chỉnh độ phản xạ / cân bằng trắng** bị tắt — khi tính năng này bị tắt, chức năng phát hiện mục tiêu sẽ không bao giờ hoạt động.
{% endhint %}

### Xử lý

Các tùy chọn xử lý hình ảnh chính và hiệu chỉnh.

**Các thiết lập chính:*** **Chỉnh sửa hiệu ứng tối góc**: Bù đắp hiện tượng tối góc do ống kính gây ra ✅ Được khuyến nghị
* **Hiệu chuẩn độ phản xạ / cân bằng trắng**: Hiệu chuẩn hình ảnh bằng cách sử dụng các mục tiêu được phát hiện (Survey3
) và/hoặc dữ liệu cảm biến ánh sáng DAQ (LATTICE) ✅ Được khuyến nghị
* **Phương pháp Debayer**: Thuật toán chuyển đổi định dạng RAW sang đa phổ 3 kênh
* **Khoảng thời gian hiệu chuẩn tối thiểu**: Thời gian tối thiểu tính bằng giây giữa các lần sử dụng mục tiêu hiệu chuẩn (mặc định:**0** = sử dụng tất cả, phạm vi 0–3600)**Sản phẩm dự phòng khi chưa hiệu chuẩn:**Khi một khung hình không thể được hiệu chuẩn phản xạ (không có mục tiêu hoặc tính năng hiệu chuẩn bị tắt), nó sẽ được xuất dưới dạng một trong hai sản phẩm dự phòng —**chỉ có chính xác một trong hai sản phẩm này tồn tại cho mỗi lần chạy**, được chọn bởi công tắc hiệu chỉnh Vignette:

* **Xuất phản hồi cảm biến**: ghi `Sensor_Response_Images` — được sử dụng khi tính năng hiệu chỉnh Vignette**bị tắt*** **Xuất hình ảnh đã hiệu chỉnh Vignette**: ghi tệp `Vignette_Corrected_Images` — được sử dụng khi tính năng hiệu chỉnh Vignette**bật**Hộp chọn không được kích hoạt sẽ bị mờ đi. Việc bỏ chọn hộp chọn đang hoạt động sẽ ngăn tệp đó được ghi hoàn toàn.**Các sản phẩm xuất của LATTICE** (hiển thị cho mọi dự án; áp dụng cho các bản chụp LATTICE):

* **Xuất hình ảnh đã khử hiệu ứng debayering**: hình ảnh tuyến tính đã khử hiệu ứng debayering (`Debayered_Images`). Áp dụng cho các mô-đunRGB
và đa phổ.
* **Xuất bản xem trước**: bản xem trước trên màn hình (`Preview_Images`).RGB
= cân bằng trắng (nguồn sáng DAQ khi có sẵn, nếu không thì chế độ gray-world) + gamma; đa phổ = kéo dãn màu giả.
* **Xuất độ rạng rỡ**: độ rạng rỡ quang phổ dạng float32 (`Radiance_Images`, W/m²/sr/nm). Chỉ áp dụng cho các mô-đun đa quang phổ — không áp dụng cho các bản chínhRGB
.
* **Xuất độ phản xạ**: độ phản xạ uint16 (`Reflectance_Calibrated_Images`, DN 32768 = ρ 1.0) khi có dữ liệu đo hướng xuống (downwelling) từ `.daq` hoặc mục tiêu trong khung hình bao phủ toàn bộ khung hình. Chỉ áp dụng cho các mô-đun đa quang phổ.

Cả bốn tùy chọn đều **được bật theo mặc định**— một khung hình thô LATTICE được nhập sẽ được phân tách thành từng sản phẩm được kích hoạt và áp dụng trong một lần xử lý duy nhất. Hộp kiểm**Xuất độ phản xạ** sẽ bị mờ khi tính năng hiệu chuẩn độ phản xạ bị tắt. Các cài đặt mà công tắc cha khiến chúng không thể sử dụng luôn bị mờ đi kèm chú giải công cụ nêu tên công tắc cần thay đổi.**Cài đặt nâng cao:*** **Chênh lệch múi giờ cảm biến ánh sáng**: Số giờ so với UTC để đồng bộ hóa thời gian cảm biến ánh sáng (mặc định: 0, phạm vi từ −12 đến +12)
* **Áp dụng hiệu chỉnh PPK**: Sử dụng dữ liệu GPS/điểm phơi sáng từ các tệp `.daq` (mặc định: tắt)
* **Điểm phơi sáng 1/2**: Gán camera cho các điểm phơi sáng trong cấu hình camera kép

{% hint style="info" %}
**Mức độ xử lý LATTICE là tự động.** Các bản ghi LATTICE mang theo mức độ xử lý của chúng trong siêu dữ liệu XMP, và quá trình xử lý luôn bắt đầu từ khung hình thô — không có gì cần cấu hình trong giao diện người dùng. (Cờ &quot;CLI
&quot; `--input-level` tồn tại như một tùy chọn ghi đè dành cho người dùng nâng cao đối với các bản ghi bị mất metadata; xem [CLI
Reference](../reference/cli-reference.md).)
{% endhint %}

### Phương pháp Debayer

Hiện tại, chúng tôi cung cấp 2 phương pháp debayering trongChloros
:

#### Tiêu chuẩn (Nhanh, Chất lượng trung bình)

Phương pháp debayer tiêu chuẩn xử lý nhanh nhưng xuất hiện nhiễu màu do quá trình debayering, dẫn đến hình ảnh kém chính xác và nhiều nhiễu hơn.

#### Nhận diện kết cấu (Chậm, Chất lượng cao nhất) \[Chỉ dành choChloros
+]

Phương pháp Nhận diện kết cấu sử dụng thuật toán loại bỏ hiệu ứng Bayer chất lượng cao có khả năng nhận diện cạnh, kết hợp với mô hình giảm nhiễu AI/ML để loại bỏ gần như toàn bộ nhiễu do quá trình này gây ra. Mô hình này cần bộ nhớ GPU (VRAM) để chạy: với **7 GB VRAM trở lên**, nó có thể xử lý nhiều hình ảnh cùng lúc; dưới 7 GB, nó chỉ xử lý một hình ảnh tại một thời điểm (chậm hơn đáng kể). Xem [Dynamic Compute Adaptation](../processing-architecture/dynamic-compute-adaptation.md).

{% hint style="info" %}
**Các hình ảnh thu được từ LATTICE luôn sử dụng phương pháp khử debayering tiêu chuẩn.** Không có mô hình Texture Aware được huấn luyện dành riêng cho LATTICE, do đó tùy chọn này không được cung cấp cho hình ảnh LATTICE — tuy nhiên, các hình ảnh từSurvey3
trong cùng dự án vẫn có thể sử dụng tính năng này.
{% endhint %}

### Chỉ số (Chỉ số đa phổ)

Cấu hình các chỉ số thực vật cần tính toán và xuất. Menu thả xuống trên giao diện người dùng cung cấp **27 công thức chỉ số được định nghĩa sẵn**.**Cách thêm chỉ số:**

1. Nhấp vào nút**&quot;Thêm chỉ số&quot;**

2. Chọn một chỉ số từ menu thả xuống (NDVI
,NDRE
,GNDVI
, v.v.)
3. Cấu hình cài đặt hiển thị (màu LUT, phạm vi giá trị)
4. Thêm nhiều chỉ số nếu cần

**Các chỉ số phổ biến:*** **NDVI**: Sức khỏe thực vật tổng quát (phổ biến nhất)
* **NDRE**: Phát hiện sớm tình trạng căng thẳng bằngRedEdge
* **GNDVI**: Nhạy cảm với nồng độ diệp lục
* **OSAVI**: Hoạt động tốt với đất có thể nhìn thấy
* **EVI**: Các khu vực có chỉ số diện tích lá (LAI
) cao

**Công thức tùy chỉnh:**

* Tạo công thức chỉ số đa quang phổ tùy chỉnh bằng phép toán dải trên tất cả các kênh hình ảnh
* Lưu công thức tùy chỉnh để sử dụng lại
* Công thức tùy chỉnh là tính năng dành cho góiChloros
+; tính khả dụng phụ thuộc vào cấp độ gói dịch vụ của bạn

Để xem tất cả các chỉ số và công thức có sẵn — bao gồm các tên chỉ hiển thị trên giao diện người dùng (GUI) và những tên cũng hoạt động trongCLI
/SDK
— hãy tham khảo [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md).

### Xuất

Điều khiển định dạng tệp đầu ra.

**Các định dạng có sẵn**(cài đặt:**Định dạng hình ảnh đã hiệu chuẩn**, mặc định**TIFF
(16-bit)**):

* **TIFF
(16-bit)**: Được khuyến nghị cho GIS và phân tích khoa học
* **TIFF
(32-bit, Phần trăm)**: Giá trị số thập phân
* **PNG
(8-bit)**: Nén không mất dữ liệu để hiển thị
* **JPG (8-bit)**: Tệp có kích thước nhỏ nhất, nén có mất dữ liệu

Các tệp đầu ra được ghi vào thư mục dự án, được nhóm theo máy ảnh và định dạng: `<project>/<camera>/<format>/<Product>_Images/`. Radiance **luôn** được ghi dưới dạng float32 vào thư mục `tiff32` bất kể cài đặt này. Các tệp xuất ra giữ nguyên tên tệp nguồn — thư mục xác định sản phẩm. Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md) để xem cấu trúc thư mục đầu ra đầy đủ.

{% hint style="warning" %}
**Đọc giá trị phản xạ**: giá trị DN tương ứng với ρ = 1.0 phụ thuộc vào máy ảnh nguồn — LATTICE sử dụng 32768 (được ghi nhãn là XMP `Chloros:PixelScale`),Survey3
sử dụng 65535. Hãy đọc giá trị thẻ thay vì giả định một hằng số. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md).
{% endhint %}

### Cảm biến ánh sáng DAQ

Phần này liệt kê mọi tệp dữ liệu ánh sáng chiếu xuống (`.daq` / `.csv`) trong dự án của bạn, mỗi tệp một hàng, hiển thị mẫu cảm biến, tên tệp và giá trị hiệu chỉnh **mũ** khuếch tán đang được áp dụng cho tệp đó.

* **Ghi đè giá trị cap (tất cả các tệp)**: một menu thả xuống duy nhất áp dụng cho toàn bộ dự án. Chế độ**Tự động** (mặc định) sử dụng giá trị cap đã ghi trong từng tệp — nếu không có giá trị nào được ghi lại, hệ thống sẽ giả định là điều kiện nắng gắt, vì tất cả các thiết bị DAQ củaMAPIR
đều được trang bị bộ hiệu chỉnh nắng gắt. Việc chọn giá trị cap sẽ ghi đè lên mọi tệp: các bản ghi thô sẽ được hiệu chỉnh theo giá trị đó, và các bản ghi đã có giá trị cap sẽ được tham chiếu lại (hiệu chỉnh đã ghi sẽ bị hủy bỏ, giá trị cap đã chọn sẽ được áp dụng).
* Các hàng sẽ hiển thị cảnh báo khi giá trị giới hạn được ghi lại là giá trị mặc định được hub giả định chứ không phải do người vận hành xác nhận, và khi giá trị giới hạn đã chọn không có cấu hình cho mẫu thiết bị đó (việc ghi đè sẽ bị từ chối đối với tệp đó).

Các bản ghi DAQ được thực hiện trong tab Cảm biến ánh sáng sẽ tự động được thêm vào dự án đang mở, và các tệp `.daq` / `.csv` được nhập sẽ xuất hiện ở đây ngay khi được thêm vào.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption><p>Cài đặt Dự án Phía dưới — Chỉ mục, Định dạng xuất, phần Cảm biến Ánh sáng DAQ và các điều khiển mẫu dự án/thư mục</p></figcaption></figure>### Căn chỉnh mảng

Phần này **chỉ** xuất hiện khi có ít nhất một hình ảnh trong dự án chứa phép biến đổi căn chỉnh giữa các mô-đun mà các mảng LATTICE đóng dấu tại thời điểm ghi (`Chloros:Alignment*` XMP). Phần này hiển thị số lượng hình ảnh có chứa thẻ và camera nào là tham chiếu, kèm theo các tùy chọn điều khiển sau:

* **Áp dụng căn chỉnh mảng** (mặc định: bật): biến dạng mọi sản phẩm đã xử lý (loại bỏ hiệu ứng Bayer / xem trước / độ sáng / độ phản xạ / chỉ số) sao cho phù hợp với hình học tham chiếu chung của mảng. Tắt = xuất ở hình học cảm biến gốc.
* **Cắt theo vùng chồng chéo chung** (mặc định: bật): cắt các tệp xuất đã căn chỉnh để chỉ giữ lại vùng mà tất cả các mô-đun đều có chung, do đó mỗi dải phổ sẽ có cùng kích thước. Tắt sẽ giữ nguyên toàn bộ khung hình cảm biến (điền màu đen bên ngoài vùng nguồn).
* **Lấy mẫu lại**:**Bilinear (mịn, mặc định)**,**Nearest (giữ nguyên giá trị chính xác)**— không trộn pixel, dành cho phân tích quang phổ nghiêm ngặt — hoặc**Cubic (sắc nét nhất)**.***

## Lưu và tải cài đặt

### Lưu mẫu dự án

Tạo các mẫu có thể tái sử dụng để đảm bảo quy trình làm việc nhất quán:

1. Cấu hình tất cả các cài đặt mong muốn trong bảng Cài đặt dự án
2. Cuộn xuống phần **&quot;Lưu mẫu dự án&quot;** ở phía dưới
3. Nhập tên mẫu mô tả (ví dụ: &quot;Survey3N

_RGN_Agriculture&quot;)
4. Nhấp vào biểu tượng lưu

**Lợi ích:**

* Áp dụng các cài đặt giống hệt nhau cho nhiều dự án
* Chia sẻ cấu hình với các thành viên trong nhóm
* Duy trì tính nhất quán cho các cuộc khảo sát lặp lại

### Tải mẫu vào dự án mới

Khi tạo dự án mới:

1. Chọn **&quot;Dự án mới&quot;** từ menu chính
2. Chọn một mẫu dự án trong trình chọn mẫu tùy chọn
3. Tất cả các cài đặt từ mẫu sẽ được áp dụng tự động

### Thư mục làm việc

Cài đặt **&quot;Thư mục làm việc&quot;** xác định vị trí mặc định để tạo các dự án mới:

* **Vị trí mặc định**: `C:\Users\[Username]\Chloros Projects`
* **Thay đổi vị trí**: Nhấp vào biểu tượng chỉnh sửa và chọn thư mục mới
* **Chia sẻ vớiCLI**: `chloros-cli` sử dụng cùng cài đặt thư mục dự án mặc định
* **Khi nào cần thay đổi**:
  * Ổ đĩa mạng để hợp tác nhóm
  * Ổ đĩa khác có dung lượng lưu trữ lớn hơn
  * Cấu trúc thư mục được tổ chức theo năm/khách hàng

***

## Cài đặt PPK (Post-Processed Kinematic)

Nếu sử dụng bộ ghi dữ liệu DAQMAPIR

có GPS để xác định vị trí địa lý chính xác:

### Yêu cầu tiên quyết

* Bộ ghi dữ liệu DAQMAPIR

có mô-đun GPS (GNSS)
* Tệp nhật ký .daq có các mục chân cắm phơi sáng
* Máy ảnh được kết nối với các chân cắm phơi sáng của DAQ trong phiên ghi hình

### Các bước cấu hình

1. Đặt tệp nhật ký .daq vào thư mục dự án của bạn
2. Trong Cài đặt Dự án, kích hoạt ô chọn **&quot;Áp dụng hiệu chỉnh PPK&quot;**

3. Cài đặt**&quot;Chênh lệch múi giờ cảm biến ánh sáng&quot;** nếu cần (mặc định: 0 cho UTC)
4. Gán máy ảnh cho các chân phơi sáng:
   * **Máy ảnh đơn**: Tự động được gán cho Chân 1
   * **Máy ảnh kép**: Gán thủ công từng máy ảnh vào chân tương ứng**Gán chân phơi sáng:*** **Chân phơi sáng 1**: Chọn mẫu camera từ danh sách thả xuống
* **Chân phơi sáng 2**: Chọn camera thứ hai hoặc &quot;Không sử dụng&quot;
* Không thể gán cùng một camera cho cả hai chân

{% hint style="warning" %}
**Quan trọng**: Các chân phơi sáng phải được gán chính xác cho các camera tương ứng. Việc gán sai sẽ dẫn đến dữ liệu định vị địa lý không chính xác.
{% endhint %}

***

## Các tình huống nâng cao

### Dự án nhiều camera

Khi xử lý hình ảnh từ nhiều cameraMAPIR

trong một dự án:

1.Chloros

tự động nhận diện từng mẫu camera (cảSurvey3

và LATTICE)
2. Mỗi camera được áp dụng các cấu hình xử lý phù hợp và mỗi camera có cây thư mục đầu ra riêng
3. PPK: Gán thủ công từng cameraSurvey3

vào chân cắm phơi sáng chính xác
4. Tất cả camera sử dụng cùng định dạng xuất và chỉ mục

**Ví dụ**:Survey3W

RGN

+Survey3N

OCN

hệ thống camera kép, hoặc mảng LATTICE kết hợp camera chínhRGB

với các mô-đun dải hẹp

### Khảo sát theo thời gian hoặc nhiều ngày

Đối với các cuộc khảo sát lặp lại cùng một khu vực theo thời gian:

1. Tạo một mẫu với các cài đặt tiêu chuẩn của bạn
2. Sử dụng thiết lập mục tiêu hiệu chuẩn nhất quán trong mỗi phiên
3. Xử lý từng ngày như một dự án riêng biệt
4. Sử dụng các cài đặt giống hệt nhau để có kết quả so sánh được
5. Xuất ra cùng một định dạng để phân tích theo thời gian

### Bộ dữ liệu lớn

Đối với các dự án có nhiều hình ảnh (500+):

* Cân nhắc chia thành các dự án nhỏ hơn theo ngày hoặc khu vực
* Sử dụngChloros

+ cùng với xử lý song song để có kết quả nhanh hơn
* Cân nhắc sử dụngCLI

hoặcAPI

để tự động hóa xử lý hàng loạt
* Điều chỉnh khoảng thời gian hiệu chuẩn lại tối thiểu để giảm thời gian phát hiện mục tiêu

***

## Kiểm tra các thiết lập của bạn

Trước khi bắt đầu xử lý, hãy kiểm tra các thiết lập quan trọng sau:

* [ ] Mô hình máy ảnh đã được nhận diện chính xác trong Trình duyệt tệp
* [ ] Chức năng chỉnh sửa hiệu ứng viền tối đã được bật
* [ ] Chức năng hiệu chuẩn phản xạ đã được bật
* [ ] Đối vớiSurvey3

: đã nhập và kiểm tra ít nhất một hình ảnh mục tiêu hiệu chuẩn; đối với LATTICE: có sẵn mục tiêu và/hoặc bản ghi hướng xuống của `.daq`
* [ ] Đã thêm các chỉ số đa phổ mong muốn
* [ ] Định dạng xuất phù hợp với quy trình làm việc của bạn
* [ ] Đã cấu hình cài đặt PPK (nếu sử dụng tệp .daq với các sự kiện phơi sáng)

***

## Các bước tiếp theo

Sau khi đã cấu hình xong các thiết lập:

1. **Đánh dấu các hình ảnh mục tiêu hiệu chuẩn** - Xem [Chọn hình ảnh mục tiêu](choosing-target-images.md)
2. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
3. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết chi tiết đầy đủ về tất cả các thiết lập có sẵn, hãy tham khảo tài liệu tham khảo [Thiết lập dự án](../project-settings/project-settings.md).
