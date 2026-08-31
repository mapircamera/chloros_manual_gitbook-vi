# Thông số kỹ thuật nắp cảm biến và dải đo đã hiệu chuẩn

> Bản thân các nắp — loại nắp nào đi kèm với cảm biến nào, cách lắp đặt chúng và hành vi quang học của chúng — được ghi chép trong **[Sổ tay người dùng DAQ](https://mapir.gitbook.io/daq)**. Trang này đề cập đến việc *khai báo* nắp đã lắp đặt choChloros

, đây chính là yếu tố đảm bảo độ chính xác của quá trình hiệu chỉnh.

Mọi cảm biến ánh sáng DAQ đều được hiệu chuẩn phóng xạ tại nhà máy dựa trên cảm biến *trần*. Nắp vật lý được lắp trên bộ khuếch tán sẽ thay đổi loại ánh sáng mà cảm biến thu nhận, do đóChloros

sẽ áp dụng **hồ sơ hiệu chỉnh nắp** được đo tại nhà máy lên gói hiệu chuẩn. Việc khai báo nắp chính xác là một phần của quá trình thu được dữ liệu đã hiệu chuẩn — trang này trình bày các loại nắp hiện có theo từng mẫu, cách khai báo chúng và dải quang phổ hiệu chuẩn thực tế của cảm biến là gì.

## Tính khả dụng của nắp che theo từng mẫu

| Hồ sơ nắp che (`cap_id`) | Nắp che vật lý | DAQ-U | DAQ-M | DAQ-E |
| --- | --- | --- | --- | --- |
| `sunshine_cosine` | Nắp hiệu chỉnh cosin ánh nắng (**mặc định trên mọi mẫu**) | Có | Có | Có |
| `fov_15` / `fov_45` / `fov_90` | Nón hạn chế trường nhìn (15° / 45° / 90°) | Có | — | Có |
| `fov_30` / `fov_60` | Nón hạn chế trường nhìn (30° / 60°) | Có | — | — |
| `none` | Không có nắp | — | — | Có |

Ghi chú cụ thể cho từng mẫu:

* **DAQ-M chỉ có một kiểu nắp: `sunshine_cosine`.** &quot;Bare-plus-Sunshine-cap&quot; là định nghĩa sản phẩm của nó, và một DAQ-M không có nắp không cần cấu hình hình học.
* **Một DAQ-U không có nắp là &quot;true-bare&quot;** — nó hoàn toàn không cần cấu hình hình học nào cả, đó là lý do tại sao không tồn tại cấu hình `none` cho nó.
* **`none` trên DAQ-E KHÔNG phải là cấu hình không hoạt động.** Bộ khuếch tán lõm và được che bằng kính của DAQ-E có hiệu chỉnh hình học thực tế riêng, do đó &quot;không có nắp&quot; chính là một cấu hình đo lường trên mẫu này.
* Một **thiết bị DAQ-E không có nắp che không thể đo ánh nắng trực tiếp ở bất kỳ góc độ nào** — nắp che Sunshine là cấu hình tiêu chuẩn khi sử dụng ngoài trời. Không nên lên kế hoạch làm việc ngoài trời với thiết bị DAQ-E không có nắp che.

Trong cài đặt từng cảm biến của GUI (biểu tượng bánh răng trong tab Cảm biến Ánh sáng), menu thả xuống **Nắp** cũng cung cấp tùy chọn “Không có (cảm biến trần)” trên DAQ-U và DAQ-M — trên hai mẫu này, “trần” đơn giản có nghĩa là không áp dụng hiệu chỉnh nắp, theo các lưu ý ở trên. Chỉ chọn tùy chọn này khi nắp đã được tháo ra thực tế.

## Khai báo nắp — và tại sao điều này lại quan trọng

**Mã `cap_id` đã khai báo phải khớp với nắp đang được lắp trên cảm biến.** Cả cảm biến lẫn phần mềm đều không thể phát hiện nắp đã được lắp. Việc khai báo này ảnh hưởng đến hai yếu tố:

1. **Điều chỉnh thời gian thực** được áp dụng cho mọi phổ.
2. **Dấu nắp được ghi vào mọi bản ghi `.daq`**, mà quá trình xử lý phản xạ ở giai đoạn sau dựa vào.

Nắp Sunshine làm suy giảm tín hiệu khoảng **12 lần theo thiết kế**, do đó việc ghi dữ liệu với thông tin nắp được khai báo sai sẽ làm sai lệch tỷ lệ phổ theo hệ số đó. Hãy khai báo ngay lập tức khi thay đổi nắp.

### Cài đặt nắp

GUI: Tab Cảm biến ánh sáng → biểu tượng bánh răng trên hàng cảm biến → menu thả xuống **Nắp**. Giá trị mặc định cho mọi mẫu là `sunshine_cosine` (tất cả cảm biến DAQ đều được cài đặt sẵn bộ hiệu chỉnh cosin), và lựa chọn này được duy trì trong dự án.



CLI<!-- SCREENSHOT-NEEDED: DAQ tab per-sensor settings modal (gear icon) scrolled to the Cap dropdown, open to show the per-model choices with "Sunshine (cosine corrector)" selected. Use a connected DAQ-E so the Hostname/Firmware/PTP rows are also visible above it. -->

(hệ thống nền phải đang chạy):

```bash
# Declare at connect time
chloros-cli daq pool-connect --eth-host daq-e-def330.local --cap-id sunshine_cosine

# Swap at runtime (after physically changing the cap)
chloros-cli daq pool-set-cap --sensor-id daq-e-def330 --cap-id fov_45
```

CLI

chấp nhận cú pháp đầy đủ của danh sách `cap_id` (ví dụ: `{none, fov_15, fov_30, fov_45, fov_60, fov_90, sunshine_cosine}`); mỗi cấu hình sẽ được kiểm tra tính hợp lệ dựa trên mô hình cảm biến khi kết nối, do đó một ID tụ điện không khả dụng (ví dụ: ID chỉ dành cho E trên DAQ-U) sẽ báo lỗi rõ ràng thay vì hiệu chỉnh sai. Giá trị mặc định của phần nền khi không có thông số nào được truyền vào là `sunshine_cosine`.

Python

SDK

lưu ý: `cap_id` **không** phải là núm điều chỉnh &quot;SDK

&quot; — `connect_daq_sensor()` / `DAQSensorSession` không hiển thị thông số cap. Chọn giới hạn thông qua các lệnhCLI

ở trên hoặc menu thả xuống trên giao diện người dùng; xem [Tài liệu tham khảoSDK

](../reference/sdk-reference.md).

Nâng cao: các cấu hình được tích hợp sẵn trong bản cài đặtChloros

tại `daq/cap_profiles/<u|m|e>/<cap_id>.json` và có thể được ghi đè theo từng người dùng tại `~/.chloros/daq_cap_profiles/<u|m|e>/<cap_id>.json`.

Ngoài các giới hạn, các cảm biến chưa từng được hiệu chuẩn lại sẽ tự động nhận được một điều chỉnh nhỏ về độ lệch tối dựa trên dữ liệu của toàn bộ đội tàu — không cần người dùng can thiệp.

## Hiệu suất giới hạn ánh sáng mặt trời (cấu hình ngoài trời)

Các số liệu để xây dựng quy trình:

| Thuộc tính | Giá trị |
| --- | --- |
| Trường nhìn | 180° bán cầu |
| Sai số đáp ứng cosin | ≤ ±4 % đến góc tới 60°; ≤ ±4,5 % đến góc tới 70° |
| Giới hạn mặt trời thấp | Không khuyến nghị dưới ~15° độ cao mặt trời |
| Độ suy giảm | ~12× (theo thiết kế) |
| Độ lặp lại khi lắp lại nắp | ≈ 1,5 % |
| Cường độ bức xạ định lượng | Lấy trung bình **≥ 15 giây** dữ liệu đo (đặc tính của thiết bị, không phải là lỗi) |

Đối với bất kỳ giá trị cường độ bức xạ định lượng nào — bao gồm cả các giá trị tham chiếu về độ phản xạ — hãy sử dụng giá trị trung bình của ít nhất 15 giây dữ liệu đo thay vì một khung hình duy nhất.

## Dải quang phổ đã hiệu chuẩn

| Thuộc tính | Giá trị |
| --- | --- |
| Lấy mẫu quang phổ | 340–1010 nm với bước nhảy 5 nm (135 điểm) |
| Dải đã hiệu chuẩn phóng xạ | **~374–974 nm** (được áp dụng trong phần mềm) |

Cảm biến báo cáo toàn bộ lưới 340–1010 nm, nhưng hệ số khuếch đại phóng xạ có thể truy xuất về NIST chỉ bao phủ khoảng ~374–974 nm.Chloros

**từ chối chia theo độ phản xạ tuyệt đối** đối với bất kỳ dải phổ nào của máy ảnh có ít hơn một nửa trọng số phổ nằm trong khoảng đó, và báo lý do bỏ qua là `dls-uncalibrated-band-<nm>` thay vì tạo ra sản phẩm chưa được hiệu chuẩn. Trong số các mã sản phẩm (SKU) camera đang được phân phối, chỉ có bộ lọc F988 nằm ngoài phạm vi này; bộ lọc này sử dụng quy trình làm việc với bảng phản xạ thay thế — xem [Quy trình làm việc với phản xạ](reflectance.md).

Để biết thông tin về các mẫu cảm biến, phương thức truyền dữ liệu và ID cảm biến, xem [Tổng quan về DAQ](README.md). Để biết cách mã cap stamp được xử lý trong quá trình xử lý, xem [Ghi dữ liệu và định dạng .daq](recording.md).
