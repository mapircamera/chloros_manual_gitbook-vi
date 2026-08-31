# Tổng quan về Linux

Chloros 1.2.0 cung cấp hỗ trợ Linux tích hợp sẵn cho **CLI**và**Python SDK** — xử lý hình ảnh đa quang phổ không cần màn hình, cùng với khả năng điều khiển trực tiếp camera LATTICE và cảm biến ánh sáng DAQ — trên các máy trạm, máy chủ Linux và các thiết bị biên NVIDIA Jetson.

{% hint style="info" %}
**Linux không có giao diện người dùng đồ họa (GUI) trên máy tính để bàn.**Giao diện người dùng đồ họa (GUI) trên máy tính để bàn của Chloros chỉ hoạt động trên Windows. Người dùng Linux tương tác với Chloros thông qua [CLI](../CLI.md) và [Python SDK](../api-python-sdk.md). `.deb` sẽ thêm mục**Chloros CLI** vào menu ứng dụng của bạn — mục này chỉ đơn giản là mở một trình giả lập terminal đang chạy `chloros-cli`.
{% endhint %}

***

## Bảng hỗ trợ nền tảng

| Tính năng | Windows (GUI) | Windows (CLI / SDK) | Linux amd64 (CLI / SDK) | Linux arm64 / Jetson (CLI / SDK) |
| --- | --- | --- | --- | --- |
| **Giao diện người dùng trên máy tính để bàn (Desktop GUI)** | Có | Không áp dụng | Không | Không |
| **Giao diện người dùng trên thiết bị di động (CLI)** (`chloros-cli`) | Có | Có | Có | Có |
| **Cảm biến ánh sáng PythonSDK** (`chloros-sdk`) | Có | Có | Có | Có |
| **Dòng xử lý hình ảnh** | Có | Có | Có | Có |
| **Điều khiển camera LATTICE (trực tiếp)** | Có (tab Camera) | Có (`chloros-cli lattice`, SDK) | Có | Có |
| **Cảm biến ánh sáng DAQ (trực tiếp)** | Có (tab Cảm biến ánh sáng) | Có (`chloros-cli daq pool-*`, SDK) | Có | Có |
| **Đồng bộ thời gian PTP (máy chủ là grandmaster)** | Có | Có (`chloros-cli time-sync`) | Có | Có |
| **Tăng tốc GPU (CUDA)** | Có | Có | Có | Có (JetPack 6) |
| **Bộ khử hiệu ứng debayer nhận diện kết cấu** | Có (Chloros trở lên) | Có (Chloros trở lên) | Có (Chloros trở lên) | Có (Chloros trở lên) |
| **Thích ứng tính toán động** | Có | Có | Có | Có |
| **Backend dưới dạng dịch vụ hệ thống** (`chloros-backend.service`) | Không | Không | Có (tùy chọn) | Có (tùy chọn) |
| **Công cụ cập nhật tại chỗ** (`chloros-cli update`) | Không (chạy trình cài đặt) | Không (chạy trình cài đặt) | Có | Có |***

## Các kiến trúc được hỗ trợ

| Kiến trúc | Mô tả | Gói |
| --- | --- | --- |
| **amd64 (x86_64)** | Bộ xử lý máy tính để bàn/máy chủ tiêu chuẩn (Intel, AMD) | `chloros_<version>_amd64.deb` |
| **arm64 (aarch64)** | Bộ xử lý ARM — Dòng NVIDIA Jetson Orin | `chloros_<version>_arm64_jp6.deb` (bản dựng JetPack 6) |

## Các bản phân phối Linux được hỗ trợ

* **Ubuntu 22.04 LTS hoặc mới hơn** (amd64)
* **Debian 12 hoặc mới hơn** (amd64)
* **NVIDIA JetPack 6** (arm64 — nền tảng Jetson Orin)***

## Những gì người dùng Linux nhận được

* **Chloros CLI** — giao diện dòng lệnh đầy đủ dành cho xử lý hàng loạt, tự động hóa và lập kịch bản
* **Chloros Python SDK** — giao diện lập trình Python dành cho các quy trình nghiên cứu và công cụ tùy chỉnh (có thể cài đặt từ PyPI, đồng thời được đóng gói sẵn trong `.deb` dưới dạng gói wheel tương thích phiên bản)
* **Điều khiển camera LATTICE** — phát hiện, kết nối, cấu hình và thu thập dữ liệu từ các camera LATTICE cũng như các mảng camera đa kênh đồng bộ thông qua `chloros-cli lattice` và SDK; `.deb` tích hợp sẵn môi trường chạy Arena SDK mà các camera yêu cầu
* **Điều khiển cảm biến ánh sáng DAQ** — kết nối các cảm biến DAQ-U/M/E, truyền phát phổ đã hiệu chuẩn và ghi lại các tệp `.daq` thông qua `chloros-cli daq pool-*` và SDK
* **Đồng bộ thời gian PTP** — phần nền Chloros chạy máy chủ PTP chính mà các camera LATTICE và cảm biến DAQ-E đồng bộ theo; kiểm tra nó bằng `chloros-cli time-sync`, và duy trì hoạt động ở chế độ không giao diện người dùng thông qua đơn vị systemd `chloros-backend.service` (xem [Cài đặt Linux](linux-installation.md#always-on-ptp-for-headless-hosts))
* **Tự động hóa dự án** — chạy các dự án đã lưu ở chế độ không giao diện người dùng với `chloros-cli project` và tệp cấu hình `SDK` (`open_project`)
* **Tăng tốc GPU** — xử lý được tăng tốc bằng CUDA trên GPU NVIDIA (máy tính để bàn và Jetson)
* **Thích ứng tính toán động** — tự động phát hiện phần cứng và lựa chọn chiến lược xử lý, với tùy chọn ghi đè `CHLOROS_STRATEGY` như một lối thoát chuyên sâu
* **Tất cả các tính năng xử lý** — cùng một quy trình xử lý như Windows: hiệu chỉnh, chỉnh sửa hiệu ứng viền tối, chỉ số thực vật và mọi định dạng xuất
* **Các tính năng Chloros+** — xử lý đa luồng (theo chuỗi), thuật toán loại bỏ hiệu ứng debayering nhận biết kết cấu (Texture Aware debayer) và chỉ số tùy chỉnh, với gói Chloros+ trả phí

## Những gì người dùng Linux không có

* **Giao diện người dùng trên máy tính để bàn (Desktop GUI)** — không có giao diện đồ họa; mọi tương tác đều thông qua CLI hoặc Python SDK
* **Trình xem hình ảnh** — không có trình xem hình ảnh tương tác, chế độ xem dạng lưới hoặc điểm đánh dấu trên bản đồ
* **Quản lý dự án trực quan** — các dự án được tạo và điều khiển thông qua các lệnh CLI và các lệnh gọi SDK (phần cứng — máy ảnh, cảm biến, thiết bị ghi hình — vẫn có thể được điều khiển hoàn toàn từ thiết bị đầu cuối)***

## Yêu cầu về giấy phép

Việc truy cập CLI và SDK yêu cầu gói **Chloros+ trả phí — Copper trở lên**(Copper, Bronze, Silver, Gold). Gói miễn phí**Iron** không có quyền truy cập CLI / SDK. Giới hạn này được áp dụng bởi hệ thống backend, không chỉ bởi CLI:

| Tình huống | Phản hồi từ hệ thống backend |
| --- | --- |
| Chưa đăng nhập | `401` với `error_code: AUTH_REQUIRED` |
| Đã đăng nhập trên gói Iron miễn phí | `403` với `error_code: PLAN_UPGRADE_REQUIRED` |

`chloros-cli status` hoạt động trên mọi gói dịch vụ — đây là tuyến duy nhất được miễn kiểm tra — do đó, lý do từ chối luôn hiển thị rõ ràng.

***

## Bắt đầu sử dụng Linux

1. **Cài đặt Chloros** — xem [Hướng dẫn cài đặt Linux](linux-installation.md) để cài đặt `.deb`
2. **Kiểm tra** — `chloros-cli --version` hiển thị thông báo `Chloros CLI 1.2.0`; `chloros-cli selftest` thực hiện quy trình chẩn đoán 7 bước
3. **Cài đặt Python SDK** (tùy chọn) — `pip install chloros-sdk`
4. **Đăng nhập** — `chloros-cli login your@email.com 'your-password'` (một lần cho mỗi máy và lặp lại sau mỗi lần nâng cấp gói phần mềm)
5. **Xử lý tập dữ liệu đầu tiên** — `chloros-cli process ~/datasets/flight001`

Đối với NVIDIA Jetson, hãy tham khảo [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) chuyên dụng để biết thông tin về thiết lập dành riêng cho nền tảng, hành vi nhiệt và triển khai thực tế.

***

## Các bước tiếp theo

* [Cài đặt Linux](linux-installation.md) — hướng dẫn cài đặt chi tiết, vị trí tệp và khắc phục sự cố cho amd64 và arm64
* [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) — Cài đặt dành riêng cho Jetson, hành vi bộ nhớ và nhiệt, triển khai thực tế
* [CLI : Dòng lệnh](../CLI.md) — Hướng dẫn về `CLI`
* [API : Python SDK](../api-python-sdk.md) — Hướng dẫn về `SDK`
* [Tài liệu tham khảo CLI](../reference/cli-reference.md) và [Tài liệu tham khảo SDK](../reference/sdk-reference.md) — danh sách đầy đủ các lệnh/API cho phiên bản 1.2.0
* [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md) — cách Chloros thích ứng với phần cứng của bạn

{% hint style="info" %}
**Đọc tài liệu này bằng cách lập trình.** Mỗi trang cũng được cung cấp dưới dạng Markdown thô tại địa chỉ URL tương ứng cộng với `.md` (ví dụ: `https://mapir.gitbook.io/chloros/linux/linux-installation.md`), và chỉ mục của toàn bộ tài liệu hướng dẫn được đăng tại [`https://mapir.gitbook.io/chloros/llms.txt`](https://mapir.gitbook.io/chloros/llms.txt).
{% endhint %}
