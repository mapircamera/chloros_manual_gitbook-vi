---
description: This page lists some multispectral indices that Chloros uses
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/multispectral-index-formulas
---

# Công thức chỉ số đa quang phổ

Các công thức chỉ số dưới đây sử dụng sự kết hợp của các dải truyền trung bình của bộ lọc Survey3:

<table><thead><tr><th align="center">Màu bộ lọc Survey3</th><th width="196.199951171875" align="center">Tên bộ lọc Survey3</th><th width="159.800048828125" align="center">Phạm vi truyền qua (FWHM)</th><th align="center">Độ truyền trung bình</th></tr></thead><tbody><tr><td align="center">Blue</td><td align="center">NGB - Blue</td><td align="center">468-483nm</td><td align="center">475 nm</td></tr><tr><td align="center">Cyan</td><td align="center">OCN- Cyan</td><td align="center">476–512 nm</td><td align="center">494 nm</td></tr><tr><td align="center">Green</td><td align="center">RGN | NGB - Green</td><td align="center">543–558 nm</td><td align="center">547 nm</td></tr><tr><td align="center">Orange</td><td align="center">OCN - Orange</td><td align="center">598–640 nm</td><td align="center">619 nm</td></tr><tr><td align="center">Red</td><td align="center">RGN - Red</td><td align="center">653–668 nm</td><td align="center">661 nm</td></tr><tr><td align="center">RedEdge</td><td align="center">Re-RedEdge</td><td align="center">712–735 nm</td><td align="center">724 nm</td></tr><tr><td align="center">NIR1</td><td align="center">OCN - NIR1</td><td align="center">798–848 nm</td><td align="center">823 nm</td></tr><tr><td align="center">NIR2</td><td align="center">RGN | NGB | NIR - NIR2</td><td align="center">835–865 nm</td><td align="center">850 nm</td></tr></tbody></table>

Khi sử dụng các công thức này, tên có thể kết thúc bằng &quot;\_1&quot; hoặc &quot;\_2&quot;, tương ứng với bộ lọc NIR nào đã được sử dụng, là NIR1 hay NIR2.

Đối với máy ảnh LATTICE M3C (bộ lọc ba dải tần Bayer), cùng một công cụ lập chỉ mục sử dụng các dải tần của bộ lọc M3C:

| Bộ lọc M3C | Dải 1 (trung tâm/ FWHM) | Dải 2 (trung tâm/ FWHM) | Dải 3 (trung tâm/ FWHM) |
| --- | --- | --- | --- |
| FRGB | Blue 475 nm / 30 nm | Green 550 nm / 30 nm | Red 625 nm / 30 nm |
| FRGN | Red 660 nm / 21 nm | Green 550 nm / 30 nm | NIR 850 nm / 30 nm |
| FOCN | Màu Orange 615 nm / 21 nm | Màu Cyan 490 nm / 38 nm | NIR 808 nm / 14 nm |
| FNGB | Xanh dương (Blue) 475 nm / 30 nm | Xanh lá (Green) 550 nm / 30 nm | Hồng ngoại gần (NIR) 850 nm / 30 nm |

Các camera LATTICE M3M là loại đơn dải (mỗi camera có một bộ lọc dải hẹp), do đó các chỉ số đa dải không được tính toán cho một hình ảnh M3M đơn lẻ. Để tính toán các chỉ số với M3M, hãy kết hợp hai hoặc nhiều máy ảnh thành một chồng ảnh đa dải đã được căn chỉnh và sử dụng công cụ tính chỉ số LATTICE (`chloros-cli lattice index`, hoặc Trình tính chỉ số trực tiếp trên giao diện người dùng).

***

## Tên chỉ số nào hoạt động ở đâu

Hệ thống Chloros có **ba** bề mặt chỉ số, và danh sách cài đặt sẵn của chúng không giống nhau. Sử dụng phần này để kiểm tra xem một tên chỉ số có hoạt động ở nơi bạn dự định sử dụng hay không.

| Vị trí của bạn | Danh sách nào áp dụng | Số lượng |
| --- | --- | --- |
| Cài đặt Dự án → Chỉ số → Thêm chỉ số (GUI) | Bề mặt 1 | 27 |
| Trình xem hình ảnh [Hộp cát Chỉ số/LUT](../image-viewer-gui/index-lut-sandbox.md) (GUI) | Bề mặt 1 | 27 |
| `chloros-cli process --indices NDVI,NDRE` | Bề mặt 2 | 22 |
| SDK `process_folder(indices=[...])` | Bề mặt 2 | 22 |
| `chloros-cli lattice index --preset` | Surface 3 | 22 (một giá trị 22 khác) |
| Trình tính toán chỉ số trực tiếp trên tab Máy ảnh | Surface 3 | 22 (một giá trị 22 khác) |

Surface 1 và 2 hoạt động trên **một hình ảnh tại một thời điểm từ một camera**, sử dụng các khe biểu tượng `x`/`y`/`z`(/`a`) được liên kết với các kênh bộ lọc của camera đó. Bề mặt 3 hoạt động trên một**bộ xếp chồng đa dải đã được căn chỉnh** — nhiều camera LATTICE được căn chỉnh chung thành một khối — và tham chiếu các kênh bằng tên viết thường.

### 1. Cài đặt Dự án GUI / Menu thả xuống hộp cát Trình xem hình ảnh — 27 công thức

Menu thả xuống liệt kê chúng theo thứ tự này (theo thứ tự chèn vào, không phải thứ tự bảng chữ cái):

`NDVI, GNDVI, CVI, ENDVI, EVI, MSR, OSAVI, TDVI, LAI, FCI1, FCI2, GARI, GCI, GEMI, GLI, GOSAVI, GRVI, GSAVI, LCI, MNLI, MSAVI2, NDRE, NLI, RDVI, SAVI, VARI, WDRVI`

Trong GUI, bạn kéo các kênh bộ lọc của camera vào các ô dải tần của công thức, do đó bất kỳ công thức nào cũng có thể được sử dụng với bất kỳ cách gán dải tần nào mà camera của bạn hỗ trợ. Các công thức tùy chỉnh mà bạn đã lưu sẽ được thêm vào phía dưới danh sách này.

**Năm công thức chỉ dành cho GUI** — những công thức mà danh sách `--indices` của CLI/SDK không chấp nhận — được triển khai như sau:

| Cài đặt sẵn chỉ dành cho GUI | Công thức (theo cách triển khai) | Khe |
| --- | --- | --- |
| FCI1 | `x*y` | x, y |
| FCI2 | `x*y` | x, y |
| GARI | `(y-(x-1.7*(z-a)))/(y+(x-1.7*(z-a)))` | x, y, z, a (bốn khe) |
| GEMI | `((2*(y*y-x*x)+1.5*y+0.5*x)/(y+x+0.5))*(1-0.25*((2*(y*y-x*x)+1.5*y+0.5*x)/(y+x+0.5)))-((x-0.125)/(1-x))` | x, y |
| LCI | `(y-x)/(y+z)` | x, y, z |

Cách ánh xạ dự kiến cho từng công thức được nêu trong phần riêng biệt ở phía dưới trang này (ví dụ: GARI yêu cầu x=Green, y=NIR, z=Blue, a=Red). GARI là công thức duy nhất trong Chloros sử dụng khe thứ tư.

### 2. CLI / SDK `--indices` mở rộng tên — 22 cài đặt sẵn

Tùy chọn `chloros-cli process --indices` (và tham số SDK `indices`) chấp nhận các tên cài đặt sẵn sau:

`NDVI, GNDVI, NDRE, OSAVI, SAVI, MSAVI2, EVI, MSR, TDVI, LAI, GCI, GRVI, GSAVI, GOSAVI, NLI, MNLI, RDVI, WDRVI, CVI, ENDVI, GLI, VARI`

{% hint style="warning" %}
**Các tên chỉ mục không được biết đến sẽ bị bỏ qua mà không có thông báo.** Một tên nằm ngoài danh sách này (bao gồm năm công thức chỉ dành cho GUI là `FCI1`, `FCI2`, `GARI`, `GEMI`, `LCI`, và bất kỳ công thức tùy chỉnh nào bạn đã lưu trong GUI) sẽ bị loại bỏ kèm theo một thông báo trong nhật ký — quá trình chạy sẽ tiếp tục mà không có chỉ mục đó, và bản thân quá trình chạy vẫn báo cáo thành công. Thông báo được hiển thị như sau:

```
[INDEX_EXPAND] skipping unknown preset 'LCI'; known: ['CVI', 'ENDVI', 'EVI', ...]
```

Tên được so khớp không phân biệt chữ hoa chữ thường sau khi loại bỏ khoảng trắng, do đó `ndvi`, `NDVI` và ` NDVI ` là cùng một cài đặt sẵn. Một cài đặt sẵn cũng sẽ bị bỏ qua nếu nó yêu cầu dải phổ mà bộ lọc của máy ảnh không cung cấp.
{% endhint %}

Các công thức chính xác như đã triển khai (các ký hiệu `x`/`y`/`z` là các khe dải; bản đồ mặc định được hiển thị cho từng cài đặt sẵn):

| Cài đặt sẵn | Công thức (như đã triển khai) | Bộ lọc mặc định | Vị trí dải phổ (x, y, z) |
| --- | --- | --- | --- |
| NDVI | `(y-x)/(y+x)` | RGN | Red, NIR |
| GNDVI | `(y-x)/(y+x)` | RGN | Green, NIR |
| NDRE | `(y-x)/(y+x)` | RE | RE, NIR |
| OSAVI | `(y-x)/(y+x+0.16)` | RGN | Red, NIR |
| SAVI | `1.5*(y-x)/(y+x+0.5)` | RGN | Red, NIR |
| MSAVI2 | `(2*y+1-sqrt((2*y+1)*(2*y+1)-8*(y-x)))/2` | RGN | Red, NIR |
| EVI | `2.5*(y-x)/(y+6*x-7.5*z+1)` | RGN | Red, NIR, Blue |
| MSR | `((y/x)-1)/(sqrt(y/x)+1)` | RGN | Red, NIR |
| TDVI | `1.5*(y-x)/sqrt(y*y+x+0.5)` | RGN | Red, NIR |
| LAI | `3.618*(2.5*(y-x)/(y+6*x-7.5*z+1))-0.118` | RGN | Red, NIR, Blue |
| GCI | `(y/x)-1` | RGN | Green, NIR |
| GRVI | `y/x` | RGN | Green, NIR |
| GSAVI | `1.5*(y-x)/(y+x+0.5)` | RGN | Green, NIR |
| GOSAVI | `(y-x)/(y+x+0.16)` | RGN | Green, NIR |
| NLI | `((y*y)-x)/((y*y)+x)` | RGN | Red, NIR |
| MNLI | `((y*y-x)*(1+0.5))/((y*y)+x+0.5)` | RGN | Red, NIR |
| RDVI | `(y-x)/sqrt(y+x)` | RGN | Red, NIR |
| WDRVI | `(0.2*y-x)/(0.2*y+x)` | RGN | Red, NIR |
| CVI | `(z/y)*(x/y)` | RGN | Red, Green, NIR |
| ENDVI | `((x+y)-(2*z))/((x+y)+(2*z))` | NGB | NIR, Green, Blue |
| GLI | `((y-x)+(y-z))/((2*y)+x+z)` | RGB | Red, Green, Blue |
| VARI | `(y-x)/(y+x-z)` | RGB | Red, Green, Blue |

#### Cách tên cài đặt sẵn được chuyển đổi thành vị trí dải tần

Khi bạn truyền một tên đơn thuần như `NDVI`, Chloros phải xác định kênh nào của tệp nào mà mỗi ký hiệu tương ứng. Chương trình sử dụng bảng này, trong đó ánh xạ mã bộ lọc sang chỉ số mảng của từng kênh:

| Mã bộ lọc | Kênh → chỉ số mảng |
| --- | --- |
| OCN | Orange 0, Cyan 1, NIR 2 (`Red` được chấp nhận là tên gọi khác của Orange, cũng là 0) |
| RGN | Red 0, Green 1, NIR 2 |
| NGB | NIR 0, Green 1, Blue 2 |
| RGB | Red 0, Green 1, Blue 2 |
| RE | RE 0 |
| NIR | NIR 0 |

**Bộ lọc mặc định** của cài đặt sẵn (cột &quot;Default filter&quot; ở trên) sẽ được sử dụng khi dự án chứa các hình ảnh có bộ lọc đó. Nếu không, Chloros sẽ quét các bộ lọc thực sự có trong dự án theo thứ tự `RGN, OCN, NGB, RGB, RE, NIR` và chọn bộ lọc đầu tiên có thể cung cấp đầy đủ các kênh mà cài đặt sẵn yêu cầu. Nếu không có bộ lọc nào đáp ứng được, cài đặt sẵn sẽ bị loại bỏ cho lần chạy đó. Đây là lý do tại sao `NDVI` được yêu cầu trên bộ dữ liệu chỉ có OCN vẫn cho kết quả hợp lý — nó liên kết với các vị trí Orange và NIR của OCN.

Các chuỗi mô hình LATTICE M3C mang bộ lọc có tiền tố `F` (`LATT-M3C-L41-FRGN`), nhưng tiền tố này sẽ bị loại bỏ khi mã bộ lọc được đọc từ hình ảnh, do đó máy ảnh FRGN sẽ giải quyết thông qua hàng `RGN` ở trên và không cần xử lý đặc biệt.

### 3. Công cụ lập chỉ mục LATTICE (`lattice index --preset`, Trình tính toán chỉ mục trực tiếp) — 22 cài đặt sẵn

Bộ xử lý LATTICE hoạt động trên các chồng ảnh đa dải đã được căn chỉnh (mảng trực tiếp hoặc tệp TIFF đa dải đã xuất) và sử dụng tên kênh viết thường (`red`, `green`, `blue`, `red_edge`, `nir`). Danh sách cài đặt sẵn của nó khác với hai bộ trên:

| Cài đặt sẵn | Công thức | Kênh |
| --- | --- | --- |
| NDVI | `(nir - red) / (nir + red)` | đỏ, hồng ngoại gần |
| GNDVI | `(nir - green) / (nir + green)` | xanh lá, hồng ngoại gần |
| BNDVI | `(nir - blue) / (nir + blue)` | xanh dương, hồng ngoại gần |
| NDRE | `(nir - red_edge) / (nir + red_edge)` | đỏ_edge, hồng ngoại gần |
| ENDVI | `((nir + green) - 2*blue) / ((nir + green) + 2*blue)` | xanh dương, xanh lá, hồng ngoại gần |
| SAVI | `1.5 * (nir - red) / (nir + red + 0.5)` | đỏ, hồng ngoại gần |
| OSAVI | `(nir - red) / (nir + red + 0.16)` | đỏ, hồng ngoại |
| MSAVI | `(2*nir + 1 - sqrt((2*nir + 1)**2 - 8*(nir - red))) / 2` | đỏ, hồng ngoại |
| EVI | `2.5 * (nir - red) / (nir + 6*red - 7.5*blue + 1)` | xanh dương, đỏ, hồng ngoại |
| EVI2 | `2.5 * (nir - red) / (nir + 2.4*red + 1)` | đỏ, hồng ngoại |
| CVI | `nir * red / green**2` | đỏ, xanh lá, hồng ngoại |
| MSR | `((nir/red) - 1) / (sqrt(nir/red) + 1)` | đỏ, hồng ngoại |
| TDVI | `1.5 * (nir - red) / sqrt(nir**2 + red + 0.5)` | đỏ, hồng ngoại |
| LAI | `3.618 * (2.5 * (nir - red) / (nir + 6*red - 7.5*blue + 1)) - 0.118` | xanh dương, đỏ, hồng ngoại |
| GLI | `(2*green - red - blue) / (2*green + red + blue)` | đỏ, xanh lá, xanh dương |
| NGRDI | `(green - red) / (green + red)` | đỏ, xanh lá |
| VARI | `(green - red) / (green + red - blue)` | đỏ, xanh lá, xanh dương |
| TGI | `green - 0.39*red - 0.61*blue` | đỏ, xanh lá, xanh dương |
| EXG | `2*green - red - blue` | đỏ, xanh lá, xanh dương |
| CIRE | `(nir / red_edge) - 1` | đỏ\_viền, hồng ngoại |
| CIGREEN | `(nir / green) - 1` | xanh lá, hồng ngoại |
| NDWI | `(green - nir) / (green + nir)` | xanh lá, hồng ngoại |

Chạy lệnh `chloros-cli lattice index --list-presets` để in bảng này từ bản dựng đã cài đặt của bạn, và `--list-gradients` để xem các dải màu có sẵn. Các ký hiệu kênh phân biệt chữ hoa chữ thường và phải trùng khớp với tên viết thường của cài đặt sẵn (ví dụ: `--channel red=Red_660 --channel nir=NIR_850`).

***

## CVI - Chỉ số Thực vật Dựa trên Diệp lục

Chỉ số này nhạy cảm với hàm lượng diệp lục trong lá trên một phạm vi rộng của mật độ tán cây. Nó sử dụng các dải NIR, đỏ và xanh lá cây.

$$
CVI = NIR * {Red \over Green^{2}}
$$

Trong giao diện người dùng (GUI) và danh sách cài đặt sẵn của CLI/SDK, công thức ký hiệu là `(z/y)*(x/y)` với x=Red, y=Green, z=NIR (các dải sóng của camera RGN); cài đặt sẵn của bộ xử lý chỉ số LATTICE là `nir * red / green**2`. Mọi bề mặt đều tính toán cùng một chỉ số.

_Tham khảo: Vincini, M., E. Frazzi, và P. D&#x27;Alessio. &quot;Chỉ số Thực vật Dựa trên Lá và Diệp lục Băng tần Rộng ở Quy mô Tán cây.&quot; Precision Agriculture 9 (2008): 303-319._

***

## ENDVI - Chỉ số Thực vật Chênh lệch Chuẩn hóa Nâng cao

Chỉ số này sử dụng dải màu xanh dương bên cạnh dải hồng ngoại gần (NIR) và dải xanh lá, và được ưa chuộng với các máy ảnh có bộ lọc NGB, nơi dải màu xanh dương thay thế cho dải màu đỏ.

$$
ENDVI = {(NIR + Green) - (2 * Blue) \over (NIR + Green) + (2 * Blue)}
$$

Cách triển khai là công thức ký hiệu `((x+y)-(2*z))/((x+y)+(2*z))` — gán các kênh NIR và Green của máy ảnh vào các ô x/y và kênh Blue vào ô z (đối với máy ảnh NGB: x=NIR, y=Green, z=Blue).

***

## EVI - Chỉ số Thực vật Nâng cao

Chỉ số này ban đầu được phát triển để sử dụng với dữ liệu MODIS như một cải tiến so với NDVI bằng cách tối ưu hóa tín hiệu thực vật ở các khu vực có chỉ số diện tích lá (LAI) cao. Nó hữu ích nhất ở các khu vực có LAI cao, nơi NDVI có thể bị bão hòa. Chỉ số này sử dụng vùng phản xạ màu xanh dương để hiệu chỉnh tín hiệu nền từ đất và giảm thiểu ảnh hưởng của khí quyển, bao gồm cả hiện tượng tán xạ của hạt aerosol.

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Giá trị EVI đối với các pixel thực vật nên nằm trong khoảng từ 0 đến 1. Các đối tượng sáng như mây và tòa nhà màu trắng, cùng với các đối tượng tối như mặt nước, có thể gây ra các giá trị pixel bất thường trong hình ảnh EVI. Trước khi tạo hình ảnh EVI, bạn nên loại bỏ mây và các đối tượng sáng khỏi hình ảnh phản xạ, và tùy chọn đặt ngưỡng cho các giá trị pixel từ 0 đến 1.

_Tham khảo: Huete, A., et al. &quot;Tổng quan về hiệu suất phóng xạ và sinh lý của các chỉ số thảm thực vật MODIS.&quot; Remote Sensing of Environment 83 (2002):195–213._

***

## FCI1 - Chỉ số phủ rừng 1

_Chỉ có trên giao diện người dùng (GUI) — không có sẵn dưới dạng cài đặt sẵn CLI/SDK `--indices`._

Chỉ số này phân biệt tán rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ bao gồm dải biên đỏ.

$$
FCI1 = Red * RedEdge
$$

Các khu vực có rừng sẽ có giá trị FCI1 thấp hơn do độ phản xạ của cây cối thấp hơn và sự hiện diện của bóng râm bên trong tán rừng.

_Tài liệu tham khảo: Becker, Sarah J., Craig S.T. Daughtry, và Andrew L. Russ. &quot;Các chỉ số che phủ rừng đáng tin cậy cho hình ảnh đa quang phổ.&quot; Photogrammetric Engineering &amp; Remote Sensing 84.8 (2018): 505-512._

***

## FCI2 - Chỉ số che phủ rừng 2

_Chỉ có trên giao diện người dùng (GUI) — không có sẵn dưới dạng cài đặt sẵn CLI/SDK `--indices`._

Chỉ số này phân biệt tán rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ không bao gồm dải tần số &quot;red edge&quot;.

$$
FCI2 = Red * NIR
$$

Các khu vực có rừng sẽ có giá trị FCI2 thấp hơn do độ phản xạ của cây cối thấp hơn và sự hiện diện của bóng râm bên trong tán rừng.

_Tài liệu tham khảo: Becker, Sarah J., Craig S.T. Daughtry, và Andrew L. Russ. &quot;Các chỉ số phủ rừng bền vững cho hình ảnh đa phổ.&quot; Photogrammetric Engineering &amp; Remote Sensing 84.8 (2018): 505-512._

***

## GEMI - Chỉ số Giám sát Môi trường Toàn cầu

_Chỉ dành cho GUI — không có sẵn dưới dạng cài đặt sẵn CLI/SDK `--indices`._

Chỉ số thảm thực vật phi tuyến tính này được sử dụng để giám sát môi trường toàn cầu từ hình ảnh vệ tinh và cố gắng hiệu chỉnh các tác động của khí quyển. Nó tương tự như NDVI nhưng ít nhạy cảm hơn với các tác động của khí quyển. Chỉ số này bị ảnh hưởng bởi đất trống; do đó, không nên sử dụng ở những khu vực có thảm thực vật thưa thớt hoặc có mật độ trung bình.

$$
GEMI = eta (1 - 0.25 * eta) - {Red - 0.125 \over 1 - Red}
$$

Trong đó:

$$
eta = {2(NIR^{2}-Red^{2}) + 1.5 * NIR + 0.5 *  Red \over NIR + Red + 0.5}
$$

_Tham khảo: Pinty, B., và M. Verstraete. “GEMI: một chỉ số phi tuyến tính để giám sát thảm thực vật toàn cầu từ vệ tinh”. Tạp chí Vegetation 101 (1992): 15-20._

***

## GARI - Chỉ số Green Atmospherically Resistant

_Chỉ có trên GUI — không có sẵn dưới dạng cài đặt sẵn CLI/SDK `--indices`._

Chỉ số này nhạy cảm hơn với phạm vi rộng các nồng độ diệp lục và ít nhạy cảm hơn với các tác động khí quyển so với NDVI.

$$
GARI = {NIR - [Green - \gamma(Blue - Red)] \over NIR + [Green - \gamma(Blue - Red)]   }
$$

Hằng số gamma là một hàm trọng số phụ thuộc vào điều kiện hạt khí trong khí quyển. ENVI sử dụng giá trị 1,7, đây là giá trị được khuyến nghị bởi Gitelson, Kaufman và Merzylak (1996, trang 296).

_Tài liệu tham khảo: Gitelson, A., Y. Kaufman và M. Merzylak. &quot;Sử dụng Kênh Xanh trong Viễn thám Thực vật Toàn cầu từ EOS-MODIS.&quot; Remote Sensing of Environment 58 (1996): 289-298._

***

## GCI - Chỉ số diệp lục màu Green

Chỉ số này được sử dụng để ước tính hàm lượng diệp lục trong lá trên một phạm vi rộng các loài thực vật.

$$
GCI = {NIR \over Green} - 1
$$

Việc sử dụng dải bước sóng hồng ngoại gần (NIR) và xanh lá cây rộng giúp dự đoán hàm lượng diệp lục chính xác hơn, đồng thời mang lại độ nhạy cao hơn và tỷ lệ tín hiệu trên nhiễu (SNR) tốt hơn.

_Tài liệu tham khảo: Gitelson, A., Y. Gritz, và M. Merzlyak. &quot;Mối quan hệ giữa hàm lượng diệp lục trong lá và độ phản xạ quang phổ, cùng các thuật toán đánh giá diệp lục không phá hủy trên lá thực vật bậc cao.&quot; Tạp chí Sinh lý Thực vật 160 (2003): 271-282._

***

## GLI - Chỉ số Lá Xanh (Green Leaf Index)

Chỉ số này ban đầu được thiết kế để sử dụng với máy ảnh kỹ thuật số RGB nhằm đo độ che phủ của lúa mì, trong đó các giá trị số kỹ thuật số (DN) của các kênh đỏ, xanh lá và xanh dương dao động từ 0 đến 255.

$$
GLI = {(Green - Red) + (Green - Blue)  \over (2 * Green) + Red + Blue }
$$

Giá trị GLI dao động từ -1 đến +1. Các giá trị âm đại diện cho đất và các đặc điểm không sống, trong khi các giá trị dương đại diện cho lá và thân cây xanh.

_Tài liệu tham khảo: Louhaichi, M., M. Borman và D. Johnson. &quot;Nền tảng định vị không gian và ảnh chụp từ trên không để ghi chép tác động của việc chăn thả đối với lúa mì.&quot; Geocarto International 16, Số 1 (2001): 65-70._

***

## GNDVI - Chỉ số Thực vật Chênh lệch Chuẩn hóa Màu Xanh (Green)

Chỉ số này tương tự như NDVI, ngoại trừ việc nó đo dải quang phổ xanh từ 540 đến 570 nm thay vì dải quang phổ đỏ. Chỉ số này nhạy cảm hơn với nồng độ diệp lục so với NDVI.

$$
GNDVI = {(NIR - Green) \over (NIR + Green)  }
$$

_Tài liệu tham khảo: Gitelson, A., và M. Merzlyak. &quot;Viễn thám nồng độ diệp lục trong lá thực vật bậc cao.&quot; Advances in Space Research 22 (1998): 689-692._

***

## GOSAVI - Chỉ số Thực vật Tối ưu hóa Dải Xanh (Green) Điều chỉnh theo Đất

Chỉ số này ban đầu được thiết kế dựa trên ảnh hồng ngoại màu để dự đoán nhu cầu nitơ cho ngô. Nó tương tự như OSAVI, nhưng thay thế dải màu đỏ bằng dải màu xanh.

$$
GOSAVI = {NIR - Green \over NIR + Green + 0.16)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. &quot;Xác định nhu cầu nitơ trong mùa vụ cho ngô bằng phương pháp chụp ảnh màu-hồng ngoại từ trên không.&quot; Luận án tiến sĩ, Đại học Bang North Carolina, 2005._

***

## GRVI - Chỉ số Thực vật Tỷ lệ Màu Xanh (Green Ratio Vegetation Index)

Chỉ số này nhạy cảm với tốc độ quang hợp trong tán rừng, vì độ phản xạ màu xanh lá cây và màu đỏ bị ảnh hưởng mạnh bởi sự thay đổi sắc tố lá.

$$
GRVI = {NIR \over Green }
$$

_Tài liệu tham khảo: Sripada, R., et al. &quot;Chụp ảnh màu hồng ngoại từ trên không để xác định nhu cầu nitơ giai đoạn đầu mùa vụ của ngô.&quot; Tạp chí Agronomy 98 (2006): 968-977._

***

## GSAVI - Chỉ số Thực vật Điều chỉnh theo Màu Xanh của Đất (Green Soil Adjusted Vegetation Index)

Chỉ số này ban đầu được thiết kế dựa trên kỹ thuật chụp ảnh màu hồng ngoại để dự đoán nhu cầu nitơ cho ngô. Nó tương tự như SAVI, nhưng thay thế dải màu đỏ bằng dải màu xanh.

$$
GSAVI = 1.5 * {(NIR - Green) \over (NIR + Green + 0.5)  }
$$

_Tài liệu tham khảo: Sripada, R., và cộng sự. &quot;Xác định nhu cầu nitơ trong mùa vụ của ngô bằng phương pháp chụp ảnh màu hồng ngoại từ trên không.&quot; Luận án tiến sĩ, Đại học Bang North Carolina, 2005._

***

## LAI - Chỉ số diện tích lá

Chỉ số này được sử dụng để ước tính độ che phủ của tán lá và dự báo sự sinh trưởng cũng như năng suất của cây trồng. ENVI tính toán LAI màu xanh lá cây bằng công thức thực nghiệm sau đây của Boegh et al (2002):

$$
LAI = 3.618 * EVI - 0.118
$$

Trong đó, EVI là:

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Các giá trị LAI cao thường dao động trong khoảng từ khoảng 0 đến 3,5. Tuy nhiên, khi cảnh quan chứa mây và các đặc điểm sáng khác gây ra các pixel bão hòa, các giá trị LAI có thể vượt quá 3,5. Tốt nhất bạn nên loại bỏ mây và các đặc điểm sáng khỏi cảnh quan của mình trước khi tạo hình ảnh LAI.

_Tài liệu tham khảo: Boegh, E., H. Soegaard, N. Broge, C. Hasager, N. Jensen, K. Schelde, và A. Thomsen. &quot;Dữ liệu đa phổ từ máy bay để định lượng Chỉ số Diện tích Lá, Nồng độ Nitơ và Hiệu suất Quang hợp trong Nông nghiệp.&quot; Remote Sensing of Environment 81, số 2-3 (2002): 179-193._

***

## Chỉ số Lá Diệp lục (LCI)

_Chỉ có trên GUI — không có sẵn dưới dạng cài đặt sẵn CLI/SDK `--indices`._

Chỉ số này được sử dụng để ước tính hàm lượng diệp lục trong thực vật bậc cao, nhạy cảm với sự biến đổi độ phản xạ do sự hấp thụ của diệp lục gây ra.

$$
LCI = {NIR2 - RedEdge \over NIR2 + Red}
$$

_Tài liệu tham khảo: Datt, B. &quot;Phân tích từ xa hàm lượng nước trong lá cây bạch đàn.&quot; Tạp chí Sinh lý Thực vật 154, số 1 (1999): 30-36._

***

## MNLI - Chỉ số phi tuyến tính đã điều chỉnh

Chỉ số này là phiên bản cải tiến của Chỉ số phi tuyến (NLI), kết hợp Chỉ số Thực vật Điều chỉnh Đất (SAVI) để tính đến nền đất. ENVI sử dụng giá trị hệ số điều chỉnh nền tán lá (_L_) là 0,5.

$$
MNLI = {(NIR^{2} - Red) * (1 + L) \over (NIR^{2} + Red + L)  }
$$

_Tài liệu tham khảo: Yang, Z., P. Willis, và R. Mueller. &quot;Tác động của hình ảnh AWIFS được cải thiện tỷ lệ dải phổ đối với độ chính xác phân loại cây trồng.&quot; Kỷ yếu Hội nghị chuyên đề Viễn thám Pecora 17 (2008), Denver, CO._

***

## MSAVI2 - Chỉ số Thực vật Điều chỉnh theo Đất 2 (Modified Soil Adjusted Vegetation Index 2)

Chỉ số này là phiên bản đơn giản hơn của chỉ số MSAVI do Qi và cộng sự (1994), nhằm cải thiện Chỉ số Thực vật Điều chỉnh theo Đất (SAVI). Chỉ số này giảm nhiễu từ đất và tăng dải động của tín hiệu thực vật. MSAVI2 dựa trên phương pháp suy diễn, không sử dụng giá trị _L_ cố định (như trong SAVI) để làm nổi bật thực vật khỏe mạnh.

$$
MSAVI2 = {2 * NIR + 1 - \sqrt{(2 * NIR + 1)^{2} - 8(NIR - Red)} \over 2}
$$

_Tài liệu tham khảo: Qi, J., A. Chehbouni, A. Huete, Y. Kerr, và S. Sorooshian. &quot;Chỉ số Thực vật Điều chỉnh theo Đất đã được Sửa đổi.&quot; Remote Sensing of Environment 48 (1994): 119-126._

***

## MSR - Tỷ lệ đơn giản đã được điều chỉnh

Chỉ số này là phiên bản cải tiến của tỷ lệ đơn giản NIR/Red, được thiết kế để tuyến tính hóa mối quan hệ của nó với các thông số sinh lý, và nhạy cảm hơn so với NDVI ở mật độ thảm thực vật cao hơn.

$$
MSR = {(NIR / Red) - 1 \over \sqrt{NIR / Red} + 1}
$$

_Tài liệu tham khảo: Chen, J. &quot;Đánh giá các chỉ số thảm thực vật và Tỷ lệ đơn giản cải tiến cho các ứng dụng ở vùng ôn đới.&quot; Tạp chí Viễn thám Canada 22 (1996): 229-242._

***

## NDRE - Chỉ số Chênh lệch Chuẩn hóa RedEdge

Chỉ số này tương tự như NDVI nhưng so sánh độ tương phản giữa NIR và RedEdge thay vì Red, điều này thường phát hiện tình trạng căng thẳng của thảm thực vật sớm hơn.

$$
NDRE = {NIR - RedEdge \over NIR + RedEdge  }
$$

***

## NDVI - Chỉ số Chênh lệch Chuẩn hóa Thực vật

Chỉ số này là thước đo mức độ khỏe mạnh của thảm thực vật xanh. Sự kết hợp giữa công thức chênh lệch chuẩn hóa và việc sử dụng các vùng hấp thụ và phản xạ cao nhất của chất diệp lục giúp chỉ số này hoạt động ổn định trong nhiều điều kiện khác nhau. Tuy nhiên, nó có thể bị bão hòa trong điều kiện thảm thực vật dày đặc khi chỉ số LAI tăng cao.

$$
NDVI = {NIR - Red \over NIR + Red  }
$$

Giá trị của chỉ số này dao động từ -1 đến 1. Phạm vi thông thường cho thảm thực vật xanh là từ 0,2 đến 0,8.

_Tài liệu tham khảo: Rouse, J., R. Haas, J. Schell, và D. Deering. Giám sát các hệ thống thảm thực vật ở vùng Great Plains bằng ERTS. Hội nghị chuyên đề ERTS lần thứ ba, NASA (1973): 309-317._

***

## NLI - Chỉ số phi tuyến tính

Chỉ số này giả định rằng mối quan hệ giữa nhiều chỉ số thảm thực vật và các thông số sinh lý-vật lý bề mặt là phi tuyến tính. Nó tuyến tính hóa các mối quan hệ với các thông số bề mặt vốn có xu hướng phi tuyến tính.

$$
NLI = {NIR^{2} - Red \over NIR^{2} + Red  }
$$

_Tài liệu tham khảo: Goel, N., và W. Qin. &quot;Ảnh hưởng của cấu trúc tán cây đối với mối quan hệ giữa các chỉ số thảm thực vật khác nhau với LAI và Fpar: Một mô phỏng máy tính.&quot; Tạp chí Đánh giá Viễn thám 10 (1994): 309-347._

***

## OSAVI - Chỉ số Thực vật Được Điều chỉnh theo Đất Tối ưu

Chỉ số này dựa trên Chỉ số Thực vật Được Điều chỉnh theo Đất (SAVI). Nó sử dụng giá trị tiêu chuẩn là 0,16 cho hệ số điều chỉnh nền tán cây. Rondeaux (1996) đã xác định rằng giá trị này mang lại sự biến thiên về đất cao hơn so với SAVI đối với vùng có độ che phủ thực vật thấp, đồng thời thể hiện độ nhạy cao hơn đối với độ che phủ thực vật trên 50%. Chỉ số này phù hợp nhất để sử dụng ở những khu vực có thảm thực vật tương đối thưa thớt, nơi đất có thể nhìn thấy qua tán cây.

$$
OSAVI = {(NIR - Red) \over (NIR + Red + 0.16)  }
$$

_Tài liệu tham khảo: Rondeaux, G., M. Steven, và F. Baret. &quot;Tối ưu hóa các chỉ số thực vật được điều chỉnh theo đất.&quot; Remote Sensing of Environment 55 (1996): 95-107._

***

## RDVI - Chỉ số Thực vật Chênh lệch Tái chuẩn hóa

Chỉ số này sử dụng sự chênh lệch giữa các bước sóng hồng ngoại gần và đỏ, kết hợp với NDVI, để làm nổi bật thảm thực vật khỏe mạnh. Nó không bị ảnh hưởng bởi tác động của đất và góc quan sát so với mặt trời.

$$
RDVI = {(NIR- Red) \over \sqrt{(NIR + Red)}  }
$$

_Tài liệu tham khảo: Roujean, J., và F. Breon. &quot;Ước tính PAR được thực vật hấp thụ từ các phép đo phản xạ hai chiều.&quot; Remote Sensing of Environment 51 (1995): 375-384._

***

## SAVI - Chỉ số Thực vật Được Điều chỉnh theo Đất

Chỉ số này tương tự như NDVI, nhưng nó loại bỏ ảnh hưởng của các điểm ảnh đất. Chỉ số này sử dụng hệ số điều chỉnh nền tán cây, _L_, là hàm của mật độ thực vật và thường yêu cầu kiến thức trước về lượng thực vật. Huete (1988) đề xuất giá trị tối ưu của _L_ là 0,5 để tính đến các biến động nền đất bậc nhất. Chỉ số này phù hợp nhất để sử dụng ở những khu vực có thảm thực vật tương đối thưa thớt, nơi đất có thể nhìn thấy qua tán cây.

$$
SAVI = {1.5 * (NIR- Red) \over (NIR + Red + 0.5)  }
$$

_Tài liệu tham khảo: Huete, A. &quot;Chỉ số Thực vật Được Điều chỉnh theo Đất (SAVI).&quot; Remote Sensing of Environment 25 (1988): 295-309._

***

## TDVI - Chỉ số Thực vật Chênh lệch Biến đổi

Chỉ số này hữu ích cho việc giám sát độ che phủ thực vật trong môi trường đô thị. Nó không bị bão hòa như NDVI và SAVI.

$$
TDVI = 1.5 * {(NIR- Red) \over \sqrt{NIR^{2} + Red + 0.5}  }
$$

_Tài liệu tham khảo: Bannari, A., H. Asalhi, và P. Teillet. “Chỉ số Thực vật Chênh lệch Biến đổi (TDVI) để Lập bản đồ Mật độ Thực vật” Trong Kỷ yếu Hội nghị Khoa học Địa chất và Viễn thám, IGARSS &#x27;02, IEEE International, Tập 5 (2002)._

***

## VARI - Chỉ số Kháng ảnh Khí quyển trong dải quang phổ nhìn thấy

Chỉ số này dựa trên ARVI và được sử dụng để ước tính tỷ lệ thảm thực vật trong một cảnh với độ nhạy thấp đối với các tác động của khí quyển.

$$
VARI = {Green - Red \over Green + Red - Blue  }
$$

_Tài liệu tham khảo: Gitelson, A., và cộng sự. &quot;Các đường phân cách thực vật và đất trong không gian quang phổ ánh sáng khả kiến: Một khái niệm và kỹ thuật để ước tính từ xa tỷ lệ thực vật.&quot; Tạp chí Quốc tế về Viễn thám 23 (2002): 2537−2562._

***

## WDRVI - Chỉ số Thực vật Dải Động Rộng

Chỉ số này tương tự như NDVI, nhưng sử dụng hệ số trọng số (_a_) để giảm sự chênh lệch giữa đóng góp của tín hiệu hồng ngoại gần và tín hiệu đỏ vào NDVI. WDRVI đặc biệt hiệu quả trong các cảnh có mật độ thực vật từ trung bình đến cao khi NDVI vượt quá 0,6. NDVI có xu hướng đạt đến mức ổn định khi tỷ lệ thảm thực vật và chỉ số diện tích lá (LAI) tăng lên, trong khi WDRVI nhạy cảm hơn với phạm vi rộng hơn của tỷ lệ thảm thực vật và với những thay đổi của LAI.

$$
WDRVI = {(\alpha * NIR- Red) \over (\alpha * NIR + Red)}
$$

Hệ số trọng số (_a_) có thể dao động từ 0,1 đến 0,2. Giá trị 0,2 được Henebry, Viña và Gitelson (2004) khuyến nghị.

_Tài liệu tham khảo_

_Gitelson, A. &quot;Chỉ số Thực vật Dải Động Rộng để Định lượng Từ xa các Đặc tính Sinh lý-Vật lý của Thực vật.&quot; Tạp chí Sinh lý Thực vật 161, Số 2 (2004): 165-173._

_Henebry, G., A. Viña và A. Gitelson. &quot;Chỉ số Thực vật Dải Động Rộng và tiềm năng ứng dụng của nó trong phân tích khoảng trống.&quot; Tạp chí Phân tích Khoảng trống 12: 50-56._
