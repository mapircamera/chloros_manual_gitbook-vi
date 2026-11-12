# Mảng nhiều camera

Một **mảng**LATTICE là hai hoặc nhiều camera LATTICE được kết nối thành một đơn vị đồng bộ. Một camera đóng vai trò là**master**: nó phát ra xung kích hoạt GPIO phần cứng trên đường dây đồng bộ chung (mặc định là**Line2**), do đó mọi thành viên đều ghi lại cùng một khoảnh khắc. Chloros bổ sung tính năng đồng bộ thời gian PTP, xem trước trực tiếp (các ô hình ảnh riêng lẻ cho từng camera hoặc một hình ảnh tổng hợp đa dải được căn chỉnh) và chụp đồng bộ — mỗi lần chụp tạo ra một **nhóm khung hình** trong đó tất cả camera đều có cùng dấu thời gian và ID khung hình (được báo cáo dưới dạng `fid:N` trên đầu ra chụp).

Mảng là cách các máy ảnh đơn sắc (M3M) tạo ra các chỉ số thực vật — mỗi máy ảnh đóng góp một dải phổ, và mảng sắp xếp chúng thành một chồng hình đa dải phổ. Xem [Camera đơn & Chỉ số thực vật](mono-indices.md).

Có ba cách tương đương để kết nối một mảng, và tất cả đều chạy cùng một quy trình "smart-prep":

| Giao diện | Điểm vào |
| --- | --- |
| GUI | Tab Máy ảnh → **Kết nối Mảng** (nút màu xanh) |
| Giao diện dòng lệnh (CLI) | `chloros-cli lattice array-connect --serials SN1,SN2,…` (số sê-ri đầu tiên = máy chủ) |
| Python SDK | `connect_array(serials=[…])` → `ArraySession` (số sê-ri đầu tiên = máy chủ) |

Smart-prep thực hiện: kiểm tra khả năng mạng (ping ICMP DF + kiểm tra GVSP), lựa chọn cấp độ đồng bộ, dự đoán tốc độ khung hình ở kích thước khung hình được yêu cầu, quyết định truyền tải 12-bit đóng gói (xem [Vận chuyển 12-bit đóng gói](#packed-12-bit-transport)), kích hoạt PTP, tự động chọn định dạng pixel cho từng camera, thiết lập phơi sáng tự động dựa trên trạng thái đã lưu của từng camera, và cấu hình kích hoạt GPIO trên Line2.

**Cảm biến đầy đủ theo mặc định (mới trong phiên bản 1.3.0).** Connect giữ nguyên kích thước khung hình mà bạn yêu cầu — toàn bộ cảm biến 2048×1536, không gộp pixel và không cắt xén, trừ khi bạn chọn khác — và cho phép tốc độ khung hình giảm xuống mức mà máy chủ và mạng cung cấp. Ứng dụng này không bao giờ tự động gộp pixel hoặc cắt xén (phiên bản 1.2.0 tự động kích hoạt tính năng gộp pixel khi tốc độ khung hình dưới 1,5 fps). Việc gộp pixel hoặc sử dụng vùng quan tâm (ROI) nhỏ hơn là tùy chọn của bạn: trong hộp thoại kết nối, hãy chọn một tùy chọn và phần "Kết quả dự kiến" sẽ hiển thị tốc độ khung hình tương ứng; lệnh `array-connect` sẽ hiển thị các tùy chọn nhanh hơn cùng với tốc độ dự kiến của chúng.

{% hint style="info" %}
Các camera phải có thể truy cập được trên kết nối trước khi bất kỳ tính năng nào hoạt động — xem [Kết nối Camera](connecting.md) để tìm kiếm, gán địa chỉ và tải xuống dữ liệu hiệu chuẩn khi kết nối lần đầu. Đối với hệ thống nhiều camera, cài đặt vòng nhận (receive-ring) của NIC máy chủ quan trọng không kém tốc độ kết nối; bảng tổng hợp triệu chứng → cách khắc phục đầy đủ có trong [Tham chiếu CLI § Cài đặt & Tối ưu hóa NIC máy chủ](../reference/cli-reference.md#host-nic-setup--tuning-lattice-arrays).
{% endhint %}

## Hộp thoại Kết nối Mảng

Tab Camera → **Kết nối Mảng**sẽ mở trình hướng dẫn ba bước:**Chọn → Chế độ hiển thị → Cài đặt**.

### Bước 1 — Chọn camera chủ và camera phụ

<!-- SCREENSHOT-NEEDED: Array Connect dialog, Select scene, with 3-4 LATTICE cameras discovered (an FRGB among them). Table showing Camera / Serial / IP / Master radio / Slave checkbox columns in camera order (M3C first, FRGB at the top), with the FRGB pre-selected as master and the green "Sync cable found on N cameras. Any of them can be master; the RGB camera (else the first) is pre-selected." probe banner visible above the table. -->



Hộp thoại sẽ quét mạng ngay khi mở ra (“Đang quét mạng...”), sau đó kiểm tra các cáp đồng bộ (“Đang kiểm tra cáp đồng bộ...”) để xác định những camera nào đang kết nối với đường dây đồng bộ M8. Bạn cần ít nhất **2 camera** để tạo một mảng.

Các cáp đồng bộ M8 gặp nhau tại một điểm nối, do đó **bất kỳ camera nào được kết nối bằng cáp đều có thể làm camera chính**. Quá trình kiểm tra sẽ tự động gán vai trò — camera RGB (`-FRGB`) làm camera chính khi được kết nối cáp, nếu không thì camera đầu tiên được kết nối cáp sẽ làm camera chính, và các camera còn lại làm camera phụ — và hiển thị một trong ba thông báo sau:

| Thông báo | Ý nghĩa |
| --- | --- |
| "Đã phát hiện cáp đồng bộ trên {n} camera. Bất kỳ camera nào trong số đó đều có thể làm camera chính; camera RGB (hoặc camera đầu tiên nếu không có) đã được chọn sẵn." (màu xanh lá) | Thiết bị dò đã phát hiện các camera trên đường truyền đồng bộ; các ô chọn "camera chính" và "camera phụ" đã được điền sẵn. |
| "Không phát hiện máy chủ - kiểm tra cáp GPIO" (màu cam) | Không có camera nào nhận được xung kích hoạt; hãy kiểm tra dây cáp đồng bộ. Các vai trò vẫn được điền sẵn (camera RGB, nếu không thì camera đầu tiên trong danh sách, làm máy chủ) và bạn có thể thay đổi chúng. |
| "Không có cáp đồng bộ: {serials}" (màu cam) | Các camera được liệt kê không có cáp đồng bộ được kết nối. |

Bảng camera có các cột **Camera / Số sê-ri / IP / Master (radio) / Slave (ô chọn)**. Các hàng được liệt kê theo**thứ tự camera** — camera M3C (Bayer) trước camera M3M (mono); trong số các camera M3C, thứ tự là FRGB, FRGN, FOCN, rồi FNGB; các máy ảnh M3M được sắp xếp theo bước sóng bộ lọc, từ thấp nhất đến cao nhất (F450 trước F808). Danh sách máy ảnh ở thanh bên và bố cục mặc định của lưới sử dụng cùng thứ tự này.

* Chọn chính xác **một máy chủ**và**một hoặc nhiều máy con**. Nhấp lại vào ô chọn của máy chủ hiện tại sẽ hủy chọn nó. Chọn một máy chủ khác chỉ cần một cú nhấp: máy chủ mới sẽ rời khỏi danh sách máy con và máy chủ trước đó trở thành máy con (trừ khi nó không có cáp đồng bộ).
* Một camera được đánh dấu **"Không có cáp đồng bộ"** không bao giờ có thể được chọn làm camera phụ — một camera phụ không có dây kích hoạt sẽ chờ trên đường dây đồng bộ mãi mãi và truyền tín hiệu chết. Thay vào đó, hãy kết nối camera đó dưới dạng camera độc lập.
* Các camera đã được kết nối ở chế độ độc lập sẽ *không* bị vô hiệu hóa: việc kết nối vào mảng sẽ giải phóng phiên làm việc độc lập và mở lại camera trong mảng.

**Tiếp theo: Chế độ hiển thị →**sẽ được kích hoạt sau khi đã chọn một máy chủ và ít nhất một máy con.**Quét lại** sẽ chạy lại quá trình phát hiện và kiểm tra dây kết nối.

{% hint style="warning" %}
**Hủy** bị vô hiệu hóa khi quá trình quét hoặc kiểm tra đang diễn ra — việc hủy giữa chừng có thể làm treo SDK của camera trên firmware camera LATTICE. Hãy đợi cho đến khi biểu tượng quay tròn hoàn tất.
{% endhint %}

### Bước 2 — Chế độ hiển thị



<!-- SCREENSHOT-NEEDED: Array Connect dialog, Display Mode scene, showing the two selectable cards ("Separate Cameras" and "Combined Cameras") with Combined selected/highlighted as the default. -->

| Chế độ | Kết quả |
| --- | --- |
| **Camera riêng biệt** | Mỗi camera hiển thị một ô trực tiếp, tất cả được kích hoạt cùng lúc để các khung hình luôn đồng bộ. Mỗi camera giữ nguyên màu sắc và cài đặt riêng. |
| **Camera kết hợp** *(mặc định)* | Một ô hiển thị duy nhất thể hiện hình ảnh tổng hợp đa dải NDVI/chỉ số đã được căn chỉnh. Các camera chia sẻ màu sắc của mảng. |

Chế độ hiển thị chỉ thay đổi cách trình bày bản xem trước trực tiếp — hành vi ghi hình vẫn giống nhau ở cả hai chế độ.

### Bước 3 — Cài đặt mảng và kết quả

<!-- SCREENSHOT-NEEDED: Array Connect dialog, Settings scene, healthy state: left column with ROI / Binning / Trigger Rate / Wire Budget controls (1.3.0 has no "Pin resolution" checkbox), right "Projected Outcome" column showing green "Simultaneous capture" tier, an fps range, the NIC line, the "Sim-emit burst" line, and the "Wire budget" line with a checkmark. -->

dự kiến

Khi vào cảnh này, Chloros sẽ yêu cầu hệ thống nền tảng đưa ra **khuyến nghị**và áp dụng nó: sử dụng**cảm biến đầy đủ — không gộp pixel, không cắt vùng ROI — bất cứ khi nào tốc độ khung hình cho phép**, ở bất kỳ tốc độ nào mà máy chủ và đường truyền cung cấp. Chỉ khi không có tốc độ nào có thể truyền tải cảm biến đầy đủ, khuyến nghị mới thu nhỏ khung hình, và lúc đó hệ thống sẽ ưu tiên ghép pixel hơn là cắt vùng ROI, vì ghép pixel giúp giữ nguyên toàn bộ trường nhìn. Từ đó, bạn sẽ phải đánh đổi độ phân giải để lấy tốc độ khung hình. Mỗi thay đổi bạn thực hiện sẽ chạy lại phân tích trực tiếp và cập nhật bảng**Kết quả dự kiến** ở bên phải.

Cột bên trái — cài đặt:

| Điều khiển | Lựa chọn | Mặc định | Ghi chú |
| --- | --- | --- | --- |
| **ROI (Trường nhìn)** | Đầy đủ (2048×1536) / Nửa (1024×768) / Một phần tư (512×384) | Đầy đủ | Cắt cảm biến: Cắt nửa/một phần tư thành vùng nhỏ hơn ở khoảng cách điểm ảnh gốc. |
| **Binning** | 1× / 2× (tổng 2×2) / 4× (tổng 4×4) | 1× | Binning phần cứng: 2×2 = FoV đầy đủ với chi phí đường truyền bằng 1/4; 4×4 = FoV đầy đủ với chi phí đường truyền bằng 1/16. Ẩn nếu camera không hỗ trợ ghép pixel. |
| **Hình ảnh trên đường truyền** (đọc dữ liệu) | — | — | Chiều rộng × chiều cao sau khi ghép pixel thực tế được truyền qua đường dây, được làm tròn xuống thành bội số của 16 (tối thiểu 64). |
| **Tần suất kích hoạt** | 0,5–60 khung hình/giây, bước 0,1 | trống = tự động | Tần suất kích hoạt của thiết bị chủ. Để trống để Chloros tự tính toán. |
| **Dung lượng đường truyền**| 20–2000 MB/s, bước 10 | trống = tự động | Lượng dữ liệu mà máy chủ thực sự có thể xử lý, tính bằng MB/s —**con số duy nhất quyết định việc phân bổ toàn bộ mảng.** Được tự động phát hiện từ bộ điều hợp mạng. Hãy giảm giá trị này nếu mảng báo cáo các khung bị hỏng: giá trị được phát hiện thường cao hơn thực tế đối với các bộ điều hợp USB và bộ chuyển mạch dùng chung. Thay đổi giá trị này sẽ chạy lại dự báo trực tiếp. Trường này mở ra với mức ngân sách mà mảng cuối cùng được kết nối (được lưu cùng với dự án), do đó mức ngân sách mà bạn đã điều chỉnh một lần sẽ được giữ nguyên. |

Hộp kiểm **Độ phân giải pin** từ phiên bản 1.2.0 đã bị loại bỏ: quá trình kết nối luôn giữ nguyên kích thước khung mà bạn chọn và thay vào đó sẽ giảm tốc độ khung hình.

Cột bên phải — **Kết quả dự đoán**:

* **Cấp độ đồng bộ** — "Ghi đồng thời" (xanh lục), "Ghi đồng thời (phát theo kiểu FTD xen kẽ)" (xanh lục), "Ghi xen kẽ (chênh lệch 100 ms)" (vàng), hoặc "Cấu hình quá lớn" (màu đỏ). Chloros sẽ chọn mức: ghi đồng thời bất cứ khi nào đường truyền có thể được chia sẻ ở tốc độ mà các camera có thể theo kịp (tốc độ khung hình giảm khi phần chia sẻ của mỗi camera thu hẹp lại), và ghi xen kẽ chỉ khi không thể — do đó, một mảng sẽ kết nối trên bất kỳ mạng nào, với tốc độ khung hình mà mạng đó cho phép.
* **Dự báo fps** — tốc độ khung hình dự kiến mà mảng camera sẽ hoạt động. Với chế độ ghi đồng thời, đây là một con số duy nhất: tốc độ khung hình được ưu tiên hàng đầu, và tính năng phơi sáng tự động sẽ điều chỉnh trong khoảng thời gian đó, sau đó tăng độ khuếch đại, do đó cảnh thiếu sáng sẽ không làm giảm tốc độ khung hình (xem [Cách tính năng phơi sáng tự động phân bổ thời gian phơi sáng và độ khuếch đại](camera-settings.md)). Với chế độ ghi hình xen kẽ, giá trị này được hiển thị dưới dạng khoảng ("tối → sáng"), vì thời gian phơi sáng của từng camera sẽ kéo dài khoảng cách xen kẽ trong cảnh tối.
* **Dòng NIC** — tốc độ kết nối và băng thông duy trì ("NIC {mbps} Mbps · băng thông duy trì {N} MB/s").
* **Kiểm tra đợt truyền đồng thời** — liệu vòng kết nối NIC của máy chủ có thể tiếp nhận một đợt truyền đồng thời từ tất cả các camera hay không ("Đợt truyền đồng thời: X MB · Dung lượng vòng kết nối NIC khả dụng: Y MB ✓/✗").
* **Kiểm tra ngân sách băng thông** — nhu cầu tổng hợp ở trạng thái ổn định so với giới hạn an toàn va chạm của đường truyền ("Ngân sách băng thông: {demand} MB/s do {n} camera yêu cầu · giới hạn {ceiling} MB/s ✓/✗ quá tải").
* **"Số camera tối đa trên đường truyền này: {n} — được xác định bởi ngưỡng băng thông tối thiểu cho mỗi camera, do đó việc gộp dữ liệu không làm tăng con số này."** — hiển thị khi bạn sắp đạt (hoặc vượt quá) giới hạn số lượng camera.
* **"CÁC KHUNG HÌNH SẼ BỊ MẤT ở các thiết lập này."**— cảnh báo màu đỏ kèm lý do từ hệ thống phía sau, cùng danh sách các yếu tố cản trở và các**gợi ý khắc phục** màu xanh ("Để phù hợp với mảng này trên mạng" / "Để mở khóa tính năng ghi hình đồng thời").**Áp dụng & Kết nối** sẽ bị khóa cho đến khi có dự báo, và nhãn của nó sẽ cho bạn biết lý do tại sao nó bị từ chối:

| Nhãn nút | Ý nghĩa | Cách khắc phục |
| --- | --- | --- |
| "Đang phân tích..." | Quá trình phân tích vẫn đang diễn ra. | Chờ. |
| **"Quá nhiều camera cho mạng này"**| Mạng không thể truyền tải mảng camera ngay cả khi chỉ một camera tại một thời điểm (kiểm tra tổng hợp thất bại): số lượng camera vượt quá khả năng xử lý của mạng, hoặc có các camera khác đang truyền dữ liệu trên cùng mạng. | Giảm số lượng camera, sử dụng khung jumbo từ đầu đến cuối, hoặc sử dụng NIC nhanh hơn.**Giảm ROI sẽ KHÔNG giúp được gì** — xem bên dưới. |
| **"Giảm ROI để kích hoạt"** | Các khung hình sẽ bị mất ở các thiết lập này (kiểm tra burst/ring thất bại). | Giảm ROI, tăng tỷ lệ gộp pixel (binning), hoặc sửa vòng nhận tín hiệu của NIC. |

<!-- SCREENSHOT-NEEDED: Array Connect dialog, Settings scene, over-subscribed state: red "Wire budget ... over-subscribed" line, the "Max cameras on this wire" hint, and the Apply button reading "Too many cameras for this network". Reproduce by configuring more cameras than the 1 GbE ceiling (e.g. 7+ cams at 1500 MTU) or with CHLOROS-simulated models via `lattice analyze-array`. -->



Trong quá trình kết nối, một **bảng tải xuống dữ liệu hiệu chuẩn** màu xanh lá cây có thể xuất hiện kèm theo thanh tiến trình cho từng cổng nối tiếp: lần đầu tiên kết nối camera với máy, Chloros sẽ tải gói hiệu chuẩn nhà máy (~3,8 MB) từ camera qua GigE (khoảng 70 giây cho mỗi camera). Các camera đã được lưu vào bộ nhớ đệm sẽ không bao giờ hiển thị bảng này; khi có hơn sáu camera, các hàng sẽ cuộn. Xem [Kết nối camera](connecting.md).

### Truyền tải 12-bit đóng gói

Các định dạng 12-bit (`BayerRG12`, `Mono12`) thường được truyền dưới dạng các từ 16-bit. **Truyền tải**đóng gói**gửi cùng 12 bit trong 1,5 byte mỗi pixel và giải nén chúng trên máy chủ, do đó mỗi khung hình mất ít hơn 25% thời gian để gửi — ở chế độ cảm biến đầy đủ, tương đương khoảng thêm một phần ba khung hình mỗi giây.**Dữ liệu được lưu trữ là giống hệt nhau.** Chế độ kết nối mảng sẽ tự động quyết định (`auto`): nó chỉ nén khi điều đó có lợi và máy chủ có đủ tài nguyên CPU để giải nén. Giao diện người dùng (GUI) không có tùy chọn cho tính năng này; từ giao diện dòng lệnh (CLI), hãy sử dụng `array-connect --packed {auto,on,off}` (mặc định là `auto`). Các định dạng 8-bit và 16-bit không bao giờ được nén.

## Băng thông: số lượng camera tối đa

Khả năng truyền tải của mảng phụ thuộc vào đường truyền, không phải Chloros, do đó các số liệu lập kế hoạch được nêu trong tài liệu hướng dẫn phần cứng: **[Lập kế hoạch băng thông mảng](https://mapir.gitbook.io/lattice-camera/setup/array-bandwidth-planning)**.

Chloros xử lý các số liệu này như sau: hộp thoại kết nối sẽ thực hiện kiểm tra mạng, dự đoán tốc độ khung hình có thể đạt được và chọn cấp độ phù hợp. Một mảng có thể kết nối trên bất kỳ mạng nào: trên liên kết chậm, mỗi camera sẽ được điều chỉnh theo phần băng thông được phân bổ và tốc độ khung hình sẽ giảm, thay vì mảng bị từ chối hoặc các khung hình bị thu nhỏ. Chỉ khi mảng camera vượt quá khả năng truyền tải của đường truyền (khi đường truyền không thể truyền tải được ngay cả một camera tại một thời điểm) thì mảng đó mới bị từ chối (**Quá nhiều camera cho mạng này**) thay vì bỏ qua các gói tin một cách im lặng — xem bảng kết quả dự đoán được mô tả ở trên.

## Khi khung hình bị mất

Một camera có thể vắng mặt trong một nhóm đã công bố vì hai lý do hoàn toàn khác nhau,
và chúng cần các biện pháp khắc phục ngược nhau. Chloros thống kê chúng riêng biệt thay vì báo cáo một
con số “không đầy đủ” mà không chỉ ra cụ thể lý do nào:

| Điều gì đã xảy ra | Ý nghĩa | Nơi cần kiểm tra |
| --- | --- | --- |
| **Bị hỏng**— khung dữ liệu đã đến nhưng có cấu trúc sai | Mất gói tin GVSP trên đường truyền mạng |**Dung lượng đường truyền**, vòng nhận của NIC, khung dữ liệu jumbo, bộ chuyển mạch |
| **Chưa bao giờ đến**— không có khung dữ liệu nào đến cả | Camera không kích hoạt, hoặc không có dữ liệu nào được gửi đi |**Cáp đồng bộ M8**, đường dây đồng bộ, liệu mọi thành viên có được kích hoạt hay không |

Tỷ lệ này được đánh giá lại mỗi 10 giây trong khi mảng camera đang truyền dữ liệu. Khi vượt quá 5%, nó sẽ
được ghi nhật ký kèm theo cả hai con số, và mỗi bộ đệm bị hỏng sẽ được báo cáo lần đầu tiên khi
xảy ra trên từng camera, sau đó được tổng hợp lại mỗi phút để phiên làm việc dài vẫn dễ đọc.

**Các khung hình bị hỏng với giá trị 0 ở trường "chưa bao giờ nhận được" có nghĩa là việc kích hoạt và đồng bộ cáp diễn ra hoàn hảo**và mọi khung hình bị mất đều nằm trên đường truyền mạng. Cách khắc phục là giảm**Wire Budget** và
kết nối lại.

{% hint style="warning" %}
**Việc giảm tần suất kích hoạt không giúp khắc phục các khung hình bị hỏng.** Tốc độ gói tin
của camera được thiết lập một lần duy nhất khi kết nối. Giảm tần suất kích hoạt chỉ thay đổi tần suất xảy ra các đợt truyền dữ liệu,
chứ không ảnh hưởng đến tốc độ truyền của chính đợt dữ liệu đó trên đường truyền. Trên một hệ thống 4 camera đã được đo lường, việc
giảm tốc độ kích hoạt xuống 1/5 không thay đổi gì, trong khi việc giảm **Wire Budget** từ 240 xuống
200 MB/s đã giúp hệ thống này giảm tỷ lệ khung hình bị hỏng từ 10,4% xuống còn 0.
{% endhint %}

Một mảng đang hoạt động không thể tự lập kế hoạch lại — hãy ngắt kết nối và kết nối lại để công cụ chọn thời điểm kết nối
có thể hoạt động dựa trên ngân sách mới.

### Bộ điều hợp mạng USB bị giới hạn

Một bộ điều hợp Ethernet USB quảng cáo tốc độ kết nối *Ethernet* của nó, nhưng tốc độ thực tế
mà nó có thể duy trì bị giới hạn bởi bus USB và trình điều khiển của nó. Một bộ điều hợp USB 10GbE từng được ghi nhận
có thông lượng khoảng 1000 MB/s — một con số chưa từng được đo lường — và việc điều chỉnh
tốc độ của bốn camera dựa trên mức dự trữ ảo đó đã làm hỏng 6–18 % số khung hình trong khi mảng
vẫn báo cáo tốc độ khung hình mục tiêu ở mức ổn định. Các bộ điều hợp kết nối qua USB hiện bị giới hạn ở
**200 MB/s** trên Windows và NVIDIA Jetson, và **400 MB/s** trên Linux x86_64. Giới hạn này là một con số tuyệt đối chứ không phải tỷ lệ phần trăm, vì giới hạn chính là
bus: một bộ chuyển đổi USB 1 GbE đạt khoảng 80 MB/s và không bị ảnh hưởng.

Nếu máy chủ của bạn thực sự nhanh hơn mức giới hạn, hãy tăng **Wire Budget** để phản ánh điều đó.

## Đồng bộ thời gian PTP

*Đồng bộ hóa* khung hình đến từ tín hiệu kích hoạt phần cứng; **PTP** (IEEE 1588 PTPv2) cung cấp các *dấu thời gian* tương đồng trên mọi thiết bị. Tính năng này được bật theo mặc định khi kết nối mảng:

* **Hệ thống máy chủ Chloros** chạy vai trò grandmaster PTP. Các camera LATTICE và cảm biến ánh sáng DAQ-E hoạt động như thiết bị phụ thuộc vào nó trong miền 0, do đó dấu thời gian hình ảnh và phổ DAQ sẽ đồng bộ trên cùng một đồng hồ (~1 ms).
* `--no-ptp` (CLI) vô hiệu hóa tính năng này cho công việc trên bàn thí nghiệm — khi đó, các dấu thời gian giữa các camera **sẽ không** tương đồng.
* Kiểm tra tình trạng đồng bộ hóa bằng CLI:

```bash
chloros-cli time-sync status     # grandmaster state, clock identity, health (OK, or why its packets are not reaching the network)
chloros-cli time-sync peers      # slaves seen (cameras + DAQ-E sensors)
chloros-cli time-sync cameras    # per-camera PtpStatus / PtpOffsetFromMaster / PtpMeanPathDelay
```

Chính tab "Cameras" không có chỉ báo PTP; các thông tin đồng bộ hóa cho từng camera ở đó bao gồm các trường chỉ đọc **Role**(Master/Slave),**Sync Line** và cấp độ khả năng của mảng. Trạng thái PTP của DAQ-E được hiển thị trong phần chi tiết cảm biến của tab "Light Sensors".

## Chế độ xem

<!-- SCREENSHOT-NEEDED: Cameras tab with a connected combined array: sidebar showing the ARRAY row (color badge, array name, "DAQ · on" pill) with indented member camera rows in camera order (M3C before M3M, monos by wavelength), and the main area showing the combined index composite tile with the LUT-colored NDVI render, top-left array name pill, and top-right fps readout. -->

mảng trực tiếp Khu vực nguồn cấp dữ liệu chính cung cấp hai bố cục (chuyển đổi ở thanh trên cùng): **chế độ xem dạng lưới**(mỗi ô là một ô; kéo để sắp xếp lại khi biểu tượng khóa lưới được mở khóa) và**chế độ xem danh sách**(các hàng có chiều rộng đầy đủ: một mảng**kết hợp**là một hàng — ô tổng hợp của nó — trong khi một mảng**riêng biệt**cho**mỗi thành viên một hàng riêng**, kèm theo luồng trực tiếp và bảng Mục tiêu hiệu chuẩn riêng; dưới các mảng là một camera độc lập — camera đang hoạt động). Thanh trượt**Zoom nguồn cấp dữ liệu** điều chỉnh kích thước các ô; khi chiều rộng ô dưới 200 px, các lớp phủ tên/fps sẽ tự động ẩn đi. Các camera thành viên xuất hiện trong thanh bên và trong bố cục mặc định của lưới theo thứ tự camera (M3C trước M3M, FRGB trước, camera đơn sắc theo bước sóng); việc mở khóa lưới và kéo thả vẫn sẽ ghi đè lên thứ tự của lưới.

<!-- SCREENSHOT-NEEDED: list view of a connected SEPARATE array (1.3.0): one full-width row per member camera, each with its live feed and its own Calibration Target table below it. -->



**Chế độ riêng biệt** hiển thị một ô cho mỗi camera. Mỗi ô hiển thị các lớp phủ:

* tên camera (góc trên bên trái),
* **chỉ số fps** (góc trên bên phải) — đây là *tốc độ chụp thực* của camera do hệ thống nền báo cáo, không phải tốc độ lấy mẫu xem trước (xem trước trực tiếp bị giới hạn ở 30 fps bất kể tốc độ chụp thực),
* một chấm trạng thái — xanh lá (đang truyền) / vàng (đang tải) / đỏ (lỗi),
* một **biểu tượng quay khi khung hình cũ** khi không có khung hình mới nào đến trong 2 giây — điều này là bình thường trong khoảng 5 giây sau bất kỳ lần kết nối/ngắt kết nối nào trong khi hệ thống nền đang cân bằng lại ngân sách băng thông giữa các camera.**Chế độ kết hợp**hiển thị một ô ghép duy nhất: hệ thống backend thực hiện quá trình debayering, thu phóng, căn chỉnh, khử nhiễu, chuyển đổi sang độ sáng theo từng dải (cộng với độ phản xạ DLS khi cảm biến ánh sáng được kết nối), đánh giá biểu thức chỉ số của mảng, áp dụng LUT, và truyền kết quả dưới dạng MJPEG. Cho đến khi khung hình căn chỉnh đầu tiên được hiển thị, ô sẽ giải thích trạng thái của nó: "Đang chuẩn bị mảng…", "Đang hiệu chỉnh căn chỉnh…", "Đang chờ khung hình đầu tiên…", hoặc — nếu các lần thử lại căn chỉnh tự động của ô (khoảng một phút) hết thời gian — "Cần căn chỉnh" kèm theo nút**Hiệu chỉnh căn chỉnh**.

Chỉ số fps của ô mảng cũng báo cáo trạng thái của luồng dữ liệu:**"Đang áp dụng cài đặt…"**khi thay đổi cài đặt (định dạng, gộp pixel, vùng quan tâm) khiến các camera khởi động lại,**"Bị đình trệ"**khi mảng vẫn hoạt động nhưng đã ngừng truyền các nhóm khung hình, và**"Dừng"**khi mảng đã ngừng truyền dữ liệu — trong trường hợp đó, hãy kết nối lại mảng. Khi quá trình tách phổ phải giới hạn một dải phổ hiển thị trên hình ảnh tổng hợp ở một phần lớn các pixel của nó (25% trở lên), ô sẽ hiển thị**"Dữ liệu dải phổ độ tin cậy thấp · N%"**: các giá trị chỉ số dựa trên dải đó là đáng ngờ (thường là một dải nhận được quá ít ánh sáng).

Thông tin hữu ích về chế độ kết hợp:

* Hình ảnh tổng hợp được căn chỉnh với khung hình của camera **chính**. Chức năng nhắm mục tiêu AE-ROI và đo sáng điểm trên hình ảnh tổng hợp là chính xác đối với camera chính và xấp xỉ đối với các camera phụ; hãy sử dụng**Split View** (cài đặt mảng → "Hiển thị các camera thành viên") để xem các ô hình ảnh chính xác đến từng pixel cho từng camera mà không cần mở thêm kết nối camera.
* **Lớp hiển thị** (cài đặt mảng; mặc định tắt) cho phép bạn chọn lớp nền trước và lớp nền sau — bất kỳ camera thành viên nào hoặc**Chỉ số**. Khi lớp nền trước = Chỉ số, các pixel nằm ngoài phạm vi LUT Min/Max sẽ hiển thị lớp nền sau.
* **Độ phân giải kết xuất** (mặc định 720p) thiết lập chiều cao luồng trực tiếp *và* kích thước xuất hình ảnh tổng hợp đã lưu. Hình ảnh của từng camera luôn được xuất ở độ phân giải đầy đủ.
* Việc căn chỉnh được tính toán cho từng phiên và không bao giờ được lưu lại. Chloros tự động căn chỉnh **mọi mảng sau khi kết nối, ở cả hai chế độ hiển thị**, sau khi độ phơi sáng tự động của các camera thành viên đã ổn định (nó sẽ tiếp tục thử trong tối đa 5 phút). Phần**Căn chỉnh**trong bảng cài đặt mảng hiển thị các sai số RMS và nút**Căn chỉnh lại** — cũng áp dụng cho các mảng riêng lẻ, không có ô ghép tổng hợp.

## Ghi hình: giám sát so với phân tích

Các bề mặt chụp mảng được phân chia rõ ràng thành **cấp độ giám sát**(ghi lại những gì bạn thấy) và**cấp độ phân tích** (ghi lại dữ liệu thô, hiệu chỉnh sau):

| Quy trình làm việc | Cấp độ | Những gì được lưu | Giao diện người dùng (GUI) | Giao diện dòng lệnh (CLI) |
| --- | --- | --- | --- | --- |
| **Ghi lại**(hình tĩnh) | Phân tích | Một nhóm khung hình đồng bộ cho mỗi lần quét; các tệp theo từng camera ở mọi mức xuất đã chọn (thô/đã loại bỏ hiệu ứng debayering/độ sáng/độ phản xạ/xem trước/chỉ số) + tệp sidecar `.daq` |**Capture All** + Cài đặt chụp | `lattice array-capture` |
| **Ghi video chỉ số** | Giám sát | Hình ảnh tổng hợp chỉ số kết hợp trực tiếp như hiển thị — 8-bit, độ phân giải xem trước, LUT được tích hợp sẵn. Từ CLI khi không mở giao diện đồ họa (GUI), hệ thống nền sẽ xử lý giá trị NDVI (hoặc `--expr`) của mảng sau khi mảng được căn chỉnh; quá trình ghi hình sẽ thất bại nếu không ghi được khung hình nào | ● Ghi video chỉ số (mảng kết hợp) | `lattice array-record` |
| **Dữ liệu thô liên tiếp → tạo video**| Phân tích | Các khung hình thô từ cảm biến ở tốc độ thu thập tối đa + tệp manifest + `.daq`, sau đó tái tạo ngoại tuyến thành video độ sáng / độ phản xạ / chỉ số đã hiệu chuẩn, đồng bộ thời gian với các giá trị đọc từ DAQ | ⦿ Ghi dữ liệu thô liên tiếp →**Tạo video** | `lattice array-burst` → `lattice array-build-video` |

Quy tắc chung: nếu các pixel sẽ cung cấp *dữ liệu đo lường*, hãy sử dụng chế độ chụp hoặc chụp liên tiếp (cấp độ phân tích); nếu bạn chỉ cần *xem hoặc trình diễn* những gì mảng cảm biến đã ghi nhận, hãy ghi lại video chỉ số (cấp độ giám sát).

### Cài đặt Ghi (GUI)

<!-- SCREENSHOT-NEEDED: Capture Settings pane (gear next to Capture All) with a connected array: capture-mode buttons (Single/Continuous/Interval), the bulk export-type toggle row, the Fastest Capture toggle, and the per-array group card showing the Aligned checkbox and the "Record index video" / "Record raw burst" buttons. -->



Biểu tượng bánh răng bên cạnh **Ghi tất cả** sẽ mở cửa sổ Cài đặt Ghi (yêu cầu có dự án đang mở — các bản ghi sẽ được lưu vào đó):

* **Chế độ chụp**:**Đơn**(một lần) /**Liên tục**(liên tiếp; giới hạn bởi số lần chụp, mặc định là 1, hoặc thời lượng, mặc định là 10 giây) /**Khoảng thời gian** (chế độ time-lapse: N khung hình mỗi khoảng thời gian X cho tổng số Y, mặc định là 1 khung hình mỗi 5 giây trong 1 phút).
* **Định dạng xuất cho từng camera**: Raw, Debayered, Radiance, Reflectance, Preview, Index — tất cả các tùy chọn áp dụng đều được bật mặc định. Radiance/Reflectance bị ẩn đối với camera có bộ lọc RGB; **Reflectance chỉ xuất hiện khi camera có cảm biến ánh sáng DAQ** (của riêng nó hoặc thừa hưởng từ mảng); Index yêu cầu một biểu thức chỉ mục đã được cấu hình.
* **Căn chỉnh**(theo mảng, mặc định**bật**): biến dạng các tệp xuất của thành viên theo cấu hình căn chỉnh của mảng để các tệp xuất được căn chỉnh theo pixel. Dữ liệu thô luôn giữ nguyên trạng thái chưa biến dạng nhưng mang theo phép biến đổi trong siêu dữ liệu. Các tệp thành viên của mảng được lưu theo hướng gốc của từng máy ảnh (bất kỳ cài đặt Gương/Xoay nào đều được ghi lại trong tệp, không được áp dụng).
* **Ghi nhanh nhất** (chuyển đổi): chỉ dữ liệu thô + giá trị DAQ được gán + hình ảnh tổng hợp chỉ số kết hợp miễn phí, bỏ qua các phép tính hiệu chuẩn tại thời điểm ghi để đạt tốc độ tối đa — tái tạo độ sáng/độ phản xạ/chỉ số sau này từ tệp `.daq` đã lưu.
* Các lựa chọn được lưu lại cùng với dự án. Các camera bị ẩn hoặc tạm dừng sẽ bị bỏ qua.
* Chế độ **Liên tục** trên mảng chạy bên trong backend Chloros (mới trong phiên bản 1.3.0): khi mọi camera trong danh sách đều thuộc về một mảng, backend sẽ tự chạy vòng lặp chụp, ghi một nhóm trong khi chọn nhóm tiếp theo, mà không cần yêu cầu cho mỗi lần chụp. Ổ đĩa đầy sẽ dừng quá trình chạy ("Đã dừng: ổ đĩa đầy. Đã lưu N, F thất bại"). Xem [Cài đặt & Chế độ Ghi hình](capture.md#capture-modes) để biết lưu ý của Microsoft Defender về tốc độ ghi hình cao.

Lệnh dòng lệnh (CLI) tương đương (cùng điểm cuối backend, cùng ngữ nghĩa):

```bash
# One synced group, every applicable export level per camera (the default)
chloros-cli lattice array-capture -o output/

# Interval timelapse: one reflectance pass every 10 s for 5 minutes
chloros-cli lattice array-capture --interval 10 --duration 300 --processing reflectance -o timelapse/

# Fastest grab for a moving rig — raw + .daq now, calibrate later
chloros-cli lattice array-capture --fastest -o flightline/

# 30-second monitoring clip of the combined index view, plus a GIF
chloros-cli lattice array-record --duration 30 --fps 10 --gif -o monitoring/

# 5-second analysis-grade raw burst, then build the combined index video
chloros-cli lattice array-burst --duration 5 --build --products combined:index --fps 10 -o capture/
```

Nén TIFF cho các lần chụp là `deflate` (không mất dữ liệu, mặc định) hoặc `none` — bảng cờ đầy đủ, cấu trúc thư mục chụp và quy tắc xử lý lại được nêu trong [Tham chiếu CLI](../reference/cli-reference.md#capture-modes-recorders--offline-reprocess).

## Ghép nối cảm biến ánh sáng DAQ

Các bản xem trước đã hiệu chỉnh độ phản xạ và độ chiếu sáng cần dữ liệu ánh sáng chiếu xuống từ cảm biến DAQ (được kết nối trong tab **Cảm biến ánh sáng**):

* **Hàng mảng**trên thanh bên hiển thị một**nút "DAQ · bật/tắt"** — *bật* khi cảm biến ánh sáng cấp mảng được thiết lập **hoặc** bất kỳ camera thành viên nào có cảm biến riêng; chú giải công cụ của nó liệt kê chính xác cảm biến nào cung cấp dữ liệu cho camera nào.
* Gán cài đặt chung cho toàn mảng trong phần cài đặt mảng → **Cảm biến ánh sáng xung quanh**→ menu thả xuống**Cảm biến ánh sáng**. Lựa chọn này được lưu cùng dự án, áp dụng cho mọi camera thành viên, và các camera riêng lẻ vẫn có thể ghi đè bằng cảm biến riêng của mình.
* Dòng trạng thái bên dưới hiển thị trạng thái thời gian thực: **Tắt**→ "Đang chờ phổ ánh sáng đầu tiên…" →**"Hoạt động — tất cả camera trong mảng đều được hiệu chỉnh độ sáng"** → hoặc, nếu không có phổ ánh sáng mới nào được nhận trong 3 giây qua, sẽ hiển thị thông báo "dữ liệu cũ" — giá trị đọc cuối cùng vẫn tiếp tục được sử dụng (các giá trị đọc không bao giờ hết hạn trên đường dẫn ghi hình).

Khi đã gán cảm biến: loại xuất dữ liệu "Reflectance" sẽ khả dụng, bản xem trước trực tiếp được hiệu chỉnh ánh sáng, tính năng phơi sáng tự động dự đoán có thể sử dụng phổ, và mỗi lần chụp phản xạ sẽ ghi giá trị DAQ thực tế được sử dụng dưới dạng tệp **`.daq`** vào thư mục `daq/` để có thể xử lý lại lần ghi sau này. Bên cạnh đó, tệp `.daq.cameras.json` liệt kê tên các camera mà giá trị đo đó mô tả; khi xử lý lại, Chloros chỉ ghép giá trị đo với hình ảnh từ các camera đó.

## Các tùy chọn CLI

** của `array-connect`

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--serials SN1,SN2,…` | tự động phát hiện tất cả camera LATTICE (cần ≥2) | **Số sê-ri đầu tiên là MASTER.** |
| `--line {Line0,Line2,Line3}` | `Line2` | Dòng đồng bộ GPIO. |
| `--target-fps F` | auto | Tần suất kích hoạt của MASTER. |
| `--binning {1,2,4}` | auto | Nhóm pixel phần cứng. Chế độ "auto" sử dụng toàn bộ cảm biến (không nhóm pixel) mỗi khi tốc độ khung hình hỗ trợ; chế độ "connect never" không bao giờ nhóm pixel để duy trì tốc độ khung hình. |
| `--packed {auto,on,off}` | `auto` | [Truyền tải 12-bit nén](#packed-12-bit-transport): `auto` chỉ nén khi điều đó có lợi và máy chủ có đủ tài nguyên để giải nén. |
| `--force-tier {sim-capture-sim-emit, sim-capture-ftd-stagger, slip-emit-and-capture}` | tự động | Chế độ chuyên gia để ghi đè lựa chọn cấp độ đồng bộ. |
| `--wire-ceiling-mbps MB_PER_S` | tự động phát hiện | Ngân sách băng thông của máy chủ tính bằng MB/s — dạng CLI của trường **Wire Budget**. Hãy giảm giá trị này nếu mảng báo cáo có các khung dữ liệu bị hỏng. Giá trị này được lưu cùng với dự án, do đó việc kết nối lại sau này sẽ khôi phục lại giá trị này. |
| `--no-recommend` | tắt | Bỏ qua bước phân tích mạng. |
| `--no-ptp` | tắt | Vô hiệu hóa PTP (dấu thời gian giữa các camera sẽ không thể so sánh được). |

`lattice array-list`, `array-status` và `array-disconnect` quản lý phiên làm việc liên tục. Tham chiếu đầy đủ về các lệnh con, bao gồm căn chỉnh (`align-calibrate` / `align-apply`) và các công cụ mạng, có trong [Tham chiếu CLI § chloros-cli lattice](../reference/cli-reference.md#chloros-cli-lattice); các lệnh tương đương trong SDK (`connect_array`, `ArraySession`, `attach_array`, `analyze_array_network`) có trong [Tham chiếu SDK](../reference/sdk-reference.md). Từ Python, ngân sách đường truyền là `connect_array(..., wire_ceiling_mbps=120)`, và việc phân chia giữa dữ liệu bị hỏng khi đang truyền và dữ liệu không bao giờ đến được được trình bày tại [`/api/camera/array/<id>/capability`](../reference/sdk-reference.md#array-health--which-subsystem-is-losing-frames).
