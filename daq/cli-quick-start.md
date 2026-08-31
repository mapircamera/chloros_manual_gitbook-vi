# Hướng dẫn khởi động nhanh CLI (pool-*)

Các thiết bị `chloros-cli` được cung cấp sẵn điều khiển các cảm biến DAQ thông qua bộ lệnh **`daq pool-*`** — các máy khách nhẹ HTTP điều khiển cảm biến thông qua nhóm cảm biến cố định của hệ thống nền Chloros. Backend quản lý kênh truyền tải, do đó giao diện người dùng (GUI), các tệp CLI và các tập lệnh SDK đều chia sẻ chung một handle hoạt động thay vì cạnh tranh để sử dụng cổng. Mọi thứ khách hàng cần đều có thể truy cập thông qua `pool-*`: kết nối, truyền dữ liệu, ghi tệp `.daq` đã hiệu chuẩn và thay đổi cấu hình cap.

`pool-*` cũng là giao diện DAQ **duy nhất** trong các bản phát hành chính thức. `chloros-cli daq --help` liệt kê các lệnh con của `pool-*`, và việc gọi một lệnh con DAQ trực tiếp với phần cứng trên bản phát hành chính thức sẽ thoát ra với một lỗi rõ ràng nêu tên gói thiếu và hướng dẫn bạn quay lại `pool-*` — không có lỗi nào xảy ra âm thầm. (Các lệnh trực tiếp với phần cứng chỉ chạy từ bản sao mã nguồn MAPIR; `pip install chloros-sdk` cũng không cung cấp các lệnh này.)

***

## Yêu cầu tiên quyết

* **Backend Chloros phải đang chạy** — các lệnh `pool-*` là các client HTTP, không phải trình điều khiển phần cứng. Trên Windows, khởi động ứng dụng máy tính để bàn Chloros (ứng dụng này sẽ khởi chạy backend). Trên Linux /Jetson không có giao diện người dùng, hãy kích hoạt dịch vụ: `sudo systemctl enable --now chloros-backend.service`.
* **Tài khoản đăng nhập Chloros+ (gói trả phí)**: hãy chạy lệnh `chloros-cli login` trước. Việc thực thi diễn ra ở phía máy chủ — nếu không đăng nhập, các lệnh sẽ thất bại với mã lỗi `401 AUTH_REQUIRED`; trên gói miễn phí (Iron), các lệnh sẽ thất bại với mã lỗi `403 PLAN_UPGRADE_REQUIRED`.
* Các lệnh nhắm đến `http://127.0.0.1:5000` theo mặc định; dòng lệnh `daq pool-*` sẽ tuân theo biến môi trường `CHLOROS_BACKEND_URL` nếu hệ thống backend của bạn chạy ở nơi khác.

***

## Một phiên làm việc kéo dài năm phút

```bash
# 1. Connect a sensor into the backend pool (pick the line matching your model)
chloros-cli daq pool-connect                                  # smart-detect any DAQ
chloros-cli daq pool-connect --port COM3                      # DAQ-U on a specific COM port
chloros-cli daq pool-connect --mac AA:BB:CC:DD:EE:FF          # DAQ-M by BLE MAC
chloros-cli daq pool-connect --eth-host daq-e-def330.local    # DAQ-E by hostname (reliable)

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
| `daq pool-connect --ble` | DAQ-M qua BLE, tự động quét MAC. |
| `daq pool-connect --mac MAC` | DAQ-M tại địa chỉ MAC BLE đã biết (ngụ ý `--ble`). |
| `daq pool-connect --eth-host HOST` | DAQ-E với tên máy chủ hoặc địa chỉ IP đã biết — **lộ trình đáng tin cậy**. |
| `daq pool-connect --eth` | DAQ-E với tính năng tự động phát hiện (mDNS, có phương án dự phòng ARP). Xem lưu ý dưới đây. |

Các cờ điều chỉnh, tất cả đều tùy chọn:

| Cờ | Ý nghĩa |
| --- | --- |
| `--integration-time MS` / `-t MS` | Thời gian tích hợp thủ công tính bằng mili giây. |
| `--frame-avg N` / `-f N` | Số khung hình được tính trung bình cho mỗi phổ được báo cáo. |
| `--no-ae` | Tắt tính năng phơi sáng tự động (AE được bật theo mặc định). |
| `--no-stream` | Kết nối mà không khởi động luồng (tiếp tục sau với `pool-stream --start`). |
| `--cap-id CAP` | Hồ sơ hiệu chỉnh giới hạn; mặc định của backend là `sunshine_cosine`. Xem [`pool-set-cap`](#pool-set-cap-declare-the-fitted-cap). |

{% hint style="warning" %}
**Lưu ý về tính năng tự động phát hiện `--eth`.** Trên một máy chủ có nhiều giao diện mạng (hơn một giao diện mạng hoạt động), lần *đầu tiên* `pool-connect --eth` sau khi khởi động có thể không tìm thấy gì ngay cả khi cảm biến vẫn hoạt động bình thường — quá trình quét phát hiện có thể bỏ qua giao diện của cảm biến khi bộ nhớ đệm ARP chưa được làm nóng. Nếu `--eth` không tìm thấy gì, hãy thử lại hoặc bỏ qua hoàn toàn quá trình phát hiện bằng cách sử dụng `--eth-host <ip-or-hostname>`, đây là phương án đáng tin cậy trên các máy chủ đa giao diện mạng. Tên máy chủ của DAQ-E là `daq-e-<id>.local` (ví dụ: `daq-e-def330.local`); địa chỉ IP thuần túy của nó cũng hoạt động được.
{% endhint %}

## `pool-list` — xem những gì đang được kết nối

Hiển thị mọi cảm biến trong nhóm máy chủ phía sau, bao gồm cả `sensor_id` mà mọi lệnh khác đều cần:

| Mẫu | Định dạng `sensor_id` | Ví dụ |
| --- | --- | --- |
| DAQ-U / DAQ-M | 5 octet có dấu gạch ngang | `CB-7C-A8-2E-5F` |
| DAQ-E | `daq-e-<6 hex digits>` | `daq-e-def330` |

## `pool-latest` — đọc khung phổ

```bash
chloros-cli daq pool-latest --sensor-id daq-e-def330 --recent 10 --json
```

Trả về khung mới nhất, hoặc các khung mới nhất `--recent N`; `--json` phát ra đầu ra có thể đọc được bởi máy tính để lập trình kịch bản. Các khung dữ liệu là cường độ bức xạ quang phổ (W/m²/nm) đã được hiệu chuẩn phóng xạ trên lưới 135 điểm, dải 340–1010 nm, với cấu hình nắp cảm biến đã được áp dụng sẵn. Để có các giá trị bức xạ định lượng, hãy tính trung bình ít nhất 15 giây dữ liệu — đây là đặc tính của thiết bị, không phải lỗi.

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
| `--duration SEC` / `-d SEC` | `0` | Thời lượng ghi (tính bằng giây); `0` có nghĩa là chạy cho đến khi bạn thực thi lệnh `--stop`. |
| `--output DIR` / `-o DIR` | `~/Documents/DAQ Live View/` | Thư mục đầu ra, được xác định **trên máy chạy backend**. |
| `--device-name NAME` | — | Nhãn được lưu kèm với bản ghi. |
| `--stop` | — | Dừng bản ghi đang diễn ra. |

{% hint style="info" %}
Quá trình ghi diễn ra ở backend, do đó tệp `.daq` sẽ được lưu vào hệ thống tệp của **máy backend** — theo mặc định là ở `~/Documents/DAQ Live View/` tại đó, không nhất thiết phải là nơi bạn chạy lệnh `CLI`. Tên tệp bao gồm ID cảm biến và dấu thời gian.
{% endhint %}

## `pool-set-cap` — khai báo nắp đã lắp

```bash
chloros-cli daq pool-set-cap --sensor-id daq-e-def330 --cap-id sunshine_cosine
```

ID nắp chọn hồ sơ hiệu chỉnh được đo tại nhà máy áp dụng cho mọi phổ, và nó **phải khớp với nắp được lắp vật lý trên cảm biến** — cả cảm biến lẫn phần mềm đều không thể tự phát hiện nắp, và lựa chọn này được ghi vào mọi tệp `.daq`. Giá trị mặc định ở mọi nơi là `sunshine_cosine` (mọi thiết bị DAQ đều được xuất xưởng với nắp hiệu chỉnh cosin Sunshine đã lắp sẵn, có độ suy giảm khoảng 12× theo thiết kế — việc thay đổi nắp mà không khai báo sẽ dẫn đến hiệu chỉnh sai phổ với hệ số sai lệch xấp xỉ như vậy).

| `--cap-id` | Có sẵn trên |
| --- | --- |
| `sunshine_cosine` (mặc định) | DAQ-U, DAQ-M, DAQ-E |
| `fov_15`, `fov_45`, `fov_90` | DAQ-U, DAQ-E |
| `fov_30`, `fov_60` | Chỉ DAQ-U |
| `none` | Chỉ DAQ-E — xem chú thích |

Mã nắp nằm ngoài bộ cài đặt của cảm biến sẽ bị từ chối khi kết nối kèm theo thông báo lỗi rõ ràng. `none` (DAQ-E) có nghĩa là nắp đã bị tháo ra về mặt vật lý — thiết bị vẫn áp dụng cấu hình hình học mặc định của nhà máy cho bộ khuếch tán kính lõm của DAQ-E, do đó đây không phải là chế độ không hoạt động (no-op), và một DAQ-E không có nắp là cấu hình trên bàn thí nghiệm, không phải cấu hình được hỗ trợ trong môi trường thực tế. (Một DAQ-U không có nắp là loại “true-bare” và không cần bất kỳ hồ sơ hiệu chỉnh nào; DAQ-M được sử dụng kèm với nắp Sunshine.)

## `pool-disconnect` — giải phóng cảm biến

```bash
chloros-cli daq pool-disconnect --sensor-id daq-e-def330   # one sensor
chloros-cli daq pool-disconnect --all                      # everything in the pool
```

***

## Tóm tắt lệnh

| Lệnh | Mục đích |
| --- | --- |
| `daq pool-connect [--port P \| --ble \| --mac M \| --eth \| --eth-host H] [-t MS] [-f N] [--no-ae] [--no-stream] [--cap-id CAP]` | Mở một cảm biến trong nhóm cảm biến nền. |
| `daq pool-list` | Hiển thị tất cả các cảm biến trong nhóm cùng với mã `sensor_id` tương ứng. |
| `daq pool-latest --sensor-id ID [--recent N] [--json]` | N khung phổ hiệu chuẩn gần đây nhất. |
| `daq pool-stream --sensor-id ID [--start \| --stop]` | Tiếp tục / tạm dừng truyền dữ liệu. |
| `daq pool-record --sensor-id ID [-d SEC] [-o DIR] [--device-name NAME] [--stop]` | Bắt đầu / dừng ghi dữ liệu `.daq` (phía backend). |
| `daq pool-set-cap --sensor-id ID --cap-id CAP` | Đổi cấu hình hiệu chỉnh mức tối đa trong khi chạy. |
| `daq pool-disconnect --sensor-id ID [--all]` | Giải phóng một cảm biến hoặc tất cả cảm biến. |

***

## Khắc phục sự cố khi kết nối DAQ-E lần đầu

1. DAQ-E không có đèn LED trạng thái — hãy kiểm tra nguồn điện qua đèn báo PoE/kết nối trên cổng bộ chuyển mạch hoặc bộ cấp nguồn, và đợi vài giây sau khi bật nguồn để thiết bị khởi động và kết nối vào mạng.
2. Máy chủ phải nằm trong **cùng miền phát sóng** với cảm biến — mDNS không đi qua các bộ định tuyến.
3. Trên Windows, hãy chấp nhận lời nhắc của tường lửa Defender khi chạy lần đầu (mDNS UDP 5353, dữ liệu DAQ-E UDP 5002, PTP UDP 319/320).
4. Vẫn không nhận được phản hồi từ `--eth`? Hãy sử dụng `--eth-host` với tên máy chủ (`daq-e-<id>.local`) hoặc địa chỉ IP của thiết bị — đây là phương án đáng tin cậy hơn, đặc biệt trên các máy chủ có nhiều địa chỉ IP.

***{% hint style="info" %}**Mẹo dành cho trợ lý AI.** Mỗi trang trong tài liệu hướng dẫn này được cung cấp dưới dạng Markdown thô — hãy thêm `.md` vào phần slug viết thường của trang URL (trang này: `https://mapir.gitbook.io/chloros/daq/cli-quick-start.md`); chỉ mục có thể đọc được bởi máy tính sẽ là `https://mapir.gitbook.io/chloros/llms.txt`. Để xem tài liệu đầy đủ về các tùy chọn của lệnh `chloros-cli daq` và mọi nhóm lệnh khác, hãy truy cập [CLI Reference](../reference/cli-reference.md) (`https://mapir.gitbook.io/chloros/reference/cli-reference.md`); đường dẫn Python là `chloros_sdk.connect_daq_sensor()` trong [SDK Reference](../reference/sdk-reference.md).
{% endhint %}
