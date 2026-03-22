# Cài đặt dự án

Thanh cài đặt dự án <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> trong Chloros> cho phép bạn định cấu hình tất cả các cạnh của bộ xử lý hình ảnh, phát hiện hiệu chỉnh tiêu điểm mục, tính toán chỉ mục đa và các tùy chọn xuất ra cho dự án của bạn. Cài đặt này được lưu cùng với dự án của bạn và có thể được lưu dưới dạng mẫu để sử dụng lại trên nhiều dự án.

## Truy cập dự án cài đặt

Để truy cập cài đặt dự án:

1. Mở một dự án trong Chloros
2. Click vào tab **Cài đặt dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> ở thanh bên trái
3. Cài đặt bảng sẽ hiển thị tất cả các cấu hình tùy chọn có sẵn được sắp xếp theo danh mục

***

## Phát hiện tiêu điểm

Kiểm soát cài đặt này Cách Chloros phát hiện và xử lý các chỉnh sửa tiêu điểm trong hình ảnh của bạn.

### Tối thiểu chuẩn mực mẫu vẽ (px)

* **Loại**: Số
* **Phạm vi**: 0 đến 10.000 pixel
* **Mặc định**: 25 pixel
* **Mô tả**: Cần thiết phải đặt mức tối thiểu diện tích (pixel thuộc tính) để vùng được phát hiện là hợp lệ chuẩn hiệu tiêu chuẩn mẫu. Giá trị nhỏ hơn sẽ phát hiện các mục tiêu nhỏ hơn nhưng có thể làm tăng kết quả dương tính. Giá trị lớn hơn yêu cầu tiêu điểm vùng lớn hơn, rõ ràng hơn để phát hiện.
* **Điều chỉnh thời điểm**:
  * Tăng nếu bạn nhận thấy sai sót trên các hình ảnh tạo tác vụ nhỏ
  * Giảm nếu hiệu chỉnh tiêu điểm của bạn xuất hiện nhỏ trong hình ảnh của bạn và không được phát hiện

### Tối thiểu phân cụm mục tiêu (0-100)

* **Loại**: Số
* **Phạm vi**: 0 đến 100
* **Mặc định**: 60
* **Mô tả**: Kiểm soát tần số phân cụm để nhóm các vùng có màu tương tự nhau khi phát hiện tiêu chuẩn tín hiệu. Giá trị cao hơn yêu cầu nhiều màu tương thích hơn được nhóm lại với nhau, dẫn đến khả năng phát hiện mục tiêu cẩn thận hơn. Giá trị thấp hơn cho phép nhiều biến đổi màu sắc hơn trong tiêu điểm nhóm.
* **Điều chỉnh thời điểm**:
  * Tăng cường nếu tiêu chuẩn mục tiêu được phát hiện thành nhiều lần
  * Giảm nếu việc chỉnh sửa tiêu điểm có biến màu có thể không được phát hiện đầy đủ

***

## Xử lý

Kiểm soát cài đặt này Kiểm soát cách xử lý Chloros và hiệu chỉnh hình ảnh của bạn.

### Chỉnh sửa chi tiết

* **Loại**: Hộp kiểm tra
* **Mặc định**: Đã bật (đã chọn)
* **Mô tả**: Áp dụng hiệu chỉnh chi tiết để bù đắp độ tối của ống kính ở các cạnh của hình ảnh. Họa tiết là một hiện tượng quang học phổ biến trong đó các góc và cạnh của hình ảnh có vẻ tối hơn phần trung tâm do đặc điểm của ống kính.
* **Thời gian tắt**: Chỉ tắt nếu bạn đã áp dụng tính năng chỉnh sửa chi tiết hoặc nếu bạn muốn chỉnh sửa chi tiết theo cách thủ công trong quá trình xử lý hậu kỳ.

### Hiệu chỉnh chế độ phản xạ / cân bằng trắng

* **Loại**: Hộp kiểm tra
* **Mặc định**: Đã bật (đã chọn)
* **Mô tả**: Cho phép tự động điều chỉnh phản xạ bằng cách sử dụng các hiệu chỉnh tiêu điểm được phát hiện trong hình ảnh của bạn. Điều này thường hóa các giá trị phản xạ trên dữ liệu của bạn và đảm bảo các thước đo được phép nhất bất chấp điều kiện ánh sáng.
* **Thời điểm tắt**: Chỉ tắt nếu bạn muốn xử lý raw image, chưa được chỉnh sửa hoặc nếu bạn đang sử dụng trình chỉnh sửa quy trình khác.

### Debayer phương pháp

* **Loại bỏ**: Lựa chọn thả xuống
* **Tùy chọn**:
  * Tiêu chuẩn (Nhanh, Chất lượng trung bình)
  * Nhận cấu hình (Chậm, Chất lượng cao nhất) \[Chloros+]
* **Mặc định**: Tiêu chuẩn (Nhanh, Chất lượng trung bình)
* **Mô tả**: Chọn thuật toán khử màu được sử dụng để chuyển đổi nguyên liệu thô mẫu cảm biến dữ liệu của Bayer thành hình ảnh đầy đủ màu sắc. Phương pháp "Tiêu chuẩn (Nhanh, Chất lượng Trung bình)" mang lại sự cân bằng tối ưu giữa tốc độ xử lý và hình ảnh chất lượng. "Phần mềm nhận biết cấu hình (Chậm, Chất lượng cao nhất)" \[Chloros+] sử dụng trình gỡ lỗi biết chất lượng cao kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ gây nhiễu ồn. Mô hình Nhận biết GPU bộ nhớ (VRAM) yêu cầu cấu hình để chạy. Chúng tôi khuyên bạn nên sử dụng nó khi bạn có sẵn >4GB VRAM để xử lý nhanh hơn.
* **Lưu ý**: Các plugin gỡ lỗi phương pháp có thể được thêm vào trong các phiên bản tương thích của Chloros.

### Tối thiểu hóa hiệu chỉnh thời gian khoảng cách

* **Loại**: Số
* **Phạm vi**: 0 đến 3.600 giây
* **Mặc định**: 0 giây
* **Mô tả**: Đặt khoảng thời gian tối thiểu (tính bằng giây) giữa các lần sử dụng tiêu chuẩn hiệu mục. Khi được đặt thành 0, Chloros sẽ sử dụng mọi tiêu chuẩn tiêu chuẩn được phát hiện. Khi được đặt thành giá trị cao hơn, Chloros sẽ chỉ sử dụng cách chuẩn hóa các mục tiêu ít nhất nhiều giây, giúp giảm thời gian xử lý các dữ liệu một cách thường xuyên.
* **Điều chỉnh thời điểm**:
  * Đặt thành 0 để có mức tối đa chuẩn xác chính xác khi thay đổi ánh sáng
  * Tăng (ví dụ: tăng 60-300 giây) để xử lý nhanh hơn khi ánh sáng tối nhất quán và bạn thường xuyên hiệu chỉnh hình ảnh mục tiêu

### Giờ độ lệch của ánh sáng biến

* **Loại**: Số
* **Phạm vi**: -12 đến +12 giờ
* **Mặc định**: 0 giờ
* **Mô tả**: Chỉ định độ lệch múi giờ (tính bằng giờ tính từ UTC) cho dấu thời gian dữ liệu cảm biến ánh sáng. Điều này được sử dụng khi xử lý PPK dữ liệu tệp (Động học sau quá trình xử lý) để đảm bảo đồng bộ hóa thời gian chính xác giữa ảnh chụp và GPS dữ liệu.
* **Điều chỉnh thời điểm**: Đặt giá trị này thành chênh lệch múi giờ địa phương nếu PPK dữ liệu của bạn sử dụng địa phương giờ thay vì UTC. Ví dụ:
  * Time Thái Bình Dương: -8 hoặc -7 (theo DST)
  * Miền Đông: -5 hoặc -4 (theo DST)
  * Time Trung Âu: +1 hoặc +2 (theo DST)

### Áp dụng PPK hiệu chỉnh

* **Loại**: Hộp kiểm tra
* **Mặc định**: Đã tắt (không thể chọn)
* **Mô tả**: Được phép sử dụng bộ xử lý Động học hiệu chỉnh (PPK) từ máy ghi MAPIR DAQ có chứa GPS (GNSS). Khi được bật, Chloros sẽ sử dụng mọi bản cập nhật tệp .daq chứa phơi sáng dữ liệu trong dự án thư mục của bạn và áp dụng các chỉnh sửa vị trí chính xác cho hình ảnh của bạn.
* **Yêu cầu**: Tệp nhật ký .daq có các mục phơi sáng phải có trong dự án thư mục của bạn
* **Thời điểm bật**: Bạn nên luôn bật tính năng sửa PPK nếu bạn có các mục phản hồi phơi sáng trong bản ghi tệp .daq của mình.

### Chân phơi sáng 1

* **Loại bỏ**: Lựa chọn thả xuống
* **Chế độ hiển thị**: Chỉ hiển thị khi bật "Áp dụng PPK hiệu chỉnh" Và phơi sáng dữ liệu có sẵn cho Ghim 1
* **Tùy chọn**:
  * Máy ảnh mẫu tên được phát hiện trong dự án
  * "Không sử dụng" - Bỏ qua pin phơi sáng này
* **Mặc định**: Được chọn tự động dựa trên cấu hình dự án
* **Mô tả**: Chỉ định một công cụ camera có thể chọn Độ phơi sáng 1 để đồng bộ hóa PPK thời gian. Hãy phơi sáng lại thời gian chính xác khi màn hình thu được kích hoạt, điều này rất quan trọng để xác định vị trí chính xác của PPK địa chỉ.
* **Hành vi tự động lựa chọn**:
  * Camera đơn + chốt đơn: Tự động chọn camera
  * Camera đơn + hai chân: Chân 1 tự động được phân bổ cho camera
  * Nhiều camera: Yêu cầu lựa chọn thủ công

### Chân phơi sáng 2

* **Loại bỏ**: Lựa chọn thả xuống
* **Chế độ hiển thị**: Chỉ hiển thị khi "Áp dụng PPK hiệu chỉnh" được bật và sẵn sàng phơi sáng dữ liệu cho Chân 2
* **Tùy chọn**:
  * Máy ảnh mẫu tên được phát hiện trong dự án
  * "Không sử dụng" - Bỏ qua pin phơi sáng này
* **Mặc định**: Được chọn tự động dựa trên cấu hình dự án
* **Mô tả**: Chỉ định một công cụ camera cho Chân phơi sáng 2 để đồng bộ hóa PPK thời gian khi sử dụng camera kép thiết lập.
* **Hành vi tự động lựa chọn**:
  * Camera menu + chốt đơn: Chân 2 tự động được cài đặt thành "Không sử dụng"
  * Camera đơn + hai chân: Chân 2 tự động cài đặt thành "Không sử dụng"
  * Nhiều camera: Yêu cầu lựa chọn thủ công
* **Lưu ý**: Không thể phân bổ cùng một camera cho cả Chân 1 và Chân 2 cùng một lúc.***

## Index

Cài đặt này cho phép bạn định cấu hình đa phổ chỉ để phân tích và trực tuyến hóa.

### Add only item

* **Loại**: Đặc biệt chỉ mục cấu hình bảng
* **Mô tả**: Mở bảng tương tác nơi bạn có thể chọn và định cấu hình các thực vật đa phổ chỉ (NDVI, NDRE, EVI, v.v.) để tính toán trong quá trình xử lý hình ảnh. Bạn có thể thêm nhiều chỉ mục, mỗi mục riêng biệt cài đặt cài đặt chỉ mục.
* **Các chỉ số có sẵn**: Hệ thống bao gồm hơn 30 chỉ số đa phổ được xác định trước bao bao:
  * NDVI (Chỉ số thực vật khác chuẩn hóa)
  * NDRE (Chênh lệch chuẩn hóa RedEdge)
  * EVI (Chỉ số thực vật tăng cường)
  * GNDVI, SAVI, OSAVI, MSAVI2
  * Và nhiều hơn nữa (xem [Công thức chỉ mục đa phổ] (multispectral-index-formulas.md) để biết danh sách đầy đủ)
* **Tính năng**:
  * Choose from the công thức được xác định chỉ định trước đó
  * Định cấu hình gradient màu trực quan (LUT - Bảng tra cứu)
  * Đặt ngưỡng giá trị để phân tích
  * Tạo tùy chỉnh chỉ mục công thức

### Tùy chỉnh công thức ( Tính năng Chloros+)

* **Loại**: Tùy chỉnh công thức định nghĩa
* **Mô tả**: Cho phép bạn tạo và lưu các công thức điều chỉnh đa tùy chọn đa số chỉ bằng cách sử dụng học toán. Tùy chỉnh công thức được lưu cùng với dự án cài đặt của bạn và có thể được sử dụng giống như các mục tích hợp sẵn.
* **Cách tạo**:
  1. Trong Index config table, hãy tìm tùy chọn tùy chọn công thức
  2. Xác định công thức của bạn bằng cách sử dụng số lượng băng tần nhận dạng (ví dụ: NIR, Red, Green, Blue)
  3. Save công thức với mô tả tên
* **Công thức pháp**: Các chuẩn toán được hỗ trợ, bao gồm:
  * Số học: `+`, `-`, `*`, `/`
  * Hiện tại dấu ngoặc đơn cho thứ tự
  * Tham chiếu băng tần: NIR, Red, Green, Blue, RedEdge, Cyan, Orange, NIR1, NIR2

***

## Xuất khẩu

Trình kiểm tra cài đặt này đã được xử lý Định dạng và chất lượng của hình ảnh đã được xuất.

### Hình ảnh định dạng đã được chỉnh sửa

* **Loại bỏ**: Lựa chọn thả xuống
* **Tùy chọn**:
  * **TIFF (16-bit)** - TIFF 16-bit dạng không nén
  * **TIFF (32-bit, Phần trăm)** - TIFF comcomoma 32-bit với các giá trị phản xạ dưới dạng phần trăm
  * **PNG (8-bit)** - PNG 8 bit được nén
  * **JPG (8-bit)** - Định dạng JPEG 8-bit được nén
* **Mặc định**: TIFF (16-bit)
* **Mô tả**: Chọn tệp định dạng để lưu hình ảnh đã được xử lý và hiệu chỉnh.
* **Khuyến nghị định dạng**:
  * **TIFF (16-bit)**: Khuyến nghị cho phân tích khoa học và quy trình làm việc chuyên nghiệp. Bảo đảm tối đa chất lượng dữ liệu mà không có biểu tượng nén. Tốt nhất cho phân tích đa phổ và xử lý sâu hơn trong phần mềm GIS.
  * **TIFF (32-bit, Phần trăm)**: Tốt nhất cho công việc quy trình yêu cầu giá trị phản xạ dưới dạng phần trăm (0-100%). Cung cấp tối đa độ chính xác cao cho các phép đo xạ được phép.
  * **PNG (8-bit)**: Tốt cho công việc xem web và hình dung chung. Tệp kích thước nhỏ hơn so với tính năng nén không bị mất dữ liệu nhưng phạm vi động giảm.
  * **JPG (8-bit)**: Kích thước tệp nhỏ nhất, chỉ phù hợp để xem trước và hiển thị trên web. Sử dụng tính năng nén phức tạp không phù hợp cho phân tích khoa học.***

## Lưu dự án mẫu

Tính năng này cho phép bạn lưu dự án cài đặt hiện tại của mình dưới dạng mẫu có thể sử dụng lại.

* **Loại**: Nhập văn bản + Nút lưu
* **Mô tả**: Nhập mô tả tên cho mẫu cài đặt của bạn và nhấp vào biểu tượng lưu. Sẽ lưu trữ tất cả hiện tại dự án cài đặt của bạn (hiện tiêu chuẩn, xử lý tùy chọn, mẫu chỉ mục và định dạng) để dễ dàng sử dụng lại trong các dự án trong tương lai.
* **Trường hợp sử dụng**:
  * Tạo mẫu cho các hệ thống camera khác nhau (RGB, multi phổ biến, NIR)
  * Lưu cấu hình tiêu chuẩn cho các loại cây trồng cụ thể hoặc quy trình phân tích
  * Chia sẻ cài đặt tối đa trong một nhóm
* **Cách sử dụng**:
  1. Định cấu hình tất cả cài đặt dự án mà bạn mong muốn
  2. Nhập mẫu tên (ví dụ: "RedEdge Survey3 NDVI Standard")
  3. Click vào biểu tượng lưu
  4. Mẫu hiện có thể được tải khi tạo dự án mới

***

## Lưu dự án thư mục

Cài đặt địa chỉ này để lưu dự án mới theo mặc định.

* **Loại**: Hiển thị thư mục đường dẫn + chỉnh sửa nút
* **Mặc định (Windows)**: `C:\Users\[Username]\Chloros Projects`
* **Mặc định (Linux)**: `~/.local/share/chloros/projects`
* **Mô tả**: Hiển thị mặc định thư mục hiện tại ở nơi dự án Chloros mới được tạo. Click vào biểu tượng chỉnh sửa để chọn một thư mục khác.
* **Khi nào cần thay đổi**:
  * Đặt mạng ổ đĩa thành công để cộng vào nhóm hoạt động
  * Thay đổi sang ổ đĩa có nhiều kho lưu trữ hơn cho các dữ liệu lớn
  * Sắp xếp các dự án theo năm, khách hàng hoặc loại dự án trong các thư mục khác nhau
* **Lưu ý**: Thay đổi cài đặt này chỉ ảnh hưởng đến các dự án MỚI. Dự án hiện tại vẫn ở vị trí ban đầu.***

## Cài đặt Kiên trì

Tất cả dự án cài đặt sẽ được lưu tự động cùng với dự án tệp của bạn (định dạng dự án `.mapir`). Khi bạn mở lại một dự án, tất cả các cài đặt sẽ được khôi phục chính xác như khi bạn rời khỏi.

### Cài đặt phân vùng

Các cài đặt được áp dụng theo thứ tự sau:

1. **Mặc định hệ thống** - Mặc định tích hợp được xác định bởi Chloros
2. **Cài đặt mẫu** - Nếu bạn tải mẫu khi tạo dự án
3. **Đã lưu cài đặt dự án** - Cài đặt được lưu cùng với dự án tệp
4. **Điều chỉnh thủ công** - Mọi thay đổi bạn thực hiện trong phiên bản hiện tại

### Cài đặt và xử lý ảnh

Hầu hết các cài đặt cài đặt (đặc biệt là trong danh mục Xử lý và Xuất) sẽ kích hoạt quá trình xử lý lại hình ảnh để xem xét mới cài đặt. Tuy nhiên, một số "xuất chỉ" cài đặt và không yêu cầu xử lý lại ngay lập tức:

* Lưu dự án mẫu
* Thư mục làm việc
* Chỉnh sửa ảnh dạng định dạng (áp dụng khi xuất)

***

## Các phương pháp hay nhất

1. **Bắt đầu với mặc định giá trị**: Cài đặt mặc định hoạt động tốt với hầu hết các hệ thống camera MAPIR và quy trình công việc thông thường.
2. **Tạo mẫu**: Sau khi bạn đã tối ưu hóa cài đặt cho công việc hoặc máy ảnh của quy trình, hãy lưu chúng dưới dạng mẫu để đảm bảo tính toán tốt nhất giữa các dự án.
3. **Kiểm tra trước khi xử lý hoàn toàn**: Khi thử nghiệm các cài đặt mới, hãy thử thử nghiệm trên một hình ảnh tập hợp nhỏ trước khi xử lý toàn bộ dữ liệu của bạn.
4. **Ghi lại cài đặt của bạn**: Sử dụng mô tả mẫu tên cho máy ảnh hệ thống, loại xử lý và mục tiêu sử dụng (ví dụ: "Survey3\_RGB\_NDVI\_Agriculture").
5. **Lựa chọn xuất định dạng định dạng**: Chọn xuất định dạng dựa trên mục tiêu sử dụng cuối cùng của bạn:
   * Phân tích khoa học → TIFF (16-bit hoặc 32-bit)
   * Xử lý GIS → TIFF (16-bit)
   * Hiển thị nhanh → PNG (8-bit)
   * Chia sẻ web → JPG (8-bit)

***

Để biết thêm thông tin về đa phổ chỉ mục trong Chloros, hãy xem trang [Công thức đa phổ chỉ mục](multispectral-index-formulas.md).