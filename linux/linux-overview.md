#Linux Tổng quan

Chloros 1.1.0 có hỗ trợ Linux dành riêng cho **CLI**và**Python SDK**, cho phép xử lý hình ảnh đa dạng không đầu trên máy tính, máy chủ và thiết bị biên dịch NVIDIA Jetson Linux.

{% hint style="info" %}
**Không có GUI trên Linux.** GUI máy tính để bàn Chloros chỉ sử dụng được trên Windows. Người dùng Linux tương tác với Chloros thông qua [CLI](../CLI.md) và [Python SDK](../api-python-sdk.md).
{% endhint %}

***

## Nền tảng hỗ trợ ma trận

| Tính năng | Windows (GUI) | Windows (CLI/SDK) | Linux AMD64 (CLI/SDK) | Linux arm64 / Jetson (CLI/SDK) |
| --- | --- | --- | --- | --- |
| **Giao diện máy tính để bàn** | Có | Không áp dụng | Không | Không |
| **CLI** | Có | Có | Có | Có |
| **Python SDK** | Có | Có | Có | Có |
| **Tăng tốc GPU (CUDA)** | Có | Có | Có | Có (JetPack 6) |
| **Gỡ lỗi cấu hình nhận dạng** | Có (Chloros+) | Có (Chloros+) | Có (Chloros+) | Có (Chloros+) |
| **Thích ứng tính toán**| Có | Có | Có | Có |***

## Hỗ trợ kiến trúc

| Kiến trúc | Description | Cài đặt phương pháp |
| --- | --- | --- |
| **AMD64 (x86_64)** | Bộ xử lý máy tính để bàn/máy chủ tiêu chuẩn (Intel, AMD) | Gói `.deb` |
| **arm64 (aarch64)** | Bộ xử lý dựa trên ARM, chủ yếu là NVIDIA Jetson | Gói `.deb` (JetPack 6) |

## Hỗ trợ bản phân phối Linux

* **Ubuntu 20.04+** (AMD64)
* **Debian 11+** (AMD64)
* **NVIDIA JetPack 6**(arm64 — nền Jetson)***

## Người dùng Linux nhận được gì

* **Chloros CLI** — Đầy đủ dòng lệnh giao diện để xử lý hàng loạt, tự động hóa và tạo lệnh
* **Chloros Python SDK** — Giao diện Python có trình cài đặt (`pip install chloros-sdk`) để phân tích hợp lý vào quy trình nghiên cứu và công cụ tùy chỉnh
* **Tăng tốc GPU** — Xử lý tăng tốc CUDA trên GPU NVIDIA (máy tính để bàn và Jetson)
* **Thích ứng điện toán** — Bộ xử lý chiến lược tối ưu hóa và phát hiện phần cứng tự động
* **Tất cả các tính năng xử lý** - Quy trình xử lý đa phổ tương tự như Windows (chuẩn hiệu, chỉnh sửa chi tiết, chỉ số thực vật, tất cả các định dạng xuất ra)
* **Tính năng Chloros+** — Xử lý đa luồng, gỡ lỗi trình xác định cấu hình, tùy chỉnh chỉ mục (có giấy phép Chloros+)

## Người dùng Linux không nhận được gì

* **GUI trên máy tính để bàn** — Không có đồ họa giao diện; Tất cả các tương tác đều thông qua CLI hoặc Python SDK
* **Xem hình ảnh** — Không có trình xem hình ảnh tương thích, chế độ xem hoặc dấu bản đồ đánh dấu
* **Quản lý dự án trực quan**— Các dự án được quản lý thông qua lệnh CLI và lệnh gọi SDK***

## Bắt đầu trên Linux

1. **Cài đặt Chloros** — Xem [Cài đặt Linux](linux-installation.md) để biết cài đặt gói `.deb`
2. **Cài đặt Python SDK** (tùy chọn) — `pip install chloros-sdk`
3. **Kích hoạt giấy phép của bạn** — `chloros-cli login your@email.com 'password'`
4. **Xử lý dữ liệu đầu tiên của bạn** — `chloros-cli process ~/datasets/flight001`

Đối với người dùng NVIDIA Jetson, hãy xem [hướng dẫn NVIDIA Jetson] (nvidia-jetson-guide.md) để biết cách thiết lập và dành riêng cho nền tảng tối ưu hóa.

***

## Các bước tiếp theo

* [Cài đặt Linux](linux-installation.md) — Hướng dẫn cài đặt chi tiết cho amd64 và arm64
* [NVIDIA Jetson Guide](nvidia-jetson-guide.md) — Thiết lập, quản lý nhiệt và phát triển khai tại trường dành riêng cho Jetson
* [CLI : Dòng lệnh](../CLI.md) — Tham khảo đầy đủ về CLI
* [API: Python SDK](../api-python-sdk.md) — Tham khảo đầy đủ về SDK
* [Điều chỉnh điện toán động](../processing-architecture/dynamic-compute-adaptation.md) — Cách Chloros thích ứng với phần cứng của bạn