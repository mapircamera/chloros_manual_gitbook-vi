# Hướng dẫn khởi động nhanh CLI (pool-*)

Các bộ điều khiển `chloros-cli` được cung cấp sẵn điều khiển các cảm biến DAQ thông qua bộ lệnh **`daq pool-*`** — các client HTTP nhẹ điều khiển cảm biến thông qua nhóm cảm biến liên tục của backend Chloros. Hệ thống backend quản lý kênh truyền tải, do đó giao diện người dùng (GUI), giao diện dòng lệnh (CLI) và các skript SDK đều chia sẻ chung một kết nối hoạt động thay vì phải cạnh tranh để sử dụng cổng. Mọi thứ khách hàng cần đều có thể truy cập thông qua `pool-*`: kết nối, truyền dữ liệu, ghi lại các tệp `.daq` đã hiệu chuẩn và thay đổi cấu hình cap.

`pool-*` là giao diện DAQ được hỗ trợ trong các bản phát hành chính thức. `chloros-cli daq --help` liệt kê các lệnh con của `pool-*`, và `daq discover` / `daq list` thực hiện cùng một quá trình quét như `pool-discover`. Trong trường hợp không có lệnh con DAQ trực tiếp trên phần cứng, chương trình sẽ thoát với một thông báo lỗi rõ ràng nêu tên gói phần mềm bị thiếu và hướng dẫn bạn quay lại `pool-*` — không có lỗi nào xảy ra mà không có thông báo. (`pip install chloros-sdk` cũng không cung cấp các lệnh điều khiển trực tiếp phần cứng.)

***

## Yêu cầu tiên quyết

* **Backend Chloros phải đang chạy** — các lệnh `pool-*` là các client HTTP, không phải trình điều khiển phần cứng. Trên Windows, hãy khởi động ứng dụng desktop Chloros (ứng dụng này sẽ khởi chạy backend). Trên Linux/Jetson không có giao diện đồ họa, hãy kích hoạt dịch vụ với tư cách người dùng của bạn: `sudo systemctl enable --now chloros-backend@$USER.service` (xem [Cài đặt Linux](../linux/linux-installation.md#always-on-ptp-for-headless-hosts)).
* **Đăng nhập Chloros+ (gói trả phí)**: hãy chạy lệnh `chloros-cli login` trước. Việc thực thi được thực hiện ở phía máy chủ — nếu không đăng nhập, các lệnh sẽ thất bại với `401 AUTH_REQUIRED`; trên gói miễn phí (Iron), các lệnh sẽ thất bại với `403 PLAN_UPGRADE_REQUIRED`.
* Các lệnh nhắm đến `http://127.0.0.1:5000` theo mặc định; dòng lệnh `daq pool-*` sẽ tuân theo biến môi trường `CHLOROS_BACKEND_URL` nếu hệ thống backend của bạn chạy ở nơi khác.

***

## Một phiên làm việc kéo dài năm phút

```bash
# 1. Connect a sensor into the backend pool (pick the line matching your model)
chloros-cli daq pool-connect                                  # smart-detect any DAQ
chloros-cli daq pool-connect --port COM3                      # DAQ-U on a specific COM port
chloros-cli daq pool-connect --mac AA:BB:CC:DD:EE:FF          # DAQ-M by BLE MAC
chloros-cli daq pool-connect --eth-host daq-e-def330.local    # DAQ-E by hostname (reliable)
#    (a DAQ-M's BLE MAC is not printed anywhere: find it with `chloros-cli daq pool-discover --only ble`)

# 2. List the pool — this shows the sensor_id used by every command below
chloros-cli daq pool-list

# 3. Read the most recent calibrated spectrum frame (add --json for scripting)
chloros-cli daq pool-latest --sensor-id daq-e-def330 --json

# 4. Record a calibrated .daq file for 60 seconds
chloros-cli daq pool-record --sensor-id daq-e-def330 --duration 60 \
  --device-name "field-A"

# 5. Release the sensor when done
chloros-cli daq pool-disconnect --sensor-id daq-e-def330
```

***

## `pool-connect` — mở một cảm biến trong nhóm

| Biến thể | Ý nghĩa |
| --- | --- |
| `daq pool-connect` | Tự động phát hiện: tìm bất kỳ thiết bị DAQ nào trên máy này. |
| `daq pool-connect --port PORT` | DAQ-U trên một cổng nối tiếp cụ thể (ví dụ: `COM3`, `/dev/ttyUSB0`). |
| `daq pool-connect --ble` | DAQ-M qua BLE, quét MAC tự động. |
| `daq pool-connect --mac MAC` | DAQ-M tại địa chỉ MAC BLE đã biết (ngụ ý `--ble`). |
| `daq pool-connect --eth-host HOST` | DAQ-E tại tên máy chủ hoặc địa chỉ IP đã biết — **đường dẫn đáng tin cậy**. |
| `daq pool-connect --eth` | DAQ-E với tính năng tự động phát hiện (mDNS, với phương án dự phòng ARP). Xem lưu ý bên dưới. |

Các cờ điều chỉnh, tất cả đều tùy chọn:

| Cờ | Ý nghĩa |
| --- | --- |
| `--integration-time MS` / `-t MS` | Thời gian tích hợp thủ công tính bằng mili giây. |
| `--frame-avg N` / `-f N` | Số khung hình được tính trung bình cho mỗi phổ được báo cáo. |
| `--no-ae` | Tắt tính năng phơi sáng tự động (AE được bật theo mặc định). |
| `--no-stream` | Kết nối mà không khởi động luồng (tiếp tục sau đó bằng `pool-stream --start`). |
| `--cap-id CAP` | Hồ sơ hiệu chỉnh giới hạn; mặc định của backend là `sunshine_cosine`. Xem [`pool-set-cap`](#pool-set-cap-declare-the-fitted-cap). |

Khi kết nối một thiết bị DAQ-E đã có trong nhóm dưới một địa chỉ khác của nó — tên máy chủ `daq-e-<id>.local` hoặc địa chỉ IP — hệ thống sẽ trả về mục nhập hiện có trong nhóm thay vì mở thiết bị lần thứ hai.

{% hint style="warning" %}
**Lưu ý về tính năng tự động phát hiện của `--eth`.** Trên một máy chủ có nhiều giao diện mạng (hơn một giao diện mạng đang hoạt động), lần *đầu tiên* `pool-connect --eth` sau khi khởi động có thể không tìm thấy gì ngay cả khi cảm biến vẫn hoạt động bình thường — quá trình tìm kiếm có thể bỏ sót giao diện của cảm biến khi bộ đệm ARP chưa được làm nóng. Nếu `--eth` không tìm thấy gì, hãy thử lại hoặc bỏ qua hoàn toàn quá trình phát hiện bằng cách sử dụng `--eth-host <ip-or-hostname>`, đây là phương án đáng tin cậy trên các máy chủ đa kết nối. Tên máy chủ của DAQ-E là `daq-e-<id>.local` (ví dụ: `daq-e-def330.local`); địa chỉ IP thuần túy của nó cũng hoạt động được.
{% endhint %}

## `pool-discover` — quét mà không kết nối

```bash
chloros-cli daq pool-discover                  # USB, BLE and Ethernet
chloros-cli daq pool-discover --only ble       # BLE only: this is how you find a DAQ-M's MAC
chloros-cli daq pool-discover --json           # machine-readable
```

Quét các cảm biến mà bạn có thể kết nối và hiển thị địa chỉ để kết nối, mà không thực sự kết nối. `--only` nhận một tập con các cảm biến được phân tách bằng dấu phẩy từ `usb,ble,eth`; `--timeout SEC` thiết lập thời gian quét cho từng giao thức truyền tải (mặc định là 5 giây, giới hạn trong khoảng 1–20 giây). Các cảm biến đã được mở trong nhóm sẽ không được liệt kê (một DAQ-M đã kết nối sẽ ngừng phát tín hiệu) — hãy sử dụng `pool-list` cho những cảm biến đó. Trên bản dựng đã phát hành, `chloros-cli daq discover` và `daq list` thực hiện quá trình quét này.

## `pool-list` — xem những gì đang được kết nối

Hiển thị mọi cảm biến trong nhóm backend, bao gồm cả `sensor_id` mà mọi lệnh khác đều cần:

| Mô hình | Hình dạng `sensor_id` | Ví dụ |
| --- | --- | --- |
| DAQ-U / DAQ-M | 5 octet có dấu gạch ngang | `CB-7C-A8-2E-5F` |
| DAQ-E | `daq-e-<6 hex digits>` | `daq-e-def330` |

Một thiết bị DAQ-E có PTP đã bị tắt (cài đặt được lưu trên thiết bị) sẽ được đánh dấu trong `pool-list` bằng `!! PTP is switched OFF on this unit (saved in its flash): its timestamps are not PTP-synced.` cùng với lệnh để bật lại — xem [`pool-enable-ptp`](#pool-enable-ptp-switch-a-daq-es-ptp-back-on).

## `pool-latest` — đọc các khung phổ

```bash
chloros-cli daq pool-latest --sensor-id daq-e-def330 --recent 10 --json
```

Trả về khung dữ liệu mới nhất hoặc các khung dữ liệu mới nhất; `--recent N` và `--json` tạo ra đầu ra có thể đọc được bởi máy tính để sử dụng trong kịch bản. Các khung dữ liệu là cường độ bức xạ quang phổ (W/m²/nm) đã được hiệu chuẩn phóng xạ trên lưới 135 điểm, dải 340–1010 nm, với cấu hình nắp cảm biến đã được áp dụng sẵn. Để có các giá trị bức xạ định lượng, hãy tính trung bình ít nhất 15 giây dữ liệu — đây là đặc tính của thiết bị, không phải lỗi.

## `pool-stream` — tạm dừng hoặc tiếp tục truyền dữ liệu

```bash
chloros-cli daq pool-stream --sensor-id daq-e-def330 --stop    # pause
chloros-cli daq pool-stream --sensor-id daq-e-def330 --start   # resume
```

## `pool-record` — ghi tệp `.daq`

```bash
chloros-cli daq pool-record --sensor-id daq-e-def330 --duration 150 \
  --output ~/Documents/spectra --device-name "rooftop-A"
chloros-cli daq pool-record --sensor-id daq-e-def330 --stop
```

| Cờ | Mặc định | Ý nghĩa |
| --- | --- | --- |
| `--duration SEC` / `-d SEC` | `0` | Thời lượng ghi trong giây; `0` có nghĩa là chạy cho đến khi bạn thực thi lệnh `--stop`. |
| `--output DIR` / `-o DIR` | `~/Documents/DAQ Live View/` | Thư mục đầu ra. Với backend trên máy tính này, đường dẫn tương đối được xác định dựa trên thư mục mà bạn đã chạy lệnh; với backend từ xa, đường dẫn được xác định **trên máy đang chạy backend**. |
| `--device-name NAME` | — | Nhãn được lưu kèm với bản ghi. |
| `--stop` | — | Dừng bản ghi đang diễn ra. |
| `--confirm-after SEC` | `20` | Sau khi bắt đầu, chờ tối đa khoảng thời gian này để dòng đầu tiên được ghi và cảnh báo lớn (`!! WARNING: no rows written after …s. This recording is producing an EMPTY file.`) nếu không có dòng nào đến. `0` vô hiệu hóa việc kiểm tra. |
| `--heartbeat SEC` | `30` | Với `--duration`, in số lượng hàng mỗi SEC giây và cảnh báo nếu số lượng ngừng tăng. `0` vô hiệu hóa tính năng này. |

{% hint style="info" %}
Việc ghi dữ liệu diễn ra ở phía máy chủ, do đó tệp `.daq` sẽ được lưu trên hệ thống tệp của **máy chủ** — theo mặc định là tại `~/Documents/DAQ Live View/` ở đó, không nhất thiết phải là nơi bạn chạy lệnh CLI. Tên tệp bao gồm ID cảm biến và dấu thời gian.
{% endhint %}

## `pool-set-cap` — khai báo nắp đã lắp

```bash
chloros-cli daq pool-set-cap --sensor-id daq-e-def330 --cap-id sunshine_cosine
```

ID nắp chọn hồ sơ hiệu chỉnh được đo tại nhà máy áp dụng cho mọi phổ, và nó **phải khớp với nắp được lắp vật lý trên cảm biến** — cả cảm biến lẫn phần mềm đều không thể tự phát hiện nắp, và lựa chọn này được ghi vào mọi tệp `.daq`. Giá trị mặc định ở mọi nơi là `sunshine_cosine` (mỗi thiết bị DAQ đều được xuất xưởng với nắp hiệu chỉnh cosin Sunshine đã được lắp sẵn, có độ suy giảm khoảng 12 lần theo thiết kế — việc thay đổi nắp mà không khai báo sẽ khiến phổ bị hiệu chỉnh sai với hệ số tương đương).

| `--cap-id` | Có sẵn trên |
| --- | --- |
| `sunshine_cosine` (mặc định) | DAQ-U, DAQ-M, DAQ-E |
| `fov_15`, `fov_45`, `fov_90` | DAQ-U, DAQ-E |
| `fov_30`, `fov_60` | Chỉ DAQ-U |
| `none` | Chỉ DAQ-E — xem chú thích |

Mã nắp nằm ngoài bộ cài đặt của cảm biến sẽ bị từ chối khi kết nối kèm theo thông báo lỗi rõ ràng. `none` (DAQ-E) có nghĩa là nắp đã được tháo ra về mặt vật lý — thiết bị vẫn áp dụng cấu hình hình học mặc định của nhà sản xuất cho bộ khuếch tán kính lõm của DAQ-E, do đó đây không phải là chế độ không hoạt động (no-op), và một DAQ-E không có nắp là cấu hình trên bàn thí nghiệm, không phải cấu hình được hỗ trợ trong môi trường thực tế. (Một DAQ-U không có nắp là loại "true-bare" và không cần bất kỳ cấu hình hiệu chỉnh nào; DAQ-M được sử dụng kèm với nắp Sunshine.)

## `pool-disconnect` — giải phóng cảm biến

```bash
chloros-cli daq pool-disconnect --sensor-id daq-e-def330   # one sensor
chloros-cli daq pool-disconnect --all                      # everything in the pool
```

`pool-disconnect` giải phóng cảm biến cho bất kỳ ai đã mở nó, do đó nó cũng giải phóng cảm biến mà một skript bị treo đã để lại trong bể (việc ghi dữ liệu trên cảm biến đó phải hoàn tất trước). Một cảm biến không nằm trong nhóm không có gì để giải phóng: lệnh sẽ hiển thị `<sensor-id>: already released (not in the pool)` và thoát khỏi `0`. Việc giải phóng bị phần nền từ chối hoặc không xác nhận sẽ thoát khỏi `1`.

## `pool-enable-ptp` — bật lại PTP của DAQ-E

```bash
chloros-cli daq pool-enable-ptp --sensor-id daq-e-def330
```

Một thiết bị DAQ-E lưu trữ cài đặt bật/tắt PTP trong bộ nhớ riêng của nó, do đó một thiết bị đã tắt PTP sẽ vẫn ở trạng thái tắt — và các dấu thời gian của nó không được đồng bộ hóa theo PTP — cho đến khi nó được bật lại. `pool-list` sẽ đánh dấu thiết bị đó và hiển thị lệnh này cho thiết bị đó. Chloros không bao giờ tự động bật lại thiết bị, vì việc tắt có thể là chủ ý. Mới trong phiên bản 1.3.0.

## Cài đặt thiết bị DAQ-E

```bash
chloros-cli daq pool-info --sensor-id daq-e-def330
chloros-cli daq pool-set-network --sensor-id daq-e-def330 \
  --static 192.168.1.50 --gateway 192.168.1.1
chloros-cli daq pool-set-network --sensor-id daq-e-def330 --auto
chloros-cli daq pool-set-auto-stream --sensor-id daq-e-def330 --on
chloros-cli daq pool-reboot --sensor-id daq-e-def330
chloros-cli daq pool-update-firmware --sensor-id daq-e-def330
```

`pool-info` hiển thị phiên bản firmware của cảm biến (và liệu bản phát hành Chloros này có phiên bản mới hơn hay không), địa chỉ của nó và cách nó nhận được địa chỉ đó, cài đặt mạng đã lưu (nếu khác với cài đặt đang hoạt động), truyền dữ liệu khi bật nguồn và PTP. `pool-set-network` khởi động lại cảm biến để áp dụng thay đổi và kết nối lại với các cài đặt ban đầu; `--no-reboot` chỉ lưu lại cài đặt. Địa chỉ tĩnh phải nằm trong mạng của máy tính này, nếu không Chloros sẽ từ chối; `--force` bỏ qua quy tắc đó, chỉ áp dụng cho máy tính sẽ được gán lại địa chỉ hoặc được định tuyến đến địa chỉ đó. Việc khởi động lại sẽ bị từ chối trong khi cảm biến đang ghi dữ liệu. `pool-update-firmware` là lệnh **Update to \<version\>**</version\> của giao diện người dùng (GUI) <version\>dành cho dòng lệnh, đồng thời là cách để cập nhật DAQ-E trên hệ điều hành Linux (không có GUI). Tính năng mới trong phiên bản 1.3.0.

***

## Tóm tắt lệnh

| Lệnh | Mục đích |
| --- | --- |
| `daq pool-connect [--port P \| --ble \| --mac M \| --eth \| --eth-host H] [-t MS] [-f N] [--no-ae] [--no-stream] [--cap-id CAP]` | Mở một cảm biến trong nhóm cảm biến nền. |
| `daq pool-discover [--only usb,ble,eth] [--timeout SEC] [--json]` | Quét tìm cảm biến mà không kết nối (tìm địa chỉ MAC BLE của DAQ-M). |
| `daq pool-list` | Hiển thị tất cả cảm biến trong nhóm cùng với `sensor_id` của chúng. |
| `daq pool-latest --sensor-id ID [--recent N] [--json]` | N khung phổ đã hiệu chuẩn gần đây nhất. |
| `daq pool-stream --sensor-id ID [--start \| --stop]` | Tiếp tục / tạm dừng truyền dữ liệu. |
| `daq pool-record --sensor-id ID [-d SEC] [-o DIR] [--device-name NAME] [--confirm-after SEC] [--heartbeat SEC] [--stop]` | Bắt đầu / dừng ghi dữ liệu `.daq` (phía máy chủ). |
| `daq pool-set-cap --sensor-id ID --cap-id CAP` | Đổi cấu hình hiệu chỉnh giới hạn trong khi chạy. |
| `daq pool-disconnect --sensor-id ID [--all]` | Giải phóng một cảm biến hoặc tất cả các cảm biến. |
| `daq pool-enable-ptp --sensor-id ID` | Bật lại PTP cho một thiết bị DAQ-E đang tắt PTP. |
| `daq pool-info --sensor-id ID [--json]` | Phần mềm hệ thống, địa chỉ, cài đặt mạng đã lưu, truyền dữ liệu khi khởi động và PTP của thiết bị DAQ-E. |
| `daq pool-set-network --sensor-id ID (--auto \| --static IP [--mask M] [--gateway G]) [--no-reboot] [--force]` | Sử dụng DHCP hoặc địa chỉ tĩnh; khởi động lại và kết nối lại cảm biến. |
| `daq pool-set-auto-stream --sensor-id ID (--on \| --off)` | Truyền dữ liệu ngay khi bật nguồn. |
| `daq pool-reboot --sensor-id ID` | Khởi động lại DAQ-E và kết nối lại. |
| `daq pool-update-firmware --sensor-id ID` | Cập nhật firmware DAQ-E của phiên bản này lên cảm biến cũ hơn. |

***

## Khắc phục sự cố khi kết nối DAQ-E lần đầu

1. DAQ-E không có đèn LED trạng thái — hãy xác nhận nguồn điện qua đèn báo PoE/kết nối trên cổng chuyển mạch hoặc cổng bộ tiêm, và đợi vài giây sau khi bật nguồn để thiết bị khởi động và kết nối với mạng.
2. Máy chủ phải nằm trong **cùng một miền phát sóng** với cảm biến — mDNS không đi qua bộ định tuyến.
3. Tường lửa phải cho phép mDNS (UDP 5353), dữ liệu DAQ-E (UDP 5002) và PTP (UDP 319/320) đi qua. Trình cài đặt Windows sẽ thêm quy tắc cho việc này, còn trình cài đặt Linux `.deb` sẽ mở các cổng này trong một `ufw` / `firewalld` đang hoạt động tại thời điểm cài đặt; tường lửa của bên thứ ba, hoặc tường lửa được kích hoạt sau đó, cần phải cho phép các cổng này.
4. Vẫn không có phản hồi từ `--eth`? Hãy sử dụng `--eth-host` kèm theo tên máy chủ (`daq-e-<id>.local`) hoặc địa chỉ IP của thiết bị, điều này sẽ bỏ qua quá trình phát hiện.

***
{% hint style="info" %}
**Mẹo dành cho trợ lý AI.** Mỗi trang của tài liệu hướng dẫn này được cung cấp dưới dạng Markdown thô — hãy thêm `.md` vào phần slug URL viết thường của trang (trang này: `https://mapir.gitbook.io/chloros/daq/cli-quick-start.md`); chỉ mục có thể đọc được bởi máy là `https://mapir.gitbook.io/chloros/llms.txt`. Để xem tài liệu đầy đủ về các tùy chọn của `chloros-cli daq` và mọi nhóm lệnh khác, hãy truy cập [Tham chiếu CLI](../reference/cli-reference.md) (`https://mapir.gitbook.io/chloros/reference/cli-reference.md`); đường dẫn Python là `chloros_sdk.connect_daq_sensor()` trong [Tham chiếu SDK](../reference/sdk-reference.md).
{% endhint %}
</version\>
