# Máy ảnh đơn sắc & Chỉ số thảm thực vật

## Một máy ảnh = một dải phổ

Máy ảnh **M3M**là phiên bản đơn sắc của mẫu**M3C** sử dụng cảm biến Bayer: một cảm biến đơn sắc IMX265 được trang bị một bộ lọc nhiễu dải hẹp duy nhất. Dãy ký tự mô hình chỉ định dải phổ — ví dụ: `M3M-<lens>-F<wavelength>`, `M3M-L87-F685` (hiển thị trong Chloros dưới dạng `LATT-M3M-L87-F685`). Cảm biến này cung cấp **một dải màu xám duy nhất** mà không có mô-zaic Bayer: không có gì để khử mô-zaic, không có nhiễu chéo giữa các kênh cần tách ra, và không cần thiết lập cân bằng trắng.

Những điều cần lưu ý trước khi lập kế hoạch cho hệ thống đơn sắc:

* **Độ rạng rỡ và độ phản xạ được định nghĩa đầy đủ cho từng dải.**Đây là các bản đồ phóng xạ theo từng dải, do đó một máy ảnh M3M tạo ra độ rạng rỡ đã hiệu chuẩn ở định dạng float32 (W/m²/sr/nm) và độ phản xạ ở định dạng uint16 (`32768` = ρ 1.0) chính xác như một dải của máy ảnh M3C. Các khung hình đơn sắc mang ma trận phản hồi cảm biến**bản sắc** — không cần hoặc không áp dụng phép tách 3×3.
* **Một camera đơn sắc duy nhất không thể tạo ra chỉ số thực vật.** NDVI, NDRE và các chỉ số tương tự cần ít nhất hai dải. Để tính toán các chỉ số từ phần cứng đơn sắc, bạn cần kết hợp nhiều camera M3M — xem bên dưới.
* Các camera M3M truyền dữ liệu **Mono12** (12-bit: 2 byte/pixel trên đường truyền, hoặc 1,5 byte/pixel khi mảng sử dụng [chuyển tải 12-bit đóng gói](arrays.md#packed-12-bit-transport)), điều này quan trọng đối với [việc lập ngân sách băng thông cho mảng](arrays.md#bandwidth-how-many-cameras-fit).

## Những gì Chloros bỏ qua đối với chế độ mono — và cách nó thông báo cho bạn

Các giai đoạn trong quy trình xử lý màu đơn giản là không áp dụng cho cảm biến đơn dải. Chloros **bỏ qua chúng kèm theo một thông báo ngắn gọn** thay vì báo lỗi, và vẫn thực thi chúng bình thường cho bất kỳ máy ảnh M3C (Bayer) nào trong cùng phiên làm việc:

| Giai đoạn | Hành vi Mono (M3M) | Hành vi M3C |
| --- | --- | --- |
| Demosaic / debayer | Bỏ qua — mức xuất `debayered` là hình ảnh xám 1 kênh. | Demosaic 3 kênh. |
| Cân bằng trắng (`lattice white-balance`) | Bỏ qua kèm theo một thông báo một dòng. | Chạy bình thường. |
| Hồ sơ màu (`lattice color-profile`) | Bỏ qua kèm theo một thông báo một dòng. | Chạy bình thường. |
| Độ bão hòa/độ tương phản (`lattice color`) | Bỏ qua kèm theo một thông báo một dòng. | Chạy bình thường. |
| Tách nhiễu phổ | Giá trị mặc định (không có ma trận 3×3). | Áp dụng ma trận 3×3 cho từng camera. |
| Độ sáng / độ phản xạ | **Hoạt động** — theo từng dải, hiệu chuẩn đầy đủ. | Hoạt động theo từng dải. |

Giao diện người dùng (GUI) áp dụng cùng cơ chế lọc: đối với camera đơn sắc, bảng cài đặt cho từng camera ẩn các hàng chỉ dành cho RGB (Cân bằng trắng, Gamma, Hồ sơ màu, Độ bão hòa, Độ tương phản, chia kênh), và biểu đồ histogram trực tiếp bị khóa vào một đường **MONO** duy nhất. Yếu tố phân biệt trong toàn bộ quá trình xếp chồng là token `M3M` trong chuỗi mô hình, được hiển thị trên GUI/SDK dưới dạng `is_mono`.

## Chỉ số yêu cầu ≥ 2 dải: căn chỉnh → xếp chồng → chỉ số

Quy trình làm việc với chỉ số mono luôn bao gồm ba bước sau:

1. **Căn chỉnh** — hướng nhiều camera M3M vào các bước sóng khác nhau (ví dụ: một camera F650 “Red” và một camera F850 “NIR”), kết nối chúng thành một [mảng đa camera](arrays.md), và để Chloros tính toán biến dạng đồng bộ hóa giữa các camera.
2. **Ghép chồng** — các khung hình đã được căn chỉnh sẽ trở thành một hình ảnh đa dải (mỗi camera đóng góp một dải có tên riêng).
3. **Chỉ số** — tính toán công thức chỉ số trên các dải phổ của hình ảnh ghép, tùy chọn hiển thị thông qua bảng tra cứu (LUT).

Trong giao diện người dùng (GUI), toàn bộ chuỗi này là chế độ hiển thị mảng **Combined Cameras**: hình ảnh tổng hợp trực tiếp đã được căn chỉnh sẵn, và công cụ Tính toán Chỉ số của mảng (bên dưới) xác định công thức được hiển thị. Các tệp xuất đã chụp có thể được điều chỉnh biến dạng để có cùng độ căn chỉnh bằng tùy chọn chụp**Aligned**.

## Trình tính toán chỉ số

Trình tính toán chỉ số tạo ra biểu thức chỉ số được sử dụng bởi chế độ xem trực tiếp và các tệp xuất chỉ số cho từng camera. Đây là một giao diện chung, có thể mở từ hai vị trí trong thanh bên của tab Cameras:

* **Theo từng camera**— Xem trước trực tiếp → biểu tượng bánh răng**Chỉ số** (chỉ dành cho camera Bayer RGN/OCN/NGB; một camera đơn sắc không có điều khiển chỉ số vì một dải phổ không thể tạo thành chỉ số).
* **Theo mảng**— Cài đặt mảng → Xem trước trực tiếp → biểu tượng bánh răng**Chỉ số**. Đây là đường dẫn dành cho máy ảnh đơn sắc: danh sách dải sóng bao gồm**tất cả các máy ảnh thành viên**, do đó một cặp máy ảnh đơn sắc sẽ đóng góp hai dải sóng của mình vào đây.

Các điều khiển<!-- SCREENSHOT-NEEDED: Index Calculator pane opened for a combined array of two mono cameras (e.g. F650 + F850): band chips row showing the two bands with wavelength labels, the operator buttons, the expression textarea containing "(NIR - Red) / (NIR + Red)", the green "Valid expression" banner, the LUT controls (Apply LUT checked, Level 7-stop, Min 0.2 / Max 1), and the live histogram with p2/p98 percentile lines. -->

của nó, từ trên xuống dưới:

* **Chip dải** ("Dải — nhấp để thêm vào biểu thức") — một nút cho mỗi dải có sẵn, được gắn nhãn tên màu + bước sóng tính bằng nm (các tên màu trùng lặp được phân biệt rõ ràng, ví dụ: "Màu 850"). Nhấp vào sẽ chèn mã dải tại vị trí con trỏ. Các dải phổ từ các máy ảnh không thể tạo ra độ sáng theo từng dải (RGB/FRGB) sẽ bị lọc bỏ.
* **Nút toán tử và hàm** — `+ - * / ( ) ^ ,` và `abs() sqrt() log() log10() exp() min() max() pow()`.
* **Khu vực nhập biểu thức** — công thức nhập tự do; ô giữ chỗ hiển thị dạng NDVI cổ điển `(NIR - Red) / (NIR + Red)`. Một bản xem trước chỉ đọc được phân tách thành các token phía trên sẽ hiển thị các chip dải, số và các cờ đánh dấu các token không xác định.
* **Dải thông báo hợp lệ**— màu xám “Trống — không áp dụng chỉ số nào”; màu xanh lá “Biểu thức hợp lệ”; màu đỏ kèm lỗi phân tích cụ thể (dải không xác định, dải mâu thuẫn do nhiều camera ghi lại, thiếu dấu ngoặc, …); hoặc màu cam khi biểu thức hợp lệ nhưng**hằng số** (ví dụ: `X/X`, hoặc mẫu số NDVI được nhập là `−` thay vì `+`) — hằng số sẽ ánh xạ toàn bộ khung hình thành một màu duy nhất.
* Một cảnh báo màu hổ phách riêng biệt sẽ xuất hiện nếu biểu thức được áp dụng là đúng nhưng **khung hình trực tiếp đồng nhất** (cảnh phẳng hoặc bão hòa) — sự sụp đổ biểu đồ tần số được phát hiện cho bạn.
* **Áp dụng LUT**(mặc định bật; tắt = kéo dãn thang xám),**Mức**2/3/5/7 bước (mặc định 7 bước), và các trường nhập**Tối thiểu / Tối đa**nằm hai bên thanh gradient. Giá trị mặc định của Min là**0,2**— nó thu nhỏ dải màu vào phạm vi liên quan đến thảm thực vật trong khi các giá trị dưới mức này sẽ được chuyển thành thang xám; đặt Min thành −1 để có phạm vi chỉ số đầy đủ (nút**Reset** khôi phục phạm vi −1…+1). Giá trị mặc định của Max là 1.
* **Biểu đồ histogram trực tiếp** của phân phối chỉ số — các thanh được quy mô theo căn bậc hai, các đường phân vị p2/p98 màu hổ phách, đường trung vị màu trắng và các chỉ báo đuôi ngoài phạm vi ("◀ N% < lo" / "hi < N% ▶") sẽ chuyển sang màu hổ phách khi vượt quá 1% như tín hiệu để mở rộng khoảng Min/Max.
* **Áp dụng** sẽ áp dụng biểu thức vào luồng trực tiếp; các điều chỉnh LUT được áp dụng ngay lập tức mà không cần nhấn Áp dụng. Khi có dự án đang mở, biểu thức và cài đặt LUT sẽ được lưu cùng với dự án và khôi phục khi bạn mở lại dự án; nếu không có dự án, chúng chỉ tồn tại trong phiên làm việc hiện tại.

<!-- SCREENSHOT-NEEDED: Combined-array live tile rendering NDVI from a mono pair through the default 7-stop LUT, with the array name pill and fps readout visible — the result of applying the expression from the previous screenshot. -->



## Đường dẫn CLI



Cùng chuỗi quy trình align → stack → index, có thể lập trình từ đầu đến cuối:

```bash
chloros-cli lattice align-calibrate --serials SN_RED,SN_NIR --save align.json
chloros-cli lattice index --live --profile align.json \
  --preset NDVI --channel red=Red_660 --channel nir=NIR_850 \
  --save-multiband -o output/
```

`align-calibrate` sử dụng một mảng kết nối chứa các camera của nó, hoặc tự mở chúng cho lần chạy đó. `index --live` luôn tự mở các camera (một [mảng tạm thời](../reference/cli-reference.md#multi-camera-transient-sessions)) và không có `--serials`: nó mở mọi camera trên mạng. Vì vậy, hãy giải phóng mọi camera mà Chloros đang giữ trước đó — GUI, `cam-connect`, `array-connect` (`chloros-cli lattice array-disconnect --all`): việc mở lại một camera đã được giữ lần thứ hai có thể làm dừng luồng dữ liệu của người giữ hoặc gây treo hệ thống backend. Một camera không gửi khung hình nào sẽ khiến quá trình chạy thất bại, và không có stack hay chỉ mục nào được ghi lại.

`--channel` ánh xạ các ký hiệu của preset sang tên dải tần của stack. Hai quy tắc giúp bạn tránh thất bại trong lần chạy:

* **Các ký hiệu phân biệt chữ hoa chữ thường** và phải khớp chính xác với tên kênh của cài đặt sẵn — các cài đặt sẵn sử dụng chữ thường (các cài đặt sẵn NDVI là `red`, `nir`; xem `--list-presets`). `--channel red=Red_660` hoạt động bình thường; `--channel RED=660` sẽ thất bại với lỗi `channel_map missing entries`.
* Phía dải phải đặt tên cho một dải trong chồng ảnh đã căn chỉnh (`lattice align-info --profile align.json` liệt kê chúng). Chế độ ngoại tuyến cũng chấp nhận các chỉ số dải bắt đầu từ 0, ví dụ: `--channel red=0 --channel nir=1`.

`lattice index` cũng chạy ngoại tuyến trên tệp TIFF đa dải đã được căn chỉnh và lưu lại. Mọi biểu mẫu đều chạy trên nền tảng, do đó nền tảng phải đang hoạt động và đã đăng nhập vào gói Chloros+ trả phí; một `--input` hoặc `--output` tương đối sẽ được giải quyết dựa trên thư mục mà bạn chạy lệnh từ đó:

```bash
chloros-cli lattice index --input aligned.tif --preset NDVI \
  --output ndvi.tif --colorize --gradient RdYlGn
```

### Cài đặt sẵn chỉ mục

`lattice index --preset` (và [Hộp cát chỉ mục/LUT](../image-viewer-gui/index-lut-sandbox.md) trong tab Hình ảnh, sử dụng cùng một công cụ) cung cấp **22 cài đặt sẵn** sau:

`NDVI, GNDVI, BNDVI, NDRE, ENDVI, SAVI, OSAVI, MSAVI, EVI, EVI2, CVI, MSR, TDVI, LAI, GLI, NGRDI, VARI, TGI, EXG, CIRE, CIGREEN, NDWI`

Chạy lệnh `chloros-cli lattice index --list-presets` để xem công thức và ký hiệu kênh của từng cài đặt sẵn, và `--list-gradients` để xem các dải màu có sẵn. Các công thức tùy chỉnh sử dụng lệnh `--formula EXPR` với cú pháp tương tự như Trình tính toán chỉ số. Lưu ý rằng danh sách cài đặt sẵn này dành riêng cho công cụ chỉ số LATTICE — danh sách trong menu thả xuống “Xử lý” của Cài đặt Dự án dành cho hình ảnh nhập vào là một danh sách khác (xem [Công thức chỉ số đa phổ](../project-settings/multispectral-index-formulas.md)).

Bộ cờ đầy đủ (`--output-format`, `--vmin/--vmax/--percentile`, `--bg-mode`, các núm điều chỉnh biến dạng căn chỉnh cho `--live`, v.v.) được ghi chép trong [Tài liệu tham khảo CLI § Toán học Chỉ số / Thực vật](../reference/cli-reference.md#index--vegetation-maths); các hàm tương đương trong SDK có trong [SDK Reference](../reference/sdk-reference.md).

## Trích xuất các sản phẩm chỉ số từ một mảng đơn sắc

Khi đã kết nối mảng và áp dụng biểu thức chỉ số, `array-capture` (hoặc tùy chọn GUI **Capture All**) sẽ lưu các mức xuất cho từng camera *và* kết quả hiển thị chỉ số — `--index`/`--no-index` cho phép bật/tắt tính năng này trên CLI, và mặc định sẽ thu thập mọi mức áp dụng được. Đóng góp của một camera đơn vào mỗi nhóm ghi lại là dải duy nhất của nó ở các mức raw/debayered (thang xám)/radiance/reflectance, cộng với hình ảnh tổng hợp chỉ số kết hợp chung khi mảng hoạt động ở chế độ kết hợp. Xem [Mảng nhiều camera § Ghi hình](arrays.md#capturing-monitoring-vs-analysis).
