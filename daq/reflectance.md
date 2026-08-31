# Quy trình xử lý độ phản xạ

Một cảm biến ánh sáng DAQ chuyển đổi hình ảnh phóng xạ thành độ phản xạ. Có hai quy trình xử lý riêng biệt:

1. **Cảm biến đơn** — một cảm biến DAQ đo cường độ bức xạ chiếu xuống trong khi máy ảnh ghi hình, và hàm chia độ phản xạ (Chloros

) chia độ sáng (radiance) từ máy ảnh cho giá trị tham chiếu đó.
2. **Hai cảm biến** — hai cảm biến DAQ, một quan sát bầu trời và một quan sát đối tượng, tạo ra đường cong phản xạ quang phổ thời gian thực mà không cần đến máy ảnh.

## Cảm biến đơn + máy ảnh (giá trị tham chiếu bức xạ xuống)

Cảm biến DAQ đóng vai trò là cảm biến ánh sáng chiếu xuống (DLS): máy ảnh đo độ sáng chiếu lên **L**(W/m²/sr/nm), DAQ đo cường độ bức xạ chiếu xuống**E** (W/m²/nm), vàChloros

tính toán hệ số phản xạ theo từng dải phổ như sau:

> ρ = π · L / E

Giá trị đọc từ DAQ luôn **được đồng bộ hóa theo dấu thời gian với thời gian phơi sáng** — đây là lý do tại sao DAQ và các camera chia sẻ cùng một đồng hồ được điều chỉnh theo PTP (xem [Mạng và Đồng bộ hóa Thời gian DAQ-E](ethernet-ptp.md)). Đeo mũ chống nắng Sunshine khi làm việc ngoài trời và khai báo chính xác; việc khai báo mũ sẽ điều chỉnh trực tiếp giá trị E (xem [Cap Profiles &amp; Calibrated Range](caps-and-range.md)). Đối với công việc định lượng, hãy nhớ đặc tính của thiết bị: bức xạ định lượng được tính từ giá trị trung bình của ít nhất 15 giây đo.

### Ghi hình trực tiếp

Kết nối thiết bị thu thập dữ liệu (DAQ) với máy ảnh trong tab &quot;Cameras&quot;: bảng cài đặt của mỗi máy ảnh có menu thả xuống **Light Sensor** liệt kê tất cả các thiết bị DAQ đã kết nối (DAQ-U/M/E) từ tab &quot;Light Sensors&quot;; đối với mảng đồng bộ, việc chọn Light Sensor cho toàn mảng sẽ áp dụng cho mọi thành viên (các máy ảnh riêng lẻ vẫn có thể ghi đè). Sau khi được liên kết, phổ của cảm biến sẽ được đưa vào khe DLS của máy ảnh và các giá trị phản xạ xuất ra sẽ được chia cho giá trị đo phù hợp.



<!-- SCREENSHOT-NEEDED: Cameras tab per-camera settings panel showing the "Light Sensor" dropdown open, with a connected DAQ sensor listed and selected. -->

Hai hành vi cần lưu ý:

* **Không có DAQ được liên kết → giá trị phản xạ bị từ chối, không bị giả mạo.**Chloros

từ chối sản phẩm phản xạ và ghi lại lý do bỏ qua thay vì âm thầm trả về một sản phẩm có giá trị thấp hơn.
* **Giá trị đo được sử dụng sẽ được giữ nguyên.** Đối với mỗi khung hình phản xạ, giá trị DAQ thực tế được áp dụng sẽ được ghi dưới dạng tệp phụ `.daq` kèm theo hình ảnh, do đó dữ liệu thu thập có thể được xử lý lại sau này ([Ghi lại &amp; Định dạng .daq](recording.md)).

### Xử lý hình ảnh đã ghi lại

Đối với quá trình xử lý sau chuyến bay, hãy ghi lại tệp `.daq` trong suốt phiên làm việc và lưu trữ nó cùng với hình ảnh — quy trình xử lý sẽ tự động giải quyết dữ liệu hướng xuống (downwelling) khớp với dấu thời gian, đồng thời lấy bất kỳ dữ liệu hiệu chuẩn nhà máy nào còn thiếu từ đám mây củaMAPIR

trong lần sử dụng đầu tiên. Các bản ghi từ giao diện người dùng (GUI) sẽ được tự động thêm vào dự án đang mở khi quá trình ghi kết thúc.

Tham chiếu phản xạ có thể được chọn tại thời điểm xử lý — `--reflectance-source` trên `chloros-cli process`, hoặc cài đặt “nguồn phản xạ” trong phần “Cài đặt Dự án” của GUI:

| Giá trị | Hành vi |
| --- | --- |
| `auto` (mặc định) | Mục tiêu hiệu chuẩn trong khung hình đạt tiêu chuẩn QA là tham chiếu tuyệt đối; giá trị phản xạ xuống của DAQ (ρ = π·L/E) là giá trị dự phòng |
| `daq` | DAQ là nguồn tham chiếu chính thức |
| `target` | Mục tiêu trong khung hình nghiêm ngặt; không thay thế bằng DAQ |

Xem [Mục tiêu hiệu chuẩn](../calibration-targets.md) để biết quy trình làm việc với mục tiêu và [chương LATTICE](../lattice/README.md) cùng với [Tham chiếuCLI

](../reference/cli-reference.md) để biết quy trình xử lý đầy đủ. Khi đọc các pixel phản xạ đã xuất, hãy sử dụng thang đo được đóng dấu (LATTICE: 32768 = ρ 1,0, XMP `Chloros:PixelScale`;Survey3

: 65535) — xem [Định dạng hình ảnh đầu ra](../output-image-formats.md).

### Các dải nằm ngoài phạm vi hiệu chuẩn của DAQ

Phạm vi hiệu chuẩn quang phổ của DAQ là ~374–974 nm.Chloros

từ chối dữ liệu phản xạ dựa trên DAQ đối với bất kỳ dải phổ nào của camera có ít hơn một nửa trọng số phổ nằm trong khoảng đó, và báo cáo lý do bỏ qua là `dls-uncalibrated-band-<nm>`. Trong số các SKU đang được phân phối, điều này chỉ ảnh hưởng đến F988: giá trị phản xạ của F988 được hiệu chuẩn bằng bảng phản xạ trong cảnh: dải sóng này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đóChloros

sẽ áp dụng dữ liệu chụp bảng phản xạ gần nhất của bạn và duy trì giá trị đó giữa các lần quan sát bảng. Nếu camera F988 được vận hành ở chế độ chỉ DAQ,Chloros

sẽ từ chối giá trị phản xạ dựa trên DAQ cho dải sóng đó với lý do bỏ qua là `dls-uncalibrated-band-988` — quy trình làm việc với bảng đo là phương thức được hỗ trợ.

## Cảm biến kép (ánh sáng xung quanh + đối tượng)

Hai cảm biến DAQ — bất kỳ cặp nào, trên bất kỳ phương tiện truyền tải nào — sẽ cung cấp phổ phản xạ thời gian thực mà không cần máy ảnh: một cảm biến hướng lên bầu trời (**Nguồn ánh sáng môi trường**), một cảm biến hướng về đối tượng (**Máy quét đối tượng**), vàChloros

tính toán theo từng bước sóng:

> R(λ) = đối tượng(λ) / môi trường(λ)

(bằng 0 khi ánh sáng xung quanh ≤ 0).

### Trong giao diện người dùng

Khi cả hai cảm biến đã được kết nối trong tab Cảm biến Ánh sáng, hãy mở cửa sổ thêm cảm biến (nút &quot;+&quot; trên ô biểu đồ trong chế độ xem lưới) và chọn **Kết hợp Ánh sáng Xung quanh + Đối tượng**. Chọn hai cảm biến trong các menu thả xuống Nguồn ánh sáng xung quanh và Máy quét đối tượng, sau đó nhấp vào Tạo. Nhóm này sẽ xuất hiện dưới dạng biểu đồ riêng biệt và dưới dạng một hàng trên thanh bên với huy hiệu màu xanh lá cây**REF**.



<!-- SCREENSHOT-NEEDED: The add-sensor overlay's "Combine Ambient + Object" panel with two connected DAQ sensors selected in the Ambient Light Source and Object Scanner dropdowns, Create button enabled. -->

<!-- SCREENSHOT-NEEDED: A live Apparent Reflectance chart from an Ambient+Object DAQ pair in list view, with the vegetation-index table visible below the chart (NDVI etc. showing live values). -->

Dưới biểu đồ độ phản xạ (chế độ xem danh sách), một **bảng chỉ số thực vật** thời gian thực tính toán các chỉ số từ đường cong bằng cách sử dụng các tần số trung tâm dải màu xanh dương 450 / xanh lá 550 / đỏ 670 /NIR

800 nm. Các chỉ số dựa trên tỷ lệ loại bỏ thang đo tuyệt đối (NDVI

,GNDVI

, ENDVI,WDRVI

,GRVI

,CVI

,GCI

, MSR) luôn được hiển thị; các chỉ số cần độ phản xạ tuyệt đối (EVI

,SAVI

,OSAVI

, GSAVI, GOSAVI, MSAVI2, RDVI, TDVI,LAI

, NLI, MNLI, FCI,GEMI

) chỉ xuất hiện khi cả hai cảm biến đều là các mô hình đã được hiệu chuẩn công suất.

### Hệ số phản xạ biểu kiến so với hệ số phản xạ tương đối — quy tắc đặt nhãn

Chloros

đặt nhãn cho đầu ra của cặp cảm biến dựa trên những gì cặp cảm biến đó thực sự có thể khẳng định:

| Cặp cảm biến | Nhãn |
| --- | --- |
| Cả hai cảm biến đều được hiệu chuẩn — gói hiệu chuẩn nhà máy đã được tải | **Hệ số phản xạ biểu kiến** |
| Một trong hai cảm biến chưa được hiệu chuẩn | **Độ phản xạ tương đối** |

Cả ba mô hình đều là mô hình phóng xạ: một khi gói hiệu chuẩn nhà máy của cảm biến được tải, phổ của nó sẽ là giá trị tuyệt đối W/m²/nm, do đó tỷ lệ giữa hai cảm biến đã hiệu chuẩn sẽ cho ra độ phản xạ biểu kiến tuyệt đối — quá trình truyền dữ liệu không quyết định điều này. Một cảm biến vẫn đang truyền dữ liệu thô (không truy cập được gói hiệu chuẩn) sẽ làm kết quả trở thành đường cong tương đối (hình dạng phổ vẫn hợp lệ). Cả hai cảm biến đều phải có các giới hạn được khai báo chính xác ([Hồ sơ giới hạn &amp; Phạm vi hiệu chuẩn](caps-and-range.md)).

### TừPython



Không có lệnh dành riêng cho cảm biến kép trong giao diệnSDK

: hãy mở hai phiên làm việc với `chloros_sdk.connect_daq_sensor()` và tự so sánh các phổ của chúng với `latest()`, áp dụng cùng quy ước đặt tên. (Một công cụ ghi dữ liệu cảm biến kép cũng tồn tại trên giao diện phần cứng trực tiếp nội bộ củaMAPIR

, được liệt kê trong [CLI

Tài liệu tham khảo](../reference/cli-reference.md) để đảm bảo tính đầy đủ — nó không thuộc phần mềmCLI

được cung cấp; quy trình làm việc trên giao diện người dùng (GUI) ở trên là lộ trình hoạt động được hỗ trợ.)
