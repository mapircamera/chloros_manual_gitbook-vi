# Cài đặt dự án

Thanh bên “Cài đặt dự án” <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line">

trong Chloros cho phép bạn cấu hình tất cả các khía cạnh liên quan đến xử lý hình ảnh, phát hiện mục tiêu hiệu chuẩn, tính toán chỉ số đa quang phổ và các tùy chọn xuất dữ liệu cho dự án của bạn. Các cài đặt này được lưu cùng với dự án và có thể được lưu dưới dạng mẫu để tái sử dụng cho nhiều dự án khác nhau.

## Truy cập Cài đặt dự án

Để truy cập Cài đặt dự án:

1. Mở một dự án trong Chloros


2. Nhấp vào tab **Cài đặt dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line">

ở thanh bên trái
3. Bảng cài đặt sẽ hiển thị tất cả các tùy chọn cấu hình có sẵn, được sắp xếp theo danh mục

<!-- SCREENSHOT-NEEDED: Full Project Settings sidebar of a LATTICE project, scrolled so the Processing category is visible showing the per-product export checkboxes (Export sensor response, Export vignette corrected, Export debayered, Export preview, Export radiance, Export reflectance) and the Debayer method row. -->



{% hint style="info" %}
**Các cài đặt phụ thuộc vào các cài đặt khác sẽ bị mờ đi.** Khi một tùy chọn cấp trên khiến một cài đặt khác không thể thực hiện được (ví dụ: bỏ chọn *Hiệu chuẩn độ phản xạ / cân bằng trắng* sẽ khiến *Xuất độ phản xạ* không thể thực hiện được), tùy chọn phụ thuộc sẽ bị vô hiệu hóa và chú thích công cụ của nó sẽ nêu tên tùy chọn cần thay đổi.
{% endhint %}

***

## Hiển thị

### Độ phân giải hình thu nhỏ

* **Loại**: Danh sách thả xuống
* **Tùy chọn**: `Default (512 px)`, `1024 px`, `2048 px`, `Full resolution`
* **Mặc định**: Mặc định (512 px)
* **Mô tả**: Độ phân giải (theo cạnh dài nhất, tính bằng pixel) mà tại đó các hình thu nhỏ trong lưới hình ảnh được hiển thị. Các giá trị cao hơn sẽ cho hình ảnh sắc nét hơn khi phóng to nhưng tải chậm hơn và tiêu tốn nhiều bộ nhớ hơn. Độ phân giải đầy đủ sẽ hiển thị kích thước hình ảnh gốc.
* **Lưu ý**: Chỉ hiển thị — cài đặt này không bao giờ ảnh hưởng đến quá trình xử lý hoặc các tệp xuất ra.

***

## Phát hiện mục tiêu

Các cài đặt này kiểm soát cách Chloros phát hiện và xử lý các mục tiêu hiệu chuẩn trong hình ảnh của bạn. Cả hai chỉ hoạt động khi **Hiệu chuẩn phản xạ / cân bằng trắng** được bật (nếu không, chúng sẽ bị mờ đi vì quá trình phát hiện mục tiêu sẽ bị bỏ qua hoàn toàn).

### Diện tích mẫu hiệu chuẩn tối thiểu (px)

* **Loại**: Số
* **Phạm vi**: 0 đến 10.000 pixel
* **Mặc định**: 25 pixel
* **Mô tả**: Đặt diện tích tối thiểu (tính bằng pixel) cần thiết để một vùng được phát hiện được coi là mẫu mục tiêu hiệu chuẩn hợp lệ. Các giá trị nhỏ hơn sẽ phát hiện được các mục tiêu nhỏ hơn nhưng có thể làm tăng số lần phát hiện sai. Các giá trị lớn hơn yêu cầu các vùng mục tiêu lớn hơn và rõ ràng hơn để phát hiện.
* **Khi nào cần điều chỉnh**:
  * Tăng giá trị nếu bạn gặp các phát hiện sai trên các chi tiết nhỏ trong hình ảnh
  * Giảm giá trị nếu các mục tiêu hiệu chuẩn của bạn xuất hiện nhỏ trong hình ảnh và không được phát hiện

### Độ tập trung tối thiểu của mục tiêu (0-100)

* **Loại**: Số
* **Phạm vi**: 0 đến 100
* **Mặc định**: 60
* **Mô tả**: Điều khiển ngưỡng nhóm để gộp các vùng có màu tương tự khi phát hiện mục tiêu hiệu chuẩn. Các giá trị cao hơn yêu cầu nhiều màu tương tự hơn phải được gộp lại với nhau, dẫn đến việc phát hiện mục tiêu thận trọng hơn. Các giá trị thấp hơn cho phép sự biến đổi màu sắc nhiều hơn trong một nhóm mục tiêu.
* **Khi nào cần điều chỉnh**:
  * Tăng giá trị nếu các mục tiêu hiệu chuẩn bị chia thành nhiều phát hiện riêng biệt
  * Giảm giá trị nếu các mục tiêu hiệu chuẩn có sự biến thiên màu sắc không được phát hiện đầy đủ

***

## Xử lý

Các cài đặt này điều khiển cách Chloros xử lý và hiệu chuẩn hình ảnh của bạn.

### Chỉnh sửa hiệu ứng tối góc

* **Loại**: Hộp chọn
* **Mặc định**: Bật (được chọn)
* **Mô tả**: Áp dụng hiệu chỉnh hiện tượng tối góc để bù đắp cho hiện tượng tối ở các cạnh của hình ảnh do đặc tính của ống kính. Hiện tượng tối góc là hiện tượng quang học phổ biến, trong đó các góc và cạnh của hình ảnh trông tối hơn so với phần trung tâm do đặc tính của ống kính.
* **Tác động phụ**: Công tắc này cũng chọn *sản phẩm dự phòng chưa được hiệu chỉnh* mà quá trình xử lý sẽ ghi ra (xem bên dưới).

### Hiệu chỉnh độ phản xạ / cân bằng trắng

* **Loại**: Hộp chọn
* **Mặc định**: Bật (được chọn)
* **Mô tả**: Kích hoạt hiệu chuẩn phản xạ — dựa trên các mục tiêu hiệu chuẩn được phát hiện trong khung hình và/hoặc dữ liệu ánh sáng chiếu xuống từ cảm biến ánh sáng DAQ, tùy thuộc vào loại camera và các nguồn dữ liệu có sẵn. Tính năng này chuẩn hóa các giá trị phản xạ trên toàn bộ tập dữ liệu và đảm bảo các phép đo nhất quán bất kể điều kiện ánh sáng.
* **Khi tắt**: Việc phát hiện mục tiêu sẽ bị bỏ qua hoàn toàn, và**không camera nào có thể tạo ra sản phẩm phản xạ** — dù là chế độ dựa trên mục tiêu của Survey3 hay chế độ dựa trên DAQ của LATTICE. Các cài đặt liên quan (*Xuất phản xạ*, *Khoảng thời gian hiệu chuẩn lại tối thiểu* và các ngưỡng phát hiện mục tiêu) sẽ bị mờ đi.

### Sản phẩm dự phòng khi chưa hiệu chuẩn: Xuất phản hồi cảm biến / Xuất đã hiệu chỉnh hiệu ứng viền tối

* **Loại**: Hai ô chọn
* **Mặc định**: Cả hai đều được bật (đã chọn)
* **Mô tả**: Khi một khung hình không thể được hiệu chuẩn phản xạ (không tìm thấy mục tiêu hiệu chuẩn hoặc tính năng hiệu chuẩn phản xạ bị tắt), nó sẽ được ghi lại dưới dạng *sản phẩm dự phòng chưa hiệu chuẩn*. **Chỉ có chính xác một trong hai sản phẩm dự phòng tồn tại cho mỗi lần chạy, đối với mọi mẫu máy ảnh**, được chọn bởi công tắc *Chỉnh sửa hiệu ứng viền tối*:
  * Chức năng **chỉnh sửa hiệu ứng viền tối** **bật**→ `Vignette_Corrected_Images/` (được điều khiển bởi tùy chọn**Xuất dữ liệu đã chỉnh sửa hiệu ứng viền tối**)
  * Chức năng **chỉnh sửa hiệu ứng viền tối** **tắt**→ `Sensor_Response_Images/` (được điều khiển bởi tùy chọn**Xuất phản ứng cảm biến**)
* Sản phẩm dự phòng không được sử dụng sẽ bị mờ đi. Bỏ chọn sản phẩm đang được sử dụng sẽ ngăn không cho tệp đó được ghi ra hoàn toàn.

### Sản phẩm xuất LATTICE

Đối với các dự án chứa dữ liệu chụp từ LATTICE, mỗi khung hình LATTICE được nhập vào sẽ được phân tách thành tất cả các sản phẩm **được kích hoạt** **và áp dụng được**trong một lần xử lý duy nhất. Bốn ô chọn kiểm soát quá trình phân tách (tất cả mặc định đều**bật**):

| Cài đặt | Thư mục đầu ra | Nội dung xuất |
| --- | --- | --- |
| **Xuất hình ảnh đã loại bỏ mô-đun** | `Debayered_Images/` | Hình ảnh tuyến tính đã loại bỏ mô-đun: đã loại bỏ độ lệch tối của gói hiệu chuẩn, sau đó thực hiện khử mô-zaic nhận diện cạnh, không có hiệu chỉnh tuyến tính, trường phẳng hoặc tách kênh — cùng tệp mà bản chụp lưu ở mức đã khử mô-zaic. Giữ nguyên hướng gốc của cảm biến (được ghi trong XMP). Áp dụng cho máy ảnh RGB và đa phổ. |
| **Xuất bản xem trước** | `Preview_Images/` | Bản xem trước trên màn hình. RGB: cân bằng trắng (nguồn sáng DAQ nếu có, nếu không thì chế độ "gray-world") cộng với gamma; đa phổ: kéo dãn màu giả. |
| **Xuất độ rạng rỡ** | `Radiance_Images/` | Độ rạng rỡ quang phổ dạng Float32 tính bằng W/m²/sr/nm. Chỉ áp dụng cho đa quang phổ (M3C/M3M) — không áp dụng cho bản gốc RGB. Luôn được ghi dưới dạng tệp TIFF 32-bit bất kể cài đặt *Định dạng hình ảnh đã hiệu chuẩn* là gì. |
| **Xuất độ phản xạ**| `Reflectance_Calibrated_Images/` | Độ phản xạ Uint16, được quy đổi sao cho**32768 = độ phản xạ 1.0** (được đóng dấu dưới dạng XMP `Chloros:PixelScale`). Chỉ áp dụng cho dữ liệu đa phổ, được ghi khi có bản ghi hướng xuống tương ứng `.daq` (hoặc mục tiêu trong khung hình đã qua kiểm tra chất lượng) bao phủ khung hình. |

* Các camera chính RGB phát ra dữ liệu đã loại bỏ hiệu ứng Bayer + bản xem trước; dữ liệu độ rạng rỡ/độ phản xạ bị bỏ qua đối với chúng vì không áp dụng.
* Độ sâu bit của dữ liệu đã loại bỏ hiệu ứng Bayer/bản xem trước tuân theo cài đặt *Định dạng hình ảnh đã hiệu chuẩn*; độ rạng rỡ luôn là float32.
* Quá trình xử lý Survey3 không bị ảnh hưởng bởi bốn tùy chọn bật/tắt này.
* Các sản phẩm độ rạng rỡ và độ phản xạ của camera đa quang phổ M3C cũng có thể ghi một **mặt nạ giới hạn** cho từng pixel vào thư mục `clamp_masks/` bên cạnh các thư mục sản phẩm, ghi lại vị trí mà quá trình tách dải đã giới hạn giá trị âm xuống 0. Xem [Định dạng hình ảnh đầu ra](../output-image-formats.md#clamp-masks).

Bốn tùy chọn này cũng tồn tại dưới dạng không giao diện đồ họa (headless) với tên `chloros-cli process --debayered / --preview / --radiance / --reflectance`. Chúng đã thay thế cờ CLI cũ `--radiometric-output`, hiện không còn tồn tại.

{% hint style="warning" %}
**Việc tắt tất cả các sản phẩm áp dụng sẽ khiến quá trình xử lý thất bại.** Kể từ phiên bản 1.2.0, một quá trình xử lý được yêu cầu tạo sản phẩm nhưng không ghi ra bất kỳ sản phẩm hình ảnh nào sẽ báo lỗi và CLI sẽ thoát với giá trị không bằng 0, thay vì báo cáo thành công im lặng. Nhật ký sẽ nêu tên sản phẩm không thể ghi và lý do. Một quá trình xử lý chỉ chứa metadata một cách có chủ đích (không yêu cầu gì) vẫn được coi là thành công.
{% endhint %}

### Nguồn phản xạ (cài đặt dự án, thiết lập qua CLI)

Dự án cũng lưu trữ thông tin về **mẫu tham chiếu phản xạ** mà sản phẩm phản xạ LATTICE sử dụng. Không có điều khiển chuyên dụng trong bảng cài đặt; giá trị được lưu trữ trong cấu hình dự án dưới dạng `Processing → "Target reflectance source"` và được thiết lập bằng `chloros-cli process --reflectance-source {auto,target,daq}` (tham số `configure()` trong SDK không có từ khóa tương ứng):

* **`auto`** (mặc định): mục tiêu hiệu chuẩn trong khung hình đã vượt qua kiểm tra chất lượng (QA) sẽ trở thành tham chiếu tuyệt đối; nếu không có mục tiêu hoặc kiểm tra QA thất bại, hệ thống sẽ chuyển sang sử dụng tỷ lệ chia tia xuống của DAQ (ρ = πL/E). Một mục tiêu vượt qua kiểm tra chất lượng (QA) với cờ báo lỗi (một tấm panel bị rơi hoặc một dải tần bị lỗi) sẽ không tự động được ưu tiên: các chuỗi mục tiêu, mục tiêu + DAQ và chỉ DAQ sẽ được thử theo thứ tự điểm số đo được trên các tấm panel được giữ lại.
* **`target`**: độ phản xạ dựa hoàn toàn vào mục tiêu — không thay thế bằng DAQ.
* **`daq`**: độ phản xạ do DAQ quyết định; các mục tiêu trong khung hình không được sử dụng làm tham chiếu.

Giá trị được lưu trữ được so khớp không phân biệt chữ hoa chữ thường và một số cách viết khác được chấp nhận như các tên gọi khác: `target`, `target_image`, `empirical` và `empirical_line` đều có nghĩa là **mục tiêu**; `daq`, `dls`, `light_sensor` và `sensor` đều có nghĩa là**daq**. Bất kỳ giá trị nào khác — bao gồm cả trường hợp không có khóa — sẽ được giải quyết thành**auto**.

Các lần quét mục tiêu **đo lường** theo đơn vị được tra cứu dựa trên số sê-ri/QR của đơn vị mục tiêu, như `<serial>.csv`, tại ba vị trí: thư mục được chỉ định với `--target-reflectance-dir` (được lưu trữ dưới dạng `Processing → "Target reflectance dir"`), thư mục `target_reflectance/` riêng của dự án, và đường dẫn trong biến môi trường `CHLOROS_TARGET_REFLECTANCE_DIR`. Khi không có bản quét đo lường nào tồn tại cho đơn vị đó, đường cong danh nghĩa đã công bố của mô hình mục tiêu sẽ được sử dụng thay thế.

### Phương pháp Debayer

* **Loại**: Chọn từ danh sách thả xuống
* **Tùy chọn**:
  * Tiêu chuẩn (Nhanh, Chất lượng trung bình)
  * Nhận diện kết cấu (Chậm, Chất lượng cao nhất) \[Chloros+]
* **Mặc định**: Tiêu chuẩn (Nhanh, Chất lượng trung bình)
* **Mô tả**: Chọn thuật toán khử mô-zaic được sử dụng để chuyển đổi dữ liệu thô từ cảm biến theo mẫu Bayer thành hình ảnh đầy đủ màu sắc. Phương pháp "Tiêu chuẩn (Nhanh, Chất lượng trung bình)" mang lại sự cân bằng tối ưu giữa tốc độ xử lý và chất lượng hình ảnh. Phương pháp "Nhận diện kết cấu (Chậm, Chất lượng cao nhất)" \[Chloros+] sử dụng thuật toán khử mẫu nhận diện cạnh chất lượng cao kết hợp với mô hình khử nhiễu AI/ML, giúp loại bỏ gần như toàn bộ nhiễu do quá trình khử mẫu gây ra. Mô hình Nhận diện kết cấu yêu cầu bộ nhớ GPU (VRAM) để chạy. Chúng tôi khuyên bạn nên sử dụng mô hình này khi có sẵn >4GB VRAM để xử lý nhanh hơn.
* **Khi hàng đó là menu thả xuống**: menu thả xuống hai tùy chọn chỉ xuất hiện khi**cả hai** điều kiện đều đúng — bạn đã đăng nhập với gói đăng ký Chloros+ đủ điều kiện, **và** dự án không chứa bất kỳ khung hình LATTICE nào. Nếu không, hàng này sẽ hiển thị dưới dạng văn bản thuần túy với nội dung `Standard (Fast, Medium Quality)` mà không có tùy chọn nào để chọn.
* **Lưu ý về LATTICE**: Không có mô hình Texture Aware nào được huấn luyện cho LATTICE, và quy trình xử lý sẽ ép buộc sử dụng phương pháp demosaic tiêu chuẩn cho các khung hình LATTICE bất kể giá trị được lưu trữ là gì. Nếu bạn thêm một thư mục LATTICE vào dự án đã chọn Texture Aware trước đó, Chloros sẽ ghi đè cài đặt này về Standard thay vì giữ lại giá trị cũ trong `project.json`.

### Khoảng thời gian hiệu chuẩn lại tối thiểu

* **Loại**: Số
* **Phạm vi**: 0 đến 3.600 giây
* **Mặc định**: 0 giây
* **Mô tả**: Đặt khoảng thời gian tối thiểu (tính bằng giây) giữa các lần sử dụng mục tiêu hiệu chuẩn. Khi đặt thành 0, Chloros sẽ sử dụng mọi mục tiêu hiệu chuẩn được phát hiện. Khi đặt thành giá trị cao hơn, Chloros sẽ chỉ sử dụng các mục tiêu hiệu chuẩn cách nhau ít nhất số giây này, giúp giảm thời gian xử lý cho các bộ dữ liệu có tần suất chụp mục tiêu hiệu chuẩn cao.
* **Khi nào cần điều chỉnh**:
  * Đặt thành 0 để đạt độ chính xác hiệu chuẩn tối đa khi điều kiện ánh sáng thay đổi
  * Tăng giá trị (ví dụ: lên 60–300 giây) để xử lý nhanh hơn khi ánh sáng ổn định và bạn có nhiều hình ảnh mục tiêu hiệu chuẩn

### Chênh lệch múi giờ của cảm biến ánh sáng

* **Loại**: Số
* **Phạm vi**: -12 đến +12 giờ
* **Mặc định**: 0 giờ
* **Mô tả**: Chỉ định độ lệch múi giờ (tính bằng giờ so với UTC) cho dấu thời gian của dữ liệu cảm biến ánh sáng, được sử dụng khi đối chiếu nhật ký cảm biến ánh sáng với thời gian chụp ảnh. Các bản ghi `.daq` mới hơn có nguồn gốc múi giờ riêng, do đó cài đặt này chủ yếu cần thiết cho các nhật ký cũ được ghi lại theo giờ địa phương.

### Áp dụng hiệu chỉnh PPK

* **Loại**: Hộp chọn
* **Mặc định**: Tắt (chưa được chọn)
* **Mô tả**: Cho phép sử dụng các hiệu chỉnh Động học Sau Xử lý (PPK) từ các thiết bị ghi dữ liệu DAQ của MAPIR có tích hợp GPS (GNSS). Khi được bật, Chloros sẽ sử dụng bất kỳ tệp nhật ký .daq nào chứa dữ liệu điểm phơi sáng trong thư mục dự án của bạn và áp dụng các hiệu chỉnh vị trí địa lý chính xác cho hình ảnh của bạn.
* **Yêu cầu**: Phải có tệp nhật ký .daq chứa các mục ghim phơi sáng trong thư mục dự án của bạn
* **Khi nào nên bật**: Nên luôn bật hiệu chỉnh PPK nếu tệp nhật ký .daq của bạn có các mục phản hồi phơi sáng.

### Chân phơi sáng 1

* **Loại**: Chọn từ danh sách thả xuống
* **Tính khả dụng**: Chỉ hiển thị khi tính năng "Áp dụng hiệu chỉnh PPK" được bật VÀ có dữ liệu phơi sáng cho Pin 1
* **Tùy chọn**:
  * Tên mẫu máy ảnh được phát hiện trong dự án
  * "Không sử dụng" - Bỏ qua điểm phơi sáng này
* **Mặc định**: Được chọn tự động dựa trên cấu hình dự án
* **Mô tả**: Gán một máy ảnh cụ thể cho Chân phơi sáng 1 để đồng bộ hóa thời gian PPK. Chân phơi sáng ghi lại thời điểm chính xác khi màn trập máy ảnh được kích hoạt, điều này rất quan trọng để xác định vị trí địa lý PPK chính xác.
* **Hành vi chọn tự động**:
  * Một camera + một chân phơi sáng: Tự động chọn camera
  * Một camera + hai chân phơi sáng: Chân phơi sáng 1 tự động được gán cho camera
  * Nhiều camera: Cần chọn thủ công

### Chân phơi sáng 2

* **Loại**: Chọn từ danh sách thả xuống
* **Tính hiển thị**: Chỉ hiển thị khi "Áp dụng hiệu chỉnh PPK" được bật VÀ có dữ liệu phơi sáng cho Chân 2
* **Tùy chọn**:
  * Tên các mẫu camera được phát hiện trong dự án
  * "Không sử dụng" - Bỏ qua chân phơi sáng này
* **Mặc định**: Được chọn tự động dựa trên cấu hình dự án
* **Mô tả**: Gán một camera cụ thể cho Chân phơi sáng 2 để đồng bộ hóa thời gian PPK khi sử dụng cấu hình hai camera.
* **Cách thức chọn tự động**:
  * Một máy ảnh + một chân: Chân 2 tự động được đặt thành "Không sử dụng"
  * Một máy ảnh + hai chân: Chân 2 tự động được đặt thành "Không sử dụng"
  * Nhiều máy ảnh: Yêu cầu chọn thủ công
* **Lưu ý**: Không thể gán cùng một camera cho cả chân 1 và chân 2 cùng lúc.

***

## Cảm biến ánh sáng DAQ

Phần này xuất hiện trong Cài đặt Dự án và liệt kê tất cả các tệp ghi dữ liệu ánh sáng hướng xuống (downwelling) của DAQ trong dự án — các bản ghi `.daq` và nhật ký ghi dữ liệu hướng xuống DAQ-M `.csv`. Các bản ghi được thực hiện trong tab Cảm biến ánh sáng sẽ tự động được thêm vào dự án đang mở.

<!-- SCREENSHOT-NEEDED: Project Settings "DAQ Light Sensor" section of a project containing at least one .daq file, showing the "Cap override (all files)" dropdown and a per-file row with its resolved cap. -->



Mỗi hàng hiển thị tệp, mẫu cảm biến và hiệu chỉnh nắp khuếch tán hiện đang được áp dụng cho tệp đó. Phía trên các hàng là một tùy chọn điều khiển chung cho toàn dự án:

### Ghi đè cài đặt nắp (tất cả tệp)

* **Loại**: Chọn từ danh sách thả xuống
* **Tùy chọn**: `Auto` cùng với các cấu hình hiệu chỉnh nắp áp dụng cho các loại cảm biến có trong dự án
* **Mặc định**: Tự động
* **Lưu dưới dạng**: `Processing → "DAQ cap id"` (mặc định là `auto`)
* **Mô tả**: `Auto` sử dụng nắp được ghi lại trong từng tệp (giả định là nắp Sunshine nếu không có dữ liệu ghi lại, nhưng DAQ-U, DAQ-M và DAQ-E được cung cấp mà không có nắp và Chloros không thể xác định nắp nào được lắp, nên quá trình xử lý sẽ cảnh báo về nắp được giả định: hãy chọn nắp đã được lắp đặt). Việc chọn một nắp cụ thể sẽ ghi đè lên **tất cả** tệp dữ liệu hướng xuống trong dự án, ngoại trừ các tệp từ DAQ-E-S (vì bộ khuếch tán của nó là một phần của quá trình hiệu chuẩn); một dự án chỉ chứa DAQ-E-S sẽ không hiển thị menu thả xuống: các bản ghi thô sẽ được hiệu chỉnh bằng nắp đó, và các bản ghi đã có nắp sẽ được tham chiếu lại (sự hiệu chỉnh đã ghi sẽ bị hủy bỏ và hiệu chỉnh đã chọn sẽ được áp dụng).
* **Quan trọng**: Nắp đã chọn phải khớp với nắp đã được lắp đặt thực tế trong quá trình ghi. Cả cảm biến lẫn phần mềm đều không thể phát hiện nắp vật lý — ID nắp không khớp sẽ hiệu chỉnh sai phổ.

Có **một** tùy chọn điều khiển chung cho toàn bộ dự án thay vì các menu thả xuống riêng lẻ cho từng tệp: cài đặt này áp dụng cho mọi nguồn ánh sáng chiếu xuống trong dự án.

***

## Căn chỉnh mảng

Phần này **chỉ** xuất hiện khi có ít nhất một hình ảnh trong dự án chứa biến đổi căn chỉnh giữa các mô-đun mà các mảng LATTICE gắn vào tại thời điểm chụp (thẻ XMP `Chloros:Alignment*`). Phần này hiển thị số lượng hình ảnh có thẻ căn chỉnh, máy ảnh nào là tham chiếu (thẻ `REF`) và bảng thống kê số lượng hình ảnh theo từng máy ảnh.

<!-- SCREENSHOT-NEEDED: Project Settings "Array Alignment" section for an imported LATTICE array capture set, showing the tagged-image count, the per-camera rows with the REF badge, and the three controls (Apply array alignment, Crop to common overlap, Resampling). -->

### Áp dụng căn chỉnh mảng

* **Loại**: Hộp chọn
* **Mặc định**: Đã bật (được chọn)
* **Lưu dưới dạng**: `Processing → "Array alignment"`
* **Mô tả**: Biến dạng mọi sản phẩm đã xử lý (debayered / xem trước / radiance / phản xạ / chỉ số) vào hình học tham chiếu chung của mảng bằng cách sử dụng phép biến đổi được ghi lại tại thời điểm chụp. Tắt = xuất dưới dạng hình học gốc của từng cảm biến.

### Cắt theo vùng chồng chéo chung

* **Loại**: Hộp chọn (chỉ hoạt động khi *Áp dụng căn chỉnh mảng* được bật)
* **Mặc định**: Bật (được chọn)
* **Lưu dưới dạng**: `Processing → "Array alignment crop"`
* **Mô tả**: Cắt các tệp xuất đã được căn chỉnh để chỉ giữ lại vùng mà tất cả các mô-đun camera đều chia sẻ, do đó mỗi dải phổ sẽ có cùng kích thước vùng quét. Tắt sẽ giữ nguyên khung hình cảm biến đầy đủ (điền màu đen bên ngoài vùng nguồn).

### Lấy mẫu lại

* **Loại**: Menu thả xuống (chỉ hoạt động khi *Áp dụng căn chỉnh mảng* được bật)
* **Tùy chọn**: `Bilinear (smooth, default)`, `Nearest (preserve exact values)`, `Cubic (sharpest)`
* **Mặc định**: Bilinear
* **Lưu dưới dạng**: `Processing → "Array alignment interpolation"`
* **Mô tả**: Phương pháp nội suy được sử dụng bởi quá trình biến dạng căn chỉnh. Nearest giữ nguyên các giá trị chính xác của nguồn (không trộn giữa các pixel) cho phân tích phóng xạ nghiêm ngặt; Bilinear là lựa chọn tốt nhất cho việc lập bản đồ và sử dụng trực quan.

Ba tùy chọn này cũng tồn tại dưới dạng không có tiền tố là `chloros-cli process --array-alignment`, `--array-alignment-crop` và `--array-alignment-interp {bilinear,nearest,cubic}`.

***

## Chỉ số

Các cài đặt này cho phép bạn định cấu hình các chỉ số đa quang phổ để phân tích và hiển thị.

### Thêm chỉ số

* **Loại**: Bảng cấu hình chỉ số đặc biệt
* **Mô tả**: Mở một bảng điều khiển tương tác nơi bạn có thể chọn và cấu hình các chỉ số thực vật đa phổ (NDVI, NDRE, EVI, v.v.) để tính toán trong quá trình xử lý hình ảnh. Bạn có thể thêm nhiều chỉ số, mỗi chỉ số có cài đặt hiển thị riêng.
* **Các chỉ số có sẵn**: Danh sách thả xuống trên giao diện người dùng (GUI) bao gồm**27** công thức chỉ số đa phổ được định nghĩa sẵn (xem [Công thức chỉ số đa phổ](multispectral-index-formulas.md) để xem danh sách đầy đủ, bao gồm các tên cũng được chấp nhận bởi tùy chọn CLI/SDK `--indices`).
* **Tính năng**:
  * Chọn từ các công thức chỉ số được định nghĩa sẵn
  * Kéo các kênh bộ lọc của camera vào các ô dải phổ của công thức
  * Cấu hình dải màu hiển thị (LUT - Bảng tra cứu)
  * Đặt các giá trị ngưỡng và chế độ cắt
  * Tạo các công thức chỉ số tùy chỉnh
* **Lưu ý**: Các chỉ số không được tính toán cho các camera đơn dải LATTICE M3M — các chỉ số đa dải không được xác định trên một dải duy nhất. Các model Survey3 và LATTICE M3C không bị ảnh hưởng.

<!-- SCREENSHOT-NEEDED: Project Settings > Index section with one index added and expanded: the filter dropdown, the formula dropdown open showing preset names, the coloured channel circles above the rendered formula, and the "+ Add LUT" button below it. -->



Mỗi chỉ số bạn thêm sẽ hiển thị công thức dưới dạng toán học, với một vòng tròn màu cho mỗi khe dải: đỏ = Red, xanh lá = Green, xanh dương = Blue, cam = Orange, lục lam = Cyan, tím = NIR, magenta = RE. Kéo một vòng tròn từ hàng phía trên công thức vào một khe để liên kết; nhấp đúp vào khe đã liên kết để xóa liên kết. Chỉ số chỉ được tính khi mỗi khe mà công thức sử dụng đều có kênh tương ứng.

### Công thức tùy chỉnh (Tính năng Chloros+)

* **Loại**: Mảng các định nghĩa công thức tùy chỉnh
* **Tính khả dụng**: Yêu cầu đăng nhập bằng gói đăng ký Chloros+ đủ điều kiện.
* **Mô tả**: Cho phép bạn tạo và lưu các công thức chỉ số đa phổ tùy chỉnh bằng toán học dải. Các công thức tùy chỉnh được lưu cùng với cài đặt dự án của bạn và có thể được sử dụng giống như các chỉ số tích hợp sẵn.
* **Cách tạo**:
  1. Trong bảng cấu hình Chỉ số, mở công cụ tính toán công thức tùy chỉnh
  2. Viết công thức bằng cách sử dụng các **ký hiệu dải-khe**, không phải tên dải
  3. Lưu công thức với một tên mô tả — công thức sẽ xuất hiện ở cuối danh sách thả xuống, và bạn có thể kéo các vòng tròn kênh của máy ảnh vào các khe tương ứng, giống hệt như khi sử dụng cài đặt sẵn tích hợp sẵn
* **Cú pháp công thức**:
  * Khe dải phổ: `x`, `y`, `z`, `a`, `b`, `c` — sáu vị trí mà bạn có thể gán cho các kênh thực bằng cách kéo thả
  * Các toán tử: `+`, `-`, `*`, `/`, `^` và `()` để nhóm
  * Chức năng: `sqrt()`, `log()`, `ln()`, `abs()`, `sign()`, `log1p()`, `log2()`
* **Tại sao lại dùng ký hiệu chứ không phải tên ban nhạc**: một công thức được viết dưới dạng `(y-x)/(y+x)` hoạt động trên bất kỳ máy ảnh nào, vì bản đồ kéo-thả sẽ quyết định liệu `y` là dải NIR 850 nm của bộ lọc RGN hay dải NIR 808 nm của bộ lọc OCN. Các các cài đặt sẵn tích hợp cũng được lưu trữ theo cách tương tự — xem [Công thức chỉ số đa quang phổ](multispectral-index-formulas.md) để biết dạng ký hiệu chính xác của tất cả 27 công thức.
* **Nơi chúng hoạt động**: các công thức tùy chỉnh được lưu cùng với cài đặt dự án và có thể được sử dụng trong [Index/LUT Sandbox](../image-viewer-gui/index-lut-sandbox.md) cũng như trong quá trình xử lý. Chúng**không** được hỗ trợ bởi danh sách tên trong CLI/SDK `--indices`, vốn chỉ mở rộng 22 tên cài đặt sẵn tích hợp sẵn.

***

## Xuất

Các cài đặt này kiểm soát định dạng và chất lượng của hình ảnh đã xử lý khi xuất.

### Định dạng hình ảnh đã hiệu chuẩn

* **Loại**: Chọn từ danh sách thả xuống
* **Tùy chọn**:
  * **TIFF (16-bit)** - Định dạng TIFF 16-bit không nén
  * **TIFF (8-bit)** - Định dạng TIFF 8-bit không nén
  * **TIFF (32-bit, Phần trăm)** - TIFF 32-bit số thập phân với các giá trị phản xạ được biểu thị dưới dạng phần trăm
  * **PNG (8-bit)** - Định dạng PNG 8-bit nén
  * **JPG (8-bit)** - Định dạng JPEG 8-bit nén
* **Mặc định**: TIFF (16-bit); dự án toàn 8-bit sẽ hiển thị TIFF (8-bit) và chỉ cung cấp các định dạng 8-bit
* **Mô tả**: Chọn định dạng tệp để lưu các hình ảnh đã xử lý và hiệu chỉnh. Các tệp xuất ra sẽ được lưu trong các thư mục con tương ứng với từng định dạng bên trong thư mục của từng máy ảnh (`tiff16`, `tiff32`, `png8`, `jpg8`), với một thư mục `<Product>_Images/` cho mỗi sản phẩm. Các tệp đã xuất giữ nguyên tên tệp nguồn — thư mục, chứ không phải phần mở rộng tên tệp, là yếu tố xác định sản phẩm.
* **Khuyến nghị về định dạng**:
  * **TIFF (16-bit)**: Được khuyến nghị cho phân tích khoa học và quy trình làm việc chuyên nghiệp. Giữ nguyên chất lượng dữ liệu tối đa mà không có hiện tượng méo do nén. Phù hợp nhất cho phân tích đa phổ và xử lý tiếp theo trong phần mềm GIS.
  * **Định dạng TIFF (32-bit, Phần trăm)**: Phù hợp nhất cho các quy trình làm việc yêu cầu giá trị phản xạ dưới dạng phần trăm (0-100%). Cung cấp độ chính xác tối đa cho các phép đo bức xạ.
  * **PNG (8-bit)**: Phù hợp để xem trên web và hiển thị chung. Kích thước tệp nhỏ hơn nhờ nén không mất dữ liệu, nhưng dải động bị giảm.
  * **JPG (8-bit)**: Kích thước tệp nhỏ nhất, chỉ phù hợp nhất cho xem trước và hiển thị trên web. Sử dụng nén có mất dữ liệu, không phù hợp cho phân tích khoa học.
* **Lưu ý**: Dữ liệu radiance của LATTICE luôn được xuất dưới dạng tệp TIFF 32-bit float bất kể cài đặt này.

***

## Lưu Mẫu Dự Án

Tính năng này cho phép bạn lưu các thiết lập dự án hiện tại dưới dạng một mẫu có thể tái sử dụng.

* **Loại**: Ô nhập văn bản + Nút Lưu
* **Mô tả**: Nhập tên mô tả cho mẫu thiết lập của bạn và nhấp vào biểu tượng lưu. Mẫu này sẽ lưu trữ tất cả các thiết lập dự án hiện tại của bạn (phát hiện mục tiêu, tùy chọn xử lý, chỉ số, và định dạng xuất) để dễ dàng tái sử dụng trong các dự án sau này. Các mẫu được lưu trữ trong thư mục `Project Templates/` bên trong thư mục lưu dự án của bạn, và cũng có thể được chọn hoặc xuất từ menu chính (*Chọn Mẫu* / *Lưu Mẫu* / *Xuất Mẫu*).
* **Các trường hợp sử dụng**:
  * Tạo mẫu cho các hệ thống camera khác nhau (RGB, đa quang phổ, NIR)
  * Lưu các cấu hình tiêu chuẩn cho các loại cây trồng cụ thể hoặc quy trình phân tích
  * Chia sẻ các cài đặt nhất quán trong toàn đội
* **Cách sử dụng**:
  1. Cấu hình tất cả các cài đặt dự án mong muốn
  2. Nhập tên mẫu (ví dụ: "RedEdge Survey3 NDVI Standard")
  3. Nhấp vào biểu tượng lưu
  4. Mẫu này giờ đây có thể được tải khi tạo các dự án mới

***

## Thư mục lưu dự án

Cài đặt này xác định vị trí lưu các dự án mới theo mặc định.

* **Loại**: Hiển thị đường dẫn thư mục + Nút Chỉnh sửa
* **Mặc định (Hệ điều hành Windows)**: `Documents\MAPIR\Chloros Projects` trong thư mục Tài liệu của bạn — thường là `C:\Users\[Username]\Documents\MAPIR\Chloros Projects`, hoặc bất kỳ vị trí nào mà OneDrive hoặc hồ sơ di động đã di chuyển thư mục Tài liệu
* **Mặc định (Hệ điều hành Linux)**: `~/Documents/MAPIR/Chloros Projects`
* **Mô tả**: Hiển thị thư mục mặc định hiện tại nơi các dự án Chloros mới được tạo. Nhấp vào biểu tượng chỉnh sửa để chọn một thư mục khác. Cài đặt tùy chỉnh này được lưu dưới dạng một dòng văn bản trong tệp `~/.chloros/working_directory.txt` — trên Windows, đó là tệp `C:\Users\<Username>\.chloros\working_directory.txt`. Nếu tệp đó bị thiếu hoặc chỉ định một đường dẫn không còn tồn tại, Chloros sẽ quay lại sử dụng giá trị mặc định ở trên. Giao diện dòng lệnh (CLI) đọc và ghi cùng một tệp, do đó `chloros-cli` và giao diện đồ họa (GUI) luôn thống nhất về vị trí lưu trữ các dự án.
* **Các mẫu dự án** được lưu trữ trong thư mục con `Project Templates/` của thư mục này.
* **Khi nào nên thay đổi**:
  * Đặt thành ổ đĩa mạng để hợp tác nhóm
  * Chuyển sang ổ đĩa có dung lượng lưu trữ lớn hơn cho các tập dữ liệu lớn
  * Sắp xếp các dự án theo năm, khách hàng hoặc loại dự án vào các thư mục khác nhau
* **Lưu ý**: Việc thay đổi cài đặt này chỉ ảnh hưởng đến các dự án MỚI. Các dự án hiện có vẫn giữ nguyên vị trí ban đầu.
* **Độ dài đường dẫn Windows**: trừ khi tính năng đường dẫn dài của Windows được bật (tùy chọn **Bật đường dẫn dài Windows** trong trình cài đặt), thư mục dự án có độ sâu vượt quá khoảng 130 ký tự sẽ bị từ chối trước khi xử lý, vì các sản phẩm của nó sẽ vượt quá giới hạn 260 ký tự của đường dẫn Windows. Xem [Bắt đầu xử lý](../processing-images-gui/starting-the-processing.md#the-project-folder-is-too-deep-for-this-computer).

***

## Lưu trữ cài đặt

Một dự án Chloros là một **thư mục**. Tất cả cài đặt dự án được lưu trong tệp `project.json` bên trong thư mục đó; thiết bị phần cứng được kết nối cũng được ghi nhớ cùng với chúng trong các tệp `cameras.json` và `sensors.json`, do đó khi mở lại dự án, các camera và cảm biến ánh sáng của dự án cũng sẽ được kết nối lại. Khi bạn mở lại dự án, tất cả cài đặt sẽ được khôi phục chính xác như khi bạn đã lưu. Các dự án đã lưu cũng có thể được điều khiển ở chế độ không giao diện người dùng (headless) thông qua tệp `chloros-cli project` hoặc tệp `open_project` trong bộ công cụ phát triển phần mềm (SDK).

### Cấu trúc phân cấp cài đặt

Các cài đặt được áp dụng theo thứ tự sau:

1. **Cài đặt mặc định của hệ thống** - Các giá trị mặc định tích hợp được định nghĩa bởi Chloros

2. **Cài đặt mẫu** - Nếu bạn tải một mẫu khi tạo dự án
3. **Cài đặt dự án đã lưu** - Các cài đặt được lưu cùng với tệp dự án
4. **Điều chỉnh thủ công** - Bất kỳ thay đổi nào bạn thực hiện trong phiên hiện tại

### Cài đặt và xử lý hình ảnh

Các cài đặt xử lý được đọc khi quá trình xử lý bắt đầu. Việc thay đổi cài đặt sẽ không tác động ngược lại đến các sản phẩm đã có trên đĩa — hãy chạy lại quá trình xử lý để áp dụng các cài đặt mới. Một số cài đặt không bao giờ ảnh hưởng đến quá trình xử lý:

* Độ phân giải hình thu nhỏ (chỉ hiển thị)
* Lưu mẫu dự án
* Lưu thư mục dự án

***

## Tham chiếu khóa cấu hình

Đối với tự động hóa (CLI `--config`, SDK `configure` hoặc đọc trực tiếp `project.json`), đây là các khóa chính xác trong `Project Settings`:

| Đường dẫn khóa | Loại | Giá trị mặc định |
| --- | --- | --- |
| `Display → Image Thumbnail Resolution` | `"512" \| "1024" \| "2048" \| "full"` | `"512"` |
| `Target Detection → Minimum calibration sample area (px)` | số từ 0 đến 10000 | `25` |
| `Target Detection → Minimum Target Clustering (0-100)` | số từ 0 đến 100 | `60` |
| `Processing → Vignette correction` | bool | `true` |
| `Processing → Reflectance calibration / white balance` | bool | `true` |
| `Processing → Export sensor response` | bool | `true` |
| `Processing → Export vignette corrected` | bool | `true` |
| `Processing → Export debayered` | bool | `true` |
| `Processing → Export preview` | bool | `true` |
| `Processing → Export radiance` | bool | `true` |
| `Processing → Export reflectance` | bool | `true` |
| `Processing → Array alignment` | bool | `true` |
| `Processing → Array alignment crop` | bool | `true` |
| `Processing → Array alignment interpolation` | `"Bilinear" \| "Nearest" \| "Cubic"` | `"Bilinear"` |
| `Processing → Debayer method` | `"Standard (Fast, Medium Quality)" \| "Texture Aware (Slow, Highest Quality)"` | Tiêu chuẩn |
| `Processing → Minimum recalibration interval` | số 0-3600 | `0` |
| `Processing → Light sensor timezone offset` | số từ -12 đến 12 | `0` |
| `Processing → Apply PPK corrections` | bool | `false` |
| `Processing → DAQ cap id` | ID cấu hình giới hạn hoặc `"auto"` | `"auto"` |
| `Processing → Target reflectance source` | `"auto" \| "target" \| "daq"` | `"auto"` |
| `Index → Add index` | danh sách cấu hình chỉ mục | `[]` |
| `Export → Calibrated image format` | `"TIFF (16-bit)" \| "TIFF (8-bit)" \| "TIFF (32-bit, Percent)" \| "PNG (8-bit)" \| "JPG (8-bit)"` | `"TIFF (16-bit)"` |

Các khóa `Array alignment` được ghi lần đầu tiên khi phần Căn chỉnh Mảng được hiển thị hoặc khi một lệnh tự động hóa thiết lập chúng. Trong khi chúng vắng mặt, đường ống xử lý sẽ sử dụng các giá trị tương tự như đã hiển thị ở trên (`true`, `true`, bilinear), do đó một dự án .json không có các khóa này sẽ hoạt động giống hệt như một dự án có chúng.

### Các khóa được lưu trữ trong `project.json` mà không có tùy chọn điều khiển trong bảng cài đặt

Các khóa này nằm trong cùng một cây `Project Settings` và được quá trình xử lý đọc, nhưng bạn sẽ không tìm thấy widget dành cho chúng trong thanh bên:

| Đường dẫn khóa | Loại | Mặc định | Được đặt bởi |
| --- | --- | --- | --- |
| `Processing → LATTICE input level` | `"auto" \| "raw" \| "debayered" \| "processed"` | `"auto"` | `chloros-cli process --input-level`, SDK `input_level=`. Ghi đè cách diễn giải các tệp TIFF đầu vào của LATTICE; `auto` suy luận từ thẻ XMP `Chloros:ProcessingLevel` của từng tệp cùng với số kênh. Bị bỏ qua đối với các bản chụp `.raw` của Survey3. Cố ý không phải là cài đặt GUI — chế độ tự động luôn chính xác trong mọi trường hợp bình thường. |
| `Processing → Target reflectance dir` | chuỗi đường dẫn | `""` | `chloros-cli process --target-reflectance-dir`, hoặc mục tiêu dự án API |
| `Processing → Target reflectance config` | từ điển được lập chỉ mục theo số sê-ri camera | `{}` | Đăng ký mục tiêu trong khung hình (chế độ `fixed_block` / `fixed_strip` / `aruco`) |
| `Processing → DAQ-U log path` | chuỗi đường dẫn | `""` | SDK `process_folder(daq_log_path=…)`. Trỏ đến một bản ghi `.daq` hoặc một thư mục chứa các bản ghi đó |
| `Target Detection → Minimum calibration target squares` | số | `4` | Giá trị mặc định cũ; không có điều khiển và không có cờ CLI |
| `UI → Grid thumbnail size` | số | `160` | Thanh trượt thu phóng hình thu nhỏ riêng của lưới hình ảnh |

Hai tùy chọn trình xem được lưu trữ **ở cấp cao nhất trong `project.json`**, hoàn toàn bên ngoài `Project Settings`, vì chúng là trạng thái hiển thị chứ không phải cài đặt xử lý:

| Đường dẫn khóa | Loại | Giá trị mặc định | Được thiết lập bởi |
| --- | --- | --- | --- |
| `viewer_display → gsd_bin` | số nguyên 1–256 | `1` | Thanh điều khiển GSD (px) của tab hình ảnh — xem [Mở hình ảnh toàn màn hình](../image-viewer-gui/opening-an-image-full-screen.md) |

***

## Các phương pháp hay nhất

1. **Bắt đầu với các thiết lập mặc định**: Các thiết lập mặc định hoạt động tốt cho hầu hết các hệ thống máy ảnh MAPIR và các quy trình làm việc thông thường.
2. **Tạo mẫu**: Sau khi đã tối ưu hóa cài đặt cho một quy trình làm việc hoặc camera cụ thể, hãy lưu chúng dưới dạng mẫu để đảm bảo tính nhất quán giữa các dự án.
3. **Kiểm tra trước khi xử lý toàn bộ**: Khi thử nghiệm các cài đặt mới, hãy kiểm tra trên một tập con nhỏ các hình ảnh trước khi xử lý toàn bộ tập dữ liệu.
4. **Ghi chép lại các thiết lập**: Sử dụng tên mẫu mô tả rõ ràng, nêu rõ hệ thống camera, loại xử lý và mục đích sử dụng (ví dụ: “Survey3\_RGB\_NDVI\_Agriculture”).
5. **Lựa chọn định dạng xuất**: Chọn định dạng xuất dựa trên mục đích sử dụng cuối cùng:
   * Phân tích khoa học → TIFF (16-bit hoặc 32-bit)
   * Xử lý GIS → TIFF (16-bit)
   * Trình bày nhanh → PNG (8-bit)
   * Chia sẻ trên web → JPG (8-bit)

***

Để biết thêm thông tin về các chỉ số đa phổ trong Chloros, hãy xem trang [Công thức chỉ số đa phổ](multispectral-index-formulas.md).
