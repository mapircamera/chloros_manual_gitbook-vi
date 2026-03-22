#CLI : Dòng lệnh

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>

**Chloros CLI** cung cấp khả năng truy cập dòng lệnh mạnh mẽ vào công cụ xử lý hình ảnh Chloros, cho phép tự động hóa, viết kịch bản và vận hành không đầu cho quy trình xử lý hình ảnh của bạn.

### Các tính năng chính

* 🚀 **Tự động hóa** - Xử lý hàng loạt tập lệnh của nhiều bộ dữ liệu
* 🔗 **Tích hợp** - Nhúng vào quy trình công việc và quy trình hiện có
* 💻 **Thao tác không đầu** - Chạy không cần GUI
* 🌍 **Đa ngôn ngữ** - Hỗ trợ 38 ngôn ngữ
* ⚡ **Xử lý song song** - [Thích ứng điện toán động](processing-architecture/dynamic-compute-adaptation.md) tự động tối ưu hóa cho phần cứng của bạn

### Yêu cầu

| Yêu cầu | Chi tiết |
| -------------------- | ------------------------------------------------------------------- |
| **Hệ điều hành** | Windows 10/11 (64-bit), Linux x86_64 (AMD64), Linux arm64 (NVIDIA Jetson JetPack 6) |
| **Giấy phép** | Chloros+ ([yêu cầu gói trả phí](https://cloud.mapir.camera/pricing)) |
| **Bộ nhớ** | RAM tối thiểu 8GB (khuyến nghị 16GB) |
| **Internet** | Cần thiết để kích hoạt giấy phép |
| **Dung lượng đĩa** | Thay đổi theo quy mô dự án |

{% hint style="warning" %}
**Yêu cầu về giấy phép**: CLI yêu cầu đăng ký Chloros+ trả phí. Các gói tiêu chuẩn (miễn phí) không có quyền truy cập CLI. Truy cập [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing) để nâng cấp.
{% endhint %}

## Bắt đầu nhanh

### Cài đặt

#### Windows

CLI được tự động đi kèm với trình cài đặt Chloros:

1. Tải xuống và chạy **Trình cài đặt Chloros.exe**

2. Hoàn tất trình hướng dẫn cài đặt
3. CLI được cài đặt vào: `C:\Program Files\Chloros\resources\cli\chloros-cli.exe`

{% hint style="success" %}
Trình cài đặt tự động thêm `chloros-cli` vào PATH hệ thống của bạn. Khởi động lại thiết bị đầu cuối của bạn sau khi cài đặt.
{% endhint %}

#### Linux

Cài đặt gói `.deb` cho kiến ​​trúc của bạn:

```bash
# Linux amd64
sudo dpkg -i chloros-amd64.deb

# Linux arm64 (NVIDIA Jetson, JetPack 6)
sudo dpkg -i chloros-arm64-jp6.deb
```

Để biết cách thiết lập Linux chi tiết, hãy xem [Cài đặt Linux](linux/linux-installation.md).

### Thiết lập lần đầu

Trước khi sử dụng CLI, hãy kích hoạt giấy phép Chloros+ của bạn:

**Windows:**

```powershell
# Login with your Chloros+ account
chloros-cli login user@example.com 'your_password'

# Check license status
chloros-cli status

# Process your first project
chloros-cli process "C:\Images\Dataset001"
```

**Linux:**

```bash
# Login with your Chloros+ account
chloros-cli login user@example.com 'your_password'

# Check license status
chloros-cli status

# Process your first project
chloros-cli process ~/images/dataset001
```

### Cách sử dụng cơ bản

Xử lý thư mục có cài đặt mặc định:

**Windows:**

```powershell
chloros-cli process "C:\Images\Dataset001"
```

**Linux:**

```bash
chloros-cli process ~/images/dataset001
```

***

## Tham chiếu lệnh

### Cú pháp chung

```
chloros-cli [global-options] <command> [command-options]
```

***

## Lệnh

### `process` - Xử lý hình ảnh

Xử lý hình ảnh trong một thư mục có hiệu chuẩn.

**Cú pháp:**

```bash
chloros-cli process <input-folder> [options]
```

**Ví dụ:**

```bash
# Windows
chloros-cli process "C:\Datasets\Survey_001" --vignette --reflectance

# Linux
chloros-cli process ~/datasets/survey_001 --vignette --reflectance
```

#### Tùy chọn lệnh xử lý

| Tùy chọn | Loại | Mặc định | Mô tả |
| --------------------- | ------- | -------------- | -------------------------------------------------------------------------------------- |
| `<input-folder>` | Đường dẫn | _Bắt buộc_ | Thư mục chứa ảnh đa phổ RAW/JPG |
| `-o, --output` | Đường dẫn | Tương tự như đầu vào | Thư mục đầu ra cho hình ảnh được xử lý |
| `-n, --project-name` | Chuỗi | Được tạo tự động | Tên dự án tùy chỉnh |
| `--vignette` | Cờ | Đã bật | Bật tính năng chỉnh sửa họa tiết |
| `--no-vignette` | Cờ | - | Tắt tính năng chỉnh sửa họa tiết |
| `--reflectance` | Cờ | Đã bật | Kích hoạt hiệu chuẩn phản xạ |
| `--no-reflectance` | Cờ | - | Vô hiệu hóa hiệu chuẩn phản xạ |
| `--ppk` | Cờ | Vô hiệu hóa | Áp dụng hiệu chỉnh PPK từ dữ liệu cảm biến ánh sáng .daq |
| `--format` | Lựa chọn | TIFF (16-bit) | Định dạng đầu ra: `TIFF (16-bit)`, `TIFF (32-bit, Percent)`, `PNG (8-bit)`, `JPG (8-bit)` |
| `--min-target-size` | Số nguyên | Tự động | Kích thước mục tiêu tối thiểu tính bằng pixel để phát hiện bảng hiệu chuẩn |
| `--target-clustering` | Số nguyên | Tự động | Ngưỡng phân cụm mục tiêu (0-100) |
| `--debayer` | Lựa chọn | `standard` | Phương pháp gỡ lỗi: `standard` hoặc `texture-aware` (chỉ Chloros+) |
| `--target`, `--targets` | Cờ | Vô hiệu hóa | Chỉ tìm kiếm các mục tiêu hiệu chuẩn trong thư mục con "target" hoặc "target" (tăng tốc độ xử lý) |
| `--indices` | Danh sách | Không có | Chỉ số thực vật cần tính toán (ví dụ: `--indices NDVI NDRE GNDVI`) |
| `--exposure-pin-1` | Chuỗi | Không có | Khóa phơi sáng cho kiểu máy ảnh (Chân 1) |
| `--exposure-pin-2` | Chuỗi | Không có | Khóa phơi sáng cho kiểu máy ảnh (Chân 2) |
| `--recal-interval` | Số nguyên | Tự động | Khoảng thời gian hiệu chuẩn lại tính bằng giây |
| `--timezone-offset` | Số nguyên | 0 | Bù trừ múi giờ theo giờ |

***

### `login` - Xác thực tài khoản

Đăng nhập bằng thông tin đăng nhập Chloros+ của bạn để kích hoạt xử lý CLI.

**Cú pháp:**

```bash
chloros-cli login <email> <password>
```

**Ví dụ:**

```bash
chloros-cli login user@example.com 'MyP@ssw0rd123'
```

{% hint style="warning" %}
**Ký tự đặc biệt**: Sử dụng dấu ngoặc đơn xung quanh mật khẩu chứa các ký tự như `$`, `!` hoặc dấu cách.
{% endhint %}

**Đầu ra:**<figure><img src=".gitbook/assets/cli login_w.JPG" alt=""><figcaption></figcaption></figure>***

### `logout` - Xóa thông tin xác thực

Xóa thông tin đăng nhập được lưu trữ và đăng xuất khỏi tài khoản của bạn.

**Cú pháp:**

```bash
chloros-cli logout
```

**Ví dụ:**

```bash
chloros-cli logout
```

**Đầu ra:**

```
✓ Logout successful
ℹ Credentials cleared from cache
```

{% hint style="info" %}
**Người dùng SDK**: Python SDK cũng cung cấp phương thức `logout()` có lập trình để xóa thông tin xác thực trong tập lệnh Python. Xem [tài liệu Python SDK](api-python-sdk.md#logout) để biết chi tiết.
{% endhint %}

***

### `status` - Kiểm tra trạng thái giấy phép

Hiển thị giấy phép hiện tại và trạng thái xác thực.

**Cú pháp:**

```bash
chloros-cli status
```

**Ví dụ:**

```bash
chloros-cli status
```

**Đầu ra:**

```
╔══════════════════════════════════════╗
║     LICENSE & ACCOUNT INFORMATION    ║
╚══════════════════════════════════════╝

📧 Email: user@example.com
📋 Plan: Chloros+ Professional
🔓 API/CLI Access: Enabled
✓ Status: Active
```

***

### `export-status` - Kiểm tra tiến trình xuất

Theo dõi tiến trình xuất Thread 4 trong hoặc sau khi xử lý.

**Cú pháp:**

```bash
chloros-cli export-status
```

**Ví dụ:**

```bash
chloros-cli export-status
```

**Trường hợp sử dụng:** Gọi lệnh này trong khi quá trình xử lý đang chạy để kiểm tra tiến trình xuất.***

### `language` - Quản lý ngôn ngữ giao diện

Xem hoặc thay đổi ngôn ngữ giao diện CLI.

**Cú pháp:**

```bash
# Show current language
chloros-cli language

# List all available languages
chloros-cli language --list

# Set a specific language
chloros-cli language <language-code>
```

**Ví dụ:**

```bash
# View current language
chloros-cli language

# List all 38 supported languages
chloros-cli language --list

# Change to Spanish
chloros-cli language es

# Change to Japanese
chloros-cli language ja
```

#### Ngôn ngữ được hỗ trợ (Tổng cộng 38)

| Mã | Ngôn ngữ | Tên bản địa |
| ------- | --------------------- | ---------------- |
| `en` | Tiếng Anh | Tiếng Anh |
| `es` | Tây Ban Nha | Tiếng Tây Ban Nha |
| `pt` | Tiếng Bồ Đào Nha | Bồ Đào Nha |
| `fr` | Tiếng Pháp | Tiếng Pháp |
| `de` | Tiếng Đức | tiếng Đức |
| `it` | Ý | Tiếng Ý |
| `ja` | Tiếng Nhật | 日本語 |
| `ko` | Tiếng Hàn | 한국어 |
| `zh` | Tiếng Trung (Giản thể) | 简体中文 |
| `zh-TW` | Tiếng Trung (Phồn Thể) | 繁體中文 |
| `ru` | Tiếng Nga | Русский |
| `nl` | Tiếng Hà Lan | Hà Lan |
| `ar` | Tiếng Ả Rập | العربية |
| `pl` | Tiếng Ba Lan | Tiếng Ba Lan |
| `tr` | Thổ Nhĩ Kỳ | Türkçe |
| `hi` | Tiếng Hindi | हिंदी |
| `id` | Tiếng Indonesia | Tiếng Bahasa Indonesia |
| `vi` | Tiếng Việt | Tiếng Việt |
| `th` | Tiếng Thái | ไทย |
| `sv` | Thụy Điển | Svenska |
| `da` | Tiếng Đan Mạch | Đan Mạch |
| `no` | Na Uy | Norsk |
| `fi` | Phần Lan | Suomi |
| `el` | Tiếng Hy Lạp | Ελληνικά |
| `cs` | Tiếng Séc | Čeština |
| `hu` | Tiếng Hungary | Tiếng Magyar |
| `ro` | Tiếng Rumani | Română |
| `uk` | Tiếng Ukraina | Українська |
| `pt-BR` | Tiếng Bồ Đào Nha Brazil | Bồ Đào Nha Brasileiro |
| `zh-HK` | Tiếng Quảng Đông | 粵語 |
| `ms` | Mã Lai | Tiếng Bahasa Melayu |
| `sk` | Tiếng Slovak | Tiếng Slovenia |
| `bg` | Tiếng Bungari | Български |
| `hr` | Tiếng Croatia | Hrvatski |
| `lt` | Tiếng Litva | Liệtuvių |
| `lv` | Tiếng Latvia | Latviešu |
| `et` | Tiếng Estonia | Eesti |
| `sl` | Tiếng Slovenia | Tiếng Sloveniaščina |

{% hint style="success" %}
**Tính kiên trì tự động**: Tùy chọn ngôn ngữ của bạn được lưu vào `~/.chloros/cli_language.json` và tồn tại trong tất cả các phiên.
{% endhint %}

***

### `set-project-folder` - Đặt thư mục dự án mặc định

Thay đổi vị trí thư mục dự án mặc định (được chia sẻ với GUI trên Windows).

**Cú pháp:**

```bash
chloros-cli set-project-folder <folder-path>
```

**Ví dụ:**

```bash
# Windows
chloros-cli set-project-folder "C:\Projects\2025"

# Linux
chloros-cli set-project-folder ~/projects/2025
```

***

### `get-project-folder` - Hiển thị thư mục dự án

Hiển thị vị trí thư mục dự án mặc định hiện tại.

**Cú pháp:**

```bash
chloros-cli get-project-folder
```

**Ví dụ:**

```bash
chloros-cli get-project-folder
```

**Đầu ra:**

```

# Windows
ℹ Current project folder: C:\Projects\2025

# Linux
ℹ Current project folder: /home/user/.local/share/chloros/projects
```

***

### `reset-project-folder` - Đặt lại về mặc định

Đặt lại thư mục dự án về vị trí mặc định.

**Cú pháp:**

```bash
chloros-cli reset-project-folder
```

***

### `selftest` - Chạy chẩn đoán hệ thống

Chạy 7 bước kiểm tra chẩn đoán để xác minh cấu hình hệ thống của bạn.

**Cú pháp:**

```bash
chloros-cli selftest
```

**Chẩn đoán được thực hiện:**

1. Kiểm tra phiên bản
2. Tính khả dụng của cổng (5000)
3. Khởi động phụ trợ
4. Kiểm tra kết nối API
5. Thông tin hệ thống và phát hiện GPU
6. Xác minh mô hình khử nhiễu
7. Kiểm tra tính khả dụng của CUDA

{% hint style="info" %}
**Hữu ích khi khắc phục sự cố**: Chạy `selftest` sau khi cài đặt để xác minh hệ thống của bạn được định cấu hình chính xác, đặc biệt là trên Linux/Jetson nơi thiết lập GPU và CUDA có thể cần xác minh.
{% endhint %}

***

### `update` - Kiểm tra cập nhật (Chỉ Linux)

Kiểm tra và cài đặt các bản cập nhật CLI trên hệ thống Linux.

**Cú pháp:**

```bash
# Check for updates without installing
chloros-cli update --check

# Check for and install updates
chloros-cli update
```

| Tùy chọn | Mô tả |
| --------- | ---------------------------------- |
| `--check` | Chỉ kiểm tra cập nhật, không cài đặt |

{% hint style="info" %}
Lệnh này chỉ khả dụng trên Linux. Trên Windows, các bản cập nhật được gửi qua trình cài đặt.
{% endhint %}

***

## Tùy chọn toàn cầu

Các tùy chọn này áp dụng cho tất cả các lệnh:

| Tùy chọn | Loại | Mặc định | Mô tả |
| ----------------- | ------- | ------------- | ------------------------------------------------ |
| `--backend-exe` | Đường dẫn | Tự động phát hiện | Đường dẫn đến phần thực thi phụ trợ |
| `--port` | Số nguyên | 5000 | Số cổng API phụ trợ |
| `--restart` | Cờ | - | Buộc khởi động lại chương trình phụ trợ (giết chết các tiến trình hiện có) |
| `--version` | Cờ | - | Hiển thị thông tin phiên bản và thoát |
| `--help` | Cờ | - | Hiển thị thông tin trợ giúp và thoát |

{% hint style="info" %}
**Tự động phát hiện phần phụ trợ**: Đường dẫn `--backend-exe` được tự động phát hiện trên mỗi nền tảng:
* **Windows**: `C:\Program Files\MAPIR\Chloros\resources\backend\chloros-backend.exe`
* **Linux (.deb)**: `/usr/lib/chloros/chloros-backend`
* **Linux (thủ công)**: `/opt/mapir/chloros/backend/chloros-backend`
{% endhint %}

**Ví dụ với Tùy chọn chung:**

**Windows:**

```powershell
chloros-cli --port 5001 process "C:\Datasets\Survey_001"
```

**Linux:**

```bash
chloros-cli --port 5001 process ~/datasets/survey_001
```

***

## Hướng dẫn cài đặt xử lý

### Xử lý song song & Thích ứng điện toán động

Chloros 1.1.0 bao gồm [Thích ứng điện toán động](processing-architecture/dynamic-compute-adaptation.md) — công cụ xử lý **tự động phát hiện phần cứng của bạn** và chọn chiến lược tối ưu:

| Nền tảng | Chiến lược | Công nhân | Đường ống | Ghi chú |
| --- | --- | --- | --- | --- |
| **Jetson Nano 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` | Tiết kiệm bộ nhớ, được tuần tự hóa |
| **Jetson Orin NX 16GB** | `GPU_PARALLEL` | 3 | `fused_gpu` | Xử lý GPU đồng thời |
| **Máy tính để bàn có GPU 8GB** | `GPU_SINGLE` | 3 | `tiled_gpu` | Hiệu suất máy tính để bàn tốt |
| **Máy tính để bàn có GPU 12GB+** | `GPU_PARALLEL` | 3-4 | `fused_gpu` | Hiệu suất máy tính để bàn tối ưu |
| **Hệ thống chỉ dùng CPU** | `CPU_PARALLEL` | lõi - 1 | `cpu_fallback` | Không cần GPU |

{% hint style="success" %}
**Không cần cấu hình thủ công!** Chloros tự động phát hiện cảm biến nhiệt CPU, GPU, RAM và (trên Jetson) của bạn, sau đó tự động định cấu hình quy trình xử lý tối ưu.
{% endhint %}

### Phương pháp Debayer

| Phương pháp | Cờ CLI | Chất lượng | Tốc độ | Giấy phép |
| --- | --- | --- | --- | --- |
| **Tiêu chuẩn (Nhanh, Chất lượng trung bình)** | `--debayer standard` | Tốt | Nhanh | Miễn phí / Chloros+ |
| **Nhận biết kết cấu (Chậm, Chất lượng cao nhất)** | `--debayer texture-aware` | Cao nhất | Chậm | Chỉ Chloros+ |

Phương thức gỡ lỗi mặc định là **Tiêu chuẩn**. Phương pháp**Nhận biết kết cấu** sử dụng mô hình khử nhiễu AI/ML để có chất lượng đầu ra cao nhất nhưng yêu cầu giấy phép Chloros+ và GPU NVIDIA.

```bash
# Use Texture Aware debayer (Chloros+ only)
chloros-cli process ~/datasets/field_a --debayer texture-aware
```

### Chỉnh sửa họa tiết

**Công dụng của nó:** Khắc phục tình trạng mất ánh sáng ở các cạnh hình ảnh (các góc tối hơn thường gặp trong hình ảnh bằng máy ảnh).

* **Bật theo mặc định** - Hầu hết người dùng nên bật tính năng này
* Sử dụng `--no-vignette` để tắt

{% hint style="success" %}
**Khuyến nghị**: Luôn bật tính năng chỉnh sửa họa tiết để đảm bảo độ sáng đồng đều trên toàn khung hình.
{% endhint %}

### Hiệu chỉnh độ phản xạ

Chuyển đổi giá trị cảm biến thô thành tỷ lệ phần trăm phản xạ được tiêu chuẩn hóa bằng bảng hiệu chuẩn.

* **Bật theo mặc định** - Cần thiết cho phân tích thực vật
* Yêu cầu bảng mục tiêu hiệu chuẩn trong hình ảnh
* Sử dụng `--no-reflectance` để tắt

{% hint style="info" %}
**Yêu cầu**: Đảm bảo bảng hiệu chuẩn được hiển thị đúng cách và hiển thị trong hình ảnh của bạn để chuyển đổi độ phản xạ chính xác.
{% endhint %}

### Chỉnh sửa PPK

**Công dụng:** Áp dụng các hiệu chỉnh Động học sau xử lý bằng cách sử dụng dữ liệu nhật ký DAQ-A-SD để cải thiện độ chính xác của GPS.

* **Bị tắt theo mặc định**
* Sử dụng `--ppk` để kích hoạt
* Yêu cầu tệp .daq trong thư mục dự án từ cảm biến ánh sáng MAPIR DAQ-A-SD.

### Định dạng đầu ra

<table><thead><tr><th width="197">Định dạng</th><th width="130.20001220703125">Độ sâu bit</th><th width="116.5999755859375">Kích thước tệp</th><th>Tốt nhất cho</th></tr></thead><tbody><tr><td><strong>TIFF (16-bit)</strong> ⭐</td><td>Số nguyên 16 bit</td><td>Lớn</td><td>Phân tích GIS, phép đo ảnh (được khuyến nghị)</td></tr><tr><td><strong>TIFF (32-bit, Phần trăm)</strong></td><td>Phôi 32 bit</td><td>Rất Lớn</td><td>Phân tích, nghiên cứu khoa học</td></tr><tr><td><strong>PNG (8-bit)</strong></td><td>Số nguyên 8 bit</td><td>Trung bình</td><td>Kiểm tra trực quan, chia sẻ web</td></tr><tr><td><strong>JPG (8-bit)</strong></td><td>8-bit số nguyên</td><td>Nhỏ</td><td>Xem trước nhanh, đầu ra được nén</td></tr></tbody></table>

***

## Tự động hóa & Viết kịch bản

### Xử lý hàng loạt PowerShell (Windows)

Tự động xử lý nhiều thư mục tập dữ liệu trên Windows:

```powershell
# process_all_datasets.ps1

$datasets = Get-ChildItem "C:\Datasets\2025" -Directory

foreach ($dataset in $datasets) {
    Write-Host "Processing $($dataset.Name)..." -ForegroundColor Cyan
    
    chloros-cli process $dataset.FullName `
        --vignette `
        --reflectance
    
    if ($LASTEXITCODE -eq 0) {
        Write-Host "✓ $($dataset.Name) complete" -ForegroundColor Green
    } else {
        Write-Host "✗ $($dataset.Name) failed" -ForegroundColor Red
    }
}

Write-Host "All datasets processed!" -ForegroundColor Green
```

### Tập lệnh hàng loạt Windows (Windows)

Vòng lặp đơn giản để xử lý hàng loạt trên Windows:

```batch
@echo off
echo Starting batch processing...

for /d %%i in (C:\Datasets\2025\*) do (
    echo.
    echo ========================================
    echo Processing: %%i
    echo ========================================
    chloros-cli process "%%i"
    
    if %ERRORLEVEL% EQU 0 (
        echo SUCCESS: %%i processed
    ) else (
        echo ERROR: %%i failed
    )
)

echo.
echo All datasets processed!
pause
```

### Xử lý hàng loạt Bash (Linux)

Xử lý nhiều thư mục tập dữ liệu trên Linux:

```bash
#!/bin/bash
# process_all_datasets.sh

for dataset in ~/datasets/2026/*/; do
    name=$(basename "$dataset")
    echo "Processing $name..."

    chloros-cli process "$dataset" \
        --vignette \
        --reflectance

    if [ $? -eq 0 ]; then
        echo "✓ $name complete"
    else
        echo "✗ $name failed"
    fi
done

echo "All datasets processed!"
```

### Tập lệnh tự động hóa Python (Đa nền tảng)

Tự động hóa nâng cao với xử lý lỗi (hoạt động trên Windows và Linux):

```python
import subprocess
import os
import sys
from pathlib import Path
from datetime import datetime

def process_dataset(input_folder):
    """Process a folder using Chloros CLI"""
    cmd = ['chloros-cli', 'process', str(input_folder)]
    
    # Execute command
    result = subprocess.run(
        cmd, 
        capture_output=True, 
        text=True,
        encoding='utf-8'
    )
    
    return result.returncode == 0, result.stdout, result.stderr

def main():
    """Process all datasets in a directory"""
    # Adjust path for your platform
    # Windows: Path('C:/Datasets/2025')
    # Linux:   Path.home() / 'datasets' / '2025'
    datasets_dir = Path('C:/Datasets/2025')
    log_file = Path('processing_log.txt')
    
    successful = []
    failed = []
    
    # Start processing
    print(f"Starting batch processing: {datetime.now()}")
    print(f"Scanning: {datasets_dir}")
    print("=" * 60)
    
    for dataset_folder in sorted(datasets_dir.iterdir()):
        if not dataset_folder.is_dir():
            continue
        
        print(f"\nProcessing: {dataset_folder.name}")
        
        success, stdout, stderr = process_dataset(dataset_folder)
        
        if success:
            print(f"✓ {dataset_folder.name} - SUCCESS")
            successful.append(dataset_folder.name)
        else:
            print(f"✗ {dataset_folder.name} - FAILED")
            failed.append(dataset_folder.name)
            
            # Log error details
            with open(log_file, 'a', encoding='utf-8') as f:
                f.write(f"\n=== {dataset_folder.name} - {datetime.now()} ===\n")
                f.write(f"STDOUT:\n{stdout}\n")
                f.write(f"STDERR:\n{stderr}\n")
    
    # Print summary
    print("\n" + "=" * 60)
    print(f"SUMMARY - Completed: {datetime.now()}")
    print(f"  Successful: {len(successful)}")
    print(f"  Failed: {len(failed)}")
    
    if failed:
        print(f"\nFailed folders:")
        for folder in failed:
            print(f"  - {folder}")
        print(f"\nCheck {log_file} for error details")
        sys.exit(1)
    else:
        print("\nAll datasets processed successfully!")
        sys.exit(0)

if __name__ == '__main__':
    main()
```

***

## Quy trình xử lý

### Quy trình làm việc tiêu chuẩn

1. **Đầu vào**: Thư mục chứa các cặp ảnh RAW/JPG
2. **Khám phá**: CLI tự động quét các tệp hình ảnh được hỗ trợ
3. **Đang xử lý**: Chế độ song song mở rộng theo lõi CPU của bạn (Chloros+)
4. **Đầu ra**: Tạo các thư mục con kiểu máy ảnh chứa các hình ảnh đã được xử lý

### Ví dụ về cấu trúc đầu ra

```

MyProject/
├── project.json                             # Project metadata
├── 2025_0203_193056_008.JPG                # Original JPG
├── 2025_0203_193055_007.RAW                # Original RAW
└── Survey3N_RGN/                           # Processed outputs ✓
    ├── 2025_0203_193056_008_Reflectance.tif   # Calibrated reflectance
    ├── 2025_0203_193056_008_Target.tif        # Target detection
    └── ...
```

### Ước tính thời gian xử lý

Thời gian xử lý thông thường cho 100 hình ảnh (mỗi hình 12MP):

| Nền tảng | Chế độ | Thời gian dự kiến ​​| Ghi chú |
| --- | --- | --- | --- |
| **GPU 12GB+ cho máy tính để bàn** | `GPU_PARALLEL` | 5-10 phút | Tùy chọn nhanh nhất |
| **GPU 8GB dành cho máy tính để bàn** | `GPU_SINGLE` | 10-15 phút | Hiệu suất tốt |
| **Jetson Orin NX 16GB** | `GPU_PARALLEL` | 15-25 phút | Tính toán cạnh |
| **Jetson Nano 8GB** | `GPU_SINGLE` | 30-60 phút | Hạn chế về bộ nhớ |
| **Chỉ dành cho CPU** | `CPU_PARALLEL` | 20-40 phút | Không cần GPU |

{% hint style="info" %}
**Mẹo về hiệu suất**: Thời gian xử lý thay đổi tùy theo số lượng hình ảnh, độ phân giải, phương pháp gỡ lỗi và phần cứng. Trình gỡ lỗi nhận biết kết cấu mất nhiều thời gian hơn đáng kể so với Tiêu chuẩn. Xem [Điều chỉnh điện toán động](processing-architecture/dynamic-compute-adaptation.md) để biết chi tiết.
{% endhint %}

***

## Khắc phục sự cố

### Không tìm thấy CLI

**Lỗi Windows:**

```
'chloros-cli' is not recognized as an internal or external command
```

**Giải pháp Windows:**

1. Xác minh vị trí cài đặt:

```powershell
dir "C:\Program Files\Chloros\resources\cli\chloros-cli.exe"
```

2. Sử dụng đường dẫn đầy đủ nếu không có trong PATH:

```powershell
"C:\Program Files\Chloros\resources\cli\chloros-cli.exe" process "C:\Datasets\Field_A"
```

3. Thêm vào PATH theo cách thủ công:
   * Thuộc tính hệ thống mở → Biến môi trường
   * Chỉnh sửa biến PATH
   * Thêm: `C:\Program Files\Chloros\resources\cli`
   * Khởi động lại thiết bị đầu cuối

**Lỗi Linux:**

```
chloros-cli: command not found
```

**Giải pháp Linux:**

1. Xác minh cài đặt:

```bash
which chloros-cli
dpkg -L chloros-amd64  # or chloros-arm64-jp6
```

2. Tải lại shell của bạn:

```bash
source ~/.bashrc
```

3. Kiểm tra quyền:

```bash
sudo chmod +x /usr/bin/chloros-cli
```

***

### Phần phụ trợ không khởi động được**Lỗi:**

```

Backend failed to start within 30 seconds
```

**Giải pháp:**

1. Kiểm tra xem chương trình phụ trợ đã chạy chưa (đóng nó trước)
2. Kiểm tra tường lửa không chặn (Windows) hoặc kiểm tra tính khả dụng của cổng (Linux: `lsof -i :5000`)
3. Thử cổng khác:

```bash
# Windows
chloros-cli --port 5001 process "C:\Datasets\Field_A"

# Linux
chloros-cli --port 5001 process ~/datasets/field_a
```

4. Buộc khởi động lại phần phụ trợ:

```bash
# Windows
chloros-cli --restart process "C:\Datasets\Field_A"

# Linux
chloros-cli --restart process ~/datasets/field_a
```

5. Trên Linux, kiểm tra xem tệp thực thi phụ trợ có tồn tại không:

```bash
ls -la /usr/lib/chloros/chloros-backend
```

***

### Vấn đề về giấy phép / xác thực**Lỗi:**

```

Chloros+ license required for CLI access
```

**Giải pháp:**

1. Xác minh rằng bạn có đăng ký Chloros+ đang hoạt động
2. Đăng nhập bằng thông tin đăng nhập của bạn:

```bash
chloros-cli login user@example.com 'password'
```

3. Kiểm tra tình trạng giấy phép:

```bash
chloros-cli status
```

4. Liên hệ hỗ trợ: info@mapir.camera

***

### Không tìm thấy hình ảnh nào**Lỗi:**

```

No images found in the specified folder
```

**Giải pháp:**

1. Xác minh thư mục chứa các định dạng được hỗ trợ (.RAW, .TIF, .JPG)
2. Kiểm tra đường dẫn thư mục có chính xác không (sử dụng dấu ngoặc kép cho đường dẫn có dấu cách)
3. Đảm bảo bạn có quyền đọc thư mục
4. Kiểm tra phần mở rộng của file có chính xác không

***

### Đang xử lý bị treo hoặc bị treo**Giải pháp:**

1. Kiểm tra dung lượng đĩa còn trống (đảm bảo đủ cho đầu ra)
2. Đóng các ứng dụng khác để giải phóng bộ nhớ
3. Giảm số lượng ảnh (xử lý theo đợt)

***

### Cổng đã được sử dụng**Lỗi:**

```

Port 5000 is already in use
```

**Giải pháp:**

**Windows:**

```powershell
chloros-cli --port 5001 process "C:\Datasets\Field_A"
```

**Linux:**

```bash
# Find what's using port 5000
lsof -i :5000

# Use a different port
chloros-cli --port 5001 process ~/datasets/field_a
```

***

##Câu hỏi thường gặp

### Hỏi: Tôi có cần giấy phép cho CLI không?

**Đ:**Vâng! CLI yêu cầu giấy phép**Chloros+ trả phí**.

* ❌ Gói tiêu chuẩn (miễn phí): CLI bị vô hiệu hóa
* ✅ Gói Chloros+ (trả phí): CLI được kích hoạt đầy đủ

Đăng ký tại: [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing)

***

### Hỏi: Tôi có thể sử dụng CLI trên máy chủ không có GUI không?**Đ:** Vâng! CLI chạy hoàn toàn không có đầu. Đây là trường hợp sử dụng chính trên Linux.**Máy chủ Windows:**
* Máy chủ Windows 2016 trở lên
* Đã cài đặt Visual C++ có thể phân phối lại

**Máy chủ Linux:**
* Ubuntu 20.04+ / Debian 11+ (amd64) hoặc JetPack 6 (arm64)
* Cài đặt qua gói `.deb`

**Cả hai nền tảng:**
* RAM tối thiểu 8GB (khuyến nghị 16GB)
* Kích hoạt giấy phép một lần: `chloros-cli login user@example.com 'password'`

***

### Hỏi: Hình ảnh đã xử lý được lưu ở đâu?**A:**Theo mặc định, hình ảnh đã xử lý sẽ được lưu trong**cùng thư mục với đầu vào** trong các thư mục con của mẫu máy ảnh (ví dụ: `Survey3N_RGN/`).

Sử dụng tùy chọn `-o` để chỉ định thư mục đầu ra khác:

```bash
# Windows
chloros-cli process "C:\Input" -o "D:\Output"

# Linux
chloros-cli process ~/input -o ~/output
```

***

### Hỏi: Tôi có thể xử lý nhiều thư mục cùng một lúc không?**A:** Không trực tiếp bằng một lệnh, nhưng bạn có thể sử dụng tập lệnh để xử lý các thư mục một cách tuần tự. Xem phần [Tự động hóa & viết tập lệnh](CLI.md#automation--scripting).***

### Hỏi: Làm cách nào để lưu đầu ra CLI vào tệp nhật ký?**PowerShell:**

```powershell
chloros-cli process "C:\Datasets\Field_A" | Tee-Object -FilePath "processing.log"
```

**Đợt:**

```batch
chloros-cli process "C:\Datasets\Field_A" > processing.log 2>&1
```

**Linux Bash:**

```bash
chloros-cli process ~/datasets/field_a 2>&1 | tee processing.log
```

***

### Hỏi: Điều gì xảy ra nếu tôi nhấn Ctrl+C trong khi xử lý?**A:** CLI sẽ:

1. Dừng xử lý một cách duyên dáng
2. Tắt phần phụ trợ
3. Thoát với mã 130

Hình ảnh được xử lý một phần có thể vẫn còn trong thư mục đầu ra.

***

### Hỏi: Tôi có thể tự động xử lý CLI không?**Đ:** Chắc chắn rồi! CLI được thiết kế để tự động hóa. Xem [Tự động hóa & viết tập lệnh](CLI.md#automation--scripting) để biết các ví dụ về PowerShell (Windows), Batch (Windows), Bash (Linux) và Python (đa nền tảng).***

### Hỏi: Làm cách nào để kiểm tra phiên bản CLI?**MỘT:**

```bash
chloros-cli --version
```

**Đầu ra:**

```

Chloros CLI 1.1.0
```

***

## Nhận trợ giúp

### Trợ giúp dòng lệnh

Xem thông tin trợ giúp trực tiếp trong CLI:

```bash
# General help
chloros-cli --help

# Command-specific help
chloros-cli process --help
chloros-cli login --help
chloros-cli language --help
```

### Kênh hỗ trợ

* **Email**: info@mapir.camera
* **Trang web**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* **Giá**: [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing)***

## Ví dụ hoàn chỉnh

### Ví dụ 1: Xử lý cơ bản

Xử lý với cài đặt mặc định (làm mờ nét ảnh, độ phản chiếu):

**Windows:**

```powershell
chloros-cli process "C:\Datasets\Field_A_2025_01_15"
```

**Linux:**

```bash
chloros-cli process ~/datasets/field_a_2025_01_15
```

***

### Ví dụ 2: Sản phẩm khoa học chất lượng cao

Phao 32 bit TIFF:

**Windows:**

```powershell
chloros-cli process "C:\Datasets\Field_A" ^
  --format "TIFF (32-bit, Percent)" ^
  --vignette ^
  --reflectance
```

**Linux:**

```bash
chloros-cli process ~/datasets/field_a \
  --format "TIFF (32-bit, Percent)" \
  --vignette \
  --reflectance
```

***

### Ví dụ 3: Xử lý xem trước nhanh

PNG 8 bit không cần hiệu chỉnh để xem xét nhanh:

**Windows:**

```powershell
chloros-cli process "C:\Datasets\Field_A" ^
  --format "PNG (8-bit)" ^
  --no-vignette ^
  --no-reflectance
```

**Linux:**

```bash
chloros-cli process ~/datasets/field_a \
  --format "PNG (8-bit)" \
  --no-vignette \
  --no-reflectance
```

***

### Ví dụ 4: Xử lý PPK-Cor Corrected

Áp dụng hiệu chỉnh PPK với độ phản xạ:

**Windows:**

```powershell
chloros-cli process "C:\Datasets\Field_A" ^
  --ppk ^
  --reflectance
```

**Linux:**

```bash
chloros-cli process ~/datasets/field_a \
  --ppk \
  --reflectance
```

***

### Ví dụ 5: Vị trí đầu ra tùy chỉnh

Xử lý đến một vị trí khác với định dạng cụ thể:

**Windows:**

```powershell
chloros-cli process "C:\Input\Raw_Images" ^
  -o "D:\Output\Processed" ^
  --format "TIFF (16-bit)"
```

**Linux:**

```bash
chloros-cli process ~/input/raw_images \
  -o ~/output/processed \
  --format "TIFF (16-bit)"
```

***

### Ví dụ 6: Quy trình xác thực

Hoàn tất quy trình xác thực (giống nhau trên tất cả các nền tảng):

```bash
# Step 1: Login
chloros-cli login user@example.com 'MyP@ssw0rd'

# Step 2: Verify status
chloros-cli status

# Step 3: Process images
# Windows: chloros-cli process "C:\Datasets\Field_A"
# Linux:   chloros-cli process ~/datasets/field_a
chloros-cli process ~/datasets/field_a

# Step 4: Logout (optional, when switching accounts)
chloros-cli logout
```

***

### Ví dụ 7: Sử dụng đa ngôn ngữ

Thay đổi ngôn ngữ giao diện (giống nhau trên tất cả các nền tảng):

```bash
# List available languages
chloros-cli language --list

# Change to Spanish
chloros-cli language es

# Process with Spanish interface
# Windows: chloros-cli process "C:\Vuelos\Campo_A"
# Linux:   chloros-cli process ~/vuelos/campo_a
chloros-cli process ~/vuelos/campo_a

# Change back to English
chloros-cli language en
```