---
description: Lab-measured panels used to calibrate captured data in post processing
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/calibration-targets
---

# Mục tiêu hiệu chuẩn

MAPIRcung cấp nhiều loại mục tiêu hiệu chuẩn khác nhau để đáp ứng các ứng dụng đa dạng. Mẫu T4-R50 nhỏ gọn được trình bày dưới đây bao gồm 4 tấm đã được đo độ phản xạ ánh sáng trong dải bước sóng từ 250 đến 2.500 nm.

<figure><img src=".gitbook/assets/t4-r50_2.jpg" alt=""><figcaption><p>MAPIR T4-R50</p></figcaption></figure>Các mục tiêu tham chiếu khuếch tán T4 có các đường cong độ phản xạ như sau, [tải dữ liệu tại đây](https://cdn.shopify.com/s/files/1/0972/5566/files/MAPIR_Diffuse_Reflectance_Standard_Calibration_Target_Data_T4.xlsx?v=1741759157):

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4 (250-2500nm).png" alt=""><figcaption><p>MAPIR Độ phản xạ T4 :: 250-2.500 nm</p></figcaption></figure>

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4 (400-1000nm).png" alt=""><figcaption><p>MAPIR Độ phản xạ T4 :: 400-1.000 nm</p></figcaption></figure>Các mục tiêu tham chiếu khuếch tán T4 có các đường cong phản xạ như sau, [tải dữ liệu tại đây](https://cdn.shopify.com/s/files/1/0972/5566/files/MAPIR_Diffuse_Reflectance_Standard_Calibration_Target_Data_T4.xlsx?v=1741759157):

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4P -- 350-2500nm.jpg" alt=""><figcaption><p>MAPIR Độ phản xạ T4P :: 250–2500 nm</p></figcaption></figure>

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4P -- 400-1000nm.jpg" alt=""><figcaption><p>MAPIR Độ phản xạ T4P :: 400-1000nm</p></figcaption></figure>Khi xem biểu đồ độ phản xạ, bạn có thể thấy các giá trị được biểu diễn theo bước sóng (trục x) so với phần trăm độ phản xạ (trục y). Khi chúng ta chụp ảnh mục tiêu hiệu chuẩn, chúng ta sẽ thiết lập mối quan hệ giữa giá trị pixel và phần trăm độ phản xạ, trong dải phổ mà từng dải cảm biến của máy ảnh nhạy cảm.

Điều này có nghĩa là với mỗi hình ảnh bạn chụp bằng máy ảnh của chúng tôi, bạn có thể sử dụng ảnh của các mục tiêu phản xạ của chúng tôi, chẳng hạn như [T4-R50](https://www.mapir.camera/collections/calibration-targets/products/diffuse-reflectance-standard-calibration-target-package-t3-r50) hoặc [T4-R125](https://www.mapir.camera/collections/multispectral-reflectance-reference-calibration-targets/products/diffuse-reflectance-standard-calibration-target-package-t4-r125) để hiệu chuẩn hình ảnh theo độ phản xạ. Sau khi hiệu chuẩn, mỗi pixel trong hình ảnh sẽ tương ứng với phần trăm độ phản xạ.

Đối với các đầu ra **Survey3**, nếu bạn xuất các hình ảnh đã hiệu chỉnh ở định dạngChloros

dưới dạng JPG thông thường hoặcTIFF

thì phần trăm phản xạ được tính bằng cách chia giá trị pixel cho độ sâu bit của định dạng hình ảnh. Vì vậy, đối với JPG, chia cho 255, và đối vớiTIFF

, chia cho 65.535. Bạn cũng có thể chọn định dạng đầu ra PERCENT trongChloros

, và khi đó mỗi pixel sẽ có giá trị phần trăm từ 0,0 đến 1,0 (độ phản xạ từ 0% đến 100%). Chỉ cần lưu ý rằng một số ứng dụng hình ảnh không chấp nhận hình ảnh dạng phần trăm (số thập phân), và chúng có kích thước lưu trữ khá lớn.

{% hint style="info" %}
**Độ phản xạ LATTICE sử dụng một tỷ lệ pixel khác.** Độ phản xạ LATTICE được lưu trữ với DN 32768 = 100% độ phản xạ (không phải 65535), và mỗi tệp đều chứa thẻ XMP `Chloros:PixelScale` nêu rõ tỷ lệ của nó. Hãy đọc thẻ này và chia cho giá trị đó thay vì giả định một hằng số — xem [Định dạng hình ảnh đầu ra](output-image-formats.md).
{% endhint %}

## Mục tiêu hiệu chuẩn với camera LATTICE

Với camera LATTICE, mục tiêu hiệu chuẩn là **tùy chọn** đối với độ phản xạ:Chloros

thay vào đó có thể tham chiếu độ phản xạ đến bức xạ chiếu xuống được đo bằng cảm biến ánh sáng DAQ (ρ = π·L/E). Tham chiếu này được chọn thông qua cài đặt nguồn độ phản xạ (Project Settings trong giao diện người dùng; `--reflectance-source` trongCLI

; `reflectance_source` trongSDK

):

| Giá trị | Hành vi |
| --- | --- |
| `auto` *(mặc định)* | Mục tiêu trong khung hình đạt tiêu chuẩn QA là **tham chiếu tuyệt đối**; khi không có mục tiêu hoặc QA không đạt,Chloros

sẽ chuyển sang sử dụng tỷ lệ chia tín hiệu xuống của DAQ. |
| `target` | Chỉ sử dụng mục tiêu nghiêm ngặt — không thay thế bằng DAQ. |
| `daq` | DAQ có thẩm quyền — phép đo hướng xuống luôn là tham chiếu. |

Hành vi mục tiêu bổ sung cho LATTICE:

* **Hình dạng mục tiêu** — Hỗ trợ các tấm có đánh dấu ArUco, các tấm ROI cố định và các mục tiêu dạng dải; hình dạng được lấy từ cấu hình mục tiêu của dự án.
* **Dữ liệu đo phản xạ mục tiêu theo đơn vị** — `--target-reflectance-dir DIR` trỏ đến thư mục chứa các bản quét phản xạ mục tiêu được đo theo từng đơn vị (`<serial>.csv`, được tra cứu bằng số sê-ri/mã QR của đơn vị mục tiêu). Khi không phát hiện được mục tiêu, `Chloros

` sẽ chuyển sang sử dụng phổ T3/T4P danh nghĩa.
* **Định vị theo thời gian** — một mục tiêu được phát hiện sẽ hiệu chỉnh các khung hình xung quanh nó và được giữ nguyên vị trí giữa các lần quan sát mục tiêu.

Ý nghĩa đầy đủ của các cờ và ví dụ có trong [CLI

Tài liệu tham khảo](reference/cli-reference.md) (xem &quot;Các tùy chọn xuất theo sản phẩm&quot;).

### F988

&quot;Độ phản xạ của F988 được hiệu chuẩn bằng bảng phản xạ trong cảnh: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đóChloros

sẽ áp dụng dữ liệu chụp bảng phản xạ gần nhất của bạn và duy trì giá trị đó giữa các lần quan sát bảng phản xạ.&quot;

Nếu F988 được chạy với hiệu chuẩn chỉ bằng DAQ,Chloros

sẽ từ chối giá trị phản xạ dựa trên DAQ cho dải đó và nêu lý do (lý do bỏ qua `dls-uncalibrated-band-988`); quy trình làm việc với bảng phản xạ là phương án được hỗ trợ.

<div><figure><img src=".gitbook/assets/t3-125.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure> <figure><img src=".gitbook/assets/t3-125_2.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure> <figure><img src=".gitbook/assets/t3-125_closed.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure></div>
