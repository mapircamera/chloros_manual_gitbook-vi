---
metaLinks: {}
---

# Bắt đầu

<div data-full-width="false"><figure><img src=".gitbook/assets/chloros_logo_transparent.png" alt=""><figcaption></figcaption></figure></div>

Chloros là một ứng dụng phần mềm từ [MAPIR](https://www.mapir.camera) để xử lý hình ảnh và dữ liệu cảm biến khác.

***{% hint style="success" %}**Có gì mới trong Chloros 1.1.0**: Hỗ trợ Linux gốc (amd64 và arm64), điện toán biên NVIDIA Jetson, Thích ứng điện toán động, quy trình xử lý 4 luồng, các lệnh và tùy chọn CLI mới. Xem [Tải xuống](download.md) để biết nhật ký thay đổi đầy đủ.
{% endhint %}

Chloros có sẵn ở 3 chế độ ứng dụng:

## Chloros: Ứng dụng GUI trên máy tính để bàn

Cửa sổ riêng biệt độc lập với tất cả các tính năng. _Chỉ dành cho Windows._

## [Chloros CLI: Giao diện dòng lệnh](CLI.md)

Xử lý hàng loạt dòng lệnh. Hoàn hảo cho tự động hóa, viết kịch bản và vận hành không đầu. Có sẵn trên **Windows, Linux amd64 và Linux arm64 (NVIDIA Jetson)**. _CLI yêu cầu giấy phép Chloros+ để truy cập._

## [Chloros API: Python SDK](api-python-sdk.md)

Giao diện Python có lập trình để tự động hóa và quy trình làm việc tùy chỉnh. Hoàn hảo cho quy trình nghiên cứu, tích hợp với các ứng dụng Python hiện có và xây dựng các công cụ tùy chỉnh. Có sẵn trên **tất cả các nền tảng** thông qua `pip install chloros-sdk`. _API yêu cầu giấy phép Chloros+ để truy cập._***

## Nền tảng được hỗ trợ

| Nền tảng | GUI | CLI | Python SDK |
| --- | --- | --- | --- |
| **Windows 11/10** | Có | Có | Có |
| **Linux amd64 (x86_64)** | Không | Có | Có |
| **Linux arm64 (NVIDIA Jetson)** | Không | Có | Có |

Để biết hướng dẫn cài đặt Linux, hãy xem phần [Linux & Điện toán biên](linux/linux-overview.md).

***

## Chloros+

Mặc dù Chloros được sử dụng miễn phí cho hầu hết các tác vụ nhưng bạn có thể thấy mình muốn nhiều hơn thế. Đó là lúc giấy phép trả phí cho Chloros+ có thể mang lại lợi ích cho bạn. Với giấy phép Chloros+, bạn có thể mở khóa các tính năng mới như:

* **Xử lý đa luồng**: tăng tốc đáng kể quá trình xử lý hình ảnh cho các dự án lớn hơn bằng cách xử lý đồng thời hình ảnh thông qua quy trình.
* **Tăng tốc GPU (CUDA)**: tận dụng các tùy chọn bộ nhớ GPU cao hơn hiện nay để tăng tốc hơn nữa quy trình xử lý hình ảnh. Chúng tôi khuyên dùng VRAM 4GB trở lên để có kết quả tốt nhất.
* **Chloros+**[**CLI**](CLI.md)**Truy cập**: chạy Chloros+ từ dòng lệnh để tự động hóa và tích hợp vào phần mềm của riêng bạn.
* **Chloros+**[**API**](api-python-sdk.md)**Truy cập:** chạy Chloros+ từ Python để kiểm soát theo chương trình, cho phép tích hợp liền mạch với quy trình nghiên cứu, quy trình phân tích dữ liệu và ứng dụng tùy chỉnh của bạn.
* **Sử dụng nhiều thiết bị**: mỗi giấy phép Chloros+ cho phép đăng ký hơn 2 thiết bị. Sử dụng tài khoản MAPIR Cloud của bạn để quản lý các thiết bị đã đăng ký. Thêm hỗ trợ cho nhiều thiết bị hơn bằng cách nâng cấp giấy phép Chloros+ của bạn.
* **Phương pháp gỡ lỗi nhận biết kết cấu nâng cao:** một công cụ gỡ lỗi nhận biết cạnh chất lượng cao kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ nhiễu gây nhiễu.&#x20;
* **Công thức chỉ mục đa phổ tùy chỉnh:** nhập các chỉ mục đa phổ tùy chỉnh vào máy tính raster Chloros, cho cả để xử lý và hộp cát xem hình ảnh.
* **Linux & Điện toán biên:** chạy Chloros trên nền tảng Linux x86\_64 và ARM64 bao gồm NVIDIA Jetson để xử lý trường và biên. Xem [Tổng quan về Linux](linux/linux-overview.md).

<palign="center"><a href="https://cloud.mapir.camera/pricing" class="button Primary" data-icon="envira">Chloros+ Giá &#x26; Đăng ký</a></p>

<figure><img src=".gitbook/assets/plus_prog.JPG" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_zoom.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_mode.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_meta.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_map_markers.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>