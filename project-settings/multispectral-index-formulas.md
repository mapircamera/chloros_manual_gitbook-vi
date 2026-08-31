---
description: This page lists some multispectral indices that Chloros uses
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/multispectral-index-formulas
---

# Công thức chỉ số đa quang phổ

Các công thức chỉ số dưới đây sử dụng sự kết hợp của các dải truyền trung bình của bộ lọc Survey3:

<table><thead><tr><th align="center">Survey3 Màu bộ lọc</th><th width="196.199951171875" align="center">Survey3 Tên bộ lọc</th><th width="159.800048828125" align="center">Phạm vi truyền qua (FWHM)</th><th align="center">Độ truyền qua trung bình</th></tr></thead><tbody><tr><td align="center">Blue</td><td align="center">NGB - Blue</td><td align="center">468-483nm</td><td align="center">475 nm</td></tr><tr><td align="center">Cyan</td><td align="center">OCN- Cyan</td><td align="center">476–512 nm</td><td align="center">494 nm</td></tr><tr><td align="center">Green</td><td align="center">RGN | NGB - Green</td><td align="center">543–558 nm</td><td align="center">547 nm</td></tr><tr><td align="center">Orange</td><td align="center">OCN - Orange</td><td align="center">598–640 nm</td><td align="center">619 nm</td></tr><tr><td align="center">Red</td><td align="center">RGN - Red</td><td align="center">653–668 nm</td><td align="center">661 nm</td></tr><tr><td align="center">RedEdge</td><td align="center">Re-RedEdge</td><td align="center">712–735 nm</td><td align="center">724 nm</td></tr><tr><td align="center">NIR1</td><td align="center">OCN - NIR1</td><td align="center">798–848 nm</td><td align="center">823 nm</td></tr><tr><td align="center">NIR2</td><td align="center">RGN | NGB | NIR - NIR2</td><td align="center">835–865 nm</td><td align="center">850 nm</td></tr></tbody></table>

Khi sử dụng các công thức này, tên có thể kết thúc bằng &quot;\_1&quot; hoặc &quot;\_2&quot;, tương ứng với bộ lọc NIR nào đã được sử dụng, là NIR1 hay NIR2.

Đối với máy ảnh LATTICE M3C (bộ lọc ba dải tần Bayer), cùng một bộ xử lý chỉ số sử dụng các dải tần của bộ lọc M3C:

| Bộ lọc M3C | Dải 1 (trung tâm/ FWHM) | Dải 2 (trung tâm/ FWHM) | Dải 3 (trung tâm/ FWHM) |
| --- | --- | --- | --- |
| FRGB | Blue 475 nm / 30 nm | Green 550 nm / 30 nm | Red 625 nm / 30 nm |
| FRGN | Red 660 nm / 21 nm | Green 550 nm / 30 nm | NIR 850 nm / 30 nm |
| FOCN | Orange 615 nm / 21 nm | Cyan 490 nm / 38 nm | NIR 808 nm / 14 nm |
| FNGB | Blue 475 nm / 30 nm | Green 550 nm / 30 nm | NIR 850 nm / 30 nm |

Các camera LATTICE M3M là loại đơn dải (mỗi camera có một bộ lọc dải hẹp), do đó các chỉ số đa dải không được tính toán cho một hình ảnh M3M đơn lẻ. Để tính toán các chỉ số với M3M, hãy kết hợp hai hoặc nhiều máy ảnh thành một chồng ảnh đa dải đã được căn chỉnh và sử dụng công cụ tính chỉ số LATTICE (`chloros-cli lattice index`, hoặc Trình tính chỉ số trực tiếp trên giao diện người dùng).

***

## Tên chỉ số nào hoạt động ở đâu

Chloros có **ba** bề mặt chỉ số, và danh sách cài đặt sẵn của chúng không giống nhau. Sử dụng phần này để kiểm tra xem một tên chỉ số có hoạt động ở nơi bạn dự định sử dụng hay không.

| Bạn đang ở đâu | Danh sách nào áp dụng | Số lượng |
| --- | --- | --- |
| Cài đặt Dự án → Chỉ số → Thêm chỉ số (GUI) | Bề mặt 1 | 27 |
| Trình xem hình ảnh [Hộp cát Chỉ số/LUT](../image-viewer-gui/index-lut-sandbox.md) (GUI) | Bề mặt 1 | 27 |
| `chloros-cli process --indices NDVI,NDRE` | Bề mặt 2 | 22 |
| SDK `process_folder(indices=[...])` | Bề mặt 2 | 22 |
| `chloros-cli lattice index --preset` | Bề mặt 3 | 22 (một giá trị 22 khác) |
| Trình tính chỉ số trực tiếp trên tab Máy ảnh | Bề mặt 3 | 22 (một giá trị 22 khác) |

Surface 1 và 2 xử lý **một hình ảnh tại một thời điểm từ một camera**, sử dụng các khe biểu tượng `x`/`y`/`z`(/`a`) được liên kết với các kênh bộ lọc của camera đó. Surface 3 hoạt động trên một**bộ chồng đa dải đã được căn chỉnh** — nhiều camera LATTICE được căn chỉnh chung thành một khối — và tham chiếu các kênh bằng tên viết thường.

### 1. Cài đặt Dự án GUI / Menu thả xuống &quot;Sandbox&quot; của Trình xem hình ảnh — 27 công thức

Menu thả xuống liệt kê các công thức theo thứ tự này (đây là thứ tự chèn, không phải thứ tự theo bảng chữ cái):

`NDVI, GNDVI, CVI, ENDVI, EVI, MSR, OSAVI, TDVI, LAI, FCI1, FCI2, GARI, GCI, GEMI, GLI, GOSAVI, GRVI, GSAVI, LCI, MNLI, MSAVI2, NDRE, NLI, RDVI, SAVI, VARI, WDRVI`

Trong GUI, bạn kéo các kênh bộ lọc của camera vào các ô dải tần của công thức, do đó bất kỳ công thức nào cũng có thể được sử dụng với bất kỳ cách gán dải tần nào mà camera của bạn hỗ trợ. Các công thức tùy chỉnh mà bạn đã lưu sẽ được thêm vào phía dưới danh sách này.

**Năm công thức chỉ dành cho GUI** — những công thức mà danh sách CLI / SDK `--indices` không chấp nhận — được triển khai như sau:

| Cài đặt sẵn chỉ dành cho GUI | Công thức (theo cách triển khai) | Ô |
| --- | --- | --- |
| FCI1 | `x*y` | x, y |
| FCI2 | `x*y` | x, y |
| GARI | `(y-(x-1.7*(z-a)))/(y+(x-1.7*(z-a)))` | x, y, z, a (bốn ô) |
| GEMI | `((2*(y*y-x*x)+1.5*y+0.5*x)/(y+x+0.5))*(1-0.25*((2*(y*y-x*x)+1.5*y+0.5*x)/(y+x+0.5)))-((x-0.125)/(1-x))` | x, y |
| LCI | `(y-x)/(y+z)` | x, y, z |

Cách ánh xạ dự kiến cho từng công thức được trình bày trong phần riêng biệt ở phía dưới trang này (ví dụ: GARI dự kiến x= Green, y= NIR, z= Blue, a= Red). GARI là công thức duy nhất trong Chloros sử dụng khe thứ tư.

### 2. CLI / SDK Mở rộng tên `--indices` — 22 cài đặt sẵn

Tùy chọn `chloros-cli process --indices` (và tham số `indices` trong SDK) chấp nhận các tên cài đặt sẵn sau:

`NDVI, GNDVI, NDRE, OSAVI, SAVI, MSAVI2, EVI, MSR, TDVI, LAI, GCI, GRVI, GSAVI, GOSAVI, NLI, MNLI, RDVI, WDRVI, CVI, ENDVI, GLI, VARI`

{% hint style="warning" %}
**Các tên chỉ mục không xác định sẽ bị bỏ qua mà không có thông báo.** Tên nằm ngoài danh sách này (bao gồm năm công thức chỉ dành cho GUI là `FCI1`, `FCI2`, `GARI`, `GEMI`, `LCI` và bất kỳ công thức tùy chỉnh nào bạn đã lưu trong giao diện đồ họa) sẽ bị loại bỏ kèm theo một thông báo trong nhật ký — quá trình chạy tiếp tục mà không có chỉ mục đó, và bản thân quá trình chạy vẫn báo cáo thành công. Thông báo được hiển thị như sau:

```
[INDEX_EXPAND] skipping unknown preset 'LCI'; known: ['CVI', 'ENDVI', 'EVI', ...]
```

Tên được so khớp mà không phân biệt chữ hoa chữ thường sau khi loại bỏ khoảng trắng, do đó `ndvi`, `NDVI` và ` NDVI ` được coi là cùng một cài đặt sẵn. Một cài đặt sẵn cũng sẽ bị bỏ qua nếu nó yêu cầu dải phổ mà bộ lọc của máy ảnh không hỗ trợ.
{% endhint %}

Các công thức chính xác như đã triển khai (các ký hiệu `x`/`y`/`z` là các khe dải; bản đồ mặc định được hiển thị cho từng cài đặt sẵn):

| Cài đặt sẵn | Công thức (như đã triển khai) | Bộ lọc mặc định | Vị trí dải (x, y, z) |
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
| CVI | `(z/y)/(x/y)` | RGB | Red, Green, Blue |
| ENDVI | `((x+y)-(2*z))/((x+y)+(2*z))` | RGB | Red, Green, Blue |
| GLI | `((y-x)+(y-z))/((2*y)+x+z)` | RGB | Red, Green, Blue |
| VARI | `(y-x)/(y+x-z)` | RGB | Red, Green, Blue |

#### Cách tên preset được chuyển đổi thành vị trí dải tần

Khi bạn truyền một tên trần như `NDVI`, Chloros phải xác định kênh nào của tệp nào mà mỗi ký hiệu tương ứng. Nó sử dụng bảng này, trong đó ánh xạ mã bộ lọc đến vị trí mảng của từng kênh:

| Mã bộ lọc | Kênh → chỉ số mảng |
| --- | --- |
| OCN | Orange 0, Cyan 1, NIR 2 (`Red` được chấp nhận là tên gọi khác của Orange, cũng là 0) |
| RGN | Red 0, Green 1, NIR 2 |
| NGB | NIR 0, Green 1, Blue 2 |
| RGB | Red 0, Green 1, Blue 2 |
| RE | RE 0 |
| NIR | NIR 0 |

**Bộ lọc mặc định** của cài đặt sẵn (cột &quot;Bộ lọc mặc định&quot; ở trên) được sử dụng khi dự án chứa các hình ảnh có bộ lọc đó. Nếu không, Chloros sẽ quét các bộ lọc thực sự có trong dự án theo thứ tự `RGN, OCN, NGB, RGB, RE, NIR` và chọn bộ lọc đầu tiên có thể cung cấp mọi kênh mà cài đặt sẵn cần. Nếu không có bộ lọc nào đáp ứng được, cài đặt sẵn sẽ bị loại bỏ cho lần chạy đó. Đây là lý do tại sao yêu cầu `NDVI` trên tập dữ liệu chỉ chứa &quot;OCN&quot; vẫn cho kết quả hợp lý — nó liên kết với các vị trí Orange và NIR của OCN.

Các chuỗi mô hình LATTICE M3C mang bộ lọc với tiền tố `F` (`LATT-M3C-L41-FRGN`), nhưng tiền tố này sẽ bị loại bỏ khi mã bộ lọc được đọc từ hình ảnh, do đó máy ảnh FRGN sẽ nhận diện thông qua hàng `RGN` ở trên và không cần xử lý đặc biệt.

### 3. Công cụ lập chỉ mục LATTICE (`lattice index --preset`, Trình tính toán chỉ mục trực tiếp) — 22 cài đặt sẵn

Bộ xử lý LATTICE hoạt động trên các chồng ảnh đa dải đã được căn chỉnh (mảng trực tiếp hoặc tệp TIFF đa dải đã xuất) và sử dụng tên kênh viết thường (`red`, `green`, `blue`, `red_edge`, `nir`). Danh sách cài đặt sẵn của nó khác với hai bộ trên:

| Cài đặt sẵn | Công thức | Kênh |
| --- | --- | --- |
| NDVI | `(nir - red) / (nir + red)` | đỏ, nir |
| GNDVI | `(nir - green) / (nir + green)` | xanh lá, nir |
| BNDVI | `(nir - blue) / (nir + blue)` | xanh dương, hồng ngoại |
| NDRE | `(nir - red_edge) / (nir + red_edge)` | đỏ\_viền, hồng ngoại |
| ENDVI | `((nir + green) - 2*blue) / ((nir + green) + 2*blue)` | xanh dương, xanh lá, hồng ngoại |
| SAVI | `1.5 * (nir - red) / (nir + red + 0.5)` | đỏ, hồng ngoại |
| OSAVI | `1.5 * (nir - red) / (nir + red + 0.16)` | đỏ, hồng ngoại |
| MSAVI | `(2*nir + 1 - sqrt((2*nir + 1)**2 - 8*(nir - red))) / 2` | đỏ, hồng ngoại |
| EVI | `2.5 * (nir - red) / (nir + 6*red - 7.5*blue + 1)` | xanh dương, đỏ, hồng ngoại |
| EVI2 | `2.5 * (nir - red) / (nir + 2.4*red + 1)` | đỏ, hồng ngoại |
| CVI | `(nir / green) - (red / green)` | đỏ, xanh lá, hồng ngoại |
| MSR | `((nir/red) - 1) / (sqrt(nir/red) + 1)` | đỏ, hồng ngoại |
| TDVI | `sqrt((nir - red) / (nir + red) + 0.5)` | đỏ, hồng ngoại |
| LAI | `3.618 * ((nir - red) / (nir + 6*red - 7.5*green + 1)) - 0.118` | đỏ, xanh lá, hồng ngoại |
| GLI | `(2*green - red - blue) / (2*green + red + blue)` | đỏ, xanh lá, xanh dương |
| NGRDI | `(green - red) / (green + red)` | đỏ, xanh lá |
| VARI | `(green - red) / (green + red - blue)` | đỏ, xanh lá, xanh dương |
| TGI | `green - 0.39*red - 0.61*blue` | đỏ, xanh lá, xanh dương |
| EXG | `2*green - red - blue` | đỏ, xanh lá, xanh dương |
| CIRE | `(nir / red_edge) - 1` | đỏ\_viền, hồng ngoại |
| CIGREEN | `(nir / green) - 1` | xanh lá, hồng ngoại |
| NDWI | `(green - nir) / (green + nir)` | xanh lá, hồng ngoại |

Chạy lệnh `chloros-cli lattice index --list-presets` để in bảng này từ bản cài đặt của bạn, và `--list-gradients` để xem các dải màu có sẵn. Các ký hiệu kênh phân biệt chữ hoa chữ thường và phải khớp với tên viết thường của các cài đặt sẵn (ví dụ: `--channel red=Red_660 --channel nir=NIR_850`).

***

## CVI

Như đã triển khai trong giao diện người dùng (GUI) và danh sách cài đặt sẵn tại CLI / SDK, CVI là công thức tỷ lệ của các tỷ lệ:

$$
CVI = {(z / y) \over (x / y)}
$$

với bản đồ kênh mặc định RGB: x= Red, y= Green, z= Blue. Trong giao diện người dùng (GUI), bạn có thể kéo bất kỳ kênh nào của máy ảnh vào các ô x/y/z. Lưu ý rằng cài đặt sẵn `CVI` của công cụ chỉ số LATTICE sử dụng công thức khác, `(NIR / Green) - (Red / Green)` — hãy kiểm tra các bảng ở trên để xác định công thức phù hợp với bề mặt bạn đang sử dụng.

***

## ENDVI - Chỉ số Thực vật Chênh lệch Chuẩn hóa Nâng cao

Chỉ số này sử dụng kênh màu xanh dương bên cạnh các kênh màu đỏ (NIR) và màu xanh lá (green), và được ưa chuộng với các máy ảnh được lọc bằng bộ lọc dải hồng ngoại (NGB), nơi dải màu xanh dương thay thế cho dải màu đỏ.

$$
ENDVI = {(NIR + Green) - (2 * Blue) \over (NIR + Green) + (2 * Blue)}
$$

Cách triển khai là công thức ký hiệu `((x+y)-(2*z))/((x+y)+(2*z))` — gán các kênh NIR và Green của máy ảnh vào các khe x/y và Blue vào khe z (đối với máy ảnh NGB: x= NIR, y= Green, z= Blue).

***

## Chỉ số Thực vật Nâng cao (EVI)

Chỉ số này ban đầu được phát triển để sử dụng với dữ liệu từ Hệ thống Quan sát Địa cầu (MODIS) như một cải tiến so với Chỉ số Thực vật (NDVI) bằng cách tối ưu hóa tín hiệu thực vật ở các khu vực có chỉ số diện tích lá cao (LAI). Nó đặc biệt hữu ích ở các khu vực có độ phản xạ xanh cao (LAI) nơi Chỉ số Phản xạ Xanh (NDVI) có thể bị bão hòa. Chỉ số này sử dụng vùng phản xạ màu xanh để hiệu chỉnh tín hiệu nền từ đất và giảm thiểu ảnh hưởng của khí quyển, bao gồm cả hiện tượng tán xạ aerosol.

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Các giá trị EVI nên nằm trong khoảng từ 0 đến 1 đối với các pixel thực vật. Các đối tượng sáng như mây và các tòa nhà màu trắng, cùng với các đối tượng tối như mặt nước, có thể dẫn đến các giá trị pixel bất thường trong hình ảnh EVI. Trước khi tạo hình ảnh chỉ số phủ thực vật (EVI), bạn nên loại bỏ mây và các đối tượng sáng khỏi hình ảnh phản xạ, và tùy chọn đặt ngưỡng giá trị pixel từ 0 đến 1.

_Tham khảo: Huete, A., et al. &quot;Tổng quan về hiệu suất phóng xạ và sinh lý của các chỉ số phủ thực vậtMODIS.&quot; Tạp chí Remote Sensing of Environment 83 (2002):195–213._

***

## Chỉ số phủ rừng (FCI1) - Chỉ số 1

_Chỉ có trên giao diện người dùng (GUI) — không có sẵn dưới dạng cài đặt sẵn (CLI / SDK) `--indices`._

Chỉ số này phân biệt tán rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ bao gồm dải tần số &quot;red edge&quot;.

$$
FCI1 = Red * RedEdge
$$

Các khu vực có rừng sẽ có giá trị chỉ số tán rừng (FCI1) thấp hơn do độ phản xạ của cây cối thấp hơn và sự hiện diện của bóng râm bên trong tán rừng.

_Tham khảo: Becker, Sarah J., Craig S.T. Daughtry, và Andrew L. Russ. &quot;Các chỉ số độ che phủ rừng đáng tin cậy cho hình ảnh đa quang phổ.&quot; Photogrammetric Engineering &amp; Remote Sensing 84.8 (2018): 505-512._

***

## FCI2 - Chỉ số phủ rừng 2

_Chỉ có trên giao diện người dùng (GUI) — không có sẵn dưới dạng cài đặt sẵn (CLI) / SDK `--indices`._

Chỉ số này phân biệt tán rừng với các loại thảm thực vật khác bằng cách sử dụng hình ảnh phản xạ đa phổ không bao gồm dải &quot;red edge&quot;.

$$
FCI2 = Red * NIR
$$

Các khu vực có rừng sẽ có giá trị &quot;FCI2&quot; thấp hơn do độ phản xạ của cây cối thấp hơn và sự hiện diện của bóng râm bên trong tán rừng.

_Tham khảo: Becker, Sarah J., Craig S.T. Daughtry, và Andrew L. Russ. &quot;Các chỉ số độ che phủ rừng ổn định cho hình ảnh đa phổ.&quot; Photogrammetric Engineering &amp; Remote Sensing 84.8 (2018): 505-512._

***

## Chỉ số Giám sát Môi trường Toàn cầu (GEMI)

_Chỉ có sẵn trong GUI — không có sẵn dưới dạng cài đặt sẵn CLI / SDK `--indices`._

Chỉ số thực vật phi tuyến tính này được sử dụng để giám sát môi trường toàn cầu từ hình ảnh vệ tinh và nhằm khắc phục các tác động của khí quyển. Chỉ số này tương tự như NDVI nhưng ít nhạy cảm hơn với các tác động của khí quyển. Chỉ số này bị ảnh hưởng bởi đất trống; do đó, không nên sử dụng chỉ số này ở các khu vực có thảm thực vật thưa thớt hoặc có mật độ vừa phải.

$$
GEMI = eta (1 - 0.25 * eta) - {Red - 0.125 \over 1 - Red}
$$

Trong đó:

$$
eta = {2(NIR^{2}-Red^{2}) + 1.5 * NIR + 0.5 *  Red \over NIR + Red + 0.5}
$$

_Tham khảo: Pinty, B., và M. Verstraete. Chỉ số Mức độ Phản xạ Thực vật (GEMI): Một chỉ số phi tuyến tính để giám sát thảm thực vật toàn cầu từ vệ tinh. Tạp chí Vegetation 101 (1992): 15-20._

***

## Chỉ số Kháng ảnh hưởng khí quyển (GARI) - Chỉ số Kháng ảnh hưởng khí quyển (Green)

_Chỉ có trên giao diện người dùng (GUI) — không có sẵn dưới dạng tệp cài đặt sẵn (CLI) / SDK `--indices`._

Chỉ số này nhạy cảm hơn với phạm vi rộng các nồng độ diệp lục và ít nhạy cảm hơn với các tác động của khí quyển so với chỉ số Hệ số Màu Xanh (NDVI).

$$
GARI = {NIR - [Green - \gamma(Blue - Red)] \over NIR + [Green - \gamma(Blue - Red)]   }
$$

Hằng số gamma là một hàm trọng số phụ thuộc vào điều kiện của các hạt aerosol trong khí quyển. ENVI sử dụng giá trị 1,7, đây là giá trị được khuyến nghị bởi Gitelson, Kaufman và Merzylak (1996, trang 296).

_Tài liệu tham khảo: Gitelson, A., Y. Kaufman và M. Merzylak. &quot;Sử dụng kênh &#x27;Green&#x27; trong quan trắc từ xa thảm thực vật toàn cầu từ EOS-MODIS.&quot; Remote Sensing of Environment 58 (1996): 289-298._

***

## Chỉ số diệp lục GCI - Green

Chỉ số này được sử dụng để ước tính hàm lượng diệp lục trong lá trên phạm vi rộng các loài thực vật.

$$
GCI = {NIR \over Green} - 1
$$

Việc sử dụng dải bước sóng rộng NIR và các bước sóng xanh giúp dự đoán hàm lượng diệp lục chính xác hơn, đồng thời tăng độ nhạy và tỷ lệ tín hiệu trên nhiễu.

_Tài liệu tham khảo: Gitelson, A., Y. Gritz, và M. Merzlyak. &quot;Mối quan hệ giữa hàm lượng diệp lục trong lá và độ phản xạ quang phổ, cùng các thuật toán đánh giá diệp lục không phá hủy trên lá thực vật cao cấp.&quot; Tạp chí Sinh lý Thực vật 160 (2003): 271-282._

***

## Chỉ số lá GLI - Green

Chỉ số này ban đầu được thiết kế để sử dụng với máy ảnh kỹ thuật số RGB nhằm đo độ che phủ của lúa mì, trong đó các giá trị số kỹ thuật số (DN) ở các dải màu đỏ, xanh lá cây và xanh dương dao động từ 0 đến 255.

$$
GLI = {(Green - Red) + (Green - Blue)  \over (2 * Green) + Red + Blue }
$$

Các giá trị GLI dao động từ -1 đến +1. Các giá trị âm biểu thị đất và các đặc điểm phi sinh vật, trong khi các giá trị dương biểu thị lá và thân cây xanh.

_Tài liệu tham khảo: Louhaichi, M., M. Borman và D. Johnson. &quot;Nền tảng định vị không gian và ảnh chụp từ trên không để ghi chép tác động của việc chăn thả đối với lúa mì.&quot; Geocarto International 16, Số 1 (2001): 65-70._

***

## Chỉ số Thực vật Chênh lệch Chuẩn hóa (GNDVI) - Green

Chỉ số này tương tự như Chỉ số Thực vật Chênh lệch Chuẩn hóa (NDVI), ngoại trừ việc nó đo dải quang phổ xanh từ 540 đến 570 nm thay vì dải quang phổ đỏ. Chỉ số này nhạy cảm hơn với nồng độ diệp lục so với Chỉ số Thực vật Chuẩn hóa (NDVI).

$$
GNDVI = {(NIR - Green) \over (NIR + Green)  }
$$

_Tài liệu tham khảo: Gitelson, A., và M. Merzlyak. &quot;Viễn thám nồng độ diệp lục trong lá cây cao cấp.&quot; Advances in Space Research 22 (1998): 689-692._

***

## GOSAVI - Chỉ số Thực vật Điều chỉnh theo Đất được Tối ưu hóa (Green)

Chỉ số này ban đầu được thiết kế dựa trên ảnh hồng ngoại màu để dự đoán nhu cầu nitơ cho ngô. Nó tương tự như Chỉ số Thực vật Điều chỉnh theo Đất (OSAVI), nhưng thay thế dải màu xanh lá cây bằng dải màu đỏ.

$$
GOSAVI = {NIR - Green \over NIR + Green + 0.16)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. &quot;Xác định nhu cầu nitơ trong mùa vụ cho cây ngô bằng ảnh chụp từ trên không bằng kỹ thuật màu-hồng ngoại.&quot; Luận án tiến sĩ, Đại học Bang North Carolina, 2005._

***

## Chỉ số thực vật tỷ lệ GRVI - Green

Chỉ số này nhạy cảm với tốc độ quang hợp trong tán rừng, do độ phản xạ màu xanh lá cây và màu đỏ bị ảnh hưởng mạnh bởi sự thay đổi sắc tố lá.

$$
GRVI = {NIR \over Green }
$$

_Tài liệu tham khảo: Sripada, R., và cộng sự. &quot;Chụp ảnh hồng ngoại màu từ trên không để xác định nhu cầu nitơ trong giai đoạn đầu mùa vụ của ngô.&quot; Tạp chí Agronomy 98 (2006): 968-977._

***

## GSAVI - Chỉ số Thực vật Điều chỉnh theo Đất (Green)

Chỉ số này ban đầu được thiết kế dựa trên ảnh màu hồng ngoại để dự đoán nhu cầu nitơ cho ngô. Nó tương tự như Chỉ số Thực vật Điều chỉnh theo Đất (SAVI), nhưng thay thế dải màu xanh lá cây bằng dải màu đỏ.

$$
GSAVI = 1.5 * {(NIR - Green) \over (NIR + Green + 0.5)  }
$$

_Tài liệu tham khảo: Sripada, R., et al. &quot;Xác định nhu cầu nitơ trong mùa vụ cho ngô bằng cách sử dụng ảnh chụp từ trên không bằng kỹ thuật màu-hồng ngoại.&quot; Luận án tiến sĩ, Đại học Bang North Carolina, 2005._

***

## Chỉ số diện tích lá (LAI)

Chỉ số này được sử dụng để ước tính độ che phủ của tán lá và dự báo sự sinh trưởng và năng suất cây trồng. ENVI tính toán chỉ số diện tích lá (LAI) bằng công thức thực nghiệm sau đây của Boegh et al (2002):

$$
LAI = 3.618 * EVI - 0.118
$$

Trong đó, EVI là:

$$
EVI = 2.5 *  {(NIR - Red) \over (NIR + 6 * Red - 7.5 * Blue + 1)}
$$

Các giá trị chỉ số diện tích lá (LAI) cao thường dao động trong khoảng từ khoảng 0 đến 3,5. Tuy nhiên, khi cảnh quan chứa mây và các đặc điểm sáng khác gây ra các pixel bão hòa, các giá trị chỉ số diện tích lá (LAI) có thể vượt quá 3,5. Tốt nhất, bạn nên loại bỏ mây và các yếu tố sáng khỏi cảnh trước khi tạo hình ảnh chỉ số lá (LAI).

_Tham khảo: Boegh, E., H. Soegaard, N. Broge, C. Hasager, N. Jensen, K. Schelde, và A. Thomsen. &quot;Dữ liệu đa phổ từ máy bay để định lượng Chỉ số Diện tích Lá (LAI), Nồng độ Nitơ và Hiệu suất Quang hợp trong Nông nghiệp.&quot; Remote Sensing of Environment 81, số 2-3 (2002): 179-193._

***

## LCI - Chỉ số diệp lục lá

_Chỉ có trên giao diện người dùng (GUI) — không có sẵn dưới dạng cài đặt sẵn CLI / SDK `--indices`._

Chỉ số này được sử dụng để ước tính hàm lượng diệp lục trong thực vật bậc cao, nhạy cảm với sự biến đổi độ phản xạ do sự hấp thụ của diệp lục gây ra.

$$
LCI = {NIR2 - RedEdge \over NIR2 + Red}
$$

_Tài liệu tham khảo: Datt, B. &quot;Phân tích từ xa hàm lượng nước trong lá cây bạch đàn.&quot; Tạp chí Sinh lý Thực vật 154, số 1 (1999): 30-36._

***

## MNLI - Chỉ số phi tuyến tính sửa đổi

Chỉ số này là phiên bản cải tiến của Chỉ số phi tuyến (NLI), kết hợp Chỉ số Thực vật Điều chỉnh Đất (SAVI) để tính đến nền đất. ENVI sử dụng giá trị hệ số điều chỉnh nền tán lá (_L_) là 0,5.

$$
MNLI = {(NIR^{2} - Red) * (1 + L) \over (NIR^{2} + Red + L)  }
$$

_Tài liệu tham khảo: Yang, Z., P. Willis, và R. Mueller. &quot;Tác động của hình ảnh AWIFS được cải tiến bằng tỷ lệ dải phổ đối với độ chính xác phân loại cây trồng.&quot; Kỷ yếu Hội nghị chuyên đề Viễn thám Pecora 17 (2008), Denver, CO._

***

## MSAVI2 - Chỉ số Thực vật Điều chỉnh theo Đất 2 (Modified Soil Adjusted Vegetation Index 2)

Chỉ số này là phiên bản đơn giản hơn của Chỉ số Thực vật Điều chỉnh theo Đất (MSAVI) do Qi và cộng sự (1994) đề xuất, nhằm cải thiện so với Chỉ số Thực vật Điều chỉnh theo Đất (SAVI). Nó giúp giảm nhiễu từ đất và tăng dải động của tín hiệu thực vật. MSAVI2 dựa trên phương pháp suy diễn, không sử dụng giá trị _L_ cố định (như trong Chỉ số Thực vật Điều chỉnh theo Đất - SAVI) để làm nổi bật thảm thực vật khỏe mạnh.

$$
MSAVI2 = {2 * NIR + 1 - \sqrt{(2 * NIR + 1)^{2} - 8(NIR - Red)} \over 2}
$$

_Tài liệu tham khảo: Qi, J., A. Chehbouni, A. Huete, Y. Kerr, và S. Sorooshian. &quot;Chỉ số Thực vật Điều chỉnh theo Đất đã được Sửa đổi.&quot; Tạp chí Viễn thám Môi trường 48 (1994): 119-126._

***

## MSR - Tỷ lệ đơn giản đã điều chỉnh

Chỉ số này là phiên bản cải tiến của tỷ lệ đơn giản NIR / Red, được thiết kế để tuyến tính hóa mối quan hệ của nó với các thông số sinh lý-vật lý, và nhạy cảm hơn so với NDVI ở các mật độ thảm thực vật cao hơn.

$$
MSR = {(NIR / Red) - 1 \over \sqrt{NIR / Red} + 1}
$$

_Tài liệu tham khảo: Chen, J. &quot;Đánh giá các chỉ số thảm thực vật và Tỷ lệ Đơn giản Được Sửa đổi cho các ứng dụng ở vùng Boreal.&quot; Tạp chí Viễn thám Canada 22 (1996): 229-242._

***

## Chỉ số Khác biệt Chuẩn hóa (NDRE) - Chỉ số Khác biệt Chuẩn hóa (Normalized Difference RedEdge)

Chỉ số này tương tự như Chỉ số Khác biệt Chuẩn hóa (NDVI) nhưng so sánh độ tương phản giữa vùng có độ hấp thụ cao nhất (NIR) với vùng có độ phản xạ cao nhất (RedEdge) thay vì vùng có độ hấp thụ thấp nhất (Red), điều này thường giúp phát hiện tình trạng căng thẳng của thảm thực vật sớm hơn.

$$
NDRE = {NIR - RedEdge \over NIR + RedEdge  }
$$

***

## Chỉ số Khác biệt Chuẩn hóa (NDVI) - Chỉ số Thực vật Chênh lệch Chuẩn hóa (NDVI)

Chỉ số này là thước đo mức độ khỏe mạnh của thảm thực vật xanh. Sự kết hợp giữa công thức chênh lệch chuẩn hóa và việc sử dụng các vùng hấp thụ và phản xạ cao nhất của chất diệp lục giúp chỉ số này hoạt động ổn định trong nhiều điều kiện khác nhau. Tuy nhiên, chỉ số này có thể bị bão hòa trong điều kiện thảm thực vật dày đặc khi giá trị của chỉ số phản xạ (LAI) trở nên cao.

$$
NDVI = {NIR - Red \over NIR + Red  }
$$

Giá trị của chỉ số này dao động từ -1 đến 1. Phạm vi giá trị phổ biến cho thảm thực vật xanh là từ 0,2 đến 0,8.

_Tài liệu tham khảo: Rouse, J., R. Haas, J. Schell, và D. Deering. Giám sát các hệ thống thực vật ở vùng Great Plains bằng ERTS. Hội nghị chuyên đề ERTS lần thứ ba, NASA (1973): 309-317._

***

## NLI - Chỉ số phi tuyến

Chỉ số này giả định rằng mối quan hệ giữa nhiều chỉ số thảm thực vật và các thông số sinh lý bề mặt là phi tuyến. Nó tuyến tính hóa các mối quan hệ với các thông số bề mặt vốn có xu hướng phi tuyến.

$$
NLI = {NIR^{2} - Red \over NIR^{2} + Red  }
$$

_Tài liệu tham khảo: Goel, N., và W. Qin. &quot;Ảnh hưởng của kiến trúc tán cây đối với mối quan hệ giữa các chỉ số thảm thực vật khác nhau vớiLAIvà Fpar: Một mô phỏng máy tính.&quot; Tạp chí Đánh giá Viễn thám 10 (1994): 309-347._

***

## Chỉ số Thực vật được Điều chỉnh theo Đất (OSAVI) - Phiên bản tối ưu hóa

Chỉ số này dựa trên Chỉ số Thực vật được Điều chỉnh theo Đất (SAVI). Nó sử dụng giá trị tiêu chuẩn là 0,16 cho hệ số điều chỉnh nền tán cây. Rondeaux (1996) đã xác định rằng giá trị này mang lại sự biến thiên về đất cao hơn so với phương pháp &quot;SAVI&quot; đối với vùng có độ che phủ thực vật thấp, đồng thời thể hiện độ nhạy cao hơn đối với độ che phủ thực vật trên 50%. Chỉ số này phù hợp nhất để sử dụng ở những khu vực có thảm thực vật tương đối thưa thớt, nơi đất có thể nhìn thấy qua tán cây.

$$
OSAVI = {(NIR - Red) \over (NIR + Red + 0.16)  }
$$

_Tài liệu tham khảo: Rondeaux, G., M. Steven, và F. Baret. &quot;Tối ưu hóa các chỉ số thảm thực vật được điều chỉnh theo đất.&quot; Remote Sensing of Environment 55 (1996): 95-107._

***

## RDVI - Chỉ số Thực vật Chênh lệch Tái chuẩn hóa

Chỉ số này sử dụng sự chênh lệch giữa các bước sóng cận hồng ngoại và đỏ, cùng với chỉ số thực vật chuẩn hóa (NDVI), để làm nổi bật thảm thực vật khỏe mạnh. Chỉ số này không bị ảnh hưởng bởi các yếu tố như đất và góc quan sát của mặt trời.

$$
RDVI = {(NIR- Red) \over \sqrt{(NIR + Red)}  }
$$

_Tài liệu tham khảo: Roujean, J., và F. Breon. &quot;Ước tính PAR được thực vật hấp thụ từ các phép đo phản xạ hai chiều.&quot; Remote Sensing of Environment 51 (1995): 375-384._

***

## Chỉ số Thực vật Điều chỉnh theo Đất (SAVI)

Chỉ số này tương tự như Chỉ số Mật độ Thực vật (NDVI), nhưng nó loại bỏ ảnh hưởng của các pixel đất. Chỉ số này sử dụng hệ số điều chỉnh nền tán cây, _L_, là hàm của mật độ thực vật và thường yêu cầu kiến thức trước về lượng thực vật. Huete (1988) đề xuất giá trị tối ưu của _L_ = 0,5 để tính đến các biến động nền đất bậc nhất. Chỉ số này được sử dụng hiệu quả nhất ở những khu vực có thảm thực vật tương đối thưa thớt, nơi đất có thể nhìn thấy qua tán cây.

$$
SAVI = {1.5 * (NIR- Red) \over (NIR + Red + 0.5)  }
$$

_Tài liệu tham khảo: Huete, A. &quot;Chỉ số Thực vật Được Điều chỉnh theo Đất (SAVI).&quot; Tạp chí Remote Sensing of Environment 25 (1988): 295-309._

***

## TDVI - Chỉ số Thực vật Chênh lệch Biến đổi

Chỉ số này hữu ích cho việc giám sát độ che phủ thực vật trong môi trường đô thị. Nó không bị bão hòa như Chỉ số Thực vật Chênh lệch (NDVI) và Chỉ số Thực vật Chênh lệch Điều chỉnh (SAVI).

$$
TDVI = 1.5 * {(NIR- Red) \over \sqrt{NIR^{2} + Red + 0.5}  }
$$

_Tài liệu tham khảo: Bannari, A., H. Asalhi, và P. Teillet. &quot;Chỉ số Thực vật Chênh lệch Biến đổi (TDVI) để Lập bản đồ Mật độ Thực vật&quot; Trong Kỷ yếu Hội nghị Khoa học Địa chất và Viễn thám, IGARSS &#x27;02, IEEE Quốc tế, Tập 5 (2002)._

***

## Chỉ số kháng nhiễu khí quyển trong dải quang phổ khả kiến (VARI)

Chỉ số này dựa trên Chỉ số Kháng nhiễu Khí quyển (ARVI) và được sử dụng để ước tính tỷ lệ thảm thực vật trong một khung cảnh với độ nhạy thấp đối với các tác động của khí quyển.

$$
VARI = {Green - Red \over Green + Red - Blue  }
$$

_Tài liệu tham khảo: Gitelson, A., và cộng sự. &quot;Các đường phân chia thảm thực vật và đất trong không gian quang phổ ánh sáng khả kiến: Một khái niệm và kỹ thuật để ước tính từ xa tỷ lệ thảm thực vật. Tạp chí Quốc tế về Viễn thám 23 (2002): 2537−2562._

***

## WDRVI - Chỉ số thảm thực vật dải động rộng

Chỉ số này tương tự như Chỉ số Thực vật Hình ảnh (NDVI), nhưng sử dụng hệ số trọng số (_a_) để giảm sự chênh lệch giữa đóng góp của tín hiệu hồng ngoại gần và tín hiệu đỏ vào Chỉ số Thực vật Hình ảnh (NDVI). Chỉ số Thực vật Dải rộng (WDRVI) đặc biệt hiệu quả trong các cảnh có mật độ thực vật từ trung bình đến cao khi Hệ số Trọng số (NDVI) vượt quá 0.6. Chỉ số độ che phủ thực vật (NDVI) có xu hướng ổn định khi tỷ lệ thực vật và chỉ số diện tích lá (LAI) tăng lên, trong khi chỉ số độ che phủ thực vật điều chỉnh (WDRVI) nhạy cảm hơn với phạm vi rộng hơn của tỷ lệ thực vật và với những thay đổi trong chỉ số diện tích lá (LAI).

$$
WDRVI = {(\alpha * NIR- Red) \over (\alpha * NIR + Red)}
$$

Hệ số trọng số (_a_) có thể dao động từ 0,1 đến 0,2. Henebry, Viña và Gitelson (2004) khuyến nghị sử dụng giá trị 0,2.

_Tài liệu tham khảo_

_Gitelson, A. &quot;Chỉ số Thực vật Dải Động Rộng để Định lượng Từ xa các Đặc tính Vật lý-Sinh học của Thực vật.&quot; Tạp chí Sinh lý Thực vật 161, Số 2 (2004): 165-173._

_Henebry, G., A. Viña và A. Gitelson. &quot;Chỉ số Thực vật Dải Động Rộng và tiềm năng ứng dụng của nó trong phân tích khoảng trống.&quot; Tạp chí Phân tích Khoảng trống 12: 50-56._
