# Tổng quan về Linux

Chloros 1.3.0 cung cấp hỗ trợ Linux gốc cho **CLI**và**Python SDK** — xử lý hình ảnh đa quang phổ ở chế độ không giao diện đồ họa, cùng với khả năng điều khiển trực tiếp camera LATTICE và cảm biến ánh sáng DAQ — trên các máy trạm, máy chủ Linux và các thiết bị biên NVIDIA Jetson.

{% hint style="info" %}
**Không có giao diện người dùng đồ họa (GUI) trên Linux.**Giao diện người dùng đồ họa Chloros Desktop chỉ dành riêng cho Windows. Người dùng Linux tương tác với Chloros thông qua [CLI](../CLI.md) và [Python SDK](../api-python-sdk.md). `.deb` sẽ thêm mục**Chloros CLI** vào menu ứng dụng của bạn — mục này chỉ đơn giản là mở một trình giả lập terminal đang chạy `chloros-cli`.
{% endhint %}

***

## Bảng hỗ trợ nền tảng

| Tính năng | Windows (GUI) | Windows (CLI/SDK) | Linux amd64 (CLI/ --- | --- | --- | --- |
| **Giao diện người dùng trên máy tính để bàn (Desktop GUI)** | Có | Không áp dụng | Không | Không |
| **Giao diện dòng lệnh (CLI)** (`chloros-cli`) | Có | Có | Có | Có |
| **Python SDK** (`chloros-sdk`) | Có | Có | Có | Có |
| **Quy trình xử lý hình ảnh** | Có | Có | Có | Có |
| **Điều khiển camera LATTICE (trực tiếp)** | Có (tab Camera) | Có (`chloros-cli lattice`, SDK) | Có | Có |
| **Cảm biến ánh sáng DAQ (trực tiếp)** | Có (tab Cảm biến ánh sáng) | Có (`chloros-cli daq pool-*`, SDK) | Có | Có |
| **Đồng bộ thời gian PTP (máy chủ là grandmaster)** | Có | Có (`chloros-cli time-sync`) | Có | Có |
| **Tăng tốc GPU (CUDA)** | Có | Có | Có | Có (JetPack 6) |
| **Bộ xử lý debayer nhận diện kết cấu** (cần GPU hỗ trợ CUDA) | Có (Chloros+) | Có (Chloros+) | Có (Chloros+) | Có (Chloros+) |
| **Thích ứng tính toán động** | Có | Có | Có | Có |
| **Backend dưới dạng dịch vụ hệ thống** (`chloros-backend@<user>.service`) | Không | Không | Có (tùy chọn) | Có (tùy chọn) |
| **Trình cập nhật tại chỗ** (`chloros-cli update`) | Không (chạy trình cài đặt) | Không (chạy trình cài đặt) | Có | Có |***

## Các kiến trúc được hỗ trợ

| Kiến trúc | Mô tả | Gói |
| --- | --- | --- |
| **amd64 (x86_64)** | Bộ xử lý máy tính để bàn/máy chủ tiêu chuẩn (Intel, AMD) | `chloros_<version>_amd64.deb` |
| **arm64 (aarch64)** | Bộ xử lý ARM — Dòng NVIDIA Jetson Orin (JetPack 6) | `chloros_<version>_arm64_jp6.deb` |

## Các bản phân phối Linux được hỗ trợ

* **Ubuntu 22.04 LTS hoặc mới hơn** (amd64)
* **Debian 12 hoặc mới hơn** (amd64)
* **NVIDIA JetPack 6** (arm64 — nền tảng Jetson Orin)***

## Những gì người dùng Linux nhận được

* **Chloros CLI** — giao diện dòng lệnh đầy đủ cho xử lý hàng loạt, tự động hóa và viết kịch bản
* **Chloros Python SDK** — giao diện Python lập trình dành cho các quy trình nghiên cứu và công cụ tùy chỉnh, được cài đặt bởi `.deb` cho hệ thống `python3` (một gói wheel phù hợp với phiên bản dành cho môi trường ảo (virtualenvs) có trong `/usr/lib/chloros/sdk/`; PyPI dành cho các máy không có gói Chloros)
* **Điều khiển camera LATTICE** — phát hiện, kết nối, cấu hình và ghi hình từ các camera LATTICE cũng như các mảng camera đa kênh đồng bộ thông qua `chloros-cli lattice` và SDK; gói `.deb` bao gồm môi trường chạy Arena SDK mà các camera yêu cầu
* **Điều khiển cảm biến ánh sáng DAQ** — kết nối các cảm biến DAQ-U/M/E, truyền phát quang phổ đã hiệu chuẩn và ghi lại các tệp `.daq` thông qua `chloros-cli daq pool-*` và SDK
* **Đồng bộ thời gian PTP** — backend Chloros chạy máy chủ chính PTP mà các camera LATTICE và cảm biến DAQ-E đồng bộ theo; kiểm tra nó bằng `chloros-cli time-sync` và duy trì hoạt động ở chế độ không giao diện người dùng thông qua đơn vị systemd `chloros-backend@<user>.service` (xem [Cài đặt trên Linux](linux-installation.md#always-on-ptp-for-headless-hosts))
* **Tự động hóa dự án** — chạy các dự án đã lưu ở chế độ không giao diện người dùng bằng `chloros-cli project` và `open_project` của SDK
* **Tăng tốc GPU** — Xử lý được tăng tốc bằng CUDA trên GPU NVIDIA (GPU máy tính để bàn trên amd64 và GPU của Jetson với JetPack 6)
* **Thích ứng tính toán động** — tự động phát hiện phần cứng và lựa chọn chiến lược xử lý, với tùy chọn ghi đè `CHLOROS_STRATEGY` như một lối thoát chuyên sâu
* **Tất cả tính năng xử lý** — cùng quy trình xử lý như trên Windows: hiệu chỉnh, chỉnh sửa hiệu ứng viền tối, chỉ số thực vật và mọi định dạng xuất
* **Các tính năng của Chloros+** — xử lý đa luồng (theo quy trình), Texture Aware debayer (trên GPU CUDA) và các chỉ số tùy chỉnh, với gói Chloros+ trả phí

## Những gì người dùng Linux không có

* **Giao diện người dùng trên máy tính để bàn** — không có giao diện đồ họa; mọi tương tác đều thông qua CLI hoặc Python SDK
* **Trình xem hình ảnh** — không có trình xem hình ảnh tương tác, chế độ xem lưới hoặc điểm đánh dấu trên bản đồ
* **Quản lý dự án trực quan** — các dự án được tạo và điều khiển thông qua lệnh CLI và các lệnh gọi SDK (phần cứng — máy ảnh, cảm biến, thiết bị thu thập — vẫn có thể điều khiển hoàn toàn từ terminal)***

## Yêu cầu về giấy phép

Quyền truy cập CLI và SDK yêu cầu **gói Chloros+ trả phí — Copper trở lên**(Copper, Bronze, Silver, Gold). Gói miễn phí**Iron** không có quyền truy cập CLI/SDK. Giới hạn này được áp dụng bởi hệ thống backend, không chỉ qua CLI:

| Tình huống | Phản hồi từ hệ thống backend |
| --- | --- |
| Chưa đăng nhập | `401` kèm theo `error_code: AUTH_REQUIRED` |
| Đã đăng nhập ở gói Iron miễn phí | `403` với `error_code: PLAN_UPGRADE_REQUIRED` |

`chloros-cli status` hoạt động trên mọi gói dịch vụ — đây là tuyến duy nhất được miễn trừ khỏi cơ chế kiểm soát — do đó lý do từ chối luôn được hiển thị.

***

## Bắt đầu trên Linux

1. **Cài đặt Chloros** — xem [Cài đặt trên Linux](linux-installation.md) để cài đặt `.deb`
2. **Kiểm tra** — `chloros-cli --version` hiển thị `Chloros CLI 1.3.0`; `chloros-cli selftest` thực hiện chẩn đoán 7 bước
3. **Kiểm tra Python SDK** — `.deb` sẽ cài đặt nó: `python3 -c "import chloros_sdk; print(chloros_sdk.__version__)"` (trong môi trường ảo virtualenv, `pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl`)
4. **Đăng nhập** — `chloros-cli login your@email.com 'your-password'` (mỗi máy chỉ cần thực hiện một lần; việc nâng cấp gói phần mềm sẽ giữ nguyên thông tin đăng nhập, gỡ bỏ gói phần mềm sẽ xóa thông tin này)
5. **Xử lý tập dữ liệu đầu tiên** — `chloros-cli process ~/datasets/flight001`

Đối với NVIDIA Jetson, hãy tham khảo [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) chuyên dụng để biết thông tin về cài đặt dành riêng cho nền tảng, hành vi nhiệt và triển khai thực tế.

***

## Các bước tiếp theo

* [Cài đặt trên Linux](linux-installation.md) — hướng dẫn cài đặt chi tiết, vị trí tệp và khắc phục sự cố cho amd64 và arm64
* [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) — Cài đặt dành riêng cho Jetson, hành vi bộ nhớ và nhiệt độ, triển khai thực tế
* [CLI: Dòng lệnh](../CLI.md) — hướng dẫn về CLI
* [API: Python SDK](../api-python-sdk.md) — hướng dẫn về SDK
* [Tham chiếu CLI](../reference/cli-reference.md) và [Tham chiếu SDK](../reference/sdk-reference.md) — danh sách đầy đủ các lệnh/API cho phiên bản 1.3.0
* [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md) — cách Chloros thích ứng với phần cứng của bạn

{% hint style="info" %}
**Đọc tài liệu này bằng cách lập trình.** Mỗi trang cũng được cung cấp dưới dạng Markdown thô tại URL riêng kèm theo `.md` (ví dụ: `https://mapir.gitbook.io/chloros/linux/linux-installation.md`), và chỉ mục của toàn bộ tài liệu hướng dẫn được đăng tại [`https://mapir.gitbook.io/chloros/llms.txt`](https://mapir.gitbook.io/chloros/llms.txt).
{% endhint %}
