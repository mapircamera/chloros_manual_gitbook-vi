# Cài đặt Linux

Chloros được phân phối cho Linux dưới dạng gói `.deb` cài đặt CLI và chương trình phụ trợ. Python SDK được cài đặt qua pip tùy chỉnh.

***

## Linux AMD64 (x86_64)

### Hệ thống yêu cầu

| Yêu cầu | Tối thiểu | Được xuất đề |
| --- | --- | --- |
| **Phân phối** | Ubuntu 20.04+ / Debian 11+ | Ubuntu 22.04+ |
| **Bộ xử lý** | x86_64 (Intel/AMD) | Intel Core i7 trở lên |
| **Bộ nhớ (RAM)** | 8GB | 16GB trở lên |
| **Đồ họa thẻ** | No (Xử lý CPU) | GPU NVIDIA với 4GB+ VRAM |
| **Lưu trữ** | Dung lượng trống 2GB | SSD có dung lượng trống hơn 10GB |
| **Python** | Python 3.7+ (dành cho SDK) | Python 3.10+ |

### Cài đặt

Tải xuống gói `.deb` và cài đặt:

```bash
sudo dpkg -i chloros-amd64.deb
```

Xác định cài đặt:

```bash
chloros-cli --version
```

***

## Linux arm64 (NVIDIA Jetson)

### Hệ thống yêu cầu

| Yêu cầu | Tối thiểu | Được xuất đề |
| --- | --- | --- |
| **Nền tảng** | NVIDIA Jetson với JetPack 6 | Jetson Orin NX 16GB hoặc AGX Orin |
| **JetPack** | JetPack 6.x | JetPack 6 mới nhất |
| **Bộ nhớ (RAM)** | 8GB (GPU/CPU dùng chung) | 16GB+ được chia sẻ |
| **Lưu trữ** | Dung lượng trống 2GB | SSD NVMe với 10GB+ miễn phí |
| **Python** | Python 3.7+ (dành cho SDK) | Python 3.10+ |

### Cài đặt

Tải xuống gói JetPack 6 `.deb` và cài đặt:

```bash
sudo dpkg -i chloros-arm64-jp6.deb
```

Xác định cài đặt:

```bash
chloros-cli --version
```

Để biết cách thiết lập Jetson chi tiết bao gồm quản lý nhiệt và phát triển tại hiện trường, hãy xem [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md).

***

## Cài đặt Python SDK (Tất cả Linux)

Python SDK được cài đặt qua pip và hoạt động riêng trên cả amd64 và arm64:

```bash
pip install chloros-sdk
```

Để bao gồm các tùy chọn tiến trình trực tuyến được hỗ trợ hỗ trợ:

```bash
pip install chloros-sdk[progress]
```

Xác minh SDK:

```bash
python -c "import chloros_sdk; print(chloros_sdk.__version__)"
```

{% hint style="info" %}
Gói `.deb` cài đặt Chloros CLI và chương trình hỗ trợ. Python SDK là gói pip riêng biệt tiếp theo với chương trình phụ trợ thông tin qua bộ cục bộ HTTP API.
{% endhint %}

***

## Cấu hình thư mục

Chloros trên Linux Cùng với [đặc tả cơ sở dữ liệu XDG](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html):

| Mục tiêu | Đường dẫn Linux | Windows tương đương |
| --- | --- | --- |
| **Cấu hình** | `~/.config/chloros/` | `%APPDATA%\Chloros\` |
| **Dữ liệu / Dự án** | `~/.local/share/chloros/` | `%LOCALAPPDATA%\Chloros\` |
| **Bộ nhớ đệm / Thông tin xác thực** | `~/.cache/chloros/` | `%APPDATA%\Chloros\cache\` |

## Vị trí có thể thực hiện ở phần cuối

Gói `.deb` cài đặt phần phụ được hỗ trợ vào tiêu chuẩn vị trí. Hỗ trợ đường dẫn tự động phát hiện CLI và SDK:

| Cài đặt phương pháp | Hỗ trợ đường dẫn |
| --- | --- |
| Gói `.deb` | `/usr/lib/chloros/chloros-backend` |
| Hướng dẫn sử dụng / tùy chỉnh | `/opt/mapir/chloros/backend/chloros-backend` |

Bạn có thể ghi đè hỗ trợ đường dẫn bằng `--backend-exe` CLI hoặc tham số tạo `backend_exe` SDK.

***

## Cài đặt lần đầu

### 1. Kích hoạt giấy phép của bạn

Cần có giấy phép Chloros+ để truy cập CLI và SDK:

```bash
chloros-cli login your@email.com 'your-password'
```

### 2. Kiểm tra giấy phép trạng thái của bạn

```bash
chloros-cli status
```

### 3. Xử lý dữ liệu đầu tiên của bạn

```bash
chloros-cli process ~/datasets/flight001
```

### 4. Chạy Dự đoán Hệ thống

Xác minh rằng hệ thống của bạn có cấu hình đúng:

```bash
chloros-cli selftest
```

Quá trình này chạy 7 bước kiểm tra dự đoán bao gồm phiên bản, phụ trợ khởi động, kết nối API và khả năng sử dụng của CUDA/GPU.

***

## Ví dụ về Bash command

### Xử lý nhiều dữ liệu

```bash
#!/bin/bash
for dataset in ~/datasets/2026/*/; do
    echo "Processing $(basename "$dataset")..."
    chloros-cli process "$dataset" --format tiff-32
    echo "Done: $(basename "$dataset")"
done
```

### Quy trình cài đặt tùy chỉnh

```bash
#!/bin/bash
chloros-cli process ~/datasets/field_a \
    --output ~/output/field_a \
    --format tiff-32 \
    --indices NDVI NDRE GNDVI \
    --debayer texture-aware \
    --no-vignette
```

### Xử lý tự động bằng Cron

Thêm vào crontab của bạn (`crontab -e`) để tự động xử lý dữ liệu mới:

```cron
# Process any new datasets at 2 AM daily
0 2 * ** /usr/bin/chloros-cli process /data/incoming --output /data/processed >> /var/log/chloros.log 2>&1
```

### Ví dụ Python SDK

```python
from chloros_sdk import process_folder

# One-line processing
result = process_folder(
    "/home/user/datasets/flight001",
    indices=["NDVI", "NDRE"],
    export_format="TIFF (32-bit, Percent)"
)
```

***

##Khắc phục sự cố

### Không thể tìm thấy CLI sau khi cài đặt

If `chloros-cli` không được tìm thấy sau khi cài đặt gói `.deb`:

```bash
# Check if the binary exists
which chloros-cli
ls -la /usr/bin/chloros-cli

# If not in PATH, check the installation
dpkg -L chloros-amd64  # or chloros-arm64-jp6

# Reload your shell
source ~/.bashrc
```

### Quyền bị từ chối

```bash
# Ensure the binary is executable
sudo chmod +x /usr/bin/chloros-cli
sudo chmod +x /usr/lib/chloros/chloros-backend
```

### Phần hỗ trợ không khởi động được

```bash
# Check if port 5000 is already in use
lsof -i :5000

# Kill any existing process on port 5000
kill $(lsof -t -i :5000)

# Try starting with a different port
chloros-cli --port 5001 process ~/datasets/flight001
```

### CUDA không được phát hiện

```bash
# Check NVIDIA driver installation
nvidia-smi

# Check CUDA availability
nvcc --version

# On Jetson, check JetPack version
cat /etc/nv_tegra_release
```

### Missing chung thư viện

```bash
# Install common dependencies
sudo apt-get update
sudo apt-get install -f

# Check for missing libraries
ldd /usr/lib/chloros/chloros-backend | grep "not found"
```

***

## Cập nhật Chloros trên Linux

Sử dụng hợp lý cập nhật lệnh để kiểm tra và cài đặt các bản cập nhật cập nhật:

```bash
# Check for updates without installing
chloros-cli update --check

# Check for and install updates
chloros-cli update
```

***

## Các bước tiếp theo

* [ Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) — Tối ưu hóa và phát triển dành riêng cho Jetson
* [CLI : Dòng lệnh](../CLI.md) - Tham chiếu lệnh CLI đầy đủ
* [API: Python SDK](../api-python-sdk.md) — Tham khảo đầy đủ về SDK
* [Điều chỉnh điện toán động](../processing-architecture/dynamic-compute-adaptation.md) — Cách Chloros thích ứng với phần cứng của bạn