---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/supported-cameras
---

# Các dòng camera được hỗ trợ:

Chloros xử lý hình ảnh từ hai dòng camera MAPIR trên **tất cả các nền tảng** (Windows, Linux amd64 và Linux arm64/Jetson):

* **Survey3** — Máy ảnh Survey3W (góc rộng) và Survey3N (góc hẹp). Đầu vào: `RAW+JPG`.
* **LATTICE**— Các mô-đun camera đa phổ M3C và M3M. Đầu vào: Các bản chụp từ `.tif`/`.tiff`. Các camera LATTICE cũng có thể được**điều khiển trực tiếp** từ Chloros — thông qua tab Cameras trên giao diện người dùng (GUI) (Windows) hoặc `chloros-cli lattice` / Python SDK (Windows và Linux) — bao gồm cả mảng camera đa kênh đồng bộ. Xem [Hướng dẫn LATTICE](lattice/).

Quy trình xử lý cũng chấp nhận các tệp đầu vào `.dng`.

## Survey3

<table data-header-hidden><thead><tr><th width="156">Nhà sản xuất</th><th width="250">Mẫu máy ảnh</th><th width="138">Mô hình bộ lọc</th><th width="187">Loại hình ảnh</th></tr></thead><tbody><tr><td><strong>Nhà sản xuất</strong></td><td><strong>Mẫu máy ảnh</strong></td><td><strong>Mẫu bộ lọc</strong></td><td><strong>Loại ảnh</strong></td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>RGB</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>RGN</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>OCN</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>NGB</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>RE</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>NIR</td><td>RAW+JPG, JPG</td></tr></tbody></table>## LATTICE

Dòng LATTICE là hệ thống camera đa quang phổ mô-đun được xây dựng dựa trên cảm biến màn trập toàn cầu Sony IMX265 (3,1 MP, điểm ảnh 3,45 µm). Mỗi camera lưu trữ thông tin nhận dạng của mình dưới dạng chuỗi ký tự mô hình:

```
<sensor>-<lens>-F<filter>        e.g.  M3C-L41-FRGN,  M3M-L87-F550
```

Chloros hiển thị nó với tiền tố `LATT-` (ví dụ: `LATT-M3M-L41-F550`), và chuỗi ký tự mẫu này điều khiển mọi thứ ở các bước tiếp theo — cấu hình cảm biến, bố cục dải phổ và hiệu chuẩn được xác định tự động; không cần phải cấu hình riêng cho từng camera. Số ống kính là **góc nhìn ngang tính bằng độ**: `L41` = hẹp 41°, `L87` = rộng 87°.

Có hai cấu hình cảm biến:

| Cấu hình | Cảm biến      | Loại bộ lọc                           | Số dải tần trên mỗi camera                                                        |
| ------------- | ----------- | ------------------------------------- | ----------------------------------------------------------------------- |
| **M3C**       | Màu Bayer | Bộ lọc ba dải thông                       | 3 dải quang phổ từ một lần phơi sáng                                 |
| **M3M**       | Đơn sắc  | Bộ lọc giao thoa dải hẹp đơn | 1 dải đã hiệu chuẩn — kết hợp nhiều camera M3M để tính chỉ số thực vật |

### Các tùy chọn bộ lọc M3C (Bayer)

| Bộ lọc | Dải (tên @ tâm nm / FWHM nm)       |
| ------ | ---------------------------------------- |
| `FRGB` | Blue 475/30 · Green 550/30 · Red 625/30  |
| `FRGN` | Red 660/21 · Green 550/30 · NIR 850/30   |
| `FOCN` | Orange 618,4/46 · Cyan 494,1/40 · NIR 821,6/54 (đo được — xem bên dưới) |
| `FNGB` | Blue 475/30 · Green 550/30 · NIR 850/30  |

Đối với `FOCN`, các giá trị này là dải nửa cực đại **được đo** của bộ lọc (trong Chloros 1.3.0 trở lên; các phiên bản trước đó sử dụng các giá trị trong bảng thông số kỹ thuật là 615/21 · 490/38 · 808/14). Tên dải vẫn là `Orange_615`, `Cyan_490` và `NIR_808`, còn tâm và độ rộng đo được là những thông số mà Chloros ghi vào mỗi tệp xuất đã hiệu chuẩn.

### Danh mục bộ lọc M3M (mono) — 23 mã sản phẩm (SKU)

Số F là nhãn mã sản phẩm (SKU); dải tần số đo được (được in trên mỗi sản phẩm xuất xưởng đã được hiệu chuẩn) là kết quả quét bộ lọc theo từng lô:

| SKU    | Tần số trung tâm (nm, đo được) | Độ rộng FWHM (nm) | Độ rộng (nm) |
| ------ | --------------------- | --------------- | ---------- |
| F385   | 379,4                 | 367–392         | 25         |
| F405   | 403,9                 | 390–417         | 27         |
| F450   | 443,7                 | 430–458         | 28         |
| F485   | 489,7                 | 478–502         | 24         |
| F520   | 519,9                 | 504–536         | 32         |
| F550   | 548,4                 | 531–566         | 35         |
| F590   | 589,0                 | 570–608         | 38         |
| F615   | 623,8                 | 614–634         | 20         |
| F632   | 633,4                 | 616–651         | 35         |
| F650   | 651,1                 | 636–666         | 30         |
| F685   | 686,2                 | 675–698         | 23         |
| F715   | — (danh nghĩa)           | 706–724         | 18         |
| F725   | 725.2                 | 712–738         | 26         |
| F750   | 746.0                 | 729–763         | 34         |
| F780   | 775.1                 | 754–796         | 42         |
| F808   | 810.3                 | 789–832         | 43         |
| F832   | 826,1                 | 810–843         | 33         |
| F850   | 846,5                 | 828–865         | 37         |
| F880   | — (danh nghĩa)           | 867–893         | 26         |
| F905   | — (danh nghĩa)           | 892–920         | 28         |
| F940   | 940,6                 | 923–958         | 35         |
| F950   | 945,1                 | 929–961         | 32         |
| F988 † | 985,3                 | 968–1003        | 35         |

_"Độ rộng dải tần được đo bằng giá trị độ rộng toàn phần tại nửa cực đại (FWHM) từ các bản quét bộ lọc theo lô của MAPIR — đây cũng chính là các giá trị mà Chloros ghi vào mỗi bản xuất đã được hiệu chuẩn."_ "— (danh nghĩa)" = chưa có quét lô nào; đối với các SKU đó, tâm được nêu là số SKU và độ rộng là số liệu của nhà sản xuất.

† "Độ phản xạ của dải F988 được hiệu chuẩn bằng bảng đo độ phản xạ trong cảnh: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros sẽ áp dụng dữ liệu chụp bảng đo độ phản xạ mới nhất của bạn và duy trì giá trị đó giữa các lần quét bảng." Xem [Mục tiêu hiệu chuẩn](calibration-targets.md).

Để biết thông tin về điều khiển camera trực tiếp, mảng cảm biến, thiết lập mạng và chuỗi xử lý phóng xạ, hãy tham khảo [Hướng dẫn LATTICE](lattice/).
