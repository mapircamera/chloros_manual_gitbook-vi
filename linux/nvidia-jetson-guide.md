#hướng dẫn NVIDIA Jetson

Chloros trên NVIDIA Jetson cho phép xử lý hình ảnh đa dạng ở Finfield — tại trường hiện tại, trên UAV và trong cài đặt từ xa. Chloros tự động phát hiện loại máy Jetson của bạn và bộ xử lý chiến lược tối ưu hóa cho phần cứng của bạn.

***

## Hỗ trợ Jetson mẫu

| Sample Person | RAM | Quá trình xử lý | Khuyến nghị sử dụng |
| -------------------- | -------------- | ----------------------------------------------------- | -------------------------------------------------------- |
| **Jetson AGX Orin** | 32-64GB chia sẻ | `GPU_PARALLEL` (4 công nhân) | Hiệu suất tối đa, bộ dữ liệu lớn |
| **Jetson Orin NX** | 8-16GB chia sẻ | `GPU_PARALLEL` (3 công nhân, 16GB) / `GPU_SINGLE` (8GB) | Khuyến nghị chính sách cho việc phát triển khai trên không và trên chiến trường |
| **Jetson Orin Nano** | 8GB chia sẻ | `GPU_SINGLE` (1 công nhân) | Đầu vào cấp độ điện toán |
| **Jetson Nano** | Chia sẻ 4-8GB | `GPU_SINGLE` (1 công nhân) | Đầu vào cấp độ, hạn chế về bộ nhớ |

{% hint style="info" %}
**Các mẫu Jetson kế thừa** (TX2, TX1, Xavier NX) có thể không được hỗ trợ. Hiệu suất sẽ thay đổi dựa trên GPU bộ nhớ sẵn có và CUDA khả năng.
{% endhint %}

***

## Yêu cầu

* **JetPack 6.x** (khuyến nghị mới nhất)
* **NVIDIA CUDA** (đi kèm với JetPack)
* **Giấy phép Chloros+** (bắt buộc để truy cập CLI/SDK)

## Cài đặt

```bash
# Install the JetPack 6 .deb package
sudo dpkg -i chloros-arm64-jp6.deb

# Verify installation
chloros-cli --version

# Install Python SDK (optional)
pip install chloros-sdk

# Run system diagnostics
chloros-cli selftest
```

Để biết chi tiết cài đặt chung Linux, hãy xem [Cài đặt Linux](linux-installation.md).

***

## Thích ứng tính toán trên Jetson

Chloros tự động phát hiện loại máy Jetson của bạn và chọn mức độ ưu tiên xử lý chiến lược. **Không cần điều chỉnh thủ công.**

### Hoạt động theo cách thức

Khi khởi động, hệ thống cấu hình Chloros của bạn:

1. **Phát hiện kiểu máy Jetson** qua `/proc/device-tree/model`
2. **Đọc GPU/chung bộ nhớ chung có sẵn**

3.** Chọn chiến lược xử lý** (`GPU_PARALLEL`, `GPU_SINGLE` hoặc `CPU_PARALLEL`)
4. **Tự động cài đặt số lượng nhân viên, loại quy trình và bộ nhớ bổ sung**

###Hành vi trên mỗi mô hình

| Mô Hình Jetson | Chiến lược | Công nhân | Đường ống | Đồng thời |
| ----------------------------- | -------------- | ------- | ------------------------------ | ----------- |
| **Jetson Nano 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` (tiết kiệm bộ nhớ) | Được đăng theo kỳ |
| **Jetson Orin Nano 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` | Được đăng theo kỳ |
| **Jetson Orin NX 8GB** | `GPU_SINGLE` | 2 | `tiled_gpu` | Được đăng theo kỳ |
| **Jetson Orin NX 16GB** | `GPU_PARALLEL` | 3 | `fused_gpu` (GPU đường dẫn đầy đủ) | Đồng thời |
| **Jetson AGX Orin 32-64GB** | `GPU_PARALLEL` | 4 | `fused_gpu` | Đồng thời |

{% hint style="success" %}
**Jetson Orin NX 16GB** là lý tưởng cho việc phát triển khai biên — nó đã được chiến lược `GPU_PARALLEL` với 3 nhân viên đồng thời, cung cấp khả năng xử lý bài hát GPU thực sự trong một phong cách nhỏ gọn.
{% endhint %}

Điểm khác biệt chính giữa các nền tảng là **bộ nhớ**. Jetson Nano với bộ nhớ 8GB dùng chung phải xử lý từng hình ảnh bằng cách sử dụng phương pháp phân loại tiết kiệm bộ nhớ, trong khi Orin NX với 16GB có thể chạy đồng thời 3 hình ảnh qua GPU bằng cách sử dụng đường dẫn hợp lý nhất có dung lượng cao hơn.

Để biết tài liệu tham khảo ứng dụng điện toán hoàn chỉnh hãy xem [Thích ứng điện toán động](../processing-architecture/dynamic-compute-adaptation.md).

***

## Quản lý nhiệt độ

Các thiết bị Jetson có khoảng trống nhiệt độ chế độ, đặc biệt là khi phát triển trong không gian kín hoặc trên không. Bao Chloros bao gồm giám sát nhiệt độ và điều chỉnh nhiệt độ tự động:

| Nhiệt độ | Hành động |
| ------------------- | ------------------------------------------------- |
| *****70°C** | Hoạt động bình thường - tối đa tốc độ xử lý |
| **70°C** (Cảnh báo) | Tự động giảm kích thước lô |
| **80°C** (Nghiêm trọng) | Điều chỉnh mạnh mẽ — đồng thời thấp hơn |
| **90°C** (Tắt máy) | Dừng hoàn toàn quá trình xử lý GPU - cần hạ nhiệt |

{% hint style="warning" %}
**Đảm bảo thông gió và sưởi nhiệt đầy đủ** để xử lý bền vững, đặc biệt là trong các không gian kín hoặc hệ thống trên không. Điều chỉnh nhiệt độ sẽ giảm bớt quy trình xử lý thông tin để bảo vệ phần cứng.
{% endhint %}

***

## Quản lý bộ nhớ

Thiết bị Jetson sử dụng **bộ nhớ hợp lý nhất** — GPU và CPU dùng chung vật lý RAM. Điều này có nghĩa là VRAM được báo cáo (ví dụ: 15,3 GB trên Orin NX 16GB) không phải là chuyên dụng GPU bộ nhớ; it được chia sẻ với hệ điều hành và các tiến trình khác.

###Mã xuất mã

Đối với cấu hình đã biết của cấu hình gỡ lỗi hoặc xử lý lỗi lớn của dữ liệu, Chloros có thể khuyên bạn nên tạo không có thay đổi lớn:

```bash
# Check current memory and swap
free -h

# Create a swap file (example: 8GB)
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make persistent across reboots
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

**Ưu đãi bộ nhớ trên mỗi hình ảnh:**

* Chuẩn lỗi trình gỡ lỗi: \~10 MB per image
* Gỡ lỗi khi xác định cấu hình: \~15 MB per image

Chloros bộ nhớ tính toán tự động cần thiết dựa trên dữ liệu kích thước của bạn và cảnh báo bạn nếu cần trao đổi.

Project ### OOM (Hết bộ nhớ)

Nếu phát hiện trạng thái hết bộ nhớ trong quá trình xử lý:

1. Chloros tự động giảm số nhân GPU
2. Chuyển ngược từ đường dẫn `fused_gpu` sang `tiled_gpu` (tiết kiệm bộ nhớ hơn)
3. Tiếp tục xử lý việc giảm số lượng vì sự cố

***

## Triển khai trường hiện tại

### Cân nhắc về quyền lực

| Mô Hình Jetson | Rút hình điển điện | Ghi chú |
| ---------------- | ------------------ | -------------- |
| Jetson Nano | 5-10W | Jack cắm USB-C hoặc thùng |
| Jetson Orin Nano | 7-15W | Jack thùng DC |
| Jetson Orin NX | 10-25W | Jack thùng DC |
| Jetson AGX Orin | 15-60W | Jack cắm USB-C PD hoặc thùng |

Bạn lập kế hoạch ngân sách để xử lý sự ổn định — khả năng thụ động năng lượng cao nhất xảy ra trong Chuỗi 3 (Bộ xử lý) sử dụng nhiều GPU.

### Xuất bản lưu trữ

* **SSD NVMe** được khuyến khích sử dụng cho việc phát triển khai arm64
* Thẻ SD quá chậm để xử lý - chỉ sử dụng phương tiện khởi động tiện ích
* Bạn có thể lập kế hoạch gấp 2-3 lần raw image data size để xử lý đầu ra

### Hoạt động không có thông tin đầu qua SSH

Chloros CLI lý tưởng cho việc phát triển Jetson không đầu:

```bash
# SSH into the Jetson
ssh user@jetson-hostname

# Process a dataset
chloros-cli process /data/datasets/flight001 --format tiff-32

# Monitor export progress
chloros-cli export-status
```

### Xử lý tự động với systemd

Tạo một systemd dịch vụ để xử lý tự động:

```ini
# /etc/systemd/system/chloros-process.service
[Unit]
Description=Chloros Automated Processing
After=network.target

[Service]
Type=oneshot
User=chloros
ExecStart=/usr/bin/chloros-cli process /data/incoming --output /data/processed
StandardOutput=append:/var/log/chloros-process.log
StandardError=append:/var/log/chloros-process.log

[Install]
WantedBy=multi-user.target
```

Kết nối với bộ hẹn giờ systemd để xử lý lịch trình:

```ini
# /etc/systemd/system/chloros-process.timer
[Unit]
Description=Run Chloros Processing Every Hour

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
sudo systemctl enable chloros-process.timer
sudo systemctl start chloros-process.timer
```

***

## Ví dụ về quy trình làm việc

### Xử lý cơ bản Jetson

```bash
#!/bin/bash
# Process a drone flight dataset on Jetson
chloros-cli process /data/flights/flight_042 \
    --output /data/processed/flight_042 \
    --format tiff-32 \
    --indices NDVI NDRE GNDVI
```

### Python SDK trên Jetson

```python
from chloros_sdk import ChlorosLocal

with ChlorosLocal() as chloros:
    chloros.create_project("field_survey_042")
    chloros.import_images("/data/flights/flight_042")
    chloros.configure(
        indices=["NDVI", "NDRE", "GNDVI"],
        export_format="TIFF (32-bit, Percent)",
        reflectance_calibration=True
    )
    chloros.process(mode="parallel")

print("Processing complete!")
```

### Xử lý hàng loạt nhiều chuyến bay

```bash
#!/bin/bash
# Process all flight datasets in a directory
for flight in /data/flights/*/; do
    name=$(basename "$flight")
    echo "Processing $name..."
    chloros-cli process "$flight" \
        --output "/data/processed/$name" \
        --format tiff-32 \
        --indices NDVI NDRE
    echo "Completed $name"
done
```

***

## Jetson System được xuất đề để sử dụng tại trường hiện tại

Để phát triển hiện trường và trên không, hãy xem xét các bảng tùy chọn vận hành Jetson Orin NX 16GB sau:

* **Airborne/drone**: Hệ thống có bậc thang (MIL-STD), nhẹ nhàng (dưới 300g), làm mát thụ động
* **Trường chắc chắn**: Vỏ chống nước IP67/IP69K có kết nối camera PoE GigE
* **Tối thiểu/ngân sách**: Bộ công cụ dành cho nhà phát triển có vỏ bổ sung

Liên hệ [Hỗ trợ MAPIR](https://www.mapir.camera/community/contact) để biết các chủ đề xuất bản phần cứng cụ thể cho bản khai triển kịch bản của bạn.

***

## Các bước tiếp theo

* [Cài đặt Linux](linux-installation.md) — Chi tiết cài đặt chung Linux
* [Điều chỉnh điện toán động](../processing-architecture/dynamic-compute-adaptation.md) — Tham khảo chiến lược điện toán đầy đủ
* [Processing Pipeline](../processing-architecture/processing-pipeline.md) — Tìm hiểu về đường ống 4 luồng
* [CLI : Dòng lệnh](../CLI.md) — Tham khảo đầy đủ về CLI
* [API: Python SDK](../api-python-sdk.md) — Tham khảo đầy đủ về SDK