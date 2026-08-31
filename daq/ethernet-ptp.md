# Mạng và đồng bộ hóa thời gian trên DAQ-E

> Việc thiết lập mạng vật lý cho cảm biến — bao gồm đi dây, PoE, gán địa chỉ IP và các cài đặt mạng của chính thiết bị — được hướng dẫn chi tiết trong **[Hướng dẫn sử dụng DAQ](https://mapir.gitbook.io/daq/daq-e/network-setup)**. Trang này tập trung vào khía cạnh của bộ điều khiển mạng (Chloros

): kết nối, đồng bộ thời gian và cách xử lý khi quá trình phát hiện không tìm thấy thiết bị.

DAQ-E là thành viên Ethernet trong dòng sản phẩm DAQ: được cấp nguồn qua PoE, được phát hiện qua mDNS (dịch vụ `_daq-e._tcp`) và có thể truy cập bằng tên máy chủ được tạo từ ID cảm biến — `daq-e-<6 hex>.local`, ví dụ: `daq-e-def330.local`. Trang này trình bày cách thiết bị truyền dữ liệu trên mạng và cách tham gia đồng bộ thời gian PTP.

## Chế độ truyền tải

| Chế độ | Điểm cuối | Người dùng | Ghi chú |
| --- | --- | --- | --- |
| **Phát đa hướng** (mặc định) | UDP `239.10.10.10:5002` | Bất kỳ thiết bị nào trên cùng mạng LAN đều nhận được cùng một luồng dữ liệu | Mỗi gói dữ liệu được kiểm tra tính toàn vẹn bằng CRC-16/CCITT |
| **Raw** | Cổng TCP `5000` | Chính xác một client (độc quyền) | Tương thích byte trực tiếp với DAQ-U |

Chế độChloros

sử dụng đa phát theo mặc định, điều này cho phép giao diện người dùng (GUI),CLI

vàSDK

cùng theo dõi một cảm biến cùng lúc.

## Yêu cầu mạng

* **Cùng một miền phát sóng.** Máy chạyChloros

phải nằm trên cùng một phân đoạn mạng L2 với cảm biến — quá trình phát hiện mDNS không đi qua các bộ định tuyến.
* **Thông báo tường lửaWindows

: hãy chấp nhận nó.** Lần đầu tiênChloros

thiết lập kết nối với các socket đa phát,Windows

Defender sẽ yêu cầu xác nhận một lần. Việc cho phép sẽ bao gồm dữ liệu DAQ-E (UDP 5002), mDNS (UDP 5353) và PTP (UDP 319/320). TrênLinux

, quá trình này diễn ra im lặng.
* **Nguồn PoE, không có đèn LED trạng thái.** DAQ-E không có đèn LED riêng — hãy kiểm tra nguồn thông qua đèn báo liên kết/PoE trên cổng bộ chuyển mạch hoặc bộ cấp nguồn, và đợi vài giây sau khi bật nguồn để thiết bị khởi động và kết nối vào mạng.

## Kết nối

**GUI:** Tab Cảm biến ánh sáng → Kết nối cảm biến → Loại thiết bị &quot;DAQ-E (Ethernet)&quot;. Quá trình phát hiện chỉ diễn ra khi hộp thoại kết nối còn hiển thị trên màn hình (duyệt mDNS kết hợp quét ARP trênWindows

), lặp lại mỗi 15 giây; nút Làm mới sẽ quét lại ngay lập tức. Các cảm biến được phát hiện sẽ xuất hiện trong danh sách thả xuống; cảm biến đầu tiên được phát hiện sẽ được chọn tự động.

<!-- SCREENSHOT-NEEDED: DAQ connect dialog with Device Type set to "DAQ-E (Ethernet)" and at least one discovered sensor listed in the Hostname/IP dropdown (e.g. daq-e-xxxxxx.local), Connect button enabled. -->

**CLI** (backend đang chạy):

```bash
chloros-cli daq pool-connect --eth                              # auto-discover on the LAN
chloros-cli daq pool-connect --eth-host daq-e-def330.local      # explicit host — the reliable form
chloros-cli daq pool-connect --eth-host 192.168.1.57            # a plain IP works too
```

### Máy chủ có nhiều NIC và lần kết nối đầu tiên sau khi khởi động

Trên các máy chủ có nhiều hơn một giao diện mạng đang hoạt động, lần kết nối **đầu tiên** `pool-connect --eth` sau khi khởi động có thể không tìm thấy gì ngay cả khi cảm biến đang hoạt động bình thường — quá trình quét phát hiện có thể bỏ sót giao diện mà cảm biến đang hoạt động do bộ đệm ARP vẫn chưa được làm nóng. Cách khắc phục đáng tin cậy là bỏ qua quá trình phát hiện và truyền địa chỉ một cách rõ ràng:

```bash
chloros-cli daq pool-connect --eth-host daq-e-def330.local
```

`--eth-host` chấp nhận tên máy chủ mDNS hoặc địa chỉ IP, luôn nhắm đến cảm biến đúng và là dạng được khuyến nghị cho các kịch bản và cài đặt không giao diện người dùng. Trong giao diện đồ họa (GUI), hãy sử dụng nút Refresh (Làm mới) trong hộp thoại kết nối và cho phép một chu kỳ quét lại.

## Cài đặt thiết bị và firmware

Bản thân cảm biến lưu trữ các cài đặt mạng — địa chỉ IP tĩnh so với DHCP + địa chỉ liên kết cục bộ, tên thiết bị, tự động truyền dữ liệu khi khởi động, mật khẩu OTA. Các cài đặt phía thiết bị này không được cung cấp dưới dạng lệnh trong bản phát hànhCLI

; chúng được quản lý thông qua giao diện người dùngChloros

khi được hiển thị, hoặc thông qua hỗ trợMAPIR

.

**Tính năng cập nhật firmware được tích hợp sẵn trong giao diện người dùng (GUI).** Khi một thiết bị DAQ-E được kết nối đang chạy phiên bản firmware cũ hơn so với hình ảnh firmware đi kèm với bản dựngChloros

của bạn, hàng cảm biến của thiết bị sẽ hiển thị một biểu tượng màu cam có nội dung **&quot;Có bản cập nhật&quot;**, và cửa sổ cài đặt hình bánh răng sẽ cung cấp <version>nút</version>

&quot;Cập nhật lên<version>&quot;. Quá trình cập nhật diễn ra qua mạng trong khoảng 30 giây; cảm biến sẽ tự động khởi động lại và kết nối lại, và nếu quá trình truyền tải bị gián đoạn, phiên bản firmware hiện tại sẽ vẫn được giữ nguyên.

<!-- SCREENSHOT-NEEDED: DAQ-E per-sensor settings modal showing the DAQ-E-only rows: Hostname/IP, Firmware row with the "Update to <ver>" button (or "Up to date"), and the PTP Sync row with a live state value. -->

## Đồng bộ thời gian PTP

Phần mềm DAQ-E phiên bản v1.2.0+ tham gia vào tiêu chuẩn IEEE 1588 PTPv2 với vai trò là đồng hồ thông thường (chỉ là thiết bị phụ). **Hệ thống nền tảng của máy chủ Chloros đóng vai trò là đồng hồ chủ PTP** — mọi thiết bị DAQ-E và mọi camera LATTICE trên mạng LAN đều đồng bộ với nó trong miền 0, đảm bảo tất cả dấu thời gian của thiết bị nằm trong dung sai khoảng 1 ms. Chính đồng hồ chia sẻ này giúp các giá trị đo của DAQ có thể khớp thời gian với các lần phơi sáng của camera (xem [Ghi hình &amp; định dạng .daq](recording.md)).

Kiểm tra trạng thái đồng bộ từ `CLI`:

| Lệnh | Hiển thị |
| --- | --- |
| `chloros-cli time-sync status` | Trạng thái grandmaster của máy chủ, mức ưu tiên BMCA, định danh đồng hồ |
| `chloros-cli time-sync peers` | Tất cả các thiết bị phụ được phát hiện (cảm biến DAQ-E + camera LATTICE) |
| `chloros-cli time-sync cameras` | Tình trạng PTP của từng camera (`PtpStatus`, `PtpOffsetFromMaster`, `PtpMeanPathDelay`) |
| `chloros-cli time-sync restart` | Khởi động lại quy trình grandmaster |

Trong giao diện người dùng (GUI), cửa sổ cài đặt DAQ-E hiển thị một hàng **PTP Sync** theo thời gian thực với trạng thái PTP hiện tại của cảm biến.

Chi tiết dành cho người dùng yêu cầu căn chỉnh nghiêm ngặt:

* Mỗi gói dữ liệu truyền phát đều chứa trường cờ (flags); **bit thứ 2 được đặt trên các khung có dấu thời gian được đồng bộ hóa PTP**. Các đường ống xử lý yêu cầu căn chỉnh nghiêm ngặt giữa camera và DAQ nên sử dụng bit này làm điều kiện lọc.
* Trước khi thực hiện ghi đồng bộ, hãy xác nhận cảm biến xuất hiện trong `chloros-cli time-sync peers`. (Công cụ phần cứng trực tiếp nội bộ của MAPIR cũng có thể điều khiển việc ghi dựa trên trạng thái khóa PTP thông qua cờ `--wait-ptp`, cho phép chờ tối đa 15 giây để cảm biến đạt trạng thái SLAVE; công cụ này không nằm trong phiên bản CLI được phân phối.)
* Trong khi PTP đang hoạt động ở chế độ slaver, cảm biến sẽ từ chối các lệnh đẩy đồng hồ thủ công (“PTP đang cung cấp đồng hồ”). Điều này là theo thiết kế — hãy tin tưởng vào PTP.

## Ghi chú về Linux

* **PTP cần cờ `libcap2-bin` tại thời điểm cài đặt.** Tập lệnh `.deb` sau cài đặt sẽ cấp quyền cho `cap_net_bind_service=+ep` trên `/usr/lib/chloros/chloros-backend` để nó có thể gán các cổng PTP 319/320 mà không cần quyền root. Nếu thiếu `libcap2-bin`, bước đó sẽ bị bỏ qua và PTP sẽ không khởi động được. Cách khắc phục:

  ```bash
  sudo apt install libcap2-bin
  sudo apt reinstall chloros
  ```

* **Jetson / Raspberry Pi không có giao diện đồ họa:** Khi cài đặt lần đầu, đơn vị systemd `chloros-backend.service` được tạo ra nhưng chưa được kích hoạt. Để đảm bảo PTP luôn hoạt động (và khả năng truy cập DAQ) mà không cần giao diện đồ họa:

  ```bash
  sudo systemctl enable --now chloros-backend.service
  ```

  Nếu không có đơn vị này, PTP chỉ chạy khi giao diện người dùng Chloros đang mở.

## Khắc phục sự cố: &quot;Không tìm thấy thiết bị DAQ-E&quot;

| Kiểm tra | Chi tiết |
| --- | --- |
| Nguồn | Đèn LED trên cảm biến không sáng — kiểm tra đèn báo PoE và đèn báo kết nối của cổng switch/injector; đợi vài giây sau khi bật nguồn |
| Vùng phát sóng | Máy chủ và cảm biến nằm trên cùng một phân đoạn L2; mDNS không định tuyến |
| Tường lửaWindows | Chấp nhận lời nhắc của Defender khi chạy lần đầu (UDP 5002, 5353, 319/320) |
| Máy chủ có nhiều NIC | Lần phát hiện đầu tiên sau khi khởi động có thể không tìm thấy cảm biến — kết nối bằng `--eth-host <ip-or-hostname>` |
| Quét lại qua GUI | Quá trình phát hiện chỉ chạy khi hộp thoại kết nối đang mở; sử dụng nút Refresh |</version>
