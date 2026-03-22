# Điểm đánh dấu bản đồ

Tab Bản đồ hiển thị hình ảnh của bạn trên bản đồ 2D tương tác dựa trên chế độ GPS của chúng. Điều này cung cấp cái nhìn tổng quan về địa lý phiên bản của bạn và giúp bạn hình dung phạm vi bao phủ không gian. Nó cũng hữu ích khi bạn nhập hình ảnh của mình lần đầu tiên để nhanh chóng xóa bất kỳ hình ảnh nào bạn không cần xử lý.

<figure><img src="../.gitbook/assets/chloros_map_markers.gif" alt=""><figcaption></figcaption></figure>

## Truy cập tab Bản đồ

1. Mở hoặc tạo dự án trong Chloros
2. Nhập hình ảnh chứa siêu dữ liệu GPS
3. Cú vào tab **Bản đồ** <img src="../.gitbook/assets/image (3).png" alt="" data-size="line"> ở thanh bên trái
4. Bản đồ sẽ hiển thị các dấu hiệu đánh dấu tại vị trí GPS của mỗi hình ảnh

{% hint style="info" %}
**Yêu cầu GPS**: Chỉ những hình ảnh có GPS được nhúng trong EXIF ​​siêu dữ liệu mới xuất hiện trên bản đồ. Đảm bảo ảnh của bạn đã bật GPS khi chụp.
{% endhint %}

***

## Điều chỉnh hình ảnh từ tab bản đồ

Tab **Bản đồ**<img src="../.gitbook/assets/image (3).png" alt="" data-size="line"> có cùng phần bổ sung <img src="../.gitbook/assets/image.png" alt="" data-size="line"> <img src="../.gitbook/assets/image (1).png" alt="" data-size="line"> và xóa các nút tệp <img src="../.gitbook/assets/image (2).png" alt="" data-size="line"> làm [**File Browser**](../processing-images-gui/adding-files-to-a-project.md) <img tab src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line"> thì có. Nó cũng hiển thị tương tự danh sách dự án tệp bảng nhưng với các cột tiêu đề khác nhau:

### Tệp tên

* Tên gốc của file ảnh từ máy ảnh
* Duy trì quy trình đặt tên máy ảnh (ví dụ: IMG\_0001.RAW)

### Vĩ độ

* Hình ảnh vĩ độ

### Kinh độ

* Hình ảnh Kinh độ

###Độ cao

* Độ cao của hình ảnh

{% hint style="info" %}
Click vào cột tiêu đề của bảng cũng sắp xếp hàng dữ liệu
{% endhint %}

***

##Hình ảnh đánh dấu điểm

Mỗi hình ảnh đều có dữ liệu GPS có thể được hiển thị bằng một dấu đánh dấu trên bản đồ:

### Show the marker

* Điểm đánh dấu cho biết độ chính xác của GPS nơi mỗi hình ảnh được chụp
* Điểm đánh dấu các nhóm có thể được nhóm lại khi thu nhỏ
* Phóng to để xem các vị trí hình ảnh riêng biệt

{% hint style="success" %}
SIÊU THU PHÓNG: Khi bạn đạt được mức thu phóng tối đa từ nhà cung cấp ô bản đồ, ô đó sẽ được phóng to khi thu phóng thêm, cho phép bạn xem các điểm chiến đấu gần nhau.
{% endhint %}

### Xem trước khi di chuột

* **Di chuột** qua bất kỳ điểm đánh dấu nào để xem bản xem trước hình thu nhỏ của hình ảnh đó
* Điều này cho phép nhận dạng trực quan nhanh chóng mà không cần phải rời khỏi chế độ xem bản đồ
* Hữu ích cho việc định vị các công cụ hình ảnh trong một phiên bản chụp lớn

***

## Nhà cung cấp ô bản đồ

{% hint style="success" %}
**Lựa chọn tự động**: Chloros tự động chọn dịch vụ ô cung cấp mức độ thu phóng tốt nhất cho vị trí bản đồ hiện tại của bạn. Bạn có thể chuyển đổi thủ công giữa các nhà cung cấp nếu muốn.
{% endhint %}

Tab cung cấp ô hỗ trợ bản đồ hỗ trợ cho hình ảnh bản đồ:

### Google Maps

* Hình ảnh được bảo vệ và tiêu chuẩn bản đồ từ Google
* Tốt nhất cho phạm vi phủ sóng chung trên toàn thế giới

###ESRI

* Hình ảnh bảo vệ và trên không từ ESRI ArcGIS
* Thường cung cấp hình ảnh có độ phân giải cao hơn ở một số vùng nhất định

***

## Các loại ô bản đồ

Bạn có thể chọn loại bản đồ (từ trái sang phải):

&#x20;<img src="../.gitbook/assets/image (23).png" alt="" data-size="original">

### Địa hình

Hiển thị cao cấu hình và ô bản đồ với các chi tiết (đường, v.v.)

###Bản đồ

Hiển thị các ô tiêu chuẩn bản đồ (băng thông thấp hơn) với các chi tiết (đường, v.v.)

###Vệ tinh

Hiển thị các ô bản đồ bảo vệ chi tiết (băng thông tin cao hơn)

###Lại

Hiển thị các ô bản đồ vệ tinh với các chi tiết bổ sung (đường, v.v.)

***

##Bản đồ điều hướng

### Zoom control

* **Phóng to/Thu nhỏ**: Sử dụng con lăn chuột hoặc nút thu phóng
* **Toàn màn hình**: Toàn màn hình bản đồ

### Pan điều khiển

* **Pan**: Tinh và kéo để di chuyển xung quanh bản đồ***

## Trường hợp sử dụng

### Vịnh đường hóa học trực tiếp

* Xem vùng phủ sóng của các phiên bản drone
* Xác định những khoảng trống trong vùng phủ sóng hình ảnh
* Xác minh việc thực hiện đường bay

###Đánh giá khảo sát mặt đất

* Xem bố cục không gian của ảnh chụp trên mặt đất
* Xác định tiêu chuẩn hình ảnh định vị liên hệ với khu vực khảo sát
* Lập kế hoạch bổ sung địa điểm

### Kiểm soát chất lượng

* Hình ảnh được xác định nhanh chóng được chụp ở những vị trí không mong đợi
* Xác định độ chính xác của GPS trên dữ liệu
* Tham chiếu hình ảnh vị trí chéo với trường ghi chú

***

##Khắc phục sự cố

### Không có dấu hiệu nào xuất hiện

**Nguyên nhân có thể:**

* Hình ảnh không chứa siêu dữ liệu GPS
* GPS đã bị tắt trên máy ảnh khi chụp
* EXIF ​​data đã được xóa bên ngoài phần mềm

**Giải pháp**: Xác minh GPS được bật trên máy ảnh của bạn và nhập lại tệp gốc

### Điểm đánh dấu sai vị trí

**Nguyên nhân có thể:**

* Camera GPS đã sửa lỗi bảo vệ tinh tế
* GPS trôi trong quá trình chụp

**Giải pháp**: Đây thường là vấn đề về thời gian chụp; Cân nhắc sử dụng GPS PPK/RTK cho các ứng dụng chính xác