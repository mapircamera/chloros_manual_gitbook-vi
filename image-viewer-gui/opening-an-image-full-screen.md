# Mở hình ảnh ở chế độ toàn màn hình

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption><p>Một hình ảnh được mở ở chế độ toàn màn hình, với thanh chọn lớp ở góc trên bên phải</p></figcaption></figure>

Trình xem hình ảnh Chloros là giao diện toàn màn hình để xem, kiểm tra và đo lường hình ảnh của bạn. Đây là nơi bạn đọc được **giá trị pixel thực** — DN theo từng kênh, phần trăm phản xạ hoặc độ rực rỡ tính bằng W/m²/sr/nm — thay vì bản xem trước bị kéo giãn mà màn hình hiển thị.

## Truy cập Trình xem hình ảnh

### Từ Trình duyệt tệp

1. Mở tab **Trình duyệt tệp** <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line">
2. Nhấp vào bất kỳ **hình thu nhỏ** nào trong [lưới hình ảnh](image-grid.md)
3. Hình ảnh sẽ mở toàn màn hình trong tab **Trình xem hình ảnh**

Hình ảnh sẽ mở trên sản phẩm mà lưới đang hiển thị. Nếu lưới được đặt thành `RAW (Reflectance)`, đó chính là lớp mà bạn sẽ truy cập.

### Mở thanh bên Trình xem hình ảnh

Nhấp vào biểu tượng **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> ở thanh bên trái để mở bảng phân tích. Bảng này chứa, từ trên xuống dưới:

* tên hình ảnh và mẫu máy ảnh
* nút **Xuất/Lưu hình ảnh** (chỉ hiển thị khi có Chỉ số hoặc LUT đang hoạt động)
* các ô chọn **Chỉ số**và**LUT** cùng bảng cấu hình chỉ số — xem [Môi trường thử nghiệm Chỉ số/LUT](index-lut-sandbox.md)
* bảng **Giá trị con trỏ**: giá trị đọc theo từng kênh, biểu đồ histogram lớp và điều khiển GSD***

## Điều hướng và phóng to

### Duyệt qua các hình ảnh

* **Hình ảnh tiếp theo**: nút → hoặc phím**→** (Mũi tên phải)
* **Hình ảnh trước đó**: nút ← hoặc phím**←** (Mũi tên trái)
* **Chuyển đến một hình ảnh cụ thể**: quay lại lưới và nhấp vào hình thu nhỏ của hình ảnh đó

Các cài đặt thu phóng và di chuyển vẫn được duy trì khi bạn chuyển giữa các hình ảnh, do đó bạn có thể duyệt qua một bộ hình ảnh trong khi vẫn giữ nguyên vị trí trên cùng một phần của khung hình.

### Thu phóng

Chức năng thu phóng được điều khiển bằng **bánh xe chuột**, theo các bước 15%, cố định tại vị trí con trỏ — điểm dưới con trỏ sẽ luôn nằm dưới con trỏ. Phạm vi thu phóng bị giới hạn bởi kích thước hình ảnh và cửa sổ: bạn không thể thu nhỏ quá mức “phù hợp với cửa sổ”, và giới hạn trên được xác định bởi độ phân giải gốc của hình ảnh.

Không có phím chuyên dụng để thu phóng trong trình xem toàn màn hình. (Trong lưới, **Ctrl + `+` / `−`** điều chỉnh kích thước hình thu nhỏ — đây là một chức năng khác.)

### Di chuyển khi đang thu phóng

Nhấp và giữ nút chuột trái trên hình ảnh rồi kéo. Chức năng di chuyển bị giới hạn để hình ảnh không bị kéo ra khỏi màn hình.

### Kiểm tra từng pixel ở mức phóng to cao

Khi độ phóng đại hiệu dụng vượt quá **60×**, Chloros sẽ vẽ một khung nổi bật xung quanh pixel hiển thị dưới con trỏ chuột và hiển thị một giá trị nổi bên cạnh nó.

Độ phóng đại “hiệu dụng” tính dựa trên kích thước khối GSD: với kích thước khối là 8, vùng đánh dấu sẽ xuất hiện ở mức phóng đại 7,5× thay vì 60×, vì một pixel hiển thị đã tương ứng với 8 × 8 pixel nguồn. Thu nhỏ lại dưới ngưỡng này, vùng đánh dấu sẽ biến mất.

### Phím tắt

| Phím                             | Vị trí       | Hành động                              |
| ------------------------------- | ----------- | ----------------------------------- |
| **→**                           | Toàn màn hình | Hình ảnh tiếp theo                          |
| **←**                           | Toàn màn hình | Hình ảnh trước đó                      |
| **Ctrl + R**                    | Toàn màn hình | Đặt lại vùng thử nghiệm chỉ mục/LUT         |
| **Ctrl + `+`**/**Ctrl + `=`** | Lưới        | Hình thu nhỏ lớn hơn (4 px mỗi lần nhấn)  |
| **Ctrl + `−`**                  | Lưới        | Hình thu nhỏ nhỏ hơn (4 px mỗi lần nhấn) |***

## Giá trị con trỏ

Di chuyển con trỏ qua hình ảnh và bảng **Giá trị con trỏ** sẽ hiển thị giá trị của từng kênh bên dưới nó.

{% hint style="success" %}
**Đây là các giá trị thực của tệp.** Khung vẽ trên màn hình là bản xem trước được kéo giãn 8-bit và không thể cung cấp các giá trị này, do đó Chloros sẽ lấy mẫu từ tệp sản phẩm thực tế để hiển thị. Đó là lý do tại sao khung hình thô 12-bit hiển thị các giá trị lớn hơn 255, và tại sao lớp radiance float32 hiển thị các đơn vị vật lý.
{% endhint %}

### Ý nghĩa của các cột

Bảng điều khiển sẽ điều chỉnh theo lớp mà bạn đang xem:

| Lớp bạn đang xem              | Các cột hiển thị    | Ghi chú                                                                                           |
| ---------------------------------- | ---------------- | ----------------------------------------------------------------------------------------------- |
| Độ phản xạ                        | **DN**và**%** | Phần trăm được tính theo thang đo riêng của tệp đó — xem bên dưới                                      |
| Độ rạng rỡ                           | **W/m²/sr/nm**   | Giá trị vật lý dạng số thực; không có cột DN, vì DN không có ý nghĩa ở đây                           |
| Dữ liệu thô / Đã tách kênh / bản xem trước / JPG    | **DN**           | Số nguyên kỹ thuật số                                                                         |
| Xuất độ phản xạ theo phần trăm 32-bit | **%**       | Giá trị số thực được lưu trữ không phải là DN, nên việc làm tròn nó thành số nguyên sẽ hiển thị kết quả vô nghĩa như `0` hoặc `1` |

Mỗi hàng được gắn nhãn bằng tên kênh tương ứng với bộ lọc của máy ảnh — `Red / Green / NIR` cho RGN, `Orange / Cyan / NIR` cho OCN, `NIR / Green / Blue` cho NGB, `Red / Green / Blue` cho RGB, và tên dải đơn cho các máy ảnh RE, NIR và máy ảnh đơn sắc M3M. Mỗi nhãn đều có một chấm màu tương ứng với các vòng tròn kênh được sử dụng trong trình chỉnh sửa công thức chỉ mục.

Các hình ảnh **chỉ số và LUT** đã lưu là một trường hợp đặc biệt: chúng chứa các thành phần bản đồ màu thay vì các dải quang phổ, do đó các hàng của chúng được gắn nhãn là `Red / Green / Blue` (hoặc `Index` đối với) thay vì tên bộ lọc của máy ảnh.

Khi một chỉ số đang hoạt động trong vùng thử nghiệm, một hàng bổ sung sẽ xuất hiện bên dưới các kênh, hiển thị **giá trị chỉ số** tại vị trí con trỏ, kèm theo tên chỉ số và một chấm trắng trùng khớp với dấu hiệu của nó trên biểu đồ histogram.

### Phần trăm phản xạ sử dụng thang đo riêng của từng tệp

{% hint style="warning" %}
**Không nên giả định rằng 65535 = 100%.** Chloros lưu trữ giá trị phản xạ theo các thang đo khác nhau tùy thuộc vào máy ảnh tạo ra nó, và trình xem sẽ xác định thang đo chính xác cho từng tệp.
{% endhint %}

| Nguồn                  | Giá trị DN tương ứng với độ phản xạ 1.0 | Cách nhận diện                                                                                                                               |
| ----------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **LATTICE**(M3C / M3M) |**32768**                      | Thẻ XMP `Chloros:PixelScale=32768` được ghi vào mọi tệp xuất độ phản xạ của LATTICE. Dải động 2× cho phép tệp chứa ρ lớn hơn 1.0 mà không bị cắt bớt |
| **Survey3**|**65535**                      | Không có thẻ tỷ lệ XMP Chloros — quá trình hiệu chuẩn Survey3 ghi giá trị ρ × dtype-max và cắt bớt tại 1.0                                                               |

Trình xem, hộp cát chỉ mục/LUT và xuất chỉ mục đều giải quyết tỷ lệ thông qua cùng một triển khai duy nhất, do đó giá trị bạn đọc tại con trỏ chính là giá trị mà phép tính chỉ mục đã sử dụng.

Hai hệ quả đáng lưu ý:

* Một tệp **32-bit phần trăm**TIFF lưu trữ DN/65535 dưới dạng số thực, và tệp**8-bit** PNG /JPG xuất ra lưu trữ DN × 255/65535 — trình xem sẽ chuyển đổi cả hai trở lại trước khi hiển thị giá trị phần trăm.
* Có một trường hợp không thể khôi phục: **tệp xuất 8-bit TIFF từ nguồn 8-bit** bị cắt giới hạn trong khoảng 0–255 thay vì được quy đổi tỷ lệ, và cố ý không chứa thẻ tỷ lệ. Đối với các tệp này, bảng điều khiển chỉ hiển thị DN, không có cột phần trăm. Đây là câu trả lời trung thực, không phải lỗi.***

## Biểu đồ phân bố màu của lớp

Dưới các hàng con trỏ là biểu đồ phân bố màu trực tiếp của lớp bạn đang xem, với **256 ô**. Theo mặc định, nó vẽ một đường cong kết hợp, được tính theo trọng số `(R + 2G + B) / 4` — cùng không gian đo lường mà các biểu đồ phân bố màu của máy ảnh LATTICE sử dụng. Khi bật tùy chọn**“RGB”**, biểu đồ này sẽ được thay thế bằng các đường cong riêng cho từng kênh với màu sắc của từng kênh, được trộn cộng để các vùng chồng chéo vẫn dễ đọc. Các lớp đơn sắc luôn hiển thị đường cong duy nhất.

Trục ngang được tính bằng đơn vị riêng của lớp:

| Lớp       | Đơn vị trục  | Giá trị tối đa của trục                                               |
| ----------- | ---------- | ---------------------------------------------------------- |
| Độ phản xạ | phần trăm    | 125% — dải động của sản phẩm cho phép ρ lớn hơn 1,0           |
| Độ rạng rỡ    | W/m²/sr/nm | Giá trị đỉnh riêng của khung hình, làm tròn lên đến hai chữ số có nghĩa |
| Dữ liệu 8-bit  | DN         | 255                                                        |
| Dữ liệu 12-bit | DN         | 4095                                                       |
| Dữ liệu 16-bit | DN         | 65535                                                      |

Khi trục hiển thị ở chế độ DN và chạm đến một trong ba ngưỡng trên, Chloros cũng xác định được độ sâu bit của dữ liệu bạn đang xem.

Có ba nút nằm phía trên biểu đồ histogram:

| Nút     | Mặc định | Hiệu ứng                                                                                                                                                                                                                                                                                   |
| ---------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CURSOR** | Bật      | Vẽ các đường đánh dấu trên biểu đồ histogram tại các giá trị chính xác được hiển thị ở các hàng phía trên, giúp bạn thấy vị trí của pixel dưới con trỏ trong phân phối của khung hình. Trong chế độ "RGB", mỗi kênh có một đường đánh dấu với màu riêng; ngược lại, chỉ có một đường đánh dấu màu trắng tại giá trị tổng hợp |
| **INDEX**| Bật      | Chỉ xuất hiện khi chỉ số đang hoạt động. Chuyển biểu đồ tần suất từ các dải nguồn sang**phân phối giá trị chỉ số**, với hai ngưỡng cắt được vẽ dưới dạng các đường đứt nét màu cam và giá trị chỉ số của con trỏ dưới dạng một đường màu trắng                                                          |
| **Chuyển đổi RGB**| Tắt     | Chuyển từ đường cong tổng hợp sang các đường cong riêng cho từng kênh. Trên cảm biến đơn sắc, nút này hiển thị**MONO** và bị vô hiệu hóa — chỉ có một kênh để hiển thị                                                                                                                                  |

Biểu đồ histogram được tính toán dựa trên các **khối mà bạn nhìn thấy**, chứ không phải các pixel nguồn phía sau chúng: khi thay đổi kích thước khối GSD, phân phối sẽ được tính toán lại, do đó biểu đồ histogram, dấu hiệu con trỏ và hình ảnh hiển thị luôn khớp nhau.***

## Kích thước khối GSD

Ở phía dưới cùng của bảng điều khiển là điều khiển **GSD (px)**: một ô nhập số, thanh trượt từ**1 đến 256**và nút**RESET**.

Nó làm thô hình ảnh _hiển thị_ bằng cách tính trung bình một khối N × N pixel nguồn thành một pixel hiển thị. `1` là độ phân giải gốc.

* Thông số này ảnh hưởng đến **chế độ xem toàn màn hình, các hình thu nhỏ trên lưới, thông tin hiển thị của con trỏ và cả hai biểu đồ tần số** — mọi thành phần hiển thị hình ảnh đều sử dụng cùng một độ phân giải cơ sở.
* Thông số này **chỉ áp dụng cho hiển thị**. Quá trình xử lý và xuất file không bị ảnh hưởng. Có một ngoại lệ duy nhất và được thiết kế có chủ đích: khi xuất qua [Index/LUT Sandbox](index-lut-sandbox.md), hệ thống sẽ lưu lại chính những gì bạn đang xem, do đó nó sẽ giữ nguyên kích thước khối hiện tại, và bảng điều khiển xuất file sẽ cảnh báo khi kích thước khối vượt quá 1.
* Giá trị này được lưu trữ **theo từng dự án** dưới dạng `viewer_display.gsd_bin` trong `project.json`, do đó nó vẫn được giữ nguyên khi đóng và mở lại.
* Chỉ báo con trỏ hiển thị giá trị của khối, không phải pixel nguồn, bất cứ khi nào kích thước khối lớn hơn 1 — giá trị hiển thị là giá trị trung bình của khối dưới con trỏ của bạn.

{% hint style="info" %}
**Tại sao lại là “kích thước khối” chứ không phải centimet trên mỗi pixel?** Con số cm/px cần có độ cao so với mặt đất. Dữ liệu EXIF của một khung hình chứa độ cao GPS so với mực nước biển trung bình, chứ không phải so với địa hình mà máy ảnh hướng tới, do đó Chloros sẽ không hiển thị khoảng cách mặt đất mà nó không thể tính toán chính xác. Kích thước khối tính bằng pixel nguồn cũng là phương án dự phòng mà các công cụ đám mây của MAPIR sử dụng khi khoảng cách mẫu mặt đất (GSD) không được biết.
{% endhint %}

***

## Các loại hình ảnh bạn có thể xem

Menu thả xuống lớp ở góc trên bên phải của trình xem liệt kê tất cả các phiên bản của hình ảnh hiện tại. Các mục hiển thị phụ thuộc vào máy ảnh và những gì đã được xử lý — xem [Lớp hình ảnh](image-layers.md) để biết danh sách đầy đủ và cách hoạt động của menu thả xuống.

### Survey3

* **JPG** — tệp xem trước gốc của máy ảnh
* **RAW (Original)** — tệp nguồn `.RAW`, đã được xử lý debayering để hiển thị, không có chỉnh sửa
* **RAW (Target)** — khung hình được xác định là chứa mục tiêu hiệu chuẩn
* **RAW (Độ phản xạ)** — sản phẩm độ phản xạ đã được hiệu chuẩn (65535 = ρ 1.0)
* **Đã hiệu chỉnh hiện tượng tối góc**/**Phản ứng cảm biến** — sản phẩm dự phòng chưa được hiệu chuẩn
* **Đã cân bằng trắng** — sản phẩm đã được cân bằng trắng
* **RAW (`<INDEX>` Index)**và**`<INDEX>` LUT** — hình ảnh chỉ số được tính toán

### LATTICE

Các bản chụp LATTICE sử dụng cùng một menu thả xuống, với tên các cấp độ của quy trình xử lý:

| Lớp                 | Nội dung chứa                                                        |
| --------------------- | -------------------------------------------------------------------- |
| **RAW (Gốc)**    | Khung hình RAW nguồn như khi được ghi lại                                     |
| **RAW (Đã loại bỏ hiệu ứng Bayer)**   | Hình ảnh tuyến tính đã loại bỏ hiệu ứng Bayer                                           |
| **RAW (Xem trước)**     | Hình xem trước trên màn hình — hiệu ứng kéo dãn màu giả cho máy ảnh đa phổ |
| **Cân bằng trắng**    | Bản xem trước trên màn hình dành cho máy ảnh chính củaRGB(cân bằng trắng + gamma)   |
| **RAW (Radiance)**    | Độ rạng rỡ phổ Float32 tính bằng W/m²/sr/nm                              |
| **RAW (Độ phản xạ)** | Độ phản xạ uint16, 32768 = ρ 1.0                                    |

Độ rạng rỡ và độ phản xạ chỉ áp dụng cho dữ liệu đa quang phổ: máy ảnh chính RGB không có đo quang phổ theo từng dải, do đó các lớp dữ liệu này không được tạo ra cho loại máy ảnh này.

***

## Áp dụng chỉ số và bảng tra cứu màu (LUT)

Áp dụng các chỉ số đa phổ và bảng tra cứu màu (LUT) từ thanh bên:

1. Mở thanh bên **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
2. Tích vào ô **Chỉ số**

3. Chọn bộ lọc của máy ảnh và công thức chỉ số, sau đó kéo các vòng tròn kênh vào các ô tương ứng trong công thức
4. Thêm bảng tra cứu màu (LUT) và chọn độ dốc, ngưỡng và chế độ cắt
5. Đọc giá trị tại vị trí con trỏ, sau đó lưu kết quả bằng **Export/Save Image(s)**Xem [Index/LUT Sandbox](index-lut-sandbox.md) để xem hướng dẫn chi tiết.***

## Khắc phục sự cố

### Hình ảnh không mở được

**Nguyên nhân có thể**: tệp đã bị di chuyển hoặc xóa sau khi nhập; sản phẩm chưa bao giờ được ghi; không đủ bộ nhớ cho hình ảnh rất lớn.**Cách khắc phục**:

1. Kiểm tra xem tệp của lớp đó vẫn còn tồn tại trong cây đầu ra của dự án hay không
2. Mở tệp trong trình xem bên ngoài để xác nhận rằng nó vẫn nguyên vẹn
3. Đóng các ứng dụng khác để giải phóng bộ nhớ

### Hình ảnh có màu đen, trắng hoặc màu sắc lộn xộn

**Nguyên nhân có thể**: vùng hiển thị không có dữ liệu để xử lý (khung hình gần như không thay đổi); lớp float32 có giá trị bất thường; chỉ số không tạo ra dữ liệu hợp lệ.**Cách khắc phục**:

1. Đọc các giá trị con trỏ — nếu mọi kênh đều bằng hoặc gần bằng 0, vấn đề nằm ở dữ liệu, không phải ở màn hình
2. Kiểm tra biểu đồ phân bố: một đỉnh nhọn duy nhất ở một đầu cho thấy khung hình bị cắt hoặc trống
3. Kiểm tra nhật ký xử lý cho lần chạy đã tạo ra lớp đó

### Các giá trị trông có vẻ sai

**Nguyên nhân có thể**: bạn đang ở trên một lớp khác với lớp bạn nghĩ; bạn đang so sánh phần trăm với giá trị DN thô; bạn đang so sánh tệp LATTICE với tệp Survey3 bằng cùng một hệ số chia.**Cách khắc phục**:

1. Xác nhận lớp đã chọn trong menu thả xuống — đơn vị hiển thị trên bảng điều khiển sẽ tuân theo lớp đó
2. Đối với độ phản xạ, hãy sử dụng cột **%** thay vì tự chia giá trị DN; nếu bắt buộc phải chia, hãy sử dụng giá trị `Chloros:PixelScale` của tệp đó (32768 cho LATTICE, nếu không có thì mặc định là 65535 cho Survey3)
3. Đặt kích thước khối GSD trở lại 1 — nếu lớn hơn 1, bạn đang đọc giá trị trung bình của khối, không phải của từng pixel
4. Kiểm tra xem quá trình hiệu chuẩn độ phản xạ đã thực sự được thực hiện cho khung hình đó chưa; sản phẩm dự phòng chưa được hiệu chuẩn (Sensor Response / Vignette Corrected) không phải là độ phản xạ

***

## Các bước tiếp theo

* [**Các lớp hình ảnh**](image-layers.md) — tên của từng lớp (nếu có) và ý nghĩa của các giá trị trong đó
* [**Hộp cát chỉ số/LUT**](index-lut-sandbox.md) — tạo, tinh chỉnh và xuất các hình ảnh trực quan hóa chỉ số
* [**Điểm đánh dấu trên bản đồ**](map-markers.md) — cùng bộ hình ảnh được hiển thị trên bản đồ
* [**Công thức chỉ số đa phổ**](../project-settings/multispectral-index-formulas.md) — tài liệu tham khảo về chỉ số

Để biết quy trình xử lý, xem [Xử lý hình ảnh (GUI)](../processing-images-gui/adding-files-to-a-project.md).
