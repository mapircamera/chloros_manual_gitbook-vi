# Môi trường thử nghiệm Index/LUT

Môi trường thử nghiệm Index/LUT là không gian làm việc tương tác nằm trong thanh bên của Trình xem hình ảnh Chloros. Bạn chọn một công thức, gán các kênh của máy ảnh vào đó, tô màu bằng hiệu ứng chuyển màu và điều chỉnh phạm vi giá trị — và hình ảnh sẽ được cập nhật trực tiếp ngay khi bạn thực hiện. Kể từ phiên bản 1.2.0, bạn cũng có thể **lưu lại những gì đã tạo**, cho một hình ảnh hoặc cho toàn bộ dự án, mà không cần xử lý lại.

## Mục đích của Sandbox

| Index/LUT Sandbox (tương tác)        | Xử lý dự án (theo lô)       |
| -------------------------------------- | -------------------------------- |
| Mỗi lần một hình ảnh, phản hồi tức thì  | Toàn bộ tập dữ liệu trong một lần chạy     |
| Thử nghiệm và lặp lại             | Cài đặt được định cấu hình sẵn          |
| Hiển thị kết quả ngay lập tức; chỉ lưu khi bạn yêu cầu  | Luôn ghi tệp sản phẩm      |
| Hoàn hảo để tìm ra cài đặt phù hợp | Tốt nhất khi cài đặt đã được chốt |

{% hint style="success" %}
**Quy trình làm việc thông thường**: điều chỉnh trong Sandbox cho đến khi hình ảnh hiển thị đúng như mong muốn, sau đó xuất trực tiếp từ Sandbox hoặc sao chép cùng các thiết lập chỉ mục và LUT vào [Cài đặt Dự án](../project-settings/project-settings.md) để lần xử lý tiếp theo áp dụng chúng vào mọi hình ảnh.
{% endhint %}

***

## Mở Sandbox

1. Nhấp vào một hình ảnh trong lưới — hình ảnh sẽ mở toàn màn hình trong tab **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line">
2. Nhấp vào biểu tượng **Trình xem hình ảnh** <img src="../.gitbook/assets/icon_image-viewer.JPG" alt="" data-size="line"> để mở thanh bên trái nếu nó chưa được mở
3. Chọn một lớp đa dải từ menu thả xuống ở góc trên bên phải — **RAW (Độ phản xạ)** là lựa chọn thông thường, vì các giá trị chỉ số được tính toán dựa trên độ phản xạ đã hiệu chuẩn có thể so sánh được giữa các hình ảnh

Thanh bên hiển thị, từ trên xuống dưới:

* tên hình ảnh và mẫu máy ảnh của nó
* nút **Xuất/Lưu hình ảnh** — xuất hiện khi đã chọn hộp kiểm Index hoặc LUT
* các hộp kiểm **Index**và**LUT**
* bảng cấu hình chỉ số
* bảng **Giá trị con trỏ** với phần hiển thị dữ liệu, biểu đồ histogram và điều khiển GSD

{% hint style="warning" %}
**Không khả dụng cho máy ảnh đơn sắc.** Trên hình ảnh LATTICE M3M đơn dải, cả hai ô chọn đều bị vô hiệu hóa, kèm chú thích _"Không khả dụng cho cảm biến đơn sắc (M3M)"_ — chỉ số đa dải không được định nghĩa trên một dải. Để tính toán chỉ số từ máy ảnh M3M, hãy kết hợp hai hoặc nhiều hình ảnh thành một chồng hình đa dải được căn chỉnh và sử dụng công cụ tính chỉ số LATTICE.
{% endhint %}

***

## Áp dụng chỉ số

1. Đánh dấu vào ô **Chỉ số** ở đầu thanh bên
2. Chọn bộ lọc của camera từ menu thả xuống bên trái (`RGN`, `OCN`, `NGB`, `RGB`, `RE`, `NIR`)
3. Chọn một công thức chỉ số từ menu thả xuống bên phải — 27 công thức có sẵn, cùng với bất kỳ công thức tùy chỉnh nào bạn đã lưu
4. Công thức sẽ hiển thị dưới dạng toán học như bên dưới, với một vòng tròn trống tại mỗi ô dải. **Kéo một vòng tròn kênh có màu vào một ô** để gán nó
5. Khi mọi ô mà công thức sử dụng đã được gán, hình ảnh sẽ cập nhật và hiển thị các giá trị chỉ số
6. Di chuyển con trỏ qua hình ảnh để xem giá trị; bảng **Giá trị con trỏ** sẽ thêm một hàng chỉ số với giá trị tại vị trí con trỏ

Nhấp đúp vào một vị trí đã liên kết để xóa nó. Công thức chưa hoàn chỉnh là trạng thái bình thường khi đang kéo, không phải là lỗi — hình ảnh chỉ đơn giản là không cập nhật cho đến khi công thức hoàn chỉnh.

Các vòng tròn kênh được mã hóa màu: đỏ = Red, xanh lá = Green, xanh dương = Blue, cam = Orange, lục lam = Cyan, tím = NIR, đỏ tía = RE. Các màu tương tự cũng được sử dụng cho các chấm kênh và đường cong biểu đồ tần số trong bảng **Giá trị con trỏ**.

### Ví dụ về công thức chỉ số đa phổ (NDVI)

```

Formula: (NIR - Red) / (NIR + Red)

For a Survey3W RGN camera:
  NIR = 850 nm band
  Red = 661 nm band

Result range:          -1.0 to +1.0
Typical vegetation:     0.4 to 0.9
Stressed vegetation:    0.2 to 0.4
Bare soil:              0.0 to 0.2
Water:                 -0.1 to 0.1
```

Để tham khảo công thức đầy đủ — bao gồm cả ba danh sách cài đặt sẵn và tên nào hoạt động ở đâu — hãy xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md).

### Khi chọn “Index” nhưng không có LUT

Hình ảnh được hiển thị dưới dạng **thang xám**, được kéo giãn giữa hai giá trị ngưỡng. Điều này là có chủ đích: hình ảnh chỉ số là dữ liệu dạng số, và thang xám là cách hiển thị trung thực nhất của nó. Thêm bảng tra cứu (LUT) khi bạn muốn hiển thị màu sắc.

***

## Làm việc với bảng tra cứu (LUT)

Một **bảng tra cứu (Look-Up Table)** ánh xạ các giá trị chỉ số sang màu sắc: nhập NDVI 0.65, xuất ra một màu xanh lá cụ thể. Nó không thay đổi dữ liệu — mà chỉ thay đổi cách bạn đọc dữ liệu đó.

### Thêm LUT

1. Nhấp vào nút **"+ Add LUT"** (<img src="../.gitbook/assets/image (1) (1) (1).png" alt="" data-size="line">) bên dưới công thức
2. Chọn một dải màu
3. Đặt giá trị tối thiểu và tối đa cho vùng cắt
4. Chọn Chế độ cắt (Clipping Mode)
5. Tích vào ô **LUT** trong thanh bên để hiển thị nó

Hộp kiểm LUT sẽ vẫn bị vô hiệu hóa cho đến khi LUT thực sự được cấu hình trên chỉ mục.

### Chọn độ dốc màu

Di chuột qua **thanh độ dốc**để mở danh sách cài đặt sẵn — Chloros cung cấp**bảy** cài đặt sẵn về độ dốc:

| # | Độ dốc                            | Hình dạng                                                               |
| - | ----------------------------------- | ------------------------------------------------------------------- |
| 1 | Red → Vàng → Green (**mặc định**)  | Phân kỳ — phù hợp với trực giác thông thường về thảm thực vật, xanh lá = khỏe mạnh |
| 2 | Tím → Vàng → Green             | Phân kỳ, với phần đầu thấp rõ rệt                                  |
| 3 | Nâu → Trắng → Blue                | Phân kỳ quanh điểm giữa sáng                                   |
| 4 | Đen → Tím → Hồng → Vàng nhạt | Tuần tự, từ tối đến sáng                                           |
| 5 | Red → Vàng → Blue                 | Phân kỳ quanh điểm giữa sáng                                   |
| 6 | Tím → Blue → Green → Vàng      | Tuần tự, từ tối đến sáng                                           |
| 7 | Orange → Trắng → Tím             | Phân kỳ quanh điểm giữa sáng                                   |

Một gradient **phân kỳ**đặt một màu trung tính ở giữa cửa sổ của bạn, điều này rất phù hợp khi điểm giữa có ý nghĩa cụ thể (một ngưỡng, một ngày cơ sở). Một gradient**từng bước** chạy theo thứ tự từ tối đến sáng, rất phù hợp cho một đại lượng chỉ có "nhiều" và "ít".

Mỗi cài đặt sẵn có bảy điểm dừng màu. Nhấp vào một cài đặt sẵn và hình ảnh sẽ được cập nhật ngay lập tức (khi hộp LUT được đánh dấu).

### Chỉnh sửa các điểm dừng màu

Dưới thanh gradient là một hàng các mẫu màu, mỗi điểm dừng tương ứng với một mẫu:

* **Thay đổi màu**: nhấp vào một mẫu màu để mở công cụ chọn màu (bánh xe màu, thanh trượt RGB/HSV, hoặc mã hex như `#FF0000`)
* **Thêm điểm dừng**: nhấp vào nút**+** ở cuối hàng — một điểm dừng màu trắng sẽ được thêm vào
* **Xóa điểm dừng**:**nhấp đúp** vào mẫu màu
* **Lưu gradient đã chỉnh sửa**: nhấp vào biểu tượng lưu bên cạnh thanh gradient để thêm gradient đã chỉnh sửa vào danh sách cài đặt sẵn, giúp bạn có thể chọn lại sau này

Gradient mà bạn đã cấu hình trên một chỉ mục sẽ được lưu trữ cùng với chỉ mục đó trong cài đặt dự án, do đó nó vẫn được giữ lại khi đóng và mở lại dự án.

**Ít điểm dừng**tạo ra các vùng riêng biệt, thể hiện như một hệ thống phân loại;**nhiều điểm dừng** tạo ra các chuyển tiếp mượt mà, gần như ảnh chụp. Ba đến năm điểm dừng phù hợp với các slide trình chiếu và bản đồ phân loại; sáu đến mười điểm dừng phù hợp với phân tích chung; mười lăm điểm dừng trở lên phù hợp với kiểm tra chi tiết và các hình minh họa trong ấn phẩm.

### Cài đặt phạm vi giá trị

Thanh trượt ngưỡng là một **thanh trượt hai đầu**chạy từ −1 đến +1, có hộp văn bản có thể chỉnh sửa ở mỗi đầu để nhập giá trị chính xác, và nút**AUTO**.

* Kéo một trong hai đầu thanh trượt, hoặc nhập số vào hộp văn bản tương ứng và nhấn Enter
* **AUTO**đặt phạm vi thành**phần trăm thứ 2 và thứ 98** của các giá trị chỉ số hợp lệ trong hình ảnh — một điểm khởi đầu tốt giúp loại bỏ các giá trị ngoại lệ. Chloros làm tròn kết quả một cách thích ứng, đến 4 chữ số thập phân đối với phạm vi rất hẹp, 3 chữ số đối với phạm vi hẹp, và 2 chữ số trong các trường hợp khác
* Bất kỳ điều chỉnh thủ công nào cũng sẽ được ưu tiên hơn chế độ AUTO cho đến khi bạn nhấn lại nút AUTO

Ví dụ về các cửa sổNDVI:

| Mục tiêu                                    | Tối thiểu | Tối đa |
| --------------------------------------- | ---- | --- |
| Hiển thị tất cả                         | −1.0 | 1.0 |
| Chỉ thực vật, loại trừ đất và nước       | 0.2  | 0.9 |
| Chỉ thảm thực vật khỏe mạnh                 | 0.5  | 0.9 |
| Nhấn mạnh tình trạng stress                        | 0.2  | 0.5 |

Việc thu hẹp cửa sổ sẽ tăng độ tương phản bên trong khu vực quan tâm của bạn và đẩy mọi thứ khác ra ngoài phạm vi — nơi **Chế độ cắt xén** quyết định điều gì sẽ xảy ra với chúng.

***

## Chế độ cắt xén

Khi giá trị chỉ số của một pixel nằm ngoài khoảng tối thiểu/tối đa, Chế độ cắt sẽ quyết định cách vẽ pixel đó.

| Nhãn menu thả xuống                  | Giá trị lưu trữ      | Các pixel nằm ngoài phạm vi sẽ được vẽ dưới dạng                                                                                                |
| ------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Tối thiểu & Tối đa** (mặc định) | `clip`            | Màu cuối gần nhất của gradient — các giá trị dưới mức tối thiểu sẽ lấy màu đầu tiên, các giá trị trên mức tối đa sẽ lấy màu cuối cùng |
| **Nền trong suốt**      | `transparent`     | Hoàn toàn trong suốt (alpha thực)                                                                                                  |
| **Nền chỉ số**| `indexColor`      | Tông xám, được kéo dài trên toàn bộ phạm vi chỉ số**đầy đủ** của hình ảnh, do đó cấu trúc nằm ngoài phạm vi vẫn hiển thị dưới dạng màu xám                |
| **Nền gốc**         | `backgroundColor` | Chính hình ảnh nền bên dưới, do đó lớp phủ màu nằm trên cảnh thực                                                |

| Chế độ                       | Phù hợp nhất cho                               | Hiệu ứng                                      |
| -------------------------- | -------------------------------------- | ----------------------------------------- |
| **Giá trị tối thiểu & tối đa**      | Hiển thị dữ liệu đầy đủ, phân tích khoa học | Mỗi pixel đều có màu                      |
| **Nền trong suốt** | Lớp phủ GIS, cô lập dải giá trị   | Màu bên trong khung, không có gì bên ngoài |
| **Nền chỉ số**       | Nhấn mạnh đồng thời giữ bối cảnh dữ liệu    | Màu bên trong, xám bên ngoài               |
| **Nền gốc**    | Báo cáo và trình bày              | Màu bên trong, ảnh bên ngoài         |

{% hint style="info" %}
**Các điểm ảnh không có dữ liệu luôn trong suốt, ở mọi chế độ.** Một điểm ảnh có chỉ số không hữu hạn (phép chia 0/0) hoặc chính xác là −1,0 hoặc +1,0 (các giá trị cảnh báo bão hòa, khi một dải có giá trị bằng 0 trong khi dải kia không) sẽ được coi là không có dữ liệu thay vì là giá trị cực trị. Điều này giúp loại bỏ các vùng sáng quá mức và vùng tối mất chi tiết khỏi thang màu của bạn, thay vì hiển thị chúng như các giá trị cực trị trong khung hình. Quy tắc này cũng xác định các pixel nào được sử dụng cho ngưỡng AUTO và biểu đồ chỉ số, do đó cả ba yếu tố này đều nhất quán với nhau.
{% endhint %}

Độ trong suốt được giữ nguyên khi xuất dưới dạng tệp ảnh định dạng tệp ảnh không nén (PNG). Định dạng này không thể được thể hiện trong JPG.

***

## Đọc giá trị trong khi điều chỉnh

Bảng **Giá trị con trỏ** bên dưới bảng cấu hình là công cụ đo lường cho Sandbox:

* Di chuyển con trỏ qua hình ảnh và đọc các giá trị nguồn theo từng kênh, cùng với giá trị chỉ số ở hàng riêng biệt
* Bật nút **INDEX** phía trên biểu đồ histogram để xem phân bố các giá trị chỉ số trong khung hình, với hai ngưỡng cắt được vẽ dưới dạng các đường đứt nét màu cam và giá trị của con trỏ dưới dạng đường màu trắng — đây là cách nhanh nhất để chọn một cửa sổ thực sự chứa dữ liệu của bạn
* Bật **CURSOR** để hiển thị các đường đánh dấu tại các giá trị dưới con trỏ
* Phóng to vượt quá 60× (hoặc ít hơn nếu đã thiết lập kích thước khối GSD) để làm nổi bật từng pixel hiển thị kèm giá trị nổi

Quy trình thực hành:

1. Ghi lại các giá trị trên thảm thực vật khỏe mạnh, thảm thực vật bị stress, đất trống và mặt nước
2. Xem các cụm đó nằm ở đâu trên biểu đồ tần số chỉ số
3. Đặt giá trị tối thiểu/tối đa để bao quanh cụm bạn quan tâm
4. Chọn chế độ cắt — _Original Background_ giữ cho cảnh xung quanh vẫn hiển thị

***

## Xuất từ Sandbox

Tất cả nội dung trên chỉ là bản xem trước trực tiếp cho đến khi bạn lưu lại. Nút **Xuất/Lưu hình ảnh(s)** ở đầu thanh bên sẽ mở ra một cửa sổ trượt qua thanh bên (thay vì che phủ hình ảnh, do đó bạn vẫn có thể nhìn thấy những gì mình đang quyết định).

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>### Tùy chọn

| Tùy chọn                          | Hiệu ứng                                                                                                                                            |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Áp dụng cho hình ảnh hiện tại**      | Lưu chính xác hình ảnh đang hiển thị, với các cài đặt này                                                                                                |
| **Áp dụng cho tất cả hình ảnh trong dự án** | Chạy lại cấu hình giống hệt trên mọi hình ảnh trong dự án. Các hình ảnh không có dải màu mà chỉ mục này yêu cầu sẽ bị bỏ qua, không được coi là lỗi |
| **Thanh độ dốc chỉ số/LUT**      | Đồng thời ghi một hình ảnh chú thích riêng cho mỗi lần xuất, kèm theo phạm vi giá trị được ghi nhãn                                                                     |
| **Biểu đồ tần số chỉ số**             | Đồng thời ghi một hình ảnh biểu đồ tần số riêng cho mỗi lần xuất, hiển thị giá trị tối thiểu/tối đa của dữ liệu và các ngưỡng cắt                                               |

Nếu **kích thước khối GSD** trên tab hình ảnh lớn hơn 1, khung điều khiển sẽ thông báo điều này trước khi bạn xác nhận: quá trình xuất sẽ lưu lại những gì bạn đang xem, bao gồm cả trung bình khối. Hãy đặt điều khiển GSD trở lại 1 trước nếu bạn muốn độ phân giải đầy đủ.

### Vị trí lưu tệp

Mỗi lần nhấp vào **Xuất**sẽ tạo ra một**thư mục mới, không bao giờ được tái sử dụng**:

```
<project folder>/Sandbox_Exports/<IndexName>_<Index|LUT>_<NNN>/
```

Ví dụ: `Sandbox_Exports/NDVI_LUT_001/`, sau đó là `Sandbox_Exports/NDVI_LUT_002/` cho lần chạy tiếp theo. Cách đánh số này được xác định bằng cách quét các tệp đã có trên đĩa, do đó nó vẫn tồn tại ngay cả khi khởi động lại hệ thống hoặc khi bạn xóa các thư mục bằng tay. Không có dữ liệu nào bị ghi đè — mục đích chính của Sandbox là so sánh một lần thử với lần thử trước đó.

Bên trong thư mục, cho mỗi hình ảnh:

| Tệp                                                   | Nội dung                                                   |
| ------------------------------------------------------ | ---------------------------------------------------------- |
| `<source name>_<IndexName>_<Index\|LUT>.png`           | Hình ảnh đã được hiển thị, từng pixel giống hệt như những gì trình xem hiển thị |
| `<source name>_<IndexName>_<Index\|LUT>_legend.png`    | Tệp phụ thanh độ dốc, nếu được yêu cầu                     |
| `<source name>_<IndexName>_<Index\|LUT>_histogram.png` | Tệp phụ biểu đồ chỉ số, nếu được yêu cầu                  |

Hai thanh bên này luôn được ghi ở **độ phân giải đầy đủ**, ngay cả khi hình ảnh chính được tính trung bình theo khối: kích thước khối là độ phân giải hiển thị, và cả hai thanh bên đều đọc các giá trị chỉ số thực sự trên từng pixel. Chúng cũng hiển thị nhiều thông tin hơn so với phiên bản trên màn hình — cả hai đều ghi lại cửa sổ kéo dãn _và_ giá trị tối thiểu/tối đa thực của dữ liệu, do đó biểu đồ được lưu trữ vẫn có thể đọc được sau nhiều tháng mà không cần mở dự án.

### Tiến trình và kết quả

Việc xuất toàn bộ dự án chỉ mất vài phút, do đó quá trình chạy sẽ báo cáo lại qua kênh tiến trình trực tiếp thay vì bị chặn:

* Thanh tiến trình hiển thị `current / total` và tệp đang được ghi
* Khi hoàn tất, khung cửa sổ sẽ báo cáo số lượng hình ảnh đã được xuất, số lượng hình ảnh bị bỏ qua và đường dẫn thư mục đầu ra
* Các hình ảnh bị bỏ qua được liệt kê kèm lý do (hiển thị tối đa năm lý do, sau đó là dòng "+N nữa"). Lý do phổ biến nhất là một lớp không có các kênh mà chỉ mục này cần
* Nếu **không** có hình ảnh nào trong dự án có thể sử dụng chỉ mục này, quá trình chạy sẽ báo lỗi thay vì để lại cho bạn một thư mục trống

Chỉ có một quá trình xuất trong môi trường thử nghiệm (sandbox) được thực thi cùng một lúc. Việc khởi chạy quá trình thứ hai trong khi quá trình đầu tiên đang diễn ra sẽ bị từ chối kèm theo thông báo rõ ràng, thay vì để hai quá trình tranh chấp cùng một tệp dự án.

### Bảng lưới hiển thị các lần chạy

Mỗi lần chạy đã hoàn tất sẽ xuất hiện dưới dạng một nút riêng biệt trên thanh công cụ [bảng lưới hình ảnh](image-grid.md), được đánh dấu là `<IndexName> <Index|LUT> <NNN>`. Đó là cách bạn so sánh các lần chạy: xuất hai lần với các độ dốc hoặc ngưỡng khác nhau, sau đó chuyển đổi giữa hai nút trên lưới.

***

## Công thức chỉ số tùy chỉnh (Chloros+)

{% hint style="info" %}
**Nơi tạo công thức**: trong thanh bên Sandbox hoặc trong**Cài đặt dự án** trước khi xử lý. Cả hai đều ghi vào cùng một danh sách cấp dự án.
{% endhint %}

1. Mở trình tính công thức tùy chỉnh từ menu thả xuống công thức chỉ số (yêu cầu đăng nhập bằng gói đăng kýChloros+ đủ điều kiện)
2. Viết công thức bằng cách sử dụng các **ký hiệu dải-khe** `x`, `y`, `z`, `a`, `b`, `c` — không phải tên dải
3. Các toán tử có sẵn: `+`, `-`, `*`, `/`, `^` và `()` để nhóm
4. Các hàm có sẵn: `sqrt()`, `log()`, `ln()`, `abs()`, `sign()`, `log1p()`, `log2()`
5. Đặt tên và lưu lại — nó sẽ xuất hiện ở cuối danh sách thả xuống công thức và bạn có thể gán các khe cắm bằng cách kéo các vòng tròn kênh, giống hệt như một preset tích hợp sẵn

```

Modified NDVI with an offset:   (y-x)/(y+x+0.5)
Simple ratio:                   y/x
Three-band difference:          (y-x)/(y+x-z)
Squared ratio:                  (y/x)^2
```

{% hint style="warning" %}
**Công thức tùy chỉnh chỉ có sẵn trên giao diện người dùng (GUI).** Tùy chọnCLI

/SDK `--indices` sẽ mở rộng danh sách 22 tên cài đặt sẵn tích hợp sẵn và tự động bỏ qua mọi thứ khác, bao gồm cả các công thức tùy chỉnh của bạn. Để xử lý hàng loạt một công thức tùy chỉnh, hãy cấu hình nó trong Cài đặt Dự án và chạy xử lý, hoặc sử dụng tính năng xuất "Áp dụng cho tất cả hình ảnh trong dự án" của Sandbox.
{% endhint %}

***

## Khắc phục sự cố

### "Lớp này không có các kênh mà chỉ số này yêu cầu"

Công thức yêu cầu vị trí kênh mà lớp hiện tại không có — ví dụ: chỉ số ba khe trên tệp một hoặc hai kênh. Chuyển sang lớp đa dải (phản xạ hoặc đã loại bỏ Bayer), hoặc chọn chỉ số phù hợp với bộ lọc của máy ảnh.

### "Không thể kết nối với hệ thống xử lý hình ảnh phía sau"

Hệ thống phía sau không phản hồi. Kiểm tra tab Nhật ký; nếu hệ thống xử lý đang khởi động lại, Sandbox sẽ tự động khôi phục khi hệ thống hoạt động trở lại.

### Hình ảnh không thay đổi khi tôi kéo một hình tròn

Công thức chưa hoàn chỉnh. Một công thức chưa hoàn chỉnh sẽ được xử lý như trạng thái kéo giữa chừng thông thường — không có gì được hiển thị và không có lỗi nào được báo cáo. Hãy điền đầy đủ vào mọi ô mà công thức sử dụng.

### Toàn bộ hình ảnh chỉ có một màu

Cửa sổ clip của bạn có thể nằm quá xa so với phạm vi dữ liệu. Nhấn **AUTO**để căn chỉnh nó với phần trăm thứ 2/98, hoặc bật biểu đồ**INDEX** để xem dữ liệu thực sự nằm ở đâu.

### Màu sắc sau khi xuất không khớp với những gì tôi thấy

Chúng phải khớp — đường dẫn xuất được thiết kế để phản ánh chính xác bản xem trước trực tiếp, bao gồm cả kênh alpha ở chế độ cắt xén, và việc tính trung bình theo khối được áp dụng _sau_ khi tô màu, hoàn toàn giống như cách trình xem thực hiện. Nếu có sự khác biệt, hãy kiểm tra xem kích thước khối GSD có thay đổi giữa lúc xem và lúc xuất hay không.

***

## Các bước tiếp theo

* [**Lớp hình ảnh**](image-layers.md) — chọn lớp nào để chạy chỉ số và ý nghĩa của các giá trị đó
* [**Mở hình ảnh toàn màn hình**](opening-an-image-full-screen.md) — thông tin hiển thị con trỏ, biểu đồ histogram và điều khiển GSD chi tiết
* [**Công thức chỉ số đa phổ**](../project-settings/multispectral-index-formulas.md) — mọi cài đặt sẵn, trên mọi bề mặt
* [**Cài đặt dự án**](../project-settings/project-settings.md) — lưu các cài đặt bạn đã tìm thấy vào một chu trình xử lý
