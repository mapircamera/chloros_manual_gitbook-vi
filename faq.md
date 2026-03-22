---
description: Frequently Asked Questions
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/faq
---

# Câu hỏi thường gặp

<chi tiết>

<tóm tắt>Tôi có thể xử lý hình ảnh từ máy ảnh không nhãn hiệu MAPIR bằng Chloros không?</summary>

Không, Chloros chỉ hỗ trợ xử lý hình ảnh camera MAPIR. Vui lòng xem danh sách [các mẫu máy ảnh được hỗ trợ] (supported-cameras.md) để biết thêm thông tin. Chúng tôi cung cấp khả năng xử lý các camera khác trên Đám mây MAPIR, xem danh sách đầy đủ [tại đây](https://mapir.gitbook.io/mapir-cloud/supported-cameras).

</ chi tiết>

<chi tiết>

<summary>Tôi có thể hiệu chỉnh hình ảnh của mình để có độ phản chiếu mà không cần mục tiêu hiệu chỉnh không?</summary>

Không. Nếu không có hình ảnh của mục tiêu hiệu chỉnh được chụp xung quanh khi chụp các hình ảnh không phải mục tiêu, bạn sẽ không thể liên hệ các giá trị pixel của hình ảnh với phần trăm phản xạ đã biết. Nếu bạn cũng không bao gồm nhật ký từ cảm biến ánh sáng MAPIR thì quang phổ ánh sáng xung quanh sẽ không được đo và kết quả phản xạ sẽ không chính xác.

</ chi tiết>

<chi tiết>

<tóm tắt>Tôi có thể chỉnh sửa hình ảnh của mình trước khi xử lý trong Chloros không?</summary>

Số Chloros giả định dữ liệu đầu vào chưa được sửa đổi. Không thay đổi tên tập tin.

</ chi tiết>

<chi tiết>

<tóm tắt>Tôi có thể đặt máy ảnh MAPIR Survey3 của mình ở chế độ tự động phơi sáng và xử lý hình ảnh trong Chloros không?</summary>

Không. Bộ dữ liệu hình ảnh Survey3 phải có độ phơi sáng cố định/khóa, do đó không có tốc độ màn trập tự động hoặc ISO tự động. Tất cả các hình ảnh của cùng một mẫu máy ảnh phải có tốc độ màn trập và ISO (phơi sáng) giống hệt nhau.

</ chi tiết>

<chi tiết>

<tóm tắt>Chloros có thể xử lý hoặc phân tích hình ảnh trực giao không?</tóm tắt>

Không. Chỉ hỗ trợ các hình ảnh camera MAPIR riêng lẻ, không hỗ trợ các hình ảnh được ghép như bản đồ trực giao.

</ chi tiết>

<chi tiết>

<tóm tắt>Làm cách nào để tăng tốc bước phát hiện mục tiêu của Chloros?</summary>

Trong bảng trình duyệt tệp, việc chọn trước các hình ảnh mục tiêu ở cột bên phải sẽ yêu cầu Chloros chỉ xem các hình ảnh đó để tìm mục tiêu hiệu chỉnh, giúp tăng tốc đáng kể quá trình xử lý.

</ chi tiết>

<chi tiết>

<summary>Nếu tôi tải hình ảnh của mình lên <a href="https://www.mapir.camera/collections/software/products/mapir-cloud-subscription">MAPIR Cloud</a> thì tôi có nên xử lý trong Chloros trước khi tải lên không?</summary>

Nếu bạn định tải lên nền tảng xử lý trực tuyến của chúng tôi [MAPIR Cloud](https://www.mapir.camera/collections/software/products/mapir-cloud-subscription), không chỉnh sửa hình ảnh trước khi tải lên. Đám mây sẽ thực hiện tất cả quá trình xử lý tương tự và hơn thế nữa.

</ chi tiết>

<chi tiết>

<tóm tắt>MAPIR có hỗ trợ tính năng X không? Tôi thực sự mong muốn MAPIR được cung cấp X.</summary>

Chúng tôi luôn mong muốn nhận được phản hồi về sản phẩm của mình. Nếu bạn phát hiện vấn đề với sản phẩm của chúng tôi hoặc có đề xuất về cách chúng tôi có thể cải thiện sản phẩm của mình, vui lòng [LIÊN HỆ VỚI CHÚNG TÔI](https://www.mapir.camera/community/contact) để chia sẻ suy nghĩ của bạn. Hầu hết hoạt động R\&D của chúng tôi đều được hướng dẫn bằng cách lắng nghe những nhu cầu lớn nhất của khách hàng.

</ chi tiết>

<chi tiết>

<tóm tắt>Chloros có sẵn cho Linux không?</tóm tắt>

Đúng! Chloros 1.1.0 hỗ trợ Linux amd64 (x86\_64) và arm64 (NVIDIA Jetson JetPack 6) thông qua các gói `.deb`. CLI và Python SDK được hỗ trợ đầy đủ trên Linux. Không có GUI cho Linux — tất cả tương tác đều thông qua [CLI](CLI.md) hoặc [Python SDK](api-python-sdk.md). Xem [Tổng quan về Linux](linux/linux-overview.md) để biết chi tiết.

</ chi tiết>

<chi tiết>

<tóm tắt>Tôi có thể chạy Chloros trên NVIDIA Jetson không?</tóm tắt>

Đúng! Chloros 1.1.0 hỗ trợ các nền tảng NVIDIA Jetson bao gồm Jetson Nano, Orin Nano, Orin NX và AGX Orin chạy JetPack 6. Chloros tự động phát hiện mô hình Jetson của bạn và tối ưu hóa chiến lược xử lý của nó. Xem [Hướng dẫn NVIDIA Jetson](linux/nvidia-jetson-guide.md) để biết hướng dẫn thiết lập và triển khai.

</ chi tiết>

<chi tiết>

<tóm tắt>Chloros có tự động tối ưu hóa cho phần cứng của tôi không?</tóm tắt>

Đúng! Chloros 1.1.0 bao gồm [Thích ứng điện toán động](processing-architecture/dynamic-compute-adaptation.md) tự động phát hiện cảm biến nhiệt CPU, GPU, RAM và (trên Jetson) của bạn. Sau đó, nó sẽ chọn chiến lược xử lý tối ưu — từ `GPU_PARALLEL` trên hệ thống có bộ nhớ cao đến `GPU_SINGLE` trên các thiết bị bị giới hạn đến `CPU_PARALLEL` trên các hệ thống không có GPU NVIDIA. Không cần cấu hình thủ công.

</ chi tiết>

<chi tiết>

<summary>Quy trình xử lý 4 luồng là gì?</summary>

Chloros 1.1.0 sử dụng kiến ​​trúc đường ống 4 luồng cho người dùng Chloros+: Luồng 1 (Phát hiện) tải hình ảnh và phát hiện mục tiêu hiệu chỉnh, Luồng 2 (Hiệu chỉnh) tính toán hiệu chỉnh phản xạ, Luồng 3 (Đang xử lý) thực hiện tính toán chỉ số và gỡ lỗi được tăng tốc GPU và Luồng 4 (Xuất) ghi các tệp đầu ra. Nhiều hình ảnh có thể ở các luồng khác nhau cùng một lúc để có thông lượng tối đa. Xem [Đường ống xử lý](processing-architecture/processing-pipeline.md) để biết chi tiết.

</ chi tiết>

<chi tiết>

<tóm tắt>Làm cách nào để chạy chẩn đoán trên bản cài đặt Chloros của tôi?</summary>

Sử dụng lệnh `selftest` để chạy 7 chẩn đoán hệ thống bao gồm kiểm tra phiên bản, tính khả dụng của cổng, khởi động phụ trợ, kết nối API, thông tin hệ thống, mô hình khử nhiễu và tính khả dụng của CUDA:

```bash
chloros-cli selftest
```

Điều này đặc biệt hữu ích trên các hệ thống Linux/Jetson để xác minh thiết lập GPU và CUDA.

</ chi tiết>