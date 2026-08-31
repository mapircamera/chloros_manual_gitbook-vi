# Mảng nhiều camera

Một **mảng**LATTICE là hai hoặc nhiều camera LATTICE được kết nối thành một đơn vị đồng bộ. Một camera đóng vai trò là**master**: nó phát ra xung kích hoạt GPIO phần cứng trên đường dây đồng bộ chung (mặc định là**Line2**), do đó mọi thành viên đều ghi lại cùng một khoảnh khắc. Chloros bổ sung tính năng đồng bộ thời gian PTP, xem trước trực tiếp (dạng ô riêng lẻ cho từng camera hoặc một hình ảnh tổng hợp đa dải được căn chỉnh), cùng với khả năng chụp đồng bộ — mỗi lần chụp sẽ tạo ra một**nhóm khung hình** trong đó tất cả các camera đều có cùng dấu thời gian và ID khung hình (được báo cáo dưới dạng `fid:N` trên đầu ra chụp).

Mảng là cách các camera đơn sắc (M3M) tạo ra các chỉ số thực vật — mỗi camera đóng góp một dải phổ, và mảng căn chỉnh chúng thành một chồng hình đa dải phổ. Xem [Camera đơn sắc &amp; Chỉ số thực vật](mono-indices.md).

Có ba cách tương đương để kết nối một mảng, và tất cả đều chạy cùng một quy trình &quot;smart-prep&quot;:

| Bề mặt | Điểm vào |
| --- | --- |
| GUI | Tab Máy ảnh → **Kết nối mảng** (nút màu xanh lam) |
| CLI | `chloros-cli lattice array-connect --serials SN1,SN2,…` (số sê-ri đầu tiên = máy chủ) |
| Python SDK | `connect_array(serials=[…])` → `ArraySession` (số sê-ri đầu tiên = máy chủ) |

Smart-prep thực hiện, theo thứ tự: kiểm tra khả năng mạng (ping ICMP DF + kiểm tra GVSP), lựa chọn cấp độ đồng bộ, tự động thu nhỏ kích thước khung hình để phù hợp với đường truyền, kích hoạt PTP, tự động chọn định dạng pixel cho từng camera, thiết lập phơi sáng tự động dựa trên trạng thái đã lưu của từng camera, và cấu hình kích hoạt GPIO trên Line2.

{% hint style="info" %}
Các camera phải có thể truy cập được trên liên kết trước khi bất kỳ thao tác nào trong số này hoạt động — xem [Kết nối camera](connecting.md) để biết thông tin về phát hiện, gán địa chỉ và tải xuống dữ liệu hiệu chuẩn cho lần kết nối đầu tiên. Đối với hệ thống nhiều camera, cài đặt vòng nhận (receive-ring) của NIC máy chủ quan trọng không kém tốc độ kết nối; bảng tổng hợp triệu chứng → cách khắc phục đầy đủ có trong [Tham khảo CLI § Cài đặt &amp; Tối ưu hóa NIC máy chủ](../reference/cli-reference.md#host-nic-setup--tuning-lattice-arrays).
{% endhint %}

## Hộp thoại Kết nối Mảng

Tab Camera → **Kết nối Mảng**sẽ mở trình hướng dẫn ba bước:**Chọn → Chế độ hiển thị → Cài đặt**.

### Bước 1 — Chọn camera chính và camera phụ



<!-- SCREENSHOT-NEEDED: Array Connect dialog, Select scene, with 3-4 LATTICE cameras discovered. Table showing Camera / Serial / IP / Master radio / Slave checkbox columns, with the green "GPIO master detected — selections pre-populated" probe banner visible above the table. -->Hộp thoại sẽ quét mạng ngay khi mở ra (“Đang quét mạng…”), sau đó kiểm tra kết nối kích hoạt GPIO (“Đang kiểm tra kết nối GPIO…”). Bạn cần ít nhất **2 camera** để tạo một mảng.

Quá trình kiểm tra kết nối sẽ tự động điền sẵn các tùy chọn vai trò khi có thể, và hiển thị một trong ba thông báo sau:

| Thông báo | Ý nghĩa |
| --- | --- |
| &quot;Đã phát hiện camera chính GPIO — các tùy chọn đã được điền sẵn&quot; (màu xanh lá) | Quá trình kiểm tra đã phát hiện cấu trúc kích hoạt; các ô chọn &quot;thiết bị chính&quot; và &quot;thiết bị phụ&quot; đã được điền sẵn. |
| &quot;Không phát hiện thiết bị chính — kiểm tra cáp GPIO&quot; (màu cam) | Không có camera nào nhận được xung kích hoạt; hãy kiểm tra dây đồng bộ. Bạn vẫn có thể chọn vai trò thủ công. |
| &quot;Không có cáp đồng bộ: {serials}&quot; (màu cam) | Các camera được liệt kê không có cáp đồng bộ được kết nối. |

Bảng camera có các cột **Camera / Số sê-ri / IP / Master (radio) / Slave (ô chọn)**:

* Chọn chính xác **một máy chủ**và**một hoặc nhiều máy con**. Nhấp lại vào ô radio của máy chủ hiện tại sẽ xóa chọn nó.
* Một camera được đánh dấu **&quot;Không có cáp đồng bộ&quot;** không bao giờ có thể được chọn làm máy con — một máy con không có dây kích hoạt sẽ chờ trên đường dây đồng bộ mãi mãi và truyền tín hiệu chết. Thay vào đó, hãy kết nối camera đó dưới dạng camera độc lập.
* Các camera đã được kết nối độc lập *sẽ không* bị vô hiệu hóa: việc kết nối vào mảng sẽ giải phóng phiên làm việc độc lập và mở lại camera trong mảng.

**Tiếp theo: Chế độ hiển thị →**sẽ được kích hoạt sau khi đã chọn một máy chủ và ít nhất một máy con.**Quét lại** sẽ chạy lại quá trình phát hiện và kiểm tra dây kết nối.

{% hint style="warning" %}
**Hủy** sẽ bị vô hiệu hóa trong khi quá trình quét hoặc kiểm tra đang diễn ra — việc hủy giữa chừng có thể khiến camera bị treo SDK trên phần mềm camera LATTICE. Hãy đợi cho đến khi biểu tượng quay tròn hoàn tất.
{% endhint %}

### Bước 2 — Chế độ hiển thị



<!-- SCREENSHOT-NEEDED: Array Connect dialog, Display Mode scene, showing the two selectable cards ("Separate Cameras" and "Combined Cameras") with Combined selected/highlighted as the default. -->| Chế độ | Kết quả |
| --- | --- |
| **Máy ảnh riêng biệt** | Mỗi máy ảnh hiển thị một ô hình trực tiếp, tất cả được kích hoạt cùng lúc để các khung hình luôn đồng bộ. Mỗi máy ảnh giữ nguyên màu sắc và cài đặt riêng. |
| **Camera kết hợp** *(mặc định)* | Một ô duy nhất hiển thị hình ảnh tổng hợp đa dải NDVI /index đã được căn chỉnh. Các camera chia sẻ cùng một màu sắc mảng. |

Chế độ hiển thị chỉ thay đổi cách trình bày bản xem trước trực tiếp — hành vi ghi hình vẫn giống nhau ở cả hai chế độ.

### Bước 3 — Cài đặt mảng và kết quả

<!-- SCREENSHOT-NEEDED: Array Connect dialog, Settings scene, healthy state: left column with ROI / Binning / Pin resolution / Trigger Rate controls, right "Projected Outcome" column showing green "Simultaneous capture" tier, an fps range, the NIC line, the "Sim-emit burst" line, and the "Wire budget" line with a checkmark. -->dự kiến

Khi vào cảnh này, Chloros sẽ yêu cầu hệ thống nền cung cấp một **đề xuất**và tự động áp dụng kết hợp vùng quan tâm (ROI) + nhóm pixel (binning) phù hợp với vòng thu nhận của NIC của bạn (nó ưu tiên nhóm pixel hơn việc cắt vùng ROI, vì nhóm pixel giữ nguyên toàn bộ trường nhìn). Mỗi thay đổi bạn thực hiện sẽ chạy lại phân tích trực tiếp và cập nhật bảng**Kết quả dự kiến** ở bên phải.

Cột bên trái — cài đặt:

| Điều khiển | Lựa chọn | Mặc định | Ghi chú |
| --- | --- | --- | --- |
| **ROI (Trường nhìn)** | Đầy đủ (2048×1536) / Nửa (1024×768) / Một phần tư (512×384) | Đầy đủ | Cắt cảm biến: Cắt nửa/một phần tư để thu nhỏ vùng hình ảnh ở độ phân giải pixel gốc. |
| **Binning** | 1× / 2× (tổng 2×2) / 4× (tổng 4×4) | 1× | Binning phần cứng: 2×2 = FoV đầy đủ với chi phí đường truyền bằng 1/4; 4×4 = FoV đầy đủ với chi phí đường truyền bằng 1/16. Ẩn nếu camera không hỗ trợ ghép pixel. |
| **Hình ảnh trên đường truyền** (đọc dữ liệu) | — | — | Chiều rộng × chiều cao sau khi ghép pixel thực tế được gửi qua đường truyền, được làm tròn xuống thành bội số của 16 (tối thiểu 64). |
| **Độ phân giải chân cắm**| ô chọn | tắt | Chế độ**Chloros**thường tự động kích hoạt gộp pixel khi kết nối nếu tốc độ dự kiến giảm xuống dưới**1,5 khung hình/giây**. Chế độ**Pinning** giữ nguyên kích thước khung hình đã chọn và chấp nhận tốc độ thấp hơn — đồng thời chuyển cấu hình quá tải thành từ chối kết nối cứng thay vì tự động giảm tốc độ. |
| **Tốc độ kích hoạt** | 0,5–60 khung hình/giây, bước 0,1 | để trống = tự động | Tốc độ kích hoạt của thiết bị chủ. Để trống để cho phép Chloros tự động tính toán. |
| **Dung lượng đường truyền**| 20–2000 MB/s, bước 10 | trống = tự động | Lượng dữ liệu mà máy chủ thực sự có thể xử lý, tính bằng MB/s —**con số duy nhất quyết định toàn bộ việc phân bổ mảng.** Được phát hiện tự động từ bộ điều hợp mạng. Hãy giảm giá trị này nếu mảng báo cáo có các khung dữ liệu bị hỏng: giá trị được phát hiện thường cao hơn thực tế đối với bộ điều hợp USB và các bộ chuyển mạch chia sẻ. Thay đổi giá trị này sẽ chạy lại dự báo theo thời gian thực. |

Cột bên phải — **Kết quả dự báo**:

* **Cấp độ đồng bộ** — &quot;Ghi đồng thời&quot; (màu xanh lá), &quot;Ghi đồng thời (phát theo kiểu FTD xen kẽ)&quot; (màu xanh lá), &quot;Ghi xen kẽ (chênh lệch 100 ms)&quot; (màu hổ phách), hoặc &quot;Cấu hình quá lớn&quot; (màu đỏ).
* **Dự báo fps** — hiển thị dưới dạng khoảng giá trị (&quot;mờ → sáng&quot;), vì tốc độ của mảng đồng bộ bị giới hạn bởi thời gian phơi sáng của camera chậm nhất.
* **Dòng NIC** — tốc độ kết nối và băng thông ổn định (&quot;NIC {mbps} Mbps · ổn định {N} MB/s&quot;).
* **Kiểm tra đợt phát đồng bộ** — liệu vòng kết nối NIC của máy chủ có thể tiếp nhận một đợt phát đồng thời từ tất cả các camera hay không (&quot;Đợt phát đồng bộ: X MB · Dung lượng vòng NIC khả dụng: Y MB ✓/✗&quot;).
* **Kiểm tra ngân sách băng thông** — nhu cầu tổng hợp ở trạng thái ổn định so với giới hạn an toàn va chạm của đường truyền (&quot;Ngân sách băng thông: {demand} MB/s do {n} camera yêu cầu · giới hạn {ceiling} MB/s ✓/✗ quá tải&quot;).
* **&quot;Số camera tối đa trên đường truyền này: {n} — được xác định bởi ngưỡng băng thông tối thiểu cho mỗi camera, do đó việc gộp khung hình sẽ không làm tăng con số này.&quot;** — hiển thị khi bạn sắp đạt (hoặc vượt quá) giới hạn số lượng camera.
* **&quot;CÁC KHUNG HÌNH SẼ BỊ MẤT ở các cài đặt này.&quot;**— cảnh báo màu đỏ kèm lý do từ hệ thống phía sau, cùng danh sách các yếu tố cản trở và các**gợi ý khắc phục** màu xanh (&quot;Để phù hợp với mảng này trên mạng&quot; / &quot;Để mở khóa tính năng ghi hình đồng thời&quot;).**Áp dụng &amp; Kết nối** sẽ bị khóa cho đến khi có dự báo, và nhãn của nó sẽ cho bạn biết lý do tại sao nó bị từ chối:

| Nhãn nút | Ý nghĩa | Cách khắc phục |
| --- | --- | --- |
| &quot;Đang phân tích...&quot; | Quá trình phân tích vẫn đang diễn ra. | Chờ. |
| **&quot;Quá nhiều camera cho mạng này&quot;**| Mảng camera vượt quá dung lượng đường truyền (kiểm tra tổng hợp thất bại). | Giảm số lượng camera, sử dụng khung jumbo từ đầu đến cuối, hoặc dùng NIC nhanh hơn.**Giảm kích thước ROI sẽ KHÔNG giúp được** — xem bên dưới. |
| **&quot;Giảm ROI để kích hoạt&quot;** | Các khung dữ liệu sẽ bị mất ở các thiết lập này (kiểm tra burst/ring thất bại). | Giảm ROI, tăng mức gộp (binning) hoặc sửa vòng nhận (receive ring) của NIC. |



<!-- SCREENSHOT-NEEDED: Array Connect dialog, Settings scene, over-subscribed state: red "Wire budget ... over-subscribed" line, the "Max cameras on this wire" hint, and the Apply button reading "Too many cameras for this network". Reproduce by configuring more cameras than the 1 GbE ceiling (e.g. 7+ cams at 1500 MTU) or with CHLOROS-simulated models via `lattice analyze-array`. -->Trong khi kết nối, một **bảng tải xuống hiệu chuẩn** màu xanh lá cây có thể xuất hiện kèm theo thanh tiến trình cho từng cổng nối tiếp: lần đầu tiên kết nối camera với máy, Chloros sẽ tải xuống gói hiệu chuẩn xuất xưởng khoảng 3,8 MB từ camera qua GigE (khoảng 70 giây cho mỗi camera). Các camera đã được lưu vào bộ nhớ đệm sẽ không bao giờ hiển thị bảng này. Xem [Kết nối camera](connecting.md).

## Băng thông: số lượng camera tối đa

Khả năng truyền tải của một mảng phụ thuộc vào đường truyền, không phải vào Chloros, do đó các số liệu lập kế hoạch được nêu trong tài liệu hướng dẫn phần cứng: **[Lập kế hoạch băng thông mảng](https://mapir.gitbook.io/lattice-camera/setup/array-bandwidth-planning)**.

Cách Chloros xử lý các số liệu này: hộp thoại kết nối sẽ thực hiện kiểm tra mạng, dự đoán tốc độ khung hình có thể đạt được và chọn gói dịch vụ phù hợp. Nếu mảng camera vượt quá dung lượng đường truyền, hệ thống sẽ từ chối kết nối thay vì bỏ qua các gói tin một cách im lặng — xem bảng kết quả dự đoán được mô tả ở trên.

## Khi các khung dữ liệu bị mất

Một camera có thể vắng mặt trong một nhóm đã công bố vì hai lý do hoàn toàn khác nhau,
và chúng cần các biện pháp khắc phục trái ngược nhau. Chloros thống kê chúng riêng biệt thay vì báo cáo một
con số &quot;không đầy đủ&quot; mà không nêu rõ nguyên nhân nào:

| Điều gì đã xảy ra | Ý nghĩa | Nơi cần kiểm tra |
| --- | --- | --- |
| **Bị hỏng**— khung hình đã đến nhưng có cấu trúc sai | Mất gói tin GVSP trên đường truyền mạng |**Dung lượng đường truyền**, vòng nhận của NIC, khung hình jumbo, bộ chuyển mạch |
| **Chưa bao giờ đến**— không có khung dữ liệu nào đến cả | Camera không kích hoạt, hoặc không có dữ liệu nào được gửi đi |**Cáp đồng bộ M8**, đường dây đồng bộ, trạng thái kích hoạt của từng thành viên |

Tỷ lệ này được đánh giá lại mỗi 10 giây trong khi mảng camera đang truyền dữ liệu. Khi vượt quá 5%, nó sẽ
được ghi nhật ký kèm theo cả hai con số, và mỗi bộ đệm bị hỏng sẽ được báo cáo lần đầu tiên khi
xảy ra trên từng camera, sau đó được tổng hợp lại mỗi phút để phiên làm việc dài vẫn dễ đọc.

**Các khung hình bị hỏng với giá trị 0 ở trường &quot;chưa bao giờ nhận được&quot; có nghĩa là việc kích hoạt và đồng bộ cáp diễn ra hoàn hảo**và mọi khung hình bị mất đều nằm trên đường truyền mạng. Cách khắc phục là giảm**Wire Budget** và
kết nối lại.

{% hint style="warning" %}
**Việc giảm tần suất kích hoạt không giúp khắc phục các khung hình bị hỏng.** Tốc độ truyền gói tin
của camera được thiết lập một lần duy nhất khi kết nối. Giảm tần suất kích hoạt chỉ thay đổi tần suất
xảy ra của các đợt truyền dữ liệu, chứ không ảnh hưởng đến tốc độ truyền thực tế của chính đợt truyền đó trên đường truyền. Trên một hệ thống 4 camera đã được đo lường, việc
giảm tần suất kích hoạt xuống 1/5 không mang lại thay đổi nào, trong khi việc giảm ngân sách băng thông từ 240 xuống
200 MB/s đã giúp hệ thống đó giảm tỷ lệ khung hình bị hỏng từ 10,4% xuống 0%.
{% endhint %}

Một mảng đang hoạt động không thể tự lập kế hoạch lại — hãy ngắt kết nối và kết nối lại để trình chọn thời gian kết nối
có thể hoạt động dựa trên giới hạn băng thông mới.

### Bộ điều hợp mạng USB bị giới hạn ở 200 MB/s

Một bộ điều hợp Ethernet USB quảng cáo tốc độ kết nối *Ethernet* của nó, nhưng tốc độ thực tế
mà nó có thể duy trì bị giới hạn bởi bus USB và trình điều khiển của nó. Một bộ điều hợp USB 10GbE từng được cho là
có thông lượng khoảng 1000 MB/s — một con số chưa từng được đo lường — và việc điều chỉnh
cho bốn camera dựa trên mức dự trữ ảo đó đã làm hỏng 6–18 % khung hình trong khi mảng
vẫn báo cáo tốc độ khung hình mục tiêu ổn định. Các bộ điều hợp kết nối qua USB hiện bị giới hạn ở
**200 MB/s**. Giới hạn này là một con số tuyệt đối chứ không phải tỷ lệ phần trăm, vì giới hạn nằm ở
bus: một bộ chuyển đổi USB 1 GbE đạt khoảng 80 MB/s và không bị ảnh hưởng.

Nếu máy chủ của bạn thực sự nhanh hơn mức giới hạn, hãy tăng **Wire Budget** để phản ánh điều đó.

## Đồng bộ thời gian PTP

*Đồng bộ hóa* khung hình đến từ tín hiệu kích hoạt phần cứng; **PTP** (IEEE 1588 PTPv2) cung cấp các *dấu thời gian* tương đương trên mọi thiết bị. Tính năng này được bật theo mặc định khi kết nối mảng:

* Hệ thống máy chủ Chloros chạy vai trò máy chủ chính (grandmaster) PTP**. Các camera LATTICE và cảm biến ánh sáng DAQ-E hoạt động như thiết bị phụ thuộc vào nó trong miền 0, do đó dấu thời gian hình ảnh và phổ DAQ đều được đồng bộ hóa trên cùng một đồng hồ (~1 ms).
* `--no-ptp` (CLI) vô hiệu hóa tính năng này cho công việc trên bàn thí nghiệm — lúc này, các dấu thời gian giữa các camera **sẽ không** tương thích với nhau.
* Kiểm tra tình trạng đồng bộ hóa bằng lệnh `CLI`:

```bash
chloros-cli time-sync status     # grandmaster state, clock identity
chloros-cli time-sync peers      # slaves seen (cameras + DAQ-E sensors)
chloros-cli time-sync cameras    # per-camera PtpStatus / PtpOffsetFromMaster / PtpMeanPathDelay
```

Chính tab &quot;Cameras&quot; không có chỉ báo PTP; các thông tin đồng bộ hóa cho từng camera ở đó bao gồm **Vai trò**(Master/Slave) chỉ đọc,**Dòng đồng bộ** và cấp độ Tính năng của mảng. Trạng thái PTP của DAQ-E được hiển thị trong chi tiết cảm biến của tab &quot;Light Sensors&quot;.

## Giao diện

<!-- SCREENSHOT-NEEDED: Cameras tab with a connected combined array: sidebar showing the ARRAY row (color badge, array name, "DAQ · on" pill) with indented member camera rows, and the main area showing the combined index composite tile with the LUT-colored NDVI render, top-left array name pill, and top-right fps readout. -->mảng trực tiếp

Khu vực nguồn cấp chính cung cấp hai bố cục (chuyển đổi ở thanh trên cùng): **chế độ lưới**(mỗi ô là một ô; kéo để sắp xếp lại khi khóa lưới được mở) và**chế độ danh sách**(các mảng chiếm toàn chiều rộng ở trên, một camera đang hoạt động ở dưới). Thanh trượt**Zoom nguồn cấp dữ liệu** điều chỉnh kích thước các ô; khi chiều rộng ô dưới 200 px, các lớp phủ tên/fps sẽ ẩn tự động.**Chế độ riêng biệt** hiển thị một ô cho mỗi camera. Mỗi ô hiển thị các lớp phủ:

* tên camera (góc trên bên trái),
* **chỉ số fps** (góc trên bên phải) — đây là *tốc độ lấy khung hình thực* của camera do hệ thống nền báo cáo, không phải tốc độ lấy mẫu xem trước (xem trước trực tiếp bị giới hạn ở 30 fps bất kể tốc độ lấy khung hình thực),
* một chấm trạng thái — xanh lá (đang phát trực tiếp) / vàng (đang tải) / đỏ (lỗi),
* **bi quay khung hình cũ** khi không có khung hình mới nào đến trong 2 giây — điều này là bình thường trong khoảng 5 giây sau khi kết nối/ngắt kết nối, trong khi hệ thống phía sau tái cân bằng ngân sách băng thông giữa các camera.**Chế độ kết hợp**hiển thị một ô tổng hợp duy nhất: hệ thống phía sau thực hiện các bước: khử hiệu ứng Bayer, thu nhỏ, căn chỉnh, khử nhiễu, chuyển đổi sang độ sáng theo từng dải (cộng với độ phản xạ DLS khi cảm biến ánh sáng được liên kết), đánh giá biểu thức chỉ số của mảng, áp dụng bảng tra cứu (LUT) và truyền phát kết quả dưới dạng MJPEG. Cho đến khi khung hình được căn chỉnh đầu tiên được hiển thị, ô này sẽ giải thích trạng thái của nó: &quot;Đang chuẩn bị mảng…&quot;, &quot;Đang hiệu chỉnh căn chỉnh…&quot;, &quot;Đang chờ khung hình đầu tiên…&quot;, hoặc — nếu ngân sách thử lại căn chỉnh tự động (~30 giây) đã hết — &quot;Cần căn chỉnh&quot; kèm nút**Hiệu chỉnh căn chỉnh**.

Thông tin hữu ích về chế độ kết hợp:

* Hình ảnh tổng hợp được căn chỉnh với khung hình của camera **chính**. Chức năng nhắm mục tiêu AE-ROI và đo sáng điểm trên hình ảnh tổng hợp sẽ chính xác đối với camera chính và xấp xỉ đối với các camera phụ; sử dụng**Chế độ chia màn hình** (cài đặt mảng → &quot;Hiển thị các camera thành viên&quot;) để xem các ô hình ảnh chính xác đến từng pixel cho từng camera mà không cần mở thêm kết nối camera.
* **Lớp hiển thị**(cài đặt mảng; mặc định tắt) cho phép bạn chọn lớp nền trước và lớp nền sau — bất kỳ camera thành viên nào hoặc**Chỉ số**. Khi lớp nền trước = Chỉ số, các pixel nằm ngoài phạm vi LUT Min/Max sẽ hiển thị lớp nền sau.
* **Độ phân giải hiển thị** (mặc định 720p) thiết lập chiều cao luồng trực tiếp *và* kích thước xuất hình ảnh tổng hợp đã lưu. Hình ảnh của từng camera luôn được xuất ở độ phân giải đầy đủ.
* Việc căn chỉnh được tính toán cho từng phiên và không bao giờ được lưu lại — xem phần căn chỉnh trong bảng cài đặt mảng để biết sai số RMS và nút **Tái hiệu chuẩn**.

## Ghi hình: giám sát so với phân tích

Các bề mặt ghi hình của mảng được chia rõ ràng thành **cấp độ giám sát**(ghi lại những gì bạn thấy) và**cấp độ phân tích** (ghi lại dữ liệu thô, hiệu chuẩn sau):

| Quy trình làm việc | Cấp độ | Dữ liệu lưu | Giao diện người dùng | CLI |
| --- | --- | --- | --- | --- |
| **Ghi lại**(hình tĩnh) | Phân tích | Một nhóm khung hình đồng bộ cho mỗi lần quét; các tệp riêng lẻ cho từng camera ở mọi mức xuất được chọn (thô/đã loại bỏ hiệu ứng debayering/radiance/độ phản xạ/xem trước/chỉ mục) + tệp sidecar `.daq` | Nút**Chụp tất cả** + Cài đặt chụp | `lattice array-capture` |
| **Ghi video chỉ số** | Giám sát | Hình ảnh tổng hợp chỉ số kết hợp trực tiếp như hiển thị — 8-bit, độ phân giải xem trước, LUT được tích hợp sẵn; cần mở luồng trực tiếp | ● Ghi video chỉ số (mảng kết hợp) | `lattice array-record` |
| **Dòng dữ liệu thô → tạo video**| Phân tích | Khung hình thô từ cảm biến ở tốc độ ghi tối đa + tệp manifest + `.daq`, sau đó tái tạo ngoại tuyến thành video radiance / phản xạ / chỉ số đã hiệu chuẩn, đồng bộ thời gian với dữ liệu từ hệ thống thu thập dữ liệu (DAQ) | ⦿ Ghi dòng dữ liệu thô →**Tạo video** | `lattice array-burst` → `lattice array-build-video` |

Nguyên tắc chung: nếu các pixel sẽ cung cấp *dữ liệu đo lường*, hãy sử dụng chế độ chụp (capture) hoặc chụp liên tục (burst) (chất lượng phân tích); nếu bạn chỉ cần *xem hoặc trình diễn* những gì mảng cảm biến đã ghi nhận, hãy ghi lại video chỉ số (chất lượng giám sát).

### Cài đặt Chụp (GUI)



<!-- SCREENSHOT-NEEDED: Capture Settings pane (gear next to Capture All) with a connected array: capture-mode buttons (Single/Continuous/Interval), the bulk export-type toggle row, the Fastest Capture toggle, and the per-array group card showing the Aligned checkbox and the "Record index video" / "Record raw burst" buttons. -->Biểu tượng bánh răng bên cạnh **Capture All** sẽ mở cửa sổ Cài đặt chụp (yêu cầu có dự án đang mở — các bản chụp sẽ được lưu vào đó):

* **Chế độ chụp**:**Single**(một lần quét) /**Continuous**(liên tục; giới hạn bởi số lần chụp, mặc định là 1, hoặc thời lượng, mặc định là 10 giây) /**Khoảng thời gian** (chụp thời gian trôi: N lần chụp mỗi khoảng thời gian X cho tổng số Y, mặc định là 1 lần mỗi 5 giây trong 1 phút).
* **Loại xuất theo từng camera**: Raw, Debayered, Radiance, Reflectance, Preview, Index — tất cả các tùy chọn áp dụng đều được bật theo mặc định. Radiance/Reflectance bị ẩn đối với các camera có bộ lọc &quot;RGB&quot;;**Reflectance chỉ xuất hiện khi camera có cảm biến ánh sáng DAQ** (của chính camera hoặc thừa hưởng từ mảng); Index yêu cầu biểu thức chỉ số đã được cấu hình.
* **Căn chỉnh**(theo mảng, mặc định**bật**): biến dạng các tệp xuất của thành viên theo cấu hình căn chỉnh của mảng để các tệp xuất được căn chỉnh theo pixel. Raw luôn giữ nguyên không biến dạng nhưng chứa thông tin biến đổi trong metadata.
* **Ghi nhanh nhất** (chế độ bật/tắt): chỉ dữ liệu thô + giá trị đọc DAQ được gán + bản tổng hợp chỉ số kết hợp miễn phí, bỏ qua các phép tính hiệu chuẩn tại thời điểm ghi để đạt tốc độ tối đa — tái tạo độ sáng/độ phản xạ/chỉ số sau này từ tệp `.daq` đã lưu.
* Các lựa chọn được lưu lại cùng với dự án. Các camera bị ẩn hoặc tạm dừng sẽ bị bỏ qua.

Tương đương với CLI (cùng điểm cuối backend, cùng ngữ nghĩa):

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

Nén TIFF cho các lần ghi là `deflate` (không mất dữ liệu, mặc định) hoặc `none` — bảng cờ đầy đủ, cấu trúc thư mục ghi và quy tắc xử lý lại được mô tả trong [Tài liệu tham khảo CLI](../reference/cli-reference.md#capture-modes-recorders--offline-reprocess).

## Ghép nối cảm biến ánh sáng DAQ

Các bản xem trước đã hiệu chỉnh độ phản xạ và độ chiếu sáng cần dữ liệu ánh sáng chiếu xuống từ cảm biến DAQ (được kết nối trong tab **Cảm biến ánh sáng**):

* **Hàng mảng**trên thanh bên hiển thị một**nút &quot;DAQ · bật/tắt&quot;** — *bật* khi cảm biến ánh sáng cấp mảng được thiết lập **hoặc** bất kỳ camera thành viên nào có cảm biến riêng; chú thích của nó liệt kê chính xác cảm biến nào cung cấp dữ liệu cho camera nào.
* Gán cài đặt chung cho toàn mảng trong phần cài đặt mảng → **Cảm biến ánh sáng môi trường**→ menu thả xuống**Cảm biến ánh sáng**. Lựa chọn này được lưu cùng dự án, áp dụng cho mọi camera thành viên, và các camera riêng lẻ vẫn có thể ghi đè bằng cảm biến riêng của mình.
* Dòng trạng thái bên dưới hiển thị trạng thái thời gian thực: **Tắt**→ &quot;Đang chờ phổ ánh sáng đầu tiên…&quot; →**&quot;Hoạt động — tất cả camera trong mảng đều được hiệu chỉnh độ sáng&quot;** → hoặc, nếu không có phổ ánh sáng mới nào đến trong 3 giây qua, sẽ hiển thị thông báo &quot;quá hạn&quot; — giá trị đọc cuối cùng vẫn tiếp tục được sử dụng (các giá trị đọc không bao giờ hết hạn trên đường dẫn ghi hình).

Khi đã gán cảm biến: loại xuất dữ liệu Phản xạ sẽ khả dụng, bản xem trước trực tiếp được hiệu chỉnh ánh sáng, tính năng phơi sáng tự động dự đoán có thể sử dụng phổ, và mỗi lần chụp phản xạ sẽ ghi giá trị DAQ thực tế được sử dụng dưới dạng **`.daq` sidecar** bên cạnh hình ảnh để có thể xử lý lại dữ liệu chụp sau này.

## Tùy chọn &quot;CLI&quot; của `.daq`

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--serials SN1,SN2,…` | tự động phát hiện tất cả các camera LATTICE (cần ≥2) | **Cổng nối tiếp đầu tiên là MASTER.** |
| `--line {Line0,Line2,Line3}` | `Line2` | Dòng đồng bộ GPIO. |
| `--target-fps F` | tự động | Tần suất kích hoạt của MASTER. |
| `--binning {1,2,4}` | tự động | Phân nhóm phần cứng. |
| `--force-tier {sim-capture-sim-emit, sim-capture-ftd-stagger, slip-emit-and-capture}` | tự động | Chế độ chuyên gia để ghi đè bộ chọn cấp độ đồng bộ. |
| `--wire-ceiling-mbps MB_PER_S` | tự động phát hiện | Ngân sách băng thông máy chủ (MB/s) — dạng &quot;CLI&quot; của trường **Wire Budget**. Hãy giảm giá trị này nếu mảng báo cáo các khung dữ liệu bị hỏng. Giá trị này được lưu cùng dự án, do đó việc kết nối lại sau này sẽ khôi phục lại giá trị này. |
| `--no-recommend` | tắt | Bỏ qua bước phân tích mạng. |
| `--no-ptp` | tắt | Vô hiệu hóa PTP (dấu thời gian giữa các camera sẽ không thể so sánh được). |

`lattice array-list`, `array-status` và `array-disconnect` quản lý phiên làm việc liên tục. Tham chiếu đầy đủ về các lệnh con, bao gồm căn chỉnh (`align-calibrate` / `align-apply`) và các công cụ mạng, có trong [CLI Reference § chloros-cli lattice](../reference/cli-reference.md#chloros-cli-lattice); các lệnh tương đương trong SDK (`connect_array`, `ArraySession`, `attach_array`, `analyze_array_network`) nằm trong [SDK Reference](../reference/sdk-reference.md). Theo [Python], ngân sách dây là `connect_array(..., wire_ceiling_mbps=120)`, và việc phân chia giữa dữ liệu bị hỏng khi đang hoạt động và dữ liệu chưa bao giờ đến được được trình bày tại [`/api/camera/array/<id>/capability`](../reference/sdk-reference.md#array-health--which-subsystem-is-losing-frames).
