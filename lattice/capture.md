# Cài đặt và chế độ chụp

Quá trình chụp trong tab “Camera” được điều khiển bởi một nút màu đỏ **Chụp tất cả**và một khung**Cài đặt chụp**, nơi quyết định kết quả mà nút đó tạo ra: những camera nào tham gia, từng camera lưu dưới dạng xuất nào, và liệu màn trập sẽ kích hoạt một lần, liên tục hay theo khoảng thời gian. Trang này mô tả toàn bộ quy trình — từ cấu hình, quá trình chụp ảnh, vị trí lưu trữ tệp trên đĩa, cho đến cách xử lý lại các tệp này thành sản phẩm đã được hiệu chuẩn sau này. Các điều khiển liên quan đến camera và mảng camera được đặt tại [Cài đặt Camera](camera-settings.md).

{% hint style="info" %}
**Quá trình chụp yêu cầu phải mở một dự án.** Tùy chọn &quot;Chụp tất cả&quot; và biểu tượng bánh răng &quot;Cài đặt chụp&quot; sẽ bị vô hiệu hóa cho đến khi có dự án được mở (&quot;Tạo hoặc mở một dự án để lưu các bản chụp&quot;). Mỗi bản chụp sẽ được lưu trong thư mục dự án tại `captures/`.
{% endhint %}

## Bảng Cài đặt Ghi hình

Mở bảng này bằng cách nhấp vào **biểu tượng bánh răng bên cạnh &quot;Ghi hình Tất cả&quot;**trong danh sách camera ở thanh bên, hoặc bằng nút**&quot;Mở Cài đặt Ghi hình…&quot;** ở cuối bất kỳ bảng cài đặt riêng cho từng camera nào. Tiêu đề hiển thị “Cài đặt chụp ảnh” kèm theo nút quay lại ←.

<!-- SCREENSHOT-NEEDED: the full Capture Settings pane — Single/Continuous/Interval mode buttons at top, the bulk export-type toggle rows (All Raw … All Index), the orange Fastest Capture toggle, an array group card with the Aligned checkbox and Record buttons, and an expanded per-camera row showing per-type checkboxes. -->

Các lựa chọn của bạn tại đây — camera được chọn, các ô chọn theo loại và chế độ chụp — được lưu **theo từng dự án** và khôi phục khi bạn mở lại dự án đó.

### Chế độ chụp

Ba nút chế độ ở đầu bảng:

| Chế độ | Chức năng | Cài đặt phụ (mặc định) |
| --- | --- | --- |
| **Đơn lẻ** *(mặc định)* | Chụp một lần trên tất cả các camera đã chọn. | — |
| **Liên tục**| Ghi hình liên tiếp cho đến khi đạt điều kiện dừng. | Dừng theo**Số lần ghi hình** (mặc định 1) *hoặc* **Thời lượng ghi hình** (mặc định 10 giây; đơn vị: giây / phút / giờ / ngày). |
| **Khoảng thời gian**(timelapse) | Chụp liên tục theo hẹn giờ. |**Số lần chụp / khoảng thời gian**(mặc định 1) ·**Mỗi**N đơn vị (mặc định 5 giây) ·**Trong** N đơn vị (mặc định 1 phút). |

Trong chế độ Liên tục hoặc Khoảng thời gian, nút Chụp tất cả sẽ chuyển thành nút **Dừng (N)** khi đang chạy, đồng thời đếm số lần chụp khi chúng được ghi lại.

<!-- SCREENSHOT-NEEDED: the capture-mode area of Capture Settings with Interval selected — showing the "Captures / interval", "Every N (unit)" and "For N (unit)" rows with their defaults (1, 5 s, 1 m). -->




### Chọn máy ảnh và loại xuất

Văn bản trợ giúp của khung này tóm tắt như sau: chọn máy ảnh và loại xuất mà &quot;Chụp Tất cả&quot; sẽ tạo ra — tất cả đều được bật theo mặc định, và các lựa chọn này được lưu cùng với dự án này.

* Các nút **Chọn tất cả / Không chọn gì cả** sẽ đồng thời bật/tắt hộp kiểm &quot;Bao gồm&quot; của mọi máy ảnh.
* **Các nút chuyển đổi loại xuất hàng loạt**(hai hàng nút):**Tất cả Raw / Tất cả Debayered / Tất cả Preview / Tất cả Radiance / Tất cả Reflectance / Tất cả Index**. Mỗi tùy chọn có ba trạng thái màu: xanh lá ✓ = bật cho mọi camera hỗ trợ, vàng – = bật cho một số camera, xám = không có camera nào hỗ trợ. Nút chuyển đổi sẽ bị vô hiệu hóa khi không có camera nào được kết nối hỗ trợ loại đó. Tất cả các tùy chọn này sẽ chuyển sang màu xám khi chế độ &quot;Fastest Capture&quot; đang bật.
* **Các hàng dành riêng cho từng máy ảnh**: một ô chọn “Bao gồm”, cùng với danh sách có thể mở rộng (▸/▾) các loại xuất áp dụng cho máy ảnh đó kèm theo các ô chọn riêng lẻ. Hàng này hiển thị số lượng đã bật như “4/6”.

### Các loại xuất và camera nào hỗ trợ chúng

Có sáu loại xuất: **Raw, Debayered, Radiance, Reflectance, Preview, Index**. Chỉ các loại áp dụng mới xuất hiện trên hàng của từng camera:

| Loại xuất | Nội dung |RGB

(FRGB) | Bayer đa quang phổ (FRGN/FOCN/FNGB) | Đơn sắc (M3M) |
| --- | --- | --- | --- | --- |
| **Raw** | Mô-zaic Bayer (đơn sắc: dải đơn) trực tiếp từ cảm biến | ✓ | ✓ | ✓ |
| **Debayered** | Demosaic tuyến tính (đơn sắc: thang xám 1 kênh) | ✓ | ✓ | ✓ |
| **Xem trước** | Chuỗi hiển thị đầy đủ (cân bằng trắng + gamma theo cấu hình máy ảnh; đa quang phổ: kéo dãn màu giả) | ✓ | ✓ | ✓ |
| **Độ sáng** | float32 W/m²/sr/nm qua chuỗi đo quang học đầy đủ | — (không hỗ trợ) | ✓ | ✓ |
| **Độ phản xạ** | uint16 ρ (32768 = 1.0) | — (không được cung cấp) | ✓ — chỉ hiển thị khi máy ảnh có cảm biến ánh sáng DAQ (của riêng máy ảnh hoặc thừa hưởng từ mảng) | giống như đa quang phổ |
| **Chỉ số** | Hiển thị chỉ số thực vật (LUT) | — | ✓ — yêu cầu biểu thức chỉ số được kích hoạt và không trống trên camera, và không được cung cấp cho các thành viên mảng kết hợp (mảng sở hữu một chỉ số chung) | — (một chỉ số cần ≥2 dải; xem [Camera đơn sắc &amp; Chỉ số thực vật](mono-indices.md)) |

Độ rạng rỡ và độ phản xạ không bao giờ được cung cấp cho các máy ảnhRGB

— độ rạng rỡ theo từng điểm Bayer không có ý nghĩa đối với cảm biến quang đo dải rộng.

### Chụp nhanh nhất

Công tắc **⚡ Chụp nhanh nhất — chỉ định dạng thô**(màu cam khi bật) sẽ ghi đè lên tất cả các lựa chọn xuất sang**chỉ định dạng thô** — cộng với một ảnh tổng hợp chỉ số kết hợp miễn phí cho các mảng — để khung hình được lưu nhanh nhất có thể: các phép tính về độ rạng rỡ/độ phản xạ/hiển thị được bỏ qua hoàn toàn tại thời điểm chụp.

{% hint style="info" %}
**Một tệp `.daq` vẫn được lưu lại.** Khi một cảm biến ánh sáng được gán, Chế độ chụp nhanh nhất vẫn ghi lại giá trị DAQ hướng xuống bên cạnh các khung hình thô — do đó, các sản phẩm về độ rạng rỡ, độ phản xạ và chỉ số đều có thể được tạo ra sau này bằng cách xử lý lại (xem [Xử lý lại các lần chụp](#re-processing-captures-into-calibrated-products)). Fastest Capture cũng không làm mất các lựa chọn trong ô chọn của bạn: tắt tính năng này đi và các lựa chọn sẽ được khôi phục.
{% endhint %}

### Các điều khiển cho từng mảng

Mỗi mảng được kết nối sẽ có thẻ nhóm riêng trong khung:

* **Hộp kiểm &quot;Include&quot;** (có ba trạng thái trên các thành viên) và tên của mảng cùng với chế độ hiển thị: &quot;(combined | separate)&quot;.
* Hộp kiểm **Căn chỉnh**(mặc định**bật**): biến dạng các thành viên xuất ra theo cấu hình căn chỉnh của mảng để các tệp xuất được căn chỉnh theo pixel trên các máy ảnh. Dữ liệu thô vẫn chưa được biến dạng nhưng mang theo phép biến đổi trong siêu dữ liệu của nó. (Hồ sơ này được tính toán trong [khung cài đặt mảng](camera-settings.md#alignment-co-registration-combined-only).)
* Các hàng camera thành viên được lồng ghép bên trong thẻ.

Thẻ mảng cũng chứa hai bộ ghi. Hãy coi chúng là **giám sát so với phân tích**:

| Bộ ghi | Cấp độ | Nội dung ghi lại |
| --- | --- | --- |
| **● Ghi video chỉ mục / ■ Dừng ghi** *(chỉ áp dụng cho mảng kết hợp)* | **Giám sát** | Hình ảnh tổng hợp chỉ mục kết hợp trực tiếp dưới dạng video ở tốc độ 10 khung hình/giây — 8-bit, độ phân giải xem trước, LUT được tích hợp sẵn. Cần có dự án đang mở và chế độ xem trực tiếp. Hiển thị các khung hình + thời gian đã trôi qua trong khi ghi. |
| **⦿ Ghi liên tiếp thô / ■ Dừng chuỗi ảnh thô** *(bất kỳ mảng nào)* | **Phân tích**| Các khung hình Bayer thô ở tốc độ chụp trực tiếp (không xử lý) kèm theo bản kê khai từng khung hình và các giá trị `.daq`, được lưu vào tệp `captures/bursts/`. Sau khi chụp liên tiếp, nút**Tạo video** sẽ xuất hiện: nút này sẽ xử lý lại các khung hình đã chụp ngoại tuyến thành video đã hiệu chỉnh — chỉ số kết hợp và/hoặc độ rạng rỡ / độ phản xạ / chỉ số cho từng camera — cùng với các tệp TIFF tùy chọn. Quá trình tạo chỉ số kết hợp sẽ tự động bắt đầu khi bạn dừng chụp liên tiếp. |

<!-- SCREENSHOT-NEEDED: an array group card in Capture Settings while a raw burst is recording — the ⦿/■ burst button in its recording state with frame count, and (in a second capture) the Build video button that appears after stopping. -->

## Quy trình

<!-- SCREENSHOT-NEEDED: the sidebar during a capture — Capture All showing live "Capturing… 3/6" progress text, and (second capture) the result flash "Saved N files". -->

“Capture All” Nhấn **Capture All** trong danh sách camera ở thanh bên:

1. Mọi camera được bao gồm, hiển thị và chưa bị tạm dừng sẽ ghi hình với các định dạng xuất đã chọn. **Các mảng camera sẽ kích hoạt đồng bộ như một tín hiệu duy nhất** (một nhóm đồng bộ duy nhất bao gồm tất cả thành viên — xem [Mảng Nhiều Camera](arrays.md)); các camera độc lập sẽ ghi hình riêng lẻ.
2. Các camera ẩn (biểu tượng mắt) hoặc đang tạm dừng sẽ bị bỏ qua. Một mảng chỉ bị chặn hoàn toàn khi *tất cả* các thành viên của nó đều ẩn hoặc đang tạm dừng.
3. Bất cứ khi nào một cảm biến ánh sáng được gán, giá trị đo ánh sáng chiếu xuống tương ứng từ DAQ sẽ được lưu dưới dạng tệp `.daq` cùng với hình ảnh — ngay cả đối với các bản ghi chỉ ở định dạng thô — để các sản phẩm đo bức xạ luôn có thể được tính toán sau này.
4. Nút này hiển thị tiến trình trực tiếp — “Đang chụp… hoàn tất/tổng số” — và ở chế độ Liên tục/Khoảng thời gian sẽ chuyển thành **Dừng (N)**. Mỗi mục chụp có thời gian chờ tối đa là 300 giây.
5. Khi đợt chụp kết thúc, một thông báo kết quả sẽ hiển thị **&quot;Đã lưu N tệp&quot;**hoặc**&quot;Đã lưu N, F thất bại&quot;**, kèm theo &quot;(S ẩn/tạm dừng/bỏ qua)&quot; khi các camera bị bỏ qua.

## Vị trí lưu trữ các tệp ghi

Các tệp ghi được lưu trong dự án đang mở tại `<project>/captures/`. Mỗi loại xuất sẽ được lưu trong **thư mục con riêng**, do đó một lần ghi đa cấp sẽ không bao giờ trộn lẫn các loại:

```
<project>/captures/
├── raw/           capture_<ts>_SN<serial>_raw.tif
├── debayered/     capture_<ts>_SN<serial>_debayered.tif
├── radiance/      capture_<ts>_SN<serial>_radiance.tif
├── reflectance/   capture_<ts>_SN<serial>_reflectance.tif
├── preview/       capture_<ts>_SN<serial>_display.tif
├── index/         per-camera vegetation-index (LUT) render, when Index is selected
├── composite/     array foreground/background live-view composite, when produced
├── bursts/        raw-burst recordings (frames + manifest + .daq per burst)
└── *.daq          the downwelling reading matched to the capture
```

* `<ts>` là dấu thời gian ghi hình và `<serial>` là số sê-ri camera. Các bản chụp độc lập được đặt tên là `capture_<ts>_SN<serial>_<level>`; các bản chụp mảng từ một tín hiệu kích hoạt đồng bộ được đặt tên là `sync_<ts>_SN<serial>_<level>` và **chia sẻ cùng một dấu thời gian cho tất cả các máy ảnh trong nhóm** (phần hậu tố cấp độ sẽ bị bỏ đi khi một camera chỉ lưu một cấp độ duy nhất).
* **Một điểm bất đối xứng cần lưu ý:** cấp độ hiển thị được lưu trữ trong một thư mục có tên `preview/` trong khi các tệp vẫn giữ tên `_display` — thư mục và hậu tố chỉ khác nhau đối với cấp độ đó.
* Các cấp độ không xác định sẽ được lưu vào một thư mục mang tên của chính chúng; nếu không thể tạo thư mục con, tệp sẽ được ghi vào thư mục gốc của các bản chụp thay vì bị mất.
* Các tệp TIFF ghi lại được nén không mất dữ liệu (DEFLATE) theo mặc định và chứa toàn bộ siêu dữ liệu hiệu chuẩn và xử lý **bên trong tệp XMP** — các bản ghi tự mô tả, không có tệp phụ nào khác ngoài tệp `.daq`.

Đây là cùng một cấu trúc mà `chloros-cli lattice capture` / `array-capture` ghi vào thư mục `-o` của chúng — được ghi chép trong [CLI

Tài liệu tham khảo § Cấu trúc của thư mục captures](../reference/cli-reference.md#what-a-captures-folder-looks-like).

<!-- SCREENSHOT-NEEDED: OS file explorer showing a real <project>/captures/ folder after a multi-level array capture — the raw/debayered/radiance/reflectance/preview subfolders, a .daq file at the root, and sync_<ts>_SN<serial>_<level>.tif filenames visible inside one subfolder. -->

## Xử lý lại các bản chụp thành sản phẩm đã hiệu chuẩn

Các khung hình thô đã chụp cùng với tệp `.daq` đã lưu là tất cả những gì quy trình xử lý cần — đây là lý do tại sao chế độ Chụp Nhanh (Fastest Capture) an toàn để sử dụng trong công việc thực tế.

* **Giao diện người dùng (GUI)**: thêm thư mục chứa các khung hình đã chụp vào một dự án ([Thêm tệp vào dự án](../processing-images-gui/adding-files-to-a-project.md)) và xử lý như bình thường.
* **Chế độ dòng lệnhCLI**: chỉ định tệp `process` đến **thư mục gốc chứa các khung hình đã chụp**:

```bash
chloros-cli process "C:/ChlorosProjects/MyField/captures"
```

`process` thông thường chỉ nhập thư mục mà bạn chỉ định, nhưng khi thư mục đó không chứa hình ảnh và có các thư mục con, nó sẽ tự động đi sâu vào các thư mục con — do đó, các thư mục con cấp dưới và các tệp gốc `.daq` sẽ được nhập cùng một lúc. Mỗi lần thu thập dữ liệu sẽ được nhập dưới dạng **một hình ảnh duy nhất** với các cấp độ khác được đính kèm dưới dạng chế độ xem, chứ không phải một hình ảnh cho mỗi cấp độ.

Việc đặt tên trực tiếp cho thư mục con của cấp độ (ví dụ: `…/captures/raw/`) cũng hoạt động, nhưng sẽ bỏ qua các tệp gốc `.daq` — hãy sao chép chúng cùng lúc khi bạn tái tạo sản phẩm phóng xạ từ `raw/`, nếu không thì việc khớp dấu thời gian sẽ không có đối tượng nào để giải quyết.

{% hint style="warning" %}
**Quá trình xử lý luôn bắt đầu từ `raw`.** Trong mỗi lần thu thập, khung hình thô là nguồn của quy trình xử lý; `debayered`, `radiance`, `reflectance`, và `preview` xuất hiện dưới dạng các chế độ hiển thị nhưng không bao giờ được đưa trở lại qua đường ống — việc xử lý lại một sản phẩm dẫn xuất sẽ áp dụng lại các hiệu ứng viền tối, màu sắc và toán học độ sáng đã được tích hợp sẵn vào các pixel của nó, do đóChloros

từ chối thay vì xử lý hai lần. Các bản hiển thị `index/` và `composite/` hoàn toàn không bao giờ được xử lý (chúng là đầu ra, không phải dữ liệu thu thập). Thư mục captures được lưu **mà không** có tệp RAW nhập vào và hiển thị bình thường, nhưng `process` sẽ bỏ qua nó và thông báo điều đó; `--input-level {raw,debayered,processed}` là lối thoát dự phòng được thiết kế sẵn để buộc tạo điểm vào. Xem [Tài liệu tham khảoCLI

](../reference/cli-reference.md#what-a-captures-folder-looks-like) để biết chính xác các thông báo bỏ qua.
{% endhint %}

Hai hành vi khác cần lưu ý khi viết kịch bản xử lý lại:

* Một lần chạy `chloros-cli process` yêu cầu sản phẩm nhưng **không tạo ra bất kỳ sản phẩm hình ảnh nào** sẽ báo lỗi rõ ràng và thoát với giá trị không bằng 0 — bạn sẽ không bao giờ nhận được một lần chạy trống im lặng. Các lần chạy thành công sẽ báo cáo số lượng sản phẩm của chúng. (Một lần chạy chỉ chứa metadata theo chủ đích vẫn được coi là thành công.)
* Các tệp xuất đã xử lý được nhập lại sẽ không bao giờ chiếm vị trí tệp thô của một lần thu thập — tệp thô gốc luôn là nguồn của quy trình xử lý.

## Các tùy chọn tương đương vớiCLI



Tất cả các thao tác trên trang này đều có thể thực hiện ở chế độ không giao diện người dùng (headless). Các chế độ thu thập trên giao diện người dùng (GUI) tương ứng trực tiếp với `chloros-cli lattice array-capture`:

| GUI |CLI

|
| --- | --- |
| Đơn lẻ | `chloros-cli lattice array-capture` |
| Liên tục | `array-capture --continuous [--count N] [--duration S]` |
| Theo khoảng thời gian | `array-capture --interval S [--duration S]` |
| Ghi lại nhanh nhất | `array-capture --fastest` |
| Hộp kiểm căn chỉnh | `--aligned / --no-aligned` |
| Các ô chọn loại xuất | `--processing LEVEL` hoặc `--levels L1,L2,…` (mặc định là `all`) |
| Ghi video chỉ mục | `chloros-cli lattice array-record` |
| Ghi chuỗi ảnh thô / Tạo video | `chloros-cli lattice array-burst` / `array-build-video` |

Bảng cờ đầy đủ, tùy chọn chụp ổn định smart-AE (`--smart`) và mô hình tốc độ duy trì được định nghĩa trong [Tham khảoCLI

§ Chế độ chụp, Thiết bị ghi và Xử lý ngoại tuyến](../reference/cli-reference.md#capture-modes-recorders--offline-reprocess).
