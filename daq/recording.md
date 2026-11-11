# Ghi dữ liệu và định dạng .daq

Tệp `.daq` là định dạng ghi dữ liệu từ cảm biến ánh sáng củChloros: một **cơ sở dữ liệu SQLite** chứa các khung hình quang phổ đã được hiệu chuẩn từ một cảm biến DAQ. Ghi lại một tệp trong phiên thu thập dữ liệu và quy trình xử lý độ phản xạ sau đó có thể chia mỗi hình ảnh cho cường độ bức xạ chiếu xuống được đo tại chính thời điểm đó.

## Nội dung của tệp .daq

| Thuộc tính | Giá trị |
| --- | --- |
| Container | Cơ sở dữ liệu SQLite, một tệp cho mỗi cảm biến trong mỗi lần ghi |
| Tên tệp | Bao gồm **ID cảm biến**và**dấu thời gian**, ví dụ: `daq_data_daq-e-def330_2026_04_13_18h30m00.daq` |
| Phổ cho mỗi khung hình | 135 điểm, dải 340–1010 nm với bước nhảy 5 nm, kèm theo hệ tọa độ ba kích thích CIE XYZ |
| Đơn vị | Bức xạ phổ đã hiệu chuẩn, **W/m²/nm** (gói hiệu chuẩn nhà máy + hồ sơ nắp đã được áp dụng) |
| Dữ liệu meta được ghi | ID cảm biến (mã khóa để truy xuất dữ liệu hiệu chuẩn nhà máy của thiết bị đó) và hồ sơ nắp hiện đang áp dụng — xem [Hồ sơ nắp & Phạm vi hiệu chuẩn](caps-and-range.md) |

Định dạng này giống hệt nhau trên các thiết bị DAQ-U, DAQ-M, DAQ-E và DAQ-E-S, do đó quá trình xử lý tiếp theo không quan tâm đến thiết bị nào đã ghi lại dữ liệu. Bản ghi của DAQ-E-S cũng lưu trữ góc nghiêng, góc lăn và góc dọc của thiết bị bên cạnh mỗi phổ.

**Các bản ghi vẫn được bảo toàn khi mất điện (1.3.0).**Một bản ghi trực tiếp được ghi vào nhật ký và đồng bộ hóa toàn bộ đĩa sau mỗi lần lưu, do đó việc mất điện hoặc quá trình bị ngắt chỉ làm mất khoảng 3 giây dữ liệu gần nhất, chứ không làm mất toàn bộ tệp. Một bản ghi bị dừng giữa chừng khi đang ghi có thể để lại tệp `<name>.daq-journal` bên cạnh tệp `.daq`:**hãy giữ lại và sao chép cả hai tệp này cùng nhau.** Chloros sẽ khôi phục nhật ký khi lần đầu tiên đọc tệp. Đĩa bận sẽ làm chậm quá trình ghi tệp, chứ không bao giờ ảnh hưởng đến dữ liệu đo từ cảm biến.

**Một thiết bị DAQ-E ngừng truyền dữ liệu sẽ được kết nối lại (phiên bản 1.3.0).** Khi luồng dữ liệu của DAQ-E ngừng hoạt động (do công tắc nguồn bị tắt/bật lại hoặc mất kết nối), hệ thống nền sẽ kết nối lại thiết bị với các cài đặt trước đó, và quá trình ghi dữ liệu trên thiết bị đó sẽ tiếp tục trong cùng tệp `.daq` với khoảng trống được đánh dấu trong tệp.

Việc ghi dữ liệu đã hiệu chuẩn yêu cầu gói hiệu chuẩn gốc của cảm biến. Đối với DAQ-U và DAQ-M, hệ thống nền sẽ lấy gói này từ đám mây của MAPIR dựa trên ID cảm biến (việc ghi dữ liệu sẽ bị từ chối nếu không thể lấy được); các thiết bị DAQ-E được miễn trừ vì chúng lưu trữ dữ liệu hiệu chuẩn trực tiếp trên thiết bị.

## Ghi dữ liệu từ giao diện người dùng (GUI)

Việc ghi dữ liệu trong GUI yêu cầu phải có **dự án đang mở** (nếu không, các nút Ghi sẽ bị vô hiệu hóa):

* **Ghi tất cả / Dừng tất cả** — ở đầu thanh bên Cảm biến ánh sáng; bắt đầu hoặc dừng ghi dữ liệu `.daq` trên tất cả các cảm biến được kết nối cùng một lúc.
* **Ghi / Dừng ghi** — cho từng cảm biến, trong cửa sổ cài đặt hình bánh răng. Một chỉ báo "REC" màu đỏ sẽ hiển thị trong các hàng thông tin trực tiếp của cảm biến trong khi ghi.

Các tệp được ghi vào `<project>/light_sensor/`, và khi quá trình ghi dừng lại — dù là qua nút Dừng, Dừng tất cả hay ngắt kết nối cảm biến đang ghi — tệp `.daq` đã hoàn thành sẽ **tự động được thêm vào dự án đang mở**. Tệp này xuất hiện trong danh sách tệp của dự án mà không cần thao tác thêm thủ công, sẵn sàng cho quá trình xử lý phản xạ.

<!-- SCREENSHOT-NEEDED: Light Sensors tab with one DAQ sensor connected and recording: sidebar showing the red "Stop All" state of the Record All button, the sensor row, and the settings modal open with the red "REC" indicator visible in the live info rows. -->

<!-- SCREENSHOT-NEEDED: File Browser / project file list immediately after stopping a DAQ recording, showing the .daq file auto-added to the open project alongside imagery. -->



## Ghi dữ liệu từ CLI



CLI thực hiện ghi dữ liệu thông qua nhóm cảm biến của backend (backend phải đang chạy — các lệnh này là các client HTTP nhẹ):

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
| `--output` / `-o` | `~/Documents/DAQ Live View/` trên hệ thống tệp của **backend**, không phải của CLI |

Sự khác biệt về thư mục đầu ra rất quan trọng khi CLI nhắm đến một backend trên một máy khác: tệp sẽ được lưu ở nơi backend chạy. Khi backend nằm trên cùng một máy tính, đường dẫn tương đối `-o` (chẳng hạn như `./out` ở trên) sẽ được giải quyết dựa trên thư mục mà bạn đã chạy lệnh.

## Ghi lại từ Python

`DAQSensorSession` (được trả về bởi `chloros_sdk.connect_daq_sensor()`) hiển thị cùng một bản ghi được gộp lại: `record_start(output_dir=None, device_name=None)` trả về đường dẫn tệp, `record_stop()` trả về `{path, rows}`. Xem [Tài liệu tham khảo SDK](../reference/sdk-reference.md) để biết đầy đủ các API phiên làm việc. Các lớp phần cứng trực tiếp của SDK (chỉ dành cho cài đặt trên máy tính để bàn) ghi bản ghi vào `~/Documents/DAQ/` theo mặc định; đối với các bản phát hành chính thức, đường dẫn gộp ở trên là phương thức được hỗ trợ.

## Sử dụng tệp .daq trong quá trình xử lý

Để tính toán độ phản xạ từ hình ảnh, Chloros cần cường độ bức xạ hướng xuống (downwelling irradiance) khớp với từng lần phơi sáng:

* **Giữ tệp `.daq` cùng với hình ảnh.**Trong quá trình xử lý, quy trình xử lý sẽ tự động xác định**cường độ bức xạ hướng xuống phù hợp với dấu thời gian** từ tệp `.daq` đã ghi (bất kỳ mẫu DAQ nào) — hoặc từ tệp `.csv` gốc của DAQ-M — được tìm thấy cùng với các hình ảnh. Các bản ghi GUI tự động đáp ứng yêu cầu này, vì chúng được thêm vào dự án ngay khi quá trình ghi kết thúc.
* **Việc hiệu chuẩn được thực hiện theo yêu cầu.** Nếu gói hiệu chuẩn nhà máy cho từng camera hoặc từng DAQ chưa được lưu trữ cục bộ, Chloros sẽ tự động tải xuống từ đám mây củMAPIR khi sử dụng lần đầu (cần kết nối internet một lần; được lưu trữ dưới dạng tệp `~/.chloros/`).
* **Các bản ghi trực tiếp tự tạo tệp sidecar riêng.** Đối với bất kỳ khung hình phản xạ nào được ghi trực tiếp, giá trị đo DAQ thực tế được sử dụng sẽ được lưu dưới dạng tệp `.daq` trong thư mục `daq/` của bản ghi, do đó bản ghi có thể được xử lý lại sau này mà không cần bản ghi gốc.
* **Các bản ghi từ các thiết bị ghi `chloros_scripts` (phiên bản 1.3.0).**Một tệp `.daq` được tạo bởi các thiết bị ghi công cộng [`chloros_scripts`](https://github.com/mapircamera/chloros_scripts) sẽ được liệt kê là mô hình DAQ đã tạo ra nó, và giới hạn của nó có thể được thiết lập trong**Cài đặt Dự án → Cảm biến Ánh sáng DAQ** giống như bất kỳ bản ghi nào khác. Các thiết bị ghi này ghi lại số đếm thô từ cảm biến; Chloros lấy dữ liệu hiệu chuẩn nhà máy của cảm biến qua cổng nối tiếp và áp dụng nó.
* **Dữ liệu này thuộc về ai (1.3.0).**Bên cạnh mỗi bản ghi `.daq`, một `.daq.cameras.json` sẽ chỉ định tên (các) máy ảnh mà giá trị đo đó mô tả, và quá trình xử lý sẽ**chỉ khớp giá trị đó với hình ảnh từ các máy ảnh đó**. Do đó, một camera độc lập không có cảm biến ánh sáng riêng sẽ không còn mượn giá trị đo từ camera khác nữa — hãy gán một cảm biến cho nó hoặc ghi lại giá trị đo từ cảm biến đó. Một bản ghi được thực hiện bằng chính DAQ (GUI Record, `daq pool-record`), và bất kỳ tệp `.daq` nào không chứa tệp đó (bao gồm cả các bản ghi cũ), vẫn phục vụ cho mọi camera.

## Lấy lại dữ liệu bức xạ

Việc xử lý một dự án cũng sẽ xuất tất cả các bản ghi cảm biến ánh sáng mà nó chứa vào một
thư mục `Light Sensor/` bên cạnh các sản phẩm hình ảnh. Quá trình này **không** yêu cầu hình ảnh: một
bản ghi cảm biến ánh sáng được ghi riêng lẻ đã là một bản ghi hoàn chỉnh, và một thư mục chỉ chứa các tệp `.daq`
là đầu vào hợp lệ. Một dự án chỉ chứa các bản ghi cảm biến ánh sáng sẽ liệt kê chúng và
được xử lý từ cả ứng dụng lẫn giao diện dòng lệnh (CLI), và chúng vẫn được liệt kê sau khi chạy. Quá trình chạy
sẽ báo cáo số lượng sản phẩm cảm biến ánh sáng đã được ghi.

| Sản phẩm | Mô tả |
| --- | --- |
| `<name>_calibrated.daq` | Một tệp lưu trữ có thể xử lý lại theo cùng cấu trúc với bản ghi trực tiếp, nay khai báo gói hiệu chuẩn đã tạo ra nó. Việc nhập lại tệp này **sẽ không** hiệu chuẩn lại lần thứ hai. |
| `<name>_calibrated.csv` | Cường độ bức xạ quang phổ tính bằng W/m²/nm trên lưới bước sóng riêng của cảm biến, mỗi hàng tương ứng với một lần đo, kèm theo các cột thông số quang học: công suất tổng, lux quang giác và tối giác, PPFD với phân tách xanh dương/xanh lục/đỏ, và bước sóng đỉnh. |

Một thiết bị DAQ-U hoặc DAQ-M mà gói hiệu chuẩn không thể được lấy về — do bạn đang ngoại tuyến, hoặc
cảm biến đó không có dữ liệu hiệu chuẩn trong tệp — sẽ bị **bỏ qua kèm lý do**, và tuyệt đối không được ghi ra
dưới dạng tệp "đã hiệu chuẩn" chứa số đếm thô. Hãy kết nối với internet và chạy lại. Một thiết bị DAQ-E
tự lưu trữ dữ liệu hiệu chuẩn của riêng mình, do đó chỉ cần thực hiện hiệu chuẩn khi thiết bị không được kết nối và
không có dữ liệu nào được lưu trữ cục bộ.

### DAQ-A: số đếm thô, và tại sao đó là câu trả lời đúng

**DAQ-A** ra đời trước hệ thống gói hiệu chuẩn theo từng cổng nối tiếp và không có gói nào để
truy xuất. Điều này không phải là sơ suất: DAQ-A được hiệu chuẩn tại hiện trường dựa trên
mục tiêu phản xạ, và hiệu chuẩn dựa trên mục tiêu chỉ cần phản ứng *tương đối*
của cảm biến — chính xác là những gì số đếm thô của nó thể hiện. Chloros hiện nay vẫn hiệu chuẩn bằng cách này.

Vì vậy, bản ghi của DAQ-A có thể xuất ra, nhưng dưới một tên khác:

```
<project>/
└── Light Sensor/
    ├── <name>_raw.daq
    └── <name>_raw.csv
```

`_raw`, chứ không phải `_calibrated` — tên tệp khác thay vì một cờ bên trong tệp,
vì thông tin này phải được giữ nguyên khi tệp được gửi qua email dưới dạng tên tệp thuần túy. Tiêu đề `.csv`
hiển thị `raw spectral sensor counts (NOT irradiance)` và cảnh báo rằng các giá trị chỉ
có thể so sánh **trong** cùng một tệp chứ không phải giữa các cảm biến. Các cột chỉ có ý nghĩa
đối với bức xạ thực — tổng công suất, lux, PPFD — được để trống thay vì
được tính toán từ số đếm.

Các bản ghi DAQ-A-SD cũ hơn (schema v1.01 / v1.02) chỉ ghi lại thời gian ghi tệp, chứ không phải
dấu thời gian cho từng lần đọc. Chloros sẽ không khớp các hình ảnh với những dữ liệu đó — việc ghép một khung hình với
thời gian ghi sẽ là sai dù bề ngoài trông có vẻ đúng — nhưng quá trình xuất dữ liệu đọc chúng bình thường và
tệp CSV ghi rõ đó là đồng hồ nào.

Để biết thông tin đầy đủ về phản xạ — cảm biến đơn với máy ảnh và cảm biến kép môi trường xung quanh/đối tượng — hãy xem [Quy trình làm việc về phản xạ](reflectance.md).
