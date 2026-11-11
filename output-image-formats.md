---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/output-image-formats
---

# Định dạng hình ảnh đầu ra

Chloros xuất các sản phẩm đã xử lý dưới bốn định dạng tệp. Chọn định dạng trong Cài đặt Dự án (GUI), bằng lệnh `--format` (CLI) hoặc bằng lệnh `export_format` (SDK). CLI và SDK chấp nhận các chuỗi chính xác như dưới đây.

| Chuỗi định dạng | Phần mở rộng | Loại pixel | Phạm vi pixel | Ghi chú |
| --- | --- | --- | --- | --- |
| `TIFF (16-bit)` *(mặc định)* | `.tif` | số nguyên 16 bit | 0 – 65535 | Được khuyến nghị cho photogrammetry / GIS. |
| `TIFF (8-bit)` | `.tif` | số nguyên 8 bit | 0 – 255 | Dành cho các lần chụp toàn 8 bit, chỉ hỗ trợ các định dạng 8 bit: Chloros không nâng cấp độ sâu bit. |
| `TIFF (32-bit, Percent)` | `.tif` | float32 | 0,0 – 1,0 | 1,0 = độ phản xạ 100%. Một số ứng dụng không thể đọc các tệp TIFF dạng số thập phân; các tệp này có dung lượng lớn hơn. |
| `PNG (8-bit)` | `.png` | số nguyên 8-bit (uint8) | 0 – 255 | Nén không mất dữ liệu, phù hợp để xem trên web và hiển thị. |
| `JPG (8-bit)` | `.jpg` | số nguyên 8 bit (uint8) | 0 – 255 | Nén có mất dữ liệu, tệp có kích thước nhỏ nhất. |

## Vị trí lưu trữ tệp đầu ra

Các tệp đầu ra được lưu trong thư mục dự án, được phân nhóm theo camera và sau đó theo định dạng tệp:

```
<project>/
└── LATT-M3M-L41-F550/                  # one folder per camera (model+lens+filter)
    ├── tiff16/                          # follows --format: tiff16, tiff8, png8, jpg8, or tiff32
    │   ├── Reflectance_Calibrated_Images/
    │   ├── Debayered_Images/
    │   ├── Preview_Images/
    │   └── NDVI_Index_Images/           # one <INDEX>_Index_Images/ folder per requested index
    └── tiff32/
        └── Radiance_Images/             # float32 radiance always lands here
```

Thư mục máy ảnh là `LATT-<sensor>-<lens>-F<filter>` cho LATTICE và `<model>_<filter>` (ví dụ: `Survey3N_RGN`) cho Survey3. **Mỗi sản phẩm được xuất đều giữ nguyên tên tệp nguồn — thư mục xác định sản phẩm, không phải phần mở rộng tên tệp.** Xem [Vị trí lưu trữ các tệp đầu ra](reference/cli-reference.md) trong Tài liệu tham khảo CLI để biết các quy tắc đầy đủ.

## Sản phẩm LATTICE (các mức thu thập và xuất)

Một khung hình thô LATTICE được chia thành tất cả các sản phẩm được yêu cầu trong một lần xử lý duy nhất. Mỗi loại sản phẩm có công tắc bật/tắt riêng (hộp kiểm trong GUI, hoặc lệnh CLI: `--debayered` / `--preview` / `--radiance` / `--reflectance`, tất cả đều được bật theo mặc định):

| Cấp độ | Nội dung | Loại dữ liệu |
| --- | --- | --- |
| `raw` | Dữ liệu Bayer trực tiếp từ cảm biến (máy ảnh đơn sắc: dải đơn). Quá trình xử lý luôn bắt đầu từ dữ liệu thô. | Như đã chụp |
| `debayered` | Demosaic tuyến tính — 3 kênh cho M3C, 1 kênh xám cho M3M: đã loại bỏ độ lệch tối từ gói hiệu chuẩn, sau đó thực hiện demosaic nhận diện cạnh, không có hiệu chỉnh tuyến tính, trường phẳng hoặc tách kênh. Việc xuất tệp `process` sẽ tạo ra cùng một tệp mà quá trình chụp lưu lại ở cấp độ này, trong mọi chế độ xử lý. | DN tuyến tính |
| `radiance` | Độ rạng rỡ quang phổ tuyệt đối từ chuỗi đo quang đầy đủ, tính bằng **W/m²/sr/nm**. Luôn được ghi dưới dạng tệp TIFF 32-bit (`tiff32/Radiance_Images/`), bất kể định dạng xuất được chọn là gì. | float32 |
| `reflectance` | Hệ số phản xạ ρ, trong đó **DN 32768 = ρ 1.0 (100%)** với dải động mở rộng đến ρ 2.0. Sẵn sàng cho Pix4D. | uint16 |
| `preview` | Hình ảnh đã xử lý sẵn để hiển thị: RGB: cân bằng trắng + gamma; đa phổ: mở rộng màu giả. | hiển thị 8-bit |

### Hướng

Các tệp `raw` và `debayered` luôn giữ nguyên hướng **gốc** của cảm biến; cài đặt lật/xoay của máy ảnh chỉ được áp dụng cho các sản phẩm khác (xem trước, độ rạng rỡ, độ phản xạ, chỉ số). Các bản chụp mảng sẽ giữ nguyên định hướng gốc cho từng tệp thành viên trừ khi bạn yêu cầu thay đổi định hướng (`--orient` trong giao diện dòng lệnh (CLI), `orient=True` trong bộ công cụ phát triển phần mềm (SDK)). Mỗi tệp ghi lại hướng thuộc về các pixel của nó trong XMP — `ChlorosRotation`, `ChlorosFlipHorizontal`, `ChlorosFlipVertical` — và `ChlorosOrientationApplied` cho biết liệu các pixel của chính nó có chứa thông tin đó hay không (`false` trên các tệp thô và tệp đã tách màu). Trình xem hình ảnh Chloros sẽ điều chỉnh các tệp RAW và tệp đã tách kênh theo bản ghi đó chỉ để hiển thị; các tệp gốc sẽ không bao giờ bị ghi đè. Một tệp không có bản ghi (được tạo trước phiên bản 1.3.0 hoặc bằng công cụ khác) sẽ được hiển thị nguyên trạng như khi lưu trữ.

## Mặt nạ giới hạn

Các sản phẩm về độ rạng rỡ và độ phản xạ của máy ảnh đa quang phổ M3C thực hiện quá trình tách dải 3×3. Khi quá trình tách dải tạo ra giá trị âm trong một dải, giá trị đó sẽ bị giới hạn xuống 0 (chế độ xem trực tiếp cũng giới hạn theo cách tương tự). Mới trong phiên bản 1.3.0, một sản phẩm trong đó có ít nhất một pixel bị cắt sẽ có một **mặt nạ giới hạn** đi kèm, chỉ ra chính xác các pixel và dải nào:

* **Trong đó:**trong thư mục `clamp_masks/`**bên cạnh** các thư mục sản phẩm, không bao giờ nằm bên trong một thư mục nào: `tiff16/Reflectance_Calibrated_Images/x.tif` → `tiff16/clamp_masks/Reflectance_Calibrated_Images/x_clampmask.tif`, và `tiff32/Radiance_Images/x.tif` → `tiff32/clamp_masks/Radiance_Images/x_clampmask.tif`.
* **Nội dung:** một tệp TIFF đơn dải `uint8` trên lưới pixel riêng của sản phẩm (bao gồm hướng, vùng quan tâm (ROI), nhóm pixel và căn chỉnh mảng). Bit 0 (`mask & 1`) được đặt ở vị trí dải trong kênh R của sản phẩm bị cắt, bit 1 (`& 2`) là kênh G, bit 2 (`& 4`) là kênh B. Tỷ lệ phần trăm pixel bị cắt theo từng dải cũng được ghi trên sản phẩm dưới dạng `Chloros:BandRUnmixClampFraction`, `Chloros:BandGUnmixClampFraction` và `Chloros:BandBUnmixClampFraction`.
* **Cách sản phẩm thể hiện thông tin này:** thẻ XMP `Chloros:UnmixClampMask` chứa đường dẫn đến tệp mặt nạ so với thư mục chứa sản phẩm (`../clamp_masks/<product folder>/<name>_clampmask.tif`, luôn được phân tách bằng dấu gạch chéo (`/`)), `none` khi không có pixel nào bị cắt (không ghi mặt nạ), hoặc `unavailable` khi không thể ghi mặt nạ. Một sản phẩm không có thẻ có nghĩa là quá trình tách màu chưa bao giờ được thực hiện (máy ảnh RGB và đơn sắc, các cấp độ raw và đã loại bỏ hiệu ứng Bayer).

Vì không có thư mục sản phẩm nào chứa mặt nạ, bạn có thể chuyển một thư mục sản phẩm như `Reflectance_Calibrated_Images/` cho Pix4D, Metashape hoặc phần mềm photogrammetry khác nguyên trạng, bao gồm cả các thư mục con — hãy chuyển thư mục sản phẩm đó, không phải thư mục định dạng ở trên nó. Tính năng nhập của Chloros bỏ qua các mặt nạ kẹp. CLI tính chúng riêng biệt: `Image products written: N (plus M clamp masks)`.

## Sản phẩm chỉ mục

Mỗi chỉ mục được lưu trong thư mục riêng của nó theo định dạng `<INDEX>_Index_Images/` (ví dụ: `NDVI_Index_Images/`).

* **Các định dạng số nguyên**(`TIFF (16-bit)`, `TIFF (8-bit)`, `PNG (8-bit)`, `JPG (8-bit)`) lưu trữ một**độ giãn tuyến tính cho từng hình ảnh**, chứ không phải bản đồ −1…1: DN 0 là giá trị chỉ số thấp nhất của khung hình đó và quy mô đầy đủ là giá trị cao nhất. Tệp chứa `Chloros:IndexScaling=linear_minmax`, `Chloros:IndexMin` và `Chloros:IndexMax`, do đó có `value = IndexMin + DN / full_scale × (IndexMax − IndexMin)`. Vì mỗi khung hình có các điểm đầu và cuối riêng, hãy so sánh các khung hình dựa trên giá trị, không phải dựa trên DN.
* **`TIFF (32-bit, Percent)`** lưu trữ chính các giá trị chỉ số dưới dạng float32, không qua quy đổi tỷ lệ — đây là lựa chọn tốt nhất cho công việc định lượng trên toàn chuyến bay.

Mới trong phiên bản 1.3.0, một sản phẩm chỉ số mô tả **chính nó** thay vì lặp lại các thẻ của dữ liệu thô nguồn. Các thẻ GPS và camera của nó được sao chép từ sản phẩm mà nó được tính toán từ đó (sản phẩm phản xạ nếu có), và sau đó:

| Thẻ | Giá trị |
| --- | --- |
| `Chloros:ProcessingLevel` | `index` (do đó tệp này sẽ không bao giờ được nhập lại dưới dạng dữ liệu chụp) |
| `Chloros:IndexApplied` / `Chloros:IndexExpression` | `True` / công thức được sử dụng |
| `Chloros:IndexLevel` | `reflectance` hoặc `radiance` — dữ liệu được dùng để tính toán chỉ số (không có nếu không có cả hai) |
| `Chloros:IlluminationInvariant` | `True` đối với chỉ số được tính toán từ độ phản xạ, `False` từ độ rạng rỡ hoặc các giá trị cảm biến |
| `Chloros:ChlorosPipelineVersion` | phiên bản Chloros đã tạo ra chỉ số |

`Chloros:PixelScale` đã bị loại bỏ khỏi các sản phẩm chỉ số — hãy sử dụng `IndexMin` / `IndexMax` ở trên. Công thức đọc lại đầy đủ nằm trong phần "Một raster chỉ số nguyên là quá trình kéo dãn độ tương phản theo từng hình ảnh" trong [Tài liệu tham khảo CLI](reference/cli-reference.md).

## Đọc giá trị phản xạ của pixel

Độ phản xạ được lưu trữ dưới dạng số nguyên kỹ thuật số (DN), và **giá trị DN tương ứng với ρ = 1.0 (độ phản xạ 100%) phụ thuộc vào máy ảnh nguồn**:

| Máy ảnh nguồn | ρ = 1.0 tương ứng với DN | Cách xác định |
| --- | --- | --- |
| LATTICE (M3C / M3M) | `32768` (dải động lên đến ρ 2.0) | Thẻ XMP `Chloros:PixelScale=32768` được gắn vào tệp. |
| Survey3 | `65535` (bị cắt tại ρ 1.0) | Không có thẻ `Chloros:PixelScale` — sự vắng mặt này chính là dấu hiệu. (Các sản phẩm Survey3 thực sự có các thẻ xử lý `Chloros:*` khác, chẳng hạn như `Chloros:CalibrationApplied`, trong mọi chế độ xử lý kể từ phiên bản 1.3.0.) |

**Đọc thẻ XMP `Chloros:PixelScale` và chia cho giá trị đó** thay vì giả định một hằng số. Thẻ này được định nghĩa trong miền uint16, do đó nó vẫn giữ nguyên giá trị `32768` trên các định dạng đầu ra thực hiện quy đổi tỷ lệ — trước tiên hãy chuẩn hóa kiểu dữ liệu được lưu trữ trở lại uint16 (nhân với 257 từ 8-bit, nhân với 65535 từ float32).

{% hint style="warning" %}
**Dữ liệu thu thập 8-bit.** Các giá trị xuất ra từ dữ liệu thu thập 8-bit cũng là 8-bit. Độ phản xạ của nó được điều chỉnh tỷ lệ và gắn nhãn `Chloros:PixelScale=32768`, do đó tỷ lệ được duy trì; Các sản phẩm DN 8-bit và các tệp độ phản xạ được ghi trước phiên bản 1.3.0 sẽ bị cắt giới hạn trong khoảng 0–255 và không chứa thẻ tỷ lệ. Nếu thẻ tỷ lệ vắng mặt trên tệp độ phản xạ LATTICE, không nên giả định có tỷ lệ; thay vào đó, hãy xuất lại ở định dạng 16-bit hoặc 32-bit.
{% endhint %}

Để xem các quy tắc đầy đủ (bao gồm các thẻ tương thích với MicaSense), hãy tham khảo phần **"Đọc các pixel phản xạ"** trong [Tài liệu tham khảo CLI](reference/cli-reference.md).
