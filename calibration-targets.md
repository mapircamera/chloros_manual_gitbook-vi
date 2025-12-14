---
mô tả: Bảng đo trong phòng thí nghiệm được sử dụng để hiệu chỉnh dữ liệu đã ghi trong quá trình xử lý hậu kỳ
liên kết meta:
  thay thế:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/calibration-targets
---

# Mục tiêu hiệu chuẩn

MAPIR cung cấp nhiều mục tiêu hiệu chuẩn khác nhau để đáp ứng nhiều ứng dụng. T4-R50 nhỏ gọn bên dưới chứa 4 tấm đã được đo độ phản xạ ánh sáng từ 250 - 2.500 nm.

<figure><img src=".gitbook/assets/t4-r50_2.jpg" alt=""><figcaption><p>MAPIR T4-R50</p></figcaption></figure>

Mục tiêu tham chiếu khuếch tán T4 có các đường cong phản xạ sau, [tải xuống dữ liệu tại đây](https://cdn.shopify.com/s/files/1/0972/5566/files/MAPIR_Diffuse_Reflectance_Standard_Calibration_Target_Data_T4.xlsx?v=1741759157):

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4 (250-2500nm).png" alt=""><figcaption><p>MAPIR T4 Reflectance :: 250-2500nm</p></figcaption></figure>

<figure><img src=".gitbook/assets/MAPIR Diffuse Reflectance Standard Calibration Target Data T4 (400-1000nm).png" alt=""><figcaption><p>MAPIR T4 Reflectance :: 400-1000nm</p></figcaption></figure>

Nhìn vào biểu đồ phản xạ, bạn có thể thấy các giá trị là bước sóng (trục x) so với phần trăm phản xạ (trục y). Khi chúng tôi chụp ảnh mục tiêu hiệu chuẩn, chúng tôi sẽ tạo mối quan hệ giữa giá trị pixel và phần trăm phản xạ, trong quang phổ mà mỗi dải cảm biến của máy ảnh đều nhạy cảm.

Điều này có nghĩa là với mỗi hình ảnh bạn chụp bằng máy ảnh của chúng tôi, bạn có thể sử dụng ảnh về các mục tiêu phản chiếu của chúng tôi, chẳng hạn như [T4-R50](https://www.mapir.camera/collections/calibration-targets/products/diffuse-reflectance-standard-calibration-target-package-t3-r50) hoặc [T4-R125](https://www.mapir.camera/collections/multispectral-reflectance-reference-calibration-targets/products/diffuse-reflectance-standard-calibration-target-package-t4-r125) để hiệu chỉnh độ phản xạ của hình ảnh. Sau khi hiệu chỉnh, mỗi pixel trong ảnh sẽ bằng phần trăm độ phản xạ.

Nếu bạn xuất hình ảnh đã hiệu chỉnh bằng Chloros dưới dạng JPG hoặc TIFF thông thường thì phần trăm phản xạ được tính bằng cách chia giá trị pixel cho độ sâu bit của định dạng hình ảnh. Vì vậy, đối với JPG chia cho 255 và đối với TIFF chia cho 65.535. Bạn cũng có thể chọn đầu ra định dạng PERCENT trong Chloros, sau đó mỗi pixel sẽ nằm trong khoảng giá trị phần trăm từ 0,0 đến 1,0 (độ phản xạ 0% đến 100%). Chỉ cần lưu ý rằng một số ứng dụng hình ảnh không thể chấp nhận hình ảnh phần trăm (dấu phẩy động) và chúng có kích thước lưu trữ lớn.

<div><figure><img src=".gitbook/assets/t3-125.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure> <figure><img src=".gitbook/assets/t3-125_2.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure> <figure><img src=".gitbook/assets/t3-125_closed.jpg" alt=""><figcaption><p>T4-R125</p></figcaption></figure></div>
