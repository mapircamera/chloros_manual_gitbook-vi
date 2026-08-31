# Lưới hình ảnh

Sau khi nhập hình ảnh vào dự án, bạn sẽ thấy chúng được sắp xếp theo dạng lưới trong khu vực chính. Lưới này là nơi bạn chọn **phiên bản nào của từng hình ảnh mà bạn đang xem** — các nút phía trên lưới cho phép chuyển đổi đồng loạt tất cả các hình thu nhỏ giữa tệp nguồn và từng sản phẩm đã xử lý.

## Kích thước hình thu nhỏ

Sử dụng thanh trượt thu phóng ở góc trên bên phải để điều chỉnh kích thước của các hình thu nhỏ. Thanh trượt có phạm vi từ **64 px đến 1200 px**.

* **Ctrl + bánh xe chuột** cũng cho phép thay đổi tỷ lệ các hình thu nhỏ.
* **Ctrl + `+`**/**Ctrl + `=`**và**Ctrl + `−`** điều chỉnh kích thước tăng 4 px mỗi lần nhấn. Phạm vi điều chỉnh bằng bàn phím dừng lại ở 64 px ở mức nhỏ nhất và, ở mức lớn nhất, tại bất kỳ kích thước nào vừa vặn với chính xác hai hình thu nhỏ trên mỗi hàng trong cửa sổ hiện tại.
* Kích thước bạn chọn sẽ được lưu cùng với dự án (`UI → Grid thumbnail size` trong `project.json`, mặc định là `160`), do đó khi mở lại dự án, cài đặt này sẽ được khôi phục.

<figure><img src="../.gitbook/assets/chloros_grid_zoom.gif" alt=""><figcaption></figcaption></figure>*Độ phân giải* hình thu nhỏ là một cài đặt riêng biệt so với *kích thước* hình thu nhỏ: xem **Hiển thị → Độ phân giải hình thu nhỏ** trong [Cài đặt dự án](../project-settings/project-settings.md) (mặc định 512 px theo cạnh dài). Kích thước là độ lớn của ô được vẽ; độ phân giải là mức độ chi tiết được lấy để lấp đầy ô đó.***

## Thanh công cụ lưới

Hàng nút phía trên lưới có tối đa ba nhóm, từ trái sang phải:

1. **Theo Trigger / Theo Camera** — chế độ nhóm. Chỉ xuất hiện đối với các dự án chứa dữ liệu chụp từ LATTICE.
2. **Nút lọc camera** — mỗi camera LATTICE có một nút. Chỉ xuất hiện trong chế độ Theo Camera.
3. **Nút chế độ xuất/xem** — sản phẩm mà mỗi hình thu nhỏ hiển thị.

Khi cửa sổ quá hẹp để hiển thị tất cả các nút, các nhóm sẽ thu gọn từ phải sang trái thành các menu thả xuống khi di chuột qua: các nút xuất/xem sẽ thu gọn trước, sau đó đến các nút camera. Nhóm đã thu gọn sẽ để lại một nút duy nhất được gắn nhãn với lựa chọn hiện đang hoạt động, và khi di chuột qua nút này, toàn bộ bộ nút sẽ trượt xuống. **Chế độ Per Trigger / Per Camera không bao giờ thu gọn.**<!-- SCREENSHOT-NEEDED: Image grid toolbar of a LATTICE array project at full width, showing all three button groups inline: Per Trigger / Per Camera, three camera filter buttons labelled "LATT-M3M (serial)", and the export/view buttons including TIFF, RAW (Original), RAW (Radiance), RAW (Reflectance). -->***

## Nút Xem Xuất

Các nút này chuyển đổi các hình thu nhỏ trên lưới giữa các loại hình ảnh. **Một nút sẽ xuất hiện ngay khi sản phẩm mà nó đề cập tồn tại** — đối với các tệp nguồn, điều này có nghĩa là ngay lập tức khi nhập, chứ không phải sau khi xử lý.Chloros

quét lại các sản phẩm của dự án trong khi quá trình đang diễn ra, do đó các nút xuất hiện trong quá trình xử lý khi mỗi sản phẩm bắt đầu được lưu vào đĩa.

### Nút cơ bản

Nút xuất ở ngoài cùng bên trái được ghi nhãn theo **những gì bạn thực sự đã nhập**:

| Những gì bạn đã nhập | Nhãn nút |
| --- | --- |
|Survey3

RAW+JPG | `JPG` |
| Các ảnh chụp LATTICE kèm bản xem trước hiển thị bên cạnh khung hình RAW | `PNG` hoặc `TIFF`, tùy thuộc vào bản xem trước |
| Các ảnh chụp LATTICE mà tệp gốc **chính là** khung hình RAW | *không có nút* — `RAW (Original)` đã hiển thị tệp đó |

Trong một dự án hỗn hợp, nhãn sẽ tuân theo phần mở rộng được sử dụng nhiều nhất trong các hình ảnh.

### Các nút sản phẩm

| Nút | Hiển thị | Khi nào xuất hiện |
| --- | --- | --- |
| **Mục tiêu** | Hình ảnh có mục tiêu hiệu chuẩn được phát hiện | Sau một lần chạy phát hiện được mục tiêu |
| **Độ phản xạ** | Hình ảnh độ phản xạ đã hiệu chuẩn | Chỉ dành cho các dự ánSurvey3

— Các dự án LATTICE sử dụng `RAW (Reflectance)` thay thế, do đó lưới không bao giờ hiển thị hai nút độ phản xạ |
| **Cân bằng trắng** | Sản phẩm đã cân bằng trắng (máy ảnhRGB

) | Sau khi xử lý |
| **Đã hiệu chỉnh hiệu ứng viền tối** | Sản phẩm dự phòng đã hiệu chỉnh hiệu ứng viền tối nhưng chưa hiệu chuẩn | Sau một lần chạy mà không thể áp dụng hiệu chuẩn độ phản xạ và tùy chọn *Hiệu chỉnh hiệu ứng viền tối* được bật |
| **Phản ứng cảm biến** | Giá trị dự phòng phản ứng cảm biến chưa hiệu chỉnh | Tương tự, nhưng với *Chức năng khắc phục hiệu ứng viền tối* bị tắt |
| **`RAW (<INDEX> Index)`** | Một nút cho mỗi chỉ số đã tính toán | Sau một lần chạy với các chỉ số đã được cấu hình |
| **`<INDEX> LUT`** | Một nút cho mỗi chỉ số được ánh xạ màu | Sau một lần chạy với bảng tra cứu (LUT) đã được cấu hình |
| **`<Index> <Index\|LUT> <NNN>`** | Một nút cho mỗi lần xuất [Index/LUT Sandbox](index-lut-sandbox.md) | Ngay khi quá trình xuất sandbox hoàn tất |

### Các nút cấp độ LATTICE

Các dự án chứa các bản chụp LATTICE sẽ thêm các nút này, được gắn nhãn bằng tên cấp độ thay vì tên sản phẩm:

| Nút | Cấp độ |
| --- | --- |
| **RAW (Gốc)** | Khung hình thô nguồn, như đã nhập |
| **RAW (Radiance)** | Độ rạng rỡ quang phổ Float32, W/m²/sr/nm |
| **RAW (Reflectance)** | Độ phản xạ uint16, 32768 = ρ 1.0 |

`RAW (Original)` có sẵn ngay từ lúc nhập — không cần xử lý thêm. Khi một tệp nhập LATTICE không có nút cơ sở nào cả (tệp cơ sở của mỗi lần chụp chính là khung hình thô của nó), lưới sẽ tự động di chuyển đến nút cấp đầu tiên có sẵn để phần đánh dấu trên thanh công cụ khớp với những gì bạn đang thấy.

Hai cấp độ xuấtChloros

**không có nút lưới riêng**:

* **Đã loại bỏ hiệu ứng bay** — chế độ xem `RAW (Original)` đã hiển thị hình ảnh đã loại bỏ hiệu ứng bay, do đó việc thêm một nút thứ hai vào hình ảnh trông giống hệt nhau sẽ gây ra nhiễu. Sản phẩm `RAW (Debayered)` vẫn được ghi vào đĩa và vẫn có thể chọn từ menu thả xuống lớp toàn màn hình.
* **Xem trước** — trên các máy ảnhRGB

, hình xem trước được đăng ký dưới dạng lớp `White Balanced`, lớp này có nút riêng. Trên các máy ảnh đa quang phổ, nó được đăng ký dưới tên `RAW (Preview)` và có thể truy cập từ menu thả xuống lớp toàn màn hình.

{% hint style="info" %}
Các nút cấp độ này chỉ hiển thị đối với các dự án thực sự chứa các khung hình LATTICE. Các dự ánSurvey3

cũng sử dụng một số tên lớp nội bộ tương tự, nhưng các nút này sẽ bị lọc bỏ trong những trường hợp đó; do đó, lướiSurvey3

vẫn giữ nguyên bộ lớp quen thuộc `JPG / Targets / Reflectance`.
{% endhint %}

Nhấp vào hình thu nhỏ của lưới sẽ mở [Trình xem hình ảnh](opening-an-image-full-screen.md) toàn màn hình trên **cùng sản phẩm mà lưới đang hiển thị** — nếu lưới được đặt thành `Targets`, hình thu nhỏ sẽ mở hình ảnh đích đã xuất.

<figure><img src="../.gitbook/assets/chloros_grid_mode.gif" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: This GIF predates the LATTICE level buttons and the toolbar group separators. Reshoot on a LATTICE project cycling base -> RAW (Original) -> RAW (Radiance) -> RAW (Reflectance) -> an index button, so the new button set and the level names are visible. -->


***

## Nhóm các dự án LATTICE: Theo Trigger so với Theo Camera

Chế độ chụp mảng (Array captures) tạo ra nhiều hình ảnh cùng một khoảnh khắc từ các mô-đun camera khác nhau. Chế độ nhóm quyết định cách lưới sắp xếp các hình ảnh này. Cả hai chế độ đều hiển thị các thanh tiêu đề có thể thu gọn với chiều rộng đầy đủ; **mỗi nhóm đều mở rộng sẵn khi bắt đầu**, vàChloros
sẽ ghi nhớ các nhóm bạn đã thu gọn. Trạng thái thu gọn được theo dõi riêng biệt cho từng chế độ, do đó việc thu gọn một nhóm trong chế độ &quot;Per Camera&quot; sẽ không ảnh hưởng đến chế độ &quot;Per Trigger&quot;.

### Theo Camera (mặc định)

Một nhóm cho mỗi mô-đun camera. Thanh tiêu đề hiển thị mẫu camera và số sê-ri (`LATT-M3M — <serial>`) cùng số lượng ảnh. Các ô ảnh bên trong một nhóm được sắp xếp theo thứ tự thời gian dựa trên sự kiện chụp.

Trong chế độ này, thanh công cụ cũng có một **nút lọc camera cho mỗi camera**, được ghi là `MODEL (SERIAL)`. Tất cả camera đều được chọn sẵn; nhấp vào nút sẽ bỏ chọn camera đó và loại bỏ nhóm của nó khỏi lưới. Đây là cách nhanh chóng để xem lại một dải tần trên toàn bộ chuyến bay.

### Theo lần kích hoạt

Mỗi sự kiện chụp ảnh tương ứng với một nhóm — tập hợp các khung hình mà tất cả các mô-đun đã chụp tại cùng một lần kích hoạt. Phần tiêu đề hiển thị thời gian chụp, số lượng camera tham gia và biểu tượng cho từng mẫu camera trong nhóm. Các ô trong một nhóm được sắp xếp theo số sê-ri camera, do đó cùng một dải phổ sẽ nằm trong cùng một cột cho mỗi lần kích hoạt.

Các hình ảnh không thuộc hệ thống<!-- SCREENSHOT-NEEDED: Image grid in Per Trigger mode for a 3-camera LATTICE array, showing two consecutive trigger groups with their header bars (chevron, capture timestamp, "3 cameras", and the three model badges) and one group collapsed to show the closed state. -->
LATTICE trong một dự án hỗn hợp sẽ không được nhóm lại — chúng sẽ hiển thị dưới dạng các ô đơn thuần sau các nhóm.

***

## Hình thu nhỏ trên lưới tuân theo kích thước khối GSD

Nếu bạn đã thiết lập kích thước khối **GSD (px)** trong thanh bên của tab hình ảnh, các hình thu nhỏ dạng lưới sẽ được hiển thị ở độ phân giải mặt đất đó — không chỉ ở chế độ xem toàn màn hình. Kích thước khối là 8 có nghĩa là mỗi pixel hiển thị là giá trị trung bình của một khối 8 × 8 pixel nguồn, ở mọi vị trí trong ứng dụng hiển thị hình ảnh.

Vì một ô ban đầu chỉ rộng vài trăm pixel, nên kích thước khối thô sẽ không còn tạo ra sự khác biệt rõ rệt trên lưới từ rất sớm so với khi hiển thị toàn màn hình: một khung hình 4000 px được vẽ vào ô 160 px đã có tỷ lệ khoảng 25 pixel nguồn trên mỗi pixel hiển thị. Xem [Mở hình ảnh toàn màn hình](opening-an-image-full-screen.md#gsd-block-size) để biết chi tiết về điều khiển này.

***

## Các trang liên quan

* [**Mở hình ảnh toàn màn hình**](opening-an-image-full-screen.md) — trình xem toàn màn hình, giá trị con trỏ và biểu đồ histogram
* [**Lớp hình ảnh**](image-layers.md) — menu thả xuống lớp bên trong trình xem toàn màn hình
* [**Hộp cát chỉ mục/LUT**](index-lut-sandbox.md) — tạo và xuất các hình ảnh hóa chỉ mục
* [**Cài đặt Dự án**](../project-settings/project-settings.md) — các công tắc xuất quyết định các sản phẩm nào sẽ tồn tại
