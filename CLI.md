# CLI: Dòng lệnh

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>

**Chloros CLI** cung cấp khả năng truy cập dòng lệnh mạnh mẽ vào công cụ xử lý hình ảnh Chloros, cho phép tự động hóa, tạo tập lệnh và vận hành không đầu cho quy trình xử lý hình ảnh của bạn.

### Các tính năng chính

* 🚀 **Tự động hóa** - Xử lý hàng loạt tập lệnh của nhiều tập dữ liệu
* 🔗 **Tích hợp** - Nhúng vào quy trình công việc và quy trình hiện có
* 💻 **Hoạt động không đầu** - Chạy không cần GUI
* 🌍 **Đa ngôn ngữ** - Hỗ trợ 38 ngôn ngữ
* ⚡ **Xử lý song song** - Tự động chia tỷ lệ cho CPU của bạn (tối đa 16 nhân viên song song)

### Yêu cầu

| Yêu cầu          | Chi tiết                                                             |
| -------------------- | ------------------------------------------------------------------- |
|**Hệ điều hành** | Windows 10/11 (64-bit)                                              |
|**Giấy phép**          | Chloros+ ([paid plan required](https://cloud.mapir.camera/pricing)) |
|**Ký ức**           | RAM tối thiểu 8GB (khuyến nghị 16GB)                                  |
|**Internet**         | Cần thiết để kích hoạt giấy phép                                     |
|**Dung lượng đĩa**       | Thay đổi theo quy mô dự án                                              |

{% hint style="warning" %}**Yêu cầu về giấy phép**: CLI yêu cầu đăng ký Chloros+ trả phí. Các gói tiêu chuẩn (miễn phí) không có quyền truy cập CLI. Thăm nom [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing) để nâng cấp.
{% endhint %}

## Bắt đầu nhanh

### Cài đặt

CLI được tự động đưa vào trình cài đặt Chloros:

1. Tải xuống và chạy**Chloros Installer.exe**
2. Hoàn tất trình hướng dẫn cài đặt
3. CLI được cài đặt để:`C:\Program Files\Chloros\resources\cli\chloros-cli.exe`

{% hint style="success" %}
Trình cài đặt tự động thêm`chloros-cli`vào PATH hệ thống của bạn. Khởi động lại thiết bị đầu cuối của bạn sau khi cài đặt.
{% endhint %}

### Thiết lập lần đầu

Trước khi sử dụng CLI, hãy kích hoạt giấy phép Chloros+ của bạn:

```bash
# Login with your Chloros+ account
chloros-cli login user@example.com 'your_password'

# Check license status
chloros-cli status

# Process your first project
chloros-cli process "C:\Images\Dataset001"
```

### Cách sử dụng cơ bản

Xử lý thư mục có cài đặt mặc định:

```powershell
chloros-cli process "C:\Images\Dataset001"
```***

## Tham chiếu lệnh

### Cú pháp chung

```
chloros-cli [global-options] <command> [command-options]
```

***

## Lệnh

### `process` - Process Images

Xử lý hình ảnh trong một thư mục có hiệu chuẩn.**Cú pháp:**

```bash
chloros-cli process <input-folder> [options]
```**Ví dụ:**

```powershell
chloros-cli process "C:\Datasets\Survey_001" --vignette --reflectance
```

#### Tùy chọn lệnh xử lý

| Lựa chọn                | Kiểu    | Mặc định        | Sự miêu tả                                                                            |
| --------------------- | ------- | -------------- | -------------------------------------------------------------------------------------- |
| `<input-folder>`      | Con đường    | _Required_     | Thư mục chứa ảnh đa phổ RAW/JPG                                         |
| `-o, --output`        | Con đường    | Tương tự như đầu vào  | Thư mục đầu ra cho hình ảnh được xử lý                                                     |
| `-n, --project-name`  | Sợi dây  | Được tạo tự động | Tên dự án tùy chỉnh                                                                    |
| `--vignette`          | Lá cờ    | Đã bật        | Bật tính năng chỉnh sửa họa tiết                                                             |
| `--no-vignette`       | Lá cờ    | -              | Tắt tính năng chỉnh sửa họa tiết                                                            |
| `--reflectance`       | Lá cờ    | Đã bật        | Kích hoạt hiệu chuẩn phản xạ                                                         |
| `--no-reflectance`    | Lá cờ    | -              | Vô hiệu hóa hiệu chỉnh phản xạ                                                        |
| `--ppk`               | Lá cờ    | Tàn tật       | Áp dụng hiệu chỉnh PPK từ dữ liệu cảm biến ánh sáng .daq                                      |
| `--format`            | Sự lựa chọn  | TIFF (16-bit)  | Output format: `TIFF (16-bit)`, `TIFF (32-bit, Percent)`, `PNG (8-bit)`, `JPG (8-bit)` |
| `--min-target-size`   | số nguyên | Tự động           | Kích thước mục tiêu tối thiểu tính bằng pixel để phát hiện bảng hiệu chuẩn                          |
| `--target-clustering` | số nguyên | Tự động           | Ngưỡng phân cụm mục tiêu (0-100)                                                    |
| `--exposure-pin-1`    | Sợi dây  | Không có           | Khóa phơi sáng cho mẫu máy ảnh (Chân 1)                                                 |
| `--exposure-pin-2`    | Sợi dây  | Không có           | Khóa phơi sáng cho mẫu máy ảnh (Chân 2)                                                 |
| `--recal-interval`    | số nguyên | Tự động           | Khoảng thời gian hiệu chuẩn lại tính bằng giây                                                      |
| `--timezone-offset`   | số nguyên | 0              | Độ lệch múi giờ tính bằng giờ                                                               |***

### `login` - Authenticate Account

Đăng nhập bằng thông tin đăng nhập Chloros+ của bạn để kích hoạt xử lý CLI.

**Cú pháp:**

```bash
chloros-cli login <email> <password>
```**Ví dụ:**

```powershell
chloros-cli login user@example.com 'MyP@ssw0rd123'
```

{% hint style="warning" %}**Ký tự đặc biệt**: Sử dụng dấu ngoặc đơn xung quanh mật khẩu chứa các ký tự như`$`, `!`, hoặc dấu cách.
{% endhint %}**đầu ra:**

<figure><img src=".gitbook/assets/cli login_w.JPG" alt=""><figcaption></figcaption></figure>***

### `logout` - Clear Credentials

Xóa thông tin đăng nhập được lưu trữ và đăng xuất khỏi tài khoản của bạn.

**Cú pháp:**

```bash
chloros-cli logout
```**Ví dụ:**

```powershell
chloros-cli logout
```**đầu ra:**

```
✓ Logout successful
ℹ Credentials cleared from cache
```***

### `status` - Check License Status

Hiển thị giấy phép hiện tại và trạng thái xác thực.

**Cú pháp:**

```bash
chloros-cli status
```**Ví dụ:**

```powershell
chloros-cli status
```**đầu ra:**

```
╔══════════════════════════════════════╗
║     LICENSE & ACCOUNT INFORMATION    ║
╚══════════════════════════════════════╝

📧 Email: user@example.com
📋 Plan: Chloros+ Professional
🔓 API/CLI Access: Enabled
✓ Status: Active
```***

### `export-status` - Check Export Progress

Theo dõi tiến trình xuất Thread 4 trong hoặc sau khi xử lý.

**Cú pháp:**

```bash
chloros-cli export-status
```**Ví dụ:**

```powershell
chloros-cli export-status
```**Trường hợp sử dụng:** Call this command while processing is running to check export progress.***

### `language` - Manage Interface Language

Xem hoặc thay đổi ngôn ngữ giao diện CLI.

**Cú pháp:**

```bash
# Show current language
chloros-cli language

# List all available languages
chloros-cli language --list

# Set a specific language
chloros-cli language <language-code>
```**Ví dụ:**

```powershell
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

| Mã số    | Ngôn ngữ              | Tên bản xứ      |
| ------- | --------------------- | ---------------- |
| `en`    | Tiếng Anh               | Tiếng Anh          |
| `es`    | tiếng Tây Ban Nha               | tiếng Tây Ban Nha          |
| `pt`    | tiếng Bồ Đào Nha            | người Bồ Đào Nha        |
| `fr`    | người Pháp                | người Pháp         |
| `de`    | tiếng Đức                | tiếng Đức          |
| `it`    | người Ý               | tiếng Ý         |
| `ja`    | tiếng Nhật              | 日本語              |
| `ko`    | Tiếng Hàn                | 한국어              |
| `zh`    | Tiếng Trung (Giản thể)  | 简体中文             |
| `zh-TW` | Tiếng Trung (truyền thống) | 繁體中文             |
| `ru`    | tiếng Nga               | Русский          |
| `nl`    | tiếng Hà Lan                 | Hà Lan       |
| `ar`    | tiếng Ả Rập                | عربية          |
| `pl`    | Đánh bóng                | Tiếng Ba Lan           |
| `tr`    | tiếng Thổ Nhĩ Kỳ               | Türkçe           |
| `hi`    | Tiếng Hindi                 | हिंदी            |
| `id`    | tiếng Indonesia            | Tiếng Bahasa Indonesia |
| `vi`    | Tiếng Việt            | Tiếng Việt       |
| `th`    | tiếng Thái                  | ไทย              |
| `sv`    | tiếng Thụy Điển               | Svenska          |
| `da`    | tiếng Đan Mạch                | Đan Mạch            |
| `no`    | người Na Uy             | Norsk            |
| `fi`    | tiếng Phần Lan               | Suomi            |
| `el`    | tiếng Hy Lạp                 | Ελληνικά         |
| `cs`    | tiếng Séc                 | Čeština          |
| `hu`    | tiếng Hungary             | Tiếng Magyar           |
| `ro`    | người Rumani              | Româna           |
| `uk`    | tiếng Ukraina             | Українська       |
| `pt-BR` | Tiếng Bồ Đào Nha Brazil  | Bồ Đào Nha Brasileiro |
| `zh-HK` | tiếng Quảng Đông             | 粵語             |
| `ms`    | Mã Lai                 | Tiếng Bahasa Melayu    |
| `sk`    | Tiếng Slovak                | Tiếng Slovenia       |
| `bg`    | tiếng Bungari             | Български        |
| `hr`    | tiếng Croatia              | Hrvatski         |
| `lt`    | tiếng Litva            | Liệtuvių         |
| `lv`    | tiếng Latvia               | Latvian         |
| `et`    | tiếng Estonia              | Eesti            |
| `sl`    | tiếng Slovenia             | Tiếng Sloveniaščina      |

{% hint style="success" %}**Tính kiên trì tự động**: Tùy chọn ngôn ngữ của bạn được lưu vào`~/.chloros/cli_language.json`và tồn tại trong tất cả các phiên.
{% endhint %}***

### `set-project-folder` - Set Default Project Folder

Thay đổi vị trí thư mục dự án mặc định (được chia sẻ với GUI).

**Cú pháp:**

```bash
chloros-cli set-project-folder <folder-path>
```**Ví dụ:**

```powershell
chloros-cli set-project-folder "C:\Projects\2025"
```***

### `get-project-folder` - Show Project Folder

Hiển thị vị trí thư mục dự án mặc định hiện tại.

**Cú pháp:**

```bash
chloros-cli get-project-folder
```**Ví dụ:**

```powershell
chloros-cli get-project-folder
```**đầu ra:**

```
ℹ Current project folder: C:\Projects\2025
```***

### `reset-project-folder` - Reset to Default

Đặt lại thư mục dự án về vị trí mặc định.

**Cú pháp:**

```bash
chloros-cli reset-project-folder
```***

## Tùy chọn toàn cầu

Các tùy chọn này áp dụng cho tất cả các lệnh:

| Lựa chọn          | Kiểu    | Mặc định       | Sự miêu tả                                      |
| --------------- | ------- | ------------- | ------------------------------------------------ |
| `--backend-exe` | Con đường    | Tự động phát hiện | Đường dẫn đến phần thực thi phụ trợ                       |
| `--port`        | số nguyên | 5000          | Số cổng API phụ trợ                          |
| `--restart`     | Lá cờ    | -             | Buộc khởi động lại chương trình phụ trợ (giết chết các tiến trình hiện có) |
| `--version`     | Lá cờ    | -             | Hiển thị thông tin phiên bản và thoát                |
| `--help`        | Lá cờ    | -             | Hiển thị thông tin trợ giúp và thoát                   |

**Ví dụ với Tùy chọn chung:**

```powershell
chloros-cli --port 5001 process "C:\Datasets\Survey_001"
```***

## Hướng dẫn cài đặt xử lý

### Xử lý song song

Chloros+ CLI **tự động chia tỷ lệ** xử lý song song để phù hợp với khả năng của máy tính của bạn:**Nó hoạt động như thế nào:**

* Phát hiện lõi CPU và RAM của bạn
* Phân bổ nhân viên: **2× lõi CPU** (sử dụng siêu phân luồng)
* **Tối đa: 16 công nhân song song** (để ổn định)**Cấp hệ thống:**

| Loại hệ thống   | CPU        | ĐẬP      | Công nhân  | Hiệu suất     |
| ------------- | ---------- | -------- | -------- | --------------- |
|**Cao cấp**  | 16+ lõi  | 32+GB   | Lên đến 16 | Tốc độ tối đa   |
|**Tầm trung** | 8-15 lõi | 16-31GB | 8-16     | Tốc độ tuyệt vời |
|**Cấp thấp**   | 4-7 lõi  | 8-15 GB  | 4-8      | Tốc độ tốt      |

{% hint style="success" %}**Tối ưu hóa tự động**: CLI tự động phát hiện thông số kỹ thuật hệ thống của bạn và định cấu hình xử lý song song tối ưu. Không cần cấu hình thủ công!
{% endhint %}

### Phương pháp Debayer

CLI sử dụng**Chất lượng cao (Nhanh hơn)** làm thuật toán gỡ lỗi mặc định và được đề xuất:

| Phương pháp                      | Chất lượng | Tốc độ | Sự miêu tả                                 |
| -------------------------- | ------- | ----- | ------------------------------------------ |
|**Chất lượng cao (Nhanh hơn)** ⭐ | ⭐⭐⭐⭐    | ⚡⚡⚡   | Thuật toán nhận biết cạnh (mặc định, được khuyến nghị) |

### Chỉnh sửa họa tiết**Nó làm gì:** Corrects light falloff at image edges (darker corners common in camera imagery).

* **Bật theo mặc định** - Hầu hết người dùng nên bật tính năng này
* Sử dụng`--no-vignette`vô hiệu hóa

{% hint style="success" %}
**Khuyến nghị**: Luôn bật tính năng chỉnh sửa họa tiết để đảm bảo độ sáng đồng đều trên toàn khung hình.
{% endhint %}

### Hiệu chỉnh phản xạ

Chuyển đổi giá trị cảm biến thô thành tỷ lệ phần trăm phản xạ được tiêu chuẩn hóa bằng bảng hiệu chuẩn.

* **Bật theo mặc định** - Cần thiết cho phân tích thảm thực vật
* Yêu cầu bảng mục tiêu hiệu chuẩn trong hình ảnh
* Sử dụng`--no-reflectance`vô hiệu hóa

{% hint style="info" %}
**Yêu cầu**: Đảm bảo bảng hiệu chuẩn được hiển thị đúng cách và hiển thị trong hình ảnh của bạn để chuyển đổi độ phản xạ chính xác.
{% endhint %}

### Chỉnh sửa PPK**Nó làm gì:** Applies Post-Processed Kinematic corrections using DAQ-A-SD log data for improved GPS accuracy.

* ** Bị tắt theo mặc định**
* Sử dụng`--ppk`để kích hoạt
* Yêu cầu tệp .daq trong thư mục dự án từ cảm biến ánh sáng MAPIR DAQ-A-SD.

### Định dạng đầu ra

<table><thead><tr><th width="197">Định dạng</th><th width="130.20001220703125">Độ sâu bit</th><th width="116.5999755859375">Kích thước tệp</th><th>Tốt nhất cho</th></tr></thead><tbody><tr><td><strong>TIFF (16-bit)</strong> ⭐</td><td>Số nguyên 16 bit</td><td>Lớn</td><td>Phân tích GIS, phép đo ảnh (được khuyến nghị)</td></tr><tr><td><strong>TIFF (32 bit, Phần trăm)</strong></td><td>Phôi nổi 32 bit</td><td>Rất lớn</td><td>Phân tích khoa học, nghiên cứu</td></tr><tr><td><strong>PNG (8 bit)</strong></td><td>Số nguyên 8 bit</td><td>Trung bình</td><td>Kiểm tra trực quan, chia sẻ web</td></tr><tr><td><strong>JPG (8 bit)</strong></td><td>Số nguyên 8 bit</td><td>Nhỏ</td><td>Xem trước nhanh, được nén đầu ra</td></tr></tbody></table>

***

## Tự động hóa & Viết kịch bản

### Xử lý hàng loạt PowerShell

Tự động xử lý nhiều thư mục dữ liệu:

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

### Tập lệnh hàng loạt Windows

Vòng lặp đơn giản để xử lý hàng loạt:

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

### Tập lệnh tự động hóa Python

Tự động hóa nâng cao với xử lý lỗi:

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

## Xử lý quy trình làm việc

### Quy trình làm việc tiêu chuẩn

1.**Đầu vào**: Thư mục chứa các cặp ảnh RAW/JPG
2.**Khám phá**: CLI tự động quét để tìm các tệp hình ảnh được hỗ trợ
3.**Đang xử lý**: Chế độ song song sẽ mở rộng theo lõi CPU của bạn (Chloros+)
4.**Đầu ra**: Tạo các thư mục con kiểu máy ảnh chứa hình ảnh đã được xử lý

### Cấu trúc đầu ra ví dụ

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

| Cách thức              | Thời gian      | Phần cứng                                     |
| ------------------ | --------- | -------------------------------------------- |
|**Chế độ song song** | 5-10 phút  | i7/Ryzen 7, RAM 16GB, SSD (tối đa 16 nhân viên) |
|**Chế độ song song** | 10-15 phút | i5/Ryzen 5, RAM 8GB, HDD (tối đa 8 nhân viên)   |

{% hint style="info" %}**Mẹo về hiệu suất**: Thời gian xử lý thay đổi tùy theo số lượng hình ảnh, độ phân giải và thông số kỹ thuật của máy tính.
{% endhint %}***

## Khắc phục sự cố

### CLI Không tìm thấy

**Lỗi:**

```
'chloros-cli' is not recognized as an internal or external command
```**Giải pháp:**

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
* Thêm vào:`C:\Program Files\Chloros\resources\cli`
* Khởi động lại thiết bị đầu cuối

***

### Phần cuối không thể bắt đầu**Lỗi:**

```
Backend failed to start within 30 seconds
```**Giải pháp:**

1. Kiểm tra xem chương trình phụ trợ đã chạy chưa (đóng nó trước)
2. Kiểm tra Tường lửa Windows không chặn
3. Hãy thử cổng khác:

```powershell
chloros-cli --port 5001 process "C:\Datasets\Field_A"
```

4. Buộc khởi động lại chương trình phụ trợ:

```powershell
chloros-cli --restart process "C:\Datasets\Field_A"
```***

### Vấn đề về giấy phép / xác thực

**Lỗi:**

```
Chloros+ license required for CLI access
```**Giải pháp:**

1. Xác minh bạn có đăng ký Chloros+ đang hoạt động
2. Đăng nhập bằng thông tin đăng nhập của bạn:

```powershell
chloros-cli login user@example.com 'password'
```

3. Kiểm tra trạng thái giấy phép:

```powershell
chloros-cli status
```

4. Liên hệ hỗ trợ: info@mapir.Camera***

### Không tìm thấy hình ảnh nào

**Lỗi:**

```
No images found in the specified folder
```**Giải pháp:**

1. Xác minh thư mục chứa các định dạng được hỗ trợ (.RAW, .TIF, .JPG)
2. Kiểm tra đường dẫn thư mục có chính xác không (sử dụng dấu ngoặc kép cho đường dẫn có dấu cách)
3. Đảm bảo bạn có quyền đọc cho thư mục
4. Kiểm tra phần mở rộng tập tin là chính xác***

### Xử lý gian hàng hoặc treo

**Giải pháp:**

1. Kiểm tra dung lượng đĩa trống (đảm bảo đủ cho đầu ra)
2. Đóng các ứng dụng khác để giải phóng bộ nhớ
3. Giảm số lượng hình ảnh (xử lý theo đợt)***

### Cổng đã được sử dụng

**Lỗi:**

```
Port 5000 is already in use
```**Giải pháp:**

Chỉ định một cổng khác:

```powershell
chloros-cli --port 5001 process "C:\Datasets\Field_A"
```***

## Câu hỏi thường gặp

### Hỏi: Tôi có cần giấy phép cho CLI không?

**A:** Yes! The CLI requires a paid**Chloros+ license**.

* ❌ Gói tiêu chuẩn (miễn phí): CLI bị vô hiệu hóa
* ✅ Gói Chloros+ (trả phí): CLI được kích hoạt đầy đủ

Đăng ký tại: [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing)

***

### Câu hỏi: Tôi có thể sử dụng CLI trên máy chủ không có GUI không?**A:** Yes! The CLI runs completely headless. Requirements:

* Windows Server 2016 trở lên
* Visual C++ có thể phân phối lại được cài đặt
* Đủ RAM (tối thiểu 8GB, khuyến nghị 16GB)
* Kích hoạt giấy phép GUI một lần trên bất kỳ máy nào

***

### Hỏi: Hình ảnh đã xử lý được lưu ở đâu?**A:** By default, processed images are saved in the**same folder as input** in camera-model subfolders (e.g., `Survey3N_RGN/`).

Sử dụng`-o`tùy chọn để chỉ định thư mục đầu ra khác nhau:

```powershell
chloros-cli process "C:\Input" -o "D:\Output"
```***

### Câu hỏi: Tôi có thể xử lý nhiều thư mục cùng một lúc không?

**A:** Not directly in one command, but you can use scripting to process folders sequentially. See [Automation & Scripting](CLI.md#automation--scripting) section.***

### Câu hỏi: Làm cách nào để lưu đầu ra CLI vào tệp nhật ký?

**PowerShell:**

```powershell
chloros-cli process "C:\Datasets\Field_A" | Tee-Object -FilePath "processing.log"
```**Lô:**

```batch
chloros-cli process "C:\Datasets\Field_A" > processing.log 2>&1
```***

### Hỏi: Điều gì xảy ra nếu tôi nhấn Ctrl+C trong khi xử lý?

**A:** The CLI will:

1. Dừng xử lý một cách duyên dáng
2. Tắt phần phụ trợ
3. Thoát với mã 130

Hình ảnh được xử lý một phần có thể vẫn còn trong thư mục đầu ra.***

### Câu hỏi: Tôi có thể tự động xử lý CLI không?

**A:** Absolutely! The CLI is designed for automation. See [Automation & Scripting](CLI.md#automation--scripting) for PowerShell, Batch, and Python examples.***

### Hỏi: Làm cách nào để kiểm tra phiên bản CLI?

**A:**

```powershell
chloros-cli --version
```**đầu ra:**

```
Chloros CLI 1.0.2
```***

## Nhận trợ giúp

### Trợ giúp dòng lệnh

Xem thông tin trợ giúp trực tiếp trong CLI:

```powershell
# General help
chloros-cli --help

# Command-specific help
chloros-cli process --help
chloros-cli login --help
chloros-cli language --help
```

### Kênh hỗ trợ

* **Email**: info@mapir.máy ảnh
* **Trang web**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* **Giá**: [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing)***

## Ví dụ hoàn chỉnh

### Ví dụ 1: Xử lý cơ bản

Xử lý với cài đặt mặc định (làm mờ nét ảnh, độ phản chiếu):

```powershell
chloros-cli process "C:\Datasets\Field_A_2025_01_15"
```

***

### Ví dụ 2: Sản phẩm khoa học chất lượng cao

TIFF nổi 32 bit:

```powershell
chloros-cli process "C:\Datasets\Field_A" ^
  --format "TIFF (32-bit, Percent)" ^
  --vignette ^
  --reflectance
```***

### Ví dụ 3: Xử lý xem trước nhanh

PNG 8 bit không cần hiệu chỉnh để xem nhanh:

```powershell
chloros-cli process "C:\Datasets\Field_A" ^
  --format "PNG (8-bit)" ^
  --no-vignette ^
  --no-reflectance
```

***

### Ví dụ 4: Xử lý đã sửa PPK

Áp dụng hiệu chỉnh PPK với độ phản xạ:

```powershell
chloros-cli process "C:\Datasets\Field_A" ^
  --ppk ^
  --reflectance
```***

### Ví dụ 5: Vị trí đầu ra tùy chỉnh

Xử lý sang ổ đĩa khác với định dạng cụ thể:

```powershell
chloros-cli process "C:\Input\Raw_Images" ^
  -o "D:\Output\Processed" ^
  --format "TIFF (16-bit)"
```

***

### Ví dụ 6: Quy trình xác thực

Luồng xác thực hoàn chỉnh:

```powershell
# Step 1: Login
chloros-cli login user@example.com 'MyP@ssw0rd'

# Step 2: Verify status
chloros-cli status

# Step 3: Process images
chloros-cli process "C:\Datasets\Field_A"

# Step 4: Logout (optional, when switching accounts)
chloros-cli logout
```***

### Ví dụ 7: Sử dụng đa ngôn ngữ

Thay đổi ngôn ngữ giao diện:

```powershell
# List available languages
chloros-cli language --list

# Change to Spanish
chloros-cli language es

# Process with Spanish interface
chloros-cli process "C:\Vuelos\Campo_A"

# Change back to English
chloros-cli language en
```
