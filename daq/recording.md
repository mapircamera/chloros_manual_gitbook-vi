# Ghi dữ liệu và định dạng .daq

Tệp `.daq` là định dạng ghi dữ liệu từ cảm biến ánh sáng củChloros

: một **cơ sở dữ liệu SQLite** chứa các khung hình quang phổ đã được hiệu chuẩn từ một cảm biến DAQ. Ghi lại một tệp trong phiên thu thập dữ liệu và quy trình xử lý độ phản xạ sau đó có thể chia mỗi hình ảnh cho bức xạ xuống được đo tại chính thời điểm đó.

## Nội dung của tệp .daq

| Thuộc tính | Giá trị |
| --- | --- |
| Container | Cơ sở dữ liệu SQLite, một tệp cho mỗi cảm biến trên mỗi lần ghi |
| Tên tệp | Bao gồm **ID cảm biến**và**dấu thời gian**, ví dụ: `daq_data_daq-e-def330_2026_04_13_18h30m00.daq` |
| Phổ cho mỗi khung hình | 135 điểm, dải 340–1010 nm với bước nhảy 5 nm, kèm theo hệ tọa độ ba kích thích CIE XYZ |
| Đơn vị | Bức xạ phổ đã hiệu chuẩn, **W/m²/nm** (gói hiệu chuẩn nhà máy + hồ sơ nắp đã được áp dụng) |
| Dữ liệu meta được ghi sẵn | ID cảm biến (mã khóa để truy xuất bộ hiệu chuẩn nhà máy của đơn vị đó) và hồ sơ nắp hiện đang áp dụng — xem [Hồ sơ nắp &amp; Phạm vi hiệu chuẩn](caps-and-range.md) |

Định dạng này giống nhau trên các thiết bị DAQ-U, DAQ-M và DAQ-E, do đó quá trình xử lý tiếp theo không quan tâm đến thiết bị truyền tải nào đã ghi lại dữ liệu.

Việc ghi dữ liệu đã hiệu chuẩn yêu cầu gói hiệu chuẩn nhà máy của cảm biến. Đối với DAQ-U và DAQ-M, hệ thống nền sẽ truy xuất gói này từ đám mây củaMAPIR

dựa trên ID cảm biến (việc ghi dữ liệu sẽ bị từ chối nếu không thể thực hiện); các thiết bị DAQ-E được miễn trừ vì chúng lưu trữ dữ liệu hiệu chuẩn trực tiếp trên thiết bị.

## Ghi dữ liệu từ giao diện người dùng (GUI)

Việc ghi dữ liệu trong GUI yêu cầu phải có **dự án đang mở** (nếu không, các nút Ghi sẽ bị vô hiệu hóa):

* **Ghi tất cả / Dừng tất cả** — ở đầu thanh bên Cảm biến ánh sáng; bắt đầu hoặc dừng ghi dữ liệu `.daq` trên tất cả các cảm biến được kết nối cùng một lúc.
* **Ghi / Dừng ghi** — cho từng cảm biến, trong cửa sổ cài đặt hình bánh răng. Một chỉ báo &quot;REC&quot; màu đỏ sẽ hiển thị trong các hàng thông tin trực tiếp của cảm biến trong khi ghi.

Các tệp được ghi vào `<project>/light_sensor/`, và khi quá trình ghi dừng lại — dù là qua nút Dừng, Dừng tất cả hay ngắt kết nối cảm biến đang ghi — tệp `.daq` hoàn thành sẽ **tự động được thêm vào dự án đang mở**. Tệp này xuất hiện trong danh sách tệp của dự án mà không cần bước thêm thủ công, và đã sẵn sàng cho quá trình xử lý phản xạ.

<!-- SCREENSHOT-NEEDED: Light Sensors tab with one DAQ sensor connected and recording: sidebar showing the red "Stop All" state of the Record All button, the sensor row, and the settings modal open with the red "REC" indicator visible in the live info rows. -->

<!-- SCREENSHOT-NEEDED: File Browser / project file list immediately after stopping a DAQ recording, showing the .daq file auto-added to the open project alongside imagery. -->



## Ghi dữ liệu từCLI



CLI

ghi dữ liệu thông qua nhóm cảm biến của backend (backend phải đang chạy — các lệnh này là các client nhẹ củaHTTP

):

```bash
# Connect the sensor into the backend pool
chloros-cli daq pool-connect --eth-host daq-e-def330.local

# Record for 150 seconds, with a human-friendly device label
chloros-cli daq pool-record --sensor-id daq-e-def330 --duration 150 \
    -o ./out --device-name "rooftop-A"

# Or run open-ended and stop explicitly
chloros-cli daq pool-record --sensor-id daq-e-def330            # --duration defaults to 0 = run until --stop
chloros-cli daq pool-record --sensor-id daq-e-def330 --stop
```

Lấy giá trị `--sensor-id` từ `chloros-cli daq pool-list`. Hai giá trị mặc định cần lưu ý:

| Tùy chọn | Mặc định |
| --- | --- |
| `--duration` | `0` — ghi lại cho đến khi đạt `pool-record --stop` |
| `--output` / `-o` | `~/Documents/DAQ Live View/` trên hệ thống tệp của **backend**, không phải củaCLI

|

Sự khác biệt về thư mục đầu ra có ý nghĩa khiCLI

nhắm đến một backend trên một máy khác: tệp sẽ được lưu tại nơi backend đang chạy.

## Ghi lại từPython

`DAQSensorSession` (được trả về bởi `chloros_sdk.connect_daq_sensor()`) hiển thị cùng một bản ghi được gộp chung: `record_start(output_dir=None, device_name=None)` trả về đường dẫn tệp, `record_stop()` trả về `{path, rows}`. Xem [Tài liệu tham khảoSDK

](../reference/sdk-reference.md) để biết thông tin đầy đủ về phiênAPI

. Các lớp phần cứng trực tiếp củaSDK

(chỉ dành cho cài đặt trên máy tính để bàn) ghi các bản ghi vào `~/Documents/DAQ/` theo mặc định; đối với các bản phát hành chính thức, đường dẫn chung ở trên là đường dẫn được hỗ trợ.

## Sử dụng tệp .daq trong quá trình xử lý

Để tính toán hệ số phản xạ từ hình ảnh,Chloros

cần có thông lượng bức xạ hướng xuống khớp với từng lần phơi sáng:

* **Giữ tệp `.daq` cùng với hình ảnh.**Trong quá trình xử lý, quy trình xử lý sẽ tự động xác định**cường độ bức xạ hướng xuống khớp với dấu thời gian** từ tệp `.daq` đã ghi lại (bất kỳ mô hình DAQ nào) — hoặc từ tệp `.csv` gốc của DAQ-M — được tìm thấy cùng với các hình ảnh. Các bản ghi GUI tự động đáp ứng yêu cầu này, vì chúng được thêm vào dự án ngay khi quá trình ghi kết thúc.
* **Dữ liệu hiệu chuẩn được tải về theo yêu cầu.** Nếu gói hiệu chuẩn xuất xưởng cho từng camera hoặc từng DAQ chưa được lưu trữ cục bộ,Chloros

sẽ tự động tải về từ đám mây củaMAPIR

khi sử dụng lần đầu (cần kết nối internet một lần; được lưu trữ dưới dạng tệp `~/.chloros/`).
* **Các bản ghi trực tiếp tạo tệp sidecar riêng.** Đối với bất kỳ khung hình phản xạ nào được ghi trực tiếp, giá trị đo DAQ thực tế được sử dụng sẽ được lưu dưới dạng tệp sidecar `.daq` bên cạnh hình ảnh, do đó bản ghi có thể được xử lý lại sau này mà không cần bản ghi gốc.

## Trích xuất lại dữ liệu bức xạ

Quá trình xử lý dự án cũng xuất tất cả các bản ghi cảm biến ánh sáng mà nó chứa vào một
thư mục `Light Sensor/` bên cạnh các sản phẩm hình ảnh. Quá trình này **không** cần hình ảnh: một
cảm biến ánh sáng được bay riêng lẻ đã là một bản ghi hoàn chỉnh, và một thư mục chỉ chứa các tệp `.daq`
là đầu vào hợp lệ. Báo cáo chạy sẽ cho biết số lượng sản phẩm cảm biến ánh sáng đã được ghi.

| Sản phẩm | Mô tả |
| --- | --- |
| `<name>_calibrated.daq` | Một kho lưu trữ có thể xử lý lại theo cùng cấu trúc với bản ghi trực tiếp, nay khai báo gói hiệu chuẩn đã tạo ra nó. Việc nhập lại tệp này **không** thực hiện hiệu chuẩn lần thứ hai. |
| `<name>_calibrated.csv` | Cường độ bức xạ quang phổ tính bằng W/m²/nm trên lưới bước sóng riêng của cảm biến, mỗi hàng tương ứng với một lần đo, kèm theo các cột dữ liệu quang học: công suất tổng, lux quang giác và lux tối giác, PPFD với phân tách xanh dương/xanh lục/đỏ, và bước sóng đỉnh. |

Một thiết bị DAQ-U hoặc DAQ-M mà gói hiệu chuẩn không thể được lấy về — do bạn đang ngoại tuyến, hoặc
cảm biến đó không có dữ liệu hiệu chuẩn trong tệp — sẽ bị **bỏ qua kèm lý do**, và tuyệt đối không được ghi ra
dưới dạng tệp &quot;đã hiệu chuẩn&quot; chứa số đếm thô. Hãy kết nối với internet và chạy lại. Một thiết bị DAQ-E
tự mang theo dữ liệu hiệu chuẩn của riêng mình, do đó chỉ cần thực hiện hiệu chuẩn khi thiết bị không được kết nối và
không có dữ liệu nào được lưu trữ cục bộ.

### DAQ-A: số đếm thô, và lý do tại sao đó là câu trả lời đúng

**DAQ-A** ra đời trước hệ thống gói hiệu chuẩn theo từng cổng nối tiếp và không có gói nào để
truy xuất. Điều này không phải là sơ suất: DAQ-A được hiệu chuẩn tại hiện trường so với một
mục tiêu phản xạ, và hiệu chuẩn dựa trên mục tiêu chỉ cần phản ứng *tương đối*
của cảm biến — chính xác là những gì các số đếm thô của nó thể hiện.Chloros

hiện nay vẫn hiệu chuẩn dựa trên các số đếm thô này.

Vì vậy, bản ghi của DAQ-A có thể xuất ra, nhưng dưới một tên khác:

```
<project>/
└── Light Sensor/
    ├── <name>_raw.daq
    └── <name>_raw.csv
```

`_raw`, chứ không phải `_calibrated` — một tên tệp khác thay vì một cờ bên trong tệp,
vì thông tin này phải được duy trì khi tệp được gửi qua email dưới dạng tên tệp thuần túy. Tiêu đề `.csv`
hiển thị `raw spectral sensor counts (NOT irradiance)` và cảnh báo rằng các giá trị chỉ
có thể so sánh **trong** cùng tệp chứ không phải giữa các cảm biến. Các cột chỉ có ý nghĩa
đối với bức xạ thực — tổng công suất, lux, PPFD — được để trống thay vì
tính toán từ số đếm.

Các bản ghi DAQ-A-SD cũ hơn (schema v1.01 / v1.02) chỉ ghi lại thời gian ghi tệp, không phải
dấu thời gian cho từng lần đọc.Chloros

sẽ không khớp với các hình ảnh đó — việc ghép một khung hình với một
thời gian ghi sẽ là sai mà không bao giờ trông có vẻ sai — nhưng quá trình xuất dữ liệu đọc chúng bình thường và
tệpCSV

cho biết đó là đồng hồ nào.

Để biết chi tiết đầy đủ về quy trình phản xạ — cảm biến đơn với máy ảnh và cảm biến kép môi trường/đối tượng — xem [Quy trình làm việc về phản xạ](reflectance.md).
