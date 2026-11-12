# Mạng và đồng bộ hóa thời gian trên DAQ-E

> Hướng dẫn thiết lập mạng vật lý cho cảm biến — bao gồm việc đi dây, PoE, gán địa chỉ IP và các cài đặt mạng riêng của thiết bị — có trong **[Hướng dẫn sử dụng DAQ](https://mapir.gitbook.io/daq/daq-e/network-setup)**. Trang này đề cập đến phần liên quan đến Chloros: kết nối, đồng bộ thời gian và cách xử lý khi quá trình phát hiện không tìm thấy thiết bị.

DAQ-E là thành viên Ethernet trong gia đình DAQ: được cấp nguồn qua PoE, được phát hiện qua mDNS (dịch vụ `_daq-e._tcp`) và có thể truy cập bằng tên máy chủ được tạo từ ID cảm biến — `daq-e-<6 hex>.local`, ví dụ: `daq-e-def330.local`. Trang này trình bày cách thiết bị truyền dữ liệu trên mạng và cách tham gia đồng bộ thời gian PTP.

## Chế độ truyền tải

| Chế độ | Điểm cuối | Người dùng | Ghi chú |
| --- | --- | --- | --- |
| **Phát đa hướng** (mặc định) | UDP `239.10.10.10:5002` | Bất kỳ thiết bị nào trên cùng mạng LAN đều nhận được cùng một luồng dữ liệu | Mỗi gói dữ liệu được xác thực bằng CRC-16/CCITT |
| **Raw** | Cổng TCP `5000` | Chính xác một client (độc quyền) | Tương thích byte trực tiếp với DAQ-U |

Chloross sử dụng đa phát theo mặc định, điều này cho phép GUI, CLI và SDK cùng theo dõi một cảm biến tại một thời điểm.

## Yêu cầu về mạng

* **Cùng một miền phát sóng.** Máy chạy Chloros phải nằm trên cùng một phân đoạn mạng L2 với cảm biến — quá trình phát hiện mDNS không đi qua các bộ định tuyến.
* **Tường lửa.** Trên Windows, trình cài đặt sẽ thêm quy tắc tường lửa **Chloros Backend (UDP-In)**cho backend vào mọi cấu hình mạng, bao gồm dữ liệu DAQ-E (UDP 5002), mDNS (UDP 5353) và PTP (UDP 319/320), cùng với quy tắc**“Chloros Backend (TCP-In)”**, giới hạn trong mạng con cục bộ, dành cho kết nối mà DAQ-E thiết lập trở lại máy tính trong quá trình cập nhật firmware. Trên hệ điều hành Linux, `.deb` sẽ mở các cổng UDP trong `ufw` hoặc `firewalld` khi tường lửa đó đang hoạt động tại thời điểm cài đặt. Tường lửa của bên thứ ba, hoặc tường lửa được kích hoạt sau này, cũng cần cho phép các cổng tương tự.
* **Nguồn PoE, không có đèn LED trạng thái.** DAQ-E không có đèn LED riêng — hãy kiểm tra nguồn điện qua đèn báo liên kết/PoE trên cổng bộ chuyển mạch hoặc bộ cấp nguồn, và đợi vài giây sau khi bật nguồn để thiết bị khởi động và kết nối vào mạng.

## Kết nối

**GUI:** Tab Cảm biến ánh sáng → Kết nối cảm biến → Loại thiết bị "DAQ-E (Ethernet)". Quá trình phát hiện chỉ diễn ra khi hộp thoại kết nối hiển thị trên màn hình (tìm kiếm mDNS kết hợp quét ARP trên Windows), lặp lại mỗi 15 giây; nút Refresh sẽ quét lại ngay lập tức. Các cảm biến được phát hiện sẽ xuất hiện trong danh sách thả xuống; cảm biến được phát hiện đầu tiên sẽ được chọn tự động.

<!-- SCREENSHOT-NEEDED: DAQ connect dialog with Device Type set to "DAQ-E (Ethernet)" and at least one discovered sensor listed in the Hostname/IP dropdown (e.g. daq-e-xxxxxx.local), Connect button enabled. -->

**CLI** (backend đang chạy):

```bash
chloros-cli daq pool-connect --eth                              # auto-discover on the LAN
chloros-cli daq pool-connect --eth-host daq-e-def330.local      # explicit host — the reliable form
chloros-cli daq pool-connect --eth-host 192.168.1.57            # a plain IP works too
```

Một thiết bị DAQ-E đã có trong danh sách sẽ được nhận diện cả bằng tên máy chủ lẫn địa chỉ IP: việc kết nối lại thiết bị đó dưới một địa chỉ khác sẽ trả về mục đã có trong danh sách thay vì tạo một mục mới.

### Máy chủ có nhiều giao diện mạng và lần kết nối đầu tiên sau khi khởi động

Trên các máy chủ có nhiều hơn một giao diện mạng đang hoạt động, lần kết nối **đầu tiên** `pool-connect --eth` sau khi khởi động có thể không tìm thấy thiết bị ngay cả khi cảm biến đang hoạt động bình thường — quá trình quét phát hiện có thể bỏ qua giao diện mạng mà cảm biến đang sử dụng do bộ nhớ đệm ARP vẫn chưa được cập nhật. Giải pháp đáng tin cậy là bỏ qua quá trình phát hiện và truyền địa chỉ một cách rõ ràng:

```bash
chloros-cli daq pool-connect --eth-host daq-e-def330.local
```

`--eth-host` chấp nhận tên máy chủ mDNS hoặc địa chỉ IP, luôn nhắm đến cảm biến đúng và là dạng được khuyến nghị cho các skript và cài đặt không giao diện người dùng. Trong giao diện đồ họa (GUI), hãy sử dụng nút Refresh trong hộp thoại kết nối và cho phép quá trình quét lại diễn ra.

## Cài đặt thiết bị và phần mềm cơ sở

Bản thân cảm biến lưu trữ cài đặt mạng của mình: DHCP (cài đặt mặc định, sẽ chuyển sang địa chỉ liên kết cục bộ 169.254.x.x khi không có máy chủ DHCP phản hồi) hoặc địa chỉ tĩnh. Nó cũng lưu trữ thông tin về việc liệu cảm biến có bắt đầu truyền phát ngay khi bật nguồn hay không. Thay đổi các thiết lập này trong giao diện đồ họa (biểu tượng bánh răng cài đặt của cảm biến, **Mạng**:**Thay đổi…**,**Truyền dữ liệu ngay khi bật nguồn**,**Khởi động lại cảm biến**; xem [Tab DAQ](gui.md)), hoặc từ giao diện dòng lệnh (CLI) và bộ công cụ phát triển phần mềm (SDK) bằng lệnh `pool-set-network`, `pool-set-auto-stream` và `pool-reboot` (`DAQSensorSession.set_network()`, `set_auto_stream()`, `reboot()`; xem [Tài liệu tham khảo CLI](../reference/cli-reference.md#daq-e-device-settings)). Cảm biến sẽ khởi động lại để áp dụng thay đổi mạng, và Chloros sẽ kết nối lại với nó. Địa chỉ tĩnh phải nằm trên mạng của máy tính đang chạy Chloros: Chloros sẽ từ chối kết nối với địa chỉ mà nó không thể truy cập được sau khi khởi động lại. ID của cảm biến và tên máy chủ `daq-e-<id>.local` của nó không bao giờ thay đổi. Mới trong phiên bản 1.3.0.

**Tính năng cập nhật firmware đã được tích hợp sẵn trong giao diện người dùng (GUI).** Khi một thiết bị DAQ-E đã kết nối đang chạy phiên bản firmware cũ hơn so với bản firmware đi kèm với bản dựng Chloros của bạn, hàng hiển thị cảm biến của nó sẽ xuất hiện biểu tượng **Có bản cập nhật** màu cam, và cửa sổ cài đặt hình bánh răng sẽ hiển thị <version>nút</version>

"Cập nhật lên <version>". Quá trình cập nhật diễn ra qua mạng trong khoảng 30 giây; cảm biến sẽ tự động khởi động lại và kết nối lại, và nếu quá trình truyền tải bị gián đoạn, phiên bản firmware hiện tại vẫn được giữ nguyên. Từ CLI hoặc SDK, và trên Linux (hệ điều hành không có GUI), hãy chạy lệnh `chloros-cli daq pool-update-firmware --sensor-id <id>` (`DAQSensorSession.update_firmware()`).

<!-- SCREENSHOT-NEEDED: DAQ-E per-sensor settings modal showing the DAQ-E-only rows: Hostname/IP, Firmware row with the "Update to <ver>" button (or "Up to date"), and the PTP Sync row with a live state value. -->

## Đồng bộ thời gian PTP

Phần mềm DAQ-E phiên bản v1.2.0+ tham gia vào tiêu chuẩn IEEE 1588 PTPv2 với vai trò là đồng hồ thông thường (chỉ là thiết bị phụ). **Hệ thống nền của máy chủ Chloros đóng vai trò là grandmaster PTP** — mọi thiết bị DAQ-E và mọi camera LATTICE trên mạng LAN đều đồng bộ với nó trong miền 0, đảm bảo tất cả dấu thời gian của thiết bị nằm trong phạm vi dung sai khoảng 1 ms. Chính đồng hồ chung này giúp các giá trị đo của DAQ có thể khớp dấu thời gian với thời gian phơi sáng của camera (xem [Ghi hình & định dạng .daq](recording.md)).

Kiểm tra đồng bộ hóa từ giao diện dòng lệnh (CLI):

| Lệnh | Hiển thị |
| --- | --- |
| `chloros-cli time-sync status` | Trạng thái máy chủ grandmaster, mức ưu tiên BMCA, định danh đồng hồ, và `health`: `OK`, hoặc `SENDS_FAILING` / `DEGRADED` / `NO_SOCKETS` / `PEERS_SILENT` kèm theo thông báo từ `health_detail` giải thích lý do tại sao các gói tin của nó không đến được mạng |
| `chloros-cli time-sync peers` | Tất cả các thiết bị phụ được phát hiện (cảm biến DAQ-E + camera LATTICE) |
| `chloros-cli time-sync cameras` | Tình trạng PTP của từng camera (`PtpStatus`, `PtpOffsetFromMaster`, `PtpMeanPathDelay`) |
| `chloros-cli time-sync restart` | Khởi động lại quá trình grandmaster |

Trong giao diện người dùng (GUI), cửa sổ cài đặt DAQ-E hiển thị một hàng **PTP Sync** theo thời gian thực với trạng thái PTP hiện tại của cảm biến.**Một thiết bị DAQ-E đã tắt PTP sẽ vẫn ở trạng thái tắt.** Thiết bị lưu cài đặt bật/tắt PTP trong bộ nhớ riêng của mình, do đó một thiết bị đã tắt PTP sẽ vẫn ở trạng thái tắt sau khi kết nối lại hoặc khởi động lại nguồn, và các dấu thời gian của nó sẽ không được đồng bộ hóa theo PTP. `chloros-cli daq pool-list` sẽ đánh dấu thiết bị đó (`!! PTP is switched OFF on this unit …`) và hiển thị lệnh để khắc phục:

```bash
chloros-cli daq pool-enable-ptp --sensor-id daq-e-def330
```

Chloros không bao giờ tự động bật lại thiết bị, vì trạng thái tắt có thể là chủ ý.

Chi tiết dành cho người dùng yêu cầu căn chỉnh nghiêm ngặt:

* Mỗi gói dữ liệu truyền phát đều chứa trường cờ; **bit 2 được đặt trên các khung có dấu thời gian được đồng bộ hóa theo PTP**. Các đường ống xử lý yêu cầu căn chỉnh nghiêm ngặt giữa camera và DAQ nên sử dụng bit này làm điều kiện lọc.
* Trước khi thực hiện ghi đồng bộ, hãy xác nhận cảm biến xuất hiện trong `chloros-cli time-sync peers` và rằng `chloros-cli daq pool-list` không đánh dấu PTP của cảm biến là đã tắt. (Công cụ phần cứng trực tiếp nội bộ của MAPIR cũng có thể lọc ghi dựa trên trạng thái khóa PTP thông qua cờ `--wait-ptp`, chờ tối đa 15 giây cho đến khi cảm biến đạt trạng thái SLAVE; công cụ này không nằm trong phiên bản CLI được phân phối.)
* Trong khi PTP đang hoạt động ở chế độ nô lệ, cảm biến sẽ từ chối các lệnh đẩy đồng hồ thủ công (“PTP đang cung cấp đồng hồ”). Điều này là theo thiết kế — hãy tin tưởng vào PTP.

## Ghi chú về Linux

* **Các cổng PTP không yêu cầu quyền root.** PTP sử dụng các cổng UDP 319/320. `.deb` ghi vào `/etc/sysctl.d/60-chloros-ptp.conf` (`net.ipv4.ip_unprivileged_port_start = 319`), do đó phần backend sẽ sử dụng các cổng này khi chạy dưới quyền người dùng của bạn. Từ phiên bản 1.3.0 trở đi, gói phần mềm không thiết lập quyền truy cập tệp (`setcap`) cho phần mềm nền và không cần `libcap2-bin`; đừng tự thêm quyền truy cập này, vì phần mềm nền sau khi giải nén sẽ không khởi động được. Nếu PTP không thể gán cổng 319, hãy tham khảo [Cài đặt trên Linux](../linux/linux-installation.md#ptp-will-not-start--cannot-bind-port-319).

* **Máy chủ Jetson không màn hình / Linux:** gói phần mềm cài đặt các dịch vụ backend nhưng không kích hoạt dịch vụ nào. Để PTP luôn hoạt động (và đảm bảo tính sẵn sàng của DAQ) mà không cần ai thực thi lệnh, hãy kích hoạt dịch vụ với tư cách người dùng của bạn:

  ```bash
  sudo systemctl enable --now chloros-backend@$USER.service    # $USER expands to your user name
  ```

  Nếu không, PTP chỉ chạy khi có thứ gì đó đã khởi động dịch vụ nền — một lệnh `chloros-cli` hoặc một tập lệnh SDK. Xem [Cài đặt trên Linux](../linux/linux-installation.md#always-on-ptp-for-headless-hosts) để biết về tài khoản root `chloros-backend.service` và cách chuyển đổi khỏi tài khoản này.

## Khắc phục sự cố: "Không tìm thấy thiết bị DAQ-E"

| Kiểm tra | Chi tiết |
| --- | --- |
| Nguồn điện | Đèn LED trên cảm biến không sáng — kiểm tra đèn báo PoE và đèn báo kết nối của cổng chuyển mạch/bộ cấp nguồn; đợi vài giây sau khi bật nguồn |
| Vùng phát sóng | Máy chủ và cảm biến nằm trên cùng một phân đoạn L2; mDNS không định tuyến |
| Tường lửa | Windows: quy tắc **Chloros Backend (UDP-In)** của trình cài đặt cho phép luồng dữ liệu DAQ-E (UDP 5002), mDNS (5353) và PTP (319/320) đi vào; tường lửa của bên thứ ba cũng cần thiết lập tương tự. Linux: `.deb` chỉ mở các cổng này trong `ufw` / `firewalld` nếu tường lửa đang hoạt động tại thời điểm cài đặt — bạn cần tự cho phép chúng nếu tường lửa được kích hoạt sau đó |
| Máy chủ đa NIC | Quá trình phát hiện duyệt mDNS với cơ chế dự phòng ARP; `--eth-host <ip-or-hostname>` bỏ qua quá trình phát hiện và kết nối trực tiếp |
| Quét lại qua GUI | Quá trình phát hiện chỉ chạy khi hộp thoại kết nối đang mở; hãy sử dụng nút Refresh |
