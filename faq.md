---
description: Frequently Asked Questions
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/faq
---

# Câu hỏi thường gặp

<details>

<summary>Tôi có thể xử lý hình ảnh từ các camera không phải thương hiệu MAPIR bằng Chloros không?</summary>

Không, Chloros chỉ hỗ trợ xử lý hình ảnh từ các camera MAPIR — dòng Survey3 và LATTICE. Vui lòng xem danh sách [các mẫu camera được hỗ trợ](supported-cameras.md) để biết thêm thông tin. Chúng tôi cung cấp dịch vụ xử lý hình ảnh từ các camera khác trên MAPIR Cloud, xem danh sách đầy đủ [tại đây](https://mapir.gitbook.io/mapir-cloud/supported-cameras).

</details>

<details>

<summary>Chloros có hỗ trợ máy ảnh LATTICE không?</summary>

Có. Chloros 1.3.0 hỗ trợ các mô-đun camera LATTICE M3C và M3M từ đầu đến cuối: **điều khiển trực tiếp**— phát hiện, kết nối, xem trước và ghi hình từ tab "Cameras" trên giao diện người dùng (GUI), `chloros-cli lattice`, hoặc SDK SDK, bao gồm mảng camera đa kênh đồng bộ hóa thời gian qua PTP — và**xử lý phóng xạ đầy đủ** cho dữ liệu ghi hình (thô → khử hiệu ứng Bayer → độ rạng rỡ → độ phản xạ → chỉ số). Xem [Các camera được hỗ trợ](supported-cameras.md) và [hướng dẫn LATTICE](lattice/README.md).

</details>

<details>

<summary>Tôi có thể hiệu chuẩn hình ảnh của mình về độ phản xạ mà không cần mục tiêu hiệu chuẩn không?</summary>

**Survey3:** Không. Nếu không có hình ảnh của mục tiêu hiệu chuẩn được chụp cùng lúc với các hình ảnh không phải mục tiêu, bạn sẽ không thể liên hệ các giá trị pixel của hình ảnh với tỷ lệ phần trăm phản xạ đã biết. Nếu bạn cũng không bao gồm nhật ký từ cảm biến ánh sáng MAPIR, thì quang phổ ánh sáng xung quanh sẽ không được đo, và kết quả độ phản xạ sẽ không chính xác.

**LATTICE:** Có. Độ phản xạ có thể được tham chiếu đến bức xạ xuống được đo bởi cảm biến ánh sáng DAQ thay vì tấm panel (ρ = π·L/E). Khi mục tiêu trong khung hình *đã* vượt qua kiểm tra chất lượng (QA), nó sẽ tự động trở thành tham chiếu tuyệt đối (`--reflectance-source auto`), trừ khi mục tiêu đó vượt qua với một cờ đánh dấu, khi đó mục tiêu và chuỗi DAQ sẽ cạnh tranh dựa trên điểm số đo được. Một trường hợp ngoại lệ: "Độ phản xạ F988 được hiệu chuẩn bằng tấm phản xạ trong khung hình: dải này nằm ngoài phạm vi hiệu chuẩn của cảm biến ánh sáng DAQ, do đó Chloros sẽ áp dụng dữ liệu chụp tấm phản xạ gần nhất của bạn và giữ nguyên giá trị đó giữa các lần quan sát tấm phản xạ." Xem [Mục tiêu hiệu chuẩn](calibration-targets.md).

</details>

<details>

<summary>Tôi có cần cảm biến ánh sáng DAQ không?</summary>

Không cần cho độ sáng: Dữ liệu độ sáng LATTICE được xuất từ quá trình hiệu chuẩn phóng xạ tại nhà máy của từng camera và không cần cả cảm biến DAQ lẫn mục tiêu hiệu chuẩn. Đối với **độ phản xạ**, bạn cần một tham chiếu cho ánh sáng môi trường — có thể là giá trị đo ánh sáng chiếu xuống từ cảm biến DAQ hoặc mục tiêu hiệu chuẩn trong khung hình. Cảm biến DAQ cho phép bạn tạo ra giá trị phản xạ đã hiệu chuẩn**mà không cần đặt bất kỳ tấm panel nào trong khung cảnh**. Các tệp `.daq` đã ghi sẽ được tự động khớp với hình ảnh của bạn dựa trên dấu thời gian. Xem [Mục tiêu hiệu chuẩn](calibration-targets.md) và [Tham chiếu CLI](reference/cli-reference.md).

</details>

<details>

<summary>Tôi có thể sử dụng Chloros với trợ lý AI (Claude, ChatGPT, v.v.) không?</summary>

Có — tài liệu hướng dẫn này cùng với CLI/SDK được thiết kế dành riêng cho mục đích đó:

* Chỉ mục đầy đủ của tài liệu hướng dẫn được cung cấp tại `https://mapir.gitbook.io/chloros/llms.txt` để các trợ lý AI có thể tìm thấy mọi trang.
* Mã nguồn Markdown thô của mỗi trang có sẵn tại URL trang tương ứng (viết thường) kèm theo `.md` (ví dụ: `https://mapir.gitbook.io/chloros/reference/cli-reference.md`).
* [Tài liệu tham khảo CLI](reference/cli-reference.md) và [Tài liệu tham khảo SDK](reference/sdk-reference.md) được viết dành cho các mô hình ngôn ngữ lớn (LLM): các tham số chính xác, giá trị mặc định, ngữ nghĩa thoát lệnh và các lệnh có thể sao chép-dán trực tiếp.

Xem [Trợ lý AI](ai-assistants.md) để biết cách thiết lập trợ lý của bạn sử dụng Chloros.

</details>

<details>

<summary>Các tệp đầu ra đã xử lý của tôi được lưu ở đâu?</summary>

Các sản phẩm được lưu trong thư mục dự án, được nhóm theo máy ảnh và sau đó theo định dạng tệp:

```
<project>/<camera-folder>/<format-folder>/<Product>_Images/
```

* **thư mục máy ảnh** — `LATT-<sensor>-<lens>-F<filter>` dành cho LATTICE, `<model>_<filter>` (ví dụ: `Survey3N_RGN`) dành cho Survey3
* **thư mục định dạng** — `tiff16`, `tiff8`, `png8`, `jpg8`, hoặc `tiff32`
* **thư mục sản phẩm** — `Reflectance_Calibrated_Images/`, `Debayered_Images/`, `Preview_Images/`, `Radiance_Images/` (luôn nằm trong thư mục `tiff32`), `<INDEX>_Index_Images/`
* **`clamp_masks/`** — Chỉ dành cho dữ liệu đa phổ LATTICE: các mặt nạ theo từng pixel nằm bên cạnh các thư mục sản phẩm, xem bên dưới**Các tệp được xuất giữ nguyên tên tệp nguồn — thư mục xác định sản phẩm, không phải phần mở rộng tên tệp.**Với CLI, thư mục dự án được tạo trong thư mục dự án mặc định của bạn (`Documents/MAPIR/Chloros Projects` trừ khi bạn đã thay đổi) trừ khi bạn truyền tham số `-o`; một đường dẫn tương đối `-o` sẽ được tính từ thư mục bạn chạy lệnh. Lưu ý rằng một lần chạy `chloros-cli process` yêu cầu các sản phẩm nhưng không ghi ra bất kỳ sản phẩm nào sẽ hiển thị `Processing finished but wrote no image products.` và**thoát với giá trị không bằng 0**, do đó các skript có thể phát hiện điều này. Xem [Các định dạng hình ảnh đầu ra](output-image-formats.md) và [Tham chiếu CLI](reference/cli-reference.md).

</details>

<details>

<summary>Các thư mục clamp_masks và các tệp *_clampmask.tif là gì?</summary>

Tính năng mới trong phiên bản 1.3.0. Các sản phẩm radiance và reflectance từ camera đa phổ LATTICE M3C thực hiện quá trình tách dải 3×3; khi một dải nào đó cho ra giá trị âm, Chloros sẽ cắt giá trị đó về 0. Một sản phẩm có ít nhất một pixel bị cắt sẽ có một mặt nạ đơn dải `uint8` đánh dấu các pixel và dải nào đã bị cắt (bit 0 = kênh R của sản phẩm, bit 1 = G, bit 2 = B). Các mặt nạ được lưu trong thư mục `clamp_masks/` **bên cạnh** các thư mục sản phẩm — ví dụ: `tiff16/clamp_masks/Reflectance_Calibrated_Images/<name>_clampmask.tif` — và thẻ XMP của sản phẩm `Chloros:UnmixClampMask` sẽ ghi tên mặt nạ của nó (hoặc ghi là `none`).

Chúng không phải là hình ảnh của cảnh: quá trình nhập của Chloros sẽ bỏ qua chúng, và vì chúng không bao giờ nằm trong thư mục sản phẩm nên bạn có thể chuyển thư mục sản phẩm sang Pix4D hoặc Metashape nguyên trạng. Xem [Mặt nạ kẹp](output-image-formats.md#clamp-masks).

</details>

<details>

<summary>Tại sao Chloros lại hiển thị thông báo "Thư mục dự án quá sâu đối với máy tính này"?</summary>

Điều này chỉ áp dụng cho Windows. Windows được cài đặt sẵn với tùy chọn đường dẫn dài bị tắt, do đó đường dẫn tệp không được dài quá 260 ký tự. Các sản phẩm của Chloros nằm ở mức khoảng 130 ký tự dưới thư mục dự án, do đó Chloros sẽ từ chối thư mục dự án nằm sâu hơn trước khi xử lý — thay vì chạy mà không ghi lại gì, như có thể xảy ra trước phiên bản 1.3.0. Thông báo này sẽ hiển thị thư mục dự án dài nhất mà hệ thống vẫn chấp nhận được.

Khắc phục bằng một trong hai cách sau:

* Sử dụng thư mục dự án ngắn hơn (thông qua tùy chọn **Thư mục làm việc** trong Cài đặt Dự án, hoặc lệnh `-o` / `chloros-cli set-project-folder` cho giao diện dòng lệnh CLI).
* Bật tính năng đường dẫn dài của Windows và khởi động lại Chloros. Tùy chọn **Enable Windows long paths** (được chọn mặc định) trong trình cài đặt Chloros sẽ thực hiện việc này; hãy chạy lại trình cài đặt nếu bạn đã bỏ chọn tùy chọn này.

Xem [Bắt đầu xử lý](processing-images-gui/starting-the-processing.md#the-project-folder-is-too-deep-for-this-computer).

</details>

<details>

<summary>Tôi có thể chỉnh sửa hình ảnh trước khi xử lý trong Chloros không?</summary>

Không. Chloros giả định rằng dữ liệu đầu vào chưa được sửa đổi. Không thay đổi tên tệp.

</details>

<details>

<summary>Tôi có thể thiết lập máy ảnh MAPIR Survey3 của mình ở chế độ phơi sáng tự động và xử lý hình ảnh trong Chloros không?</summary>

Không. Bộ dữ liệu hình ảnh Survey3 phải có độ phơi sáng cố định/khóa, do đó không được sử dụng tốc độ màn trập tự động hoặc ISO tự động. Tất cả hình ảnh từ cùng một mẫu máy ảnh phải có tốc độ màn trập và ISO (độ phơi sáng) giống hệt nhau.

Máy ảnh LATTICE không có hạn chế này: Chloros điều khiển độ phơi sáng của chúng theo thời gian thực (Smart AE) và mỗi lần chụp đều ghi lại độ phơi sáng và độ khuếch đại thực tế được sử dụng, điều mà quy trình đo quang học sẽ tính đến.

</details>

<details>

<summary>Chloros có thể xử lý hoặc phân tích hình ảnh orthomosaic không?</summary>

Không. Chỉ hỗ trợ các hình ảnh riêng lẻ từ máy ảnh MAPIR, không hỗ trợ các hình ảnh ghép lại như bản đồ ghép chính xác.

</details>

<details>

<summary>Làm thế nào để tăng tốc bước phát hiện mục tiêu trong Chloros?</summary>

Trong bảng trình duyệt tệp, việc chọn trước các hình ảnh mục tiêu ở cột bên phải sẽ yêu cầu Chloros chỉ tìm kiếm các mục tiêu hiệu chuẩn trong những hình ảnh đó, giúp tăng tốc đáng kể quá trình xử lý.

</details>

<details>

<summary>Nếu tôi định tải hình ảnh lên <a href="https://www.mapir.camera/collections/software/products/mapir-cloud-subscription">MAPIR Cloud,</a> tôi có nên xử lý trong Chloros trước khi tải lên không?</summary>

Nếu bạn có kế hoạch tải lên nền tảng xử lý trực tuyến của chúng tôi [MAPIR Cloud](https://www.mapir.camera/collections/software/products/mapir-cloud-subscription), đừng chỉnh sửa hình ảnh trước khi tải lên. Cloud sẽ thực hiện tất cả các bước xử lý tương tự và hơn thế nữa.

</details>

<details>

<summary>MAPIR có bao giờ hỗ trợ tính năng X không? Tôi thực sự mong MAPIR cung cấp tính năng X.</summary>

Chúng tôi luôn quan tâm đến việc nhận phản hồi về các sản phẩm của mình. Nếu bạn phát hiện vấn đề với sản phẩm của chúng tôi hoặc có đề xuất về cách cải thiện sản phẩm, vui lòng [LIÊN HỆ VỚI CHÚNG TÔI](https://www.mapir.camera/community/contact) để chia sẻ ý kiến của bạn. Hầu hết các hoạt động R&D của chúng tôi đều được định hướng dựa trên việc lắng nghe những nhu cầu lớn nhất của khách hàng.

</details>

<details>

<summary>Chloros có sẵn cho Linux không?</summary>

Có! Chloros 1.3.0 hỗ trợ Linux amd64 (x86_64) và arm64 (NVIDIA Jetson JetPack 6) thông qua các gói `.deb`. Giao diện dòng lệnh (CLI) và SDK SDK được hỗ trợ đầy đủ trên Linux, bao gồm cả việc điều khiển camera LATTICE và cảm biến DAQ theo thời gian thực. Không có giao diện đồ họa (GUI) cho Linux — tất cả tương tác đều thông qua [CLI](CLI.md) hoặc [Python SDK](api-python-sdk.md). Xem [Tổng quan về Linux](linux/linux-overview.md) để biết thêm chi tiết.

</details>

<details>

<summary>Tôi có thể chạy Chloros trên NVIDIA Jetson không?</summary>

Có! Chloros hỗ trợ **dòng sản phẩm Orin** của NVIDIA Jetson — Orin Nano, Orin NX và AGX Orin — chạy trên JetPack 6. Các mẫu cũ hơn (Jetson Nano, TX2, Xavier NX) không thể chạy JetPack 6 và không được hỗ trợ bởi gói phần mềm hiện tại. Chloros tự động phát hiện mẫu Jetson của bạn và tối ưu hóa chiến lược xử lý. Gói phần mềm Jetson sử dụng phiên bản PyTorch được xây dựng trên CUDA của JetPack 6, do đó quá trình xử lý diễn ra trên GPU của Jetson. Xem [Hướng dẫn NVIDIA Jetson](linux/nvidia-jetson-guide.md) để biết hướng dẫn cài đặt và triển khai.

</details>

<details>

<summary>Chloros có tự động tối ưu hóa cho phần cứng của tôi không?</summary>

Có! Chloros tích hợp tính năng [Dynamic Compute Adaptation](processing-architecture/dynamic-compute-adaptation.md) giúp tự động phát hiện CPU, GPU, RAM và (trên Jetson) các cảm biến nhiệt độ. Sau đó, hệ thống sẽ chọn chiến lược xử lý tối ưu — từ `GPU_PARALLEL` trên các GPU có bộ nhớ lớn đến `GPU_SINGLE` trên các GPU nhỏ hơn và `CPU_PARALLEL` trên các hệ thống không có GPU CUDA mà PyTorch có thể sử dụng. Không cần cấu hình thủ công.

</details>

<details>

<summary>Đường ống xử lý 4 luồng là gì?</summary>

Chloros sử dụng kiến trúc đường ống 4 luồng cho người dùng Chloros+: Luồng 1 (Phát hiện) tải hình ảnh và phát hiện các mục tiêu hiệu chuẩn, Luồng 2 (Hiệu chuẩn) tính toán hiệu chuẩn phản xạ, Luồng 3 (Xử lý) thực hiện khử hiệu ứng bayering và tính toán chỉ số được tăng tốc bằng GPU, và Luồng 4 (Xuất) ghi các tệp đầu ra. Nhiều hình ảnh có thể được xử lý đồng thời trong các luồng khác nhau để đạt thông lượng tối đa. Xem [Đường ống xử lý](processing-architecture/processing-pipeline.md) để biết thêm chi tiết.

</details>

<details>

<summary>Làm thế nào để chạy chẩn đoán trên bản cài đặt Chloros của tôi?</summary>

Sử dụng lệnh `selftest` để chạy bài kiểm tra nhanh 7 bước: phiên bản, tính khả dụng của cổng, khởi động backend, kết nối API (`/api/test`), thông tin hệ thống (`/api/system-info` — GPU/CUDA/PyTorch), sự hiện diện của mô hình khử nhiễu và trạng thái sẵn sàng của CUDA + mô hình khử nhiễu:

```bash
chloros-cli selftest
```

Điều này đặc biệt hữu ích trên các hệ thống Linux để xác minh cài đặt GPU và CUDA. Trên Jetson, `CUDA: False` thường có nghĩa là thiếu các thư viện CUDA và cuDNN của JetPack 6: hãy cài đặt chúng bằng lệnh `sudo apt install nvidia-jetpack`.

</details>
