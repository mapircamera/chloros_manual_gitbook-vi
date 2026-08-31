# Tab DAQ trong Chloros

Tab DAQ — được ghi là **Cảm biến ánh sáng** trong thanh bên của Chloros — là giao diện điều khiển thời gian thực dành cho [các cảm biến ánh sáng DAQ-U, DAQ-M và DAQ-E](README.md): kết nối cảm biến qua bất kỳ giao thức truyền tải nào, theo dõi phổ đã hiệu chuẩn theo thời gian thực, tính toán độ phản xạ trực tiếp từ cặp cảm biến và ghi tệp `.daq` trực tiếp vào dự án của bạn.

Tab này sẽ khả dụng ngay sau khi phần nền của Chloros hoàn tất quá trình khởi động. Các biểu đồ trên tab được cung cấp dữ liệu từ dịch vụ DAQ của Chloros qua kết nối trực tiếp, tự động kết nối lại (thời gian chờ 2–10 giây) nếu bị gián đoạn; khi dịch vụ không thể truy cập, hàng Trạng thái của cảm biến sẽ hiển thị **No Server**.

Bố cục bao gồm **thanh bên cảm biến**(một hàng cho mỗi cảm biến được kết nối) cùng với**khu vực biểu đồ** (một ô biểu đồ cho mỗi cảm biến hoặc nhóm).

<!-- SCREENSHOT-NEEDED: full DAQ (Light Sensors) tab in list view with one DAQ-E connected — sensor sidebar on the left (Connect Sensor + Record All buttons, one sensor row), spectrum chart with rainbow fill in the main area, live data table below the chart -->

***

## Kết nối cảm biến

Nhấp vào **Kết nối cảm biến** ở đầu thanh bên. Hộp thoại kết nối sẽ mở ra trong khu vực chính (hoặc dưới dạng lớp phủ khi thêm cảm biến khác — trong trường hợp đó, nút Hủy sẽ xuất hiện).

| Điều khiển | Hành vi |
| --- | --- |
| **Loại thiết bị** | `DAQ-U (USB)` (mặc định), `DAQ-M (Bluetooth)` hoặc `DAQ-E (Ethernet)`. Việc chuyển đổi sẽ khởi động lại quá trình quét cho giao thức truyền tải mới được chọn. |
| **Cổng / Thiết bị BLE / Tên máy chủ / IP** | Liệt kê các thiết bị đã phát hiện dưới dạng `device - description`; mục đầu tiên được nhận diện là cảm biến sẽ được tự động chọn. Trong quá trình quét, màn hình hiển thị `Scanning...` (USB), `Scanning (N)...` kèm theo đếm ngược 8 giây (BLE) hoặc `Discovering ethernet sensors (N)...` kèm theo đếm ngược 5 giây (Ethernet). Kết quả trống hiển thị `No ports` / `No BLE devices` / `No ethernet sensors found`. |
| **↻ Làm mới** | Quét lại giao thức đã chọn ngay lập tức (tính năng này bị vô hiệu hóa trong khi đang quét BLE/Ethernet). |
| **Kết nối** | Bật khi thiết bị đã được chọn; đổi tên thành `Connecting...` trong khi đang thiết lập kết nối. |

Quá trình phát hiện chỉ chạy **khi hộp thoại kết nối đang hiển thị trên màn hình** và lặp lại cứ sau 15 giây chỉ cho giao thức được chọn — chỉ mở tab không thực hiện quét. Nếu thất bại, hộp thoại sẽ hiển thị: *&quot;Kết nối thất bại. Hãy thử rút và cắm lại cảm biến, sau đó nhấp vào Kết nối một lần nữa.&quot;*

Thanh bên sẽ tự động bật lên khi cảm biến đầu tiên của bạn kết nối.

{% hint style="info" %}
**DAQ-E không hiển thị?** DAQ-E không có đèn LED trạng thái — hãy kiểm tra đèn báo PoE/kết nối trên cổng chuyển mạch hoặc cổng bộ tiêm mà nó được cắm vào, và đợi vài giây sau khi bật nguồn để thiết bị khởi động. Máy chủ Chloros phải nằm trong cùng một miền phát sóng (mDNS không đi qua bộ định tuyến). Trên Windows, hãy chấp nhận lời nhắc của tường lửa Defender lần đầu tiên khi Chloros thiết lập kết nối cho các socket đa phát (mDNS UDP 5353, dữ liệu DAQ-E UDP 5002, PTP UDP 319/320). Hai thiết bị DAQ-E trên cùng một mạng LAN được phát hiện riêng biệt, mỗi thiết bị có tên máy chủ riêng là `daq-e-<id>.local`.
{% endhint %}

<figure><img src="../.gitbook/assets/v120-daq-device-type.png" alt=""><figcaption>Loại thiết bị bao gồm DAQ-U (USB), DAQ-M (Bluetooth) và DAQ-E (Ethernet)</figcaption></figure>

***

## Thanh bên cảm biến

Mỗi cảm biến được kết nối sẽ có một hàng (cộng thêm một hàng cho mỗi nhóm Môi trường + Đối tượng). Các hàng có thể được sắp xếp lại bằng cách kéo thả, và thứ tự của chúng cũng sẽ thay đổi thứ tự các ô biểu đồ. Nhấp vào một hàng để chuyển cảm biến/nhóm đó thành biểu đồ đang hoạt động trong chế độ xem danh sách.

| Thành phần | Ý nghĩa |
| --- | --- |
| Đường viền trái có màu | Màu biểu đồ của cảm biến. |
| Biểu tượng kết nối | `DAQ-U` / `DAQ-M` / `DAQ-E`, hoặc biểu tượng màu xanh lá cây `REF` dành cho nhóm phản xạ Ambient+Object. |
| Tên thiết bị | Mặc định là số sê-ri của cảm biến (danh tính ổn định để hiệu chuẩn, tên tệp `.daq` và khớp khi nhập); tên tùy chỉnh được lưu giữ cho từng dự án. |
| Biểu tượng **Đã hiệu chuẩn** (màu xanh lá) | Hiển thị khi gói hiệu chuẩn nhà máy của cảm biến được tải, tức là các phổ là giá trị thực W/m²/nm. |
| Biểu tượng **Có bản cập nhật** (màu hổ phách, chỉ dành cho DAQ-E) | Phiên bản firmware đang chạy cũ hơn so với hình ảnh đi kèm trong bản dựngChloros

này. Trong quá trình cập nhật, nó hiển thị tiến trình trực tiếp (`Flashing… N%`, `Restarting sensor…`, sau đó là `Updated X → Y` hoặc `Failed`). |
| Mắt | Bật/tắt hiển thị cảm biến này trên biểu đồ. |
| Bánh răng | Mở cửa sổ cài đặt riêng cho từng cảm biến (bên dưới). |
| ✕ (màu đỏ) | Ngắt kết nối cảm biến, hoặc xóa nhóm Môi trường xung quanh + Đối tượng. |

Phía trên các hàng có hai nút:

* **Kết nối cảm biến** — mở hộp thoại kết nối (sẽ đổi tên thành `Connecting...` khi đang xử lý).
* **Ghi lại tất cả / Dừng tất cả**— bắt đầu hoặc dừng quá trình ghi lại `.daq` trên**tất cả**cảm biến đã kết nối. Yêu cầu ít nhất một cảm biến**và một dự án đang mở** (chú thích công cụ: &quot;Mở một dự án để ghi lại&quot;); nút sẽ chuyển sang màu đỏ khi bất kỳ quá trình ghi lại nào đang chạy.

Trạng thái trống hiển thị &quot;Không có cảm biến nào được kết nối&quot;.

<!-- SCREENSHOT-NEEDED: sensor sidebar with three rows — a DAQ-E showing both the green Calibrated pill and the amber Update Available pill, a DAQ-U row, and a green REF group row — plus the Connect Sensor and Record All buttons -->

***

## Cài đặt cho từng cảm biến (cửa sổ hình bánh răng)

Mở bằng biểu tượng bánh răng trên hàng cảm biến. Nội dung theo thứ tự:

* **Hàng thông tin** — Loại thiết bị (DAQ-U/M/E), Kết nối (`Serial (USB)` / `Bluetooth` / `Ethernet`), Cổng (cổng COM, địa chỉ BLE hoặc máy chủ) và Số sê-ri.
* **Báo cáo hiệu chuẩn: Tải xuống** — tải xuống chứng chỉ hiệu chuẩn có thể truy xuất nguồn gốc NIST (PDF) của thiết bị này và mở nó trong trình xem PDF của bạn. Tính năng này có sẵn khi đã biết số sê-ri; chứng chỉ được lưu trong bộ nhớ cache khi kết nối lần đầu tiên.
* **Tên thiết bị** — nhấp vào biểu tượng bút chì để đổi tên; được lưu cho từng dự án.
* **Màu đường đồ thị** — bảng màu; được lưu cho từng dự án.
* **Thời gian tích hợp (ms)**— thanh trượt + số,**1–500 ms**, mặc định**32 ms**. Tính năng này bị vô hiệu hóa khi AE bật.
* **Trung bình khung hình**— thanh trượt + số,**1–50 khung hình**, mặc định**20**.
* **AE: BẬT/TẮT**— công tắc tự động phơi sáng;**mặc định BẬT** khi kết nối. Tắt tính năng này để thiết lập thời gian tích hợp thủ công.
* **Dừng phát trực tiếp / Bắt đầu phát trực tiếp** — tạm dừng hoặc tiếp tục phát trực tiếp.
* **Ghi / Dừng ghi** — ghi hình theo từng cảm biến `.daq` (cần có dự án đang mở).
* **Cap** — hồ sơ hiệu chỉnh cap (phần tiếp theo).
* **Các hàng thông tin trực tiếp** — Thời gian tích hợp (ms), FPS, Số mẫu, Đang ghi (màu đỏ `REC` hoặc `Off`) và Trạng thái (`Streaming` / `Paused` / `SATURATED` / `No Server`).

### Chỉ dành cho DAQ-E: các hàng mạng, phần mềm nhúng và PTP

* **Tên máy chủ / IP** — địa chỉ hiện tại của thiết bị.
* **Phần mềm nhúng** — phiên bản phần mềm nhúng hiện tại, kèm theo ô thao tác:<version\>

nút</version\>

**Cập nhật lên \<version\>** sẽ xuất hiện khi bản dựngChloros

này chứa hình ảnh phần mềm nhúng DAQ-E mới hơn. Quá trình cập nhật được thực hiện qua mạng trong khoảng 30 giây; cảm biến sẽ tự động khởi động lại và kết nối lại, và nếu quá trình truyền dữ liệu bị gián đoạn, phiên bản phần mềm nhúng hiện tại vẫn được giữ nguyên. Tiến trình được cập nhật trực tiếp (`Flashing… N%` → `Restarting sensor…` → `Updated X → Y`), và ô này hiển thị `Up to date` khi là phiên bản hiện tại.
* **Đồng bộ PTP** — trạng thái PTP thời gian thực (quay trở lại `unknown`). Phần mềm DAQ-E phiên bản v1.2.0 trở lên tham gia tiêu chuẩn IEEE 1588 PTPv2 với vai trò đồng hồ nô lệ; hệ thống nền (backend) của máy chủChloros

đóng vai trò là đồng hồ chủ (grandmaster) PTP, và mọi thiết bị DAQ-E cùng camera LATTICE trên mạng LAN đều hoạt động như thiết bị phụ thuộc vào nó trong miền 0, duy trì độ lệch thời gian trong khoảng 1 ms.

Đối với nhóm Ambient+Object, cửa sổ chế độ thiết bị chỉ hiển thị các cảm biến nguồn của nhóm, Tên Thiết Bị và Màu Đường Đồ Thị.



<!-- SCREENSHOT-NEEDED: per-sensor settings modal for a DAQ-E — info rows, Calibration Report Download, Hostname/IP + Firmware row with an "Update to <ver>" button, PTP Sync row, Integration Time / Frame Average sliders, AE ON toggle, and the Cap dropdown all visible (scrolled composite acceptable) -->

### Lựa chọn nắp

Menu thả xuống **Cap** (Nắp) cho biếtChloros

nắp vật lý nào được lắp lên bộ khuếch tán của cảm biến, và áp dụng hồ sơ hiệu chỉnh được đo tại nhà máy của nắp đó cho mọi phổ. Các lựa chọn tùy thuộc vào mẫu:

| Mẫu | Lựa chọn nắp |
| --- | --- |
| DAQ-U | Không có (cảm biến trần), FOV 15°, FOV 30°, FOV 45°, FOV 60°, FOV 90°, Sunshine (bộ hiệu chỉnh cosin) |
| DAQ-M | Không có (cảm biến trần), Sunshine (bộ hiệu chỉnh cosin) |
| DAQ-E | Không có (cảm biến trần), FOV 15°, FOV 45°, FOV 90°, Sunshine (bộ hiệu chỉnh cosin) |

**Cài đặt mặc định cho mọi mẫu là Sunshine (bộ hiệu chỉnh cosin)** —MAPIR

cung cấp mọi thiết bị DAQ với nắp Sunshine đã được lắp sẵn, và đây là cấu hình tiêu chuẩn cho môi trường ngoài trời: góc nhìn bán cầu 180° với sai số cosin ≤ ±4 % đến 60° và ≤ ±4,5 % đến 70° (không khuyến nghị khi góc nâng Mặt Trời dưới ~15°), có độ suy giảm theo thiết kế (~12×). Lựa chọn của bạn sẽ được lưu trong dự án.

{% hint style="warning" %}
**Lựa chọn nắp phải khớp với nắp thực tế.**Cả cảm biến lẫn phần mềm đều không thể phát hiện nắp nào đang được lắp. Lựa chọn này ảnh hưởng đến cả hiệu chỉnh thời gian thực và dấu ghi vào mỗi tệp `.daq` — với độ suy giảm ~12× của nắp Sunshine, việc thay đổi nắp mà không khai báo sẽ dẫn đến hiệu chỉnh sai quang phổ với hệ số sai lệch xấp xỉ như vậy. (Việc tháo ra và lắp lại cùng một nắp sẽ lặp lại sai số khoảng 1,5 %.) Chỉ chọn**None (cảm biến trần)** khi nắp đã được tháo ra thực tế; trên DAQ-E, &quot;None&quot; vẫn áp dụng cấu hình hình học mặc định cho bộ khuếch tán kính lõm của nó — đây không phải là chế độ không hoạt động — và một DAQ-E trần là cấu hình trên bàn thí nghiệm, không phải cấu hình được hỗ trợ trong thực tế.
{% endhint %}

{% hint style="info" %}
Cập nhật từ phiên bản hướng dẫn trước đây: công tắc &quot;Sunshine Diffuser Installed&quot; (Bật/Tắt) ở phía trình duyệt từ phiên bản 1.1.0 đã bị loại bỏ. Việc xử lý nắp giờ đây được thực hiện thông qua hồ sơ nắp riêng cho từng cảm biến, được áp dụng ở phía máy chủ.
{% endhint %}

***

## Khu vực biểu đồ

Một thanh trên cùng cố định chứa nút chuyển đổi **danh sách ⇄ chế độ xem dạng lưới**và thanh trượt**Phóng to biểu đồ** (kích thước ô 200–2000 px). Chế độ xem sẽ tự động chuyển sang dạng lưới khi có nhiều hơn một nhóm biểu đồ, và quay lại dạng danh sách khi chỉ có một hoặc ít hơn một nhóm. Chế độ xem và kích thước biểu đồ được lưu theo từng dự án.**Biểu đồ phổ** của mỗi cảm biến hiển thị:

* **Trục X** — Bước sóng (nm). Lưới cảm biến là 340–1010 nm với bước nhảy 5 nm (135 điểm), được nội suy thành 1 nm để hiển thị.
* **Trục Y** — Công suất (W/m²), kèm tiền tố SI tự động (m/µ/n) được chọn dựa trên giá trị đỉnh. Các phổ được hiệu chuẩn bức xạ học dưới dạng cường độ bức xạ phổ (W/m²/nm) trên cả ba phương tiện truyền tải.
* Màu nền quang phổ cầu vồng dưới một đường cong; nhiều cảm biến trên cùng một biểu đồ sẽ chồng lên nhau dưới dạng các đường màu với nền mờ.
* **Di chuột**— con trỏ dọc hiển thị bước sóng và giá trị của từng cảm biến;**kéo** để phóng to (nút thu nhỏ sẽ xuất hiện khi đang phóng to).
* Nút **+** (chỉ trong chế độ xem lưới) để thêm cảm biến vào biểu đồ này hoặc tạo nhóm (xem bên dưới).
* Tên thiết bị được căn giữa ở phía trên, và biểu tượng quay tròn cho đến khi khung hình đầu tiên xuất hiện.

**Độ bão hòa** không được đánh dấu trực tiếp trên biểu đồ: cảm biến bị bão hòa sẽ hiển thị văn bản trạng thái màu đỏ `SATURATED` và một hàng màu đỏ `Saturated: Yes` trong bảng dữ liệu trực tiếp. Hãy giảm thời gian tích hợp hoặc bật lại chế độ AE để xóa trạng thái này.



<!-- SCREENSHOT-NEEDED: grid view with at least two chart tiles visible, the Chart Zoom slider and list/grid toggle in the top bar, and the "+" add-sensor button visible on one tile -->

***

## Bảng dữ liệu trực tiếp (chế độ xem danh sách)

Phía dưới biểu đồ ở chế độ xem danh sách, được làm mới mỗi 500 ms:

* **Tất cả các mẫu**: Mẫu màu ánh sáng (sRGB từ CIE XYZ), Bão hòa (Có/Không), CIE 1931 X/Y/Z, Độ sắc màu x/y, CIE u′/v′, CCT (K), CRI (Ra), Bước sóng chủ đạo (nm), Bước sóng đỉnh (nm), Độ tinh khiết kích thích, Duv, CIE L\*/a\*/b\*, và Munsell H/V/C.
* **Chỉ dành cho cảm biến đã hiệu chuẩn**(bất kỳ mẫu DAQ-U / DAQ-M / DAQ-E nào sau khi gói hiệu chuẩn nhà máy được cài đặt — biểu tượng**Đã hiệu chuẩn** màu xanh lá trên hàng cảm biến là dấu hiệu nhận biết): Công suất tổng (W/m²), Lux quang (lx), Lux quang cảm (lx), Tỷ lệ S/P, PPFD cùng với PPFDRed

/Green

/Blue

(µmol/m²/s), và các cường độ bức xạ opic — S-cone, Melanopic, Rhodopic, M-cone, L-cone (tất cả đều tính bằng W/m²).

<!-- SCREENSHOT-NEEDED: list view live data table for a DAQ-E showing both the colorimetric rows and the power-calibrated rows (Total Power, Photopic/Scotopic Lux, PPFD, opic irradiances) -->

***

## Nhóm độ phản xạ (Ánh sáng xung quanh + Đối tượng)

Hai cảm biến được kết nối có thể được kết hợp thành một màn hình hiển thị độ phản xạ trực tiếp — không cần sử dụng camera:

1. Trong chế độ xem lưới, nhấp vào **+**trên ô biểu đồ và chọn**Kết hợp Ánh sáng xung quanh + Đối tượng**.
2. Chọn một cảm biến **Nguồn sáng môi trường**và một cảm biến**Quét đối tượng**(hai cảm biến riêng biệt), sau đó nhấp**Tạo**. Chế độ**Chloros** tính toán R(λ) = object(λ) / ambient(λ) cho từng bước sóng từ hai luồng dữ liệu thời gian thực (giá trị bằng 0 khi ánh sáng môi trường ≤ 0). Nhãn của nhóm tuân theo lớp hiệu chuẩn của các cảm biến:

* Cả hai cảm biến đều đã được hiệu chuẩn (gói đã được tải) → **&quot;Độ phản xạ biểu kiến&quot;**.
* Một trong hai cảm biến chưa được hiệu chuẩn → **&quot;Độ phản xạ tương đối&quot;**.

Nhóm này xuất hiện dưới dạng một hàng màu xanh lá cây `REF` trong thanh bên và biểu đồ riêng của nó (lấp đầy màu cầu vồng, hiển thị giá trị khi di chuột đến 4 chữ số thập phân, thu phóng bằng cách kéo).

Menu **+**cũng cung cấp tùy chọn**Thêm cảm biến mới** với ba vị trí: *Kết hợp cảm biến mới* (thêm vào biểu đồ này), *Di chuyển cảm biến hiện có đến đây* hoặc *Xem cảm biến mới* (biểu đồ riêng).



<!-- SCREENSHOT-NEEDED: the "+" add-sensor overlay open on a chart tile showing the menu (Add New Sensor / Combine Ambient + Object / Cancel), and the Ambient + Object sub-dialog with its two sensor selects -->

### Bảng chỉ số thực vật

Trong chế độ xem danh sách, bảng chỉ số thực vật nằm bên dưới biểu đồ của nhóm độ phản xạ, được tính toán từ độ phản xạ thời gian thực tại các tâm dải **xanh dương 450 / xanh lá 550 / đỏ 670 /NIR

800 nm** (giá trị hiển thị đến 4 chữ số thập phân, `---` khi không thể tính toán; di chuột qua tên chỉ số để xem tên đầy đủ):

* **Luôn hiển thị** (không phụ thuộc vào tỷ lệ, với bất kỳ kết hợp cảm biến nào):NDVI

,GNDVI

, ENDVI,WDRVI

,GRVI

,CVI

,GCI

, MSR.
* **Chỉ khi cả hai cảm biến đều được hiệu chuẩn nguồn điện** (cả hai gói dữ liệu đều được tải):EVI

,SAVI

,OSAVI

, GSAVI, GOSAVI, MSAVI2, RDVI, TDVI,LAI

, NLI, MNLI, FCI,GEMI

.

<!-- SCREENSHOT-NEEDED: an Ambient+Object reflectance group in list view — reflectance chart labeled "Apparent Reflectance" with the vegetation index table below it showing live NDVI etc. -->

***

## Ghi tệp `.daq`

* Việc ghi yêu cầu phải có **dự án đang mở** — nếu không, cả tùy chọn Ghi Tất cả (trên thanh bên) và nút Ghi cho từng cảm biến đều bị vô hiệu hóa.
* Các tệp được ghi vào **`<project folder>/light_sensor/`**; tên tệp chứa ID cảm biến và dấu thời gian, và tên thiết bị được lưu trữ cùng với bản ghi.
* Khi quá trình ghi dừng lại (Dừng, Dừng tất cả hoặc ngắt kết nối giữa chừng), tệp `.daq` đã hoàn thành sẽ **được thêm vào dự án đang mở tự động** — nó xuất hiện trong danh sách tệp của dự án mà không cần thêm thủ công, sẵn sàng để sử dụng làm dữ liệu bức xạ xuống cho [xử lý độ phản xạ](README.md).
* Một chỉ báo màu đỏ `REC` sẽ hiển thị trong các hàng trực tiếp của cửa sổ cài đặt trong quá trình ghi.

Đối với các giá trị bức xạ định lượng, hãy tính trung bình ít nhất 15 giây dữ liệu — đây là đặc tính của thiết bị, không phải lỗi.

<!-- SCREENSHOT-NEEDED: recording in progress — sidebar Stop All button in its red state and the settings modal live rows showing Recording: REC -->



***

## Bố cục đa cảm biến và tính liên tục của dự án

* Kết hợp nhiều cảm biến trên một biểu đồ (trục chung), giữ các biểu đồ riêng biệt (bố cục lưới tự động), di chuyển cảm biến giữa các biểu đồ, kéo để sắp xếp lại các hàng/ô, và ẩn từng cảm biến bằng nút chuyển đổi hình mắt.
* Đối với mỗi dự án, các cài đặt &quot;Chloros

&quot; được lưu lại: tên thiết bị, màu biểu đồ, kích thước biểu đồ, chế độ xem và cài đặt của từng cảm biến (thời gian tích hợp, trung bình khung hình, trạng thái AE, lựa chọn nắp).
* **Khi mở lại dự án, các cảm biến sẽ tự động kết nối lại** theo địa chỉ — cổng COM cho DAQ-U, thiết bị BLE cho DAQ-M, tên máy chủ mDNS cho DAQ-E (vẫn được giải quyết ngay cả khi địa chỉ IP của thiết bị thay đổi) — đồng thời áp dụng lại cấu hình cap, trung bình khung hình, trạng thái AE và thời gian tích hợp thủ công đã lưu của từng cảm biến.***

## Ghép nối camera (DLS)

Không cần ghép nối. Khác với quy trình làm việc DLS trên máy bay không người lái (drone) — vốn liên kết cảm biến ánh sáng với camera ngay từ đầu — phần mềm “Chloros

” sẽ khớp dữ liệu DAQ với hình ảnh ở giai đoạn sau: tại thời điểm nhập/xử lý, các giá trị đo từ `.daq` sẽ được nội suy theo dấu thời gian phơi sáng của từng khung hình. Ghi dữ liệu với bất kỳ cảm biến nào được kết nối (cảm biến `.daq` sẽ tự động xuất hiện trong dự án), và quá trình xử lý phản xạ sẽ xác định các giá trị đo phù hợp theo thời gian — xem [Cảm biến ánh sáng DAQ](README.md) để biết cách dữ liệu ánh sáng chiếu xuống được sử dụng.</version\>
