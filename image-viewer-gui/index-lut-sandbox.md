# Box cát chỉ mục/LUT

Hộp cát Index/LUT là một không gian làm việc tương tác trong Trình xem ảnh Chloros cho phép bạn thử nghiệm các phép tính chỉ số đa phổ và trực tuyến hóa màu sắc trong thời gian thực. Công cụ mạnh mẽ này giúp bạn kiểm tra các chỉ mục khác nhau, điều chỉnh phạm vi giá trị và tạo bản xuất sẵn có sẵn trực tuyến mà không cần xử lý lại toàn bộ dữ liệu của bạn.

## Sandbox Index/LUT là gì?

### Đích đến

Cung cấp hộp cát:

* **Tính toán chỉ số thời gian thực** - Áp dụng bất kỳ chỉ số thực vật nào ngay lập tức
* **Điều chỉnh LUT tương tác** - Tinh chỉnh chuyển màu và phạm vi màu
* **Công việc ưu tiên hóa quy trình** - Xác định cài đặt tốt nhất trước khi xử lý hàng loạt

### Sandbox để xử lý dự án

**Hộp cát chỉ mục/LUT (Tương tác):**

* Hình ảnh duy nhất tại một thời điểm
* Phản hồi tức thì
* Thử nghiệm và lặp lại
* Không có sự thay đổi vĩnh viễn cho tập tin
* Hoàn hảo để khám phá và thử nghiệm

**Xử lý dự án (hàng loạt):**

* Toàn bộ tập tin cùng một lúc
* Cài đặt sẵn cấu hình
* Tệp đầu ra vĩnh viễn
* Uốn nhiều thời gian
* Tốt nhất khi cài đặt hoàn tất

{% hint style="success" %}
**Quy trình làm việc tốt nhất**: Sử dụng hộp cát để thử nghiệm và tìm chỉ mục cài đặt và tối ưu LUT, sau đó áp dụng các cài đặt đó trong quá trình xử lý dự án tập tin cho toàn bộ dữ liệu của bạn.
{% endhint %}

***

##Làm việc với Box sand Index/LUT

### Tìm hiểu các số được tính toán trước đó

Trong Chloros, các mục có thể được áp dụng trong quá trình xử lý dự án. Để xác định cài đặt chỉ mục và LUT bất kỳ ứng dụng nào bạn muốn áp dụng để xuất, cách dễ nhất là sử dụng hộp xem hình ảnh hộp cát.

Hộp cát cho phép bạn:

* **Áp dụng chỉ mục mới và màu gradient (LUT)** để trực quan hóa dữ liệu
* **Điều chỉnh màn hình cài đặt** một cách tương thích
* **Xem** chỉ mục hình ảnh đã được tính toán
* **Kiểm tra** pixel giá trị ở mọi mức thu phóng

### Mở Sandbox

Box cát Index/LUT được truy cập trong tab thanh bên **Image Viewer** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">:

1. Bấm vào một hình ảnh trong lưới hình ảnh của tệp trình duyệt. Nó sẽ mở trong tab **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
2. Click vào tab **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> để mở thanh bên bật ra bên trái nếu nó chưa mở

### Chọn một hình ảnh để áp dụng Chỉ mục/LUT cho

Để làm việc với một mục trong hộp cát <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> của Trình xem ảnh:

1. **Mở một hình ảnh** từ mạng hình ảnh chính bằng cách nhấp vào nó
2. Sau đó, tab **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> sẽ mở
3. Bước vào **Đơn giản thả xuống lớp** (ở cùng bên phải của trình xem)
4. Chọn lớp từ danh sách thả xuống:
   * RAW (Phản xạ)

### Áp dụng chỉ mục cho hình ảnh

Khi hiển thị ảnh ở chế độ toàn màn hình và tab thanh bên **Image Viewer** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> sẽ mở:

1. Chọn hộp thư ở đầu thanh bên
2. Chọn bộ lọc máy ảnh của bạn từ menu thả xuống bên trái
3. Chọn công thức chỉ mong muốn từ danh sách thả xuống bên phải
4. Kéo sợi kênh tròn màu đến các vị trí trong công thức chỉ số bên dưới
5. Khi hợp lệ công thức, hình ảnh sẽ cập nhật và hiển thị các chỉ số giá trị
6. Di chuyển con trỏ chuột xung quanh để xem các giá trị tại vị trí của con trỏ
7. Phóng to để xem từng pixel và giá trị liên quan của chúng

Mỗi số chỉ có một công cụ giá trị phạm vi và ý nghĩa:

#### Ví dụ NDVI

```

Formula: (NIR - Red) / (NIR + Red)

For Survey3W RGN camera:
NIR = 850nm band
Red = 661nm band

Result range: -1.0 to +1.0
Typical vegetation: 0.4 to 0.9
Stressed vegetation: 0.2 to 0.4
Bare soil: 0.0 to 0.2
Water: -0.1 to 0.1
```

Để biết đầy đủ tài liệu về công thức chỉ mục công thức, hãy xem [Đa phổ chỉ mục công thức](../project-settings/multispectral-index-formulas.md).

***

## Làm việc với LUT (Bảng nghiên cứu)

### LUT là gì?

**Bảng tra cứu (LUT)** ánh xạ các giá trị chỉ số thành phần màu sắc để trực quan hóa:

* **Đầu vào**: Mục lục pixel giá trị (Ví dụ: NDVI 0,65)
* **Đầu ra**: Màu RGB (ví dụ: xanh lục sáng)
* **Mục đích**: Làm cho các mẫu dễ nhìn và diễn giải hơn**Thang độ xám so với LUT màu:**

* Thang độ xám: Khoa học và trung tính, hiển thị dữ liệu thô
* Màu LUT: Trực quan và có tác động mạnh mẽ, làm nổi bật các mẫu và sự khác biệt

{% hint style="success" %}
**Sức mạnh trực quan**: Áp dụng LUT màu cho hình ảnh chỉ mục thang độ xám giúp xác định nhanh chóng các mẫu, điểm bất thường và khu vực quan tâm dễ dàng hơn đáng kể.
{% endhint %}

### Áp dụng LUT cho chỉ mục ảnh

Khi bạn có một màn hình chỉ mục hình ảnh

1. Bấm vào nút <img src="../.gitbook/assets/image (1) (1).png" alt="" data-size="line"> nút "+Thêm LUT"
2. Choose color gradient
3. Điều chỉnh điểm cắt tối thiểu/tối đa
4. Điều chỉnh chế độ cắt
5. Chọn hộp mục trong thanh bên **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> để áp dụng LUT

### Choose a range color

**Chọn chuyển màu:**

1. Trong LUT bảng, tìm**thanh gradient màu**

2. Di chuột qua nó để xem các cài đặt trước độ dốc có sẵn
3. Select gradient mong muốn
4. Hình ảnh **cập nhật ngay lập tức** với màu sắc mới khi chỉ mục được chọn

{% hint style="success" %}
**Phương pháp tốt nhất**: Đối với các số thực vật như NDVI, độ dốc Red-Yellow-Green là trực quan nhất vì nó phù hợp với các liên kết màu sắc tự nhiên (xanh lá cây=khỏe mạnh, vàng=trung bình, đỏ=căng thẳng).
{% endhint %}

### Điều chỉnh các lớp màu

**Điều khiển lớp**xác định số bước màu riêng biệt xuất hiện trong phạm vi màu của bạn:**Tùy chọn số lớp:*** **2-5 lớp**: Danh mục rất rộng, khu vực riêng biệt
* **6-10 lớp**: Cân bằng, tốt cho việc phân loại
* **11-20 lớp**: Chuyển màu mượt, xuất hiện liên tục
* **Hơn 20 lớp**: Gần như liên tục, độ tối tối đa**Cách chỉnh sửa:**

1. Trong LUT bảng, xác định vị trí**ô mẫu màu bên dưới thanh gradient**

2. Điều chỉnh lớp bằng cách thêm nút +
3. Xóa số lớp bằng cách nhấp vào mẫu màu
4. Cập nhật độ dốc **theo thời gian thực**trên hình ảnh**Hiệu ứng trực quan:*** **Ít lớp hơn** (3-5): Tạo các vùng riêng biệt, phân loại đơn giản hóa, dễ phân biệt danh mục hơn
* **Lớp trung bình** (6-10): Cách tiếp cận cân bằng, phù hợp với hầu hết các ứng dụng
* **Các lớp khác** (15-20): Chuyển tiếp mượt mà, biến thể chi tiết, hình thức chụp ảnh**Khi nào nên sử dụng:*** **Ít lớp (3-5)**: Slide trình bày, loại sơ đồ phân loại, báo cáo đơn giản
* **Lớp trung bình (6-10)**: Phân tích tổng, cân bằng chi tiết, báo cáo tiêu chuẩn
* **Nhiều lớp (15-20)**: Phân tích khoa học, kiểm tra chi tiết, đầu ra chất lượng công bố

### Tinh chỉnh giá trị phạm vi

**Kiểm soát phạm vi giá trị**xác định giá trị của bất kỳ mục tiêu giá trị nào trong phạm vi màu của bạn:**Kiểm soát phạm vi trong LUT bảng:*** **Tối thiểu giá trị**: Giới hạn dưới của thang màu
* **Giá trị tối đa**: Giới hạn trên của thang màu
* **Giá trị trung bình**: Tự động phân tích giữa mức tối thiểu và tối đa (dựa trên số lượng lớp)

#### Điều chỉnh Giá trị tối thiểu/Tối đa

**Để điều chỉnh giá trị phạm vi:**

1. Trong LUT bảng, tìm các trường**Giá trị tối thiểu**và**Giá trị tối đa**

2. Đột nhập vào**Giá trị tối thiểu**

3. Nhập giá trị tối thiểu mong muốn (ví dụ: `0.2`)
4. Nhấn **Enter** hoặc nhấp vào bên ngoài trường
5. Lặp lại cho trường **Giá trị tối đa** (ví dụ: `0.9`)
6. Hình ảnh **cập nhật ngay lập tức**{% hint style="info" %}**Tự động chia tỷ lệ**: Khi bạn áp dụng LUT lần đầu tiên, Chloros sẽ tự động thiết lập mức tối thiểu/tối đa cho phạm vi dữ liệu thực tế trong hình ảnh. Sau đó, bạn có thể thu hẹp phạm vi này để tập trung vào phạm vi giá trị cụ thể mà bạn quan tâm.
{% endhint %}

**Ví dụ điều chỉnh phạm vi NDVI:*** **Phạm vi đầy đủ**: `-1.0` đến `1.0` (hiển thị tất cả các giá trị có thể)
* **Tập trung vào thảm thực vật**: `0.2` đến `0.9` (không bao gồm đất trống và nước)
* **Chỉ thực vật khỏe mạnh**: `0.5` đến `0.9` (chỉ dấu dấu những thực vật mạnh mẽ)
* **Phát hiện căng thẳng**: `0.2` đến `0.5` (nhấn mạnh các khu vực có vấn đề)
* **Phạm vi tùy chỉnh**: Điều chỉnh dựa trên pixel giá trị được quan sát của bạn**Tại sao phải điều chỉnh phạm vi?*** **Tăng độ tương phản** trong lĩnh vực bạn quan tâm
* **Loại trừ các giá trị không liên quan** (ví dụ: vùng nước, đất trống)
* **Chẩn hóa trực tuyến** trên nhiều hình ảnh hoặc ngày
* **Nhấn mạnh những điều khác biệt nhỏ** trong phạm vi giá trị thu hẹp

### Cắt giảm giá trị ngoài phạm vi

Khi pixel giá trị nằm ngoài phạm vi xác định tối thiểu/tối đa của bạn, bạn có thể kiểm soát cách chúng hiển thị bằng cách sử dụng **cắt chế độ**.

#### **Có sẵn chế độ cắt tùy chọn:**

#### 1. Tối thiểu và tối đa

* Điểm ảnh **dưới mức tối thiểu**→ hiển thị bằng cách sử dụng**màu đầu tiên** trong phạm vi màu (ví dụ: màu đỏ)
* Điểm ảnh **trên mức tối đa**→ hiển thị bằng cách sử dụng**màu cuối cùng** trong phạm vi màu (ví dụ: màu xanh lá cây)
* **Trường hợp sử dụng**: Nhấn mạnh đến cực chất, hiển thị đầy đủ phạm vi dữ liệu với màu sắc bão hòa ở giới hạn
* **Ví dụ**: Các giá trị NDVI dưới 0,2 đều có màu đỏ, các giá trị trên 0,9 đều có màu xanh lục

#### 2. Nền tảng vững chắc

* Các pixel **nngoài phạm vi**trở nên**hoàn toàn trong suốt***Chỉ các pixel**trong phạm vi** hiển thị phạm vi màu
* **Trường hợp sử dụng**: Lớp phủ GIS, phân biệt các phạm vi giá trị cụ thể, chỉ làm nổi bật các lĩnh vực quan tâm
* **Ví dụ**: Chỉ hiển thị NDVI 0,4-0,7 ở màu sắc, mọi thứ khác trong suốt

{% hint style="warning" %}
**Giới hạn độ trong suốt**: Các pixel trong suốt sẽ hiển thị dưới dạng màu nền trong quá trình xem. Khi xuất trong quá trình xử lý, mức độ được giữ nguyên ở định dạng PNG nhưng không ở định dạng JPG.
{% endhint %}

#### 3. Mục chỉ nền tảng

* Điểm ảnh **ngoài phạm vi**hiển thị ở**thang độ xám** (hiển thị thô chỉ mục giá trị)
* Các điểm ảnh **trong phạm vi**hiển thị**độ dốc màu*** **Trường hợp sử dụng**: Làm nổi bật tinh tế, duy trì ngữ cảnh trong khi nhấn mạnh các lĩnh vực quan tâm
* **Ví ví**: nổi bật màu thực vật bị căng thẳng (NDVI 0,3-0,5) khi hiển thị các vùng khỏe mạnh bằng màu xám

#### 4. Nền tảng gốc

* Các pixel **ngoài phạm vi**hiển thị**hình ảnh đa phổ gốc***Các điểm ảnh**trong phạm vi**hiển thị**độ dốc** **Trường hợp sử dụng**: Trực quan nhất - kết hợp hình ảnh bối cảnh tự nhiên với phân tích lớp phủ màu
* **Ví dụ**: Xem diện mạo cánh đồng/cây trồng thực tế với các vùng căng thẳng được mã hoá màu được phủ lên

###Chọn chế độ cắt phù hợp

| Cut mode | Tốt nhất cho | Phong cách trực quan |
| -------------------------- | ------------------------------------------ | ---------------------------- |
| **Tối thiểu và tối đa** | Hiển thị đầy đủ dữ liệu, phân tích khoa học | Tất cả các pixel được tô màu |
| **Nền trong suốt** | Lớp phủ GIS, cô lập các công cụ vi phạm | Màu sắc trên phạm vi, khoảng trống ngoài |
| **Mục chỉ số** | Nhấn mạnh tinh tế, duy trì bối cảnh dữ liệu | Màu sắc trong phạm vi, màu xám xa hơn |
| **Hình nền gốc** | Báo cáo, trình bày, phân tích trực quan | Màu sắc trên phạm vi, hình ảnh xa hơn |

### Tạo tùy chỉnh LUT màu

Để kiểm soát khả năng hiển thị hoàn toàn của mình, bạn có thể tạo **điều chỉnh màu chuyển tiếp**bằng cách chỉnh sửa các điểm dừng màu riêng biệt.**Để tạo tùy chỉnh phạm vi màu:**

1. Trong bảng LUT, tìm**thanh xem trước gradient**

2. Tìm**hình vuông mẫu màu** bên dưới dải màu
3. **Nhấp vào điểm dừng màu** để chọn nó
4. **Mở ra bộ chọn màu**

5. Chọn màu mới bằng cách sử dụng:
   * **Bánh xe màu**: Vòng chọn màu sắc trực quan
   * **Thanh trượt RGB/HSV**: Kiểm soát màu sắc chính xác
   * **Hex mã hóa mục nhập**: Thông số màu chính xác (ví dụ: `#FF0000` cho màu đỏ)
6. Click vào bộ chọn màu **để áp dụng màu mới**

7. Độ dốc**cập nhật ngay lập tức**trên hình ảnh**Thêm hoặc xóa các điểm dừng màu:*** **Thêm điểm dừng**: Nhấn vào biểu tượng + để thêm mẫu màu mới vào cuối
* **Xóa điểm dừng**: Cú nhấp chuột vào ô vuông màu để xóa mẫu màu**Chiến lược tùy chỉnh:*** **Đảo ngược gradient**: Lật thứ tự màu để đảo ngược ý nghĩa (ví dụ: xanh lá cây=thấp, đỏ=cao)
* **Màu sắc thương mại**: Phù hợp với bảng màu của tổ chức bạn cho báo cáo
* **Tân thiện với người mù màu**: Sử dụng kết hợp màu cam-xanh hoặc tím-vàng
* **Ưu hóa in**: Chọn màu phù hợp với cả in màu và thang độ xám
* **Đa ngưỡng**: Sử dụng các màu riêng biệt ở ngưỡng giá trị công cụ để phân loại

{% hint style="info" %}
**Lưu tùy chỉnh độ dốc**: Tùy chỉnh độ dốc có thể được lưu và sử dụng lại. Click vào biểu tượng lưu trong LUT bảng để chứa các tùy chọn chỉnh sửa màu sắc của bạn nhằm sử dụng trong tương lai.
{% endhint %}

***

## Tương tác quy trình làm việc

### Update cập nhật theo thời gian thực

Tất cả các điều chỉnh LUT trong sandbox đều cập nhật hình ảnh **ngay lập tức và tương tác**:

* **Chuyển lớp** → Hình ảnh thay đổi ngay lập tức
* **Chọn độ dốc** → Cập nhật màu sắc ngay lập tức
* **Điều chỉnh phạm vi giá trị** → Thay đổi độ tương phản trong thời gian thực
* **Thay đổi lớp** → Cập nhật độ dốc mượt mà ngay lập tức
* **Sửa phần cắt** → Hiển thị thay đổi nền ngay lập tức
* **Chỉ sửa màu**→ Tùy chỉnh màu sắc sẽ được áp dụng ngay lập tức**Không cần nút "Áp dụng"** - Tất cả các thay đổi đều trực tiếp và tương tác!

{% hint style="success" %}
**Phản hồi trực tiếp**: Phản hồi trực tiếp sẽ cho phép bạn nhanh chóng thử nghiệm các cài đặt khác khi bạn tìm thấy hình ảnh trực tiếp tối ưu cho nhu cầu phân tích của mình.
{% endhint %}

### Công việc chuẩn bị sẵn sàng để lặp lại vòng lặp

**Màn hình LUT tối ưu hóa quy trình:**

1.**Chọn lớp chỉ mục** (Ví dụ: RAW (Phản xạ))
2. **Áp dụng chỉ mục** - Chọn bộ lọc máy ảnh và chỉ mục công thức, kéo các vòng tròn màu đến vị trí thích hợp trong chỉ mục công thức
3. **Áp dụng gradient LUT** - Bắt đầu cài đặt trước Red-Yellow-Green
4. **Kiểm tra pixel giá trị** - Di chuyển con trỏ xung quanh, ghi chú phạm vi giá trị
5. **Điều chỉnh tối thiểu/tối đa** - Thu gọn để tập trung vào thảm thực vật (Ví dụ: 0,2 đến 0,9)
6. **Chọn cắt** - Thử "Nền gốc" để biết ngữ cảnh
7. **Tinh chỉnh màu sắc** - Màu chuyển tùy chỉnh nếu cần để có thể nhấn vào công cụ
8. **Hoàn tất cài đặt**- Cài đặt tài liệu và sao chép vào Cài đặt dự án để xử lý xuất

### Kiểm tra pixel giá trị

Hiểu biết thực tế về giá trị pixel là rất quan trọng để thiết lập phạm vi Kết quả hiệu quả LUT:**Cách kiểm tra giá trị:**

1. Pixel có giá trị hiển thị khi hình ảnh có Chỉ mục hoặc cả hai mục Mục và LUT**được chọn**.
2. **Di chuyển con trỏ** qua các hình ảnh khác nhau của vùng
3. **Quan sát các pixel giá trị** được hiển thị trong phần chú thích khi bạn di chuột
4. Phóng to để xem từng pixel được đánh dấu bằng giá trị nổi
5. **Ghi chú** phạm vi giá trị cho các tính năng khác nhau:
   * **Thảm thực vật khỏe mạnh**: ví dụ: NDVI 0,55-0,85
   * **Thực vật bị căng thẳng**: ví dụ: NDVI 0,30-0,50
   * **Đất trống**: ví dụ: NDVI 0,05-0,25
   * **Nước**(nếu có): ví dụ: NDVI -0,05 đến 0,10**Dùng pixel giá trị để đặt phạm vi LUT:**Sau khi kiểm tra giá trị pixel, hãy điều chỉnh LUT tối thiểu/tối đa cho phù hợp:**Tình huống ví dụ:*** **Quan sát**: Giá trị đất = 0,05-0,25, Sử dụng = 0,25-0,50, mạnh = 0,50-0,85
* **Mục tiêu**: Chỉ hình dung tình trạng thực vật (không bao gồm đất)
* **Cài đặt LUT**: Tối thiểu = `0.25`, Tối đa = `0.85`
* **Chắt**: "Nền gốc" để xem đất có màu sắc tự nhiên
* **Kết quả**: Độ dốc màu chỉ áp dụng cho thảm thực vật, đất hiển thị như ảnh gốc

{% hint style="info" %}
**Phạm vi động**: Các loại cây trồng, mùa vụ và giai đoạn tăng trưởng khác nhau sẽ có phạm vi giá trị khác nhau. Luôn kiểm tra pixel giá trị trong công cụ dữ liệu của bạn trước khi đặt LUT phạm vi.
{% endhint %}

***

## Chỉ tùy chỉnh số (Chloros+)

### Tạo tùy chỉnh chỉ mục công thức

{% hint style="info" %}
**Nơi tạo**: Bạn có thể định cấu hình các tùy chọn chỉ mục trong**Cài đặt dự án** trước khi xử lý, cũng như trong hộp cát của Trình xem ảnh.
{% endhint %}

**Để tạo tùy chỉnh chỉ mục:**

1.**Mở Cài đặt dự án** (trước khi xử lý) hoặc thanh bên trong cát của Trình xem ảnh
2. Điều hướng đến **danh sách thả xuống chỉ mục công thức**

3. Tìm tùy chọn**"Tùy chỉnh"** (phải đăng nhập bằng giấy phép Chloros+)
4. **Xác định công thức** bằng cách sử dụng các biến trong băng tần:
   * Tên ban nhạc: `NIR`, `Red`, `Green`, `Blue`, `RedEdge`, v.v.
   * Toán tử: `+`, `-`, `*`, `/`, `^` (số mũ)
   * Chức năng: `sqrt()`, `abs()`, v.v. (nếu được hỗ trợ)
   * Dấu ngoặc đơn: `()` cho thực hiện thứ tự
5. **Đặt tên cho chỉ mục của bạn** (Ví dụ: "MyIndex" hoặc "CustomNDVI")
6. **Lưu cấu hình**

**Ví dụ về tùy chỉnh công thức:**

```

Modified NDVI with offset:
(NIR - Red) / (NIR + Red + 0.5)

Simple ratio:
NIR / Red

Complex multi-band:
(NIR - Red) / (NIR + Red - Blue)

Exponential index:
(NIR / Red) ^ 2
```

{% hint style="warning" %}
**Xác thực công thức**: Đảm bảo công thức của bạn sử dụng các băng tần có sẵn trong máy ảnh của bạn. Ví dụ: RedEdge chỉ sử dụng được trên máy ảnh có bộ lọc RedEdge.
{% endhint %}

***

## Các bước tiếp theo

Bây giờ bạn đã hiểu được Hộp cát Index/LUT:

* **Áp dụng để xử lý**: Sử dụng các cài đặt được phát hiện trong [Cài đặt dự án](../project-settings/project-settings.md)
* **Chuỗi hàng hàng**: Áp dụng các mục chỉ được tối ưu hóa cho đầy đủ dữ liệu
* **Tìm hiểu thêm**: Đọc [Công thức đa mục mục đa phổ](../project-settings/multispectral-index-formulas.md)

Link tài liệu:

* [**Lớp hình ảnh**](image-layers.md) - Quản lý và trực quan hóa lớp
* [**Mở hình ảnh toàn màn hình**](opening-an-image-full-screen.md) - Thông tin cơ bản về Trình xem hình ảnh
* [**Dang xử lý hình ảnh (GUI)**](../processing-images-gui/adding-files-to-a-project.md) - Quy trình xử lý đầy đủ