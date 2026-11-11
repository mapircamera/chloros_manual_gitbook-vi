# Kết nối máy ảnh

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption><p>Tab &quot;Camera&quot; trước khi kết nối bất kỳ thiết bị nào</p></figcaption></figure>

Chloros
tự động phát hiện các camera LATTICE trên kết nối — từ tab &quot;Camera&quot; trên giao diện người dùng (GUI), từ tệp `chloros-cli lattice`, hoặc từ Python
SDK
. Chuỗi mô hình của camera quyết định mọi thứ ở các bước tiếp theo: Chloros
xác định cấu hình cảm biến, bố cục dải tần và hiệu chuẩn nhà máy từ các tệp `DeviceUserID` + `DeviceSerialNumber` của camera, do đó **không cần cấu hình gì cho từng camera**.

Trước khi kết nối, hãy đảm bảo mạng máy chủ đã được thiết lập — địa chỉ liên kết cục bộ, khung jumbo và, đối với mảng camera, các cài đặt bộ đệm nhận của NIC. Đó là thiết lập phía phần cứng và được nêu trong hướng dẫn sử dụng LATTICE: [**Thiết lập mạng**](https://mapir.gitbook.io/lattice-camera/setup/network-setup).

## Kết nối từ GUI

Mở tab **Camera** trong thanh bên của Chloros
(các tab phần cứng sẽ xuất hiện sau khi quá trình khởi động backend hoàn tất) hoặc sử dụng menu chính → **Kết nối với camera**. Cả hai cách này đều mở hộp thoại**Kết nối camera**.

### Hộp thoại **Kết nối camera(s)**Hộp thoại sẽ quét mạng ngay khi mở ra (&quot;Đang quét mạng...&quot;), và liệt kê tất cả các camera mà nó tìm thấy. Mỗi hàng hiển thị**mẫu máy**của camera (ví dụ: `LATT-M3M-L41-F550`),**số sê-ri**và**địa chỉ IP**.

* **Nhấp vào một hàng để chọn nó**(được tô sáng màu xanh lá cây). Bạn có thể chọn**nhiều camera** và kết nối chúng cùng lúc — Chloros
sẽ kết nối chúng theo thứ tự.
* Các hàng có biểu tượng **&quot;Đã kết nối&quot;** đã được kết nối và không thể chọn lại.
* Các hàng có biểu tượng **&quot;Trong mảng&quot;** thuộc về một mảng camera đang được kết nối. Hãy ngắt kết nối mảng camera trước để sử dụng camera đó ở chế độ độc lập.
* **Kết nối** — kết nối (các) camera đã chọn; nút này hiển thị số lượng, ví dụ: &quot;Kết nối (3)&quot;, khi chọn nhiều hơn một camera.
* **Quét lại** — thực hiện quá trình phát hiện lại.
* **Đóng** — đóng hộp thoại.
* Nếu quá trình quét kết thúc mà không có kết quả, hộp thoại sẽ hiển thị **&quot;Không tìm thấy camera nào trên mạng&quot;** — xem [Khắc phục sự cố](connecting.md#troubleshooting) bên dưới.

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p>Hộp thoại Kết nối camera — được hiển thị ở đây khi không có camera nào trên mạng</p></figcaption></figure>

### Kết nối lần đầu: tải xuống gói hiệu chuẩn

**Lần đầu tiên** một camera cụ thể được kết nối với máy tính, Chloros
sẽ tải xuống gói hiệu chuẩn gốc của camera (\~3,8 MB) trực tiếp từ chính camera đó qua GigE. Trong khi quá trình này diễn ra, hộp thoại sẽ hiển thị một bảng màu xanh lá cây **&quot;Đang tải xuống dữ liệu hiệu chuẩn từ camera&quot;**kèm theo thanh tiến trình cho từng số sê-ri — dự kiến mất khoảng**70 giây** cho mỗi camera. Nếu có hơn sáu camera, các hàng tiến trình sẽ cuộn bên trong bảng. Gói dữ liệu này được lưu trong bộ nhớ đệm trên máy chủ, do đó các lần kết nối sau của cùng một máy ảnh sẽ bỏ qua hoàn toàn quá trình tải xuống (và không bao giờ hiển thị bảng điều khiển này).

### Phân tích hệ thống

Nút **Phân tích hệ thống** trên hộp thoại sẽ quét máy chủ và mạng (nhãn hiển thị &quot;Đang phân tích...&quot; trong khi quá trình diễn ra) và tạo báo cáo chẩn đoán:

* **Máy chủ** — Số lõi CPU và RAM; tên GPU và bộ nhớ, hoặc &quot;GPU: Không phát hiện thấy&quot;.
* **Giao diện mạng** — tên của mỗi NIC, tốc độ kết nối, MTU (kèm nhãn &quot;jumbo&quot; nếu đang hoạt động), trạng thái lên/xuống, và liệu nó có nằm trên bus USB hay không.
* **Camera**— số sê-ri, mẫu, địa chỉ IP và**cổng mạng mà mỗi camera kết nối**.
* **Hiệu suất** — so sánh fps hiện tại với fps lý tưởng cho từng camera theo định dạng pixel, kèm dòng chữ màu xanh lá cây &quot;Tiềm năng: Có thể cải thiện gấp N lần&quot; khi giá trị lý tưởng vượt quá giá trị hiện tại.
* **Cảnh báo và các khuyến nghị được đánh số** — hoặc thông báo “Hệ thống hoạt động tốt với số lượng camera hiện tại” khi không có vấn đề cần khắc phục.

Chạy công cụ này bất cứ khi nào quá trình phát hiện hoặc truyền phát hoạt động bất thường — công cụ sẽ xác định hầu hết các vấn đề liên quan đến NIC (MTU sai, camera kết nối sai giao diện, giới hạn của bộ điều hợp USB) mà không cần thoát khỏi hộp thoại.

### Kết nối mảng camera

Để kết nối hai hoặc nhiều camera thành một **mảng đồng bộ**, hãy sử dụng trình hướng dẫn kết nối mảng (**Connect Camera Array**) thay thế: trình này sẽ hướng dẫn bạn qua quá trình chọn camera chủ/phụ (được điền sẵn thông qua kiểm tra cáp đồng bộ — bất kỳ camera nào được kết nối bằng cáp đều có thể làm camera chủ, và camera RGB
`-FRGB` sẽ được chọn sẵn khi được kết nối bằng cáp), lựa chọn chế độ hiển thị (Tấm riêng biệt so với Tấm kết hợp), và một màn hình cài đặt mảng với dự đoán thời gian thực về fps có thể đạt được và băng thông đường truyền trước khi bạn xác nhận. Trình hướng dẫn và quy trình làm việc với mảng camera được đề cập trong phần “Mảng camera đa camera” của tài liệu hướng dẫn này; quy trình tương đương trên CLI
là “Quy trình kết nối lần đầu camera LATTICE” trong [Tài liệu tham khảo CLI
](../reference/cli-reference.md).

## Kết nối từ CLI
và SDK
Việc truy cập

CLI
và SDK
yêu cầu gói dịch vụ Chloros
+ trả phí và phải đăng nhập; yêu cầu này được áp dụng ở phía máy chủ (`401 AUTH_REQUIRED` khi chưa đăng nhập, `403 PLAN_UPGRADE_REQUIRED` trên gói miễn phí).

```bash
# List cameras on the network (vendor, model, serial, IP, MAC)
chloros-cli lattice info

# Single-camera smoke test: capture one frame (saves every applicable export type)
chloros-cli lattice capture -o output/

# Connect a synchronized array — same smart-prep flow as the GUI
chloros-cli lattice array-connect --serials 213800234,214000533
```

```python
import chloros_sdk

# Persistent live-camera session through the backend
with chloros_sdk.connect_camera("213800234") as cam:
    ...

# Array session (smart-prep: network probe, tier auto-pick, PTP, AE seeding, trigger config)
with chloros_sdk.connect_array(["213800234", "214000533"]) as array:
    ...
```

Các lệnh đầy đủ, tùy chọn và quy trình thu thập dữ liệu: [Tham chiếu CLI
](../reference/cli-reference.md) § `chloros-cli lattice`, [Tham chiếu SDK
](../reference/sdk-reference.md) § `connect_camera()` / `connect_array()`.

## Cách giải quyết hiệu chuẩn khi kết nối

Mỗi camera LATTICE đều mang theo gói hiệu chuẩn nhà máy **trên camera**, và Chloros
cũng kiểm tra đám mây của MAPIR
khi camera kết nối:

| Tình huống   | Chloros
sử dụng gì                                                                                                                                                                                                          |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Trực tuyến**|**Bản hiệu chuẩn mới nhất được công bố cho số sê-ri đó** — bản sao trên đám mây sẽ được ưu tiên hơn bản sao trên máy ảnh. Do đó, máy ảnh đã được hiệu chuẩn lại hoặc cập nhật bởi MAPIR
sẽ tự động cập nhật; người dùng không cần thực hiện bất kỳ thao tác nào. |
| **Ngoại tuyến**|**Gói hiệu chuẩn trên máy ảnh**, giữ nguyên trạng thái hiện tại. Các quy trình làm việc hoàn toàn ngoại tuyến vẫn hoạt động bình thường; chúng chỉ không cập nhật các bản hiệu chuẩn mới hơn cho đến khi máy ảnh kết nối trực tuyến ít nhất một lần (hoặc được khôi phục cài đặt gốc).                                                  |

Tại thời điểm chụp, các hệ số thực tế được áp dụng sẽ được **ghi cố định vào metadata XMP của từng hình ảnh**. Bản cập nhật hiệu chuẩn sau này sẽ không bao giờ tự động thay đổi các hình ảnh bạn đã chụp — việc xử lý lại một bản chụp cũ sẽ sử dụng các hệ số được ghi trong metadata XMP của nó, chứ không phải bản mới nhất hiện tại.

## Khắc phục sự cố

* **&quot;Không tìm thấy camera nào trên mạng&quot;**— hãy kiểm tra cấu hình liên kết cục bộ trong [Cấu hình Mạng](https://mapir.gitbook.io/lattice-camera/setup/network-setup): NIC máy chủ có địa chỉ tĩnh `169.254.x.x/16`, các camera nằm trên cùng một liên kết, không sử dụng DHCP/cổng mặc định. Sau đó, sử dụng**Phân tích hệ thống**trong hộp thoại kết nối để kiểm tra camera nào hiển thị (hoặc không hiển thị) trên mỗi NIC.**Quét lại** sau khi thay đổi cáp hoặc NIC.
* **Hệ thống từng hoạt động bình thường nhưng hiện không thể kết nối** (các cổng bảng mảng với `FRAMES WILL DROP` / `Reduce ROI to enable`) — việc cập nhật trình điều khiển NIC đã tự động đặt lại cài đặt vòng nhận (receive-ring). Áp dụng lại các cài đặt này hoặc chạy lệnh `chloros-cli lattice network --fix` từ một cửa sổ terminal có quyền quản trị; xem [Cài đặt Mạng](https://mapir.gitbook.io/lattice-camera/setup/network-setup).
* **Một camera hiển thị &quot;In Array&quot;** — camera này thuộc về một phiên mảng đã kết nối. Hãy ngắt kết nối mảng để sử dụng camera ở chế độ độc lập.
