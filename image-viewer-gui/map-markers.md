# Điểm đánh dấu trên bản đồ

Tab Bản đồ hiển thị các hình ảnh của bạn trên một bản đồ 2D tương tác dựa trên tọa độ GPS của chúng. Tính năng này cung cấp cho bạn cái nhìn tổng quan về mặt địa lý của một phiên chụp ảnh và là cách nhanh nhất, ngay sau khi nhập ảnh, để loại bỏ những hình ảnh bạn không muốn xử lý.

<figure><img src="../.gitbook/assets/chloros_map_markers.gif" alt=""><figcaption></figcaption></figure>

## Truy cập tab Bản đồ

1. Mở hoặc tạo một dự án trong Chloros
2. Nhập các hình ảnh có chứa siêu dữ liệu GPS
3. Nhấp vào tab **Bản đồ** <img src="../.gitbook/assets/image (3) (1).png" alt="" data-size="line"> ở thanh bên trái
4. Bản đồ sẽ hiển thị một điểm đánh dấu tại vị trí GPS của mỗi hình ảnh

{% hint style="info" %}
**Yêu cầu có GPS**: chỉ những hình ảnh có tọa độ GPS trong siêu dữ liệu EXIF mới xuất hiện trên bản đồ. Hình ảnh không có tọa độ vẫn nằm trong dự án và vẫn được xử lý bình thường — chỉ là không có điểm đánh dấu mà thôi.
{% endhint %}

***

## Điều chỉnh hình ảnh từ tab Bản đồ

Tab **Bản đồ**<img src="../.gitbook/assets/image (3) (1).png" alt="" data-size="line"> có các nút thêm <img src="../.gitbook/assets/image (3).png" alt="" data-size="line"> <img src="../.gitbook/assets/image (1) (1).png" alt="" data-size="line"> và xóa <img src="../.gitbook/assets/image (2) (1).png" alt="" data-size="line"> tệp tương tự như tab [**Trình duyệt tệp**](../processing-images-gui/adding-files-to-a-project.md) <img src="../.gitbook/assets/icon_file-browser.JPG" alt="" data-size="line">. Tab này hiển thị danh sách tệp dự án tương tự, kèm theo các cột thông tin địa lý:

| Cột        | Nội dung                                                           |
| ------------- | ------------------------------------------------------------------ |
| **Tên**      | Tên tệp như khi được xuất từ máy ảnh                             |
| **Vĩ độ**  | Độ thập phân, sáu chữ số thập phân                                |
| **Kinh độ** | Độ thập phân, sáu chữ số thập phân                                |
| **Độ cao**  | Mét, một chữ số thập phân — `-` khi hình ảnh không có thông tin độ cao |

{% hint style="info" %}
Nhấp vào bất kỳ tiêu đề cột nào để sắp xếp theo cột đó; nhấp lại để đảo ngược thứ tự.
{% endhint %}

{% hint style="warning" %}
**Độ cao là độ cao so với mực nước biển, không phải độ cao so với mặt đất.** Giá trị này lấy từ thẻ EXIF `GPSAltitude` của hình ảnh, được tham chiếu đến mực nước biển trung bình. Đây không phải là độ cao bay so với địa hình, và Chloros sẽ không tính toán khoảng cách mẫu mặt đất (GSD) từ giá trị này — trên một cánh đồng ở độ cao 300 m so với mực nước biển, một máy bay không người lái ở độ cao 100 m so với mặt đất (AGL) sẽ ghi nhận khoảng 400 m tại đây. Sử dụng cột này để phát hiện các giá trị ngoại lệ và xác nhận độ cao bay nhất quán, chứ không phải để đo độ cao so với mặt đất (AGL).
{% endhint %}

***

## Điểm đánh dấu hình ảnh

Mỗi hình ảnh có dữ liệu GPS sẽ có một điểm đánh dấu tại tọa độ của nó.

### Hiển thị điểm đánh dấu

* Các điểm đánh dấu nằm chính xác tại tọa độ được ghi lại cho mỗi lần chụp
* Các điểm đánh dấu gần nhau có thể chồng chéo lên nhau khi thu nhỏ bản đồ — hãy phóng to để tách chúng ra
* Các điểm đánh dấu được chọn và làm nổi bật sẽ được hiển thị phía trên các điểm còn lại

### Xem trước khi di chuột

* **Di chuột** qua bất kỳ điểm đánh dấu nào để hiển thị hình thu nhỏ của ảnh đó kèm tên tệp
* **Nhấp**vào một điểm đánh dấu để chọn hình ảnh và**ghim** cửa sổ bật lên — cửa sổ này sẽ vẫn hiển thị cho đến khi bạn nhấp vào vị trí khác. Khi cửa sổ bật lên được ghim, việc di chuột qua các điểm đánh dấu khác sẽ không làm biến mất cửa sổ này
* Đây là cách nhanh chóng để tìm một khung hình cụ thể trong một phiên chụp lớn mà không cần rời khỏi bản đồ

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption><p>Tab Bản đồ hiển thị mọi hình ảnh có gắn thẻ địa lý trong dự án</p></figcaption></figure>

### Siêu phóng to

{% hint style="success" %}
**SIÊU PHÓNG TO**: khi bạn đạt đến mức phóng to tối đa mà nhà cung cấp ô bản đồ có hình ảnh, việc phóng to thêm sẽ làm các ô bản đồ lớn hơn thay vì dừng lại, nhờ đó bạn có thể phân biệt các điểm đánh dấu nằm gần như chồng lên nhau.
{% endhint %}

* Chức năng siêu phóng to chỉ hoạt động khi bạn đang **ở** mức phóng to tối đa của nhà cung cấp cho vị trí đó và các ô bản đồ đã tải xong. Dưới mức đó, thao tác phóng to sẽ diễn ra bình thường
* Phạm vi là **1× đến 32×** so với mức tối đa của chính nhà cung cấp
* Một chỉ báo ở góc màn hình hiển thị mức siêu phóng to hiện tại dưới dạng phần trăm, và nút **×** bên cạnh sẽ đưa bạn trở lại chế độ phóng to bình thường chỉ với một cú nhấp
* Việc thu nhỏ luôn chuyển trực tiếp về bản đồ gốc, do đó bạn sẽ không bao giờ bị kẹt trong chế độ siêu phóng to
* Khi thu phóng và di chuyển bản đồ trong chế độ siêu thu phóng, độ lệch kết quả sẽ được truyền lại cho bản đồ, do đó khu vực lệch tâm mà bạn di chuyển đến sẽ tiếp tục yêu cầu các ô bản đồ thay vì bị trống
* Các điểm đánh dấu được vẽ dưới dạng các phần tử vectơ thay vì được raster hóa, do đó chúng vẫn sắc nét ở mọi mức độ siêu thu phóng

***

## Nhà cung cấp ô bản đồ

{% hint style="success" %}
**Lựa chọn tự động**: Chloros sẽ chọn dịch vụ ô bản đồ cung cấp mức thu phóng tối ưu nhất cho vị trí hiện tại của hình ảnh. Bạn có thể chuyển đổi thủ công bất cứ lúc nào.
{% endhint %}

| Nhà cung cấp        | Ghi chú                                                                                                                                                             |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Google Maps** | Phạm vi phủ sóng rộng trên toàn thế giới; hỗ trợ cả bốn loại ô                                                                                                            |
| **Esri ArcGIS**| Thường có hình ảnh chụp từ trên không có độ phân giải cao hơn ở một số khu vực cụ thể. Loại ô**Terrain** không được cung cấp cho Esri và nút tương ứng sẽ bị vô hiệu hóa khi Esri được chọn |***

## Các loại ô bản đồ

Chọn loại lớp bản đồ bằng các nút (từ trái sang phải):

![](&lt;../.gitbook/assets/image (14).png&gt;)

| Loại                 | Hiển thị                                                                |
| -------------------- | -------------------------------------------------------------------- |
| **Địa hình**          | Hiệu ứng bóng độ cao kèm chi tiết bản đồ (đường, nhãn). Chỉ có trên Google       |
| **Bản đồ**              | Ô bản đồ đường phố tiêu chuẩn — tùy chọn có băng thông thấp nhất              |
| **Vệ tinh**        | Hình ảnh vệ tinh chi tiết, không có nhãn — tùy chọn có băng thông cao nhất |
| **Kết hợp** (mặc định) | Hình ảnh vệ tinh với đường và nhãn được vẽ chồng lên                |

Tab Bản đồ mở ra ở chế độ **Kết hợp**. Lựa chọn của bạn sẽ được áp dụng khi chuyển đổi nhà cung cấp, nếu nhà cung cấp đó hỗ trợ tính năng này.***

## Điều hướng bản đồ

* **Phóng to/thu nhỏ**: bánh xe cuộn chuột hoặc các nút phóng to/thu nhỏ trên bản đồ
* **Di chuyển**: nhấp và kéo
* **Toàn màn hình**: nút điều khiển toàn màn hình sẽ mở rộng bản đồ ra toàn bộ cửa sổ***

## Các trường hợp sử dụng

### Xem lại đường bay

* Xem nhanh phạm vi phủ sóng của một phiên bay drone
* Phát hiện các khoảng trống do bỏ sót lượt bay
* Xác nhận chuyến bay đã tuân theo lộ trình đã lập kế hoạch

### Kiểm tra khảo sát mặt đất

* Xem cách phân bố các hình ảnh chụp từ mặt đất
* Xác định vị trí các khung hình mục tiêu hiệu chuẩn so với khu vực khảo sát
* Quyết định vị trí cần chụp bổ sung

### Kiểm soát chất lượng

* Tìm các hình ảnh được chụp ở vị trí không mong muốn và loại bỏ chúng trước khi xử lý
* Sắp xếp theo Độ cao để phát hiện khung hình được chụp ở độ cao sai, hoặc khung hình có tín hiệu GPS yếu
* So sánh vị trí hình ảnh với ghi chú thực địa

***

## Khắc phục sự cố

### Không hiển thị điểm đánh dấu

**Nguyên nhân có thể**

* Các hình ảnh không chứa siêu dữ liệu GPS
* GPS đã bị tắt trên máy ảnh trong quá trình chụp
* Dữ liệu EXIF đã bị phần mềm khác xóa bỏ trước khi nhập

**Cách khắc phục**: xác nhận GPS đã được bật trên máy ảnh và nhập lại các tệp gốc. Bạn có thể kiểm tra xem một tệp cụ thể có tọa độ hay không bằng cách tìm kiếm tệp đó trong bảng tệp của tab Bản đồ — một hình ảnh không có tọa độ sẽ không có hàng tương ứng ở đó.

### Các điểm đánh dấu nằm sai vị trí

**Nguyên nhân có thể**: tín hiệu vệ tinh yếu tại thời điểm chụp, hoặc GPS bị lệch trong suốt phiên chụp.**Cách khắc phục**: đây là vấn đề phát sinh tại thời điểm chụp chứ không phải là điều mà Chloros có thể sửa chữa sau đó. Đối với công việc yêu cầu độ chính xác cao, hãy sử dụng quy trình làm việc GPS PPK/RTK — xem cài đặt**Áp dụng hiệu chỉnh PPK** trong [Cài đặt Dự án](../project-settings/project-settings.md).

### Bản đồ trống hoặc các ô bản đồ ngừng tải

Các nhà cung cấp ô bản đồ là các dịch vụ trực tuyến. Nếu các ô bản đồ ngừng được tải về, hãy kiểm tra kết nối mạng của máy, sau đó thử chuyển sang nhà cung cấp khác. Nếu bạn đang phóng to quá mức, hãy nhấn nút **×** để đặt lại về mức thu phóng bình thường và để bản đồ yêu cầu lại các ô bản đồ.***

## Các trang liên quan

* [**Lưới hình ảnh**](image-grid.md) — cùng bộ hình ảnh được sử dụng làm hình thu nhỏ
* [**Mở hình ảnh toàn màn hình**](opening-an-image-full-screen.md) — xem chi tiết một hình ảnh
* [**Thêm tệp vào dự án**](../processing-images-gui/adding-files-to-a-project.md) — các nút thêm/xóa tệp chung của tab này
