# Cảm biến ánh sáng DAQ

> **Bạn đang tìm kiếm thông tin về phần cứng?**Các thông tin về chính các cảm biến — bao gồm các mẫu, cách lắp đặt, nắp bảo vệ, cổng kết nối, nguồn điện và ứng dụng SCANNER — được ghi chi tiết trong**[Hướng dẫn sử dụng DAQ](https://mapir.gitbook.io/daq)**. Chương này hướng dẫn cách sử dụng chúng trong Chloros. Các cảm biến ánh sáng**DAQ** của

MAPIR đo ánh sáng môi trường dưới dạng phổ được hiệu chuẩn theo phương pháp quang phổ. Trong Chloros, chúng đảm nhận hai vai trò:

* **Một thiết bị quang phổ độc lập** — biểu đồ quang phổ thời gian thực, dữ liệu màu và bản ghi `.daq`, tất cả đều từ [tab Cảm biến ánh sáng](gui.md), [Giao diện dòng lệnh CLI](cli-quick-start.md) hoặc Python SDK.
* **Nguồn bức xạ chiếu xuống cho độ phản xạ** — trong quá trình xử lý, Chloros sẽ nội suy các giá trị đo `.daq` của bạn theo dấu thời gian phơi sáng của từng lần chụp và sử dụng cường độ ánh sáng chiếu xuống đã đo để chuyển đổi độ rạng rỡ của camera thành độ phản xạ (`--reflectance-source daq`), không cần bảng hiệu chuẩn trong cảnh đối với các dải đã hiệu chuẩn.

<!-- SCREENSHOT-NEEDED: product photo of the DAQ-U, DAQ-M, and DAQ-E units side by side, each with its Sunshine cosine-corrector cap fitted (request from hardware team — no repo asset exists) -->

***

## Ba mô hình, một định dạng dữ liệu

| Mô hình | Giao thức truyền tải | Khám phá |
| --- | --- | --- |
| **DAQ-U** | USB (nối tiếp) | quét cổng nối tiếp |
| **DAQ-M** | Bluetooth Low Energy | quét BLE theo tên |
| **DAQ-E** | Ethernet (IPv4, cấp nguồn qua PoE) | mDNS `_daq-e._tcp` (tên máy chủ `daq-e-<id>.local`) |**DAQ-E-S (mới trong phiên bản 1.3.0).** Một thiết bị DAQ-E được trang bị bộ khuếch tán cố định và cảm biến nghiêng. Chloros liệt kê thiết bị này với tên `DAQ-E-S` và vận hành nó hoàn toàn giống như một thiết bị DAQ-E, ngoại trừ việc nó không yêu cầu hiệu chỉnh nắp và mỗi giá trị đo đều chứa thông tin về góc nghiêng, góc lăn và góc dọc của thiết bị. Xem [DAQ-E-S: bộ khuếch tán cố định và cảm biến nghiêng](caps-and-range.md#daq-e-s-fixed-diffuser-and-tilt).

Cả ba thiết bị đều sử dụng cùng một giao thức truyền dữ liệu và cung cấp dữ liệu giống hệt nhau:

* Một **phổ 135 điểm từ 340 đến 1010 nm với bước 5 nm**, kèm theo các giá trị ba kích thích CIE XYZ, trong mỗi khung dữ liệu.
* **Bức xạ quang phổ được hiệu chuẩn theo phương pháp đo quang trong đơn vị W/m²/nm** — gói hiệu chuẩn tại nhà máy của từng thiết bị (cùng với hồ sơ hiệu chỉnh nắp hoạt động) được áp dụng trước khi dữ liệu đến tay bạn.
* Cùng một **định dạng ghi `.daq`** (một tệp SQLite). Quá trình xử lý sau đó là giống hệt nhau bất kể phương thức truyền tải nào đã tạo ra tệp đó.

Các giao thức truyền tải (USB serial, BLE, mDNS/zeroconf) được tích hợp sẵn trong phần mềm nền Chloros — không cần cài đặt thêm bất kỳ phần mềm nào để giao tiếp với bất kỳ mô hình nào trong ba mô hình này thông qua giao diện người dùng (GUI) hoặc các lệnh `pool-*` trên giao diện dòng lệnh (CLI).

***

## Dải đo đã hiệu chuẩn: 340–1010 nm (dữ liệu báo cáo), ~374–974 nm (dải hiệu chuẩn)

Cảm biến báo cáo toàn bộ lưới 340–1010 nm, nhưng dải độ lợi bức xạ có thể truy xuất theo tiêu chuẩn NIST chỉ bao phủ khoảng **374–974 nm**. Chloros sẽ từ chối chia tỷ lệ phản xạ tuyệt đối đối với bất kỳ dải phổ nào của camera có ít hơn một nửa trọng số phổ nằm trong khoảng đã hiệu chuẩn đó; dải phổ bị bỏ qua sẽ được báo cáo kèm lý do bỏ qua là `dls-uncalibrated-band-<nm>`.

Trong số các mã sản phẩm (SKU) bộ lọc LATTICE đang được phân phối, chỉ có **F988** bị ảnh hưởng:

Độ phản xạ của F988 được hiệu chuẩn bằng bảng đo độ phản xạ tại hiện trường: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros sẽ áp dụng dữ liệu chụp bảng đo gần nhất của bạn và duy trì giá trị đó giữa các lần đo bảng.

Nếu dữ liệu thu thập từ F988 được xử lý khi chỉ có dữ liệu DAQ sẵn có, Chloros sẽ từ chối giá trị phản xạ dựa trên DAQ cho dải đó với lý do bỏ qua là `dls-uncalibrated-band-988` — [quy trình làm việc với bảng phản xạ](../calibration-targets.md) là phương pháp được hỗ trợ cho F988.

***

## Mã định danh cảm biến

Mỗi thiết bị DAQ báo cáo một ID cảm biến ổn định. Cấu trúc của ID này khác nhau tùy theo mô hình:

| Mô hình | Cấu trúc ID | Ví dụ |
| --- | --- | --- |
| DAQ-U | 5 octet có dấu gạch ngang | `CB-7C-A8-2E-5F` |
| DAQ-M | 5 octet có dấu gạch ngang | `CB-74-02-30-6B` |
| DAQ-E | `daq-e-<6 hex digits>` | `daq-e-def330` |

ID cảm biến là:

* được ghi vào mỗi tệp `.daq` mà nó ghi lại,
* khóa mà Chloros sử dụng để lấy gói hiệu chuẩn nhà máy của thiết bị đó,
* giá trị mà bạn truyền cho `--sensor-id` trong các lệnh CLI, và
* đối với DAQ-E, còn là tên máy chủ mDNS của nó (`daq-e-def330.local`) — giá trị mà `--eth-host` chấp nhận.

***

## Hiệu chuẩn nhà máy và đám mây

Mỗi thiết bị DAQ đều được hiệu chuẩn riêng tại nhà máy bằng chuỗi đo quang có thể truy xuất nguồn gốc theo tiêu chuẩn NIST, và Chloros sẽ tải gói hiệu chuẩn của từng thiết bị dựa trên ID cảm biến của thiết bị đó. Báo cáo hiệu chuẩn cho từng thiết bị (PDF) có thể tải xuống từ phần cài đặt cảm biến trong [tab Cảm biến ánh sáng](gui.md).

{% hint style="warning" %}
**DAQ-U và DAQ-M yêu cầu kết nối đám mây để hiệu chuẩn.**Cả hai mẫu này đều không lưu trữ dữ liệu nào trên thiết bị: gói hiệu chuẩn tại nhà máy của chúng được lưu trữ trên đám mây củMAPIR và được truy xuất theo ID cảm biến (sau đó được lưu vào bộ nhớ đệm cục bộ). Chloros cần kết nối internet để truyền dữ liệu W/m²/nm đã được hiệu chuẩn từ DAQ-U hoặc DAQ-M.**DAQ-E là trường hợp ngoại lệ** — thiết bị này lưu trữ dữ liệu hiệu chuẩn ngay trên thiết bị.
{% endhint %}
<!-- PRE-PUBLISH-CHECK: LAUNCH item 3 (DAQ-M end-to-end connect smoke) was still unverified as of 2026-08-16 — re-confirm the DAQ-M cloud-calibration flow on the release build before publishing this page. -->

***

## Nơi lưu trữ các bản ghi

| Bề mặt | Địa điểm đích mặc định của `.daq` |
| --- | --- |
| Giao diện người dùng (GUI) — tab Cảm biến ánh sáng | `<project folder>/light_sensor/` (các bản ghi đã hoàn thành được tự động thêm vào dự án) |
| Giao diện dòng lệnh (CLI) — `daq pool-record` | `~/Documents/DAQ Live View/` trên máy chạy phần nền (một đường dẫn tương đối `-o` sẽ được giải quyết dựa trên thư mục bạn đã chạy lệnh khi backend nằm trên cùng một máy tính) |

Mỗi tên tệp `.daq` đều bao gồm ID cảm biến và dấu thời gian.

***

## Trong chương này

* [**Tab DAQ trong Chloros**](gui.md) — hướng dẫn chi tiết về giao diện người dùng (GUI): kết nối từng mô hình, cài đặt cho từng cảm biến, biểu đồ phổ, dữ liệu màu trực tiếp, độ phản xạ của cảm biến kép và ghi dữ liệu.
* [**Hướng dẫn nhanh CLI (pool-\*)**](cli-quick-start.md) — điều khiển cảm biến DAQ từ `chloros-cli daq pool-*`, đường dẫn dòng lệnh được hỗ trợ.
* [**Hồ sơ giới hạn &amp; Phạm vi hiệu chuẩn**](caps-and-range.md) — các giới hạn có sẵn cho từng mẫu, cách khai báo chúng và phạm vi quang phổ hiệu chuẩn chi tiết.
* [**Ghi dữ liệu &amp; định dạng .daq**](recording.md) — định dạng SQLite của `.daq` và quy trình ghi dữ liệu.
* [**Kết nối mạng DAQ-E &amp; Đồng bộ hóa thời gian**](ethernet-ptp.md) — các chế độ truyền tải DAQ-E và đồng bộ hóa thời gian PTP.
* [**Quy trình làm việc về độ phản xạ**](reflectance.md) — sử dụng dữ liệu chiếu xuống từ DAQ để tính toán độ phản xạ.
* Để xem tài liệu đầy đủ về cấp độ cờ, hãy tham khảo [Tài liệu tham khảo CLI](../reference/cli-reference.md) (phần `chloros-cli daq`) và [Tài liệu tham khảo SDK](../reference/sdk-reference.md) (`chloros_sdk.connect_daq_sensor()`), cả hai đều được viết để các trợ lý AI có thể sử dụng trực tiếp.
