---
metaLinks: {}
---

# Bắt đầu

<div data-full-width="false"><figure><img src=".gitbook/assets/chloros_logo_transparent.png" alt=""><figcaption></figcaption></figure></div>

Chloros

là một ứng dụng phần mềm từ [MAPIR

](https://www.mapir.camera) dùng để xử lý hình ảnh đa phổ, điều khiển trực tiếp thiết bịMAPIR

và ghi lại dữ liệu cảm biến.Chloros

1.2.0 hỗ trợ đầy đủ dòng sản phẩmMAPIR

:

* **Máy ảnhSurvey3** — xử lý các bản chụp RAW+JPG thành bản đồ phản xạ và chỉ số thực vật đã được hiệu chuẩn. Xem [Các máy ảnh được hỗ trợ](supported-cameras.md).
* **Máy ảnh LATTICE** — kết nối các mô-đun máy ảnh đa quang phổ GigE trực tiếp, riêng lẻ hoặc dưới dạng mảng nhiều máy ảnh đồng bộ: xem trước, chụp và xử lý thành các sản phẩm độ rạng rỡ và độ phản xạ đã được hiệu chuẩn. Xem [phần LATTICE](lattice/README.md).
* **Cảm biến ánh sáng DAQ** — Cảm biến quang phổ DAQ-U (USB), DAQ-M (Bluetooth) và DAQ-E (Ethernet): phổ đã hiệu chuẩn theo thời gian thực, bản ghi `.daq` và ánh sáng chiếu xuống để xử lý độ phản xạ. Xem [phần DAQ](daq/README.md).

{% hint style="success" %}
**Các tính năng mới trongChloros

1.2.0**: điều khiển camera và mảng cảm biến LATTICE theo thời gian thực, tích hợp cảm biến ánh sáng DAQ, các chế độ chụp và công cụ ghi dữ liệu, quy trình xử lý phóng xạ LATTICE hoàn chỉnh, tự động hóa dự án từCLI

/SDK

, và nhiều tính năng khác. Xem danh sách &quot;Có gì mới&quot; bên dưới và [Tải xuống](download.md) để xem nhật ký thay đổi.
{% endhint %}

{% hint style="info" %}
**Đang sử dụngChloros

với trợ lý AI?** Hướng dẫn này được thiết kế dành riêng cho mục đích đó. Hãy chỉ trợ lý của bạn đến:

* `https://mapir.gitbook.io/chloros/llms.txt` — chỉ mục có thể đọc được bằng máy của mọi trang.
* Bất kỳ trang nào dưới dạng Markdown thô — thêm `.md` vào cuối địa chỉURL

của trang đó (ví dụ: `https://mapir.gitbook.io/chloros/reference/cli-reference.md`).
* [CLI

Tài liệu tham khảo](reference/cli-reference.md) và [SDK

Tài liệu tham khảo](reference/sdk-reference.md) — các trang tài liệu tham khảo đầy đủ, có giá trị chính xác, được viết dành riêng cho việc sử dụng bởi các mô hình ngôn ngữ lớn (LLM).

Ví dụ về lời nhắc: *&quot;Đọc https://mapir.gitbook.io/chloros/reference/cli-reference.md, rồi viết một skript để đăng nhập và xử lý thư mục ~/flights/flight_001 thành các tệp GeoTIFF có thông tin phản xạ vàNDVI

.&quot;*

Hướng dẫn đầy đủ: [Sử dụngChloros

với Trợ lý AI](ai-assistants.md).
{% endhint %}

***

## Những điểm mới trong phiên bản 1.2.0 củaChloros



* **Điều khiển camera trực tiếp — tab &quot;Cameras&quot; mới.** Kết nối các camera LATTICE từng chiếc một hoặc dưới dạng mảng nhiều camera đồng bộ (đồng bộ thời gian PTP, chụp kích hoạt bằng phần cứng), với lớp phủ xem trước trực tiếp, biểu đồ phân bố theo dải tần, tự động phơi sáng thông minh, máy tính chỉ số trực tiếp và cập nhật phần mềm camera trong ứng dụng.
* **Cảm biến ánh sáng — tab Cảm biến ánh sáng mới.** Kết nối các cảm biến DAQ-U (USB), DAQ-M (Bluetooth) và DAQ-E (Ethernet); xem phổ đã hiệu chuẩn trực tiếp (W/m²/nm), ghi tệp `.daq` vào dự án của bạn, chọn cấu hình hiệu chỉnh nắp và cập nhật phần mềm cơ sở DAQ-E qua mạng.
* **Chế độ chụp và bộ ghi.** Chế độ chụp Đơn / Liên tục / Theo khoảng thời gian cùng chế độ Chụp Nhanh Nhất chỉ ghi dữ liệu thô; lựa chọn theo dự án về các camera và loại tệp xuất mà &quot;Chụp Tất Cả&quot; tạo ra; bộ ghi mảng cho video chỉ số chất lượng giám sát và các đợt dữ liệu thô chất lượng phân tích với khả năng xây dựng video ngoại tuyến.
* **Dòng xử lý LATTICE.** Nhập các thư mục ghi hình LATTICE và tách từng khung hình thô thành các sản phẩm đã loại bỏ hiệu ứng Bayer, bản xem trước, độ rạng rỡ float32 (W/m²/sr/nm) và độ phản xạ với các nút bật/tắt cho từng sản phẩm. Độ phản xạ có thể lấy từ mục tiêu hiệu chuẩn trong khung hình hoặc dữ liệu thu thập từ cảm biến (DAQ); việc căn chỉnh mảng được áp dụng cho các tệp xuất; hiệu chuẩn nhà máy bị thiếu sẽ được tải xuống tự động dựa trên số sê-ri máy ảnh.
* **Các dự án ghi nhớ cấu hình phần cứng.** Các camera và cảm biến ánh sáng đã kết nối được lưu cùng với dự án (`cameras.json` / `sensors.json`) và sẽ tự động kết nối lại với các cài đặt đã lưu khi bạn mở lại dự án. Xem [GUI: Dự án](projects.md).
* **Cập nhật trình xem hình ảnh.** Hiển thị pixel/chỉ số con trỏ với tỷ lệ phản xạ chính xác theo từng tệp, biểu đồ histogram từng lớp, thanh trượt nhóm GSD, chế độ lưới &quot;Per Trigger&quot; / &quot;Per Camera&quot;, chế độ xem sản phẩm LATTICE và xuất dữ liệu chỉ số/LUT vào đĩa dưới dạng sandbox.
* **Các lệnhCLI

vàSDK

được mở rộng đáng kể.** Các nhóm lệnh mới `lattice`, `daq pool-*`, `project` và `time-sync`; các tùy chọn `process` mới (`--input-level`, các tùy chọn bật/tắt theo từng sản phẩm, `--reflectance-source`, các cờ căn chỉnh mảng);SDK

các đối tượng smart-connect (`connect_camera` / `connect_array` / `connect_daq_sensor`) tự động khởi động phần nền; Tự động hóa `open_project()`; bánh xeSDK

được gói kèm với các trình cài đặt và được phát hành trên PyPI dưới tên `chloros-sdk`.
* **Cơ chế báo lỗi minh bạch.** Một lần chạy `chloros-cli process` yêu cầu tạo sản phẩm nhưng không tạo được sản phẩm nào giờ đây sẽ báo lỗi rõ ràng và thoát với mã trạng thái không bằng 0; các lần chạy thành công sẽ báo cáo số lượng sản phẩm hình ảnh đã tạo.
* **Cấu trúc đầu ra mới.** Các sản phẩm được lưu trong các thư mục `<project>/<camera>/<format>/<Product>_Images/` và giữ nguyên tên tệp nguồn — thư mục, chứ không phải hậu tố tên tệp, là yếu tố xác định sản phẩm. Xem [Định dạng hình ảnh đầu ra](output-image-formats.md).
* **Thêm đầu vào, gói dịch vụ và ngôn ngữ.** Hỗ trợ đầu vào `.dng`; đầy đủ 38 ngôn ngữ giao diện; giới hạn phần cứng theo từng gói dịch vụ với việc sử dụng miễn phí (không cần đăng nhập) tối đa 4 camera và 2 cảm biến ánh sáng.
* **Độ tin cậy.** Chức năng “Dừng xử lý” kết thúc một cách trơn tru kèm theo bản tóm tắt quá trình chạy chi tiết, các dự án đa camera xuất dữ liệu từ mọi camera, và các bản cập nhật trình cài đặt không còn khiến bạn bị đăng xuất.***

Chloros

có sẵn trên 3 nền tảng ứng dụng:

##Chloros

: Ứng dụng GUI trên máy tính để bàn

Cửa sổ độc lập riêng biệt với đầy đủ tính năng, bao gồm các tab Camera trực tiếp và Cảm biến ánh sáng. _Chỉ dành cho Windows._

## [Chloros

CLI

: Giao diện dòng lệnh](CLI.md)

Xử lý hàng loạt qua dòng lệnh cùng các lệnh trực tiếp `lattice`, `daq pool-*`, `project` và `time-sync`. Hoàn hảo cho tự động hóa, lập kịch bản và vận hành không giao diện người dùng. Có sẵn trên **Windows

,Linux

amd64 vàLinux

arm64 (NVIDIA Jetson)**. _CLI yêu cầu góiChloros

+ trả phí để truy cập._

## [Chloros

API

:Python

SDK

](api-python-sdk.md)

Giao diện lập trìnhPython

dành cho tự động hóa và quy trình làm việc tùy chỉnh: xử lý toàn bộ quy trình, phiên camera/mảng trực tiếp, phiên cảm biến DAQ và tự động hóa dự án đã lưu. Được cài đặt cùng với gói desktop/CLI

và cũng được phát hành dưới tên `pip install chloros-sdk`. _API yêu cầu góiChloros

+ trả phí để truy cập._

***

## Các nền tảng được hỗ trợ

| Nền tảng | Giao diện người dùng (GUI) |CLI

|Python

SDK

|
| --- | --- | --- | --- |
| **Windows

10/11 (x64)** | Có | Có | Có |
| **Linux

amd64 (x86_64)** | Không | Có | Có |
| **Linux

arm64 (NVIDIA Jetson)** | Không | Có | Có |

Để biết hướng dẫn cài đặt trênLinux

, hãy tham khảo phần [Linux

&amp; Edge Computing](linux/linux-overview.md).

***

## Bắt đầu với ba bước

1. **Cài đặt** — tải xuống và chạy trình cài đặt dành cho nền tảng của bạn. Xem [Tải xuống](download.md).
2. **Đăng nhập (tùy chọn đối với giao diện người dùng)** — giao diện người dùng xử lý hình ảnh miễn phí mà không cần tài khoản. [Chloros

+ đăng nhập](chloros+-login.md) sẽ mở khóa tính năng xử lý song song, tăng tốc GPU, giới hạn thiết bị cao hơn và quyền truy cập vàoCLI

/SDK

.
3. **Tạo dự án đầu tiên của bạn** — truy cậpChloros

, tạo [Dự án mới](projects.md), [thêm hình ảnh của bạn](processing-images-gui/adding-files-to-a-project.md) và [bắt đầu xử lý](processing-images-gui/starting-the-processing.md). Để điều khiển phần cứng trực tiếp, hãy mở tab &quot;Cameras&quot; hoặc &quot;Light Sensors&quot; — xem [GUI: Điều hướng](navigation.md).

***

##Chloros

+

Mặc dùChloros

miễn phí sử dụng cho hầu hết các tác vụ, nhưng có thể bạn sẽ cần nhiều tính năng hơn. Đó là lúc giấy phép trả phí choChloros

+ sẽ mang lại lợi ích cho bạn. Với giấy phépChloros

+, bạn có thể mở khóa các tính năng mới như:

* **Xử lý đa luồng**: tăng tốc đáng kể quá trình xử lý hình ảnh cho các dự án lớn hơn bằng cách xử lý đồng thời các hình ảnh qua quy trình xử lý.
* **Tăng tốc GPU (CUDA)**: tận dụng các tùy chọn bộ nhớ GPU cao hơn hiện nay để tăng tốc hơn nữa quy trình xử lý hình ảnh. Chúng tôi khuyến nghị sử dụng 4GB VRAM trở lên để đạt kết quả tốt nhất.
* **Quyền truy cậpChloros**[**CLI**](CLI.md): chạy lệnh `Chloros

` từ dòng lệnh để tự động hóa và tích hợp vào phần mềm của riêng bạn. Có sẵn trên mọi gói trả phí; được áp dụng ở phía máy chủ.
* **Chloros

+**[**API**](api-python-sdk.md) **Quyền truy cập:** chạy lệnhChloros

+ từPython

để điều khiển lập trình, cho phép tích hợp liền mạch với các quy trình nghiên cứu, quy trình phân tích dữ liệu và ứng dụng tùy chỉnh của bạn. Có sẵn trên mọi gói dịch vụ trả phí; được áp dụng ở phía máy chủ.
* **Giới hạn phần cứng cao hơn**: kết nối nhiều camera và cảm biến ánh sáng cùng lúc. Khi không đăng nhập, giao diện người dùng (GUI) hỗ trợ tối đa 4 camera và 2 cảm biến ánh sáng DAQ; các gói trả phí nâng cao cả hai giới hạn này:

| Gói | Camera | Cảm biến ánh sáng DAQ |
| --- | --- | --- |
| Iron (miễn phí, không cần đăng nhập) | 4 | 2 |
| Copper / Bronze | 6 | 3 |
| Silver | 10 | 6 |
| Gold | 20 | 12 |

* **Sử dụng nhiều thiết bị**: mỗi giấy phépChloros

+ cho phép đăng ký từ 2 thiết bị trở lên. Sử dụng tài khoảnMAPIR

Cloud của bạn để quản lý các thiết bị đã đăng ký. Mở rộng hỗ trợ cho nhiều thiết bị hơn bằng cách nâng cấp giấy phépChloros

+.
* **Phương pháp loại bỏ hiệu ứng debayer nâng cao có nhận diện kết cấu:** phương pháp loại bỏ hiệu ứng debayer chất lượng cao có nhận diện cạnh kết hợp với mô hình khử nhiễu AI/ML, giúp loại bỏ gần như toàn bộ nhiễu do quá trình debayer gây ra.
* **Công thức chỉ số đa quang phổ tùy chỉnh:** nhập các chỉ số đa quang phổ tùy chỉnh vào các công cụ tính toán raster tạiChloros

, dành cho cả xử lý và môi trường thử nghiệm xem hình ảnh.
* **Tính toán biên (Linux

) &amp; Tính toán biên (Edge Computing):** ChạyChloros

trên các nền tảng x86_64 và ARM64 củaLinux

, bao gồm NVIDIA Jetson, để xử lý tại hiện trường và tính toán biên. Xem [Linux

Tổng quan](linux/linux-overview.md).

<p align="center"><a href="https://cloud.mapir.camera/pricing" class="button primary" data-icon="envira">Chloros+ Giá cả &amp; Đăng ký</a></p>

<figure><img src=".gitbook/assets/plus_prog.JPG" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_zoom.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_mode.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_meta.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_map_markers.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: cli.JPG shows the 1.1.0 CLI banner. Re-shoot a terminal running `chloros-cli --version` + `chloros-cli status` on the 1.2.0 build so the banner prints "Chloros CLI 1.2.0". -->
