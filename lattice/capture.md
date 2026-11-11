# Cài đặt và chế độ chụp

Quá trình chụp trong tab “Camera” được điều khiển bởi một nút màu đỏ **Chụp tất cả**và một khung**Cài đặt chụp**, nơi quyết định kết quả mà nút đó tạo ra: những camera nào tham gia, từng camera lưu dưới dạng xuất nào, và liệu màn trập sẽ kích hoạt một lần, liên tục hay theo khoảng thời gian nhất định. Trang này ghi lại toàn bộ quy trình — cấu hình, quá trình chụp, vị trí lưu trữ tệp trên đĩa và cách xử lý lại chúng thành các sản phẩm đã được hiệu chuẩn sau này. Các điều khiển camera và mảng camera được đặt tại [Cài đặt Camera](camera-settings.md).

{% hint style="info" %}
**Quá trình chụp ảnh yêu cầu phải có một dự án đang mở.** Tùy chọn &quot;Capture All&quot; và biểu tượng bánh răng &quot;Capture Settings&quot; sẽ bị vô hiệu hóa cho đến khi có dự án được mở (&quot;Tạo hoặc mở một dự án để lưu các lần chụp&quot;). Mỗi lần chụp sẽ được lưu trong thư mục dự án tại `captures/`.
{% endhint %}

## Cửa sổ Cài đặt Ghi hình

Mở cửa sổ này bằng cách nhấp vào **biểu tượng bánh răng bên cạnh &quot;Ghi hình tất cả&quot;**trong danh sách camera ở thanh bên, hoặc bằng nút**&quot;Mở Cài đặt Ghi hình…&quot;** ở cuối bất kỳ cửa sổ cài đặt riêng cho từng camera nào. Tiêu đề hiển thị là &quot;Cài đặt Ghi hình&quot; kèm nút quay lại ←.

<!-- SCREENSHOT-NEEDED: the full Capture Settings pane — Single/Continuous/Interval mode buttons at top, the bulk export-type toggle rows (All Raw … All Index), the orange Fastest Capture toggle, an array group card with the Aligned checkbox and Record buttons, and an expanded per-camera row showing per-type checkboxes. -->



Các lựa chọn của bạn tại đây — camera được chọn, các ô chọn theo loại và chế độ chụp — được lưu **theo từng dự án** và khôi phục khi bạn mở lại dự án.

### Chế độ chụp

Ba nút chế độ ở đầu bảng điều khiển:

| Chế độ | Chức năng | Cài đặt phụ (mặc định) |
| --- | --- | --- |
| **Đơn lẻ** *(mặc định)* | Một lần chụp trên tất cả các camera đã chọn. | — |
| **Liên tục**| Chụp liên tiếp cho đến khi đạt điều kiện dừng. | Dừng theo**Số lần chụp** (mặc định 1) *hoặc* **Thời lượng chụp** (mặc định 10 giây; đơn vị: giây / phút / giờ / ngày). |
| **Khoảng thời gian**(timelapse) | Chụp liên tục theo hẹn giờ. |**Số lần chụp / khoảng thời gian**(mặc định 1) ·**Mỗi**N đơn vị (mặc định 5 giây) ·**Trong** N đơn vị (mặc định 1 phút). |

Trong chế độ Liên tục hoặc Khoảng thời gian, nút Chụp tất cả sẽ chuyển thành nút **Dừng (N)** khi đang chạy, đồng thời đếm số lần chụp khi chúng được ghi lại. Một ổ đĩa đầy sẽ dừng quá trình chạy (xem [Quy trình &quot;Capture All&quot;](#the-capture-all-flow)).**Chế độ Liên tục trên mảng chạy bên trong backend (mới trong phiên bản 1.3.0).** Khi mọi camera trong danh sách đều thuộc về một mảng (các camera độc lập bị ẩn hoặc loại trừ), chế độ Liên tục sẽ không còn gửi một yêu cầu cho mỗi lần ghi lại: backend Chloros

tự chạy vòng lặp ghi lại, ghi một nhóm đồng bộ hóa trong khi chọn nhóm tiếp theo, và nút này sẽ kiểm tra số lượng của nó. Với các camera độc lập trong danh sách, Chloros

sẽ ghi lại từng đợt như trước đây.

{% hint style="warning" %}
**Chụp nhanh trên Windows

: hãy loại trừ backend Chloros

khỏi Microsoft Defender.** Việc quét thời gian thực từng tệp mà chế độ chụp tốc độ cao ghi ra tiêu tốn khoảng 30% tốc độ chụp trong các thử nghiệm của MAPIR

(hàng trăm tệp mỗi giây với vùng quan tâm (ROI) nhỏ). Trình cài đặt Windows

cung cấp tùy chọn **&quot;Loại trừ backend Chloros

khỏi quét của Microsoft Defender (ghi hình camera nhanh hơn nhiều; được khuyến nghị)&quot;**, được chọn mặc định, và sẽ gỡ bỏ loại trừ này khi gỡ cài đặt. Nếu bạn đã bỏ chọn tùy chọn này, hãy chạy lại trình cài đặt với tùy chọn này được chọn, hoặc thêm một ngoại lệ quy trình Defender cho `chloros-backend.exe` (trong thư mục `backend` của thư mục cài đặt Chloros

). Khi Defender sử dụng một lõi CPU trở lên trong quá trình ghi hình liên tục của một mảng camera, backend Chloros

sẽ ghi lại điều này một lần trong nhật ký của nó, kèm theo hai giải pháp tương tự như trên.
{% endhint %}

<!-- SCREENSHOT-NEEDED: the capture-mode area of Capture Settings with Interval selected — showing the "Captures / interval", "Every N (unit)" and "For N (unit)" rows with their defaults (1, 5 s, 1 m). -->




### Chọn camera và loại xuất

Văn bản trợ giúp của khung này tóm tắt như sau: chọn camera và loại xuất mà &quot;Capture All&quot; sẽ tạo ra — mọi tùy chọn đều được bật theo mặc định và các lựa chọn này sẽ được lưu cùng với dự án này.

* Các nút **Chọn tất cả / Không chọn gì cả** sẽ đồng thời bật/tắt hộp kiểm &quot;Bao gồm&quot; của mọi camera.
* **Các nút chuyển đổi loại xuất hàng loạt**(hai hàng nút):**Tất cả Raw / Tất cả Debayered / Tất cả Preview / Tất cả Radiance / Tất cả Reflectance / Tất cả Index**. Mỗi tùy chọn có ba trạng thái màu: xanh lá ✓ = bật cho mọi camera hỗ trợ, vàng – = bật cho một số, xám = không có camera nào hỗ trợ. Một nút chuyển đổi sẽ bị vô hiệu hóa khi không có camera nào được kết nối hỗ trợ loại đó. Tất cả các nút này sẽ chuyển sang màu xám khi chế độ &quot;Fastest Capture&quot; được bật.
* **Các hàng dành riêng cho từng camera**: một ô chọn &quot;Bao gồm&quot;, kèm theo danh sách có thể mở rộng (▸/▾) các loại xuất áp dụng cho camera đó với các ô chọn riêng lẻ. Hàng hiển thị số lượng đã bật như &quot;4/6&quot;.

### Các loại xuất và camera nào hỗ trợ chúng

Có sáu loại xuất: **Raw, Debayered, Radiance, Reflectance, Preview, Index**. Chỉ các loại áp dụng mới xuất hiện trên hàng của từng camera:

| Loại xuất | Nội dung | RGB

(FRGB) | Đa quang phổ Bayer (FRGN/FOCN/FNGB) | Đơn sắc (M3M) |
| --- | --- | --- | --- | --- |
| **Raw** | Mô-đun Bayer (đơn sắc: dải đơn) trực tiếp từ cảm biến | ✓ | ✓ | ✓ |
| **Đã loại bỏ hiệu ứng Bayer** | Loại bỏ hiệu ứng Bayer tuyến tính: loại bỏ độ lệch tối (DSNU) của gói hiệu chuẩn khỏi dữ liệu thô, sau đó thực hiện loại bỏ hiệu ứng Bayer có nhận diện cạnh — không có hiệu chỉnh tuyến tính, trường phẳng hoặc tách màu. Được lưu dưới dạng tệp RGB

3 kênh TIFF

(đơn sắc: 1 kênh thang xám) | ✓ | ✓ | ✓ |
| **Xem trước** | Chuỗi hiển thị đầy đủ (cân bằng trắng + gamma theo cấu hình máy ảnh, cộng với cài đặt Điểm sáng (Highlights) của máy ảnh RGB

; đa quang phổ: kéo dãn màu giả) | ✓ | ✓ | ✓ |
| **Độ rạng rỡ** | float32 W/m²/sr/nm thông qua chuỗi đo quang học đầy đủ | — (không hỗ trợ) | ✓ | ✓ |
| **Độ phản xạ** | uint16 ρ (32768 = 1,0) | — (không được cung cấp) | ✓ — chỉ hiển thị khi camera có cảm biến ánh sáng DAQ (của riêng nó hoặc thừa hưởng từ mảng) | giống như đa phổ |
| **Chỉ số** | Hiển thị chỉ số thực vật (LUT) | — | ✓ — yêu cầu biểu thức chỉ số được kích hoạt và không trống trên camera, và không được cung cấp cho các thành viên mảng kết hợp (mảng sở hữu một chỉ số chung) | — (một chỉ số cần ≥2 dải; xem [Camera đơn sắc &amp; Chỉ số thực vật](mono-indices.md)) |

Độ rạng rỡ và độ phản xạ không bao giờ được cung cấp cho máy ảnh RGB

— độ rạng rỡ theo từng điểm Bayer không có ý nghĩa đối với cảm biến quang đo dải rộng.

### Chụp nhanh nhất

Công tắc **⚡ Ghi nhanh nhất — chỉ định dạng thô**(màu cam khi bật) sẽ ghi đè lên tất cả các lựa chọn xuất sang chế độ**chỉ định dạng thô** — kèm theo một bản tổng hợp chỉ số kết hợp miễn phí cho các mảng — để khung hình được lưu nhanh nhất có thể: các phép tính về độ rạng rỡ/độ phản xạ/hiển thị sẽ được bỏ qua hoàn toàn tại thời điểm ghi.

{% hint style="info" %}
**Một tệp `.daq` vẫn được lưu lại.** Khi một cảm biến ánh sáng được gán, Chế độ chụp nhanh nhất vẫn ghi lại giá trị đo hướng xuống của DAQ (vào thư mục `daq/` của các khung hình) — do đó, các sản phẩm về độ rạng rỡ, độ phản xạ và chỉ số đều có thể được xây dựng sau này thông qua quá trình xử lý lại (xem [Xử lý lại các bản chụp](#re-processing-captures-into-calibrated-products)). Chế độ Fastest Capture cũng không làm mất các lựa chọn trong hộp kiểm của bạn: tắt chế độ này đi và các lựa chọn sẽ được khôi phục.
{% endhint %}

### Các điều khiển cho từng mảng

Mỗi mảng được kết nối sẽ có thẻ nhóm riêng trong khung:

* **Hộp chọn &quot;Bao gồm&quot;** (có ba trạng thái cho các thành viên) và tên của mảng cùng chế độ hiển thị: &quot;(kết hợp | riêng biệt)&quot;.
* **Hộp kiểm Căn chỉnh**(mặc định**bật**): biến dạng các tệp thành viên theo hồ sơ căn chỉnh của mảng để các tệp xuất ra được căn chỉnh theo pixel giữa các máy ảnh. Dữ liệu thô vẫn giữ nguyên trạng thái chưa được biến dạng nhưng mang theo thông tin biến đổi trong siêu dữ liệu của nó. (Hồ sơ này được tính toán trong [khung cài đặt mảng](camera-settings.md#alignment-co-registration).) Các tệp thành viên của mảng được lưu ở định hướng gốc của từng máy ảnh, với bất kỳ cài đặt Gương/Xoay nào được ghi lại trong tệp thay vì được áp dụng.
* Các hàng camera thành viên được lồng ghép bên trong thẻ.

Thẻ mảng cũng chứa hai bộ ghi. Hãy coi chúng như **giám sát so với phân tích**:

| Bộ ghi | Cấp độ | Ghi lại gì |
| --- | --- | --- |
| **● Ghi video chỉ mục kết hợp / ■ Dừng ghi** *(chỉ dành cho mảng kết hợp)* | **Giám sát** | Hình ảnh tổng hợp chỉ mục kết hợp trực tiếp dưới dạng video ở tốc độ 10 khung hình/giây — 8-bit, độ phân giải xem trước, LUT được tích hợp sẵn. Cần có dự án đang mở và chế độ xem trực tiếp. Hiển thị các khung hình + thời gian đã trôi qua trong khi ghi. |
| **⦿ Ghi liên tiếp thô / ■ Dừng chuỗi ảnh thô** *(bất kỳ mảng nào)* | **Phân tích**| Các khung hình Bayer thô ở tốc độ chụp trực tiếp (không xử lý) kèm theo bản kê khai từng khung hình và các giá trị đo `.daq`, được lưu vào tệp `captures/bursts/`. Sau khi chụp liên tiếp, nút**Tạo video** sẽ xuất hiện: nút này sẽ xử lý lại các khung hình đã chụp ngoại tuyến thành video đã hiệu chỉnh — chỉ số kết hợp và/hoặc độ rạng rỡ / độ phản xạ / chỉ số cho từng camera — cùng với các tệp TIFF tùy chọn. Quá trình tạo chỉ số kết hợp sẽ tự động bắt đầu khi bạn dừng chụp liên tiếp. |

<!-- SCREENSHOT-NEEDED: an array group card in Capture Settings while a raw burst is recording — the ⦿/■ burst button in its recording state with frame count, and (in a second capture) the Build video button that appears after stopping. -->

## Quy trình

<!-- SCREENSHOT-NEEDED: the sidebar during a capture — Capture All showing live "Capturing… 3/6" progress text, and (second capture) the result flash "Saved N files". -->

“Capture All”

Nhấn **Capture All** trong danh sách camera ở thanh bên:

1. Mọi camera được bao gồm, hiển thị và chưa bị tạm dừng sẽ ghi hình theo các định dạng xuất đã chọn. **Các mảng camera sẽ kích hoạt đồng bộ như một tín hiệu duy nhất** (một nhóm đồng bộ duy nhất trên tất cả các thành viên — xem [Mảng Nhiều Camera](arrays.md)); các camera độc lập sẽ ghi hình riêng lẻ.
2. Các camera ẩn (biểu tượng mắt) hoặc đang tạm dừng sẽ bị bỏ qua. Một mảng chỉ bị chặn hoàn toàn khi *tất cả* các thành viên của nó đều ẩn hoặc đang tạm dừng.
3. Mỗi khi một cảm biến ánh sáng được gán, giá trị đo ánh sáng chiếu xuống tương ứng từ hệ thống thu thập dữ liệu (DAQ) sẽ được lưu dưới dạng tệp `.daq` trong thư mục `daq/` của các bản ghi — ngay cả đối với các bản ghi chỉ chứa dữ liệu thô — để các sản phẩm đo bức xạ luôn có thể được tính toán sau này.
4. Nút này hiển thị tiến trình thời gian thực — “Đang ghi… hoàn tất/tổng số” — và ở chế độ Liên tục/Khoảng thời gian sẽ chuyển thành **Dừng (N)**. Mỗi mục ghi hình có thời gian chờ tối đa là 300 giây.
5. Khi lần quét kết thúc, một thông báo kết quả sẽ hiển thị **&quot;Đã lưu N tệp&quot;**hoặc**&quot;Đã lưu N, F thất bại&quot;**, kèm theo &quot;(S ẩn/tạm dừng/bỏ qua)&quot; khi các camera bị bỏ qua. Nếu ổ đĩa đầy, quá trình sẽ dừng lại và báo cáo**&quot;Đã dừng: ổ đĩa đầy. Đã lưu N, F thất bại&quot;** — mọi tệp không thể ghi được đều được tính là thất bại.

## Vị trí lưu trữ các tệp ghi lại

Các tệp ghi lại được lưu trong dự án đang mở tại `<project>/captures/`. Mỗi loại xuất được lưu trong **thư mục con riêng**, do đó quá trình ghi lại nhiều cấp độ sẽ không bao giờ trộn lẫn các loại:

```
<project>/captures/
├── raw/           capture_<ts>_SN<serial>_raw.tif
├── debayered/     capture_<ts>_SN<serial>_debayered.tif
├── radiance/      capture_<ts>_SN<serial>_radiance.tif
├── reflectance/   capture_<ts>_SN<serial>_reflectance.tif
├── preview/       capture_<ts>_SN<serial>_display.tif
├── index/         per-camera vegetation-index (LUT) render, when Index is selected
├── composite/     array foreground/background live-view composite, when produced
├── clamp_masks/   radiance/…_clampmask.tif, reflectance/…, index/… — unmix clamp masks,
│                  one subfolder per product folder, only when a band was clamped
├── bursts/        raw-burst recordings (frames + manifest + .daq per burst)
└── daq/           the downwelling reading(s) matched to each capture (.daq), each with a
                   .daq.cameras.json naming the cameras it describes
```

* `<ts>` là dấu thời gian của bản ghi và `<serial>` là số sê-ri của camera. Các bản chụp độc lập được đặt tên là `capture_<ts>_SN<serial>_<level>`; các bản chụp mảng từ một tín hiệu kích hoạt đồng bộ được đặt tên là `sync_<ts>_SN<serial>_<level>` và **chia sẻ cùng một dấu thời gian cho tất cả các máy ảnh trong nhóm** (phần hậu tố mức độ sẽ bị bỏ đi khi một camera chỉ lưu một mức độ duy nhất).
* **Một điểm bất đối xứng cần lưu ý:** cấp độ hiển thị được lưu trữ trong một thư mục có tên `preview/` trong khi các tệp vẫn giữ `_display` trong tên — thư mục và hậu tố chỉ khác nhau đối với cấp độ đó.
* Các cấp độ không xác định sẽ được lưu vào một thư mục mang tên của chính chúng; nếu không thể tạo thư mục con, tệp sẽ được ghi vào thư mục gốc của các bản ghi thay vì bị mất.
* Các tệp TIFF ghi hình được nén không mất dữ liệu (DEFLATE) theo mặc định và chứa đầy đủ siêu dữ liệu hiệu chuẩn và xử lý **bên trong tệp XMP** — các bản chụp tự mô tả, không có tệp phụ nào khác ngoài tệp đọc `.daq` (và tệp `.daq.cameras.json` của nó).
* **Mặt nạ giới hạn (mới trong phiên bản 1.3.0).**Trong trường hợp quá trình tách phổ 3×3 phải giới hạn một dải phổ xuống 0 trên ít nhất một pixel, các sản phẩm về độ rạng rỡ, độ phản xạ và chỉ số sẽ được áp dụng một mặt nạ cho từng pixel, `clamp_masks/<product folder>/<product name>_clampmask.tif` (bit 0 = dải trong kênh R của sản phẩm, bit 1 = G, bit 2 = B); thẻ XMP `Chloros:UnmixClampMask` của sản phẩm trỏ đến mặt nạ này. Các mặt nạ được lưu trữ**bên cạnh** các thư mục sản phẩm, không bao giờ nằm bên trong thư mục nào, do đó một thư mục sản phẩm như `reflectance/` có thể được chuyển trực tiếp cho phần mềm photogrammetry mà không cần chỉnh sửa. Công cụ nhập liệu của Chloros

sẽ bỏ qua các mặt nạ này.
* **Hướng (1.3.0).** Các sản phẩm Preview, Radiance, Reflectance và Index của máy ảnh độc lập được xoay theo cài đặt Gương/Xoay; các tệp Raw và Debayered giữ nguyên như cách cảm biến đọc. Tất cả các tệp thành viên trong mảng đều giữ nguyên định dạng gốc. Mỗi tệp đều ghi lại cài đặt này trong XMP của nó, do đó quá trình xử lý lại sẽ áp dụng cài đặt đó theo cách tương tự.
* **Máy ảnh nào sở hữu tệp `.daq` (1.3.0).** `.daq` của một lần chụp chỉ được khớp, tại thời điểm xử lý, với các hình ảnh từ các máy ảnh mà `.daq.cameras.json` của nó chỉ định. Do đó, một máy ảnh độc lập không có cảm biến ánh sáng riêng sẽ không còn mượn dữ liệu từ máy ảnh khác — hãy gán một cảm biến cho nó hoặc ghi lại một cảm biến trong tab Cảm biến Ánh sáng. Một bản ghi cảm biến ánh sáng (và bất kỳ tệp `.daq` nào không có tệp đó, bao gồm cả các bản chụp cũ hơn) vẫn phục vụ cho mọi camera.

Đây là cùng một cấu trúc mà `chloros-cli lattice capture` / `array-capture` ghi vào thư mục `-o` của chúng — được ghi chép trong [Tham chiếu CLI

§ Cấu trúc của thư mục bản ghi](../reference/cli-reference.md#what-a-captures-folder-looks-like).

<!-- SCREENSHOT-NEEDED: OS file explorer showing a real <project>/captures/ folder after a multi-level array capture — the raw/debayered/radiance/reflectance/preview subfolders, the daq/ and clamp_masks/ folders, and sync_<ts>_SN<serial>_<level>.tif filenames visible inside one subfolder. -->

## Xử lý lại các bản ghi thành sản phẩm đã hiệu chuẩn

Các khung hình thô đã ghi cùng với tệp `.daq` đã lưu là tất cả những gì quy trình xử lý cần — đây là lý do tại sao chế độ “Ghi nhanh nhất” (Fastest Capture) an toàn để sử dụng trong công việc thực tế.

* **GUI**: thêm thư mục ghi hình vào dự án ([Thêm tệp vào dự án](../processing-images-gui/adding-files-to-a-project.md)) và xử lý như bình thường.
* **CLI**: chỉ định `process` đến **thư mục gốc ghi hình**:

```bash
chloros-cli process "C:/ChlorosProjects/MyField/captures"
```

`process` thông thường chỉ nhập thư mục bạn chỉ định, nhưng khi thư mục đó không chứa hình ảnh và có các thư mục con, nó sẽ tự động đi sâu vào các thư mục con — do đó, các thư mục con cùng cấp và thư mục `daq/` sẽ được nhập cùng một lúc. Mỗi lần chụp sẽ được nhập dưới dạng **một hình ảnh duy nhất** với các cấp độ khác được đính kèm dưới dạng chế độ xem, chứ không phải một hình ảnh cho mỗi cấp độ.

Việc đặt tên trực tiếp cho thư mục con của cấp độ (ví dụ: `…/captures/raw/`) cũng hoạt động, nhưng sẽ bỏ qua thư mục `daq/` — hãy sao chép nó cùng lúc khi bạn-tạo sản phẩm đo xạ từ `raw/`, nếu không thì việc khớp dấu thời gian sẽ không có đối tượng nào để giải quyết.

{% hint style="warning" %}
**Quá trình xử lý luôn bắt đầu từ `raw`.** Trong mỗi lần thu thập dữ liệu, khung hình thô là nguồn của quy trình xử lý; `debayered`, `radiance`, `reflectance`, và `preview` xuất hiện dưới dạng các chế độ hiển thị nhưng không bao giờ được đưa trở lại qua đường ống — việc xử lý lại một sản phẩm dẫn xuất sẽ áp dụng lại các hiệu ứng vignette, màu sắc và toán học về độ sáng đã được tích hợp sẵn vào các pixel của nó, do đó Chloros

s từ chối thay vì xử lý hai lần. Các bản render `index/` và `composite/` hoàn toàn không được xử lý (chúng là đầu ra, không phải dữ liệu thu thập). Một thư mục ghi lại được lưu **mà không** có dữ liệu thô nhập vào và hiển thị bình thường, nhưng `process` sẽ bỏ qua nó và thông báo điều đó; `--input-level {raw,debayered,processed}` là lối thoát có chủ đích buộc phải có điểm vào. Xem [Tham chiếu CLI

](../reference/cli-reference.md#what-a-captures-folder-looks-like) để biết chính xác các thông báo bỏ qua.
{% endhint %}

Hai hành vi khác cần lưu ý khi viết kịch bản xử lý lại:

* Một lần chạy `chloros-cli process` yêu cầu sản phẩm nhưng **không tạo ra bất kỳ sản phẩm hình ảnh nào** sẽ báo lỗi rõ ràng và thoát với giá trị không bằng 0 — bạn sẽ không bao giờ nhận được kết quả chạy trống im lặng. Các lần chạy thành công sẽ báo cáo số lượng sản phẩm. (Một lần chạy chỉ chứa metadata theo thiết kế vẫn được coi là thành công.)
* Các tệp xuất đã xử lý được nhập lại sẽ không bao giờ chiếm khe dữ liệu thô của bản chụp — dữ liệu thô gốc luôn là nguồn của quy trình xử lý.

## Các lệnh tương đương trên giao diện dòng lệnh (CLI

)

Tất cả các thao tác trên trang này đều có thể thực hiện ở chế độ không giao diện đồ họa (headless). Các chế độ chụp trên giao diện đồ họa (GUI) tương ứng trực tiếp với các lệnh `chloros-cli lattice array-capture`:

| Giao diện đồ họa (GUI) | Giao diện dòng lệnh (CLI

) |
| --- | --- |
| Đơn lẻ | `chloros-cli lattice array-capture` |
| Liên tục | `array-capture --continuous [--count N] [--duration S]` |
| Theo khoảng thời gian | `array-capture --interval S [--duration S]` |
| Ghi nhanh nhất | `array-capture --fastest` |
| Hộp kiểm căn chỉnh | `--aligned / --no-aligned` |
| Các ô chọn loại xuất | `--processing LEVEL` hoặc `--levels L1,L2,…` (mặc định là `all`) |
| Ghi video chỉ mục | `chloros-cli lattice array-record` |
| Ghi dữ liệu thô liên tục / Tạo video | `chloros-cli lattice array-burst` / `array-build-video` |

Bảng cờ đầy đủ, tùy chọn chụp ổn định smart-AE (`--smart`) và mô hình tốc độ duy trì được mô tả trong [Tham khảo CLI

§ Chế độ chụp, Máy ghi và Xử lý lại ngoại tuyến](../reference/cli-reference.md#capture-modes-recorders--offline-reprocess).
