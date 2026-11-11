# Quy trình xử lý độ phản xạ

Một cảm biến ánh sáng DAQ chuyển đổi hình ảnh phóng xạ thành độ phản xạ. Có hai quy trình xử lý riêng biệt:

1. **Cảm biến đơn** — một cảm biến DAQ đo cường độ bức xạ chiếu xuống trong khi máy ảnh ghi hình, và phần mềm Chloros
chia độ rạng rỡ từ máy ảnh cho giá trị tham chiếu đó.
2. **Hai cảm biến** — hai cảm biến DAQ, một cảm biến quan sát bầu trời và một cảm biến quan sát vật thể, tạo ra đường cong phản xạ quang phổ trực tiếp mà không cần đến máy ảnh.

## Cảm biến đơn + máy ảnh (giá trị tham chiếu bức xạ xuống)

Cảm biến DAQ đóng vai trò là cảm biến ánh sáng chiếu xuống (DLS): máy ảnh đo độ rạng lên **L**(W/m²/sr/nm), DAQ đo cường độ bức xạ chiếu xuống**E** (W/m²/nm), và Chloros
tính toán độ phản xạ theo từng dải phổ như sau:

> ρ = π · L / E

Giá trị đo của DAQ luôn **được đồng bộ hóa theo dấu thời gian với thời gian phơi sáng** — đây là lý do tại sao DAQ và các camera chia sẻ một đồng hồ được đồng bộ hóa theo PTP (xem [Mạng và Đồng bộ hóa Thời gian DAQ-E](ethernet-ptp.md)). Hãy lắp mũ cosine Sunshine khi làm việc ngoài trời và khai báo chính xác; việc khai báo mũ này sẽ trực tiếp điều chỉnh giá trị E (xem [Hồ sơ mũ &amp; Phạm vi hiệu chuẩn](caps-and-range.md)). Đối với công việc định lượng, hãy nhớ đặc tính của thiết bị: bức xạ định lượng được tính từ giá trị trung bình của ít nhất 15 giây đo.

### Ghi hình trực tiếp

Kết nối DAQ với máy ảnh trong tab &quot;Cameras&quot;: bảng cài đặt của mỗi máy ảnh có menu thả xuống **Light Sensor** liệt kê tất cả các DAQ đã kết nối (DAQ-U/M/E) từ tab &quot;Light Sensors&quot;; đối với mảng đồng bộ, việc chọn cảm biến ánh sáng cho toàn mảng sẽ được áp dụng cho từng thành viên (các máy ảnh riêng lẻ vẫn có thể ghi đè). Sau khi được liên kết, phổ của cảm biến sẽ được đưa vào khe DLS của máy ảnh và các giá trị phản xạ xuất ra sẽ được chia cho giá trị đo phù hợp. Trong một mảng, một thành viên có cảm biến riêng sẽ sử dụng giá trị đo của chính nó — cho các dải phổ trong hình ảnh tổng hợp trực tiếp, các dữ liệu xuất ra và chế độ phơi sáng tự động — trong khi các thành viên khác sử dụng giá trị của mảng.

<!-- SCREENSHOT-NEEDED: Cameras tab per-camera settings panel showing the "Light Sensor" dropdown open, with a connected DAQ sensor listed and selected. -->


Hai hành vi cần lưu ý:

* **Không có DAQ được liên kết → dữ liệu phản xạ bị từ chối, không bị giả mạo.** Chloros
từ chối sản phẩm phản xạ và ghi lại lý do bỏ qua thay vì im lặng trả về một sản phẩm kém chất lượng.
* **Giá trị đo được sử dụng sẽ được giữ nguyên.** Đối với mỗi khung hình phản xạ, giá trị đo DAQ thực sự được áp dụng sẽ được ghi dưới dạng tệp `.daq` trong thư mục `daq/` của các bản ghi, do đó bản ghi có thể được xử lý lại sau này ([Ghi hình &amp; Định dạng .daq](recording.md)). Một tệp `.daq.cameras.json` bên cạnh nó sẽ chỉ định tên các camera mà nó mô tả, và quá trình xử lý lại sẽ chỉ khớp tệp này với hình ảnh của chính các camera đó.

### Xử lý hình ảnh đã ghi

Để xử lý sau chuyến bay, hãy ghi lại tệp `.daq` trong suốt phiên ghi và lưu nó cùng với hình ảnh — quy trình xử lý sẽ tự động khớp các dữ liệu hướng xuống theo dấu thời gian, đồng thời tải về bất kỳ dữ liệu hiệu chuẩn nhà máy nào còn thiếu từ đám mây củMAPIR
khi sử dụng lần đầu. Các bản ghi giao diện người dùng (GUI) sẽ được tự động thêm vào dự án đang mở khi quá trình ghi kết thúc.

Tham chiếu phản xạ có thể được chọn tại thời điểm xử lý — `--reflectance-source` trên `chloros-cli process`, hoặc cài đặt nguồn phản xạ trong phần Cài đặt Dự án của GUI:

| Giá trị | Hành vi |
| --- | --- |
| `auto` (mặc định) | Mục tiêu hiệu chuẩn trong khung hình đã vượt qua kiểm tra chất lượng (QA) là tham chiếu tuyệt đối; tín hiệu xuống từ DAQ (ρ = π·L/E) được sử dụng làm phương án dự phòng. Một mục tiêu vượt qua kiểm tra chất lượng (QA) nhưng có cờ báo lỗi (một bảng bị loại bỏ hoặc một dải tần bị lỗi) sẽ không tự động được ưu tiên: các chuỗi mục tiêu, mục tiêu + DAQ và chỉ DAQ sẽ được thử theo thứ tự điểm số đo được trên các bảng được giữ lại. |
| `daq` | DAQ có thẩm quyền |
| `target` | Mục tiêu nghiêm ngặt trong khung; không thay thế bằng DAQ |

Xem [Mục tiêu hiệu chuẩn](../calibration-targets.md) để biết quy trình làm việc với mục tiêu và [chương LATTICE](../lattice/README.md) cùng với [Tài liệu tham khảo CLI
](../reference/cli-reference.md) để biết toàn bộ quy trình xử lý. Khi đọc các pixel phản xạ đã xuất, hãy sử dụng thang đo được ghi (LATTICE: 32768 = ρ 1.0, XMP `Chloros:PixelScale`; Survey3
: 65535) — xem [Định dạng hình ảnh đầu ra](../output-image-formats.md).

### Các dải nằm ngoài phạm vi hiệu chuẩn của DAQ

Phạm vi hiệu chuẩn quang phổ của DAQ là ~374–974 nm. Chloros
sẽ từ chối dữ liệu phản xạ từ DAQ đối với bất kỳ dải phổ nào của camera có ít hơn một nửa trọng số phổ nằm trong khoảng đó, và báo cáo lý do bỏ qua là `dls-uncalibrated-band-<nm>`. Trong số các SKU đang được phân phối, điều này chỉ ảnh hưởng đến F988: giá trị phản xạ của F988 được hiệu chuẩn bằng bảng phản xạ trong cảnh; dải phổ này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros
sẽ áp dụng dữ liệu chụp bảng phản xạ gần nhất của bạn và duy trì giá trị đó giữa các lần đo bảng. Nếu camera F988 chỉ chạy chế độ DAQ, Chloros
sẽ từ chối giá trị phản xạ dựa trên DAQ cho dải đó với lý do bỏ qua là `dls-uncalibrated-band-988` — quy trình làm việc với bảng đo là phương án được hỗ trợ.

## Cảm biến kép (ánh sáng xung quanh + đối tượng)

Hai cảm biến DAQ — bất kỳ cặp nào, trên bất kỳ phương tiện truyền tải nào — cung cấp phổ phản xạ trực tiếp mà không cần camera: một cảm biến hướng lên trời (**Nguồn ánh sáng xung quanh**), một cảm biến hướng vào đối tượng (**Máy quét đối tượng**), và Chloros
tính toán theo từng bước sóng:

> R(λ) = đối tượng(λ) / ánh sáng xung quanh(λ)

(bằng 0 khi ánh sáng xung quanh ≤ 0).

### Trong giao diện người dùng (GUI)

Khi cả hai cảm biến đã được kết nối trong tab Cảm biến Ánh sáng, hãy mở cửa sổ thêm cảm biến (nút &quot;+&quot; trên ô biểu đồ trong chế độ xem lưới) và chọn **Kết hợp Ánh sáng Môi trường + Đối tượng**. Chọn hai cảm biến trong danh sách thả xuống Nguồn ánh sáng xung quanh và Máy quét đối tượng, sau đó nhấp vào Tạo. Nhóm này sẽ xuất hiện dưới dạng biểu đồ riêng và dưới dạng một hàng trên thanh bên với huy hiệu màu xanh lá cây**REF**.

<!-- SCREENSHOT-NEEDED: The add-sensor overlay's "Combine Ambient + Object" panel with two connected DAQ sensors selected in the Ambient Light Source and Object Scanner dropdowns, Create button enabled. -->

<!-- SCREENSHOT-NEEDED: A live Apparent Reflectance chart from an Ambient+Object DAQ pair in list view, with the vegetation-index table visible below the chart (NDVI etc. showing live values). -->


Dưới biểu đồ độ phản xạ (chế độ xem danh sách), một **bảng chỉ số thực vật** thời gian thực tính toán các chỉ số từ đường cong bằng cách sử dụng các tần số trung tâm dải sóng: xanh dương 450 / xanh lá 550 / đỏ 670 / NIR
800 nm. Các chỉ số dựa trên tỷ lệ loại bỏ thang đo tuyệt đối (NDVI
, GNDVI
, ENDVI, WDRVI
, GRVI
, CVI
, GCI
, MSR) luôn được hiển thị; các chỉ số cần độ phản xạ tuyệt đối (EVI
, SAVI
, OSAVI
, GSAVI, GOSAVI, MSAVI2, RDVI, TDVI, LAI
, NLI, MNLI, FCI, GEMI
) chỉ xuất hiện khi cả hai cảm biến đều là các mẫu đã được hiệu chuẩn công suất.

### Hệ số phản xạ biểu kiến so với hệ số phản xạ tương đối — quy tắc đặt nhãn

Chloros
đặt nhãn cho đầu ra của cặp cảm biến dựa trên những gì cặp cảm biến đó thực sự có thể khẳng định:

| Cặp cảm biến | Nhãn |
| --- | --- |
| Cả hai cảm biến đều đã được hiệu chuẩn — gói hiệu chuẩn nhà máy đã được tải | **Hệ số phản xạ biểu kiến** |
| Một trong hai cảm biến chưa được hiệu chuẩn | **Độ phản xạ tương đối** |

Cả ba mô hình đều là mô hình phóng xạ: một khi gói hiệu chuẩn nhà máy của cảm biến được tải, phổ của nó sẽ là giá trị tuyệt đối W/m²/nm, do đó tỷ lệ giữa hai cảm biến đã hiệu chuẩn sẽ cho ra độ phản xạ biểu kiến tuyệt đối — quá trình chuyển đổi không quyết định điều này. Một cảm biến vẫn đang truyền dữ liệu thô (không truy cập được gói hiệu chuẩn) sẽ làm kết quả trở thành đường cong tương đối (hình dạng phổ vẫn hợp lệ). Cả hai cảm biến đều phải có các giới hạn được khai báo chính xác ([Hồ sơ giới hạn &amp; Phạm vi hiệu chuẩn](caps-and-range.md)).

### Từ Python


Không có hàm gọi chuyên dụng cho cảm biến kép trong giao diện SDK
chung: hãy mở hai phiên làm việc với `chloros_sdk.connect_daq_sensor()` và tự tính tỷ lệ các phổ của chúng với `latest()`, áp dụng cùng quy ước đặt tên. (Một công cụ ghi dữ liệu cảm biến kép cũng tồn tại trên giao diện phần cứng trực tiếp nội bộ của MAPIR
, được liệt kê trong [Tài liệu tham khảo CLI
](../reference/cli-reference.md) để đảm bảo tính đầy đủ — công cụ này không nằm trong phiên bản CLI
được phát hành; quy trình làm việc qua giao diện người dùng (GUI) ở trên là phương thức được hỗ trợ chính thức.)
