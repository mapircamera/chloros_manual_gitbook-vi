---
metaLinks: {}
---

# Bắt đầu

<div data-full-width="false"><figure><img src=".gitbook/assets/chloros_logo_transparent.png" alt=""><figcaption></figcaption></figure></div>

Chloros
là một ứng dụng phần mềm của [MAPIR
](https://www.mapir.camera) dùng để xử lý hình ảnh đa quang phổ, điều khiển trực tiếp phần cứng MAPIR
và ghi lại dữ liệu cảm biến. Chloros
1.3.0 hỗ trợ toàn bộ dòng sản phẩm MAPIR
:

* **Máy ảnh Survey3** — xử lý các ảnh RAW+JPG thành bản đồ phản xạ và chỉ số thực vật đã được hiệu chuẩn. Xem [Các máy ảnh được hỗ trợ](supported-cameras.md).
* **Máy ảnh LATTICE** — kết nối trực tiếp các mô-đun máy ảnh đa phổ GigE, riêng lẻ hoặc dưới dạng mảng nhiều máy ảnh đồng bộ: xem trước, chụp và xử lý thành các sản phẩm độ rạng rỡ và phản xạ đã được hiệu chuẩn. Xem [phần LATTICE](lattice/README.md).
* **Cảm biến ánh sáng DAQ** — Cảm biến quang phổ DAQ-U (USB), DAQ-M (Bluetooth), và DAQ-E, DAQ-E-S (Ethernet): phổ đã hiệu chuẩn trực tiếp, `.daq` bản ghi, và ánh sáng chiếu xuống để xử lý độ phản xạ. Xem [phần DAQ](daq/README.md).

{% hint style="success" %}
**Các tính năng mới trong Chloros
1.3.0**: Trợ lý AI có thể điều khiển Chloros
thông qua `chloros-mcp`, và CLI
, SDK
cùng `chloros-mcp` đều cung cấp các tính năng tương tự. Các sản phẩm LATTICE index đã được cải tiến, mảng cảm biến đầy đủ hoạt động nhanh hơn và tương thích với mọi mạng, đồng thời hệ thống nền vẫn tiếp tục chạy giữa các lệnh. Xem danh sách “Có gì mới” bên dưới và [Tải xuống](download.md) để lấy các trình cài đặt.
{% endhint %}

{% hint style="info" %}
**Bạn đang sử dụng Chloros
với trợ lý AI?** Hướng dẫn này được thiết kế dành riêng cho mục đích đó. Hãy yêu cầu trợ lý của bạn truy cập:

* `https://mapir.gitbook.io/chloros/llms.txt` — chỉ mục có thể đọc được bởi máy tính của mọi trang.
* Bất kỳ trang nào dưới dạng Markdown thô — thêm `.md` vào URL
của trang đó (ví dụ: `https://mapir.gitbook.io/chloros/reference/cli-reference.md`).
* [Tài liệu tham khảo CLI
](reference/cli-reference.md) và [Tài liệu tham khảo SDK
](reference/sdk-reference.md) — các trang tham khảo đầy đủ, chứa giá trị chính xác, được viết dành riêng cho mô hình ngôn ngữ lớn (LLM).

Ví dụ về lời nhắc: *&quot;Đọc https://mapir.gitbook.io/chloros/reference/cli-reference.md, rồi viết một skript để đăng nhập và xử lý thư mục ~/flights/flight_001 thành các tệp GeoTIFF phản xạ + NDVI
.&quot;*

Hoặc để nó chạy trực tiếp Chloros
: `chloros-mcp`, một máy chủ MCP được cài đặt cùng với SDK
, tương thích với Claude Desktop, Claude Code, Cursor và các ứng dụng khách MCP khác.

Hướng dẫn đầy đủ: [Sử dụng Chloros
với Trợ lý AI](ai-assistants.md).
{% endhint %}

***

## Những điểm mới trong Chloros
1.3.0

* **Các trợ lý AI có thể điều khiển Chloros
.** `chloros-mcp`, một máy chủ MCP được cài đặt cùng với Python
SDK
, cho phép Claude Desktop, Claude Code, Cursor và các ứng dụng khách MCP khác xử lý hình ảnh cũng như điều khiển camera LATTICE, mảng cảm biến và cảm biến ánh sáng DAQ. Xem [Sử dụng Chloros
với các trợ lý AI](ai-assistants.md).
* **CLI
, SDK
và `chloros-mcp` đều cung cấp các tính năng tương tự.** Chỉ có đăng nhập, cấu hình DAQ-E và các máy chủ DAQ trực tiếp vẫn được duy trì trên CLI
vì lý do bảo mật. Một số tập lệnh phiên bản 1.2.0 cần được điều chỉnh: xem phần **Thay đổi trong phiên bản 1.3.0** ở đầu tài liệu [Tham chiếu CLI
](reference/cli-reference.md) và [Tham chiếu SDK
](reference/sdk-reference.md).
* **Các sản phẩm chỉ số LATTICE được cải thiện.** Chỉ số NDVI
và các sản phẩm chỉ số khác được xuất từ các bản chụp LATTICE đã được cải thiện, trong mọi chế độ xử lý.
* **Tùy chọn trình cài đặt Windows
.**Trong phần**Cài đặt hệ thống**, trình cài đặt cung cấp các tùy chọn**Bật đường dẫn dài của Windows
**và**Loại trừ backend Chloros
khỏi quét của Microsoft Defender**, cả hai đều được chọn mặc định. Tùy chọn đầu tiên cho phép các thư mục dự án có độ sâu vượt quá 260 ký tự; tùy chọn thứ hai ngăn việc quét thời gian thực làm chậm quá trình thu thập dữ liệu từ camera (khoảng 30% tốc độ thu thập ở các tốc độ cao). Trình cài đặt hiện cũng chỉ thêm thư mục CLI
vào `PATH` một lần duy nhất và xóa các mục trùng lặp mà các phiên bản trình cài đặt trước đó để lại. Xem [Tải xuống](download.md).
* **Các thư mục dự án quá sâu sẽ bị từ chối ngay từ đầu.** Khi tính năng đường dẫn dài của Windows
bị tắt (mặc định của Windows
), một thư mục dự án quá sâu so với giới hạn đường dẫn 260 ký tự sẽ bị từ chối trước khi bất kỳ thao tác nào được thực hiện, kèm theo thông báo về thư mục dự án dài nhất vẫn phù hợp và cách bật tính năng đường dẫn dài. Trước đây, trong trường hợp này, quá trình chạy sẽ không tạo ra bất kỳ sản phẩm nào. Khi bật đường dẫn dài, các sản phẩm trong các thư mục rất sâu giờ đây vẫn giữ được siêu dữ liệu (thẻ GPS và XMP) của chúng.
* **Mặt nạ giới hạn.** Khi quá trình tách dải (band unmix) của sản phẩm độ sáng (radiance) hoặc độ phản xạ (reflectance) từ LATTICE tạo ra giá trị dải âm, giá trị đó sẽ bị giới hạn xuống 0, và một mặt nạ theo từng pixel trong thư mục `clamp_masks/` bên cạnh các thư mục sản phẩm sẽ ghi lại các pixel và dải bị ảnh hưởng. Tên mặt nạ của sản phẩm được ghi trong thẻ `Chloros:UnmixClampMask`. Các chế độ xem trực tiếp hiện được cắt chính xác như khi xuất dữ liệu. Xem [Định dạng hình ảnh đầu ra](output-image-formats.md).
* **Các sản phẩm tự mô tả bản thân.** Các sản phẩm chỉ mục mang các thẻ xử lý riêng (`Chloros:ProcessingLevel=index`, biểu thức chỉ mục và liệu chúng được tính toán từ độ sáng hay độ phản xạ) thay vì các thẻ của tệp thô nguồn. Các sản phẩm Survey3
mang các thẻ xử lý Chloros
trong mọi chế độ xử lý. Các tệp thô và tệp đã loại bỏ hiệu ứng Bayer của LATTICE vẫn giữ hướng gốc của cảm biến và ghi lại hướng của chúng trong XMP; Trình xem hình ảnh chỉ xoay chúng để hiển thị.
* **Các gói phần mềm Linux
được phân phối dưới dạng chưa giải nén.** Phần backend và giao diện dòng lệnh (CLI
) được cài đặt dưới dạng các thư mục trong thư mục `/usr/lib/chloros/`, do đó chúng không còn tự giải nén mỗi khi khởi động. Trên nền tảng x86_64, phần backend không còn mất khoảng 100 giây để khởi động, và `.deb` có kích thước chỉ bằng khoảng một nửa so với trước đây. ExifTool đi kèm hiện đã chạy được trên hệ thống Ubuntu mới cài đặt. `.deb` hiện cũng cài đặt Python
SDK
cho hệ thống `python3` (cài đặt bằng `sudo apt install ./chloros_1.3.0_<arch>.deb`), tăng dung lượng bộ đệm nhận socket của kernel mà các luồng camera cần, và thêm một dịch vụ backend chạy dưới quyền người dùng của bạn. Gói Jetson, `chloros_1.3.0_arm64_jp6.deb`, được xây dựng cho JetPack 6 và xử lý trên GPU của Jetson bằng phiên bản PyTorch hỗ trợ CUDA của JetPack. Trên Linux
, mỗi hình ảnh được xuất hiện được ghi vào đĩa một lần (được ghi và gắn thẻ trong bộ nhớ, sau đó sao chép vào vị trí đích): trên Jetson, nơi tốc độ đĩa quyết định thời gian xử lý, một lần chạy LATTICE với 288 hình ảnh mất 14 phút thay vì 24. Xem [Cài đặt Linux
](linux/linux-installation.md).
* **Camera LATTICE OCN
đọc dữ liệu chính xác hơn dưới ánh nắng mặt trời.** Độ rạng rỡ, độ phản xạ và các chỉ số được hiệu chỉnh nhanh chóng sau khi tách dải phổ, do đó mục tiêu màu xám sẽ cho giá trị NDVI
gần trung tính dưới ánh nắng. Camera RGN
không có thay đổi.
* **Báo cáo lỗi trong ứng dụng.** Biểu tượng lỗi ở đầu tab Nhật ký gỡ lỗi sẽ gửi mô tả, tối đa 10 hình ảnh, nhật ký và thông tin hệ thống đến MAPIR
. Xem [GUI: Điều hướng](navigation.md).
* **Mảng LATTICE.**Theo mặc định, các mảng giữ nguyên cảm biến đầy đủ, và việc truyền dữ liệu nén 12-bit (`--packed`, mặc định là `auto`) giúp tăng tốc độ khung hình của cảm biến đầy đủ lên khoảng một phần ba; mảng hoạt động trên bất kỳ mạng nào với tốc độ khung hình mà mạng đó cho phép (kết nối chậm sẽ làm giảm tốc độ khung hình thay vì từ chối mảng hoặc thu nhỏ kích thước khung hình); mảng có camera bị tắt/bật nguồn sẽ tự động phục hồi, với PTP được kích hoạt lại; chế độ phơi sáng tự động không còn làm giảm tốc độ đó trong điều kiện ánh sáng yếu (nó lấp đầy khoảng thời gian, sau đó tăng độ nhạy); Chế độ chụp liên tục theo kịp các mảng camera tốc độ cao; tính năng điều khiển**Highlights** dành cho camera RGB
; và các phiên bản `ArraySession`, `grab()`, `frame_stream()` và `continuous()`. Xem [phần LATTICE](lattice/README.md).
* **Phơi sáng tự động ổn định hơn.** Độ phơi sáng được duy trì ổn định khi ánh sáng không thay đổi và ổn định lại trong vòng vài giây sau khi có sự thay đổi; các vùng sáng nhỏ không còn làm tối cảnh quay; khi thiết lập phơi sáng thủ công, thông số gain vẫn được giữ ở chế độ Tự động; và máy ảnh ngừng phản hồi dưới ánh nắng chói chang sẽ được duy trì ở mức độ phơi sáng phù hợp với cường độ ánh sáng hiện tại, được đo bởi cảm biến ánh sáng hoặc, nếu không có cảm biến nào được kết nối, dựa trên các khung hình của chính máy ảnh. Xem [Cài đặt máy ảnh](lattice/camera-settings.md).
* **Cảm biến ánh sáng.** **Hỗ trợ DAQ-E-S:** bộ khuếch tán cố định của nó không cần hiệu chỉnh nắp, và mỗi lần đọc đều chứa thông tin về góc nghiêng, góc lăn và góc xoay của thiết bị. Một bản ghi sẽ cho biết nắp cảm biến đã được chọn hay được giả định, và quá trình xử lý sẽ cảnh báo nếu nắp được giả định: DAQ-U, DAQ-M và DAQ-E được xuất xưởng mà không có nắp, vì vậy hãy đặt tên cho nắp đã lắp. Các bản ghi trực tiếp `.daq` vẫn tồn tại sau khi mất điện (giữ lại tệp `.daq-journal` cùng với tệp `.daq`), và một thiết bị DAQ-E ngừng truyền dữ liệu sẽ được kết nối lại với khoảng trống được đánh dấu trong bản ghi của nó. Một dự án chỉ chứa các bản ghi từ cảm biến ánh sáng sẽ liệt kê và xử lý chúng từ ứng dụng và giao diện dòng lệnh (CLI
), và một bản ghi được tạo bằng `chloros_scripts` sẽ được liệt kê với tên DAQ đã tạo ra nó, kèm theo giới hạn mà bạn có thể thiết lập. Mỗi lần ghi lại đều lưu lại giá trị đọc từ DAQ đã sử dụng. Xem [phần DAQ](daq/README.md).
* **Xử lý.**Các khung hình LATTICE bắt đầu xử lý trong quá trình phát hiện mục tiêu. Các tham chiếu mục tiêu và DAQ được xác định theo từng dải tần. Khi dự án được di chuyển, các tệp của nó sẽ được liên kết lại. Các tệp xuất từ bản ghi 8-bit sẽ có định dạng 8-bit, với định dạng mới**TIFF
(8-bit)**. Một lần chạy Survey3
không có mục tiêu hiệu chuẩn sẽ được lưu dưới dạng sản phẩm dự phòng (`Vignette_Corrected_Images` hoặc `Sensor_Response_Images`), chứ không phải dưới dạng hình ảnh phản xạ. Các thẻ DLS theo dải tần của hình ảnh phản xạ luôn thuộc về khung hình của chính nó. Tính năng Texture Aware bị từ chối đối với các bản chụp LATTICE, vốn luôn được xử lý bằng thuật toán debayer tiêu chuẩn.
* **CLI
&amp; SDK
.** Các đường dẫn đầu ra tương đối hiện được lưu trong thư mục mà bạn chạy lệnh hoặc kịch bản (trước đây, backend đã cài đặt sẽ giải quyết chúng trong thư mục cài đặt riêng của nó và gây lỗi). Backend tiếp tục chạy giữa các lệnh và tập lệnh và tự động dừng sau 30 phút không sử dụng; `chloros-cli backend status` và `backend stop` quản lý điều này. Các lệnh chụp ảnh sẽ liệt kê tên từng tệp mà chúng không thể ghi được và thoát với mã 1; việc chụp ảnh lặp lại sẽ dừng lại khi ổ đĩa đầy. Một lần chạy `process` thành công sẽ báo cáo `Image products written: N (plus M clamp masks)`. Trên Windows
, `ChlorosLocal` của SDK
/ `process_folder` sẽ tìm backend đã cài đặt (và sử dụng backend đang chạy), còn `read_image_audit_tags` hoạt động từ một tệp `pip install` thuần túy.

***

## Những điểm mới trong Chloros
1.2.0

* **Điều khiển camera trực tiếp — tab Camera mới.** Kết nối camera LATTICE từng chiếc một hoặc dưới dạng mảng đa camera đồng bộ (đồng bộ thời gian PTP, chụp kích hoạt bằng phần cứng), với lớp phủ xem trước trực tiếp, biểu đồ tần suất theo dải, tự động phơi sáng thông minh, công cụ tính chỉ số trực tiếp và cập nhật firmware camera ngay trong ứng dụng.
* **Cảm biến ánh sáng — tab Cảm biến ánh sáng mới.** Kết nối các cảm biến DAQ-U (USB), DAQ-M (Bluetooth) và DAQ-E (Ethernet); xem phổ đã hiệu chuẩn trực tiếp (W/m²/nm), ghi tệp `.daq` vào dự án của bạn, chọn cấu hình hiệu chỉnh nắp và cập nhật phần mềm cơ sở DAQ-E qua mạng.
* **Chế độ chụp và bộ ghi.** Chế độ chụp Đơn / Liên tục / Theo khoảng thời gian cùng chế độ Chụp Nhanh Nhất chỉ ghi dữ liệu thô; lựa chọn theo dự án về các camera và loại tệp xuất mà &quot;Chụp Tất cả&quot; tạo ra; bộ ghi mảng cho video chỉ số chất lượng giám sát và các đợt dữ liệu thô chất lượng phân tích với khả năng xây dựng video ngoại tuyến.
* **Dòng xử lý LATTICE.** Nhập các thư mục ghi hình LATTICE và tách từng khung hình thô thành các sản phẩm đã khử hiệu ứng Bayer, xem trước, độ rạng rỡ float32 (W/m²/sr/nm) và độ phản xạ với các nút bật/tắt cho từng sản phẩm. Độ phản xạ có thể lấy từ mục tiêu hiệu chuẩn trong khung hình hoặc dữ liệu DAQ từ phía trên; việc căn chỉnh mảng được áp dụng cho các tệp xuất; thông tin hiệu chuẩn nhà máy bị thiếu sẽ được tải xuống tự động dựa trên số sê-ri máy ảnh.
* **Dự án ghi nhớ cấu hình phần cứng.** Các máy ảnh và cảm biến ánh sáng đã kết nối được lưu cùng với dự án (`cameras.json` / `sensors.json`) và sẽ tự động kết nối lại với các cài đặt đã lưu khi bạn mở lại dự án. Xem [GUI: Dự án](projects.md).
* **Cập nhật trình xem hình ảnh.** Hiển thị pixel/chỉ số con trỏ với tỷ lệ phản xạ chính xác theo từng tệp, biểu đồ histogram từng lớp, thanh trượt nhóm GSD, chế độ lưới &quot;Per Trigger&quot; / &quot;Per Camera&quot;, chế độ xem sản phẩm LATTICE, và xuất dữ liệu thử nghiệm chỉ số/LUT ra đĩa.
* **Giao diện dòng lệnh (CLI
) &amp; Bộ công cụ phát triển phần mềm (SDK
) được mở rộng đáng kể.** Các nhóm lệnh mới `lattice`, `daq pool-*`, `project` và `time-sync`; các tùy chọn mới `process` (`--input-level`, công tắc bật/tắt theo từng sản phẩm, `--reflectance-source`, cờ căn chỉnh mảng); các đối tượng kết nối thông minh SDK
(`connect_camera` / `connect_array` / `connect_daq_sensor`) tự động khởi động phần nền; Tự động hóa `open_project()`; bánh xe SDK
được gói cùng với các trình cài đặt và được phát hành trên PyPI dưới tên `chloros-sdk`.
* **Cơ chế xử lý lỗi minh bạch.** Một lần chạy `chloros-cli process` yêu cầu tạo sản phẩm nhưng không ghi được sản phẩm nào giờ đây sẽ báo lỗi rõ ràng và thoát với mã trạng thái khác 0; các lần chạy thành công sẽ báo cáo số lượng sản phẩm hình ảnh đã được ghi.
* **Cấu trúc đầu ra mới.** Các sản phẩm được lưu trong các thư mục `<project>/<camera>/<format>/<Product>_Images/` và giữ nguyên tên tệp nguồn — thư mục, chứ không phải hậu tố tên tệp, là yếu tố xác định sản phẩm. Xem [Định dạng hình ảnh đầu ra](output-image-formats.md).
* **Thêm đầu vào, gói dịch vụ và ngôn ngữ.** Hỗ trợ đầu vào `.dng`; đầy đủ 38 ngôn ngữ giao diện; giới hạn phần cứng theo từng gói dịch vụ với việc sử dụng miễn phí (không cần đăng nhập) tối đa 4 camera và 2 cảm biến ánh sáng.
* **Độ tin cậy.** Chức năng “Dừng xử lý” kết thúc một cách trơn tru kèm theo bản tóm tắt quá trình chạy chi tiết, các dự án đa camera xuất dữ liệu từ mọi camera, và các bản cập nhật trình cài đặt không còn khiến bạn bị đăng xuất.***

Chloros
có sẵn trên 3 nền tảng ứng dụng:

## Chloros
: Ứng dụng GUI trên máy tính để bàn

Cửa sổ độc lập riêng biệt với đầy đủ tính năng, bao gồm các tab Camera trực tiếp và Cảm biến ánh sáng. _Chỉ dành cho Windows
._

## [Chloros
CLI
: Giao diện dòng lệnh](CLI.md)

Xử lý hàng loạt trên dòng lệnh cùng các lệnh trực tiếp `lattice`, `daq pool-*`, `project` và `time-sync`. Hoàn hảo cho tự động hóa, lập kịch bản và vận hành không giao diện người dùng. Có sẵn trên **Windows
, Linux
amd64 và Linux
arm64 (NVIDIA Jetson)**. _CLI
yêu cầu gói Chloros
+ trả phí để truy cập._

## [Chloros
API
: Python
SDK
](api-python-sdk.md)

Giao diện Python
lập trình dành cho tự động hóa và quy trình làm việc tùy chỉnh: xử lý toàn bộ quy trình, phiên camera/mảng trực tiếp, phiên cảm biến DAQ và tự động hóa dự án đã lưu. Được cài đặt cùng với gói desktop/CLI và cũng được phát hành dưới tên `pip install chloros-sdk`. _API yêu cầu gói Chloros
+ trả phí để truy cập._

***

## Các nền tảng được hỗ trợ

| Nền tảng | Giao diện đồ họa (GUI) | Giao diện dòng lệnh (CLI
) | Python
SDK
|
| --- | --- | --- | --- |
| **Windows 10/11 (x64)** | Có | Có | Có |
| **Linux
amd64 (x86_64)** | Không | Có | Có |
| **Linux
arm64 (NVIDIA Jetson)** | Không | Có | Có |

Để biết hướng dẫn cài đặt trên Linux
, hãy tham khảo phần [Linux
&amp; Edge Computing](linux/linux-overview.md).

***

## Bắt đầu với ba bước

1. **Cài đặt** — tải xuống và chạy trình cài đặt dành cho nền tảng của bạn. Xem [Tải xuống](download.md).
2. **Đăng nhập (tùy chọn đối với giao diện đồ họa)** — giao diện đồ họa xử lý hình ảnh miễn phí mà không cần tài khoản. [Tài khoản Chloros
+](chloros+-login.md) cho phép xử lý song song, tăng tốc GPU, giới hạn thiết bị cao hơn và quyền truy cập CLI
/SDK
.
3. **Tạo dự án đầu tiên của bạn** — mở Chloros
, tạo [Dự án mới](projects.md), [thêm hình ảnh của bạn](processing-images-gui/adding-files-to-a-project.md) và [bắt đầu xử lý](processing-images-gui/starting-the-processing.md). Để điều khiển phần cứng trực tiếp, hãy mở tab &quot;Camera&quot; hoặc &quot;Cảm biến ánh sáng&quot; — xem [GUI: Điều hướng](navigation.md).

***

## Chloros


Mặc dù Chloros
miễn phí cho hầu hết các tác vụ, nhưng có thể bạn sẽ muốn nhiều tính năng hơn. Đó là lúc giấy phép trả phí cho Chloros
+ sẽ mang lại lợi ích cho bạn. Với giấy phép Chloros
+, bạn có thể mở khóa các tính năng mới như:

* **Xử lý đa luồng**: tăng tốc đáng kể quá trình xử lý hình ảnh cho các dự án quy mô lớn bằng cách xử lý đồng thời các hình ảnh qua quy trình xử lý.
* **Tăng tốc GPU (CUDA)**: tận dụng các tùy chọn bộ nhớ GPU cao hơn hiện nay để tăng tốc thêm quy trình xử lý hình ảnh. Chúng tôi khuyến nghị sử dụng 4GB VRAM trở lên để đạt kết quả tốt nhất.
* **Chloros
+**[**CLI**](CLI.md) **Truy cập**: chạy Chloros
+ từ dòng lệnh để tự động hóa và tích hợp vào phần mềm của riêng bạn. Có sẵn trên mọi gói trả phí; được áp dụng ở phía máy chủ.
* **Chloros
+**[**API**](api-python-sdk.md) **Truy cập:** chạy Chloros
+ từ Python
để điều khiển bằng lập trình, cho phép tích hợp liền mạch với các quy trình nghiên cứu, quy trình phân tích dữ liệu và các ứng dụng tùy chỉnh của bạn. Có sẵn trên mọi gói trả phí; được áp dụng ở phía máy chủ.
* **Giới hạn phần cứng cao hơn**: kết nối nhiều camera và cảm biến ánh sáng cùng lúc. Nếu không đăng nhập, giao diện người dùng (GUI) chỉ hỗ trợ tối đa 4 camera và 2 cảm biến ánh sáng DAQ; các gói trả phí sẽ nâng cao cả hai giới hạn này:

| Gói | Camera | Cảm biến ánh sáng DAQ |
| --- | --- | --- |
| Iron (miễn phí, không cần đăng nhập) | 4 | 2 |
| Copper / Bronze | 6 | 3 |
| Silver | 10 | 6 |
| Gold | 20 | 12 |

* **Sử dụng nhiều thiết bị**: mỗi giấy phép Chloros
+ cho phép đăng ký từ 2 thiết bị trở lên. Sử dụng tài khoản MAPIR
Cloud của bạn để quản lý các thiết bị đã đăng ký. Nâng cấp giấy phép Chloros
+ để hỗ trợ thêm nhiều thiết bị hơn.
* **Phương pháp khử răng cưa nâng cao nhận biết kết cấu:** phương pháp khử răng cưa chất lượng cao nhận biết các cạnh kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ nhiễu do quá trình khử răng cưa.
* **Công thức chỉ số đa quang phổ tùy chỉnh:** nhập các chỉ số đa quang phổ tùy chỉnh vào các công cụ tính toán raster của Chloros
, cả cho quá trình xử lý lẫn môi trường thử nghiệm xem hình ảnh.
* **Linux
&amp; Edge Computing:** chạy Chloros
trên các nền tảng Linux
x86\_64 và ARM64, bao gồm NVIDIA Jetson để xử lý tại hiện trường và tại thiết bị đầu cuối. Xem [Tổng quan về Linux
](linux/linux-overview.md).

<p align="center"><a href="https://cloud.mapir.camera/pricing" class="button primary" data-icon="envira">Giá cả và đăng ký Chloros+</a></p>

<figure><img src=".gitbook/assets/plus_prog.JPG" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_zoom.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_mode.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_grid_meta.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/chloros_map_markers.gif" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: cli.JPG shows the 1.1.0 CLI banner. Re-shoot a terminal running `chloros-cli --version` + `chloros-cli status` on the 1.3.0 build so the banner prints "Chloros CLI 1.3.0". -->
