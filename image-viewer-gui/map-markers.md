# Điểm đánh dấu bản đồ

Tab Bản đồ hiển thị hình ảnh của bạn trên bản đồ 2D tương tác dựa trên tọa độ GPS của chúng. Điều này cung cấp cái nhìn tổng quan về mặt địa lý của phiên chụp của bạn và giúp bạn hình dung phạm vi bao phủ không gian. Nó cũng hữu ích khi nhập hình ảnh lần đầu tiên để nhanh chóng xóa bất kỳ hình ảnh nào bạn không cần xử lý.

<figure><img src="../.gitbook/assets/chloros_map_markers.gif" alt=""><figcaption></figcaption></figure>

## Truy cập tab Bản đồ

1. Mở hoặc tạo dự án trong Chloros
2. Nhập hình ảnh có chứa siêu dữ liệu GPS
3. Nhấp vào tab **Bản đồ** <img src="../.gitbook/assets/image (3).png" alt="" data-size="line"> ở thanh bên trái
4. Bản đồ sẽ hiển thị các điểm đánh dấu tại vị trí GPS của mỗi hình ảnh

{% hint style="info" %}
**Yêu cầu GPS**: Chỉ những hình ảnh có tọa độ GPS được nhúng trong siêu dữ liệu EXIF ​​của chúng mới xuất hiện trên bản đồ. Đảm bảo máy ảnh của bạn đã bật GPS trong khi chụp.
{% endhint %}

***

## Điều chỉnh hình ảnh từ tab bản đồ

Tab **Bản đồ** <img src="../.gitbook/assets/image (3).png" alt="" data-size="line"> có cùng các nút thêm <img src="../.gitbook/assets/image.png" alt="" data-size="line"> <img src="../.gitbook/assets/image (1).png" alt="" data-size="line"> và xóa các nút tệp <img src="../.gitbook/assets/image (2).png" alt="" data-size="line"> giống như tab [**File Browser**](../processing-images-gui/adding-files-to-a-project.md) <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line">. Nó cũng hiển thị cùng một danh sách bảng tệp dự án nhưng với các tiêu đề cột khác nhau:

### Tên tệp

* Tên file gốc từ máy ảnh
* Duy trì quy ước đặt tên máy ảnh (ví dụ: IMG\_0001.RAW)

### Vĩ độ

* Vĩ độ của hình ảnh

### Kinh độ

* Kinh độ của hình ảnh

### Độ cao

* Độ cao của hình ảnh

{% hint style="info" %}
Nhấp vào tiêu đề cột của bảng cũng sắp xếp dữ liệu hàng
{% endhint %}

***

## Điểm đánh dấu hình ảnh

Mỗi hình ảnh có dữ liệu GPS được thể hiện bằng một điểm đánh dấu trên bản đồ:

### Hiển thị điểm đánh dấu

* Điểm đánh dấu cho biết tọa độ GPS chính xác nơi mỗi hình ảnh được chụp
* Điểm đánh dấu được nhóm có thể nhóm lại với nhau khi thu nhỏ
* Phóng to để xem các vị trí hình ảnh riêng lẻ

{% hint style="success" %}
SIÊU THU PHÓNG: Khi bạn đạt đến mức thu phóng tối đa từ nhà cung cấp ô bản đồ, ô đó sẽ được phóng to khi thu phóng thêm, cho phép bạn xem các điểm đánh dấu gần nhau.
{% endhint %}

### Xem trước khi di chuột

* **Di chuột** qua bất kỳ điểm đánh dấu nào để xem bản xem trước hình thu nhỏ của hình ảnh đó
* Điều này cho phép nhận dạng trực quan nhanh chóng mà không cần rời khỏi chế độ xem bản đồ
* Hữu ích cho việc định vị các hình ảnh cụ thể trong một phiên chụp lớn

***

## Nhà cung cấp ô bản đồ

{% hint style="success" %}
**Lựa chọn tự động**: Chloros tự động chọn dịch vụ ô cung cấp mức thu phóng tốt nhất cho vị trí bản đồ hiện tại của bạn. Bạn có thể chuyển đổi thủ công giữa các nhà cung cấp nếu muốn.
{% endhint %}

Tab Bản đồ hỗ trợ hai nhà cung cấp ô cho hình ảnh bản đồ nền:

### Google Maps

* Hình ảnh vệ tinh và bản đồ tiêu chuẩn từ Google
* Tốt nhất cho phạm vi phủ sóng chung trên toàn thế giới

###ESRI

* Hình ảnh vệ tinh và trên không từ ESRI ArcGIS
* Thường cung cấp hình ảnh có độ phân giải cao hơn ở một số vùng nhất định

***

## Các loại ô bản đồ

Bạn có thể chọn loại lớp bản đồ (từ trái sang phải):

&#x20;<img src="../.gitbook/assets/image (23).png" alt="" data-size="original">

### Địa hình

Hiển thị cấu hình độ cao và ô bản đồ với các chi tiết (đường, v.v.)

###Bản đồ

Hiển thị các ô bản đồ tiêu chuẩn (băng thông thấp hơn) với các chi tiết (đường, v.v.)

### Vệ tinh

Hiển thị các ô bản đồ vệ tinh chi tiết (băng thông cao hơn)

### Lai

Hiển thị các ô bản đồ vệ tinh với các chi tiết bổ sung (đường, v.v.)

***

## Điều hướng bản đồ

### Điều khiển thu phóng

* **Phóng to/Thu nhỏ**: Sử dụng con lăn chuột hoặc nút thu phóng
* **Toàn màn hình**: Toàn màn hình bản đồ

### Điều khiển Pan

* **Pan**: Nhấp và kéo để di chuyển xung quanh bản đồ

***

## Trường hợp sử dụng

### Trực quan hóa đường bay

* Xem vùng phủ sóng của các phiên chụp drone
* Xác định những khoảng trống trong vùng phủ sóng hình ảnh
* Xác minh việc thực hiện đường bay

### Đánh giá khảo sát mặt đất

* Xem sự phân bố không gian của ảnh chụp trên mặt đất
* Xác định vị trí hình ảnh mục tiêu hiệu chuẩn liên quan đến khu vực khảo sát
* Lập kế hoạch các địa điểm chụp bổ sung

### Kiểm soát chất lượng

* Nhanh chóng xác định hình ảnh được chụp ở những vị trí không ngờ tới
* Xác minh độ chính xác của GPS trên tập dữ liệu
* Tham chiếu chéo vị trí hình ảnh với ghi chú hiện trường

***

## Khắc phục sự cố

### Không có dấu hiệu nào xuất hiện

**Nguyên nhân có thể:**

* Hình ảnh không chứa siêu dữ liệu GPS
* GPS đã bị tắt trên máy ảnh trong khi chụp
* Dữ liệu EXIF ​​đã bị phần mềm bên ngoài xóa

**Giải pháp**: Xác minh GPS được bật trên máy ảnh của bạn và nhập lại tệp gốc

### Điểm đánh dấu sai vị trí

**Nguyên nhân có thể:**

* Camera GPS đã sửa lỗi vệ tinh kém
* GPS trôi trong quá trình chụp

**Giải pháp**: Đây thường là sự cố về thời gian chụp; cân nhắc sử dụng GPS PPK/RTK cho các ứng dụng chính xác