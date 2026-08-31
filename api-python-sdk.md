# API : Python SDK

{% hint style="info" %}
**Bạn đang tìm kiếm tài liệu tham khảo đầy đủ về API?** Trang này là một hướng dẫn thực hành. Mọi lớp công khai, phương thức, chữ ký chính xác và ví dụ có thể sao chép-dán đều có trong [SDK Tài liệu tham khảo](reference/sdk-reference.md), được tối ưu hóa cho các trợ lý AI.**Đang làm việc với trợ lý AI?** Hãy dán liên kết này URL vào khung chat để trợ lý có được tài liệu tham khảo đầy đủ và cập nhật nhất về Chloros 1.2.0 API:

`https://mapir.gitbook.io/chloros/reference/sdk-reference.md`

Mỗi trang của tài liệu hướng dẫn này đều có sẵn dưới dạng mã nguồn Markdown tại đường dẫn slug viết thường + `.md`, và toàn bộ tài liệu được lập chỉ mục tại `https://mapir.gitbook.io/chloros/llms.txt`.
{% endhint %}

**Chloros Python SDK** (`chloros-sdk` trên PyPI) điều khiển tất cả các chức năng mà ứng dụng máy tính để bàn có thể thực hiện từ Python: xử lý hình ảnh hàng loạt, điều khiển camera và mảng LATTICE trực tiếp, các phiên thu thập dữ liệu (DAQ) từ cảm biến ánh sáng, và tự động hóa dự án đã lưu. Đây là một lớp mỏng phủ lên cùng một hệ thống nền cục bộ mà giao diện người dùng (GUI) và CLI sử dụng (HTTP trên `127.0.0.1:5000`), do đó hành vi hoạt động giống hệt nhau trên cả ba nền tảng.

## Cài đặt

Quá trình cài đặt gồm hai bước: trước tiên là gói phần mềm máy tính để bàn Chloros (cung cấp nền tảng xử lý và các runtime phần cứng), sau đó là gói Python.

**Bước 1 — Cài đặt Chloros.** Windows: chạy trình cài đặt dành cho máy tính để bàn (đường dẫn mặc định là `C:\Program Files\MAPIR\Chloros\`) từ trang [Tải xuống](download.md). Linux: cài đặt gói `.deb` ([Linux Cài đặt](linux/linux-installation.md)).**Bước 2 — Cài đặt SDK** (Python 3.7+):

```bash
pip install chloros-sdk
```

Bạn có thể thậm chí không cần pip: mỗi trình cài đặt đều đi kèm với một gói SDK tương ứng. Trình cài đặt Windows sẽ tự động cài đặt nó vào thư mục hệ thống Python; trong khi Linux `.deb` sẽ đặt nó tại `/usr/lib/chloros/sdk/` và hiển thị lệnh chính xác là `pip install --user`. PyPI được cập nhật trên các bản dựng phát hành, do đó `pip install chloros-sdk` tương ứng với phiên bản ổn định mới nhất.

**Bước 3 — Đăng nhập một lần trên mỗi máy:**

```bash
chloros-cli login user@example.com 'YourPassword'
```

Thông tin đăng nhập được lưu trong bộ nhớ đệm tại `~/.chloros/` (cả hai nền tảng). Trên Windows, bạn có thể đăng nhập tương tự qua tab &quot;User&quot; <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> trong ứng dụng trên máy tính để bàn. SDK yêu cầu gói trả phí Chloros+ — xem [Yêu cầu về giấy phép](#license-requirement) bên dưới.

| Yêu cầu | Chi tiết |
| --- | --- |
| **Cài đặt Chloros** | Windows: trình cài đặt máy tính để bàn; Linux: gói `.deb` (cung cấp tệp nhị phân backend) |
| **Python** | Phiên bản 3.7 trở lên (được phát triển/kiểm thử trên phiên bản 3.10) |
| **Hệ điều hành** | Windows Windows 10/11 64-bit, Ubuntu 22.04 LTS trở lên, hoặc NVIDIA Jetson (JetPack 6) |
| **Giấy phép** | Tài khoản Chloros+ đang hoạt động, bất kỳ gói trả phí nào (Copper trở lên) |

## Thành công chỉ trong 60 giây

Chỉ với một lệnh, bạn có thể tạo dự án, nhập thư mục, cấu hình xử lý và chạy quy trình — tự động khởi động phần backend nếu phần này chưa được chạy:

```python
import chloros_sdk

results = chloros_sdk.process_folder(
    "C:/DroneImages/Flight001",
    indices=["NDVI", "NDRE", "GNDVI"],
)
```

(Trên Linux, sử dụng đường dẫLinux: `/home/user/drone_images/flight001`. Lệnh SDK hoạt động giống hệt nhau trên cả hai nền tảng.)

Đang xử lý thư mục ghi hình LATTICE? Sử dụng trình bao bọc tương thích với LATTICE — nó áp dụng các giá trị mặc định phù hợp (không phát hiện mục tiêu bảng điều khiển, thuật toán loại bỏ hiệu ứng Bayer tiêu chuẩn):

```python
results = chloros_sdk.process_lattice_capture(
    folder_path="C:/Captures/2026-05-13_Field",
    indices=["NDVI"],
)
```

## `ChlorosLocal` — kiểm soát toàn bộ quy trình xử lý

Đối với bất kỳ tác vụ nào phức tạp hơn một dòng lệnh, hãy sử dụng `ChlorosLocal`. Công cụ này sẽ khởi chạy phần lõi xử lý lần đầu tiên (`auto_start_backend=True`), tạo và cấu hình các dự án, theo dõi tiến trình và trả về bản tóm tắt sau khi hoàn thành.

```python
ChlorosLocal(
    api_url="http://127.0.0.1:5000",   # backend URL (also: backend_url=)
    auto_start_backend=True,            # spawn backend if not running
    backend_exe=None,                   # override backend binary path
    timeout=30,                         # request timeout seconds
    backend_startup_timeout=60,         # backend boot timeout
    processing_timeout=14400,           # hard cap on process() (4 h)
    processing_stuck_timeout=1800,      # no-progress threshold (30 min)
)
```

{% hint style="info" %}
Hãy giữ nguyên `http://127.0.0.1:5000` mặc định thay vì thay thế bằng `localhost` — trên Windows, `localhost` sẽ được giải quyết thành `::1` trước và tốn khoảng 2 giây cho mỗi yêu cầu khi truy cập vào máy chủ chỉ hỗ trợ IPv4.
{% endhint %}

Sử dụng nó như một trình quản lý ngữ cảnh để đảm bảo dọn dẹp:

```python
import chloros_sdk

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("FieldA_2026-05-26", camera="Survey3N_RGN")
    cl.import_images("C:/DroneImages/Flight001")
    cl.configure(
        vignette_correction=True,
        reflectance_calibration=True,
        indices=["NDVI", "NDRE", "GNDVI"],
        export_format="TIFF (16-bit)",
    )
    results = cl.process(mode="parallel", wait=True)
print(results["summary"])
```

`configure()` chấp nhận các từ khóa sau: `debayer`, `vignette_correction`, `reflectance_calibration`, `indices`, `export_format`, `ppk`, `daq_log_path`, `input_level`, `radiometric_output`, `array_alignment`, `array_alignment_crop`, `array_alignment_interpolation` và `custom_settings`. Các giá trị chính:

```python
# export_format
"TIFF (16-bit)"           # default, recommended
"TIFF (32-bit, Percent)"  # reflectance percentage as float32
"PNG (8-bit)"
"JPG (8-bit)"

# debayer
"High Quality (Faster)"                  # standard, default
"Texture Aware (Slow, Highest Quality)"  # neural debayer, Chloros+ only
```

Các thông số điều chỉnh dành riêng cho LATTICE (`input_level`, `radiometric_output`, và họ `array_alignment*`) được ghi chép kèm theo bảng giá trị đầy đủ trong [Tài liệu tham khảo SDK](reference/sdk-reference.md#supported-values).

### Theo dõi tiến trình

```python
def show_progress(percent, message):
    print(f"[{percent:3d}%] {message}")

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("FieldA")
    cl.import_images("C:/DroneImages/Flight001")
    cl.configure(indices=["NDVI"])
    cl.process(progress_callback=show_progress, poll_interval=1.0)
```

### Đọc bản tóm tắt sau khi chạy — và phát hiện các lần chạy trống

Khi hoàn tất, `process()` đính kèm bản tóm tắt xử lý của backend dưới dạng `result["summary"]`. Mỗi mục trong `summary["hints"]` là một câu hoàn chỉnh giải thích bất kỳ điều gì đáng chú ý — ví dụ: tại sao một lần chạy lại không tạo ra kết quả nào — và mọi gợi ý cũng được phát lại dưới dạng Python `UserWarning`, do đó các lần chạy trống sẽ tự chẩn đoán ngay cả khi bạn không bao giờ kiểm tra từ điển:

```python
result = cl.process()
for hint in result.get("summary", {}).get("hints", []):
    print("HINT:", hint)
# hints also arrive on the warnings channel:
#   python -W always::UserWarning your_script.py
```

{% hint style="warning" %}
**`process()` không được kích hoạt khi một lần chạy không tạo ra hình ảnh nào.** Đây là điểm duy nhất mà SDK và CLI được thiết kế khác biệt: `chloros-cli process` coi &quot;đã yêu cầu sản phẩm nhưng không có sản phẩm nào được ghi&quot; là lỗi và thoát với giá trị không bằng 0, trong khi SDK thoát bình thường và báo cáo tình trạng này thông qua `summary` / hints. Nếu quy trình của bạn cần dừng lại khi chạy trống, hãy tự kiểm tra — kiểm tra `summary` (hoặc đếm số tệp trong thư mục dự án) thay vì dựa vào ngoại lệ.
{% endhint %}

## Smart Connect — phần cứng trực tiếp

Ba trình trợ giúp mở các phiên làm việc liên tục trong nhóm phần cứng của backend — cùng nhóm mà GUI sử dụng, do đó các skript SDK có thể cùng tồn tại với ứng dụng trên máy tính để bàn mà không phải tranh giành cổng nối tiếp hay băng thông mạng. Cả ba đều tự động khởi động một backend cục bộ nếu chưa có backend nào đang chạy.

### Máy ảnh LATTICE đơn lẻ — `connect_camera`

```python
import chloros_sdk

# Open by serial; reuses existing pool entry if one exists
with chloros_sdk.connect_camera("213800234") as cam:
    cam.set_settings(exposure_time=10000, gain=0.0)   # microseconds, dB
    cam.capture("output/")
```

### Mảng đồng bộ — `connect_array`

`connect_array` là điểm khởi đầu được khuyến nghị cho các hệ thống nhiều camera. Nó chạy quy trình chuẩn bị thông minh giống như GUI: phân tích mạng, tự động chọn cấp độ đồng bộ, đồng bộ thời gian PTP, lựa chọn định dạng pixel cho từng camera, gieo hạt AE và kích hoạt kích hoạt GPIO. **Thiết bị nối tiếp đầu tiên là thiết bị chủ** (nó phát xung kích hoạt phần cứng); các thiết bị còn lại là thiết bị phụ.

```python
with chloros_sdk.connect_array(
        ["213800234", "214000533", "214701288", "214701292"]) as arr:
    arr.capture("output/", processing="reflectance")
```

Thêm `smart=True` vào bất kỳ quá trình chụp mảng nào để chờ cho chế độ phơi sáng tự động ổn định trên tất cả các camera trước khi kích hoạt. Đối với các chế độ chụp (Đơn / Liên tục / Khoảng thời gian / Nhanh nhất), bộ ghi, chụp liên tiếp thành video và căn chỉnh mảng, hãy xem [Tài liệu tham khảo SDK](reference/sdk-reference.md#synchronized-array--arraysession-smart-prep).

### Cảm biến ánh sáng DAQ — `connect_daq_sensor`

Nếu không có tham số nào, `connect_daq_sensor()` sẽ tự động phát hiện giao thức truyền tải (thứ tự ưu tiên: Ethernet → BLE → USB):

```python
with chloros_sdk.connect_daq_sensor() as daq:    # smart-detect USB / BLE / ETH
    for frame in daq.latest(n=5):
        print(frame["spectrum"][:10])
```

Mỗi khung dữ liệu chứa giá trị 135 điểm `spectrum` (W/m²/nm sau khi hiệu chuẩn), cờ `is_saturated` và CIE `x`, `y`, `z`. Để chỉ định cụ thể một cảm biến hoặc giao thức truyền tải — đây là lựa chọn đáng tin cậy trên các máy chủ có nhiều giao diện mạng, nơi tính năng tự động phát hiện Ethernet có thể bỏ sót một thiết bị DAQ-E đang hoạt động bình thường trong lần thử đầu tiên — hãy truyền một gợi ý rõ ràng:

```python
daq = chloros_sdk.connect_daq_sensor(transport="usb", port="COM3")
daq = chloros_sdk.connect_daq_sensor(mac="AA:BB:CC:DD:EE:FF")        # implies BLE
daq = chloros_sdk.connect_daq_sensor(eth_host="daq-e-xxx.local")     # implies Ethernet
```

Lưu ý rằng các cấu hình hiệu chỉnh giới hạn (`cap_id`) **không** phải là núm điều chỉnh &quot;SDK&quot; — hãy chọn chúng thông qua `chloros-cli daq pool-connect --cap-id …` / `pool-set-cap` thay thế.

### Các dự án đã lưu — `open_project`

Một dự án đã lưu Chloros sẽ duy trì phần cứng được kết nối (`cameras.json` + `sensors.json` cùng với `project.json`), và `chloros_sdk.open_project(path)` có thể kết nối lại tất cả cùng lúc và ghi lại dữ liệu theo tên thiết bị. Xem [Tự động hóa dự án](reference/sdk-reference.md#project-automation--chlorosproject) trong tài liệu tham khảo.

## Những gì bạn nhận được khi cài đặt chỉ qua pip

Kiểm tra các cờ khả dụng ở cấp mô-đun trước khi sử dụng các bề mặt phần cứng:

```python
import chloros_sdk
print(chloros_sdk.__version__)
print("CAMERA_AVAILABLE =", chloros_sdk.CAMERA_AVAILABLE)    # True iff lattice_sdk imported cleanly
print("DAQ_AVAILABLE    =", chloros_sdk.DAQ_AVAILABLE)       # True iff daq_sdk imported cleanly
print("PROJECT_AVAILABLE =", chloros_sdk.PROJECT_AVAILABLE)  # True iff ChlorosProject deps available
```

Trên một máy chủ **chỉ** có `pip install chloros-sdk` và không có gói desktop Chloros:

* `ChlorosLocal`, `process_folder` và `process_lattice_capture` **không** hoạt động — chúng cần tệp nhị phân nền tảng được cung cấp trong trình cài đặt máy tính để bàn.
* Các công cụ hỗ trợ kết nối thông minh (`connect_camera`, `connect_array`, `connect_daq_sensor`) là các khách hàng kết nối thông minh thuần túy (HTTP), do đó chúng hoạt động với một máy chủ (backend) trên một máy tính khác — nhưng các backend đi kèm chỉ kết nối với loopback, do đó bạn phải tự chuyển tiếp cổng (ví dụ: `ssh -N -L 5000:127.0.0.1:5000 user@chloros-host`) và truyền `backend_url="http://127.0.0.1:5000"` cùng với `auto_start_backend=False`. Xem [Chế độ máy chủ từ xa](reference/sdk-reference.md#remote-backend-mode-pip-only-host-via-tunnel).
* Các lớp LATTICE truy cập trực tiếp vào phần cứng (`LatticeCamera`, `CameraPool`, …) có thể được nhập, nhưng cần có môi trường chạy Arena SDK từ gói cài đặt trên máy tính để bàn — nếu thiếu môi trường này, `CAMERA_AVAILABLE` sẽ là `False`.
* `daq_sdk` (các lớp DAQ trực tiếp) được cung cấp kèm theo bản cài đặt trên máy tính để bàn, không phải trong gói PyPI, vì vậy `DAQ_AVAILABLE` sẽ là `False` trên máy chủ chỉ sử dụng pip — thay vào đó, hãy điều khiển các cảm biến DAQ thông qua `connect_daq_sensor()` với một backend (được kết nối qua đường hầm).

## Yêu cầu về giấy phép

Để truy cập SDK, bạn cần có tài khoản Chloros+ đang hoạt động ở bất kỳ gói trả phí nào — **Copper trở lên**(Copper / Bronze / Silver / Gold); gói miễn phí Iron không có quyền truy cập SDK / CLI. Việc thực thi được thực hiện**tại phía máy chủ**: mọi yêu cầu SDK phải đi kèm cả phiên hoạt động và gói trả phí, nếu không hệ thống phía sau sẽ trả về mã lỗi `403` / `PLAN_UPGRADE_REQUIRED` (được nâng lên thành `ChlorosLicenseError` bởi `ChlorosLocal`, và thành `ChlorosConnectError` bởi các hàm trợ giúp `connect_*`). Người gọi đã đăng xuất nhận được `401` / `AUTH_REQUIRED` (`ChlorosAuthenticationError`) — việc chạy lại `chloros-cli login` sẽ khắc phục trường hợp đầu tiên nhưng không khắc phục được trường hợp thứ hai.

Việc sử dụng ngoại tuyến hoạt động trong khoảng thời gian ân hạn của gói: cấp độ được đọc từ bộ nhớ đệm xác thực máy chủ (5 phút) hoặc bộ nhớ đệm giấy phép đã ký và gắn với máy (30 ngày đối với gói hàng tháng; đến khi hết hạn đăng ký đối với gói hàng năm). Khi thời gian ân hạn kết thúc, gói dịch vụ sẽ chuyển sang chế độ miễn phí và truy cập SDK sẽ bị tạm dừng cho đến khi máy tính kết nối với máy chủ ít nhất một lần. `chloros-cli status` vẫn có thể truy cập được ở cấp độ miễn phí nên lý do luôn hiển thị. Xem [Chloros+ Đăng nhập](chloros+-login.md).

## Các trường hợp ngoại lệ

Bắt lớp cơ sở để xử lý “bất kỳ sự cố nào xảy ra với Chloros”:

```python
import chloros_sdk

try:
    chloros_sdk.process_folder("/path/to/folder")
except chloros_sdk.ChlorosAuthenticationError:
    print("Run `chloros-cli login` first.")
except chloros_sdk.ChlorosLicenseError:
    print("Chloros+ subscription required.")
except chloros_sdk.ChlorosError as e:
    print(f"Chloros error: {e}")
```

Tất cả các ngoại lệ trong quy trình (`ChlorosBackendError`, `ChlorosConnectionError`, `ChlorosLicenseError`, `ChlorosAuthenticationError`, `ChlorosConfigurationError`, `ChlorosProcessingError`) đều bắt nguồn từ `ChlorosError`. Một điểm cần lưu ý: `ChlorosConnectError` — chỉ được tạo ra bởi `connect_camera` / `connect_array` / `connect_daq_sensor` — có nguồn gốc từ `Exception` thông thường, **không** từ `ChlorosError`, do đó `except ChlorosError` sẽ không phát hiện được lỗi này. Cấu trúc phân cấp đầy đủ có trong [SDK Tài liệu tham khảo](reference/sdk-reference.md#exceptions).

## Xem thêm

* [SDK Tài liệu tham khảo](reference/sdk-reference.md) — bề mặt API hoàn chỉnh, được tối ưu hóa cho các trợ lý AI.
* [CLI Tài liệu tham khảo](reference/cli-reference.md) — mỗi lệnh con CLI đều tương ứng với một lệnh SDK.
* [Tải xuống](download.md) — trình cài đặt cho Windows và Linux.
