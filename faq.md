---
description: Frequently Asked Questions
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/faq
---

# Câu hỏi thường gặp

<details>

<summary>Tôi có thể xử lý hình ảnh từ các camera không thuộc thương hiệu MAPIR bằng Chloros không?</summary>

Không, Chloros chỉ hỗ trợ xử lý hình ảnh từ các máy ảnh MAPIR — các dòng sản phẩm Survey3 và LATTICE. Vui lòng xem danh sách [các mẫu camera được hỗ trợ](supported-cameras.md) để biết thêm thông tin. Chúng tôi cung cấp dịch vụ xử lý hình ảnh từ các camera khác trên nền tảng đám mây MAPIR, xem danh sách đầy đủ [tại đây](https://mapir.gitbook.io/mapir-cloud/supported-cameras).

</details>

<details>

<summary>Chloros có hỗ trợ camera LATTICE không?</summary>

Có. Chloros phiên bản 1.2.0 hỗ trợ các mô-đun camera LATTICE M3C và M3M từ đầu đến cuối: **điều khiển trực tiếp**— phát hiện, kết nối, xem trước và ghi hình từ tab “Cameras” trên giao diện người dùng (GUI), `chloros-cli lattice`, hoặc tại Python SDK, bao gồm cả các mảng camera đa kênh đồng bộ hóa bằng PTP — và**xử lý phóng xạ đầy đủ** cho các dữ liệu ghi hình (thô → loại bỏ hiệu ứng Bayer → độ sáng → độ phản xạ → chỉ số). Xem [Các camera được hỗ trợ](supported-cameras.md) và [Hướng dẫn LATTICE](lattice/README.md).

</details>

<details>

<summary>Tôi có thể hiệu chuẩn hình ảnh của mình về độ phản xạ mà không cần mục tiêu hiệu chuẩn không?</summary>

**Survey3

:** Không. Nếu không có hình ảnh của mục tiêu hiệu chuẩn được chụp cùng lúc với các hình ảnh không phải mục tiêu, bạn sẽ không thể liên hệ các giá trị pixel của hình ảnh với tỷ lệ phần trăm độ phản xạ đã biết. Nếu bạn cũng không bao gồm nhật ký từ cảm biến ánh sángMAPIR

thì phổ ánh sáng xung quanh sẽ không được đo, và kết quả độ phản xạ sẽ không chính xác.

**LATTICE:** Có. Độ phản xạ có thể được tham chiếu đến bức xạ xuống được đo bằng cảm biến ánh sáng DAQ thay vì bảng điều khiển (ρ = π·L/E). Khi mục tiêu trong khung hình *đã* vượt qua kiểm tra chất lượng (QA) có mặt, nó sẽ tự động trở thành tham chiếu tuyệt đối theo mặc định (`--reflectance-source auto`). Một trường hợp ngoại lệ: &quot;Độ phản xạ F988 được hiệu chuẩn bằng bảng đo độ phản xạ trong cảnh: dải phổ này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đóChloros

sẽ áp dụng dữ liệu chụp bảng đo gần nhất của bạn và giữ nguyên giá trị đó giữa các lần quan sát bảng đo.&quot; Xem [Mục tiêu hiệu chuẩn](calibration-targets.md).

</details>

<details>

<summary>Tôi có cần cảm biến ánh sáng DAQ không?</summary>

Không cần cho độ sáng: Dữ liệu độ sáng LATTICE được xuất từ quá trình hiệu chuẩn phóng xạ tại nhà máy của từng camera và không cần cả cảm biến DAQ lẫn mục tiêu hiệu chuẩn. Đối với **độ phản xạ**, bạn cần một tham chiếu cho ánh sáng môi trường — có thể là giá trị đo ánh sáng chiếu xuống từ cảm biến ánh sáng DAQ hoặc mục tiêu hiệu chuẩn trong khung hình. Cảm biến DAQ cho phép bạn tạo ra giá trị phản xạ đã hiệu chuẩn**mà không cần đặt bất kỳ tấm panel nào trong khung cảnh**. Các tệp `.daq` được ghi lại sẽ tự động được khớp với hình ảnh của bạn dựa trên dấu thời gian. Xem [Mục tiêu hiệu chuẩn](calibration-targets.md) và [Tham chiếuCLI

](reference/cli-reference.md).

</details>

<details>

<summary>Tôi có thể sử dụng Chloros với trợ lý AI (Claude, ChatGPT, v.v.) không?</summary>

Có — tài liệu hướng dẫn này và các trang CLI / SDK được thiết kế dành riêng cho mục đích đó:

* Chỉ mục đầy đủ của tài liệu hướng dẫn được cung cấp tại `https://mapir.gitbook.io/chloros/llms.txt` để các trợ lý AI có thể tìm thấy mọi trang.
* Mã nguồn Markdown thô của mỗi trang có sẵn tại địa chỉ trang viết thường URL kèm theo mã `.md` (ví dụ: `https://mapir.gitbook.io/chloros/reference/cli-reference.md`).
* [Tài liệu tham khảo CLI](reference/cli-reference.md) và [Tài liệu tham khảo SDK](reference/sdk-reference.md) được viết dành cho các mô hình ngôn ngữ lớn (LLM): các tham số chính xác, giá trị mặc định, ngữ nghĩa kết thúc và các lệnh có thể sao chép và dán.

Xem [Trợ lý AI](ai-assistants.md) để biết cách chỉ định trợ lý của bạn đến Chloros.

</details>

<details>

<summary>Các tệp đầu ra đã xử lý sẽ được lưu ở đâu?</summary>

Các sản phẩm được lưu trong thư mục dự án, được nhóm theo camera và sau đó theo định dạng tệp:

```
<project>/<camera-folder>/<format-folder>/<Product>_Images/
```

* **thư mục máy ảnh** — `LATT-<sensor>-<lens>-F<filter>` dành cho LATTICE, `<model>_<filter>` (ví dụ: `Survey3N_RGN`) dành cho Survey3
* **thư mục định dạng** — `tiff16`, `tiff8`, `png8`, `jpg8`, hoặc `tiff32`
* **thư mục sản phẩm** — `Reflectance_Calibrated_Images/`, `Debayered_Images/`, `Preview_Images/`, `Radiance_Images/` (luôn nằm trong `tiff32`), `<INDEX>_Index_Images/`**Các tệp đã xuất giữ nguyên tên của tệp nguồn — thư mục xác định sản phẩm, không phải phần mở rộng tên tệp.**Với tùy chọn `CLI`, thư mục dự án sẽ được tạo bên cạnh thư mục đầu vào trừ khi bạn truyền tham số ``-o``. Lưu ý rằng một lần chạy `chloros-cli process` yêu cầu sản phẩm nhưng không ghi ra sản phẩm nào sẽ in ra `Processing finished but wrote no image products.` và**thoát với giá trị không bằng 0**, do đó các skript có thể phát hiện điều này. Xem [Định dạng hình ảnh đầu ra](output-image-formats.md) và [Tài liệu tham khảo CLI](reference/cli-reference.md).

</details>

<details>

<summary>Tôi có thể chỉnh sửa hình ảnh trước khi xử lý trong Chloros không?</summary>

Không. Chloros giả định rằng dữ liệu đầu vào chưa bị sửa đổi. Không thay đổi tên tệp.

</details>

<details>

<summary>Tôi có thể thiết lập các máy ảnh Survey3 của MAPIR ở chế độ phơi sáng tự động và xử lý hình ảnh trong Chloros không?</summary>

Không. Bộ dữ liệu hình ảnh của Survey3 phải có độ phơi sáng cố định/khóa, do đó không được sử dụng tốc độ màn trập tự động hoặc ISO tự động. Tất cả hình ảnh từ cùng một mẫu camera phải có tốc độ màn trập và ISO (độ phơi sáng) giống hệt nhau.

Máy ảnh LATTICE không có hạn chế này: Chloros điều khiển độ phơi sáng theo thời gian thực (Smart AE) và mỗi lần chụp đều ghi lại độ phơi sáng và độ khuếch đại thực tế được sử dụng, mà quy trình đo xạ sẽ tính đến.

</details>

<details>

<summary>Chloros có thể xử lý hoặc phân tích hình ảnh orthomosaic không?</summary>

Không. Chỉ hỗ trợ các hình ảnh riêng lẻ từ máy ảnh MAPIR, không hỗ trợ các hình ảnh ghép như bản đồ ảnh ghép chính xác.

</details>

<details>

<summary>Làm thế nào để tăng tốc bước phát hiện mục tiêu trong Chloros?</summary>

Trong bảng trình duyệt tệp, việc chọn trước các hình ảnh mục tiêu ở cột bên phải sẽ yêu cầu Chloros chỉ tìm kiếm các mục tiêu hiệu chuẩn trong những hình ảnh đó, giúp tăng tốc đáng kể quá trình xử lý.

</details>

<details>

<summary>Nếu tôi định tải hình ảnh lên <a href="https://www.mapir.camera/collections/software/products/mapir-cloud-subscription">MAPIR Cloud,</a> liệu tôi có nên xử lý trên Chloros trước khi tải lên không?</summary>

Nếu bạn có kế hoạch tải lên nền tảng xử lý trực tuyến của chúng tôi [MAPIR

Cloud](https://www.mapir.camera/collections/software/products/mapir-cloud-subscription), vui lòng không chỉnh sửa các hình ảnh trước khi tải lên. Cloud sẽ thực hiện tất cả các bước xử lý tương tự và nhiều hơn thế nữa.

</details>

<details>

<summary>MAPIR có bao giờ hỗ trợ tính năng X không? Tôi thực sự mong MAPIR cung cấp tính năng X.</summary>

Chúng tôi luôn quan tâm đến việc nhận phản hồi về các sản phẩm của mình. Nếu bạn phát hiện vấn đề với sản phẩm của chúng tôi hoặc có đề xuất về cách cải thiện sản phẩm, vui lòng [LIÊN HỆ VỚI CHÚNG TÔI](https://www.mapir.camera/community/contact) để chia sẻ ý kiến của bạn. Hầu hết các hoạt động nghiên cứu và phát triển (R&amp;D) của chúng tôi đều được định hướng dựa trên việc lắng nghe những nhu cầu lớn nhất của khách hàng.

</details>

<details>

<summary>Chloros có hỗ trợ Linux không?</summary>

Có! Chloros phiên bản 1.2.0 hỗ trợ Linux với kiến trúc amd64 (x86_64) và arm64 (NVIDIA Jetson JetPack 6) thông qua các gói phần mềm `.deb`. Các thiết bị CLI và Python SDK được hỗ trợ đầy đủ trên Linux, bao gồm điều khiển trực tiếp camera LATTICE và cảm biến DAQ. Không có giao diện người dùng (GUI) cho Linux — tất cả tương tác đều thông qua [CLI](CLI.md) hoặc [Python SDK](api-python-sdk.md). Xem [Linux Tổng quan](linux/linux-overview.md) để biết thêm chi tiết.

</details>

<details>

<summary>Tôi có thể chạy Chloros trên NVIDIA Jetson không?</summary>

Có! Chloros hỗ trợ các nền tảng NVIDIA Jetson bao gồm Jetson Nano, Orin Nano, Orin NX và AGX Orin chạy JetPack 6. Chloros tự động phát hiện mẫu Jetson của bạn và tối ưu hóa chiến lược xử lý của nó. Xem [Hướng dẫn NVIDIA Jetson](linux/nvidia-jetson-guide.md) để biết hướng dẫn cài đặt và triển khai.

</details>

<details>

<summary>Chloros có tự động tối ưu hóa cho phần cứng của tôi không?</summary>

Có! Chloros bao gồm [Dynamic Compute Adaptation](processing-architecture/dynamic-compute-adaptation.md) giúp tự động phát hiện CPU, GPU, RAM và (trên Jetson) các cảm biến nhiệt độ của bạn. Sau đó, hệ thống sẽ chọn chiến lược xử lý tối ưu — từ `GPU_PARALLEL` trên các hệ thống có bộ nhớ cao đến `GPU_SINGLE` trên các thiết bị có tài nguyên hạn chế và `CPU_PARALLEL` trên các hệ thống không có GPU NVIDIA. Không cần cấu hình thủ công.

</details>

<details>

<summary>Đường ống xử lý 4 luồng là gì?</summary>

Chloros

Sử dụng kiến trúc đường ống 4 luồng dành cho người dùngChloros

+: Luồng 1 (Phát hiện) tải hình ảnh và phát hiện các mục tiêu hiệu chuẩn, Luồng 2 (Hiệu chuẩn) tính toán hiệu chuẩn phản xạ, Luồng 3 (Xử lý) thực hiện khử hiệu ứng Bayer và tính toán chỉ số được tăng tốc bằng GPU, và Luồng 4 (Xuất) ghi các tệp đầu ra. Nhiều hình ảnh có thể được xử lý đồng thời trong các luồng khác nhau để đạt thông lượng tối đa. Xem [Đường ống xử lý](processing-architecture/processing-pipeline.md) để biết thêm chi tiết.

</details>

<details>

<summary>Làm thế nào để chạy chẩn đoán trên cài đặt Chloros của tôi?</summary>

Sử dụng lệnh `selftest` để chạy bài kiểm tra nhanh 7 bước: phiên bản, tính khả dụng của cổng, khởi động backend, kết nối với API (`/api/test`), thông tin hệ thống (`/api/system-info` — GPU/CUDA/PyTorch), sự hiện diện của mô hình khử nhiễu và tình trạng sẵn sàng của CUDA + mô hình khử nhiễu:

```bash
chloros-cli selftest
```

Điều này đặc biệt hữu ích trên các hệ thống Linux /Jetson để xác minh cấu hình GPU và CUDA.

</details>
