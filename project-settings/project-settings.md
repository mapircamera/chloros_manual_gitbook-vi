# Cài đặt dự án

Thanh bên Cài đặt dự án <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> trong Chloros cho phép bạn định cấu hình tất cả các khía cạnh của xử lý hình ảnh, phát hiện mục tiêu hiệu chuẩn, tính toán chỉ mục đa phổ và các tùy chọn xuất cho dự án của bạn. Các cài đặt này được lưu cùng với dự án của bạn và có thể được lưu dưới dạng mẫu để sử dụng lại trên nhiều dự án.

## Truy cập cài đặt dự án

Để truy cập Cài đặt dự án:

1. Mở một dự án bằng Chloros
2. Nhấp vào tab **Cài đặt dự án** <img src="../.gitbook/assets/icon_project-settings.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng cài đặt sẽ hiển thị tất cả các tùy chọn cấu hình có sẵn được sắp xếp theo danh mục

***

## Phát hiện mục tiêu

Các cài đặt này kiểm soát cách Chloros phát hiện và xử lý các mục tiêu hiệu chỉnh trong hình ảnh của bạn.

### Diện tích mẫu hiệu chuẩn tối thiểu (px)

* **Loại**: Số
* **Phạm vi**: 0 đến 10.000 pixel
* **Mặc định**: 25 pixel
* **Mô tả**: Đặt diện tích tối thiểu (tính bằng pixel) cần thiết để vùng được phát hiện được coi là mẫu mục tiêu hiệu chuẩn hợp lệ. Giá trị nhỏ hơn sẽ phát hiện các mục tiêu nhỏ hơn nhưng có thể làm tăng kết quả dương tính giả. Giá trị lớn hơn yêu cầu vùng mục tiêu lớn hơn, rõ ràng hơn để phát hiện.
* **Thời điểm điều chỉnh**:
  * Tăng nếu bạn nhận được phát hiện sai trên các tạo tác hình ảnh nhỏ
  * Giảm nếu mục tiêu hiệu chỉnh của bạn xuất hiện nhỏ trong hình ảnh của bạn và không bị phát hiện

### Phân cụm mục tiêu tối thiểu (0-100)

* **Loại**: Số
* **Phạm vi**: 0 đến 100
* **Mặc định**: 60
* **Mô tả**: Kiểm soát ngưỡng phân cụm để nhóm các vùng có màu tương tự nhau khi phát hiện mục tiêu hiệu chuẩn. Giá trị cao hơn yêu cầu nhiều màu tương tự hơn được nhóm lại với nhau, dẫn đến khả năng phát hiện mục tiêu thận trọng hơn. Giá trị thấp hơn cho phép biến đổi màu sắc nhiều hơn trong nhóm mục tiêu.
* **Thời điểm điều chỉnh**:
  * Tăng nếu mục tiêu hiệu chuẩn được chia thành nhiều lần phát hiện
  * Giảm nếu mục tiêu hiệu chỉnh có biến thể màu không được phát hiện đầy đủ

***

## Xử lý

Các cài đặt này kiểm soát cách Chloros xử lý và hiệu chỉnh hình ảnh của bạn.

### Chỉnh sửa họa tiết

* **Loại**: Hộp kiểm
* **Mặc định**: Đã bật (đã chọn)
* **Mô tả**: Áp dụng hiệu chỉnh họa tiết để bù cho độ tối của ống kính ở các cạnh của hình ảnh. Họa tiết là một hiện tượng quang học phổ biến trong đó các góc và cạnh của hình ảnh có vẻ tối hơn phần trung tâm do đặc điểm của ống kính.
* **Thời điểm tắt**: Chỉ tắt nếu tổ hợp máy ảnh/ống kính của bạn đã áp dụng tính năng chỉnh sửa họa tiết hoặc nếu bạn muốn sửa họa tiết theo cách thủ công trong quá trình xử lý hậu kỳ.

### Hiệu chỉnh độ phản xạ / cân bằng trắng

* **Loại**: Hộp kiểm
* **Mặc định**: Đã bật (đã chọn)
* **Mô tả**: Cho phép hiệu chỉnh phản xạ tự động bằng cách sử dụng các mục tiêu hiệu chỉnh được phát hiện trong hình ảnh của bạn. Điều này bình thường hóa các giá trị phản xạ trên tập dữ liệu của bạn và đảm bảo các phép đo nhất quán bất kể điều kiện ánh sáng.
* **Thời điểm tắt**: Chỉ tắt nếu bạn muốn xử lý hình ảnh thô, chưa được hiệu chỉnh hoặc nếu bạn đang sử dụng quy trình hiệu chỉnh khác.

### Phương pháp Debayer

* **Loại**: Lựa chọn thả xuống
* **Tùy chọn**:
  * Chất lượng cao (Nhanh hơn) - Hiện tại là tùy chọn duy nhất có sẵn
* **Mặc định**: Chất lượng cao (Nhanh hơn)
* **Mô tả**: Chọn thuật toán khử màu được sử dụng để chuyển đổi dữ liệu cảm biến mẫu thô của Bayer thành hình ảnh đầy đủ màu sắc. Phương pháp "Chất lượng cao (Nhanh hơn)" mang lại sự cân bằng tối ưu giữa tốc độ xử lý và chất lượng hình ảnh.
* **Lưu ý**: Các phương pháp gỡ lỗi bổ sung có thể được thêm vào trong các phiên bản tương lai của Chloros.

### Khoảng thời gian hiệu chuẩn lại tối thiểu

* **Loại**: Số
* **Phạm vi**: 0 đến 3.600 giây
* **Mặc định**: 0 giây
* **Mô tả**: Đặt khoảng thời gian tối thiểu (tính bằng giây) giữa các lần sử dụng mục tiêu hiệu chuẩn. Khi được đặt thành 0, Chloros sẽ sử dụng mọi mục tiêu hiệu chuẩn được phát hiện. Khi được đặt thành giá trị cao hơn, Chloros sẽ chỉ sử dụng các mục tiêu hiệu chuẩn cách nhau ít nhất nhiều giây, giảm thời gian xử lý cho các tập dữ liệu thường xuyên chụp mục tiêu hiệu chuẩn.
* **Thời điểm điều chỉnh**:
  * Đặt thành 0 để có độ chính xác hiệu chuẩn tối đa khi điều kiện ánh sáng thay đổi
  * Tăng (ví dụ: lên 60-300 giây) để xử lý nhanh hơn khi ánh sáng nhất quán và bạn thường xuyên hiệu chỉnh hình ảnh mục tiêu

### Độ lệch múi giờ của cảm biến ánh sáng

* **Loại**: Số
* **Phạm vi**: -12 đến +12 giờ
* **Mặc định**: 0 giờ
* **Mô tả**: Chỉ định độ lệch múi giờ (tính bằng giờ tính từ UTC) cho dấu thời gian dữ liệu cảm biến ánh sáng. Điều này được sử dụng khi xử lý các tệp dữ liệu PPK (Động học sau xử lý) để đảm bảo đồng bộ hóa thời gian chính xác giữa ảnh chụp và dữ liệu GPS.
* **Thời điểm điều chỉnh**: Đặt giá trị này thành chênh lệch múi giờ địa phương nếu dữ liệu PPK của bạn sử dụng giờ địa phương thay vì UTC. Ví dụ:
  * Giờ Thái Bình Dương: -8 hoặc -7 (tùy theo DST)
  * Giờ miền Đông: -5 hoặc -4 (tùy theo DST)
  * Giờ Trung Âu: +1 hoặc +2 (tùy theo DST)

### Áp dụng hiệu chỉnh PPK

* **Loại**: Hộp kiểm
* **Mặc định**: Đã tắt (không được chọn)
* **Mô tả**: Cho phép sử dụng các hiệu chỉnh Động học sau xử lý (PPK) từ máy ghi MAPIR DAQ có chứa GPS (GNSS). Khi được bật, Chloros sẽ sử dụng mọi tệp nhật ký .daq chứa dữ liệu ghim phơi sáng trong thư mục dự án của bạn và áp dụng các chỉnh sửa vị trí địa lý chính xác cho hình ảnh của bạn.
* **Yêu cầu**: Tệp nhật ký .daq có các mục ghim phơi sáng phải có trong thư mục dự án của bạn
* **Thời điểm bật**: Bạn nên luôn bật tính năng sửa PPK nếu bạn có các mục phản hồi phơi sáng trong tệp nhật ký .daq của mình.

### Chân phơi sáng 1

* **Loại**: Lựa chọn thả xuống
* **Chế độ hiển thị**: Chỉ hiển thị khi bật "Áp dụng hiệu chỉnh PPK" VÀ dữ liệu phơi sáng có sẵn cho Ghim 1
* **Tùy chọn**:
  * Tên mẫu máy ảnh được phát hiện trong dự án
  * "Không sử dụng" - Bỏ qua pin phơi sáng này
* **Mặc định**: Được chọn tự động dựa trên cấu hình dự án
* **Mô tả**: Chỉ định một camera cụ thể cho Chân phơi sáng 1 để đồng bộ hóa thời gian PPK. Chốt phơi sáng ghi lại thời gian chính xác khi màn trập camera được kích hoạt, điều này rất quan trọng để định vị địa lý PPK chính xác.
* **Hành vi tự động lựa chọn**:
  * Camera đơn + chốt đơn: Tự động chọn camera
  * Camera đơn + hai chân: Chân 1 tự động được gán cho camera
  * Nhiều camera: Yêu cầu lựa chọn thủ công

### Chân phơi sáng 2

* **Loại**: Lựa chọn thả xuống
* **Chế độ hiển thị**: Chỉ hiển thị khi "Áp dụng hiệu chỉnh PPK" được bật VÀ dữ liệu phơi sáng có sẵn cho Chân 2
* **Tùy chọn**:
  * Tên mẫu máy ảnh được phát hiện trong dự án
  * "Không sử dụng" - Bỏ qua pin phơi sáng này
* **Mặc định**: Được chọn tự động dựa trên cấu hình dự án
* **Mô tả**: Chỉ định một camera cụ thể cho Chân phơi sáng 2 để đồng bộ hóa thời gian PPK khi sử dụng thiết lập camera kép.
* **Hành vi tự động lựa chọn**:
  * Camera đơn + chốt đơn: Chân 2 tự động được đặt thành "Không sử dụng"
  * Camera đơn + hai chân: Chân 2 tự động được đặt thành "Không sử dụng"
  * Nhiều camera: Yêu cầu lựa chọn thủ công
* **Lưu ý**: Không thể gán cùng một camera cho cả Chân 1 và Chân 2 cùng một lúc.

***

## Chỉ mục

Các cài đặt này cho phép bạn định cấu hình các chỉ số đa phổ để phân tích và trực quan hóa.

### Thêm chỉ mục

* **Loại**: Bảng cấu hình chỉ mục đặc biệt
* **Mô tả**: Mở bảng tương tác nơi bạn có thể chọn và định cấu hình các chỉ số thực vật đa phổ (NDVI, NDRE, EVI, v.v.) để tính toán trong quá trình xử lý hình ảnh. Bạn có thể thêm nhiều chỉ mục, mỗi chỉ mục có cài đặt hiển thị riêng.
* **Các chỉ số có sẵn**: Hệ thống bao gồm hơn 30 chỉ số đa phổ được xác định trước bao gồm:
  * NDVI (Chỉ số thực vật khác biệt chuẩn hóa)
  * NDRE (RedEdge chênh lệch chuẩn hóa)
  * EVI (Chỉ số thực vật tăng cường)
  * GNDVI, SAVI, OSAVI, MSAVI2
  * Và nhiều hơn nữa (xem [Công thức chỉ số đa phổ](multispectral-index-formulas.md) để biết danh sách đầy đủ)
* **Đặc trưng**:
  * Chọn từ các công thức chỉ số được xác định trước
  * Định cấu hình độ dốc màu trực quan (LUT - Bảng tra cứu)
  * Đặt giá trị ngưỡng để phân tích
  * Tạo công thức chỉ mục tùy chỉnh

### Công thức tùy chỉnh (Tính năng Cloros+)

* **Loại**: Mảng định nghĩa công thức tùy chỉnh
* **Mô tả**: Cho phép bạn tạo và lưu các công thức chỉ số đa phổ tùy chỉnh bằng cách sử dụng toán học. Các công thức tùy chỉnh được lưu cùng với cài đặt dự án của bạn và có thể được sử dụng giống như các chỉ mục tích hợp sẵn.
* **Cách tạo**:
  1. Trong bảng cấu hình Chỉ mục, hãy tìm tùy chọn công thức tùy chỉnh
  2. Xác định công thức của bạn bằng cách sử dụng mã định danh băng tần (ví dụ: NIR, Red, Green, Blue)
  3. Lưu công thức với tên mô tả
* **Cú pháp công thức**: Các phép toán tiêu chuẩn được hỗ trợ, bao gồm:
  * Số học: `+`, `-`, `*`, `/`
  * Dấu ngoặc đơn cho thứ tự thực hiện
  * Tham chiếu băng tần: NIR, Red, Green, Blue, RedEdge, Cyan, Orange, NIR1, NIR2

***

## Xuất khẩu

Các cài đặt này kiểm soát định dạng và chất lượng của hình ảnh đã xử lý được xuất.

### Định dạng hình ảnh đã được hiệu chỉnh

* **Loại**: Lựa chọn thả xuống
* **Tùy chọn**:
  * **TIFF (16-bit)** - Định dạng TIFF 16-bit không nén
  * **TIFF (32-bit, Percent)** - TIFF dấu phẩy động 32-bit với các giá trị phản xạ dưới dạng phần trăm
  * **PNG (8-bit)** - Định dạng PNG 8-bit được nén
  * **JPG (8-bit)** - Định dạng nén JPEG 8-bit
* **Mặc định**: TIFF (16-bit)
* **Mô tả**: Chọn định dạng tệp để lưu hình ảnh đã được xử lý và hiệu chỉnh.
* **Khuyến nghị về định dạng**:
  * **TIFF (16-bit)**: Được khuyến nghị cho phân tích khoa học và quy trình làm việc chuyên nghiệp. Bảo toàn chất lượng dữ liệu tối đa mà không có hiện tượng nén. Tốt nhất cho phân tích đa phổ và xử lý sâu hơn trong phần mềm GIS.
  * **TIFF (32-bit, Percent)**: Tốt nhất cho các quy trình công việc yêu cầu giá trị phản xạ dưới dạng phần trăm (0-100%). Cung cấp độ chính xác tối đa cho các phép đo phóng xạ.
  * **PNG (8-bit)**: Phù hợp để xem trên web và hiển thị tổng thể. Kích thước tệp nhỏ hơn với tính năng nén không mất dữ liệu nhưng dải động giảm.
  * **JPG (8-bit)**: Kích thước tệp nhỏ nhất, chỉ phù hợp để xem trước và hiển thị trên web. Sử dụng tính năng nén tổn thất không phù hợp cho phân tích khoa học.

***

## Lưu mẫu dự án

Tính năng này cho phép bạn lưu cài đặt dự án hiện tại của mình dưới dạng mẫu có thể sử dụng lại.

* **Loại**: Nhập văn bản + Nút lưu
* **Mô tả**: Nhập tên mô tả cho mẫu cài đặt của bạn và nhấp vào biểu tượng lưu. Mẫu sẽ lưu trữ tất cả cài đặt dự án hiện tại của bạn (phát hiện mục tiêu, tùy chọn xử lý, chỉ mục và định dạng xuất) để dễ dàng sử dụng lại trong các dự án trong tương lai.
* **Trường hợp sử dụng**:
  * Tạo mẫu cho các hệ thống camera khác nhau (RGB, multispectral, NIR)
  * Lưu cấu hình tiêu chuẩn cho các loại cây trồng cụ thể hoặc quy trình phân tích
  * Chia sẻ cài đặt nhất quán trong một nhóm
* **Cách sử dụng**:
  1. Định cấu hình tất cả cài đặt dự án mong muốn của bạn
  2. Nhập tên mẫu (ví dụ: "Tiêu chuẩn NDVI của RedEdge Survey3")
  3. Nhấp vào biểu tượng lưu
  4. Mẫu hiện có thể được tải khi tạo dự án mới

***

## Lưu thư mục dự án

Cài đặt này chỉ định nơi lưu dự án mới theo mặc định.

* **Loại**: Hiển thị đường dẫn thư mục + Nút chỉnh sửa
* **Mặc định**: `C:\Users\[Tên người dùng]\Chloros Project`
* **Mô tả**: Hiển thị thư mục mặc định hiện tại nơi các dự án Chloros mới được tạo. Nhấp vào biểu tượng chỉnh sửa để chọn một thư mục khác.
* **Khi nào cần thay đổi**:
  * Đặt thành ổ đĩa mạng để cộng tác nhóm
  * Thay đổi sang ổ đĩa có nhiều dung lượng lưu trữ hơn cho các tập dữ liệu lớn
  * Sắp xếp các dự án theo năm, khách hàng hoặc loại dự án trong các thư mục khác nhau
* **Lưu ý**: Thay đổi cài đặt này chỉ ảnh hưởng đến các dự án MỚI. Các dự án hiện tại vẫn ở vị trí ban đầu.

***

## Cài đặt Kiên trì

Tất cả các cài đặt dự án sẽ được lưu tự động cùng với tệp dự án của bạn (định dạng dự án `.mapir`). Khi bạn mở lại một dự án, tất cả các cài đặt sẽ được khôi phục chính xác như khi bạn rời khỏi chúng.

### Phân cấp cài đặt

Các cài đặt được áp dụng theo thứ tự sau:

1. **Mặc định hệ thống** - Giá trị mặc định tích hợp được xác định bởi Chloros
2. **Cài đặt mẫu** - Nếu bạn tải mẫu khi tạo dự án
3. **Đã lưu cài đặt dự án** - Cài đặt được lưu cùng với tệp dự án
4. **Điều chỉnh thủ công** - Mọi thay đổi bạn thực hiện trong phiên hiện tại

### Cài đặt và xử lý ảnh

Hầu hết các thay đổi cài đặt (đặc biệt là trong danh mục Xử lý và Xuất) sẽ kích hoạt quá trình xử lý lại hình ảnh để phản ánh các cài đặt mới. Tuy nhiên, một số cài đặt "chỉ xuất" và không yêu cầu xử lý lại ngay lập tức:

* Lưu mẫu dự án
* Thư mục làm việc
* Định dạng hình ảnh đã hiệu chỉnh (áp dụng khi xuất)

***

## Các phương pháp hay nhất

1. **Bắt đầu với giá trị mặc định**: Cài đặt mặc định hoạt động tốt với hầu hết các hệ thống camera MAPIR và quy trình làm việc thông thường.
2. **Tạo mẫu**: Sau khi bạn đã tối ưu hóa cài đặt cho quy trình làm việc hoặc máy ảnh cụ thể, hãy lưu chúng dưới dạng mẫu để đảm bảo tính nhất quán giữa các dự án.
3. **Kiểm tra trước khi xử lý hoàn toàn**: Khi thử nghiệm các cài đặt mới, hãy thử nghiệm trên một tập hợp con hình ảnh nhỏ trước khi xử lý toàn bộ tập dữ liệu của bạn.
4. **Ghi lại cài đặt của bạn**: Sử dụng tên mẫu mô tả cho biết hệ thống camera, loại xử lý và mục đích sử dụng (ví dụ: "Survey3\_RGB\_NDVI\_Agriculture").
5. **Lựa chọn định dạng xuất**: Chọn định dạng xuất dựa trên mục đích sử dụng cuối cùng của bạn:
   * Phân tích khoa học → TIFF (16-bit hoặc 32-bit)
   * Xử lý GIS → TIFF (16-bit)
   * Hình dung nhanh → PNG (8-bit)
   * Chia sẻ web → JPG (8-bit)

***

Để biết thêm thông tin về các chỉ số đa phổ trong Chloros, hãy xem trang [Công thức chỉ số đa phổ](multispectral-index-formulas.md).
