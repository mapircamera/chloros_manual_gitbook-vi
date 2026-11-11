# Thông số nắp cảm biến và dải đo đã hiệu chuẩn

> Bản thân các nắp — loại nắp nào đi kèm với cảm biến nào, cách lắp đặt và hành vi quang học của chúng — được ghi chép trong **[Hướng dẫn sử dụng DAQ](https://mapir.gitbook.io/daq)**. Trang này đề cập đến việc *khai báo* nắp đã lắp đặt cho Chloros
, đây chính là yếu tố đảm bảo độ chính xác của quá trình hiệu chỉnh.

Mọi quá trình hiệu chuẩn quang phổ tại nhà máy của cảm biến ánh sáng DAQ đều mô tả cảm biến *trần*. Nắp vật lý được lắp trên bộ khuếch tán sẽ thay đổi loại ánh sáng mà cảm biến thu nhận, do đó Chloros
áp dụng **hồ sơ hiệu chỉnh nắp** được đo tại nhà máy lên trên gói hiệu chuẩn. Việc khai báo nắp chính xác là một phần của quá trình thu được dữ liệu đã được hiệu chuẩn — trang này trình bày các loại nắp hiện có cho từng mẫu, cách khai báo chúng và dải quang phổ hiệu chuẩn thực tế của cảm biến là gì.

## Tính sẵn có của nắp theo từng mẫu

| Hồ sơ nắp (`cap_id`) | Nắp vật lý | DAQ-U | DAQ-M | DAQ-E |
| --- | --- | --- | --- | --- |
| `sunshine_cosine` | Nắp hiệu chỉnh cosin ánh nắng (**mặc định trên mọi mẫu**) | Có | Có | Có |
| `fov_15` / `fov_45` / `fov_90` | Nón hạn chế trường nhìn (15° / 45° / 90°) | Có | — | Có |
| `fov_30` / `fov_60` | Nón hạn chế trường nhìn (30° / 60°) | Có | — | — |
| `none` | Không có nắp | — | — | Có |

Ghi chú cụ thể cho từng mẫu:

* **DAQ-M có một kiểu nắp duy nhất: `sunshine_cosine`.** &quot;Bare-plus-Sunshine-cap&quot; là định nghĩa sản phẩm của nó, và một DAQ-M &quot;bare&quot; không cần cấu hình hình học.
* **Một DAQ-U &quot;bare&quot; là &quot;true-bare&quot;** — nó hoàn toàn không cần cấu hình hình học, đó là lý do tại sao không có cấu hình `none` nào dành cho nó.
* **`none` trên DAQ-E KHÔNG phải là cấu hình không hoạt động (no-op).** Bộ khuếch tán lõm và được che bằng kính của DAQ-E có hiệu chỉnh hình học thực tế riêng, do đó &quot;không có nắp&quot; chính là một cấu hình đo lường trên mẫu này.
* Một **DAQ-E không có nắp che không thể đo ánh nắng trực tiếp ở bất kỳ góc độ nào** — nắp che Sunshine là cấu hình tiêu chuẩn khi sử dụng ngoài trời. Không nên lên kế hoạch làm việc ngoài trời với một DAQ-E không có nắp che.

Trong cài đặt từng cảm biến của GUI (biểu tượng bánh răng trong tab Cảm biến Ánh sáng), menu thả xuống **Nắp** cũng cung cấp tùy chọn &quot;Không có (cảm biến trần)&quot; trên DAQ-U và DAQ-M — trên hai mẫu này, &quot;trần&quot; đơn giản có nghĩa là không áp dụng hiệu chỉnh nắp, theo các lưu ý ở trên. Chỉ chọn tùy chọn này khi nắp đã được tháo ra thực tế.

## Khai báo nắp — và tại sao điều này lại quan trọng

**Mã `cap_id` được khai báo phải khớp với nắp đang được lắp trên cảm biến.** Cả cảm biến lẫn phần mềm đều không thể phát hiện nắp đã được lắp. Việc khai báo này quyết định hai yếu tố:

1. **Chỉnh sửa thời gian thực** được áp dụng cho mọi phổ.
2. **Dấu nắp được ghi vào mọi bản ghi `.daq`**, mà quá trình xử lý phản xạ ở giai đoạn sau dựa vào.

Nắp Sunshine làm suy giảm tín hiệu khoảng **12 lần theo thiết kế**, do đó việc ghi dữ liệu với thông tin nắp được khai báo sai sẽ làm sai lệch tỷ lệ phổ theo hệ số đó. Hãy khai báo ngay lập tức khi thay đổi nắp.

### Cài đặt nắp

GUI: Tab Cảm biến Ánh sáng → biểu tượng bánh răng trên hàng cảm biến → menu thả xuống **Nắp**. Giá trị mặc định cho mọi mô hình là `sunshine_cosine`, đây là giả định (các mô hình DAQ-U, DAQ-M và DAQ-E được cung cấp mà không có nắp, và bản ghi sẽ ghi nhận rằng nắp được giả định cho đến khi bạn chọn một giá trị cụ thể), và lựa chọn này sẽ được duy trì trong suốt dự án.



CLI<!-- SCREENSHOT-NEEDED: DAQ tab per-sensor settings modal (gear icon) scrolled to the Cap dropdown, open to show the per-model choices with "Sunshine (cosine corrector)" selected. Use a connected DAQ-E so the Hostname/Firmware/PTP rows are also visible above it. -->
(phần nền phải đang chạy):

```bash
# Declare at connect time
chloros-cli daq pool-connect --eth-host daq-e-def330.local --cap-id sunshine_cosine

# Swap at runtime (after physically changing the cap)
chloros-cli daq pool-set-cap --sensor-id daq-e-def330 --cap-id fov_45
```

CLI
chấp nhận cú pháp danh sách đầy đủ `cap_id` (ví dụ: `{none, fov_15, fov_30, fov_45, fov_60, fov_90, sunshine_cosine}`); mỗi cấu hình sẽ được kiểm tra tính hợp lệ dựa trên mô hình cảm biến khi kết nối, do đó nếu ID giới hạn không khả dụng (ví dụ: ID chỉ dành cho E trên DAQ-U) sẽ báo lỗi rõ ràng thay vì điều chỉnh sai. Giá trị mặc định của backend khi không có thông số nào được truyền vào là `sunshine_cosine`.

Python
SDK
: đặt tên cho cảm biến tại thời điểm kết nối bằng `connect_daq_sensor(cap_id="sunshine_cosine")`, hoặc thay đổi tên tại thời gian chạy bằng `DAQSensorSession.set_cap("fov_45")`; xem [Tài liệu tham khảo SDK
](../reference/sdk-reference.md#daq-sensor-sessions).

Nâng cao: các cấu hình được tích hợp sẵn trong bản cài đặt Chloros
tại `daq/cap_profiles/<u|m|e>/<cap_id>.json` và có thể được ghi đè theo từng người dùng tại `~/.chloros/daq_cap_profiles/<u|m|e>/<cap_id>.json`.

Ngoài các giới hạn, các cảm biến chưa từng được hiệu chuẩn lại sẽ tự động nhận được một sự tinh chỉnh nhỏ về độ lệch tối (dark-offset) dựa trên dữ liệu từ toàn bộ hệ thống — không cần người dùng can thiệp.

## DAQ-E-S: bộ khuếch tán cố định và góc nghiêng

**DAQ-E-S** là một DAQ-E với hai điểm khác biệt quan trọng đối với Chloros
. Tất cả các chức năng còn lại (phát hiện, kết nối, PTP, ghi dữ liệu, cài đặt thiết bị) hoạt động chính xác như trên DAQ-E.

* **Bộ khuếch tán của nó là cố định, do đó không cần hiệu chỉnh nắp.** Bộ khuếch tán đã được lắp sẵn trên thiết bị khi thực hiện hiệu chuẩn tại nhà máy, do đó hiệu chuẩn này đã phản ánh chính xác cường độ bức xạ chiếu xuống. Chloros
không áp dụng bất kỳ hồ sơ nắp nào cho nó dù được yêu cầu thế nào: một nắp được đặt tên cho nó sẽ bị bỏ qua kèm thông báo, các bản ghi của nó cho thấy nắp đến từ mô hình, và cảnh báo về nắp giả định không bao giờ được kích hoạt đối với nó. Trong Cài đặt Dự án → Cảm biến Ánh sáng DAQ, hàng của thiết bị này không hiển thị giới hạn nào, và việc ghi đè giới hạn trên toàn dự án không ảnh hưởng đến nó. Không áp dụng giới hạn cho dữ liệu của thiết bị này bằng tay: hiệu chỉnh Sunshine áp dụng lên trên sẽ cho giá trị cao gấp khoảng 11 lần.
* **Thiết bị này được trang bị cảm biến gia tốc 3 trục.** Đáp ứng cosin của bộ khuếch tán chỉ chính xác khi thiết bị ở trạng thái cân bằng, do đó cảm biến bị nghiêng sẽ báo cáo giá trị bức xạ sai mà không kèm theo triệu chứng nào khác. Mỗi giá trị đọc từ DAQ-E-S do đó chứa thông tin về góc nghiêng, góc lăn và góc xoay, và dữ liệu được ghi lại từ một thiết bị (`.daq`) sẽ lưu trữ các thông tin này bên cạnh mỗi phổ (`imu_tilt_deg`, `imu_roll_deg`, `imu_pitch_deg`, …), nhờ đó bạn có thể phân biệt được hiện tượng mây với trường hợp giá đỡ bị va đập.

Để biết chi tiết về từng trường tư thế và cách báo cáo lỗi trong luồng dữ liệu tư thế, xem [DAQ-E-S trong Tài liệu tham khảo CLI
](../reference/cli-reference.md#daq-e-s--fixed-diffuser-and-tilt-sensor).

## Hiệu suất của Sunshine cap (cấu hình ngoài trời)

Các số liệu để xây dựng quy trình:

| Thuộc tính | Giá trị |
| --- | --- |
| Trường nhìn | 180° hình bán cầu |
| Sai số đáp ứng cosin | ≤ ±4 % đến góc tới 60°; ≤ ±4,5 % đến góc tới 70° |
| Giới hạn mặt trời thấp | Không khuyến nghị dưới ~15° độ cao mặt trời |
| Suy giảm | ~12× (theo thiết kế) |
| Độ lặp lại khi lắp lại nắp | ≈ 1,5 % |
| Cường độ bức xạ định lượng | Lấy trung bình **≥ 15 giây** dữ liệu đo (đặc tính của thiết bị, không phải lỗi) |

Đối với bất kỳ giá trị cường độ bức xạ định lượng nào — bao gồm cả các giá trị tham chiếu phản xạ — hãy sử dụng giá trị trung bình của ít nhất 15 giây dữ liệu đo thay vì một khung hình duy nhất.

## Dải quang phổ đã hiệu chuẩn

| Thuộc tính | Giá trị |
| --- | --- |
| Lấy mẫu quang phổ | 340–1010 nm với bước nhảy 5 nm (135 điểm) |
| Dải đã hiệu chuẩn phóng xạ | **~374–974 nm** (được áp dụng trong phần mềm) |

Cảm biến báo cáo toàn bộ lưới 340–1010 nm, nhưng hệ số khuếch đại phóng xạ có thể truy xuất về NIST chỉ bao phủ khoảng ~374–974 nm. Chloros
**từ chối phân chia độ phản xạ tuyệt đối** đối với bất kỳ dải quang phổ nào của máy ảnh có ít hơn một nửa trọng số quang phổ nằm trong khoảng đó, và báo cáo lý do bỏ qua là `dls-uncalibrated-band-<nm>` thay vì tạo ra sản phẩm chưa được hiệu chuẩn. Trong số các mã sản phẩm (SKU) máy ảnh đang được phân phối, chỉ có bộ lọc F988 nằm ngoài phạm vi này; bộ lọc này sử dụng quy trình làm việc với bảng phản xạ thay thế — xem [Quy trình làm việc với phản xạ](reflectance.md).

Để biết thông tin về các mẫu cảm biến, phương thức truyền dữ liệu và ID cảm biến, hãy tham khảo [Tổng quan về DAQ](README.md). Để biết cách dấu thời gian (cap stamp) được xử lý trong quá trình xử lý, hãy tham khảo [Ghi dữ liệu và định dạng .daq](recording.md).
