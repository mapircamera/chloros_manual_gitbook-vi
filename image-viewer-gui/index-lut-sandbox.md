# Hộp cát chỉ mục/LUT

Hộp cát Index/LUT là một không gian làm việc tương tác trong Trình xem ảnh Chloros cho phép bạn thử nghiệm các phép tính chỉ số đa phổ và trực quan hóa màu sắc trong thời gian thực. Công cụ mạnh mẽ này giúp bạn kiểm tra các chỉ mục khác nhau, tinh chỉnh phạm vi giá trị và tạo trực quan hóa sẵn sàng xuất bản mà không cần xử lý lại toàn bộ tập dữ liệu của bạn.

## Sandbox Index/LUT là gì?

### Mục đích

Hộp cát cung cấp:

* **Tính toán chỉ số thời gian thực** - Áp dụng bất kỳ chỉ số thực vật nào ngay lập tức
* **Điều chỉnh LUT tương tác** - Tinh chỉnh độ chuyển màu và phạm vi màu
* **Tối ưu hóa quy trình làm việc**- Xác định cài đặt tốt nhất trước khi xử lý hàng loạt

### Sandbox so với xử lý dự án**Hộp cát chỉ mục/LUT (Tương tác):**

* Hình ảnh duy nhất tại một thời điểm
* Phản hồi tức thì
* Thử nghiệm và lặp lại
* Không có thay đổi vĩnh viễn đối với tập tin
* Hoàn hảo để khám phá và thử nghiệm

**Xử lý dự án (hàng loạt):**

* Toàn bộ tập dữ liệu cùng một lúc
* Cài đặt được cấu hình sẵn
* Tệp đầu ra vĩnh viễn
* Tốn nhiều thời gian
* Tốt nhất khi cài đặt được hoàn tất

{% hint style="success" %}
**Quy trình làm việc tốt nhất**: Sử dụng Hộp cát để thử nghiệm và tìm cài đặt chỉ mục và LUT tối ưu, sau đó áp dụng các cài đặt đó trong quá trình Xử lý dự án cho toàn bộ tập dữ liệu của bạn.
{% endhint %}***

## Làm việc với Hộp cát Index/LUT

### Tìm hiểu các chỉ số được tính toán trước

Trong Chloros, các chỉ số có thể được áp dụng trong quá trình xử lý dự án. Để xác định cài đặt chỉ mục và LUT nào bạn muốn áp dụng cho xuất, cách dễ nhất là sử dụng hộp cát trình xem hình ảnh.

Hộp cát cho phép bạn:

* **Áp dụng chỉ mục mới và độ dốc màu (LUT)** để trực quan hóa dữ liệu
* **Điều chỉnh cài đặt hiển thị** một cách tương tác
* **Xem** hình ảnh chỉ mục đã được tính toán
* **Kiểm tra** giá trị pixel ở mọi mức thu phóng

### Mở Sandbox

Hộp cát Index/LUT được truy cập trong**Trình xem hình ảnh**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> tab thanh bên:

1. Nhấp vào một hình ảnh trong lưới hình ảnh của trình duyệt tệp, nó sẽ mở trong tab**Trình xem hình ảnh**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
2. Nhấp vào tab**Trình xem hình ảnh**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> để mở thanh bên bật ra bên trái nếu nó chưa mở

### Chọn một Hình ảnh để Áp dụng Chỉ mục/LUT cho

Để làm việc với một chỉ mục trong hộp cát <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> của Trình xem hình ảnh:

1.**Mở một hình ảnh**từ lưới hình ảnh chính bằng cách nhấp vào nó
2. Sau đó, tab**Trình xem hình ảnh**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> sẽ mở
3. Nhấp vào**Thả xuống lớp** (trên cùng bên phải của trình xem)
4. Chọn lớp từ danh sách thả xuống:
   * RAW (Phản xạ)

### Áp dụng chỉ mục cho hình ảnh

Khi hình ảnh ở chế độ toàn màn hình và thanh bên tab **Image Viewer**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> được mở:

1. Chọn hộp Chỉ mục ở đầu thanh bên
2. Chọn bộ lọc máy ảnh của bạn từ menu thả xuống bên trái
3. Chọn công thức chỉ số mong muốn từ danh sách thả xuống bên phải
4. Kéo vòng tròn màu kênh lọc đến các vị trí trong công thức chỉ số bên dưới
5. Khi công thức hợp lệ, hình ảnh sẽ cập nhật và hiển thị các giá trị chỉ mục
6. Di chuyển con trỏ chuột xung quanh để xem các giá trị tại vị trí của con trỏ
7. Phóng to để xem từng pixel và giá trị liên quan của chúng

Mỗi chỉ số có một phạm vi giá trị cụ thể và ý nghĩa:

#### Ví dụ NDVI

```

Công thức: (NIR - Đỏ) / (NIR + Đỏ)

Đối với máy ảnh Survey3W RGN:
NIR = băng tần 850nm
Màu đỏ = dải 661nm

Phạm vi kết quả: -1,0 đến +1,0
Thảm thực vật điển hình: 0,4 đến 0,9
Thảm thực vật bị căng thẳng: 0,2 đến 0,4
Đất trống: 0,0 đến 0,2
Nước: -0,1 đến 0,1
```

Để có tài liệu về công thức chỉ mục đầy đủ, hãy xem [Công thức chỉ mục đa phổ](../project-settings/multispectral-index-formulas.md).***

## Làm việc với LUT (Bảng tra cứu)

### LUT là gì?
**Bảng tra cứu (LUT)** ánh xạ các giá trị chỉ số số thành màu sắc để trực quan hóa:

* **Đầu vào**: Giá trị pixel chỉ mục (ví dụ: NDVI 0,65)
* **Đầu ra**: Màu RGB (ví dụ: xanh lục sáng)
* **Mục đích**: Làm cho các mẫu dễ nhìn và diễn giải hơn**Thang độ xám so với LUT màu:**

* Thang độ xám: Khoa học và trung lập, hiển thị dữ liệu thô
* Màu LUT: Trực quan và có tác động mạnh mẽ, làm nổi bật các mẫu và sự khác biệt

{% hint style="success" %}
**Sức mạnh trực quan**: Áp dụng LUT màu cho hình ảnh chỉ mục thang độ xám giúp việc xác định nhanh chóng các mẫu, điểm bất thường và khu vực quan tâm dễ dàng hơn đáng kể.
{% endhint %}

### Áp dụng LUT cho ảnh chỉ mục

Khi bạn có một hình ảnh chỉ mục hiển thị

1. Nhấp vào nút <img src="../.gitbook/assets/image.png" alt="" data-size="line"> "+Thêm LUT"
2. Chọn độ dốc màu
3. Điều chỉnh điểm cuối cắt tối thiểu/tối đa
4. Điều chỉnh chế độ cắt
5. Chọn hộp Chỉ mục trong thanh bên tab**Image Viewer**<img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> để áp dụng LUT

### Chọn một dải màu**Chọn độ chuyển màu:**

1. Trong bảng LUT, tìm**thanh gradient màu**

2. Di chuột qua nó để xem các cài đặt trước độ dốc có sẵn
3. Chọn độ dốc mong muốn
4. Hình ảnh**cập nhật ngay lập tức**với màu sắc mới khi hộp Chỉ mục được chọn

{% hint style="success" %}**Phương pháp tốt nhất**: Đối với các chỉ số thực vật như NDVI, chuyển màu Đỏ-Vàng-Xanh lục mang tính trực quan nhất vì nó phù hợp với các liên kết màu sắc tự nhiên (xanh lục=khỏe mạnh, vàng=trung bình, đỏ=căng thẳng).
{% endhint %}

### Điều chỉnh các lớp màu**Điều khiển lớp**xác định số lượng bước màu riêng biệt xuất hiện trong dải màu của bạn:** Tùy chọn số lớp:*** **2-5 lớp**: Danh mục rất rộng, khu vực riêng biệt
* **6-10 lớp**: Cân bằng, tốt cho việc phân loại
* **11-20 lớp**: Chuyển màu mượt mà, xuất hiện liên tục
* **Hơn 20 lớp**: Gần như liên tục, độ mượt tối đa**Cách điều chỉnh:**

1. Trong bảng LUT, xác định vị trí**ô màu mẫu bên dưới thanh gradient**

2. Điều chỉnh số lớp bằng cách thêm bằng nút +
3. Xóa số lớp bằng cách nhấp đúp vào mẫu màu
4. Cập nhật độ dốc**theo thời gian thực**trên hình ảnh**Hiệu ứng trực quan:***
**Ít lớp hơn** (3-5): Tạo các vùng riêng biệt, phân loại đơn giản hóa, dễ phân biệt danh mục hơn
* **Lớp trung bình** (6-10): Cách tiếp cận cân bằng, phù hợp với hầu hết các ứng dụng
* **Các lớp khác**(15-20): Chuyển tiếp mượt mà, biến đổi chi tiết, hình thức chụp ảnh**Khi nào nên sử dụng:***
**Ít lớp (3-5)**: Slide thuyết trình, sơ đồ phân loại, báo cáo đơn giản
* **Lớp trung bình (6-10)**: Phân tích tổng quát, cân bằng chi tiết, báo cáo chuẩn
* **Nhiều lớp (15-20)**: Phân tích khoa học, kiểm tra chi tiết, đầu ra chất lượng công bố

### Tinh chỉnh phạm vi giá trị**Điều khiển phạm vi giá trị**xác định giá trị chỉ mục nào ánh xạ tới màu nào trong dải màu của bạn:**Điều khiển phạm vi trong bảng LUT:*** ** Giá trị tối thiểu**: Giới hạn dưới của thang màu
* **Giá trị tối đa**: Giới hạn trên của thang màu
* **Giá trị trung gian**: Tự động phân bổ giữa mức tối thiểu và tối đa (dựa trên số lượng lớp)

#### Điều chỉnh Giá trị Tối thiểu/Tối đa**Để điều chỉnh phạm vi giá trị:**1. Trong bảng LUT, tìm các trường nhập** Giá trị tối thiểu**và**Giá trị tối đa**

2. Nhấp vào trường**Giá trị tối thiểu**

3. Nhập giá trị tối thiểu mong muốn (ví dụ: `0,2`)
4. Nhấn**Enter**hoặc nhấp vào bên ngoài trường
5. Lặp lại cho trường**Giá trị tối đa**(ví dụ: `0,9`)
6. Hình ảnh**cập nhật ngay lập tức**{% hint style="info" %}**Tự động chia tỷ lệ**: Khi bạn áp dụng LUT lần đầu tiên, Chloros sẽ tự động đặt mức tối thiểu/tối đa cho phạm vi dữ liệu thực tế trong hình ảnh. Sau đó, bạn có thể thu hẹp phạm vi này để tập trung vào phạm vi giá trị cụ thể mà bạn quan tâm.
{% endhint %}**Ví dụ về điều chỉnh phạm vi NDVI:***
**Phạm vi đầy đủ**: `-1.0` đến `1.0` (hiển thị tất cả các giá trị có thể)
* **Tập trung vào thảm thực vật**: `0,2` đến `0,9` (không bao gồm đất trống và nước)
* **Chỉ thực vật khỏe mạnh**: `0,5` đến `0,9` (chỉ đánh dấu những thực vật khỏe mạnh)
* **Phát hiện căng thẳng**: `0,2` đến `0,5` (nhấn mạnh các khu vực có vấn đề)
* **Phạm vi tùy chỉnh**: Điều chỉnh dựa trên giá trị pixel được quan sát của bạn**Tại sao phải điều chỉnh phạm vi?***
**Tăng độ tương phản** trong lĩnh vực bạn quan tâm
* **Loại trừ các giá trị không liên quan** (ví dụ: vùng nước, đất trống)
* **Chuẩn hóa trực quan** trên nhiều hình ảnh hoặc ngày tháng
* **Nhấn mạnh những khác biệt tinh tế** trong phạm vi giá trị hẹp

### Cắt bớt các giá trị ngoài phạm vi

Khi giá trị pixel nằm ngoài phạm vi tối thiểu/tối đa đã xác định của bạn, bạn có thể kiểm soát cách chúng hiển thị bằng cách sử dụng**chế độ cắt**.

####**Các tùy chọn chế độ cắt có sẵn:**

#### 1. Tối thiểu và tối đa

* Điểm ảnh **dưới mức tối thiểu**→ hiển thị bằng cách sử dụng**màu đầu tiên** trong dải màu (ví dụ: màu đỏ)
* Điểm ảnh **trên mức tối đa**→ hiển thị bằng cách sử dụng**màu cuối cùng** trong dải màu (ví dụ: màu xanh lá cây)
* **Trường hợp sử dụng**: Nhấn mạnh đến mức cực đoan, hiển thị đầy đủ phạm vi dữ liệu với màu sắc bão hòa ở giới hạn
* **Ví dụ**: Các giá trị NDVI dưới 0,2 đều có màu đỏ, các giá trị trên 0,9 đều có màu xanh lục

#### 2. Nền trong suốt

* Các pixel **nằm ngoài phạm vi**trở nên**hoàn toàn trong suốt***Chỉ các pixel** trong phạm vi** hiển thị dải màu
* **Trường hợp sử dụng**: Lớp phủ GIS, tách biệt các phạm vi giá trị cụ thể, chỉ làm nổi bật các lĩnh vực quan tâm
* **Ví dụ**: Chỉ hiển thị màu NDVI 0,4-0,7, mọi thứ khác trong suốt

{% hint style="warning" %}**Giới hạn độ trong suốt**: Các pixel trong suốt sẽ xuất hiện dưới dạng màu nền trong trình xem. Khi xuất trong quá trình xử lý, độ trong suốt được giữ nguyên ở định dạng PNG nhưng không ở định dạng JPG.
{% endhint %}

#### 3. Nền chỉ mục

* Điểm ảnh **ngoài phạm vi**hiển thị ở**thang độ xám** (hiển thị giá trị chỉ mục thô)
* Các điểm ảnh **trong phạm vi**hiển thị**độ dốc màu*** ** Trường hợp sử dụng**: Làm nổi bật tinh tế, duy trì ngữ cảnh trong khi nhấn mạnh các lĩnh vực quan tâm
* **Ví dụ**: Làm nổi bật màu thực vật bị căng thẳng (NDVI 0,3-0,5) trong khi hiển thị các vùng khỏe mạnh bằng màu xám

#### 4. Nền gốc

* Các pixel **ngoài phạm vi**hiển thị**hình ảnh đa phổ gốc***Các điểm ảnh** trong phạm vi**hiển thị**độ dốc màu*** ** Trường hợp sử dụng**: Trực quan nhất - kết hợp bối cảnh hình ảnh tự nhiên với lớp phủ màu phân tích
* **Ví dụ**: Xem diện mạo cánh đồng/cây trồng thực tế với các vùng căng thẳng được mã hóa màu được phủ lên

### Chọn chế độ cắt phù hợp

| Chế độ cắt | Tốt nhất cho | Phong cách trực quan |
| -------------------------- | ------------------------------------------ | ---------------------------- |
|**Tối thiểu và Tối đa** | Hiển thị đầy đủ dữ liệu, phân tích khoa học | Tất cả các pixel được tô màu |
|**Nền trong suốt** | Lớp phủ GIS, cô lập các phạm vi cụ thể | Màu sắc trên phạm vi, khoảng trống ngoài |
|**Nền chỉ mục** | Nhấn mạnh tinh tế, duy trì bối cảnh dữ liệu | Màu sắc trong phạm vi, màu xám xa hơn |
|**Hình nền gốc** | Báo cáo, thuyết trình, phân tích trực quan | Màu sắc trên phạm vi, hình ảnh xa hơn |

### Tạo màu LUT tùy chỉnh

Để kiểm soát hoàn toàn khả năng hiển thị của mình, bạn có thể tạo**chuyển màu tùy chỉnh**bằng cách chỉnh sửa các điểm dừng màu riêng lẻ.**Để tạo dải màu tùy chỉnh:**1. Trong bảng LUT, tìm** thanh xem trước gradient**2. Tìm** hình vuông mẫu màu**bên dưới dải màu
3.**Nhấp vào điểm dừng màu**để chọn nó
4.**Bộ chọn màu** mở ra
5. Chọn màu mới bằng cách sử dụng:
   * **Bánh xe màu**: Lựa chọn màu sắc trực quan
   * **Thanh trượt RGB/HSV**: Kiểm soát màu chính xác
   * **Mục nhập mã hex**: Thông số màu chính xác (ví dụ: `#FF0000` cho màu đỏ)
6. Nhấp vào bộ chọn màu**để áp dụng màu mới**7. Độ dốc** cập nhật ngay lập tức**trên hình ảnh**Thêm hoặc xóa các điểm dừng màu:***
**Thêm điểm dừng**: Nhấp vào biểu tượng + để thêm mẫu màu mới vào cuối
* **Xóa điểm dừng**: Nhấp đúp chuột vào ô vuông màu để xóa mẫu màu**Chiến lược tùy chỉnh:***
**Đảo ngược gradient**: Lật thứ tự màu để đảo ngược ý nghĩa (ví dụ: xanh lá cây=thấp, đỏ=cao)
* **Màu sắc thương hiệu**: Phù hợp với bảng màu của tổ chức bạn cho các báo cáo
* **Thân thiện với người mù màu**: Sử dụng kết hợp màu cam-xanh hoặc tím-vàng
* **Tối ưu hóa in**: Chọn màu phù hợp với cả in màu và in thang độ xám
* **Đa ngưỡng**: Sử dụng các màu riêng biệt ở ngưỡng giá trị cụ thể để phân loại

{% hint style="info" %}**Lưu các gradient tùy chỉnh**: Các gradient tùy chỉnh có thể được lưu và sử dụng lại. Nhấp vào biểu tượng lưu trong bảng LUT để giữ nguyên cách phối màu tùy chỉnh của bạn để sử dụng trong tương lai.
{% endhint %}***

## Quy trình làm việc tương tác

### Cập nhật theo thời gian thực

Tất cả các điều chỉnh LUT trong sandbox đều cập nhật hình ảnh**ngay lập tức và tương tác**:

* **Chuyển lớp** → Hình ảnh thay đổi ngay lập tức
* **Chọn độ dốc** → Màu sắc cập nhật ngay lập tức
* **Điều chỉnh phạm vi giá trị** → Thay đổi độ tương phản trong thời gian thực
* **Thay đổi lớp** → Cập nhật độ mượt của gradient ngay lập tức
* **Sửa đổi phần cắt** → Hiển thị nền thay đổi ngay lập tức
* **Chỉnh sửa màu**→ Dải màu tùy chỉnh sẽ được áp dụng ngay lập tức**Không cần nút "Áp dụng"**- tất cả các thay đổi đều trực tiếp và tương tác!

{% hint style="success" %}**Phản hồi trực tiếp**: Phản hồi trực quan tức thì cho phép bạn nhanh chóng thử nghiệm các cài đặt khác nhau cho đến khi bạn tìm thấy hình ảnh trực quan tối ưu cho nhu cầu phân tích của mình.
{% endhint %}

### Quy trình làm việc sàng lọc lặp đi lặp lại**Quy trình tối ưu hóa LUT điển hình:**

1.**Chọn lớp chỉ mục**(ví dụ: RAW (Phản xạ))
2.**Áp dụng chỉ mục**- Chọn bộ lọc máy ảnh và công thức chỉ mục, kéo các vòng tròn màu đến vị trí thích hợp trong công thức chỉ mục
3.**Áp dụng độ dốc LUT**- Bắt đầu với cài đặt trước Đỏ-Vàng-Xanh
4.**Kiểm tra giá trị pixel**- Di chuyển con trỏ xung quanh, ghi chú phạm vi giá trị
5.**Điều chỉnh tối thiểu/tối đa**- Thu hẹp để tập trung vào thảm thực vật (ví dụ: 0,2 đến 0,9)
6.**Chọn cắt**- Thử "Nền gốc" để biết ngữ cảnh
7.**Tinh chỉnh màu sắc**- Tùy chỉnh độ chuyển màu nếu cần để có điểm nhấn cụ thể
8.**Hoàn tất cài đặt**- Cài đặt tài liệu và sao chép vào Cài đặt dự án để xử lý xuất

### Kiểm tra giá trị pixel

Hiểu giá trị pixel thực tế là rất quan trọng để thiết lập phạm vi LUT hiệu quả:**Cách kiểm tra giá trị:**

1. Giá trị pixel hiển thị khi hình ảnh có Chỉ mục hoặc cả hai hộp Chỉ mục và LUT**được chọn**.
2.**Di chuyển con trỏ**qua các vùng khác nhau của hình ảnh
3.**Quan sát giá trị pixel**hiển thị trong chú giải khi bạn di chuột
4. Phóng to để xem từng pixel được đánh dấu bằng giá trị nổi
5.**Ghi chú** phạm vi giá trị cho các tính năng khác nhau:
   * **Thảm thực vật khỏe mạnh**: ví dụ: NDVI 0,55-0,85
   * **Thực vật bị căng thẳng**: ví dụ: NDVI 0,30-0,50
   * **Đất trống**: ví dụ: NDVI 0,05-0,25
   * **Nước**(nếu có): ví dụ: NDVI -0,05 đến 0,10**Sử dụng giá trị pixel để đặt phạm vi LUT:** Sau khi kiểm tra giá trị pixel, hãy điều chỉnh LUT tối thiểu/tối đa cho phù hợp:**Tình huống ví dụ:***
**Quan sát**: Giá trị đất = 0,05-0,25, Căng thẳng = 0,25-0,50, Khỏe mạnh = 0,50-0,85
* **Mục tiêu**: Chỉ hình dung tình trạng thực vật (không bao gồm đất)
* **Cài đặt LUT**: Tối thiểu = `0,25`, Tối đa = `0,85`
* **Cắt**: "Nền gốc" để xem đất có màu sắc tự nhiên
* **Kết quả**: Độ dốc màu chỉ áp dụng cho thảm thực vật, đất hiển thị như ảnh gốc

{% hint style="info" %}**Phạm vi động**: Các loại cây trồng, mùa vụ và giai đoạn tăng trưởng khác nhau sẽ có phạm vi giá trị khác nhau. Luôn kiểm tra giá trị pixel trong tập dữ liệu cụ thể của bạn trước khi đặt phạm vi LUT.
{% endhint %}***

## Chỉ số tùy chỉnh (Chloros+)

### Tạo công thức chỉ mục tùy chỉnh

{% hint style="info" %}**Nơi tạo**: Bạn có thể định cấu hình các chỉ mục tùy chỉnh trong**Cài đặt dự án**trước khi xử lý, cũng như trong thanh bên hộp cát của Trình xem hình ảnh.
{% endhint %}**Để tạo chỉ mục tùy chỉnh:**1.** Mở Cài đặt dự án**(trước khi xử lý) hoặc thanh bên hộp cát của Trình xem hình ảnh
2. Điều hướng đến**danh sách thả xuống Công thức chỉ mục**

3. Tìm tùy chọn**"Tùy chỉnh"**(phải đăng nhập bằng giấy phép Chloros+)
4.**Xác định công thức** bằng cách sử dụng các biến số trong băng tần:
   * Tên ban nhạc: `NIR`, `Red`, `Green`, `Blue`, `RedEdge`, v.v.
   * Toán tử: `+`, `-`, `*`, `/`, `^` (số mũ)
   * Hàm: `sqrt()`, `abs()`, v.v. (nếu được hỗ trợ)
   * Dấu ngoặc đơn: `()` cho thứ tự thực hiện các thao tác
5. **Đặt tên cho chỉ mục của bạn**(ví dụ: "MyIndex" hoặc "CustomNDVI")
6.**Lưu cấu hình** **Ví dụ về công thức tùy chỉnh:**

```

NDVI đã sửa đổi có bù:
(NIR - Đỏ) / (NIR + Đỏ + 0,5)

Tỷ lệ đơn giản:
NIR / Đỏ

Đa băng tần phức tạp:
(NIR - Đỏ) / (NIR + Đỏ - Xanh)

Chỉ số mũ:
(NIR / Đỏ) ^ 2
```

{% hint style="warning" %}**Xác thực công thức**: Đảm bảo công thức của bạn sử dụng các băng tần có sẵn trong máy ảnh của bạn. Ví dụ: RedEdge chỉ khả dụng trên máy ảnh có bộ lọc RedEdge.
{% endhint %}***

## Các bước tiếp theo

Bây giờ bạn đã hiểu Hộp cát Index/LUT:

* **Áp dụng để xử lý**: Sử dụng các cài đặt được phát hiện trong [Cài đặt dự án](../project-settings/project-settings.md)
* **Quy trình hàng loạt**: Áp dụng các chỉ mục được tối ưu hóa cho bộ dữ liệu đầy đủ
* **Tìm hiểu thêm**: Đọc [Công thức chỉ mục đa phổ](../project-settings/multispectral-index-formulas.md)

Tài liệu liên quan:

* [ **Image Layers**](image-layers.md) - Quản lý và hiển thị lớp
* [ **Mở một hình ảnh toàn màn hình**](opening-an-image-full-screen.md) - Thông tin cơ bản về Trình xem hình ảnh
* [ **Đang xử lý hình ảnh (GUI)**](../processing-images-gui/adding-files-to-a-project.md) - Quy trình xử lý đầy đủ
