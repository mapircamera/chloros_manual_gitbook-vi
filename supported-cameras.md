---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/supported-cameras
---

# Các dòng camera được hỗ trợ

Chloros

xử lý hình ảnh từ hai dòng cameraMAPIR

trên **tất cả các nền tảng** (Windows

,Linux

amd64 vàLinux

arm64/Jetson):

* **Survey3** — Các dòng cameraSurvey3W

(góc rộng) vàSurvey3N

(góc hẹp). Đầu vào: `RAW+JPG`.
* **LATTICE**— Các mô-đun camera đa quang phổ M3C và M3M. Đầu vào: Các bản chụp từ `.tif`/`.tiff`. Các camera LATTICE cũng có thể được**điều khiển trực tiếp** từChloros

— thông qua tab Cameras trên giao diện người dùng (Windows

) hoặc `chloros-cli lattice` /Python

SDK

(Windows

vàLinux

) — bao gồm cả các mảng camera đa kênh đồng bộ. Xem [Hướng dẫn LATTICE](lattice/).

Quy trình xử lý cũng chấp nhận các tệp đầu vào có định dạng `.dng`.

##Survey3

<table data-header-hidden><thead><tr><th width="156">Nhà sản xuất</th><th width="250">Mẫu camera</th><th width="138">Mô hình bộ lọc</th><th width="187">Loại hình ảnh</th></tr></thead><tbody><tr><td><strong>Nhà sản xuất</strong></td><td><strong>Mẫu máy ảnh</strong></td><td><strong>Mẫu bộ lọc</strong></td><td><strong>Loại hình ảnh</strong></td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>RGB</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>RGN</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>OCN</td><td>RAW + JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>NGB</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>RE</td><td>RAW+JPG, JPG</td></tr><tr><td>MAPIR</td><td>Survey3W, Survey3N</td><td>NIR</td><td>RAW+JPG, JPG</td></tr></tbody></table>## LATTICE

Dòng sản phẩm LATTICE là hệ thống camera đa quang phổ mô-đun được xây dựng dựa trên cảm biến màn trập toàn cầu Sony IMX265 (3,1 MP, điểm ảnh 3,45 µm). Mỗi camera lưu trữ thông tin nhận dạng của mình dưới dạng chuỗi ký tự mô hình:

```
<sensor>-<lens>-F<filter>        e.g.  M3C-L41-FRGN,  M3M-L87-F550
```

Chloros

hiển thị thông tin này với tiền tố `LATT-` (ví dụ: `LATT-M3M-L41-F550`), và chuỗi ký tự mô hình này điều khiển toàn bộ các bước tiếp theo — cấu hình cảm biến, bố cục dải phổ và hiệu chuẩn đều được thiết lập tự động; không cần cấu hình gì cho từng camera. Số ống kính tương ứng với **góc nhìn ngang tính bằng độ**: `L41` = góc hẹp 41°, `L87` = góc rộng 87°.

Có hai cấu hình cảm biến:

| Cấu hình | Cảm biến      | Loại bộ lọc                           | Số dải tần trên mỗi camera                                                        |
| ------------- | ----------- | ------------------------------------- | ----------------------------------------------------------------------- |
| **M3C**       | Màu Bayer | Bộ lọc ba dải tần                       | 3 dải quang phổ từ một lần phơi sáng                                 |
| **M3M**       | Đơn sắc  | Bộ lọc nhiễu dải hẹp đơn | 1 dải đã hiệu chuẩn — kết hợp nhiều camera M3M để tính chỉ số thực vật |

### Các tùy chọn bộ lọc M3C (Bayer)

| Bộ lọc | Dải (tên @ trung tâm nm /FWHM

nm)       |
| ------ | ---------------------------------------- |
| `FRGB` |Blue

475/30 ·Green

550/30 ·Red

625/30  |
| `FRGN` |Red

660/21 ·Green

550/30 ·NIR

850/30   |
| `FOCN` |Orange

615/21 ·Cyan

490/38 ·NIR

808/14 |
| `FNGB` |Blue

475/30 ·Green

550/30 ·NIR

850/30  |

### Danh mục bộ lọc M3M (mono) — 23 mã sản phẩm (SKU)

Số F là mã sản phẩm (SKU); dải tần số đo được (được in trên mỗi sản phẩm xuất xưởng đã hiệu chuẩn) là kết quả quét bộ lọc theo từng lô:

| SKU    | Tần số trung tâm (nm, đo được) | Ranh giớiFWHM

(nm) | Độ rộng (nm) |
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
| F750   | 746,0                 | 729–763         | 34         |
| F780   | 775,1                 | 754–796         | 42         |
| F808   | 810,3                 | 789–832         | 43         |
| F832   | 826,1                 | 810–843         | 33         |
| F850   | 846,5                 | 828–865         | 37         |
| F880   | — (danh nghĩa)           | 867–893         | 26         |
| F905   | — (danh nghĩa)           | 892–920         | 28         |
| F940   | 940,6                 | 923–958         | 35         |
| F950   | 945,1                 | 929–961         | 32         |
| F988 † | 985,3                 | 968–1003        | 35         |

_&quot;Độ rộng tại nửa cực đại (FWHM) của các cạnh dải được đo từ các bản quét bộ lọc theo lô củMAPIR

— đây cũng chính là các giá trị màChloros

ghi vào mỗi tệp xuất đã được hiệu chuẩn.&quot;_ &quot;— (danh nghĩa)&quot; = chưa có bản quét lô; đối với các SKU này, điểm trung tâm được ghi là số SKU và độ rộng là số liệu do nhà sản xuất cung cấp.

† &quot;Độ phản xạ của dải F988 được hiệu chuẩn bằng bảng phản xạ trong cảnh: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đóChloros

sẽ áp dụng dữ liệu chụp bảng phản xạ mới nhất của bạn và duy trì giá trị đó giữa các lần quan sát bảng phản xạ.&quot; Xem [Mục tiêu hiệu chuẩn](calibration-targets.md).

Để điều khiển camera thời gian thực, mảng cảm biến, thiết lập mạng và chuỗi xử lý quang phổ, hãy tham khảo [Hướng dẫn LATTICE](lattice/).
