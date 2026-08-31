# Máy ảnh đơn sắc và các chỉ số thảm thực vật

## Một máy ảnh = một dải sóng

Máy ảnh **M3M**là phiên bản đơn sắc của mẫu**M3C** sử dụng cảm biến Bayer: một cảm biến đơn sắc IMX265 được trang bị một bộ lọc nhiễu dải hẹp duy nhất. Dãy ký tự của mẫu máy ảnh chỉ định dải phổ — ví dụ: `M3M-<lens>-F<wavelength>`, `M3M-L87-F685` (được hiển thị trongChloros

dưới dạng `LATT-M3M-L87-F685`). Cảm biến này cung cấp **một dải màu xám duy nhất** không có mô-zaic Bayer: không có gì để khử mô-zaic, không có nhiễu xuyên kênh cần tách ra và không cần thiết lập cân bằng trắng.

Những điều cần biết trước khi bạn lên kế hoạch cho một hệ thống đơn sắc:

* **Độ rạng rỡ và độ phản xạ được định nghĩa đầy đủ cho từng dải.**Đây là các bản đồ phóng xạ theo từng dải, do đó một camera M3M tạo ra độ sáng đã hiệu chuẩn ở định dạng float32 (W/m²/sr/nm) và độ phản xạ ở định dạng uint16 (`32768` = ρ 1.0), hoàn toàn giống như một dải của camera M3C. Các khung hình đơn sắc mang ma trận phản ứng cảm biến**đồng nhất** — không cần hoặc không áp dụng phép tách 3×3.
* **Một camera đơn sắc duy nhất không thể tạo ra chỉ số thực vật.** Chỉ số đa phổ (NDVI

), chỉ số bù độ mờ (NDRE

) và các chỉ số tương tự cần ít nhất hai dải phổ. Để tính toán các chỉ số từ phần cứng đơn sắc, bạn cần kết hợp nhiều camera M3M — xem phần dưới đây.
* Các camera M3M truyền dữ liệu **Mono12** (12-bit, 2 byte/pixel trên đường truyền), điều này rất quan trọng đối với [quản lý băng thông mảng](arrays.md#bandwidth-the-rules-of-thumb).

## Những gìChloros

bỏ qua đối với cảm biến đơn sắc — và cách nó thông báo cho bạn

Các giai đoạn trong quy trình xử lý màu đơn giản là không áp dụng cho cảm biến đơn dải.Chloros

**bỏ qua chúng bằng một thông báo một dòng** thay vì báo lỗi, và vẫn chạy chúng bình thường cho bất kỳ camera M3C (Bayer) nào trong cùng phiên làm việc:

| Giai đoạn | Hành vi trên máy ảnh đơn kênh (M3M) | Hành vi trên máy ảnh M3C |
| --- | --- | --- |
| Demosaic / debayer | Bỏ qua — tệp xuất `debayered` là hình ảnh xám một kênh. | Demosaic 3 kênh. |
| Cân bằng trắng (`lattice white-balance`) | Bỏ qua kèm theo một thông báo ngắn. | Chạy bình thường. |
| Hồ sơ màu (`lattice color-profile`) | Bỏ qua kèm theo một thông báo ngắn. | Chạy bình thường. |
| Độ bão hòa/độ tương phản (`lattice color`) | Bỏ qua kèm theo một thông báo ngắn. | Hoạt động bình thường. |
| Tách nhiễu phổ | Giá trị mặc định (không có ma trận 3×3). | Áp dụng ma trận 3×3 cho từng camera. |
| Độ sáng / độ phản xạ | **Hoạt động** — theo từng dải, được hiệu chuẩn đầy đủ. | Hoạt động theo từng dải. |

Giao diện người dùng (GUI) áp dụng cùng cơ chế lọc: đối với camera đơn sắc, bảng cài đặt cho từng camera ẩn các hàng chỉ dành cho chế độ đa kênh (RGB

) (Cân bằng trắng, Gamma, Hồ sơ màu, Độ bão hòa, Độ tương phản, chia kênh), và biểu đồ histogram trực tiếp bị khóa vào một đường **MONO** duy nhất. Yếu tố phân biệt trong toàn bộ quá trình xếp chồng là mã `M3M` trong chuỗi mô hình, được hiển thị trên GUI/SDK

dưới dạng `is_mono`.

## Chỉ số yêu cầu ≥ 2 dải: căn chỉnh → xếp chồng → chỉ số

Quy trình làm việc với chỉ số mono luôn bao gồm ba bước sau:

1. **Căn chỉnh** — hướng nhiều camera M3M vào các bước sóng khác nhau (ví dụ: một camera F650 “Red

” và một camera F850 “NIR

”), kết nối chúng thành một [mảng camera đa kênh](arrays.md), và đểChloros

tính toán biến dạng đồng bộ hóa giữa các camera.
2. **Ghép chồng** — các khung hình đã được căn chỉnh sẽ trở thành một hình ảnh đa dải (mỗi camera đóng góp một dải có tên riêng).
3. **Chỉ số** — đánh giá công thức chỉ số trên các dải của hình ảnh ghép, tùy chọn hiển thị thông qua bảng tra cứu (LUT).

Trong giao diện người dùng (GUI), toàn bộ chuỗi này là chế độ hiển thị mảng **Combined Cameras**: hình ảnh tổng hợp trực tiếp đã được căn chỉnh sẵn, và Bộ tính toán chỉ số của mảng (bên dưới) xác định công thức mà nó hiển thị. Các tệp xuất đã chụp có thể được điều chỉnh biến dạng để đạt cùng độ căn chỉnh bằng tùy chọn chụp**Aligned**.

## Trình tính toán chỉ số

Trình tính toán chỉ số tạo ra biểu thức chỉ số được sử dụng cho chế độ xem trực tiếp và các tệp xuất chỉ số theo từng camera. Đây là một giao diện chung, có thể mở từ hai vị trí trong thanh bên của tab Cameras:

* **Theo từng camera**— Xem trước trực tiếp → biểu tượng bánh răng**Chỉ số** (chỉ dành cho camera Bayer:RGN

/OCN

/NGB

; camera đơn sắc không có điều khiển chỉ số vì một dải phổ không thể tạo thành chỉ số).
* **Theo mảng**— cài đặt mảng → Xem trước trực tiếp → bánh răng**Chỉ số**. Đây là đường dẫn dành cho máy ảnh đơn sắc: danh sách dải bao gồm**tất cả các máy ảnh thành viên**, do đó một cặp máy ảnh đơn sắc sẽ đóng góp hai dải của mình vào đây.

<!-- SCREENSHOT-NEEDED: Index Calculator pane opened for a combined array of two mono cameras (e.g. F650 + F850): band chips row showing the two bands with wavelength labels, the operator buttons, the expression textarea containing "(NIR - Red) / (NIR + Red)", the green "Valid expression" banner, the LUT controls (Apply LUT checked, Level 7-stop, Min 0.2 / Max 1), and the live histogram with p2/p98 percentile lines. -->

Các điều khiển, từ trên xuống dưới:

* **Chip dải** (“Dải — nhấp để thêm vào biểu thức”) — một nút cho mỗi dải có sẵn, được gắn nhãn tên màu + bước sóng tính bằng nm (các tên màu trùng lặp được phân biệt rõ ràng, ví dụ: “Màu 850”). Nhấp vào sẽ chèn mã dải vào vị trí con trỏ. Các dải phổ từ các máy ảnh không thể tạo ra độ rạng rỡ theo từng dải phổ (RGB

/FRGB) sẽ bị lọc bỏ.
* **Nút toán tử và hàm** — `+ - * / ( ) ^ ,` và `abs() sqrt() log() log10() exp() min() max() pow()`.
* **Khu vực nhập biểu thức** — công thức nhập tự do; ký hiệu giữ chỗ hiển thị dạng cổ điểnNDVI

dưới dạng `(NIR - Red) / (NIR + Red)`. Một bản xem trước được phân tích thành các token chỉ đọc phía trên sẽ hiển thị các dải phổ dưới dạng ô, số và các cờ biểu thị các token không xác định.
* **Biểu ngữ hợp lệ**— màu xám “Trống — không áp dụng chỉ số nào”; màu xanh lá “Biểu thức hợp lệ”; màu đỏ kèm lỗi phân tích cụ thể (dải tần không xác định, dải tần mơ hồ do nhiều camera ghi nhận, thiếu dấu ngoặc đơn, …); hoặc màu hổ phách khi biểu thức hợp lệ nhưng**hằng số** (ví dụ: `X/X`, hoặc mẫu sốNDVI

được nhập là `−` thay vì `+`) — hằng số sẽ ánh xạ toàn bộ khung hình thành một màu duy nhất.
* Một cảnh báo màu hổ phách riêng biệt sẽ xuất hiện nếu biểu thức được áp dụng là đúng nhưng **khung hình trực tiếp đồng nhất** (cảnh phẳng hoặc bão hòa) — sự sụp đổ biểu đồ tần suất được phát hiện cho bạn.
* **Áp dụng LUT**(mặc định bật; tắt = kéo giãn thang xám),**Mức**2/3/5/7-stop (mặc định 7-stop), và các trường nhập**Min / Max**nằm hai bên thanh gradient. Giá trị mặc định của Min là**0,2**— nó thu nhỏ dải màu vào phạm vi liên quan đến thảm thực vật, trong khi các giá trị dưới mức này sẽ được chuyển thành thang xám; đặt Min thành −1 để có phạm vi chỉ số đầy đủ (nút**Reset** khôi phục phạm vi từ −1 đến +1). Giá trị mặc định của Max là 1.
* **Biểu đồ histogram trực tiếp** của phân phối chỉ số — các thanh được quy chuẩn theo căn bậc hai, các đường phân vị p2/p98 màu hổ phách, đường trung vị màu trắng và các chỉ báo đuôi ngoài phạm vi (&quot;◀ N% &lt; lo&quot; / &quot;hi &lt; N% ▶&quot;) sẽ chuyển sang màu hổ phách khi vượt quá 1% như tín hiệu để mở rộng khoảng Min/Max.
* **Áp dụng**sẽ áp dụng biểu thức vào luồng trực tiếp; các điều chỉnh LUT được áp dụng ngay lập tức mà không cần nhấn Áp dụng. Các biểu thức được thiết kế là**chỉ dành cho phiên làm việc** — chúng không được lưu giữ giữa các phiên.



<!-- SCREENSHOT-NEEDED: Combined-array live tile rendering NDVI from a mono pair through the default 7-stop LUT, with the array name pill and fps readout visible — the result of applying the expression from the previous screenshot. -->

## Quy trình &quot;CLI

&quot;

Cùng một chuỗi quy trình align → stack → index, có thể lập trình từ đầu đến cuối:

```bash
chloros-cli lattice array-connect --serials SN_RED,SN_NIR
chloros-cli lattice index --live --profile align.json \
  --preset NDVI --channel red=Red_660 --channel nir=NIR_850 \
  --save-multiband -o output/
```

`--channel` ánh xạ các ký hiệu của preset sang tên dải tần của stack. Hai quy tắc giúp bạn tránh được lỗi khi chạy:

* **Các ký hiệu phân biệt chữ hoa chữ thường** và phải khớp chính xác với tên kênh của preset — các preset sử dụng chữ thường (củaNDVI

là `red`,`nir`; kiểm tra `--list-presets`). `--channel red=Red_660` hoạt động; `--channel RED=660` sẽ thất bại với lỗi `channel_map missing entries`.
* Phía dải tần phải chỉ định tên một dải tần trong ngăn xếp đã căn chỉnh (`lattice align-info --profile align.json` liệt kê các dải tần này). Chế độ ngoại tuyến cũng chấp nhận chỉ số dải tần bắt đầu từ 0, ví dụ: `--channel red=0 --channel nir=1`.

`lattice index` cũng chạy hoàn toàn ngoại tuyến với tệpTIFF

đa dải đã được căn chỉnh và lưu lại:

```bash
chloros-cli lattice index --input aligned.tif --preset NDVI \
  --output ndvi.tif --colorize --gradient RdYlGn
```

### Cài đặt sẵn chỉ số

`lattice index --preset` (và [Index/LUT sandbox](../image-viewer-gui/index-lut-sandbox.md) trong tab Hình ảnh, sử dụng cùng một bộ xử lý) cung cấp **22 cài đặt sẵn** sau:

`NDVI, GNDVI, BNDVI, NDRE, ENDVI, SAVI, OSAVI, MSAVI, EVI, EVI2, CVI, MSR, TDVI, LAI, GLI, NGRDI, VARI, TGI, EXG, CIRE, CIGREEN, NDWI`

Chạy `chloros-cli lattice index --list-presets` để xem công thức và ký hiệu kênh của từng cài đặt sẵn, và `--list-gradients` để xem các dải màu có sẵn. Các công thức tùy chỉnh sử dụng `--formula EXPR` với cú pháp tương tự như Trình tính toán Chỉ số. Lưu ý rằng danh sách cài đặt sẵn này dành riêng cho công cụ chỉ số LATTICE — danh sách trong menu thả xuống &quot;Xử lý&quot; của Cài đặt Dự án dành cho hình ảnh nhập vào là một danh sách khác (xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md)).

Bộ cờ đầy đủ (`--output-format`, `--vmin/--vmax/--percentile`, `--bg-mode`, các núm điều chỉnh biến dạng căn chỉnh cho `--live`, v.v.) được ghi chép trong [Tài liệu tham khảoCLI

§ Toán học chỉ số / Thực vật](../reference/cli-reference.md#index--vegetation-maths); cácSDK

các giá trị tương đương có trong [SDK

Reference](../reference/sdk-reference.md).

## Trích xuất các sản phẩm chỉ số từ một mảng đơn

Khi mảng đã được kết nối và biểu thức chỉ số được áp dụng, `array-capture` (hoặc tùy chọn GUI **Capture All**) sẽ lưu các mức xuất cho từng camera *và* kết quả hiển thị chỉ số — `--index`/`--no-index` cho phép bật/tắt tính năng này trênCLI

, và mặc định sẽ thu thập mọi mức áp dụng được. Đóng góp của một camera đơn vào mỗi nhóm ghi hình là một dải của nó ở các mức thô/đã loại bỏ hiệu ứng debayering (thang xám)/cường độ bức xạ/độ phản xạ, cộng với hình ảnh tổng hợp chỉ số kết hợp chung khi mảng hoạt động ở chế độ kết hợp. Xem [Mảng Nhiều Camera § Ghi hình](arrays.md#capturing-monitoring-vs-analysis).
