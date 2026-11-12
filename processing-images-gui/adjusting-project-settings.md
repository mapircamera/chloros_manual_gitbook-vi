# Điều chỉnh cài đặt dự án

Trước khi xử lý hình ảnh, việc cấu hình cài đặt dự án sao cho phù hợp với yêu cầu quy trình làm việc của bạn là rất quan trọng. Bảng điều khiển Cài đặt dự án <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> cho phép bạn kiểm soát toàn diện các thiết lập hiệu chuẩn, tùy chọn xử lý, chỉ số đa quang phổ và định dạng xuất.

## Truy cập Cài đặt Dự án

1. Mở dự án của bạn trong Chloros
2. Nhấp vào biểu tượng **Cài đặt Dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng điều khiển Cài đặt Dự án hiển thị tất cả các tùy chọn cấu hình

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption><p>Bảng điều khiển Cài đặt dự án — Hiển thị, Phát hiện mục tiêu và Xử lý</p></figcaption></figure>

{% hint style="info" %}
**Các cài đặt được lưu tự động** cùng với dự án của bạn. Khi bạn mở lại dự án, tất cả cài đặt sẽ được khôi phục.
{% endhint %}

***

## Cài đặt nhanh cho các quy trình làm việc thông dụng

### Cài đặt mặc định (Được khuyến nghị cho hầu hết người dùng)

Các cài đặt mặc định hoạt động tốt cho các quy trình làm việc thông thường của Survey3 và LATTICE:

* ✅ **Chỉnh sửa hiệu ứng tối góc**: Bật
* ✅ **Hiệu chỉnh độ phản xạ / cân bằng trắng**: Đã bật (sử dụng các mục tiêu MAPIR và/hoặc dữ liệu cảm biến ánh sáng DAQ)
* ✅ **Phương pháp Debayer**: Tiêu chuẩn (Nhanh, Chất lượng trung bình)
* ✅ **Định dạng xuất**: TIFF (16-bit) (TIFF (8-bit) cho dự án toàn 8-bit)
* ✅ **Tất cả sản phẩm xuất**: Bật (LATTICE tự động chia tách thành các tệp đã loại bỏ hiệu ứng vignette, xem trước, độ sáng và độ phản xạ)

Chỉ cần nhập hình ảnh của bạn và bắt đầu xử lý với các thiết lập mặc định này.

***

## Tổng quan về Cài đặt Dự án

Bảng Cài đặt Dự án được tổ chức thành các phần dưới đây. Hai phần bổ sung — **Cảm biến ánh sáng DAQ**và**Căn chỉnh mảng** — sẽ tự động xuất hiện khi dự án của bạn chứa các tệp liên quan. Để xem tài liệu hướng dẫn đầy đủ, hãy tham khảo [Cài đặt Dự án](../project-settings/project-settings.md).

### Hiển thị

* **Độ phân giải hình thu nhỏ**: Độ phân giải của các hình thu nhỏ trong lưới hình ảnh. Tùy chọn:**Mặc định (512 px)**,**1024 px**,**2048 px**,**Độ phân giải đầy đủ**. Chỉ dành cho hiển thị — không bao giờ ảnh hưởng đến quá trình xử lý. Các giá trị cao hơn sẽ hiển thị sắc nét hơn khi phóng to nhưng tải chậm hơn.

### Phát hiện mục tiêu

Kiểm soát cách Chloros xác định các mục tiêu hiệu chuẩn trong hình ảnh của bạn.

**Các cài đặt chính:**

* **Diện tích mẫu hiệu chuẩn tối thiểu (px)**: Ngưỡng kích thước để phát hiện mục tiêu (mặc định:**25**, phạm vi 0–10000)
* **Độ tương đồng tối thiểu để nhóm mục tiêu (0–100)**: Ngưỡng tương đồng để nhóm các vùng mục tiêu (mặc định:**60**)**Khi nào cần điều chỉnh:**

* Tăng diện tích mẫu nếu xuất hiện các phát hiện sai
* Giảm nếu các mục tiêu không được phát hiện
* Điều chỉnh độ nhóm nếu các mục tiêu bị chia thành nhiều phát hiện

{% hint style="info" %}
Các cài đặt này sẽ bị mờ đi khi tính năng **Hiệu chỉnh độ phản xạ / cân bằng trắng** bị tắt — khi tính năng này bị tắt, chức năng phát hiện mục tiêu sẽ không bao giờ hoạt động.
{% endhint %}

### Xử lý

Các tùy chọn xử lý hình ảnh chính và hiệu chỉnh.

**Các cài đặt chính:**

* **Chỉnh sửa hiệu ứng tối góc**: Bù đắp hiện tượng tối góc do ống kính gây ra ✅ Được khuyến nghị
* **Hiệu chuẩn phản xạ / cân bằng trắng**: Hiệu chuẩn hình ảnh bằng cách sử dụng các mục tiêu được phát hiện (phần mềm Survey3) và/hoặc dữ liệu cảm biến ánh sáng DAQ (phần mềm LATTICE) ✅ Được khuyến nghị
* **Phương pháp Debayer**: Thuật toán chuyển đổi định dạng RAW sang đa phổ 3 kênh
* **Khoảng thời gian hiệu chuẩn tối thiểu**: Thời gian tối thiểu tính bằng giây giữa các lần sử dụng mục tiêu hiệu chuẩn (mặc định:**0** = sử dụng tất cả, phạm vi 0–3600)**Các sản phẩm dự phòng chưa được hiệu chuẩn:**Khi không thể hiệu chuẩn độ phản xạ cho một khung hình (không có mục tiêu hoặc tính năng hiệu chuẩn bị tắt), khung hình đó sẽ được xuất dưới dạng một trong hai sản phẩm dự phòng —**chỉ có chính xác một trong hai sản phẩm này tồn tại cho mỗi lần chạy**, được chọn bởi công tắc hiệu chỉnh Vignette:

* **Xuất phản hồi cảm biến**: ghi `Sensor_Response_Images` — được sử dụng khi tính năng hiệu chỉnh Vignette**tắt**
* **Xuất dữ liệu đã hiệu chỉnh hiệu ứng Vignette**: ghi tệp `Vignette_Corrected_Images` — được sử dụng khi tính năng hiệu chỉnh hiệu ứng Vignette được**bật**Hộp kiểm không được chọn sẽ bị mờ đi. Việc bỏ chọn hộp kiểm đang hoạt động sẽ ngăn tệp đó được ghi hoàn toàn.**Các sản phẩm xuất của LATTICE** (hiển thị cho mọi dự án; chúng áp dụng cho các bản chụp LATTICE):

* **Xuất hình ảnh đã khử hiệu ứng bay**: hình ảnh đã loại bỏ hiệu ứng debayering tuyến tính (`Debayered_Images`): đã loại bỏ độ lệch tối của gói hiệu chuẩn, sau đó được demosaic, không có trường phẳng hoặc tách kênh — cùng tệp mà dữ liệu thu thập lưu ở mức đã loại bỏ hiệu ứng debayering — theo hướng gốc của cảm biến. Áp dụng cho các mô-đun RGB và đa phổ.
* **Xuất bản xem trước**: bản xem trước trên màn hình (`Preview_Images`). RGB = cân bằng trắng (nguồn sáng DAQ khi có sẵn, nếu không thì chế độ gray-world) + gamma; đa phổ = kéo dãn màu giả.
* **Xuất độ rạng rỡ**: độ rạng rỡ phổ dạng float32 (`Radiance_Images`, W/m²/sr/nm). Chỉ áp dụng cho các mô-đun đa phổ — không áp dụng cho các tệp gốc RGB.
* **Xuất độ phản xạ**: độ phản xạ uint16 (`Reflectance_Calibrated_Images`, DN 32768 = ρ 1.0) khi có dữ liệu chiếu xuống từ `.daq` hoặc mục tiêu trong khung hình bao phủ toàn bộ khung hình. Chỉ áp dụng cho các mô-đun đa quang phổ.

Cả bốn tùy chọn đều **được bật theo mặc định**— một khung hình thô LATTICE được nhập vào sẽ được phân tách thành từng sản phẩm được kích hoạt và áp dụng trong một lần xử lý duy nhất. Hộp kiểm**Xuất độ phản xạ** sẽ bị mờ đi khi tính năng hiệu chuẩn độ phản xạ bị tắt. Các cài đặt mà công tắc cha khiến chúng không thể thực hiện được sẽ luôn bị mờ đi kèm theo chú giải công cụ nêu tên công tắc cần thay đổi.

**Cài đặt nâng cao:**

* **Chênh lệch múi giờ cảm biến ánh sáng**: Số giờ so với UTC để đồng bộ hóa thời gian cảm biến ánh sáng (mặc định: 0, phạm vi từ −12 đến +12)
* **Áp dụng hiệu chỉnh PPK**: Sử dụng dữ liệu GPS/điểm phơi sáng từ các tệp `.daq` (mặc định: tắt)
* **Điểm phơi sáng 1/2**: Gán máy ảnh cho các điểm phơi sáng trong cấu hình hai máy ảnh

{% hint style="info" %}
**Mức độ xử lý đầu vào LATTICE là tự động.** Các bản ghi LATTICE mang theo mức độ xử lý của chúng trong siêu dữ liệu XMP, và quá trình xử lý luôn bắt đầu từ khung hình thô — không cần cấu hình gì trong giao diện người dùng. (Cờ CLI `--input-level` tồn tại như một tùy chọn ghi đè dành cho người dùng nâng cao đối với các tệp ghi hình bị mất metadata; xem [Tài liệu tham khảo CLI](../reference/cli-reference.md).)
{% endhint %}

### Phương pháp loại bỏ hiệu ứng Bayer

Hiện tại, Chloros cung cấp 2 phương pháp loại bỏ hiệu ứng Bayer:

#### Tiêu chuẩn (Nhanh, Chất lượng trung bình)

Phương pháp loại bỏ hiệu ứng Bayer tiêu chuẩn xử lý nhanh nhưng gây ra nhiễu màu trong quá trình xử lý, dẫn đến hình ảnh kém chính xác và nhiều nhiễu hơn.

#### Nhận diện kết cấu (Chậm, Chất lượng cao nhất) \[Chỉ dành cho Chloros+]

Phương pháp Nhận diện kết cấu sử dụng thuật toán loại bỏ ma trận chất lượng cao có khả năng nhận diện cạnh, kết hợp với mô hình giảm nhiễu AI/ML để loại bỏ gần như toàn bộ nhiễu do quá trình loại bỏ ma trận gây ra. Mô hình này yêu cầu bộ nhớ GPU (VRAM) để chạy: với **7 GB VRAM trở lên**, nó có thể xử lý nhiều hình ảnh cùng lúc; dưới 7 GB, nó chỉ xử lý một hình ảnh tại một thời điểm (chậm hơn đáng kể). Xem [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md).

{% hint style="info" %}
**Các hình ảnh thu được từ LATTICE luôn sử dụng phương pháp demosaic tiêu chuẩn.** Không có mô hình Texture Aware được huấn luyện cho LATTICE, do đó tùy chọn này không được cung cấp cho hình ảnh LATTICE — tuy nhiên, hình ảnh từ Survey3 trong cùng dự án vẫn có thể sử dụng nó.
{% endhint %}

### Chỉ số (Chỉ số đa quang phổ)

Cấu hình các chỉ số thực vật cần tính toán và xuất. Menu thả xuống trên giao diện người dùng cung cấp **27 công thức chỉ số được định nghĩa sẵn**.**Cách thêm chỉ số:**

1. Nhấp vào nút**"Thêm chỉ số"**

2. Chọn một chỉ số từ menu thả xuống (NDVI, NDRE, GNDVI, v.v.)
3. Cấu hình cài đặt hiển thị (màu LUT, phạm vi giá trị)
4. Thêm nhiều chỉ số nếu cần

**Các chỉ số phổ biến:**

* **NDVI**: Sức khỏe thực vật tổng quát (phổ biến nhất)
* **NDRE**: Phát hiện sớm tình trạng căng thẳng bằng RedEdge
* **GNDVI**: Nhạy cảm với nồng độ diệp lục
* **OSAVI**: Hoạt động tốt với đất có thể nhìn thấy
* **EVI**: Các khu vực có chỉ số diện tích lá (LAI) cao

**Công thức tùy chỉnh:**

* Tạo công thức chỉ số đa phổ tùy chỉnh bằng phép toán dải trên tất cả các kênh hình ảnh
* Lưu công thức tùy chỉnh để sử dụng lại
* Công thức tùy chỉnh là tính năng của Chloros+; tính khả dụng phụ thuộc vào gói dịch vụ của bạn

Để xem tất cả các chỉ số và công thức có sẵn — bao gồm những tên chỉ hiển thị trên giao diện người dùng (GUI) và những tên cũng hoạt động trong CLI/SDK — hãy tham khảo [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md).

### Xuất

Điều khiển định dạng tệp đầu ra.

**Các định dạng có sẵn**(cài đặt:**Định dạng hình ảnh đã hiệu chuẩn**, mặc định**TIFF (16-bit)**):

* **TIFF (16-bit)**: Được khuyến nghị cho GIS và phân tích khoa học
* **TIFF (8-bit)**: TIFF 8-bit* **TIFF (32-bit, Phần trăm)**: Giá trị số thập phân
* **PNG (8-bit)**: Nén không mất dữ liệu để hiển thị
* **JPG (8-bit)**: Tệp nhỏ nhất, nén có mất dữ liệu

Danh sách này cho thấy các hình ảnh trong dự án có thể chứa những gì: Chloros không nâng cấp độ sâu bit. Tất cả các hình ảnh 8-bit chỉ cung cấp các định dạng TIFF (8-bit), PNG (8-bit) và JPG (8-bit), và mặc định là TIFF (8-bit). Trong một dự án hỗn hợp, mỗi lần chụp sẽ được xuất ở độ sâu của chính nó hoặc thấp hơn. Radiance vẫn giữ nguyên ở định dạng số thực 32-bit.

Các tệp đầu ra được lưu trong thư mục dự án, được nhóm theo máy ảnh và định dạng: `<project>/<camera>/<format>/<Product>_Images/`. Dữ liệu Radiance **luôn** được lưu dưới dạng float32 vào thư mục `tiff32` bất kể cài đặt này. Các tệp xuất giữ nguyên tên tệp nguồn — thư mục xác định sản phẩm. Các tệp xuất đa phổ LATTICE cũng có thể ghi các mặt nạ giới hạn theo từng pixel vào thư mục `clamp_masks/` bên cạnh các thư mục sản phẩm. Xem [Hoàn tất quá trình xử lý](finishing-the-processing.md) để biết cấu trúc thư mục đầu ra đầy đủ.

{% hint style="warning" %}
**Đọc giá trị phản xạ**: giá trị DN tương ứng với ρ = 1.0 phụ thuộc vào máy ảnh nguồn — LATTICE sử dụng 32768 (được ghi nhãn là XMP `Chloros:PixelScale`), Survey3 sử dụng 65535. Hãy đọc giá trị thẻ thay vì giả định một hằng số. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md).
{% endhint %}

### Cảm biến ánh sáng DAQ

Phần này liệt kê mọi tệp dữ liệu ánh sáng chiếu xuống (`.daq` / `.csv`) trong dự án của bạn, mỗi tệp một hàng, hiển thị mẫu cảm biến, tên tệp và **mức bù** bộ khuếch tán đang được áp dụng cho tệp đó.

* **Ghi đè giá trị cap (tất cả các tệp)**: một menu thả xuống duy nhất áp dụng cho toàn bộ dự án.**Tự động** (mặc định) sử dụng giá trị nắp khuếch tán được ghi lại trong từng tệp — giả định là ánh nắng mặt trời nếu không có dữ liệu nào được ghi lại, nhưng các cảm biến DAQ-U, DAQ-M và DAQ-E được cung cấp mà không có giá trị nắp khuếch tán, và Chloros không thể xác định giá trị nào đã được hiệu chỉnh, do đó quá trình xử lý sẽ cảnh báo khi sử dụng giá trị nắp khuếch tán giả định: hãy chọn giá trị nắp khuếch tán đã được hiệu chỉnh. Việc chọn giới hạn sẽ ghi đè lên mọi tệp ngoại trừ tệp của DAQ-E-S, vì bộ khuếch tán của thiết bị này là một phần của quá trình hiệu chuẩn: các bản ghi thô được hiệu chỉnh bằng bộ khuếch tán này, và các bản ghi đã có giới hạn sẽ được tham chiếu lại (hiệu chỉnh đã ghi bị hủy bỏ, giới hạn đã chọn được áp dụng).
* Các hàng sẽ hiển thị cảnh báo khi nắp ghi lại là giá trị mặc định được giả định của hub thay vì được người vận hành xác nhận, và khi nắp đã chọn không có hồ sơ cho mẫu thiết bị đó (việc ghi đè sẽ bị từ chối đối với tệp đó).

Các bản ghi DAQ được thực hiện trong tab Cảm biến ánh sáng sẽ tự động được thêm vào dự án đang mở, và các tệp `.daq` / `.csv` đã nhập sẽ xuất hiện tại đây ngay khi được thêm vào.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption><p>Cài đặt Dự án Phía Dưới — Chỉ mục, Định dạng xuất, phần Cảm biến Ánh sáng DAQ và các điều khiển mẫu dự án/thư mục</p></figcaption></figure>### Căn chỉnh mảng

Phần này **chỉ** xuất hiện khi có ít nhất một hình ảnh trong dự án chứa phép biến đổi căn chỉnh giữa các mô-đun mà các mảng LATTICE đóng dấu tại thời điểm chụp (`Chloros:Alignment*` XMP). Phần này hiển thị số lượng hình ảnh có chứa thẻ và camera nào là tham chiếu, kèm theo các tùy chọn sau:

* **Áp dụng căn chỉnh mảng** (mặc định: bật): biến dạng mọi sản phẩm đã xử lý (loại bỏ hiệu ứng Bayer / xem trước / độ rạng rỡ / độ phản xạ / chỉ số) vào hình học tham chiếu chung của mảng. Tắt = xuất ở hình học cảm biến gốc.
* **Cắt theo vùng chồng chéo chung** (mặc định: bật): cắt các tệp xuất đã căn chỉnh để chỉ giữ lại vùng mà tất cả các mô-đun đều có chung, do đó mỗi dải phổ sẽ có cùng diện tích phủ sóng. Tắt sẽ giữ nguyên toàn bộ khung hình cảm biến (điền màu đen bên ngoài vùng nguồn).
* **Lấy mẫu lại**:**Bilinear (mịn, mặc định)**,**Nearest (giữ nguyên giá trị chính xác)**— không trộn giữa các pixel, dành cho phân tích quang phổ nghiêm ngặt — hoặc**Cubic (sắc nét nhất)**.

***

## Lưu và Tải Cài đặt

### Lưu Mẫu Dự án

Tạo các mẫu có thể tái sử dụng để đảm bảo quy trình làm việc nhất quán:

1. Cấu hình tất cả các cài đặt mong muốn trong bảng Cài đặt Dự án
2. Cuộn xuống phần **"Lưu Mẫu Dự án"** ở phía dưới
3. Nhập tên mẫu mô tả (ví dụ: “Survey3N_RGN_Agriculture”)
4. Nhấp vào biểu tượng lưu

**Lợi ích:**

* Áp dụng các cài đặt giống hệt nhau cho nhiều dự án
* Chia sẻ cấu hình với các thành viên trong nhóm
* Duy trì tính nhất quán cho các cuộc khảo sát lặp lại

### Tải mẫu vào dự án mới

Khi tạo một dự án mới:

1. Chọn **"Dự án mới"** từ menu chính
2. Chọn một mẫu dự án trong trình chọn mẫu tùy chọn
3. Tất cả các cài đặt từ mẫu sẽ được áp dụng tự động

### Thư mục làm việc

Cài đặt **"Thư mục làm việc"** chỉ định vị trí mặc định để tạo các dự án mới:

* **Vị trí mặc định**: `Documents\MAPIR\Chloros Projects` trong thư mục Tài liệu (Windows Documents) của bạn (thường là `C:\Users\[Username]\Documents\MAPIR\Chloros Projects`; vị trí này sẽ thay đổi theo thư mục Tài liệu nếu OneDrive hoặc hồ sơ di động di chuyển nó)
* **Thay đổi vị trí**: Nhấp vào biểu tượng chỉnh sửa và chọn thư mục mới
* **Chia sẻ với CLI**: `chloros-cli` sử dụng cùng cài đặt thư mục dự án mặc định
* **Giữ tên thư mục ngắn trên Windows**: trừ khi tính năng đường dẫn dài của Windows được bật, thư mục dự án có tên dài hơn khoảng 130 ký tự sẽ bị từ chối trước khi xử lý — xem [Bắt đầu xử lý](starting-the-processing.md#the-project-folder-is-too-deep-for-this-computer)
* **Khi nào nên thay đổi**:
  * Ổ đĩa mạng để hợp tác nhóm
  * Ổ đĩa khác có dung lượng lưu trữ lớn hơn
  * Cấu trúc thư mục được tổ chức theo năm/khách hàng

***

## Cài đặt PPK (Post-Processed Kinematic)

Nếu sử dụng máy ghi dữ liệu DAQ củMAPIR kèm GPS để xác định vị trí địa lý chính xác:

### Yêu cầu tiên quyết

* Thiết bị ghi dữ liệu MAPIR DAQ có mô-đun GPS (GNSS)
* Tệp nhật ký .daq có các mục chân cắm phơi sáng
* Máy ảnh được kết nối với các chân cắm phơi sáng của DAQ trong phiên ghi hình

### Các bước cấu hình

1. Đặt tệp nhật ký .daq vào thư mục dự án của bạn
2. Trong Cài đặt Dự án, chọn hộp kiểm **"Áp dụng hiệu chỉnh PPK"**

3. Cài đặt**"Chênh lệch múi giờ cảm biến ánh sáng"** nếu cần (mặc định: 0 cho UTC)
4. Gán máy ảnh vào các chân phơi sáng:
   * **Máy ảnh đơn**: Tự động được gán vào Chân 1
   * **Máy ảnh kép**: Gán thủ công từng máy ảnh vào chân tương ứng

**Gán chân phơi sáng:**

* **Chân phơi sáng 1**: Chọn mẫu camera từ danh sách thả xuống
* **Chân phơi sáng 2**: Chọn camera thứ hai hoặc "Không sử dụng"
* Không thể gán cùng một camera cho cả hai chân

{% hint style="warning" %}
**Quan trọng**: Các chân phơi sáng phải được gán chính xác cho các camera tương ứng. Việc gán sai sẽ dẫn đến dữ liệu định vị địa lý không chính xác.
{% endhint %}

***

## Các tình huống nâng cao

### Dự án đa camera

Khi xử lý hình ảnh từ nhiều camera MAPIR trong một dự án:

1. Chloros tự động nhận diện từng mẫu camera (cả Survey3 và LATTICE)
2. Mỗi camera sẽ được áp dụng các cấu hình xử lý phù hợp và có hệ thống thư mục đầu ra riêng
3. PPK: Gán thủ công từng camera Survey3 vào chân cắm phơi sáng chính xác
4. Tất cả camera đều sử dụng cùng định dạng xuất và chỉ mục

**Ví dụ**: Bộ thiết bị hai camera Survey3W RGN + Survey3N OCN, hoặc mảng camera LATTICE kết hợp camera chính RGB với các mô-đun dải hẹp

### Khảo sát theo thời gian hoặc nhiều ngày

Đối với các cuộc khảo sát lặp lại cùng một khu vực theo thời gian:

1. Tạo một mẫu với các cài đặt tiêu chuẩn của bạn
2. Sử dụng thiết lập mục tiêu hiệu chuẩn nhất quán trong mỗi phiên
3. Xử lý từng ngày như một dự án riêng biệt
4. Sử dụng các cài đặt giống hệt nhau để có kết quả so sánh được
5. Xuất dưới cùng định dạng để phân tích theo thời gian

### Bộ dữ liệu lớn

Đối với các dự án có nhiều hình ảnh (500+):

* Cân nhắc chia thành các dự án nhỏ hơn theo ngày hoặc khu vực
* Sử dụng tính năng xử lý song song của Chloros+ để có kết quả nhanh hơn
* Cân nhắc sử dụng CLI hoặc API để tự động hóa xử lý hàng loạt
* Điều chỉnh khoảng thời gian hiệu chuẩn lại tối thiểu để giảm thời gian phát hiện mục tiêu

***

## Kiểm tra các thiết lập của bạn

Trước khi bắt đầu xử lý, hãy kiểm tra các thiết lập quan trọng sau:

* [ ] Mô hình máy ảnh đã được nhận diện chính xác trong Trình duyệt tệp
* [ ] Chức năng chỉnh sửa hiệu ứng viền tối đã được bật
* [ ] Chức năng hiệu chuẩn phản xạ đã được bật
* [ ] Đối với Survey3: đã nhập và kiểm tra ít nhất một hình ảnh mục tiêu hiệu chuẩn; đối với LATTICE: có sẵn mục tiêu và/hoặc bản ghi hướng xuống `.daq`
* [ ] Đã thêm các chỉ số đa phổ mong muốn
* [ ] Định dạng xuất phù hợp với quy trình làm việc của bạn
* [ ] Đã cấu hình cài đặt PPK (nếu sử dụng tệp .daq có các sự kiện phơi sáng)

***

## Các bước tiếp theo

Sau khi đã cấu hình xong các cài đặt:

1. **Đánh dấu các hình ảnh mục tiêu hiệu chuẩn** - Xem [Chọn hình ảnh mục tiêu](choosing-target-images.md)
2. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)
3. **Theo dõi tiến trình** - Xem [Theo dõi quá trình xử lý](monitoring-the-processing.md)

Để biết chi tiết đầy đủ về tất cả các cài đặt có sẵn, hãy tham khảo tài liệu tham khảo [Cài đặt dự án](../project-settings/project-settings.md).
