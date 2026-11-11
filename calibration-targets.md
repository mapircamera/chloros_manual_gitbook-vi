---
description: Lab-measured panels used to calibrate captured data in post processing
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/calibration-targets
---

# Mục tiêu hiệu chuẩn

MAPIR cung cấp nhiều loại mục tiêu hiệu chuẩn khác nhau để đáp ứng đa dạng các ứng dụng. Mẫu T4-R50 nhỏ gọn như hình dưới đây bao gồm 4 tấm đã được đo độ phản xạ ánh sáng trong dải bước sóng từ 250 đến 2.500 nm.

<figure><img src=".gitbook/assets/t4-r50_2.jpg" alt=""><figcaption><p>MAPIR T4-R50</p></figcaption></figure>Các mục tiêu tham chiếu khuếch tán T4 có các đường cong độ phản xạ như sau, [tải dữ liệu tại đây](https://cdn.shopify.com/s/files/1/0972/5566/files/MAPIR_Diffuse_Reflectance_Standard_Calibration_Target_Data_T4.xlsx?v=1741759157):

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4 (250-2500nm).png" alt=""><figcaption><p>Độ phản xạ MAPIR T4 :: 250-2.500 nm</p></figcaption></figure>

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4 (400-1000nm).png" alt=""><figcaption><p>Độ phản xạ MAPIR T4 :: 400–1.000 nm</p></figcaption></figure>Các mục tiêu tham chiếu khuếch tán T4P có các đường cong phản xạ như sau, [tải dữ liệu tại đây](https://cdn.shopify.com/s/files/1/0972/5566/files/MAPIR_Diffuse_Reflectance_Standard_Calibration_Target_Data_T4.xlsx?v=1741759157):

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4P -- 350-2500nm.jpg" alt=""><figcaption><p>Độ phản xạ MAPIR T4P :: 250–2500 nm</p></figcaption></figure>

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4P -- 400-1000nm.jpg" alt=""><figcaption><p>Độ phản xạ MAPIR T4P :: 400-1000nm</p></figcaption></figure>Khi xem biểu đồ độ phản xạ, bạn có thể thấy các giá trị được biểu diễn theo bước sóng (trục x) so với phần trăm độ phản xạ (trục y). Khi chụp ảnh mục tiêu hiệu chuẩn, chúng ta sẽ thiết lập mối quan hệ giữa giá trị pixel và phần trăm độ phản xạ, trong dải phổ mà từng dải cảm biến của máy ảnh nhạy cảm.

Điều này có nghĩa là với mỗi hình ảnh bạn chụp bằng máy ảnh của chúng tôi, bạn có thể sử dụng ảnh của các mục tiêu phản xạ của chúng tôi, chẳng hạn như [T4-R50](https://www.mapir.camera/collections/calibration-targets/products/diffuse-reflectance-standard-calibration-target-package-t3-r50) hoặc [T4-R125](https://www.mapir.camera/collections/multispectral-reflectance-reference-calibration-targets/products/diffuse-reflectance-standard-calibration-target-package-t4-r125) để hiệu chuẩn hình ảnh theo độ phản xạ. Sau khi hiệu chuẩn, mỗi pixel trong hình ảnh sẽ tương ứng với một phần trăm độ phản xạ.

Đối với đầu ra của **Survey3**, nếu bạn xuất các hình ảnh đã hiệu chỉnh trong Chloros

dưới dạng JPG hoặc TIFF

thông thường, thì phần trăm phản xạ được tính bằng cách chia giá trị pixel cho độ sâu bit của định dạng hình ảnh. Vì vậy, đối với JPG thì chia cho 255, và đối với TIFF

thì chia cho 65.535. Bạn cũng có thể chọn định dạng đầu ra PERCENT trong Chloros

; khi đó, mỗi pixel sẽ có giá trị phần trăm nằm trong khoảng từ 0,0 đến 1,0 (tương ứng với độ phản xạ từ 0% đến 100%). Tuy nhiên, cần lưu ý rằng một số ứng dụng xử lý hình ảnh không hỗ trợ định dạng phần trăm (số thập phân), và các tệp hình ảnh này thường có dung lượng lưu trữ lớn.

{% hint style="info" %}
**Độ phản xạ LATTICE sử dụng tỷ lệ pixel khác.** Độ phản xạ LATTICE được lưu trữ với DN 32768 = 100% độ phản xạ (không phải 65535), và mỗi tệp đều chứa thẻ XMP `Chloros:PixelScale` ghi rõ tỷ lệ của nó. Hãy đọc thẻ này và chia cho giá trị đó thay vì giả định một hằng số — xem [Định dạng hình ảnh đầu ra](output-image-formats.md).
{% endhint %}

## Mục tiêu hiệu chuẩn với camera LATTICE

Với camera LATTICE, mục tiêu hiệu chuẩn là **tùy chọn** đối với độ phản xạ: Thay vào đó, Chloros

có thể tham chiếu độ phản xạ đến bức xạ xuống được đo bằng cảm biến ánh sáng DAQ (ρ = π·L/E). Điểm tham chiếu được chọn thông qua cài đặt nguồn phản xạ của dự án, được thiết lập bằng `chloros-cli process --reflectance-source` (giao diện người dùng (GUI) không có tùy chọn điều khiển cho thông số này và sẽ sử dụng giá trị đã lưu của dự án, mặc định là `auto` — xem [Cài đặt Dự án](project-settings/project-settings.md)):

| Giá trị | Hành vi |
| --- | --- |
| `auto` *(mặc định)* | Mục tiêu trong khung hình vượt qua kiểm tra chất lượng (QA) là **điểm tham chiếu tuyệt đối**; khi không có mục tiêu hoặc kiểm tra QA thất bại, Chloros

sẽ chuyển sang sử dụng tỷ lệ phân chia hướng xuống của DAQ. Một mục tiêu vượt qua kiểm tra chất lượng (QA) kèm theo cờ báo lỗi (một bảng bị loại bỏ hoặc một dải tần bị lỗi) sẽ không tự động được ưu tiên: các chuỗi mục tiêu, mục tiêu+DAQ và chỉ DAQ sẽ được thử theo thứ tự điểm số đo được trên các bảng được giữ lại. |
| `target` | Chỉ mục tiêu nghiêm ngặt — không thay thế bằng DAQ. |
| `daq` | DAQ có thẩm quyền — phép đo hướng xuống luôn là tham chiếu. |

Hành vi mục tiêu bổ sung cho LATTICE:

* **Hình học mục tiêu** — Hỗ trợ các bảng có đánh dấu ArUco, bảng ROI cố định và mục tiêu dải; hình học được lấy từ cấu hình mục tiêu của dự án.
* **Dữ liệu đo phản xạ mục tiêu theo đơn vị** — `--target-reflectance-dir DIR` trỏ đến thư mục chứa các bản quét phản xạ mục tiêu được đo theo từng đơn vị (`<serial>.csv`, được tra cứu dựa trên số seri/mã QR của đơn vị mục tiêu). Nếu không tìm thấy, Chloros

sẽ sử dụng phổ T3/T4P danh nghĩa làm giá trị dự phòng.
* **Gắn mốc thời gian** — một mục tiêu được phát hiện sẽ hiệu chỉnh các khung hình xung quanh nó và được giữ lại giữa các lần phát hiện mục tiêu.

Ý nghĩa đầy đủ của các cờ và ví dụ có trong [Tài liệu tham khảo CLI

](reference/cli-reference.md) (xem &quot;Các tùy chọn xuất theo sản phẩm&quot;).

### F988

&quot;Độ phản xạ F988 được hiệu chuẩn bằng bảng đo độ phản xạ trong cảnh: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros

áp dụng dữ liệu chụp bảng đo gần nhất của bạn và duy trì giá trị đó giữa các lần quan sát bảng đo.&quot;

Nếu F988 được chạy với hiệu chuẩn chỉ bằng DAQ, Chloros

sẽ từ chối giá trị phản xạ dựa trên DAQ cho dải đó và giải thích lý do (lý do bỏ qua: `dls-uncalibrated-band-988`); quy trình làm việc với bảng phản xạ là phương pháp được hỗ trợ.

<div><figure><img src=".gitbook/assets/t3-125.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure> <figure><img src=".gitbook/assets/t3-125_2.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure> <figure><img src=".gitbook/assets/t3-125_closed.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure></div>
