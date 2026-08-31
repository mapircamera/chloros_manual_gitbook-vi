---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/output-image-formats
---

# Định dạng hình ảnh đầu ra

Chlorosxuất các sản phẩm đã xử lý dưới bốn định dạng tệp. Chọn định dạng trong Cài đặt Dự án (GUI), bằng `--format` (CLI), hoặc bằng `export_format` (SDK). Các tham số CLI và SDK chấp nhận các chuỗi chính xác như dưới đây.

| Chuỗi định dạng | Phần mở rộng | Loại pixel | Phạm vi pixel | Ghi chú |
| --- | --- | --- | --- | --- |
| `TIFF (16-bit)` *(mặc định)* | `.tif` | số nguyên 16 bit | 0 – 65535 | Được khuyến nghị cho photogrammetry / GIS. |
| `TIFF (32-bit, Percent)` | `.tif` | float32 | 0.0 – 1.0 | 1.0 = độ phản xạ 100%. Một số ứng dụng không thể đọc tệp TIFF dạng số thập phân; tệp có dung lượng lớn hơn. |
| `PNG (8-bit)` | `.png` | số nguyên 8-bit | 0 – 255 | Nén không mất dữ liệu, phù hợp để xem trên web và trực quan hóa. |
| `JPG (8-bit)` | `.jpg` | số nguyên 8 bit (uint8) | 0 – 255 | Nén có mất dữ liệu, tệp nhỏ nhất. |

## Vị trí lưu trữ tệp đầu ra

Các tệp được lưu trong thư mục dự án, được phân nhóm theo camera và sau đó theo định dạng tệp:

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

Thư mục camera là `LATT-<sensor>-<lens>-F<filter>` cho LATTICE và `<model>_<filter>` (ví dụ: `Survey3N_RGN`) cho Survey3. **Mỗi sản phẩm được xuất đều giữ nguyên tên tệp nguồn — thư mục xác định sản phẩm, không phải phần mở rộng tên tệp.** Xem [Vị trí lưu trữ đầu ra](reference/cli-reference.md) trong Tài liệu tham khảo CLI để biết các quy tắc đầy đủ.

## Sản phẩm LATTICE (các mức thu thập và xuất)

Một khung hình thô LATTICE được chia thành từng sản phẩm được yêu cầu trong một lần xử lý duy nhất. Mỗi loại sản phẩm có công tắc bật/tắt riêng (hộp kiểm trong giao diện người dùng hoặc CLI `--debayered` / `--preview` / `--radiance` / `--reflectance`, mặc định đều bật):

| Cấp độ | Nội dung | Loại dữ liệu |
| --- | --- | --- |
| `raw` | Dữ liệu Bayer trực tiếp từ cảm biến (máy ảnh đơn sắc: dải đơn). Quá trình xử lý luôn bắt đầu từ dữ liệu thô. | Như đã chụp |
| `debayered` | Demosaic tuyến tính — 3 kênh cho M3C, 1 kênh thang xám cho M3M. | DN tuyến tính |
| `radiance` | Độ rạng quang phổ tuyệt đối từ chuỗi đo quang học đầy đủ, tính bằng **W/m²/sr/nm**. Luôn được ghi dưới dạng số nguyên 32-bit (TIFF) (`tiff32/Radiance_Images/`), bất kể định dạng xuất được chọn là gì. | float32 |
| `reflectance` | Độ phản xạ ρ, trong đó **DN 32768 = ρ 1,0 (100%)** với dải động lên đến ρ 2,0. Sẵn sàng cho Pix4D. | uint16 |
| `preview` | Hình ảnh đã xử lý sẵn để hiển thị: RGB = cân bằng trắng + gamma; đa phổ = kéo dãn màu giả. | hiển thị 8-bit |

## Đọc giá trị pixel độ phản xạ

Độ phản xạ được lưu trữ dưới dạng số nguyên kỹ thuật số, và **giá trị DN tương ứng với ρ = 1.0 (độ phản xạ 100%) phụ thuộc vào máy ảnh nguồn**:

| Máy ảnh nguồn | ρ = 1.0 tương ứng với DN | Cách xác định |
| --- | --- | --- |
| LATTICE (M3C / M3M) | `32768` (dải động lên đến ρ 2.0) | Thẻ XMP `Chloros:PixelScale=32768` được ghi trên tệp. |
| Survey3 | `65535` (bị cắt tại ρ 1.0) | Không có thẻ XMP `Chloros:*` — sự vắng mặt này chính là dấu hiệu. |

**Đọc thẻ XMP `Chloros:PixelScale` và chia cho giá trị đó** thay vì giả định một hằng số. Thẻ này được định nghĩa trong miền uint16, do đó nó vẫn giữ nguyên giá trị `32768` trên các định dạng đầu ra thực hiện quy đổi tỷ lệ — hãy chuẩn hóa kiểu dữ liệu được lưu trữ trở lại uint16 trước (nhân với 257 từ 8-bit, nhân với 65535 từ float32).

{% hint style="warning" %}
**Một trường hợp không có tỷ lệ quy đổi, theo thiết kế.** Khi dữ liệu nguồn 8-bit (BayerRG8) được ghi dưới dạng tệp phản xạ 8-bit (TIFF), quy trình xử lý sẽ cắt giới hạn trong khoảng 0–255 thay vì điều chỉnh tỷ lệ, do đó tệp không chứa thông tin tỷ lệ — Chloros đã cố ý loại bỏ `Chloros:PixelScale` trong trường hợp này. Nếu thẻ này vắng mặt trong tệp phản xạ LATTICE, đừng giả định có hệ số tỷ lệ; thay vào đó, hãy xuất lại ở định dạng 16-bit hoặc 32-bit.
{% endhint %}

Để xem các quy tắc đầy đủ (bao gồm các thẻ tương thích với MicaSense), hãy tham khảo phần **&quot;Đọc các pixel phản xạ&quot;** trong [CLI Tài liệu tham khảo](reference/cli-reference.md).
