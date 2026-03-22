#API : Python SDK

**Chloros Python SDK** cung cấp quyền truy cập theo chương trình vào công cụ xử lý hình ảnh Chloros, cho phép tự động hóa, quy trình làm việc tùy chỉnh và tích hợp liền mạch với các ứng dụng Python của bạn và quy trình nghiên cứu.

### Các tính năng chính

* 🐍 **Python** - API sạch, Pythonic để xử lý hình ảnh
* 🔧 **Quyền truy cập API đầy đủ** - Kiểm soát hoàn toàn quá trình xử lý Chloros
* 🚀 **Tự động hóa** - Xây dựng quy trình xử lý hàng loạt tùy chỉnh
* 🔗 **Tích hợp** - Nhúng Chloros vào các ứng dụng Python hiện có
* 📊 **Sẵn sàng cho nghiên cứu** - Hoàn hảo cho quy trình phân tích khoa học
* ⚡ **Xử lý song song** - Cân theo lõi CPU của bạn (Chloros+)

### Yêu cầu

| Yêu cầu | Chi tiết |
| -------------------- | ------------------------------------------------------------------- |
| **Đã cài đặt Chloros** | Windows: Trình cài đặt máy tính để bàn; Linux: Gói `.deb` |
| **Giấy phép** | Chloros+ ([yêu cầu gói trả phí](https://cloud.mapir.camera/pricing)) |
| **Hệ điều hành** | Windows 10/11 (64-bit), Linux x86_64 (AMD64), Linux arm64 (NVIDIA Jetson JetPack 6) |
| **Python** | Python 3.7 trở lên |
| **Bộ nhớ** | RAM tối thiểu 8GB (khuyến nghị 16GB) |
| **Internet** | Cần thiết để kích hoạt giấy phép |

{% hint style="warning" %}
**Yêu cầu về giấy phép**: Python SDK yêu cầu đăng ký Chloros+ trả phí để truy cập API. Các gói tiêu chuẩn (miễn phí) không có quyền truy cập API/SDK. Truy cập [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing) để nâng cấp.
{% endhint %}

## Bắt đầu nhanh

### Cài đặt

Cài đặt qua pip:

```bash
pip install chloros-sdk
```

{% hint style="info" %}
**Thiết lập lần đầu**: Trước khi sử dụng SDK, hãy kích hoạt giấy phép Chloros+ của bạn bằng cách mở Chloros, Chloros (Trình duyệt) hoặc Chloros CLI và đăng nhập bằng thông tin xác thực của bạn. Điều này chỉ cần được thực hiện một lần. Trên Linux (không có GUI), hãy sử dụng: `chloros-cli login user@example.com 'password'`
{% endhint %}

### Cách sử dụng cơ bản

Xử lý một thư mục chỉ với một vài dòng:

```python
from chloros_sdk import process_folder

# One-line processing (Windows)
results = process_folder("C:\\DroneImages\\Flight001")

# One-line processing (Linux)
results = process_folder("/home/user/drone_images/flight001")
```

{% hint style="info" %}
**Đường dẫn đa nền tảng**: Ví dụ về mã trên trang này sử dụng đường dẫn kiểu Windows (ví dụ: `C:\\DroneImages\\Flight001`). Trên Linux, hãy sử dụng đường dẫn Linux thay thế (ví dụ: `/home/user/drone_images/flight001` hoặc `~/drone_images/flight001`). SDK hoạt động giống hệt nhau trên cả hai nền tảng.
{% endhint %}

### Kiểm soát hoàn toàn

Đối với quy trình công việc nâng cao:

```python
from chloros_sdk import ChlorosLocal

# Initialize SDK
chloros = ChlorosLocal()

# Create project
chloros.create_project("MyProject", camera="Survey3N_RGN")

# Import images
chloros.import_images("C:\\DroneImages\\Flight001")  # Windows
# chloros.import_images("/home/user/drone_images/flight001")  # Linux

# Configure settings
chloros.configure(
    vignette_correction=True,
    reflectance_calibration=True,
    indices=["NDVI", "NDRE", "GNDVI"]
)

# Process images
chloros.process(mode="parallel", wait=True)
```

***

## Hướng dẫn cài đặt

### Điều kiện tiên quyết

Trước khi cài đặt SDK, hãy đảm bảo bạn có:

1. **Đã cài đặt Chloros** — Windows: Trình cài đặt máy tính để bàn ([tải xuống](download.md)); Linux: Gói `.deb` ([Cài đặt Linux](linux/linux-installation.md))
2. **Đã cài đặt Python 3.7+** ([python.org](https://www.python.org))
3. **Giấy phép Chloros+ đang hoạt động** ([nâng cấp](https://cloud.mapir.camera/pricing))

### Cài đặt qua pip

**Cài đặt tiêu chuẩn:**

```bash
pip install chloros-sdk
```

**Với sự hỗ trợ theo dõi tiến độ:**

```bash
pip install chloros-sdk[progress]
```

**Cài đặt phát triển:**

```bash
pip install chloros-sdk[dev]
```

### Xác minh cài đặt

Kiểm tra xem SDK đã được cài đặt chính xác chưa:

```python
import chloros_sdk
print(f"Chloros SDK version: {chloros_sdk.__version__}")
```

***

## Thiết lập lần đầu

### Kích hoạt giấy phép

SDK sử dụng cùng giấy phép với Chloros, Chloros (Trình duyệt) và Chloros CLI. Kích hoạt một lần qua GUI hoặc CLI:**Windows:**Mở**Chloros hoặc Chloros (Trình duyệt)** và đăng nhập trên tab Người dùng <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> hoặc sử dụng CLI.**Linux:** Sử dụng CLI (không có GUI):

```bash
chloros-cli login user@example.com 'your_password'
```

Giấy phép được lưu vào bộ nhớ đệm cục bộ và vẫn tồn tại qua các lần khởi động lại.

{% hint style="success" %}
**Thiết lập một lần**: Sau khi đăng nhập qua GUI hoặc CLI, SDK sẽ tự động sử dụng giấy phép được lưu trong bộ nhớ đệm. Không cần xác thực bổ sung!
{% endhint %}

{% hint style="info" %}
**Đăng xuất**: Người dùng SDK có thể xóa thông tin xác thực được lưu trong bộ nhớ đệm theo chương trình bằng phương pháp `logout()`. Xem [phương thức logout()](#logout) trong Tài liệu tham khảo API.
{% endhint %}

### Kiểm tra kết nối

Xác minh SDK có thể kết nối với Chloros:

```python
from chloros_sdk import ChlorosLocal

# Initialize SDK (auto-starts backend if needed)
chloros = ChlorosLocal()

# Check status
status = chloros.get_status()
print(f"Backend running: {status['running']}")
```

***

## Tham khảo API

### Lớp ChlorosLocal

Lớp chính để xử lý hình ảnh Chloros cục bộ.

#### Người xây dựng

```python
ChlorosLocal(
    api_url="http://localhost:5000",     # Backend URL
    auto_start_backend=True,             # Auto-start backend if not running
    backend_exe=None,                    # Backend path (auto-detected)
    timeout=30,                          # Request timeout (seconds)
    backend_startup_timeout=60           # Backend startup timeout
)
```

**Thông số:**

| Tham số | Loại | Mặc định | Mô tả |
| ------------------------- | ---- | ------------------------- | ------------------------------------- |
| `api_url` | str | `"http://localhost:5000"` | URL của chương trình phụ trợ Chloros cục bộ |
| `auto_start_backend` | bool | `True` | Tự động khởi động phụ trợ nếu cần |
| `backend_exe` | str | `None` (tự động phát hiện) | Đường dẫn đến phần thực thi phụ trợ |
| `timeout` | int | `30` | Yêu cầu hết thời gian tính bằng giây |
| `backend_startup_timeout` | int | `60` | Thời gian chờ khởi động phụ trợ (giây) |

**Ví dụ:**

```python
# Default (auto-start backend, auto-detect path on Windows and Linux)
chloros = ChlorosLocal()

# Connect to running backend
chloros = ChlorosLocal(auto_start_backend=False)

# Custom backend path (Windows)
chloros = ChlorosLocal(backend_exe="C:/Custom/chloros-backend.exe")

# Custom backend path (Linux)
chloros = ChlorosLocal(backend_exe="/opt/mapir/chloros/backend/chloros-backend")

# Custom timeout with longer startup (e.g., for Jetson)
chloros = ChlorosLocal(timeout=60, backend_startup_timeout=120)
```

{% hint style="info" %}
**Tự động phát hiện đa nền tảng**: SDK tự động thử đường dẫn phụ trợ chính xác cho nền tảng của bạn:
* **Windows**: `C:\Program Files\MAPIR\Chloros\resources\backend\chloros-backend.exe`
* **Linux (.deb)**: `/usr/lib/chloros/chloros-backend`
* **Linux (thủ công)**: `/opt/mapir/chloros/backend/chloros-backend`
{% endhint %}

***

### Phương thức

#### `create_project(project_name, camera=None)`

Tạo dự án Chloros mới.

**Thông số:**

| Tham số | Loại | Bắt buộc | Mô tả |
| -------------- | ---- | -------- | -------------------------------------------------------- |
| `project_name` | str | Có | Tên dự án |
| `camera` | str | Không | Mẫu máy ảnh (ví dụ: "Survey3N\_RGN", "Survey3W\_OCN") |

**Trả về:** `dict` - Phản hồi tạo dự án**Ví dụ:**

```python
# Basic project
chloros.create_project("DroneField_A")

# With camera template
chloros.create_project("DroneField_A", camera="Survey3N_RGN")
```

***

#### `import_images(folder_path, recursive=False)`

Nhập hình ảnh từ một thư mục.

**Thông số:**

| Tham số | Loại | Bắt buộc | Mô tả |
| ------------- | -------- | -------- | ---------------------------------- |
| `folder_path` | str/Đường dẫn | Có | Đường dẫn đến thư mục có hình ảnh |
| `recursive` | bool | Không | Tìm kiếm thư mục con (mặc định: Sai) |

**Trả về:** `dict` - Nhập kết quả với số lượng tệp**Ví dụ:**

```python
# Import from folder
chloros.import_images("C:\\DroneImages\\Flight001")

# Import recursively
chloros.import_images("C:\\DroneImages", recursive=True)
```

***

#### `configure(**settings)`

Định cấu hình cài đặt xử lý.

**Thông số:**

| Tham số | Loại | Mặc định | Mô tả |
| ------------------------- | ---- | -------------- | ------------------------------- |
| `debayer` | str | "Tiêu Chuẩn (Nhanh, Chất Lượng Trung Bình)" | Phương pháp Debayer |
| `vignette_correction` | bool | `True` | Bật tính năng chỉnh sửa họa tiết |
| `reflectance_calibration` | bool | `True` | Kích hoạt hiệu chuẩn phản xạ |
| `indices` | danh sách | `None` | Chỉ số thực vật cần tính toán |
| `export_format` | str | "TIFF (16-bit)" | Định dạng đầu ra |
| `ppk` | bool | `False` | Kích hoạt tính năng chỉnh sửa PPK |
| `custom_settings` | chính tả | `None` | Cài đặt tùy chỉnh nâng cao |

**Định dạng xuất:**

* `"TIFF (16-bit)"` - Được khuyên dùng cho GIS/phép quang trắc
* `"TIFF (32-bit, Percent)"` - Phân tích khoa học
* `"PNG (8-bit)"` - Kiểm tra bằng mắt
* `"JPG (8-bit)"` - Đầu ra nén

**Chỉ số có sẵn:**NDVI, NDRE, GNDVI, OSAVI, CIG, EVI, SAVI, MSAVI, MTVI2, v.v.**Ví dụ:**

```python
# Basic configuration
chloros.configure(
    vignette_correction=True,
    reflectance_calibration=True,
    indices=["NDVI", "NDRE"]
)

# Advanced configuration
chloros.configure(
    debayer="Standard (Fast, Medium Quality)",
    vignette_correction=True,
    reflectance_calibration=True,
    ppk=True,
    export_format="TIFF (32-bit, Percent)",
    indices=["NDVI", "NDRE", "GNDVI", "OSAVI", "CIG"]
)
```

***

#### `process(mode="parallel", wait=True, progress_callback=None)`

Xử lý hình ảnh dự án

**Thông số:**

| Tham số | Loại | Mặc định | Mô tả |
| ------------------- | -------- | ------------ | ----------------------------------------- |
| `mode` | str | `"parallel"` | Chế độ xử lý: "song song" hoặc "nối tiếp" |
| `wait` | bool | `True` | Chờ hoàn thành |
| `progress_callback` | có thể gọi được | `None` | Hàm gọi lại tiến trình(tiến trình, tin nhắn) |
| `poll_interval` | phao | `2.0` | Khoảng thời gian bỏ phiếu cho tiến trình (giây) |

**Trả về:** `dict` - Kết quả xử lý

{% hint style="warning" %}
**Chế độ song song**: Yêu cầu giấy phép Chloros+. Tự động điều chỉnh quy mô theo lõi CPU của bạn (tối đa 16 nhân viên).
{% endhint %}

**Ví dụ:**

```python
# Simple processing
results = chloros.process()

# With progress monitoring
def show_progress(progress, message):
    print(f"[{progress}%] {message}")

chloros.process(
    mode="parallel",
    progress_callback=show_progress,
    wait=True
)

# Fire-and-forget (non-blocking)
chloros.process(wait=False)
```

***

#### `get_config()`

Nhận cấu hình dự án hiện tại.

**Trả về:** `dict` - Cấu hình dự án hiện tại**Ví dụ:**

```python
config = chloros.get_config()
print(config['Project Settings'])
```

***

#### `get_status()`

Nhận thông tin trạng thái phụ trợ bao gồm tiến trình xử lý theo từng luồng.

**Trả về:** `dict` - Trạng thái phụ trợ có cấu trúc sau:

```python
{
    "running": True,
    "url": "http://localhost:5000",
    "processing": {
        "percent": 75.0,
        "phase": "processing"
    },
    "export": {
        "percent": 50.0,
        "phase": "exporting",
        "active": True
    }
}
```

**Ví dụ:**

```python
status = chloros.get_status()
print(f"Running: {status['running']}")
print(f"URL: {status['url']}")
print(f"Processing: {status['processing']['percent']}%")
print(f"Export: {status['export']['percent']}% - Active: {status['export']['active']}")
```

***

#### `shutdown_backend()`

Tắt phần phụ trợ (nếu được khởi động bởi SDK).

**Ví dụ:**

```python
chloros.shutdown_backend()
```

***

#### `logout()`

Xóa thông tin đăng nhập được lưu trong bộ nhớ cache khỏi hệ thống cục bộ.

**Sự miêu tả:**

Đăng xuất theo chương trình bằng cách xóa thông tin xác thực được lưu trong bộ nhớ đệm. Điều này hữu ích cho:
* Chuyển đổi giữa các tài khoản Chloros+ khác nhau
* Xóa thông tin đăng nhập trong môi trường tự động
* Mục đích bảo mật (ví dụ: xóa thông tin đăng nhập trước khi gỡ cài đặt)

**Trả về:** `dict` - Kết quả hoạt động đăng xuất**Ví dụ:**

```python
from chloros_sdk import ChlorosLocal

# Initialize SDK
chloros = ChlorosLocal()

# Clear cached credentials
result = chloros.logout()
print(f"Logout successful: {result}")

# After logout, login required via GUI/CLI/Browser before next SDK use
```

{% hint style="info" %}
**Yêu cầu xác thực lại**: Sau khi gọi `logout()`, bạn phải đăng nhập lại qua Chloros, Chloros (Trình duyệt) hoặc Chloros CLI trước khi sử dụng SDK.
{% endhint %}

***

###Chức năng tiện lợi

#### `process_folder(folder_path, **options)`

Chức năng tiện lợi một dòng để xử lý một thư mục.

**Thông số:**

| Tham số | Loại | Mặc định | Mô tả |
| ------------------------- | -------- | --------------- | ------------------------------ |
| `folder_path` | str/Đường dẫn | Bắt buộc | Đường dẫn đến thư mục có hình ảnh |
| `project_name` | str | Được tạo tự động | Tên dự án |
| `camera` | str | `None` | Mẫu máy ảnh |
| `indices` | danh sách | `["NDVI"]` | Chỉ số để tính toán |
| `vignette_correction` | bool | `True` | Bật tính năng chỉnh sửa họa tiết |
| `reflectance_calibration` | bool | `True` | Kích hoạt hiệu chuẩn phản xạ |
| `export_format` | str | "TIFF (16-bit)" | Định dạng đầu ra |
| `mode` | str | `"parallel"` | Chế độ xử lý |
| `progress_callback` | có thể gọi được | `None` | Gọi lại tiến độ |

**Trả về:** `dict` - Kết quả xử lý**Ví dụ:**

```python
from chloros_sdk import process_folder

# Simple one-liner
results = process_folder("C:\\DroneImages\\Flight001")

# With custom settings
results = process_folder(
    "C:\\DroneImages\\Flight001",
    project_name="Field_A_Survey",
    camera="Survey3N_RGN",
    indices=["NDVI", "NDRE", "GNDVI"],
    mode="parallel"
)

# With progress monitoring
def show_progress(progress, message):
    print(f"[{progress}%] {message}")

results = process_folder(
    "C:\\DroneImages\\Flight001",
    progress_callback=show_progress
)
```

***

## Hỗ trợ Trình quản lý bối cảnh

SDK hỗ trợ trình quản lý bối cảnh để tự động dọn dẹp:

```python
from chloros_sdk import ChlorosLocal

# Auto-cleanup when done
with ChlorosLocal() as chloros:
    chloros.create_project("MyProject")
    chloros.import_images("C:\\Images")
    chloros.configure(indices=["NDVI"])
    chloros.process()
# Backend automatically shut down here
```

***

## Ví dụ hoàn chỉnh

{% hint style="info" %}
**Người dùng Linux**: Tất cả các ví dụ bên dưới đều sử dụng đường dẫn Windows. Thay thế đường dẫn `C:\\...` bằng đường dẫn Linux của bạn (ví dụ: `/home/user/...` hoặc `~/...`). Tất cả chức năng của SDK đều giống hệt nhau trên các nền tảng.
{% endhint %}

### Ví dụ 1: Xử lý cơ bản

Xử lý thư mục có cài đặt mặc định:

```python
from chloros_sdk import process_folder

# Process with default settings
results = process_folder("C:\\Datasets\\Field_A_2025_01_15")

print(f"Processing complete: {results}")
```

***

### Ví dụ 2: Quy trình làm việc tùy chỉnh

Kiểm soát hoàn toàn đường ống xử lý:

```python
from chloros_sdk import ChlorosLocal

# Initialize SDK
chloros = ChlorosLocal()

# Create project with camera template
chloros.create_project("Research_Plot_A", camera="Survey3N_RGN")

# Import images
import_results = chloros.import_images("C:\\Research\\PlotA")
print(f"Imported {len(import_results.get('files', []))} images")

# Configure advanced settings
chloros.configure(
    debayer="Standard (Fast, Medium Quality)",
    vignette_correction=True,
    reflectance_calibration=True,
    ppk=False,
    export_format="TIFF (16-bit)",
    indices=["NDVI", "NDRE", "GNDVI", "OSAVI"]
)

# Process with progress monitoring
def show_progress(progress, message):
    print(f"Progress: {progress}% - {message}")

chloros.process(
    mode="parallel",
    progress_callback=show_progress,
    wait=True
)

print("Processing complete!")
```

***

### Ví dụ 3: Xử lý hàng loạt nhiều thư mục

Xử lý nhiều tập dữ liệu chuyến bay:

```python
from chloros_sdk import ChlorosLocal
from pathlib import Path

# Initialize SDK once
chloros = ChlorosLocal()

# List of flight folders
flights = [
    "C:\\Datasets\\Flight_001",
    "C:\\Datasets\\Flight_002",
    "C:\\Datasets\\Flight_003"
]

for flight_path in flights:
    flight_name = Path(flight_path).name
    print(f"\n{'='*60}")
    print(f"Processing: {flight_name}")
    print('='*60)
    
    try:
        # Create project
        chloros.create_project(flight_name, camera="Survey3N_RGN")
        
        # Import images
        chloros.import_images(flight_path)
        
        # Configure
        chloros.configure(
            vignette_correction=True,
            reflectance_calibration=True,
            indices=["NDVI", "NDRE", "GNDVI"]
        )
        
        # Process
        chloros.process(mode="parallel", wait=True)
        
        print(f"✓ {flight_name} completed successfully")
    
    except Exception as e:
        print(f"✗ {flight_name} failed: {e}")

print("\n" + "="*60)
print("All flights processed!")
```

***

### Ví dụ 4: Tích hợp quy trình nghiên cứu

Tích hợp Chloros với phân tích dữ liệu:

```python
from chloros_sdk import ChlorosLocal
import pandas as pd
import matplotlib.pyplot as plt

# Initialize Chloros
chloros = ChlorosLocal()

# Field survey data
surveys = [
    {"name": "Plot_A", "folder": "C:\\Research\\PlotA", "biomass": 4500},
    {"name": "Plot_B", "folder": "C:\\Research\\PlotB", "biomass": 3800},
    {"name": "Plot_C", "folder": "C:\\Research\\PlotC", "biomass": 5200}
]

results = []

for survey in surveys:
    # Process with Chloros
    chloros.create_project(survey['name'])
    chloros.import_images(survey['folder'])
    chloros.configure(indices=["NDVI", "NDRE"])
    chloros.process(mode="parallel", wait=True)
    
    # Get results
    config = chloros.get_config()
    
    # Extract NDVI values (example - adjust based on your needs)
    # In real implementation, you would read the processed TIFF files
    
    results.append({
        'plot': survey['name'],
        'biomass': survey['biomass'],
        # Add your NDVI extraction here
    })

# Statistical analysis
df = pd.DataFrame(results)
print("\nResults:")
print(df)

# Create correlation plot
# plt.scatter(df['ndvi'], df['biomass'])
# plt.xlabel('NDVI')
# plt.ylabel('Biomass (kg/ha)')
# plt.title('NDVI vs Biomass Correlation')
# plt.show()
```

***

### Ví dụ 5: Giám sát tiến độ tùy chỉnh

Theo dõi tiến trình nâng cao bằng tính năng ghi nhật ký:

```python
from chloros_sdk import ChlorosLocal
from datetime import datetime
import logging

# Setup logging
logging.basicConfig(
    filename=f'processing_{datetime.now():%Y%m%d_%H%M%S}.log',
    level=logging.INFO,
    format='%(asctime)s - %(message)s'
)

# Progress callback with logging
def log_progress(progress, message):
    log_msg = f"[{progress}%] {message}"
    logging.info(log_msg)
    print(log_msg)

# Process with logging
chloros = ChlorosLocal()
chloros.create_project("LoggedProcess")
chloros.import_images("C:\\DroneImages")
chloros.configure(indices=["NDVI", "NDRE"])

logging.info("Starting processing...")
chloros.process(
    mode="parallel",
    progress_callback=log_progress,
    wait=True
)
logging.info("Processing complete!")
```

***

### Ví dụ 6: Xử lý lỗi

Xử lý lỗi mạnh mẽ khi sử dụng trong sản xuất:

```python
from chloros_sdk import ChlorosLocal
from chloros_sdk.exceptions import (
    ChlorosError,
    ChlorosBackendError,
    ChlorosLicenseError,
    ChlorosProcessingError
)

def process_safely(folder_path):
    """Process with comprehensive error handling"""
    try:
        with ChlorosLocal() as chloros:
            chloros.create_project("SafeProcess")
            chloros.import_images(folder_path)
            chloros.configure(indices=["NDVI"])
            chloros.process()
            
        return True, "Success"
    
    except ChlorosLicenseError as e:
        return False, f"License error: {e}. Upgrade to Chloros+ at cloud.mapir.camera/pricing"
    
    except ChlorosBackendError as e:
        return False, f"Backend error: {e}. Ensure Chloros is installed (Windows installer or Linux .deb package)."
    
    except ChlorosProcessingError as e:
        return False, f"Processing error: {e}"
    
    except FileNotFoundError as e:
        return False, f"Folder not found: {e}"
    
    except ChlorosError as e:
        return False, f"Chloros error: {e}"
    
    except Exception as e:
        return False, f"Unexpected error: {e}"

# Use the safe function
success, message = process_safely("C:\\DroneImages\\Flight001")
if success:
    print(f"✓ {message}")
else:
    print(f"✗ {message}")
```

***

### Ví dụ 7: Quản lý tài khoản và đăng xuất

Quản lý thông tin xác thực theo chương trình:

```python
from chloros_sdk import ChlorosLocal

def switch_account():
    """Clear credentials to switch to a different account"""
    try:
        chloros = ChlorosLocal()
        
        # Clear current credentials
        result = chloros.logout()
        print("✓ Credentials cleared successfully")
        print("Please log in with new account via Chloros, Chloros (Browser), or CLI")
        
        return True
    
    except Exception as e:
        print(f"✗ Logout failed: {e}")
        return False

def secure_cleanup():
    """Remove credentials for security purposes"""
    try:
        chloros = ChlorosLocal()
        chloros.logout()
        print("✓ Credentials removed for security")
        
    except Exception as e:
        print(f"Warning: Cleanup error: {e}")

# Switch accounts
if switch_account():
    print("\nRe-authenticate via Chloros GUI/CLI/Browser before next SDK use")

# Or perform secure cleanup
# secure_cleanup()
```

***

### Ví dụ 8: Công cụ dòng lệnh

Xây dựng công cụ CLI tùy chỉnh với SDK:

```python
#!/usr/bin/env python
"""
Custom Chloros CLI Tool
Process multiple folders from command line
"""

import sys
import argparse
from pathlib import Path
from chloros_sdk import process_folder

def main():
    parser = argparse.ArgumentParser(description='Custom Chloros Processor')
    parser.add_argument('folders', nargs='+', help='Folders to process')
    parser.add_argument('--indices', nargs='+', default=['NDVI'],
                       help='Indices to calculate (default: NDVI)')
    parser.add_argument('--camera', default=None,
                       help='Camera template')
    parser.add_argument('--format', default='TIFF (16-bit)',
                       help='Export format')
    parser.add_argument('--logout', action='store_true',
                       help='Clear cached credentials before processing')
    
    args = parser.parse_args()
    
    # Handle logout if requested
    if args.logout:
        from chloros_sdk import ChlorosLocal
        chloros = ChlorosLocal()
        chloros.logout()
        print("Credentials cleared. Please re-login via Chloros GUI/CLI/Browser.")
        return 0
    
    successful = []
    failed = []
    
    for folder in args.folders:
        folder_path = Path(folder)
        
        if not folder_path.exists():
            print(f"✗ Skipping {folder}: not found")
            failed.append(folder)
            continue
        
        print(f"\nProcessing: {folder_path.name}...")
        
        try:
            process_folder(
                folder_path,
                camera=args.camera,
                indices=args.indices,
                export_format=args.format
            )
            print(f"✓ {folder_path.name} complete")
            successful.append(folder)
        
        except Exception as e:
            print(f"✗ {folder_path.name} failed: {e}")
            failed.append(folder)
    
    # Summary
    print(f"\n{'='*60}")
    print(f"Summary: {len(successful)} successful, {len(failed)} failed")
    
    return 0 if not failed else 1

if __name__ == '__main__':
    sys.exit(main())
```

**Cách sử dụng:**

```bash
# Process multiple folders
python my_processor.py "C:\Flight001" "C:\Flight002" --indices NDVI NDRE GNDVI

# Clear cached credentials
python my_processor.py --logout
```

***

## Xử lý ngoại lệ

SDK cung cấp các lớp ngoại lệ cụ thể cho các loại lỗi khác nhau:

### Phân cấp ngoại lệ

```python
ChlorosError                    # Base exception
├── ChlorosBackendError        # Backend startup/connection issues
├── ChlorosLicenseError        # License validation issues
├── ChlorosConnectionError     # Network/connection failures
├── ChlorosProcessingError     # Image processing failures
├── ChlorosAuthenticationError # Authentication failures
└── ChlorosConfigurationError  # Configuration errors
```

### Ví dụ về ngoại lệ

```python
from chloros_sdk import ChlorosLocal
from chloros_sdk.exceptions import *

try:
    chloros = ChlorosLocal()
    chloros.process()

except ChlorosLicenseError:
    print("Chloros+ license required. Upgrade at cloud.mapir.camera/pricing")

except ChlorosBackendError:
    print("Backend failed to start. Ensure Chloros is installed (Windows installer or Linux .deb package).")

except ChlorosProcessingError as e:
    print(f"Processing failed: {e}")

except ChlorosError as e:
    print(f"General Chloros error: {e}")
```

***

## Chủ đề nâng cao

### Cấu hình phụ trợ tùy chỉnh

Sử dụng vị trí hoặc cấu hình phụ trợ tùy chỉnh:

```python
chloros = ChlorosLocal(
    backend_exe="C:\\Custom\\chloros-backend.exe",
    api_url="http://localhost:5001",  # Custom port
    timeout=60,                        # Longer timeout
    backend_startup_timeout=120        # 2 minutes startup
)
```

### Xử lý không chặn

Bắt đầu xử lý và tiếp tục với các tác vụ khác:

```python
# Start processing (non-blocking)
chloros.process(wait=False)

# Do other work here...
print("Processing started in background...")

# Check status later
import time
while True:
    status = chloros.get_config()
    if status.get('processing_complete'):
        break
    time.sleep(5)

print("Processing complete!")
```

### Quản lý bộ nhớ

Đối với các tập dữ liệu lớn, xử lý theo đợt:

```python
from pathlib import Path

base_folder = Path("C:\\LargeDataset")
batch_size = 100

# Get all image files
images = list(base_folder.glob("*.RAW"))

# Process in batches
for i in range(0, len(images), batch_size):
    batch = images[i:i+batch_size]
    batch_folder = base_folder / f"batch_{i//batch_size}"
    
    # Create batch folder and move images
    # ... (implementation details)
    
    # Process batch
    process_folder(batch_folder)
```

***

## Khắc phục sự cố

### Phần cuối không bắt đầu

**Vấn đề:** SDK không khởi động được chương trình phụ trợ**Giải pháp:**

1. Xác minh Chloros đã được cài đặt:

```python
import os
import platform

# Auto-detect backend path
if platform.system() == "Windows":
    backend_path = r"C:\Program Files\MAPIR\Chloros\resources\backend\chloros-backend.exe"
else:
    backend_path = "/usr/lib/chloros/chloros-backend"

print(f"Backend exists: {os.path.exists(backend_path)}")
```

2. Kiểm tra tường lửa (Windows) hoặc tính khả dụng của cổng (Linux: `lsof -i :5000`)
3. Thử đường dẫn phụ trợ thủ công:

```python
# Windows
chloros = ChlorosLocal(backend_exe="C:\\Path\\To\\chloros-backend.exe")

# Linux
chloros = ChlorosLocal(backend_exe="/opt/mapir/chloros/backend/chloros-backend")
```

***

### Giấy phép không được phát hiện**Vấn đề:** SDK cảnh báo về việc thiếu giấy phép**Giải pháp:**

1. Mở Chloros, Chloros (Trình duyệt) hoặc Chloros CLI và đăng nhập.
2. Xác minh giấy phép được lưu vào bộ nhớ đệm:

```python
from pathlib import Path
import os
import platform

# Check cache location
if platform.system() == "Windows":
    cache_path = Path(os.getenv('APPDATA')) / 'Chloros' / 'cache'
else:
    cache_path = Path.home() / '.cache' / 'chloros'

print(f"Cache exists: {cache_path.exists()}")
```

3. Nếu gặp vấn đề về thông tin xác thực, hãy xóa thông tin xác thực được lưu trong bộ nhớ đệm và đăng nhập lại:

```python
from chloros_sdk import ChlorosLocal

# Clear cached credentials
chloros = ChlorosLocal()
chloros.logout()

# Then login again via Chloros, Chloros (Browser), or Chloros CLI
```

4. Liên hệ hỗ trợ: info@mapir.camera

***

### Lỗi nhập**Vấn đề:** `ModuleNotFoundError: No module named 'chloros_sdk'`**Giải pháp:**

```bash
# Verify installation
pip show chloros-sdk

# Reinstall if needed
pip uninstall chloros-sdk
pip install chloros-sdk

# Check Python environment
python -c "import sys; print(sys.path)"
```

***

### Hết thời gian xử lý**Vấn đề:** Đã hết thời gian xử lý**Giải pháp:**

1. Tăng thời gian chờ:

```python
chloros = ChlorosLocal(timeout=120)  # 2 minutes
```

2. Xử lý các lô nhỏ hơn
3. Kiểm tra dung lượng đĩa trống
4. Giám sát tài nguyên hệ thống

***

### Cổng đã được sử dụng**Vấn đề:** Cổng phụ trợ 5000 đã bị chiếm dụng**Giải pháp:**

```python
# Use different port
chloros = ChlorosLocal(api_url="http://localhost:5001")
```

Hoặc tìm và đóng quá trình xung đột:

```powershell
# Windows PowerShell
Get-NetTCPConnection -LocalPort 5000
```

```bash
# Linux
lsof -i :5000
kill $(lsof -t -i :5000)
```

***

## Mẹo về hiệu suất

### Tối ưu hóa tốc độ xử lý

1. **Sử dụng Chế độ song song** (yêu cầu Chloros+)

```python
chloros.process(mode="parallel")  # Up to 16 workers
```

2. **Giảm độ phân giải đầu ra** (nếu chấp nhận được)

```python
chloros.configure(export_format="PNG (8-bit)")  # Faster than TIFF
```

3. **Vô hiệu hóa các chỉ số không cần thiết**

```python
# Only calculate needed indices
chloros.configure(indices=["NDVI"])  # Not all indices
```

4. **Xử lý trên SSD** (không phải HDD)***

### Tối ưu hóa bộ nhớ

Đối với tập dữ liệu lớn:

```python
# Process in batches instead of all at once
# See "Memory Management" in Advanced Topics
```

***

### Xử lý nền

Giải phóng Python cho các tác vụ khác:

```python
chloros.process(wait=False)  # Non-blocking

# Continue with other work
# ...
```

***

## Ví dụ về tích hợp

### Tích hợp Django

```python
# views.py
from django.http import JsonResponse
from chloros_sdk import process_folder

def process_images_view(request):
    if request.method == 'POST':
        folder_path = request.POST.get('folder_path')
        
        try:
            results = process_folder(folder_path)
            return JsonResponse({'success': True, 'results': results})
        except Exception as e:
            return JsonResponse({'success': False, 'error': str(e)})
```

### Bình API

```python
# app.py
from flask import Flask, request, jsonify
from chloros_sdk import process_folder

app = Flask(__name__)

@app.route('/api/process', methods=['POST'])
def process():
    data = request.get_json()
    folder_path = data.get('folder_path')
    
    try:
        results = process_folder(folder_path)
        return jsonify({'success': True, 'results': results})
    except Exception as e:
        return jsonify({'success': False, 'error': str(e)}), 500

if __name__ == '__main__':
    app.run()
```

### Sổ tay Jupyter

```python
# notebook.ipynb
from chloros_sdk import ChlorosLocal
import matplotlib.pyplot as plt

# Initialize
chloros = ChlorosLocal()

# Process
chloros.create_project("JupyterTest")
chloros.import_images("C:\\Data")
chloros.configure(indices=["NDVI"])

# Progress in notebook
from IPython.display import clear_output

def notebook_progress(progress, message):
    clear_output(wait=True)
    print(f"Progress: {progress}%")
    print(message)

chloros.process(progress_callback=notebook_progress)

# Visualize results
# ... (your visualization code)
```

***

##Câu hỏi thường gặp

### Hỏi: SDK có cần kết nối internet không?

**A:** Chỉ dành cho kích hoạt giấy phép ban đầu. Sau khi đăng nhập qua Chloros, Chloros (Trình duyệt) hoặc Chloros CLI, giấy phép được lưu vào bộ nhớ đệm cục bộ và hoạt động ngoại tuyến trong 30 ngày.***

### Hỏi: Tôi có thể sử dụng SDK trên máy chủ không có GUI không?**Đ:** Vâng! SDK hoạt động không đầu trên cả máy chủ Windows và Linux.**Linux (được khuyên dùng cho thiết bị không đầu):**
* Cài đặt qua gói `.deb`
* Kích hoạt giấy phép: `chloros-cli login user@example.com 'password'`

**Máy chủ Windows:**
* Máy chủ Windows 2016 trở lên
* Đã cài đặt Chloros (một lần)
* Giấy phép được kích hoạt qua CLI hoặc trên bất kỳ máy nào

***

### Hỏi: Sự khác biệt giữa Máy tính để bàn, CLI và SDK là gì?

| Tính năng | GUI trên máy tính để bàn | Dòng lệnh CLI | Python SDK |
| --------------- | ----------- | ---------------- | ----------- |
| **Giao diện** | Bấm chuột | Lệnh | Python API |
| **Tốt nhất cho** | Tác phẩm trực quan | Viết kịch bản | Tích hợp |
| **Tự động hóa** | Hạn chế | Tốt | Xuất sắc |
| **Tính linh hoạt** | Cơ bản | Tốt | Tối đa |
| **Giấy phép** | Chloros+ | Chloros+ | Chloros+ |***

### Hỏi: Tôi có thể phân phối các ứng dụng được xây dựng bằng SDK không?**A:** Mã SDK có thể được tích hợp vào ứng dụng của bạn, nhưng:

* Người dùng cuối cần cài đặt Chloros
* Người dùng cuối cần có giấy phép Chloros+ đang hoạt động
* Phân phối thương mại yêu cầu giấy phép OEM

Liên hệ info@mapir.camera để được giải đáp thắc mắc về OEM.

***

### Hỏi: Làm cách nào để cập nhật SDK?

```bash
pip install --upgrade chloros-sdk
```

***

### Hỏi: Hình ảnh đã xử lý được lưu ở đâu?

Theo mặc định, trong Đường dẫn dự án:

```

Project_Path/
└── MyProject/
    └── Survey3N_RGN/          # Processed outputs
```

***

### Hỏi: Tôi có thể xử lý hình ảnh từ tập lệnh Python chạy theo lịch trình không?**Đ:** Vâng! Sử dụng bộ lập lịch hệ điều hành của bạn với tập lệnh Python:

```python
# scheduled_processing.py
from chloros_sdk import process_folder

# Process today's flights
results = process_folder("/data/flights/today")  # Linux
# results = process_folder("C:\\Flights\\Today")  # Windows
```

**Windows:** Lập lịch thông qua Trình lập lịch tác vụ để chạy hàng ngày.**Linux:** Lên lịch qua cron:

```cron
# Run at 2 AM daily
0 2 * ** /usr/bin/python3 /home/user/scheduled_processing.py >> /var/log/chloros.log 2>&1
```

***

### Hỏi: SDK có hỗ trợ async/await không?**A:** Phiên bản hiện tại là đồng bộ. Đối với hành vi không đồng bộ, hãy sử dụng `wait=False` hoặc chạy trong chuỗi riêng biệt:

```python
import threading

def process_thread():
    chloros.process()

thread = threading.Thread(target=process_thread)
thread.start()

# Continue with other work...
```

***

### Hỏi: Làm cách nào để chuyển đổi giữa các tài khoản Chloros+ khác nhau?**A:** Sử dụng phương pháp `logout()` để xóa thông tin xác thực được lưu trong bộ nhớ đệm, sau đó đăng nhập lại bằng tài khoản mới:

```python
from chloros_sdk import ChlorosLocal

# Clear current credentials
chloros = ChlorosLocal()
chloros.logout()

# Re-login via Chloros, Chloros (Browser), or Chloros CLI with new account
```

Sau khi đăng xuất, hãy xác thực bằng tài khoản mới thông qua GUI, Trình duyệt hoặc CLI trước khi sử dụng lại SDK.

***

## Nhận trợ giúp

### Tài liệu

* **Tham khảo API**: Trang này

### Kênh hỗ trợ

* **Email**: info@mapir.camera
* **Trang web**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* **Giá**: [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing)

### Mã mẫu

Tất cả các ví dụ được liệt kê ở đây đều đã được thử nghiệm và sẵn sàng sản xuất. Sao chép và điều chỉnh chúng cho trường hợp sử dụng của bạn.

***

## Giấy phép**Phần mềm độc quyền** - Bản quyền (c) 2025 MAPIR Inc.

SDK yêu cầu đăng ký Chloros+ đang hoạt động. Việc sử dụng, phân phối hoặc sửa đổi trái phép đều bị cấm.