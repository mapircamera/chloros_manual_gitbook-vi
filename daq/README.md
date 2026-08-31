# Cảm biến ánh sáng DAQ

> **Bạn đang tìm kiếm thông tin về phần cứng?**Các thông tin về chính các cảm biến — bao gồm các mẫu sản phẩm, cách lắp đặt, nắp bảo vệ, cổng kết nối, nguồn điện và ứng dụng SCANNER — được trình bày chi tiết trong**[Hướng dẫn sử dụng DAQ](https://mapir.gitbook.io/daq)**. Chương này hướng dẫn cách sử dụng chúng từ Chloros. Các cảm biến ánh sáng**DAQ** của

MAPIRđo ánh sáng môi trường dưới dạng phổ được hiệu chuẩn phóng xạ. Trong Chloros, chúng đảm nhận hai vai trò:

* **Một thiết bị quang phổ độc lập** — biểu đồ quang phổ thời gian thực, dữ liệu màu và bản ghi `.daq`, tất cả đều từ [tab Cảm biến ánh sáng](gui.md), [CLI](cli-quick-start.md) hoặc Python SDK.
* **Nguồn bức xạ chiếu xuống để tính phản xạ** — trong quá trình xử lý, Chloros sẽ nội suy các giá trị đo `.daq` của bạn theo dấu thời gian phơi sáng của từng ảnh chụp và sử dụng cường độ ánh sáng chiếu xuống đã đo để chuyển đổi độ rạng rỡ của máy ảnh thành độ phản xạ (`--reflectance-source daq`); không cần bảng hiệu chuẩn trong cảnh đối với các dải đã hiệu chuẩn.



<!-- SCREENSHOT-NEEDED: product photo of the DAQ-U, DAQ-M, and DAQ-E units side by side, each with its Sunshine cosine-corrector cap fitted (request from hardware team — no repo asset exists) -->***

## Ba mô hình, một định dạng dữ liệu

| Mô hình | Giao thức truyền tải | Phát hiện |
| --- | --- | --- |
| **DAQ-U** | USB (nối tiếp) | quét cổng nối tiếp |
| **DAQ-M** | Bluetooth Low Energy | quét BLE theo tên |
| **DAQ-E** | Ethernet (IPv4, cấp nguồn qua PoE) | mDNS `_daq-e._tcp` (tên máy chủ `daq-e-<id>.local`) |

Cả ba thiết bị đều sử dụng cùng một giao thức truyền dẫn và cung cấp dữ liệu giống hệt nhau:

* Một **bảng phổ 135 điểm từ 340 đến 1010 nm với bước nhảy 5 nm**, kèm theo các giá trị ba thành phần màu CIE XYZ, trong mỗi khung dữ liệu.
* **Bức xạ quang phổ được hiệu chuẩn bằng phương pháp đo quang trong W/m²/nm** — gói hiệu chuẩn xuất xưởng của mỗi thiết bị (cùng với cấu hình hiệu chỉnh nắp hoạt động) được áp dụng trước khi dữ liệu đến tay bạn.
* Cùng một **định dạng ghi dữ liệu `.daq`** (tệp SQLite). Quy trình xử lý sau đó hoàn toàn giống nhau bất kể phương thức truyền tải nào tạo ra tệp đó.

Các giao thức truyền tải (USB serial, BLE, mDNS/zeroconf) được tích hợp sẵn trong phần mềm nền tảng Chloros — không cần cài đặt thêm bất kỳ phần mềm nào để tương tác với ba mẫu máy này thông qua giao diện người dùng (GUI) hoặc các lệnh `pool-*` của CLI.

***

## Dải đo đã hiệu chuẩn: 340–1010 nm (dữ liệu báo cáo), ~374–974 nm (dải hiệu chuẩn)

Cảm biến báo cáo toàn bộ lưới 340–1010 nm, nhưng dải độ lợi bức xạ có thể truy xuất về NIST chỉ bao phủ khoảng **374–974 nm**. Chloros từ chối chia tỷ lệ phản xạ tuyệt đối cho bất kỳ dải quang phổ nào của camera có ít hơn một nửa trọng số quang phổ nằm trong khoảng hiệu chuẩn đó; dải bị bỏ qua sẽ được báo cáo kèm lý do bỏ qua là `dls-uncalibrated-band-<nm>`.

Trong số các mã sản phẩm (SKU) bộ lọc LATTICE đang được phân phối, chỉ có **F988** bị ảnh hưởng:

Độ phản xạ của F988 được hiệu chuẩn bằng bảng đo độ phản xạ tại hiện trường: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros sẽ áp dụng dữ liệu chụp bảng đo gần nhất của bạn và duy trì giá trị đó giữa các lần đo bảng.

Nếu dữ liệu ghi nhận từ F988 được xử lý khi chỉ có dữ liệu DAQ sẵn có, Chloros sẽ từ chối giá trị phản xạ dựa trên DAQ cho dải sóng đó với lý do bỏ qua là `dls-uncalibrated-band-988` — [quy trình làm việc với bảng đo phản xạ](../calibration-targets.md) là quy trình được hỗ trợ cho F988.

***

## Mã định danh cảm biến (Sensor IDs)

Mỗi thiết bị DAQ báo cáo một mã định danh cảm biến ổn định. Cấu trúc của mã này khác nhau tùy theo mẫu:

| Mẫu | Cấu trúc mã | Ví dụ |
| --- | --- | --- |
| DAQ-U | 5 octet có gạch nối | `CB-7C-A8-2E-5F` |
| DAQ-M | 5 octet có dấu gạch ngang | `CB-74-02-30-6B` |
| DAQ-E | `daq-e-<6 hex digits>` | `daq-e-def330` |

ID cảm biến là:

* được ghi vào mỗi tệp `.daq` mà nó ghi lại,
* khóa mà Chloros sử dụng để lấy gói hiệu chuẩn nhà máy của thiết bị đó,
* giá trị mà bạn truyền cho `--sensor-id` trong các lệnh `CLI` và ``pool-*``, và
* đối với DAQ-E, còn có tên máy chủ mDNS của nó (`daq-e-def330.local`) — giá trị mà `--eth-host` chấp nhận.

***

## Hiệu chuẩn tại nhà máy và đám mây

Mỗi thiết bị DAQ đều được hiệu chuẩn riêng tại nhà máy bằng chuỗi đo quang học có thể truy xuất nguồn gốc theo tiêu chuẩn NIST, và Chloros tải gói hiệu chuẩn của từng thiết bị dựa trên ID cảm biến của nó. Báo cáo hiệu chuẩn cho từng thiết bị (PDF) có thể tải xuống từ phần cài đặt của cảm biến trong [tab Cảm biến ánh sáng](gui.md).

{% hint style="warning" %}
**DAQ-U và DAQ-M yêu cầu kết nối đám mây để hiệu chuẩn.**Cả hai mẫu này đều không lưu trữ bất kỳ dữ liệu nào trên thiết bị: gói hiệu chuẩn xuất xưởng của chúng được lưu trữ trên đám mây của MAPIR và được truy xuất dựa trên ID cảm biến (sau đó được lưu vào bộ nhớ đệm cục bộ). Chloros cần kết nối internet để cung cấp dữ liệu W/m²/nm đã được hiệu chuẩn từ DAQ-U hoặc DAQ-M.**DAQ-E là trường hợp ngoại lệ** — thiết bị này lưu trữ dữ liệu hiệu chuẩn ngay trên thiết bị.
{% endhint %}


<!-- PRE-PUBLISH-CHECK: LAUNCH item 3 (DAQ-M end-to-end connect smoke) was still unverified as of 2026-08-16 — re-confirm the DAQ-M cloud-calibration flow on the release build before publishing this page. -->***

## Vị trí lưu trữ bản ghi

| Bề mặt | Vị trí lưu trữ mặc định của `.daq` |
| --- | --- |
| Giao diện người dùng (GUI) — tab Cảm biến ánh sáng | `<project folder>/light_sensor/` (các bản ghi đã hoàn thành được tự động thêm vào dự án) |
| CLI — `daq pool-record` | `~/Documents/DAQ Live View/` trên máy chủ chạy phần nền |

Mỗi tên tệp `.daq` đều bao gồm ID cảm biến và dấu thời gian.

***

## Trong chương này

* [**Tab DAQ trong Chloros**](gui.md) — hướng dẫn chi tiết về giao diện người dùng (GUI): kết nối từng mô hình, cài đặt cho từng cảm biến, biểu đồ phổ, dữ liệu màu trực tiếp, độ phản xạ hai cảm biến và ghi dữ liệu.
* [**Hướng dẫn khởi động nhanh CLI (pool-\*)**](cli-quick-start.md) — điều khiển cảm biến DAQ từ `chloros-cli daq pool-*`, đường dẫn dòng lệnh được hỗ trợ.
* [**Hồ sơ giới hạn &amp; Phạm vi hiệu chuẩn**](caps-and-range.md) — các giới hạn tồn tại cho từng mô hình, cách khai báo chúng và phạm vi quang phổ hiệu chuẩn chi tiết.
* [**Ghi dữ liệu &amp; định dạng .daq**](recording.md) — định dạng SQLite của `.daq` và quy trình ghi dữ liệu.
* [**Mạng DAQ-E &amp; Đồng bộ hóa thời gian**](ethernet-ptp.md) — các chế độ truyền tải DAQ-E và đồng bộ hóa thời gian PTP.
* [**Quy trình làm việc về độ phản xạ**](reflectance.md) — sử dụng dữ liệu hướng xuống từ DAQ để tính toán độ phản xạ.
* Để xem tài liệu đầy đủ về cấp độ cờ, hãy tham khảo [Tài liệu tham khảo CLI](../reference/cli-reference.md) (phần `chloros-cli daq`) và [Tài liệu tham khảo SDK](../reference/sdk-reference.md) (`chloros_sdk.connect_daq_sensor()`), cả hai đều được viết để các trợ lý AI có thể sử dụng trực tiếp.
