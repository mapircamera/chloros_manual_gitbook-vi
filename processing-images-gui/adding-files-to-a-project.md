# Thêm tệp vào dự án

Sau khi bạn đã tạo hoặc mở một dự án trong Chloros, bước tiếp theo là thêm các hình ảnh đa quang phổ để bắt đầu xử lý. Tab Trình duyệt tệp<img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> giúp bạn dễ dàng nhập hình ảnh và quản lý tập dữ liệu của mình.

## Truy cập Trình duyệt tệp

1. Mở hoặc tạo một dự án trong Chloros
2. Nhấp vào biểu tượng **File Browser** <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> ở thanh bên trái
3. Bảng điều khiển File Browser sẽ hiển thị danh sách tệp của dự án

{% hint style="info" %}
**Các định dạng tệp được hỗ trợ**:

* **Survey3W / Survey3N**: Cặp tệp RAW+JPG và hình ảnh JPG (khuyến nghị sử dụng RAW+JPG)
* **LATTICE**: Các bản ghi `.tif` / `.tiff` — được ghi bởi phần mềm điều khiển camera Chloros hoặc bởi hub LATTICE
* **Dữ liệu cảm biến ánh sáng**: Bản ghi `.daq` (DAQ-U/M/E) và nhật ký hướng xuống DAQ-M `.csv` — được nhập cùng với hình ảnh để thực hiện hiệu chuẩn độ phản xạ
{% endhint %}

***

## Thêm hình ảnh vào dự án của bạn

Có hai cách chính để thêm hình ảnh vào dự án của bạn:

### Phương pháp 1: Thêm tệp

Sử dụng tùy chọn này để nhập các tệp hình ảnh riêng lẻ hoặc một số lượng nhỏ tệp.

1. Nhấp vào nút **&quot;Thêm tệp&quot;** <img src="../.gitbook/assets/image (3).png" alt="" data-size="line"> ở đầu bảng điều khiển Trình duyệt tệp
2. Điều hướng đến thư mục chứa hình ảnh của bạn
3. Chọn một hoặc nhiều tệp hình ảnh (giữ phím **Ctrl** để chọn nhiều tệp)
4. Nhấp vào **&quot;Mở&quot;** để nhập các tệp đã chọn

### Phương pháp 2: Thêm thư mục

Sử dụng tùy chọn này để nhập tất cả hình ảnh từ một thư mục cùng một lúc. Bạn có thể chọn **nhiều thư mục** trong một hộp thoại.

1. Nhấp vào nút **&quot;Thêm thư mục&quot;** <img src="../.gitbook/assets/image (1) (1).png" alt="" data-size="line"> ở đầu bảng điều khiển Trình duyệt tệp
2. Điều hướng đến và chọn (các) thư mục chứa hình ảnh từ phiên chụp của bạn
3. Nhấp vào **&quot;Chọn thư mục&quot;** để nhập tất cả hình ảnh được hỗ trợ

{% hint style="info" %}
**Các tệp không tải được sẽ được báo cáo.** Nếu một thư mục chứa các tệp mà Chloros nhận diện được nhưng không thể tải, một cảnh báo sẽ thông báo cho bạn — hình ảnh sẽ không tự động biến mất khỏi lưới.
{% endhint %}

***

## Nhập các thư mục ghi hình LATTICE

Các bản ghi hình LATTICE được lưu với **một thư mục con cho mỗi cấp độ xuất** — ví dụ: `raw/`, `debayered/`, `radiance/`, `reflectance/`, `preview/` — cùng với tệp xuống (downwelling) tương ứng `.daq` ở thư mục gốc:

```
output/
├── raw/           capture_<timestamp>_SN<serial>_raw.tif
├── debayered/     capture_<timestamp>_SN<serial>_debayered.tif
├── preview/       capture_<timestamp>_SN<serial>_display.tif
└── *.daq          the downwelling reading matched to the capture
```

**Chọn Thêm thư mục tại thư mục gốc của các ảnh chụp** (`output/` ở trên). Khi thư mục đã chọn không chứa hình ảnh nào nhưng có các thư mục con, Chloros sẽ tự động đi sâu vào các thư mục đó — các thư mục con cùng cấp và thư mục gốc `.daq` đều được chọn cùng một lúc.**Cách nhập các bản chụp:*** Mỗi bản chụp được nhập dưới dạng **một hình ảnh duy nhất**, được nhóm theo bản chụp (không phải một mục cho mỗi cấp độ). Các cấp độ khác của cùng một bản chụp sẽ xuất hiện dưới dạng các chế độ xem của hình ảnh đó.
* **Quá trình xử lý luôn bắt đầu từ khung hình thô.** Các cấp độ khác có thể xem được, nhưng chỉ có `raw` mới được đưa vào quy trình xử lý — việc xử lý lại một sản phẩm đã được xử lý sẽ áp dụng các chỉnh sửa hai lần, do đó Chloros sẽ từ chối. Một tệp xuất được nhập lại không bao giờ có thể chiếm vị trí khung hình thô của một lần chụp.
* Một thư mục chụp được lưu **mà không** có các khung hình thô sẽ được nhập và hiển thị bình thường, nhưng quá trình xử lý sẽ bỏ qua nó và ghi lại điều này trong nhật ký. (Tham số dòng lệnh CLI `--input-level` có thể ép buộc điểm vào cho trường hợp này — xem [Tài liệu tham khảo CLI](../reference/cli-reference.md#what-a-captures-folder-looks-like).)
* Kể từ phiên bản 1.3.0, một thư mục ghi lại cũng có thể chứa một thư mục `clamp_masks/` (mặt nạ theo từng pixel cho mức độ bức xạ và phản xạ — xem [Định dạng hình ảnh đầu ra](../output-image-formats.md#clamp-masks)). Các mặt nạ không phải là hình ảnh của cảnh, và Chloros sẽ bỏ qua chúng khi nhập, bất kể chúng nằm ở đâu.

**Các phiên làm việc trên LATTICE hub** được nhập theo cách tương tự: chọn &quot;Add Folder&quot; (Thêm thư mục) vào thư mục phiên làm việc được sao chép từ hub (thư mục này chứa `raw/` và `previews/`), cùng với bất kỳ tệp nhật ký hướng xuống nào của DAQ-M (`.csv`). Nếu thông tin hiệu chuẩn của camera hoặc DAQ chưa được lưu trong bộ nhớ đệm trên máy của bạn, Chloros sẽ tự động lấy thông tin đó theo số sê-ri khi nhập (cần kết nối internet một lần).***

## Hiểu về bảng Trình duyệt tệp

Sau khi hình ảnh được nhập, chúng sẽ xuất hiện trong một bảng với các cột sau:

### Tên tệp

* Tên tệp gốc từ máy ảnh
* Giữ nguyên quy ước đặt tên của máy ảnh (ví dụ: IMG\_0001.RAW hoặc capture\_20260816\_101500\_SN213800234\_raw.tif)

### Dấu thời gian

* Ngày và giờ chụp ảnh
* Trích xuất từ siêu dữ liệu EXIF của ảnh
* Được sử dụng để khớp cảm biến ánh sáng, đồng bộ hóa PPK và lập lịch mục tiêu hiệu chuẩn

### Mẫu máy ảnh

* Tự động phát hiện cấu hình máy ảnh và bộ lọc
* Ví dụ về Survey3: Survey3W_RGN, Survey3N_OCN, Survey3W_RGB
* Ví dụ về LATTICE: LATT-M3M-L41-F550, LATT-M3C-L87-FRGN
* Được sử dụng để áp dụng các cấu hình xử lý chính xác

### Cột Mục tiêu (Hộp chọn)

* Chọn hộp này cho các hình ảnh chứa mục tiêu hiệu chuẩn
* Khi có ít nhất một hình ảnh được chọn, **chỉ các hình ảnh được chọn mới được quét** để tìm mục tiêu
* Xem [Chọn Hình ảnh Mục tiêu](choosing-target-images.md) để biết chi tiết

### Xem siêu dữ liệu hình ảnh

Nhấp vào nút chuyển đổi ở góc trên bên phải phía trên bảng sẽ hiển thị siêu dữ liệu của hình ảnh đã chọn trong khu vực lưới hình ảnh.

<figure><img src="../.gitbook/assets/chloros_grid_meta.gif" alt=""><figcaption></figcaption></figure>

***

## Các tệp cảm biến ánh sáng trong dự án của bạn

* Các tệp `.daq` và `.csv` xuất hiện trong danh sách Trình duyệt tệp nhưng không phải là hình ảnh có thể nhấp vào — chúng cung cấp cường độ bức xạ chiếu xuống để hiệu chuẩn độ phản xạ.
* Mỗi tệp `.daq`/`.csv` được nhập vào sẽ được liệt kê trong **Cài đặt Dự án → Cảm biến Ánh sáng DAQ**, nơi bạn có thể xem lại hiệu chỉnh nắp khuếch tán đang được áp dụng cho từng tệp. Xem [Điều chỉnh Cài đặt Dự án](adjusting-project-settings.md).
* Các bản ghi bạn thực hiện trong tab **Cảm biến Ánh sáng** sẽ được tự động thêm vào dự án đang mở — không cần nhập thủ công.***

## Quản lý Tệp trong Dự án của Bạn

### Xóa tệp

Để xóa các hình ảnh không mong muốn khỏi dự án của bạn:

1. Chọn một hoặc nhiều hình ảnh trong bảng Trình duyệt tệp
2. Nhấp vào nút **&quot;Xóa các mục đã chọn&quot;** <img src="../.gitbook/assets/image (2) (1).png" alt="" data-size="line">
3. Xác nhận việc xóa (các tệp không bị xóa khỏi đĩa, chỉ bị loại bỏ khỏi dự án)

### Sắp xếp và lọc

* **Sắp xếp theo cột**: Nhấp vào tiêu đề bất kỳ cột nào để sắp xếp hình ảnh
* **Sắp xếp theo dấu thời gian**: Hữu ích để tổ chức các chuỗi hình ảnh theo thứ tự thời gian
* **Lọc theo mẫu máy ảnh**: Nhóm hình ảnh theo loại máy ảnh nếu sử dụng nhiều máy ảnh***

## Xem trước hình ảnh

### Xem hình ảnh đầy đủ

Nhấp vào bất kỳ hình thu nhỏ nào trong Trình duyệt tệp để hiển thị hình ảnh đó trong khu vực xem trước chính:

1. Hình ảnh xuất hiện trong bảng xem trước ở giữa
2. Sử dụng các nút điều khiển thu phóng để kiểm tra chi tiết hình ảnh
3. Di chuyển giữa các hình ảnh bằng các phím mũi tên

### Điều hướng nhanh

* **Hình ảnh trước**: Nhấp vào mũi tên trái hoặc nhấn phím ←
* **Hình ảnh tiếp theo**: Nhấp vào mũi tên phải hoặc nhấn phím →
* **Phóng to/Thu nhỏ**: Sử dụng con lăn chuột hoặc các nút phóng to
* **Di chuyển**: Nhấp và kéo trên hình ảnh khi đã phóng to***

## Xử lý tệp trùng lặp

Chloros tự động phát hiện và bỏ qua các tệp trùng lặp:

* Các tệp có tên tệp giống hệt nhau sẽ bị bỏ qua
* Ngăn chặn việc xử lý trùng lặp vô tình
* Hiển thị thông báo cảnh báo khi phát hiện tệp trùng lặp

{% hint style="warning" %}
**Lưu ý quan trọng**: Không đổi tên hoặc sửa đổi các tệp hình ảnh gốc trước khi nhập. Chloros dựa vào tên tệp và siêu dữ liệu gốc để xử lý chính xác.
{% endhint %}

***

## Bộ dữ liệu từ nhiều loại camera

Nếu dự án của bạn chứa hình ảnh từ nhiều camera MAPIR:

1. Chloros tự động phát hiện từng mẫu camera — Survey3, LATTICE hoặc kết hợp
2. Mỗi loại camera được xử lý bằng hồ sơ hiệu chuẩn phù hợp
3. Trình duyệt tệp hiển thị mẫu camera trong cột &quot;Camera Model&quot;
4. Mỗi camera sẽ có cây thư mục đầu ra riêng khi được xử lý

**Ví dụ về các tình huống**: Cấu hình hai camera Survey3W RGN + Survey3N OCN, hoặc mảng camera LATTICE với camera chính RGB và một số mô-đun dải hẹp***

## Các thực hành tốt nhất

### Sắp xếp trước khi nhập

* Giữ các hình ảnh mục tiêu hiệu chuẩn trong cùng thư mục với các hình ảnh khảo sát
* Lưu các tệp cảm biến ánh sáng `.daq` / `.csv` của mỗi phiên chụp cùng với hình ảnh của phiên đó
* Giữ nguyên cấu trúc thư mục gốc từ camera/thẻ SD/hub
* Không trộn lẫn các bộ dữ liệu từ các phiên chụp khác nhau trong cùng một dự án

### Đặt tên tệp

* Giữ nguyên tên tệp gốc của máy ảnh (IMG\_0001.RAW, capture\_..., v.v.)
* Không đổi tên tệp trước khi nhập
* Tên gốc chứa các siêu dữ liệu quan trọng

### Hình ảnh mục tiêu hiệu chuẩn

* Luôn bao gồm 1–2 hình ảnh mục tiêu hiệu chuẩn cho mỗi phiên chụp (phần mềm Survey3; đối với LATTICE, bản ghi DAQ có thể thay thế — xem [Chọn hình ảnh mục tiêu](choosing-target-images.md))
* Chụp các hình ảnh mục tiêu trước và sau phiên chụp
* Đặt các hình ảnh mục tiêu trong điều kiện ánh sáng tương tự như khu vực chụp
* Đánh dấu các hình ảnh mục tiêu bằng cách chọn hộp kiểm &quot;Target&quot;

***

## Các vấn đề thường gặp và cách khắc phục

### Hình ảnh không hiển thị sau khi nhập

**Nguyên nhân có thể:**

* Định dạng tệp không được hỗ trợ (xem danh sách các định dạng được hỗ trợ ở đầu trang này)
* Hình ảnh được chụp từ máy ảnh không phải của MAPIR (xem [Các máy ảnh được hỗ trợ](../supported-cameras.md))
* Tệp bị hỏng hoặc quá trình chuyển dữ liệu từ thẻ SD không hoàn tất

**Giải pháp**: Kiểm tra tính tương thích giữa định dạng tệp và mẫu máy ảnh, đồng thời xem cảnh báo tải tệp để xác định chính xác các tệp không thành công

### Không phát hiện được mẫu máy ảnh

**Nguyên nhân có thể:**

* Dữ liệu EXIF bị sửa đổi
* Hình ảnh đã được chỉnh sửa bằng phần mềm bên ngoài
* Quá trình chuyển tệp không hoàn tất

**Giải pháp**: Nhập lại các tệp gốc, chưa bị sửa đổi từ máy ảnh/thẻ SD

### Thiếu dấu thời gian

**Nguyên nhân có thể:**

* Đồng hồ máy ảnh không được cài đặt chính xác
* Dữ liệu EXIF bị xóa bởi phần mềm bên ngoài

**Giải pháp**: Kiểm tra xem cài đặt thời gian trên máy ảnh đã chính xác trong quá trình chụp hay chưa

### Báo cáo thiếu tệp khi mở lại dự án

Nếu các tệp nguồn đã bị di chuyển hoặc xóa kể từ lần mở dự án gần nhất, Chloros sẽ thông báo cho bạn biết **những**tệp nào đã bị mất thay vì mở ra một lưới trống. Nếu chính thư mục dự án bị di chuyển, Chloros sẽ điều chỉnh lại đường dẫn nguồn trỏ đến thư mục mà `project.json` hiện đang nằm, đồng thời giữ lại bản sao lưu (`project.json.before-relink-<date>-<time>`). Khi không tìm thấy các tệp đó, cửa sổ**Không tìm thấy tệp nguồn**sẽ hiển thị tùy chọn**Duyệt** đến vị trí mới, sau đó là Tạo dự án mới hoặc Xóa dự án. Từ giao diện dòng lệnh (CLI), sử dụng `chloros-cli project open PATH --relink FOLDER`; từ bộ công cụ phát triển phần mềm (SDK), sử dụng `open_project(path, relink=...)`.***

## Các bước tiếp theo

Sau khi các tệp của bạn đã được nhập:

1. **Kiểm tra danh sách tệp** - Đảm bảo tất cả hình ảnh đã được tải đúng cách
2. **Kiểm tra mẫu máy ảnh** - Xác minh việc nhận diện máy ảnh chính xác
3. **Đánh dấu hình ảnh mục tiêu** - Xem [Chọn hình ảnh mục tiêu](choosing-target-images.md)
4. **Điều chỉnh cài đặt** - Cấu hình các tùy chọn xử lý trong [Cài đặt dự án](adjusting-project-settings.md)
5. **Bắt đầu xử lý** - Xem [Bắt đầu xử lý](starting-the-processing.md)

Để biết thông tin chi tiết về cấu hình dự án, hãy xem [Điều chỉnh Cài đặt Dự án](adjusting-project-settings.md).
