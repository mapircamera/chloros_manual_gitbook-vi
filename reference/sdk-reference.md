# Chloros Python SDK Tham khảo

**Phiên bản:**

1.2.0**Ngày tạo:**29/07/2026 19:19 ·**Ngày cập nhật:** 30/08/2026**Gói:** `chloros-sdk` (PyPI)**Đối tượng:** Tối ưu hóa cho việc sử dụng bởi các mô hình ngôn ngữ lớn (LLM); dễ đọc cho con người.**Phạm vi:** Tất cả các lớp, hàm và hàm trợ giúp công khai được `import chloros_sdk` cung cấp, kèm theo các ví dụ có thể sao chép và dán liên quan đến xử lý hình ảnh, điều khiển camera đơn, mảng đồng bộ, cảm biến DAQ và tự động hóa dự án.

Nếu bạn chỉ cần xem những nội dung chính, hãy chuyển đến:
- [Cài đặt &amp; Bắt đầu nhanh](#installation)
- [Smart-Connect cho mảng camera LATTICE](#smart-connect-for-lattice-cameras)
- [Phiên cảm biến DAQ](#daq-sensor-sessions)
- [Tự động hóa dự án](#project-automation--chlorosproject)
- [Smart-AE / Smart-Capture](#smart-ae--smart-capture)

---

## Kiến trúc trong 60 giây

SDK là một lớp ứng dụng (Python) mỏng được xây dựng trên nền tảng máy chủ Flask (Chloros) (cùng máy chủ Flask mà giao diện người dùng trên máy tính để bàn và CLI sử dụng). Để tự động hóa, bạn chỉ cần nhập `chloros_sdk` và gọi các phương thức cấp cao; bên trong, mỗi lệnh gọi sẽ được chuyển thành một yêu cầu HTTP đến backend cục bộ trên cổng 5000 — `http://127.0.0.1:5000/api/...` (cố ý không dùng `localhost`, vì nó sẽ giải quyết thành `::1` trước trên Windows và tốn khoảng 2 giây cho mỗi yêu cầu khi kết nối với máy chủ nền chỉ hỗ trợ IPv4). Máy chủ nền quản lý nhóm phần cứng — camera, cảm biến DAQ, hồ sơ căn chỉnh, bộ đệm khung hình — do đó các tập lệnh SDK có thể cùng tồn tại với GUI mà không cần tranh giành cổng nối tiếp hay băng thông NIC.

Có ba giao diện mà bạnsẽ sử dụng:

1. **`ChlorosLocal` + các hàm miễn phí** (`process_folder`, `process_lattice_capture`) — Quy trình xử lý hình ảnh. Chạy toàn bộ thư mục qua các bước hiệu chuẩn / loại bỏ hiệu ứng Bayer / xuất chỉ mục chỉ với một lệnh gọi Python.
2. **Các đối tượng kết nối thông minh** (`connect_camera`, `connect_array`, `connect_daq_sensor`) — Mở một phiên làm việc nền liên tục cho phần cứng trực tiếp. Quy trình “smart-prep” như trên GUI: kiểm tra mạng, tự động chọn tầng, PTP, khởi tạo AE, cấu hình kích hoạt GPIO.
3. **`ChlorosProject` / `open_project`** — Tải một dự án đã lưu (thư mục chứa `cameras.json` + `sensors.json` + `project.json`), kết nối mọi thứ cùng lúc và điều khiển việc ghi dữ liệu bằng các handle được đặt tên.

Các bề mặt 1 và 2 **tự động khởi động một backend cục bộ** nếu chưa có backend nào đang lắng nghe (cùng một tệp nhị phân đi kèm mà GUI/CLI khởi tạo) — do đó, một tập lệnh đơn giản có thể chạy trực tiếp từ một cửa sổ terminal mới mà không cần bạn khởi động backend trước. Truyền tham số `auto_start_backend=False` để tắt tính năng này (ví dụ: khi kết nối với một backend từ xa, vốn không bao giờ được khởi tạo). Xem [Tự động khởi động backend](#backend-auto-start). Surface 3 hoạt động khác biệt: `open_project()` không nhận tham số `auto_start_backend`, và `connect_all()` không bao giờ khởi chạy backend — nó chỉ kiểm tra `http://127.0.0.1:5000` một lần và, nếu không có phản hồi, nó sẽ tự động chuyển sang chế độ điều khiển thiết bị trực tiếp (không qua backend) `lattice_sdk`. Chỉ có `proj.process()` và `stream(..., overlays=True)` mới khởi tạo lười biếng một `ChlorosLocal()` (có tính năng tự khởi động).

Cả ba đều yêu cầu xác thực: chạy `chloros-cli login` một lần trên máy, hoặc đăng nhập qua giao diện người dùng trên máy tính để bàn. Các cuộc gọi SDK mà không có phiên làm việc hợp lệ sẽ gây ra lỗi `ChlorosAuthenticationError`.

Yêu cầu:
- Python 3.7+ (theo thông báo của gói phần mềm; được phát triển/kiểm thử trên phiên bản 3.10)
- Chloros Desktop được cài đặt cục bộ (tệp nhị phân backend được tích hợp sẵn trong trình cài đặt)
- Tài khoản Chloros+ đang hoạt động. Mức tối thiểu cho SDK / CLI là gói **Copper**trở lên (Copper / Bronze / Silver / Gold); gói miễn phí**Iron**không có quyền truy cập vào SDK / CLI. Quy định này được áp dụng**từ phía máy chủ**: mọi yêu cầu có cờ SDK / CLI được gắn cờ phải đi kèm cả phiên trực tiếp và gói trả phí, nếu không hệ thống phía sau sẽ trả về mã lỗi `403` cùng với `error_code: PLAN_UPGRADE_REQUIRED` (được hiển thị dưới dạng `ChlorosLicenseError` bởi `ChlorosLocal`, và dưới dạng `ChlorosConnectError` bởi các trình trợ giúp `connect_*`). Người gọi đã đăng xuất sẽ nhận được `401` / `AUTH_REQUIRED` (`ChlorosAuthenticationError`) thay vào đó — hai mã này khác nhau vì- `chloros-cli login` đang chạy sẽ khắc phục lỗi đầu tiên nhưng không thể khắc phục lỗi thứ hai.
- Việc sử dụng ngoại tuyến được hỗ trợ trong thời gian ân hạn của gói: cấp độ dịch vụ được đọc từ bộ nhớ đệm xác thực máy chủ (5 phút) hoặc bộ nhớ đệm giấy phép đã ký và gắn với máy (30 ngày đối với gói hàng tháng, đến khi hết hạn đăng ký đối với gói hàng năm). Khi thời gian ân hạn đó hết hiệu lực, gói dịch vụ sẽ chuyển sang chế độ miễn phí và quyền truy cập SDK / CLI sẽ bị ngừng cho đến khi máy có thể kết nối với máy chủ ít nhất một lần. `chloros-cli status` (`GET /api/license-status`) vẫn có thể truy cập được ở gói miễn phí nên lý do rất rõ ràng — đây là tuyến đường duy nhất SDK / CLI được miễn trừ khỏi giới hạn gói.
- Windows 10/11 64-bit, **Ubuntu 22.04 LTS hoặc mới hơn**, hoặc Jetson (JetPack 6). Ubuntu 20.04**không** được hỗ trợ: các phụ thuộc của `.deb` được dẫn xuất từ những gì mà backend liên kết, bao gồm `libc6 (>= 2.34)`, và Focal phân phối glibc 2.31.

---

## Cài đặt

Python SDK là một lớp Python mỏng phủ lên backend Chloros. Đối với mọi tác vụ ngoài một vài quy trình làm việc chỉ liên quan đến DAQ, bạn cần **cài đặt gói máy tính để bàn Chloros tại địa phương** (trình cài đặt Windows hoặc Linux `.deb`) — đây chính là gói cung cấp tệp nhị phân nền tảng, môi trường chạy Arena SDK cho camera LATTICE và các gói hiệu chuẩn.

Tải xuống mới nhất: [`https://mapir.gitbook.io/chloros/download`](https://mapir.gitbook.io/chloros/download)

### Bước 1 — Cài đặt gói nền tảng Chloros

#### Windows (.exe)

1. Tải xuống `Chloros-Setup-x.y.z.exe` từ trang tải xuống.
2. Chạy trình cài đặt và làm theo hướng dẫn của trình hướng dẫn. Đường dẫn cài đặt mặc định là `C:\Program Files\MAPIR\Chloros\`.
3. Khởi chạy Chloros ít nhất một lần và đăng nhập bằng tài khoản Chloros+ của bạn.

#### Linux amd64 (.deb)

```bash
sudo dpkg -i chloros-amd64.deb
sudo apt-get install -f         # only if dpkg reports missing dependencies
chloros-cli --version
chloros-cli login user@example.com 'YourPassword'
```

#### Linux arm64 — Jetson (JetPack 6)

```bash
sudo dpkg -i chloros-arm64-jp6.deb
sudo apt-get install -f
chloros-cli --version
chloros-cli login user@example.com 'YourPassword'
```

### Bước 2 — Cài đặt gói `Python` SDK

**Trình cài đặt `Chloros` đi kèm với gói `SDK` tương ứng.** Mỗi trình cài đặt `Windows` và gói `Linux` .deb đều đặt tệp ``chloros_sdk-X.Y.Z-py3-none-any.whl`` trên đĩa, hoàn toàn khớp với phiên bản GUI / `CLI` / backend. Bạn không cần phải theo dõi PyPI để đảm bảo sự đồng bộ.

#### Windows

Trình cài đặt sẽ tự động chạy tệp `pip install` với tệp wheel đi kèm bằng cách sử dụng trình khởi chạy Python trên hệ thống của bạn (ưu tiên trình khởi chạy `py.exe`, nếu không có sẽ chuyển sang `python -m pip`). Không cần thực hiện thêm thao tác nào — `import chloros_sdk` hoạt động trong môi trường Python của bạn sau khi cài đặt thành công. Nếu không có Python trên hệ thống, trình cài đặt sẽ tự động bỏ qua bước này và giao diện người dùng (GUI) cùng CLI vẫn tiếp tục hoạt động.

#### Linux (.deb)

Gói .deb sẽ đặt tệp wheel vào thư mục `/usr/lib/chloros/sdk/`. Tệp `postinst` hiển thị lệnh chính xác — Các bản phân phối tuân thủ PEP 668 từ chối ghi vào biến môi trường pip toàn cục theo mặc định, do đó chúng tôi không tự động cài đặt:

```bash
pip install --user /usr/lib/chloros/sdk/chloros_sdk-*.whl
```

Đối với các triển khai Jetson không kết nối mạng (air-gapped), quá trình này hoàn toàn diễn ra ngoại tuyến — tệp wheel đã có sẵn trên đĩa.

#### PyPI công khai

Đối với các máy chủ chỉ sử dụng pip (không cài đặt gói desktop Chloros; các quy trình làm việc chỉ sử dụng remote-backend hoặc DAQ):

```bash
pip install chloros-sdk
```

PyPI được cập nhật theo các bản dựng trình cài đặt phiên bản phát hành, do đó gói wheel được đăng tải sẽ khớp với phiên bản ổn định mới nhất. Các bản dựng phát triển (ví dụ: `1.1.4.dev1`) chỉ được phân phối thông qua gói wheel đi kèm trong trình cài đặt.

#### Xác minh

```python
import chloros_sdk
print(chloros_sdk.__version__)
print("CAMERA_AVAILABLE =", chloros_sdk.CAMERA_AVAILABLE)
print("DAQ_AVAILABLE    =", chloros_sdk.DAQ_AVAILABLE)
print("PROJECT_AVAILABLE =", chloros_sdk.PROJECT_AVAILABLE)
```

> **Yêu cầu đăng ký Chloros+.** Tất cả các lệnh SDK đều yêu cầu tài khoản Chloros+ đang hoạt động. Chạy `chloros-cli login user@example.com 'YourPassword'` một lần trên mỗi máy; thông tin đăng nhập sẽ được lưu vào bộ nhớ đệm trong `~/.chloros/`.

### Tôi có cần Gói Desktop không?

Chỉ riêng gói pip **không** đủ cho hầu hết các quy trình làm việc. Dưới đây là những gì mỗi bề mặt SDK cần:

| Bề mặt SDK | Có cần Gói Desktop không? | Tại sao |
| --- | --- | --- |
| `ChlorosLocal`, `process_folder`, `process_lattice_capture` | **Có** | Tự động khởi động tệp nhị phân backend tại `/usr/lib/chloros/chloros-backend` (Linux) hoặc `C:\Program Files\MAPIR\Chloros\…` (Windows). |
| `connect_camera`, `connect_array`, `connect_daq_sensor`, `analyze_array_network`, `list_*`, `discover_*` | **Có**(cục bộ)**/ Không**(từ xa) | Các client thuần túy HTTP qua backend. Backend cục bộ → yêu cầu gói phần mềm desktop. Backend từ xa → `backend_url=`**qua đường hầm** (xem Chế độ Backend Từ Xa — các backend được cung cấp sẵn chỉ kết nối với loopback). |
| `ChlorosProject` / `open_project` | **Có** | Truyền các dự án đã lưu qua backend. |
| Các lớp LATTICE trực tiếp (`LatticeCamera`, `CameraPool`, `Calibration`, `DLS`, …) | **Có** | Cần có môi trường chạy bản địa Arena SDK được cung cấp trong gói phần mềm dành cho máy tính để bàn. Nếu không, `CAMERA_AVAILABLE` sẽ là `False` khi nhập. |
| Các lớp DAQ trực tiếp (`DAQUSensor`, `DAQMSensor`, `DAQESensor`, `SensorFleet`, `discover_all`) | **Không** | Giao tiếp thu thập dữ liệu thuần túy (Python) qua pyserial/bleak/zeroconf. Một môi trường chỉ sử dụng pip có thể điều khiển các thiết bị thu thập dữ liệu (DAQ) từ đầu đến cuối. |

### Chế độ Backend Từ Xa (máy chủ chỉ sử dụng pip, qua đường hầm)

> **Backend được cung cấp không thể truy cập qua mạng LAN.** Các bản dựng
> sản xuất chỉ liên kết với loopback (cả hai họ loopback) và từ chối hoàn toàn
> chế độ không loopback duy nhất (`CHLOROS_CLOUD_MODE`), do đó
> `backend_url="http://<lan-ip>:5000"` **không thể hoạt động với một
> Chloros** — mẫu này chỉ từng hoạt động với backend nguồn/dev
> . Để điều khiển một backend trên máy khác, hãy tự chuyển tiếp cổng loopback
> của nó và chỉ định SDK trỏ đến đường hầm:

```bash
# on the pip-only host: forward local 5000 to the Chloros machine's loopback
ssh -N -L 5000:127.0.0.1:5000 user@chloros-host
```

```python
import chloros_sdk

BACKEND = "http://127.0.0.1:5000"   # the tunnel endpoint

chloros_sdk.connect_camera("213800234", backend_url=BACKEND)
chloros_sdk.connect_array(serials, backend_url=BACKEND)
chloros_sdk.connect_daq_sensor(eth_host="daq-e-1.local", backend_url=BACKEND)
```

Các máy chủ không màn hình / CI / robot có thể giữ lại một máy tính với bản cài đặt desktop đầy đủ làm &quot;máy chủ Chloros&quot; và sử dụng `pip install chloros-sdk` ở mọi nơi khác — nhưng kết nối giữa chúng là đường hầm do người dùng thiết lập ở trên, chứ không phải là kết nối trực tiếp qua mạng LAN (URL).

> **Hạn chế đã biết — `ChlorosLocal` không chỉ hỗ trợ pip.** `ChlorosLocal(backend_url=BACKEND)` hiện giải quyết tệp nhị phân backend cục bộ trong hàm tạo của nó *trước khi* kiểm tra URL, và báo lỗi `ChlorosBackendError` (&quot;Chloros backend not found…&quot;) khi không có gói máy tính để bàn nào được cài đặt — ngay cả khi có backend từ xa có thể truy cập được. Chỉ các giao diện kết nối thông minh ở trên (`connect_camera` / `connect_array` / `connect_daq_sensor`, cùng với `analyze_array_network` và các hàm trợ giúp `list_*` / `discover_*`) mới hoạt động trên máy chủ chỉ sử dụng pip.

### Quy trình làm việc chỉ dành cho DAQ (máy chủ chỉ sử dụng pip)

Nếu bạn chỉ cần các cảm biến DAQ và không sử dụng camera LATTICE hay xử lý hình ảnh, gói pip đã đủ đầy đủ:

```bash
pip install chloros-sdk
```

```python
from chloros_sdk import DAQUSensor, DAQMSensor, DAQESensor, discover_all

for d in discover_all(timeout=3.0):
    print(d.model, d.display, d.address)   # USB serials: d.extra.get("serial_number")

sensor = DAQUSensor(port="/dev/ttyUSB0")
sensor.connect()
sensor.start_streaming()
```

Không cần backend, không cần tệp .deb, cũng không cần đăng nhập qua Chloros+ để thực hiện công việc DAQ trực tiếp trên phần cứng.

---

## Bắt đầu nhanh

```python
import chloros_sdk

# === Image processing ===
results = chloros_sdk.process_folder(
    "C:/DroneImages/Flight001",
    indices=["NDVI", "NDRE", "GNDVI"],
)

# === Live LATTICE single-cam ===
with chloros_sdk.connect_camera("213800234") as cam:
    cam.set_settings(exposure_time=10000, gain=0.0)
    cam.capture("output/")

# === Live LATTICE synchronized array (GUI smart-prep flow) ===
with chloros_sdk.connect_array(
        ["213800234", "214000533", "214701288", "214701292"]) as arr:
    arr.capture("output/", processing="reflectance")

# === Live DAQ spectral sensor ===
with chloros_sdk.connect_daq_sensor() as daq:    # smart-detect USB / BLE / ETH
    for frame in daq.latest(n=5):
        print(frame["spectrum"][:10])

# === Drive a saved project end-to-end ===
proj = chloros_sdk.open_project("/path/to/project")
proj.connect_all()
proj.arrays["main_rig"].capture("output/", processing="reflectance")
proj.disconnect_all()
```

---

## Chỉ mục cấp cao nhất API

```python
import chloros_sdk

# === Image processing (full pipeline) ===
chloros_sdk.ChlorosLocal                          # class
chloros_sdk.process_folder(...)                   # one-shot helper
chloros_sdk.process_lattice_capture(...)          # LATTICE-friendly defaults
chloros_sdk.read_image_audit_tags(path)           # post-run audit

# === Live cameras (persistent backend pool) ===
chloros_sdk.connect_camera(serial, ...)           # → CameraSession
chloros_sdk.connect_array(serials, ...)           # → ArraySession (smart-prep)
chloros_sdk.attach_array(serials_or_id, ...)      # → ArraySession (attach without re-connecting)
chloros_sdk.list_cameras()
chloros_sdk.list_arrays()
chloros_sdk.discover_lattice_cameras()
chloros_sdk.analyze_array_network(...)            # network capability + recommendation
chloros_sdk.CaptureResult                         # list subclass returned by ArraySession.capture
chloros_sdk.RecorderHandle                        # handle for an array record()/burst() job

# === Live DAQ sensors (persistent backend pool) ===
chloros_sdk.connect_daq_sensor(...)               # → DAQSensorSession
chloros_sdk.discover_daq_sensors()                # scan USB/BLE/ETH (finds a DAQ-M MAC)
chloros_sdk.list_daq_sensors()

# === Project lifecycle ===
chloros_sdk.open_project(path)                    # → ChlorosProject
chloros_sdk.ChlorosProject                        # class
chloros_sdk.AlignmentSpec                         # dataclass
chloros_sdk.ArrayHandle, CameraHandle, SensorHandle

# === Direct-hardware (no-backend) classes (from lattice_sdk / daq_sdk) ===
chloros_sdk.LatticeCamera, CameraSettings, PRESETS, CameraPool
chloros_sdk.Calibration, CalibrationCoefficients, FilterModel, list_filters
chloros_sdk.DLS, NetworkDiagnostics
chloros_sdk.DAQUSensor, DAQMSensor, DAQESensor, SensorFleet, discover_all

# === Exceptions ===
chloros_sdk.ChlorosError                          # base
chloros_sdk.ChlorosBackendError
chloros_sdk.ChlorosLicenseError
chloros_sdk.ChlorosConnectionError
chloros_sdk.ChlorosProcessingError
chloros_sdk.ChlorosAuthenticationError
chloros_sdk.ChlorosConfigurationError
chloros_sdk.ChlorosConnectError                   # raised by smart-connect surface
chloros_sdk.LatticeError, CameraNotFoundError, ...  # from lattice_sdk

# === Availability flags ===
chloros_sdk.CAMERA_AVAILABLE     # True iff lattice_sdk imported cleanly
chloros_sdk.DAQ_AVAILABLE        # True iff daq_sdk imported cleanly
chloros_sdk.PROJECT_AVAILABLE    # True iff ChlorosProject deps available
```

---

## Xử lý hình ảnh — `ChlorosLocal`

Lớp đường ống chính. Khởi chạy backend khi sử dụng lần đầu, tạo/cấu hình dự án, theo dõi tiến trình và trả về bản tóm tắt sau khi chạy.

### Hàm tạo

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

### Phương thức

| Phương thức | Mô tả |
| --- | --- |
| `create_project(project_name, camera=None)` | Tạo một dự án mới (tùy chọn kèm theo mẫu camera như `"Survey3N_RGN"`). |
| `import_images(folder_path, recursive=False)` | Nhập hình ảnh RAW/TIF/JPG/DNG **và các bản ghi từ cảm biến ánh sáng `.daq`**. Trả về `count` (hình ảnh) và `scan_count` (bản ghi). Chỉ cảnh báo nếu thư mục không chứa cả hai. |
| `export_light_sensor(daq=True, csv=True)` | Ghi các tệp đã hiệu chuẩn `.daq` + `.csv` cho mỗi bản ghi cảm biến ánh sáng trong dự án, vào `<project>/Light Sensor/`. Xem [Bản ghi cảm biến ánh sáng](#light-sensor-recordings--calibrated-daq--csv). |
| `configure(debayer=..., vignette_correction=..., reflectance_calibration=..., indices=[...], export_format=..., ppk=..., daq_log_path=..., input_level=..., radiometric_output=..., array_alignment=..., array_alignment_crop=..., array_alignment_interpolation=..., custom_settings=None)` | Đặt các thông số xử lý. |
| `process(mode="parallel", wait=True, progress_callback=None, poll_interval=2.0)` | Chạy quy trình xử lý. Trả về `{"status": "complete", "async": False}`, kèm theo khóa `summary` nếu hệ thống nền cung cấp — xem [Tóm tắt sau khi chạy &amp; Gợi ý](#post-run-summary--hints). |
| `get_config()` / `get_status()` / `status()` | Kiểm tra trạng thái hệ thống nền. |
| `logout()` | Xóa thông tin đăng nhập đã lưu trong bộ nhớ đệm. |
| `shutdown_backend()` | Kết thúc quá trình chạy của backend (nếu được khởi động bằng lệnh `SDK -start`). |
| `discover_cameras()` | Phát hiện các camera LATTICE **thông qua backend của phiên bản này** (`/api/camera/discover`). Trả về danh sách các từ điển (`serial`, `model`, `ip`, …) — có cấu trúc giống như những gì GUI/CLI hiển thị. Danh sách trống nếu không tìm thấy camera nào hoặc không thể kết nối với backend. |
| `camera_capture(output_dir, format="tiff", **settings)` | Ghi lại một khung hình duy nhất**qua backend**(được tự động khởi chạy bởi handle này) để nó được chuẩn bị giống như trong GUI/ CLI (mặc định 12-bit, tái sử dụng bộ nhớ đệm, metadata hiệu chỉnh nhúng). Xác định mục tiêu bằng `serial=` hoặc `device_index=`; chuyển `exposure`/`gain`/`pixel_format`/`preset` thành `**settings`. Trả về từ điển metadata cũ (`filepath`, `width`, `height`, `pixel_format`, `exposure_time`, `gain`, `timestamp`). |
| `camera_stream(serial, *, fps=10.0, overlay=None, decode=True, connect_timeout=10.0, read_timeout=15.0)` | Trả về các khung hình xem trước được ghép chồng- các khung hình xem trước được ghép từ camera chung — ứng dụng khách MJPEG nhẹ qua đường dẫn `/api/camera/<serial>/stream-annotated` của hệ thống nền (các hiệu ứng zebra / lưới / thập tự / biểu đồ histogram / peaking / điểm đánh dấu được vẽ phía máy chủ). `decode=True` trả về mảng BGR; `False` trả về dữ liệu thô JPEG. Cũng có thể truy cập theo từng dự án dưới dạng `ChlorosProject.stream(overlays=True)`. |

Sử dụng như một trình quản lý bối cảnh để đảm bảo dọn dẹp:

```python
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

### Ghi dữ liệu cảm biến ánh sáng — đã hiệu chuẩn `.daq` + `.csv`

Một thiết bị DAQ-U / DAQ-M / DAQ-E có thể được ghi lại **mà không cần** gói hiệu chuẩn của nó. Đó là
cách các trình ghi công khai [`chloros_scripts`](https://github.com/mapircamera/chloros_scripts) (`record_daq.py`) thực hiện theo mặc định: chúng ghi lại số đếm thô từ cảm biến và gắn dấu thời gian vào
tệp để Chloros lấy dữ liệu hiệu chuẩn nhà máy của cảm biến đó **theo số sê-ri** — trước tiên từ bộ nhớ đệm cục bộ,
sau đó từ đám mây MAPIR — và áp dụng khi nhập dữ liệu.

Chloros viết kết quả trở lại dưới dạng hai sản phẩm cho mỗi bản ghi, dưới
`<project>/Light Sensor/`:

| Sản phẩm | Mô tả |
| --- | --- |
| `<name>_calibrated.daq` | Kho lưu trữ có thể xử lý lại — cùng cấu trúc với bản ghi trực tiếp, giờ đây khai báo gói đã tạo ra nó. Việc nhập lại **không** không hiệu chuẩn lại lần thứ hai. |
| `<name>_calibrated.csv` | Cường độ bức xạ quang phổ tính bằng W/m²/nm trên lưới bước sóng riêng của cảm biến, mỗi hàng tương ứng với một giá trị đo, kèm theo các cột thông số quang học (công suất tổng, lux quang giác/tối giác, PPFD và bước sóng đỉnh màu xanh/xanh lá/đỏ, bước sóng đỉnh). |
| `<name>_raw.daq` / `<name>_raw.csv` | **Chỉ dành cho cảm biến không có gói (DAQ-A).** Số đếm quang phổ thô của cảm biến — *không* phải cường độ bức xạ. Xem bên dưới. |

`process()` thực hiện việc xuất dữ liệu này như một trong các giai đoạn của nó. Nó **không** yêu cầu hình ảnh:
một cảm biến ánh sáng được triển khai độc lập là một quy trình làm việc hoàn chỉnh, và dự án như vậy không có
hình ảnh nào theo thiết kế.

**Các bản ghi DAQ-A được xuất dưới dạng số đếm thô.** Dòng cảm biến DAQ-A ra đời trước hệ thống gói dữ liệu theo số sê-ri
và không có gói dữ liệu nào để lấy — thay vào đó, nó được hiệu chuẩn tại hiện trường so với một
mục tiêu phản xạ, đó là lý do tại sao nó không bao giờ cần đến gói dữ liệu. Các bản ghi này được xuất
dưới tiền tố `_raw` thay vì `_calibrated`: tên tệp khác thay vì một cờ
bên trong tệp, vì thông tin này phải được giữ nguyên khi được gửi qua email dưới dạng tên tệp thuần túy. Các
Tiêu đề `.csv` ghi là `raw spectral sensor counts (NOT irradiance)` và cảnh báo rằng các
giá trị này có thể so sánh được **trong phạm vi** tệp — chính xác là mục đích mà hiệu chuẩn dựa trên mục tiêu
sử dụng chúng — chứ không phải giữa các cảm biến. Các cột quang độ phụ thuộc vào công suất (công suất tổng,
lux quang giác/tối giác, PPFD) trả về giá trị **NULL** thay vì được tích phân từ số đếm.

Một thiết bị DAQ-U / DAQ-M / DAQ-E mà gói dữ liệu của nó không thể lấy được vẫn sẽ bị **bỏ qua**,
không được ghi dưới dạng thô: trong trường hợp đó, gói dữ liệu vẫn tồn tại và &quot;kết nối lại và xử lý lại&quot; là lời khuyên thực tế.

Các bản ghi cũ **v1.01 / v1.02** (thiết bị DAQ-A-SD ghi lại những bản ghi này) không chứa thời điểm cho từng lần đọc,
chỉ có thời gian ghi của tệp. Trình khớp hình ảnh↔ánh sáng chiếu xuống vẫn từ chối chúng — việc so khớp một
khung hình với thời gian ghi sẽ dẫn đến sai sót không thể nhận biết — nhưng trình xuất dữ liệu vẫn đọc được chúng, và
CSV hiển thị mã lỗi `clock=daq_created_on` để sản phẩm chỉ ra rằng nó đang chạy trên đồng hồ nào.

```python
import chloros_sdk

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("DAQ-U_2026-08-26")
    cl.import_images("C:/Flights/raw_daq")     # .daq only — no camera involved
    result = cl.export_light_sensor()          # or just cl.process()

for rec in result["exported"]:
    print(rec["csv"])
for rec in result["skipped"]:
    print("skipped", rec["source"], "--", rec["reason"])
```

Một bản ghi mà gói hiệu chuẩn không thể được lấy (chế độ ngoại tuyến, hoặc cảm biến không có
hiệu chuẩn trong tệp) sẽ được báo cáo dưới dạng `skipped` **kèm theo lý do**. Dữ liệu này sẽ không bao giờ
được ghi ra dưới dạng tệp “đã hiệu chuẩn” chứa số đếm thô — hãy kết nối với internet và
chạy lại, quá trình xuất dữ liệu sẽ hoàn tất.

### Gọi lại tiến trình

```python
def show_progress(percent, message):
    print(f"[{percent:3d}%] {message}")

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("FieldA")
    cl.import_images("C:/DroneImages/Flight001")
    cl.configure(indices=["NDVI"])
    cl.process(progress_callback=show_progress, poll_interval=1.0)
```

### Tóm tắt sau khi chạy &amp; Gợi ý

Khi hoàn tất, `process()` sẽ lấy `GET /api/processing-summary` và đính kèm nội dung dưới dạng `result["summary"]`. Việc lấy dữ liệu này được thực hiện theo nguyên tắc &quot;cố gắng hết sức&quot; và không bao giờ chặn kết quả trả về thành công — nếu bản tóm tắt không khả dụng, `process()` sẽ chuyển sang dạng `{"status": "complete", "async": False}` thuần túy. Mỗi mục trong `summary["hints"]` — các câu hoàn chỉnh kèm theo đề xuất khắc phục, ví dụ: lý do tại sao một lần chạy không tạo ra đầu ra nào — cũng được phát lại dưới dạng Python `UserWarning`, do đó các lần chạy đầu ra 0- các lần chạy không có đầu ra sẽ tự chẩn đoán ngay cả khi bạn không bao giờ kiểm tra từ điển:

```python
result = cl.process()
for hint in result.get("summary", {}).get("hints", []):
    print("HINT:", hint)
# hints also arrive on the warnings channel:
#   python -W always::UserWarning your_script.py
```

`summary["totals"]` là phần có thể đọc được bởi máy:

| Khóa | Điều nó đếm |
| --- | --- |
| `models` | Các nhóm camera trong lần chạy. |
| `images_in_groups` | Hình ảnh nguồn trong các nhóm đó. |
| `targets_found` | Các mục tiêu phản xạ được phát hiện. |
| `images_calibrated` | Các hình ảnh được hiệu chuẩn trong đợt chạy. |
| `exported_files` | **Các tệp sản phẩm hình ảnh mà đợt chạy đã tạo ra.** |
| `daq_recordings_exported` / `daq_recordings_skipped` | Dữ liệu ghi từ cảm biến ánh sáng, được tính riêng một cách có chủ đích — chúng đến từ một giai đoạn khác và tồn tại ngay cả trong các lần chạy không có hình ảnh nào, nên nếu gộp chúng vào sẽ khiến một lần chạy chỉ thu thập dữ liệu (DAQ) trông như thể đã xuất hình ảnh. |

Cùng với chúng: `summary["output_dirs"]` (mọi thư mục đã được ghi vào),
`summary["light_sensor_export"]`, `summary["stopped"]` (có giá trị true khi người dùng ngắt quá trình
quá trình chạy, do đó số đếm một phần sẽ không được coi là một lần chạy hoàn thành nhưng sản lượng thấp), và
`summary["groups"]` (phân tích theo từng nhóm).

`exported_files` được hệ thống ghi lại **trong quá trình ghi**, chứ không phải được quét từ
các đối tượng hình ảnh của dự án sau đó. Các chiến lược song song và GPU tự tạo ra các đối tượng hình ảnh
của riêng mình (trong các tiến trình con của worker đối với các đường dẫn GPU), do đó quá trình quét cũ đã báo cáo
`0 file(s) written` cho mọi lần chạy như vậy và sau đó phát ra gợi ý “zero-exports” — ngay cả trong các lần chạy
mà mọi thứ đều hoạt động bình thường. Nếu bạn viết kịch bản dựa trên con số này, một lần chạy song song hoạt động bình thường hiện nay
sẽ báo cáo số lượng không bằng không.

Các báo cáo bỏ qua do cảm biến ánh sáng sẽ nêu rõ lý do mà trình đọc thực sự xác định cho từng tệp — một
schema không thể đọc được, một gói thiếu, một lỗi ghi — **được loại trừ trùng lặp**, do đó, hai mươi tệp
bị bỏ qua vì một nguyên nhân sẽ được ghi nhận là một nguyên nhân duy nhất thay vì hai mươi lần lặp lại của nó.

> **`process()` không được kích hoạt khi một lần chạy không tạo ra hình ảnh nào.** Đây là điểm duy nhất mà SDK và
> CLI có sự khác biệt có chủ đích: `chloros-cli process` coi &quot;đã yêu cầu sản phẩm nhưng không có sản phẩm nào
> được ghi&quot; là lỗi và thoát với giá trị không bằng 0, trong khi SDK trả về kết quả bình thường và báo cáo
> tình trạng này thông qua các gợi ý của `summary` / gợi ý. Nếu quy trình xử lý của bạn dừng lại khi chạy trống, hãy tự kiểm tra
> — kiểm tra `summary` (hoặc đếm số tệp trong thư mục dự án) thay vì chỉ dựa vào
> việc không có ngoại lệ. Các nguyên nhân thường gặp là thư mục đầu vào không được nhận diện là
> vùng chụp và các sản phẩm bị bỏ qua do không áp dụng được cho các camera hiện có (ví dụ: độ sáng từ các camera chỉ hỗ trợ chế độ &quot;RGB&quot;
>).

### Các hàm tiện ích

```python
# One-call process: project + import + configure + process
results = chloros_sdk.process_folder(
    folder_path="C:/DroneImages/Flight001",
    project_name="FieldA_2026-05-26",
    camera="Survey3N_RGN",
    indices=["NDVI", "NDRE", "GNDVI"],
    vignette_correction=True,
    reflectance_calibration=True,
    export_format="TIFF (16-bit)",
    mode="parallel",
    debayer="High Quality (Faster)",      # or "Texture Aware (Slow, Highest Quality)"
    ppk=False,
    recursive=False,
    processing_timeout=14400,
)

# LATTICE-friendly defaults (no panel-target detection, standard debayer)
results = chloros_sdk.process_lattice_capture(
    folder_path="C:/Captures/2026-05-13_Field",
    indices=["NDVI"],
)

# Audit which calibration sources were applied to a processed image
tags = chloros_sdk.read_image_audit_tags("output/Reflectance_Calibrated/x.tif")
print(tags["CalibrationSource"])   # 'per_serial' / 'legacy_lookup' / 'none'
print(tags["VignetteSource"])      # 'per_serial' / 'legacy_polynomial' / 'none'
```

### Các giá trị được hỗ trợ

```python
# export_format
"TIFF (16-bit)"           # default, recommended
"TIFF (32-bit, Percent)"  # reflectance percentage as float32
"PNG (8-bit)"
"JPG (8-bit)"

# debayer
"High Quality (Faster)"               # standard, default
"Texture Aware (Slow, Highest Quality)"  # neural debayer, Chloros+ only
"Standard (Fast, Medium Quality)"      # alias used internally for LATTICE

# input_level (LATTICE only; Survey3 .raw ignores)
"auto"        # default — infers from each file's XMP ProcessingLevel tag
"raw"         # force-treat as raw Bayer
"debayered"   # force-treat as already-debayered BGR
"processed"   # force-treat as already-calibrated radiance

# array_alignment / array_alignment_crop (LATTICE arrays; None = keep saved setting)
True          # backend default — apply the module-to-module transform stamped
              # in each capture's Chloros:Alignment* XMP to every product
False         # export in native sensor geometry / skip the common-overlap crop

# array_alignment_interpolation (alignment warp resampling)
"bilinear"    # backend default
"nearest"     # preserves exact source DNs (no inter-pixel value mixing)
"cubic"
```

#### Đầu ra quang phổ (dòng xử lý đa quang phổ LATTICE)

Cấp độ xuất đa quang phổ LATTICE (M3C/M3M) của dòng xử lý `process` — `reflectance` (mặc định), `radiance`, `sensor-response` hoặc `all` (mỗi chế độ áp dụng cho từng hình ảnh) — tương ứng với cài đặt xử lý **&quot;Đầu ra phóng xạ&quot;** của dự án. `configure()` có từ khóa chuyên dụng cho nó:

```python
with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("Field_A")
    cl.import_images("C:/Captures/lattice_flight")
    cl.configure(
        radiometric_output="radiance",   # reflectance (default) / radiance / sensor-response / all
        export_format="TIFF (32-bit, Percent)",
    )
    cl.process()
```

Cách thoát khẩn cấp nâng cao — ghi khóa `"Radiometric output"` của dự án thông qua `custom_settings` — vẫn hoạt động, nhưng hãy nhớ rằng nó sẽ thay thế toàn bộ khối cài đặt (xem cảnh báo bên dưới):

```python
cl.configure(custom_settings={
    "Project Settings": {
        "Processing": {"Radiometric output": "radiance"},
        "Export": {"Calibrated image format": "TIFF (32-bit, Percent)"},
    }
})
```

`reflectance` (giá trị mặc định) chia độ rạng rỡ của camera cho **dữ liệu DAQ hướng xuống khớp với dấu thời gian**, được xác định tự động từ một tệp `.daq` (DAQ-U/M/E)**hoặc một giá trị `.csv` gốc của DAQ-M**được tìm thấy cùng với hình ảnh; bất kỳ gói hiệu chuẩn nào cho từng máy ảnh hoặc DAQ bị thiếu tại địa phương sẽ được**tự động lấy từ AWS** trong lần sử dụng đầu tiên. Giao diện dòng lệnh CLI hiển thị các tùy chọn này dưới dạng công tắc bật/tắt theo từng loại sản phẩm trên `chloros-cli process`: `--radiance`/`--no-radiance`, `--reflectance`/`--no-reflectance`, `--debayered`, `--preview`.

> `custom_settings` **thay thế** toàn bộ khối cài đặt được tính toán (nó bỏ qua các từ khóa khác và quá trình xác thực của `configure()` theo thiết kế). Khi sử dụng, hãy bao gồm mọi khóa `Project Settings` mà bạn quan tâm, như trong ví dụ trên.

---

## Smart-Connect cho camera LATTICE

Các phiên làm việc nền liên tục cho phần cứng trực tiếp. Sử dụng cùng các điểm cuối (endpoint) như giao diện người dùng (GUI), do đó hành vi hoạt động giống hệt nhau trên SDK / CLI / GUI.

### Máy ảnh đơn — `CameraSession`

```python
import chloros_sdk

# Open by serial; reuses existing pool entry if one exists
with chloros_sdk.connect_camera("213800234") as cam:
    # cam is a CameraSession; supports context manager + manual disconnect
    cam.set_settings(
        exposure_time=10000,    # microseconds
        gain=0.0,               # dB
        pixel_format="BayerRG12",
        target_brightness=80,
        ae_damping=8.0,
    )
    cam.capture("output/", ext=".tiff")
```

#### Chữ ký `connect_camera()`

```python
connect_camera(
    serial,
    *,
    preset=None,                       # "default" | "high_quality" | "high_speed" | "triggered"
    settings=None,                     # dict overlaid on the preset
    backend_url="http://127.0.0.1:5000",  # deliberately not 'localhost' (::1-first on Windows ≈ 2 s/request)
    timeout=60.0,
    auto_start_backend=True,           # spawn a local backend if none is running
) -> CameraSession
```

#### Các phương thức của `CameraSession`

| Phương thức | Mô tả |
| --- | --- |
| `read_nodes(names, enum_names=(), timeout=30.0)` | Đọc các nút GenICam; trả về `{nodes, errors, enums, device}`. |
| `set_settings(**kwargs)` | Ghi các nút theo tên thân thiện (`exposure_time`, `gain`, `pixel_format`, `width`, `height`, `target_brightness`, `ae_damping`, `ae_upper_limit`, `trigger_mode`, `trigger_source`, …). |
| `capture(output_dir="output", ext=".tiff", jpeg_quality=95, processing=None, levels=None, force_daq=None, settings=None, timeout=None)` | Chụp **một** khung hình. Trả về một danh sách gồm một phần tử chứa các từ điển siêu dữ liệu khung hình. (Chức năng chụp liên tiếp/nhiềuđã bị loại bỏ — hãy gọi `capture()` trong vòng lặp nếu bạn cần một chuỗi khung hình.) |
| `disconnect()` | Giải phóng khỏi bể chứa. Không thực hiện thao tác nào nếu đã kết nối với một phiên đang mở. |

Kiểm soát xuất dữ liệu `capture()` (cùng mô hình với mảng + GUI):

- `processing` / `levels` — `processing="all"` lưu mọi loại xuất áp dụng; `levels=["raw","radiance"]` chỉ lưu những loại (ghi đè lên `processing`). Bỏ qua cả hai để sử dụng mặc định của backend.
- `force_daq=True` — lưu giá trị đọc DAQ/DLS đã gán dưới dạng tệp phụ `.daq` ngay cả khi chỉ thu thập dữ liệu thô, để khung hình có thể được xử lý lại thành phản xạ/chỉ số sau này. Không thực hiện gì nếu không có DAQ nào được liên kết.

### Mảng đồng bộ — `ArraySession` (Smart-Prep)

`connect_array` là **điểm vào được khuyến nghị** cho các thiết lập nhiều camera. Tính năng này chạy toàn bộ quy trình Smart-Prep trên giao diện người dùng (GUI) ở chế độ nền:

1. **Phân tích mạng** (`/api/camera/array/recommend`) — tìm kích thước khung hình lớn nhất phù hợp với cấp độ sim-emit mà không làm mất khung hình.
2. **Chọn cấp độ tự động** — `sim-capture-sim-emit` nếu đường truyền có thể xử lý được; nếu không thì `sim-capture-ftd-stagger` hoặc `slip-emit-and-capture`.
3. **Tự động thu nhỏ**— âm thầm thu nhỏ kích thước khung / tăng tỷ lệ gộp khi đường truyền không thể duy trì độ phân giải được yêu cầu.**Biện pháp an toàn này không bao gồm tình trạng quá tải tổng hợp**: không thể khắc phục tình trạng có quá nhiều camera so với khả năng của đường truyền bằng cách thu nhỏ khung — xem [Quá tải](#over-subscription-the-per-cam-floor).
4. **PTP được bật**theo mặc định — các dấu thời gian giữa các camera được đồng bộ hóa trên một đồng hồ chung với độ chính xác**~1 ms**. Việc phơi sáng đồng thời đến từ tín hiệu kích hoạt phần cứng M8 (**&lt; 100 µs** giữa các mô-đun), không phải từ PTP: PTP đồng bộ hóa *dấu thời gian*, không phải thời điểm phơi sáng.
5. **Tự động chọn định dạng pixel cho từng camera** — camera RGB → `BayerRG8`, đa phổ → `BayerRG12`.
6. **Ghi nhớ giá trị AE** — ghi lại trạng thái AE hiện tại của từng camera để kết nối không làm reset thời gian phơi sáng giữa chừng.
7. **Cấu hình kích hoạt GPIO** — `connect_array` kích hoạt mọi camera (`TriggerMode=On`, `TriggerSource=Line2`) để xung từ camera chính điều khiển các camera phụ qua cáp M8. Đây là bước chỉ áp dụng cho mảng camera: một camera đơn lẻ được kích hoạt bằng `LatticeCamera` sẽ hoạt động tự do thay thế.

```python
import chloros_sdk

# First serial is the MASTER (fires the trigger pulse); rest are slaves.
with chloros_sdk.connect_array(
        ["213800234", "214000533", "214701288", "214701292"]) as arr:
    print(arr.array_id, arr.sync_mode, arr.ptp_enabled)
    arr.capture("output/", processing="reflectance")
```

#### Chữ ký `connect_array()`

```python
connect_array(
    serials,                              # list[str]; serials[0] = master
    *,
    line="Line2",                         # GPIO sync line: Line0 | Line2 | Line3
    target_fps=None,                      # master trigger fire rate (auto if None)
    force_tier=None,                      # override tier picker; see below
    wire_ceiling_mbps=None,               # host sustained wire budget, MB/s (auto if None)
    width=None,                           # explicit frame size; skips network analysis
    height=None,
    pixel_format=None,
    binning=None,
    recommend=True,                       # set False to skip the recommend step
    ptp_enable=True,                      # set False to disable PTP
    backend_url="http://127.0.0.1:5000",  # same IPv6-avoidance default as connect_camera
    timeout=180.0,
    auto_start_backend=True,              # spawn a local backend if none is running
) -> ArraySession
```

Các giá trị của `force_tier`:
- `"sim-capture-sim-emit"` — đồng bộ thực sự (tất cả các camera kích hoạt cùng một cạnh xung đồng hồ).
- `"sim-capture-ftd-stagger"` — lệch pha linh hoạt trong miền thời gian (các cam phát tín hiệu ở các thời điểm lệch nhau một chút để các gói tin được tuần tự hóa trên đường truyền).
- `"slip-emit-and-capture"` — thu thập tuần tự theo từng cam (không đồng bộ thời gian; là tùy chọn duy nhất khi không có kích thước khung nào phù hợp với chế độ đồng bộ).

`wire_ceiling_mbps` ghi đè lên **ngân sách băng thông liên tục của máy chủ** tính bằng MB/s — con số duy nhất
mà toàn bộ việc phân bổ mảng phụ thuộc vào. Giữ nguyên giá trị `None` để sử dụng giá trị được phát hiện tự động
. Hãy giảm giá trị này khi mảng báo cáo các khung hình bị hỏng theo tiêu chuẩn GVSP: giá trị tự động được tính toán
từ tốc độ kết nối được NIC công bố, vốn thường đánh giá quá cao đối với bộ điều hợp USB, các làn PCIe hẹp và
các mạng chia sẻ đang bận rộn — và các bề mặtsẽ biểu hiện dưới dạng các khung dữ liệu bị hỏng thay vì là một
liên kết chậm có thể nhận thấy. Giá trị này được lưu trữ trong khối ghi lại mảng của dự án, do đó việc
mở lại hoặc thiết lập lại `connect_array` sau này sẽ khôi phục nó giống như bất kỳ cài đặt mảng nào khác.
Xem [Tình trạng mảng](#array-health--which-subsystem-is-losing-frames).

#### Quá tải đăng ký (giới hạn tối thiểu cho mỗi camera)

Cơ chế điều tiết Sim-emit phân bổ cho mỗi camera một phần ngân sách đường truyền an toàn va chạm, với giới hạn tối thiểu là **8 MB/s cho mỗi camera**(`per_cam_floor_bps`). Khi `N × floor` vượt quá giới hạn trên an toàn va chạm, mảng**sẽ đăng ký quá mức băng thông**— chế độ lỗi là mất gói tin GVSP, không phải tốc độ khung hình thấp hơn — và không có giải pháp nào liên quan đến kích thước khung hình:**việc gộp khung hình (binning) và vùng quan tâm (ROI) làm giảm số byte trên mỗi khung hình, chứ không phải số byte được phân bổ theo nhịp mỗi giây**mà kiểm tra tổng hợp so sánh. Giới hạn thực tế ở độ phân giải đầy đủ trên máy chủ 1 GbE:**6 camera @ 1500 MTU, 9 camera với khung hình jumbo** (`max_cams_collision_safe` trong báo cáo phản hồi phân tích cho biết giới hạn trên của đường truyền của bạn). Các giải pháp: giảm số lượng camera, sử dụng khung jumbo từ đầu đến cuối, hoặc sử dụng NIC nhanh hơn.

- Các phản hồi `analyze_array_network()` và `/api/camera/array/connect` chứa `oversubscribed`, `aggregate_demand_bps`, `collision_safe_ceiling_bps`, `max_cams_collision_safe` và `per_cam_floor_bps`. Khi `oversubscribed` là true, phép chiếu **sẽ đặt các trường fps về 0** (`achievable_fps_max` / `fps_bright` / `fps_dark`) thay vì báo cáo một tốc độ chậm nhưng vẫn hoạt động, điều này có thể gây hiểu lầm.
- `POST /api/camera/array/connect` chấp nhận tham số nội dung `pin_resolution` (**chỉ HTTP— không phải là tham số từ khóa SDK**; `connect_array` không hiển thị tham số này). Việc ghim loại bỏ cơ chế an toàn giảm dần theo nhóm, do đó một kết nối bị quá tải với `pin_resolution` được thiết lập sẽ bị**từ chối cứng** kèm theo lỗi nêu rõ mọi biện pháp khắc phục. Nếu không có tính năng ghim, quá trình kết nối sẽ tiếp tục với cơ chế giảm dần nhưng sẽ cảnh báo rằng việc thu hẹp không thể xóa bỏ tổng hợp.
- Lối thoát trong môi trường thử nghiệm: đặt `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED=1` trong môi trường của hệ thống nền để hạ cấp mức từ chối xuống thành cảnh báo mạnh — bạn vẫn có thể kết nối và chấp nhận việc mất gói tin.

#### Tình trạng mảng — hệ thống con nào đang mất khung

`GET /api/camera/array/<array_id>/capability` mang theo một khối `health` đang hoạt động trên một
mảng được kết nối, đượcđược đánh giá lại trong một cửa sổ **10 giây** lăn. Nó chia tách tình trạng mất khung
thành hai nguyên nhân cần các biện pháp khắc phục trái ngược nhau, thay vì một tỷ lệ “không hoàn chỉnh”
không chỉ ra cụ thể nguyên nhân nào:

| Trường | Ý nghĩa | Hệ thống con nào |
| --- | --- | --- |
| `gvsp_corrupt_rate_pct` (theo từng cổng nối tiếp) | Khung hình **đã đến nhưng có cấu trúc sai**— mất gói tin GVSP. |**Mạng**: băng thông đường truyền, tốc độ truyền, vòng nhận NIC, MTU |
| `never_arrived_rate_pct` (theo cổng nối tiếp) | Khung dữ liệu **chưa bao giờ đến**— camera không kích hoạt, hoặc không có dữ liệu nào được gửi đi. |**Kích hoạt / đồng bộ**: cáp M8, `line=`, `TriggerMode` |
| `worst_gvsp_corrupt_pct` / `worst_never_arrived_pct` | Tỷ lệ camera kém nhất cho từng trường hợp. | — |
| `per_cam_rate_pct` | Tỷ lệ không hoàn thành tổng hợp trên mỗi máy ảnh (cả hai nguyên nhân cộng lại). | — |
| `stable_for_seconds` | Thời gian mỗi máy ảnh duy trì ở mức dưới 0,01 %. | — |

Bên cạnh `health`, bản ghi này cũng báo cáo số liệu về toàn bộ dung lượng phân bổ còn trống:

| Trường | Ý nghĩa |
| --- | --- |
| `wire_ceiling_mbps` | Ngân sách băng thông dây duy trì hiện tại của máy chủ, MB/s. |
| `wire_ceiling_source` | Nguồn gốc của con số đó, bằng văn bản — ví dụ: `USB-capped 200 MB/s (was theoretical 1062; …)` hoặc `user override 120 MB/s (auto said 200)`. |
| `wire_ceiling_is_user_set` | `true` khi `wire_ceiling_mbps=` thiết lập giá trị này. |
| `nic_is_usb` | `true` dành cho bộ chuyển đổi Ethernet qua USB. |

Không có gói SDK nào cho điểm cuối này — hãy đọc trực tiếp:

```python
import requests, chloros_sdk

arr = chloros_sdk.attach_array(["213800234", "214000533"])
h = requests.get(
    f"http://127.0.0.1:5000/api/camera/array/{arr.array_id}/capability",
    timeout=10).json()

health = h.get("health", {})
print("wire ceiling:", h["wire_ceiling_mbps"], "MB/s", h["wire_ceiling_source"])
print("corrupt (network) :", health.get("worst_gvsp_corrupt_pct"), "%")
print("absent  (trigger) :", health.get("worst_never_arrived_pct"), "%")

if (health.get("worst_gvsp_corrupt_pct") or 0) > 1.0:
    # Network path. Reconnect with a lower budget -- NOT a lower target_fps.
    arr.disconnect()
    arr = chloros_sdk.connect_array(serials, wire_ceiling_mbps=120)
```

**Cách đọc:** giá trị `gvsp_corrupt_rate_pct` khác 0 và `never_arrived_rate_pct` bằng 0 có nghĩa là
tín hiệu kích hoạt và đồng bộ cáp hoàn hảo, và 100% mức mất mát nằm trên đường truyền mạng — giảm
giá trị `wire_ceiling_mbps` và kết nối lại. Mô hình ngược lại chỉ ra vấn đề nằm ở cáp đồng bộ hoặc
dây kích hoạt.

> **`target_fps` không phải là yếu tố gây ra các khung dữ liệu bị hỏng.** Tốc độ điều chỉnh GevSCPD được thiết lập một lần khi
> kết nối, do đó việc giảm tần số kích hoạt sẽ thay đổi chu kỳ làm việc chứ không phải
> tốc độ phát đồng thời theo đợt. Việc giảm 5 lần nhu cầu đã đo được không mang lại cải thiện, trong khi
> việc hạ giới hạn băng thông từ 240 xuống 200 MB/s đã giảm tỷ lệ khung bị hỏng của cùng một hệ thống từ 10,4% xuống
> 0,00%.

> **Tính năng tự động thu hẹp giữa luồng (mid-stream auto-shrink) không khả dụng trên firmware TRI032S.** Một mảng đang hoạt động không thể
> tự khắc phục vấn đề này; hãy ngắt kết nối và kết nối lại để trình chọn thời điểm kết nối lập kế hoạch lại dựa trên
> giới hạn mới.

**Bộ điều hợp Ethernet USB bị giới hạn ở 200 MB/s** bởi công cụ kiểm tra bất kể
thông số kỹ thuật ghi trên nhãn: bảng hiệu suất chuyển đổi tốc độ liên kết thành giá trị duy trì được
dựa trên PCIe, và bộ điều hợp mạng USB (NIC) công bố tốc độ liên kết Ethernet của mình nhưng bị giới hạn bởi
bus USB và trình điều khiển của nó. Giới hạn này là tuyệt đối, không phải tỷ lệ — một bộ chuyển đổi USB 1 GbE
đạt ~80 MB/s và không bị ảnh hưởng.

#### `ArraySession` Phương pháp

| Phương pháp | Mô tả |
| --- | --- |
| `status(timeout=10.0)` | Chế độ trực tiếp `{fps, ptp, frame_count, last_error, …}`. |
| `capture(output_dir="output", format="tiff", processing="debayered", levels=None, aligned=None, render_index=None, force_daq=None, smart=False, timeout=300.0)` | Một nhóm ghi đồng bộ. Trả về một `CaptureResult` (danh sách các từ điển khung hình + `.skipped`). Các điều khiển xuất được liệt kê bên dưới. |
| `capture(..., smart=True)` | **Ghi hình thông minh** — chờ cho đến khi AE ổn định trên tất cả các camera, sau đó kích hoạt. |
| `capture_fastest(output_dir="output", force_daq=True, render_index=True, timeout=120.0)` | Ghi hình nhanh nhất: chỉ dữ liệu thô + giá trị đọc DAQ được gán (+ chỉ số kết hợp tự do). Tương ứng với nút &quot;Ghi hình nhanh nhất&quot; trên giao diện người dùng. |
| `capture_repeated(output_dir="output", count=None, duration_s=None, interval_s=0.0, on_capture=None, **capture_kwargs)` | Chế độ đơn / Liên tục / Theo khoảng thời gian trong một vòng lặp có giới hạn. Trả về `list[CaptureResult]`.**Yêu cầu `count` và/hoặc `duration_s`** để kết thúc quá trình (SDK không hỗ trợ phím tắt Ctrl+C). |
| `record(output_dir="output", fps=10.0, duration_s=None, video=True, gif=False, timeout=30.0)` | Bắt đầu ghi lại chế độ xem chỉ số kết hợp trực tiếp thành video/GIF → `RecorderHandle`. Mỗi mảng chỉ có một trình ghi tổng hợp. |
| `burst(output_dir="output", duration_s=None, max_frames=None, index_config=None, serial_index_config=None, timeout=30.0)` | Bắt đầu chụp liên tiếp Bayer thô với tốc độ khung hình cao → `RecorderHandle`. Xử lý lại ngoại tuyến bằng `build_video()`. |
| `build_video(burst_dir, products=None, fps=10.0, video=True, gif=False, save_tiffs=False, wait=True, poll_s=2.0, timeout=1800.0)` | Xử lý lại ngoại tuyến một loạt ảnh thô đã lưu thành video đã hiệu chỉnh. Chờ cho đến khi hoàn tất (`wait=True`) và trả về `{outputs, errors, combined}`. |
| `build_video_status(job_id, timeout=15.0)` | Kiểm tra tiến độ công việc xây dựng ngoại tuyến: `{running, result, error, burst_dir}`. |
| `disconnect()` | Giải phóng toàn bộ mảng. |

Kiểm soát xuất dữ liệu `capture()` (cùng điểm cuối với GUI/CLI ):

- `processing` / `levels` — `processing="all"` (hoặc `levels=["raw","radiance",…]`) lưu mọi loại xuất áp dụng cho từng cam; một một giá trị `processing` duy nhất chỉ lưu cấp độ đó.
- `aligned=True` — điều chỉnh (warp) mọi xuất dữ liệu không thô (non-raw) của từng thành viên theo [hồ sơ căn chỉnh](#array-alignment) (được đăng ký chung); dữ liệu thô không bị biến dạng nhưng mang theo phép biến đổi trong siêu dữ liệu. Sẽ quay lại trạng thái không căn chỉnh (với cảnh báo xuất hiện trong `alignment` của kết quả) nếu mảng không có cấu hình.
- `render_index=False` — bỏ qua lớp phủ chỉ số thảm thực vật cho từng camera; mặc định sẽ hiển thị nó ở vị trí đã cấu hình.
- `force_daq=True` — lưu giá trị đọc DAQ/DLS đã gán dưới dạng tệp phụ `.daq` ngay cả khi không có cấp độ nào được chọn cần đến nó.

**NénTIFF (chỉ có tùy chọn HTTP):** `ArraySession.capture()` không gửi khóa `compression`, do đó áp dụng mặc định của backend — `POST /api/camera/array/capture` đọc tham số cơ thể `compression` theo mặc định (zlib L1 không mất dữ liệu + bộ dự đoán ngang, ~4,1 MB cho mỗi khung hình độ phân giải đầy đủ). `"none"` ghi dữ liệu không nén (~6,3 MB/khung hình) với tốc độ ghi**~5 lần nhanh hơn** — cả hai đều không mất dữ liệu và được đọc giống hệt nhau khi nhập. Phương thức `SDK` không cung cấp tham số nào cho tính năng này; giải pháp thay thế là sử dụng ``chloros-cli lattice array-capture --compression none`` hoặc định dạng thô `HTTP`. DEFLATE cũng giữ GIL tại `Python`, do đó việc ghi dữ liệu nén không thể song song hóa trên các luồng ghi riêng lẻ cho từng camera — việc ghi liên tục 8 camera ở độ phân giải đầy đủ với tốc độ cảm biến cần `compression: "none"`. Chi tiết: [CLI Tham khảo → array-capture](cli-reference.md).**Ghi đè xuất theo từng thành viên (chỉ áp dụng cho HTTP):**điểm cuối này cũng chấp nhận `exclude_serials` (danh sách — loại bỏ các thành viên khỏi tập hợp đã lưu; mảng vẫn kích hoạt như một nhóm đồng bộ và các thành viên bị loại trừ được trả về trong `excluded`), `serial_levels` (các tùy chỉnh cấp độ camera riêng lẻ `{serial: [level tokens]}`), và `serial_index` (các tùy chỉnh lớp phủ chỉ mục cấp độ camera riêng lẻ `{serial: bool}`). Đây là các tham số cơ bản tương đương với giao diện người dùng và**chưa phải là các tham số kwargs củSDK**; các thành viên không có trong các bản đồ sẽ được thay thế bằng các giá trị chung cho toàn mảng `levels` / `render_index`.

##### Kiểm tra các cam bị bỏ qua — `CaptureResult.skipped`

`ArraySession.capture()` trả về một `CaptureResult`, là một lớp con của `list`: lặp qualặp, lập chỉ mục, `len()` — mọi mẫu hiện có vẫn hoạt động bình thường. Mã mới có thể kiểm tra thuộc tính `.skipped` để xem những cảm biến nào đã bị loại trừ và lý do tại sao. Trường hợp phổ biến nhất là các cảm biến RGB trong một mảngkhi bạn yêu cầu `processing="radiance"` hoặc `"reflectance"` — độ rạng rỡ trên mỗi điểm Bayer không có ý nghĩa đối với cảm biến băng thông rộng, do đó phần xử lý phía sau sẽ bỏ qua các cảm biến đó thay vì tạo ra kết quả vô nghĩa.

```python
with chloros_sdk.connect_array(serials) as arr:
    result = arr.capture("output/", processing="reflectance")

    # Back-compat: iterate as a plain list
    for frame in result:
        print(frame["filepath"], frame["serial"])

    # New: see why N-1 cams were saved
    for skip in result.skipped:
        print(f"skipped SN:{skip['serial']} reason={skip['reason']}")
        # e.g. {'serial': '214701292', 'level': 'reflectance',
        #       'reason': 'reflectance-not-applicable-to-rgb-cam',
        #       'filter': 'RGB'}
```

Các mã lý do tuân theo mẫu `<level>-not-applicable-to-rgb-cam` (mỗi mức bị bỏ qua tương ứng với một mục, mỗi mục chứa `level`). Các trường hợp bỏ qua liên quan đến độ phản xạ cụ thể là `reflectance-skipped-no-fresh-dls` (không có dữ liệu đo xuống mới), `reflectance-skipped-bound-daq-unavailable (…)` (không thể kết nối với thiết bị DAQ bị giới hạn), và `dls-uncalibrated-band-<nm>` — dải này chủ yếu nằm ngoài phạm vi hiệu chuẩn quang phổ của cảm biến ánh sáng DAQ (~374–974 nm), do đó việc phân chia độ phản xạ tuyệt đối dựa trên DAQ bị từ chối và khung hình sẽ chuyển xuống chế độ phản hồi cảm biến. Trong số các SKU đang được phân phối, chỉ có F988 kích hoạt tính năng này; quy trình làm việc được hỗ trợ cho máy ảnh đó là quy trình làm việc với bảng phản xạ.

Các mức `processing`:

| Mức | Đầu ra |
| --- | --- |
| `"raw"` | Kênh đơn Bayer (máy ảnh đơn sắc: dải đơn) trực tiếp từ cảm biến. |
| `"debayered"` *(mặc định của SDK)* | 3 kênh BGR thông qua khử mô-saic tuyến tính (máy ảnh đơn sắc: độ xám 1 kênh). |
| `"radiance"` | float32 W/m²/sr/nm qua chuỗi đo quang học đầy đủ. Chỉ dành cho đa quang phổ — các camera RGB sẽ bị bỏ qua. |
| `"reflectance"` | uint16 0..32768 (tương thích với Pix4D); yêu cầu ghép nối DAQ trực tiếp để tham chiếu tuyệt đối. Chỉ dành cho đa quang phổ. |
| `"display"` | Chuỗi đầy đủ khớp với bản xem trước trên giao diện người dùng (CCM + WB + gamma theo cấu hình của camera). |
| `"all"` | **Một tệp cho mỗi mức áp dụng** cho mỗi camera (phù hợp với cài đặt mặc định &quot;Capture All&quot; / CLI trên giao diện người dùng). Tệp `CaptureResult` trả về sau đó chứa một từ điển khung hình cho mỗi `(cam, level)`, với mức độ được ghi trong mỗi từ điển; các mức độ không áp dụng sẽ xuất hiện trong `.skipped`. Dữ liệu DAQ được sử dụng cho bất kỳ khung hình phản xạ nào sẽ được lưu dưới dạng tệp phụ `.daq`. |

> **Lưu ý — cài đặt mặc định khác với CLI.** `ArraySession.capture()` mặc định là `processing="debayered"`; lệnh `chloros-cli lattice array-capture` mặc định là `processing="all"`. Truyền `processing="all"` một cách rõ ràng từ SDK để phản ánh tính năng lưu đa cấp trên CLI /GUI.

### Chế độ chụp &amp; Bộ ghi

Bảng điều khiển phản ánh bảng điều khiển chụp trên GUI: Chế độ chụp Đơn / Liên tục / Khoảng thời gian / Tốc độ màn trập nhanh nhất, cùng với hai bộ ghi (video tổng hợp trực tiếp và chụp liên tiếp thô → xử lý lại ngoại tuyến).

```python
import time, chloros_sdk

with chloros_sdk.connect_array(serials) as arr:
    # Single (default) — one synced group
    arr.capture("out/", processing="reflectance")

    # Fastest — raw + .daq + combined index now, calibrate later
    arr.capture_fastest("flightline/")

    # Interval — one reflectance pass every 2 s, 5 passes (bounded so it ends)
    arr.capture_repeated("timelapse/", count=5, interval_s=2.0,
                         processing="reflectance",
                         on_capture=lambda i, r: print(f"pass {i}: {len(r)} frames"))

    # Combined-index video/GIF recorder (needs the combined live view streaming)
    with arr.record("monitoring/", fps=10, gif=True) as rec:
        time.sleep(30)
    print(rec.result["video_path"])

    # Raw-Bayer burst → offline reprocess into calibrated video(s)
    with arr.burst("capture/", duration_s=5) as b:
        pass
    out = arr.build_video(b.result["out_dir"], products=[
        {"kind": "per_cam", "level": "reflectance"},
        {"kind": "combined", "level": "index"}])
    print(out["outputs"])
```

- **`capture_repeated`**là chế độ Liên tục/khoảng thời gian. Vì không có `Ctrl+C` để ngắt chu trình này từ tập lệnh, bạn**phải** truyền `count` và/hoặc `duration_s` (nó sẽ dừng khi đạt đến một trong hai giá trị này). `interval_s` được đo từ đầu mỗi lần lặp (phù hợp với GUI). Các kwargs còn lại được truyền thẳng đến `capture()`.
- **`record`** là *cấp độ giám sát*: nó ghi lại luồng chỉ số kết hợp trực tiếp như được hiển thị, do đó luồng kết hợp phải được mở để các khung hình có thể được ghi lại. Mỗi mảng chỉ có một trình ghi luồng kết hợp (sẽ gây ra lỗi nếu đã có một trình ghi đang chạy).
- **`burst` → `build_video`** là *cấp độ phân tích*: `burst` ghi các khung hình thô + một- khung hình + một tệp `.daq` cho mỗi lần đọc DLS riêng biệt dưới `<output>/bursts/<base>/` ở tốc độ tối đa của vòng lặp thu thập (không có chuỗi, không có exiftool, không có chế độ xem trực tiếp). `build_video` đồng bộ hóa thời gian từng khung hình với `.daq` gần nhất và chạy lại chuỗi xử lý độ rạng rỡ/độ phản xạ/chỉ số. `products` là danh sách các `{"kind": "per_cam"|"combined", "level": "radiance"|"reflectance"|"index"}` (mặc định: chỉ số kết hợp). `burst().stop()` cũng tự động khởi chạy quá trình xây dựng chỉ số kết hợp tốt nhất có thể, được trả về dưới dạng `build_job` trong kết quả dừng.

#### `RecorderHandle`

Được trả về bởi `ArraySession.record()` và `ArraySession.burst()`. Sử dụng nó như một trình quản lý bối cảnh để tự động dừng khi thoát khỏi phạm vi, hoặc điều khiển thủ công.

| Thành phần | Mô tả |
| --- | --- |
| `job_id` | ID công việc backend (chuỗi). |
| `kind` | `"composite"` (từ `record`) hoặc `"raw"` (từ `burst`). |
| `start_stats` | Từ điển được trả về bởi cuộc gọi `start`. |
| `result` | `None` trong khi đang chạy; đối tượng kết quả dừng cuối cùng sau khi dừng. |
| `stats(timeout=10.0)` | Thống kê công việc thời gian thực (số khung hình đã ghi, fps thực tế, thời gian đã trôi qua). |
| `stop(timeout=60.0)` | Dừng trình ghi; trả về và lưu trữ kết quả cuối cùng vào bộ nhớ đệm. Tính idempotent (lần gọi thứ hai sẽ trả về kết quả đã được lưu trong bộ nhớ đệm). |

```python
rec = arr.burst("capture/")
# ... drive manually ...
print(rec.stats()["frames"])
result = rec.stop()
print(result["out_dir"], result.get("build_job"))
```

### Kết nối với một mảng đã được kết nối — `attach_array`

Nếu mảng đã được khởi tạo (GUI đã mở nó, hoặc một phiên làm việc trước đó tại SDK đã gọi hàm `connect_array`), hãy sử dụng `attach_array` để lấy handle của mảng thay vì kết nối lại. `connect_array` luôn báo lỗi với thông báo &quot;Camera <sn>đã </sn>có <sn>trong mảng<id>&quot; trong tình huống đó, vì việc gửi yêu cầu POST với `/array/connect` cho một thành viên trong nhóm không phải là idempotent; `attach_array` đọc `/api/camera/array/list` và so khớp dựa trên array_id hoặc số sê-ri.

```python
import chloros_sdk

# By serials (matches if every serial is a member of one existing array)
arr = chloros_sdk.attach_array(
    ["213800234", "214000533", "214701288", "214701292"])

# By array_id (when you've already noted it down)
arr = chloros_sdk.attach_array("array-1779862544497")

# attach_array returns the same ArraySession as connect_array
arr.capture("output/", processing="reflectance")
```

Mẫu: Các skript SDK chạy chung với giao diện người dùng desktop nên thử `attach_array` trước và chuyển sang `connect_array` nếu chưa có mảng nào trong nhóm.

```python
import chloros_sdk

try:
    arr = chloros_sdk.attach_array(serials)
except chloros_sdk.ChlorosConnectError:
    arr = chloros_sdk.connect_array(serials)
```

> **Quan trọng — việc thoát khỏi trình quản lý bối cảnh SẼ ngắt kết nối.**`ArraySession.disconnect()` luôn thực hiện POST cho `/array/disconnect`; không có cơ chế bảo vệ &quot;attached-not-owned&quot; như đối với `CameraSession` / `DAQSensorSession`. Nếu bạn đang chia sẻ không gian với GUI và không muốn dỡ bỏ mảng khi thoát khỏi phạm vi,**không sử dụng khối `with`** — hãy lưu handle vào một biến thông thường và bỏ qua lệnh `disconnect()` rõ ràng:
>
> ```python
> arr = chloros_sdk.attach_array(serials)
> arr.capture("output/", processing="reflectance")
> # … script ends; array stays up for the GUI
> ```

### Trợ giúp Phân tích Mạng

Hữu ích trước khi mở mảng — dự đoán xem các thiết lập đề xuất của bạn có phù hợp hay không:

```python
result = chloros_sdk.analyze_array_network(
    master_serial="214701288",
    slave_serials=["213800234", "214000533", "214701162"],
    width=2048, height=1536,
    pixel_format="BayerRG12",
    binning=1,
)

if result["status"] == "ok":
    print("Use the requested settings.")
elif result["status"] == "auto_capped_fps":
    r = result["recommended"]
    print(f"Keep the resolution; cap the trigger rate at {r['recommended_target_fps']} fps")
elif result["status"] == "auto_shrunk":
    r = result["recommended"]
    print(f"Shrink to {r['out_width']}x{r['out_height']} binning={r['binning']}")
elif result["status"] == "needs_force_slip":
    print("Sim-sync impossible on this wire; force_tier='slip-emit-and-capture' required")
```

`status` là một trong các giá trị: `ok` / `auto_capped_fps` / `auto_shrunk` / `needs_force_slip` (nếu không thì là `error`). `auto_capped_fps` có nghĩa là độ phân giải được yêu cầu chỉ phù hợp với vòng RX ở tốc độ kích hoạt bị giới hạn — giữ nguyên độ phân giải và chuyển từ `target_fps=result["recommended"]["recommended_target_fps"]` sang `connect_array` (xem [Ví dụ 6](#6-capability-probe-before-connecting-a-4-cam-array)).

**Cách đọc thông tin chiếu** (cùng mô hình với bảng Cài đặt Mảng trên GUI):

- **Chế độ Burst (`frame_bytes_total`) được tổng hợp theo từng camera ở định dạng pixel thực của từng camera.**Các camera đơn sắc**M3M**truyền dữ liệu ở chế độ Mono12 (2 bit/pixel) bất kể giá trị `pixel_format` bạn truyền vào là gì, do đó một khung hình độ phân giải đầy đủ từ 4 camera sẽ có dung lượng**~25 MB** với ba camera mono, chứ không phải ~12,6 MB như giả định tất cả đều là 8-bit. Hệ thống backend xác định định dạng của từng camera dựa trên mẫu của nó.
- **Độ dẫn (`burst_fits_nic_ring`) phụ thuộc vào tốc độ xả dữ liệu**, không phải so sánh giữa chế độ toàn bộ đợt dữ liệu và chế độ vòng lặp: chế độ sim-emit phù hợp khi máy chủ xả dữ liệu khỏi vòng lặp RX nhanh hơn tốc độ các camera nạp dữ liệu vào vòng lặp đó. Một máy chủ 10G kết hợp với các cam 1 GbE**cho phép** độ phân giải đầy đủ ngay cả khi gói dữ liệu vượt quá dung lượng vòng; một máy chủ 1 GbE sẽ bị chặn (`needs_force_slip` / `auto_shrunk`).
- **`achievable_fps_max` là giới hạn bảo thủ cho việc**** — `max(readout+emit, N×emit)` với tốc độ phát ra của từng camera bị giới hạn ở liên kết camera 1 GbE, không phụ thuộc vào độ phơi sáng. Ví dụ:g. ~2,8 khung hình/giây (fps) cho mảng 4 camera độ phân giải đầy đủ 12-bit (trùng khớp với giá trị đo được trong thời gian chạy là ~2,7–3,0). Mô hình đầy đủ: [CLI Tham khảo → Mô hình fps &amp; burst của mảng](cli-reference.md#array-fps--burst-model).
- **Quá tải (`oversubscribed: true`) có nghĩa là giới hạn tối thiểu N × trên mỗi camera vượt quá giới hạn an toàn va chạm** — các trường fps (`achievable_fps_max` / `fps_bright` / `fps_dark`) hiển thị giá trị 0, và tính năng tự động thu nhỏ/gộp không thể khắc phục vấn đề này (các tính năng này chỉ giảm số byte trên mỗi khung hình, chứ không phải số byte được phân phối đều mỗi giây). Các giải pháp bao gồm giảm số lượng camera, sử dụng khung jumbo hoặc card mạng (NIC) nhanh hơn; `max_cams_collision_safe` báo cáo giới hạn trên (6 camera độ phân giải đầy đủ trên 1 GbE @ 1500 MTU, 9 camera khi dùng khung jumbo). Phản hồi này cũng chứa các mã lỗi `aggregate_demand_bps`, `collision_safe_ceiling_bps` và `per_cam_floor_bps` (8 MB/s). Xem [Quá tải](#over-subscription-the-per-cam-floor).

### Khám phá &amp; Liệt kê

```python
chloros_sdk.discover_lattice_cameras()   # list all cams visible to the backend
chloros_sdk.list_cameras()               # cams currently in the pool
chloros_sdk.list_arrays()                # active arrays in the pool
```

---

## Smart-AE / Smart-Capture

Các mảng LATTICE chạy chế độ AE liên tục ở chế độ nền ngay khi được kết nối, nhưng một cảnh vừa được định hướng sẽ mất một chút thời gian để hội tụ. **Smart-Capture** là giải pháp tiện lợi được tích hợp sẵn: nó kiểm tra mức phơi sáng của từng camera, chờ cho đến khi mảng ổn định trên toàn khung hình, sau đó kích hoạt quá trình chụp. Tính năng này tương đương với giao diện người dùng (GUI): nút &quot;Smart Capture&quot; trên ứng dụng máy tính để bàn sẽ gọi cùng một điểm cuối phía máy chủ.

```python
import chloros_sdk

with chloros_sdk.connect_array([
        "213800234", "214000533", "214701288", "214701292"]) as arr:
    # Initial pose
    arr.capture("pose_a/", processing="reflectance", smart=True)
    input("Move the rig, then press Enter...")
    # New pose — smart-capture waits for AE to re-settle automatically
    arr.capture("pose_b/", processing="reflectance", smart=True)
```

Khi điều khiển qua `ChlorosProject` (phần tiếp theo), bạn sẽ có thêm các tùy chọn điều chỉnh:

```python
proj.arrays["main_rig"].capture_smart(
    output_dir="out/",
    processing="reflectance",
    settle_timeout_s=5.0,           # max wait
    stability_window_s=1.5,         # exposure must hold steady this long
    exposure_tolerance_pct=5.0,     # %-spread allowed within the window
)
```

Chính sách AE thông minh mặc định là thận trọng. Hãy điều chỉnh `exposure_tolerance_pct` chặt chẽ hơn cho các công việc đo quang học đòi hỏi độ chính xác cao; nới lỏng cho các cảnh thay đổi nhanh, nơi bạn chỉ cần độ chính xác &quot;gần đúng&quot;.

---

## Phiên cảm biến DAQ

Nhóm backend liên tục cho các cảm biến quang phổ (DAQ-U qua USB, DAQ-M qua BLE, DAQ-E qua Ethernet). Phản ánh bề mặt camera: phát hiện thông minh, tái sử dụng nhóm, kết nối idempotent.

### Phát hiện thông minh (Zero-Config)

```python
import chloros_sdk

with chloros_sdk.connect_daq_sensor() as daq:
    print(daq.model, daq.transport, daq.address)
    for frame in daq.latest(n=10):
        spectrum = frame["spectrum"]   # list[float] (W/m²/nm if calibrated)
        is_sat = frame["is_saturated"]
        x, y, z = frame["x"], frame["y"], frame["z"]
        print(len(spectrum), is_sat)
```

Thứ tự ưu tiên: Ethernet → BLE → USB. Truyền bất kỳ gợi ý rõ ràng nào để cố định giao thức truyền tải.

### Giao thức truyền tải được cố định

```python
# DAQ-U on a specific serial port
daq = chloros_sdk.connect_daq_sensor(transport="usb", port="COM3")

# DAQ-M over BLE by MAC (implies transport="ble")
daq = chloros_sdk.connect_daq_sensor(mac="AA:BB:CC:DD:EE:FF")

# DAQ-E over Ethernet by hostname (implies transport="eth")
daq = chloros_sdk.connect_daq_sensor(eth_host="daq-e-xxx.local")

# Tuning knobs
daq = chloros_sdk.connect_daq_sensor(
    port="COM3",
    integration_time=64,      # ms
    frame_avg=20,
    enable_ae=True,
    start_streaming=True,
)
```

### Các phương thức của `DAQSensorSession`

| Phương thức | Mô tả |
| --- | --- |
| `status(timeout=10.0)` | Tóm tắt mục trong nhóm (trạng thái truyền phát/ghi, dải bước sóng, giá trị SHA hiệu chuẩn, thời gian tích hợp, frame_avg, trạng thái AE). |
| `latest(n=1, timeout=10.0)` | Trả về tối đa N khung phổ gần nhất. |
| `stream_start()` / `stream_stop()` | Tiếp tục / tạm dừng truyền dữ liệu (cổng vẫn mở). |
| `record_start(output_dir=None, device_name=None)` | Bắt đầu ghi tệp .daq. Trả về đường dẫn tệp. Bị từ chối đối với DAQ-U/M không có gói hiệu chuẩn AWS (DAQ-E được miễn trừ). |
| `record_stop()` | Dừng ghi. Trả về `{path, rows}`. |
| `disconnect()` | Giải phóng khỏi vùng nhớ. Không thực hiện thao tác nào đối với các handle đã được gắn nhưng không thuộc sở hữu. |

> **Các-correction profiles (`cap_id`) không phải là công cụ điều chỉnh &quot;SDK&quot;.** `connect_daq_sensor()` / `DAQSensorSession` không cung cấp tham số `cap_id` hoặc phương thức `set_cap`. Chọn cấu hình hiệu chỉnh giới hạn cho đội tàu thông qua giao diện người dùng (CLI) (`chloros-cli daq pool-connect --cap-id …` / `chloros-cli daq pool-set-cap …`) hoặc các đường dẫn xử lý phía sau (backend) của `/api/daq` HTTP (`/api/daq/connect` và `/api/daq/<id>/cap-id` chấp nhận `cap_id`).

### Khám phá — tìm địa chỉ để kết nối

`discover_daq_sensors()` quét USB / BLE / ETH để tìm các cảm biến mà bạn *có thể* mở. Đây là phiên bản DAQ tương ứng với `discover_lattice_cameras()`, và là cách duy nhất để lấy **địa chỉ MAC BLE của DAQ-M** — một DAQ-E có tên máy chủ và DAQ-U có cổng COM, nhưng địa chỉ MAC không được in trên thiết bị cũng như không được hệ điều hành liệt kê.

```python
for s in chloros_sdk.discover_daq_sensors():
    print(s["transport"], s["address"], s["model"], s["extra"])
# ble  C3:D8:85:E0:0A:19  DAQ-M  {'name': 'NSP32_SPECTRUM'}
# usb  COM3               None   {'manufacturer': 'Intel'}

# `address` is exactly what connect_daq_sensor wants:
for s in chloros_sdk.discover_daq_sensors(transports=["ble"]):
    if s["model"] == "DAQ-M":
        daq = chloros_sdk.connect_daq_sensor(mac=s["address"])
```

| Trường | Mô tả |
| --- | --- |
| `transport` | `usb` \| `ble` \| `eth`. |
| `address` | Cổng COM / MAC BLE / tên máy chủ — truyền đến `connect_daq_sensor` dưới dạng `port=` / `mac=` / `eth_host=`. |
| `display` | Nhãn dễ đọc. |
| `model` | `DAQ-U` \| `DAQ-M` \| `DAQ-E`, hoặc `None` đối với cổng mà quá trình quét không thể xác định (các bộ chuyển đổi nối tiếp USB không thể phân biệt được nếu không có đầu dò, do đó các giá trị không xác định sẽ được hiển thị thay vì ẩn đi). |
| `extra` | Chi tiết theo từng giao thức (tên quảng cáo BLE, nhà sản xuất USB, địa chỉ IP/phần mềm hệ thống của DAQ-E…). Các giá trị trống sẽ bị bỏ qua. |

| Thông số | Mặc định | Mô tả |
| --- | --- | --- |
| `transports` | cả ba | Dãy số (hoặc chuỗi CSV) giới hạn quá trình quét. Nên truyền tham số này khi bạn biết chính xác những gì mình muốn — BLE là phần chậm nhất. |
| `scan_timeout` | 5 | Khoảng thời gian quét cho từng giao thức tính bằng giây; hệ thống nền sẽ giới hạn trong khoảng 1–20. |
| `timeout` | 60.0 | Giới hạn tối đa của `HTTP` cho toàn bộ cuộc gọi (giống như ở các phần khác trong `SDK`). |
| `auto_start_backend` | `True` | Khởi tạo một backend cục bộ nếu chưa có đang chạy. Không bao giờ khởi tạo cho một `backend_url` từ xa. |

> **Các cảm biến đã mở trong nhóm sẽ không xuất hiện.** Một thiết bị ngoại vi BLE đã kết nối sẽ ngừng phát tín hiệu quảng cáo và cổng COM đang mở không thể được quét, do đó quá trình phát hiện chỉ liệt kê những gì *có sẵn để kết nối*. Kết quả trống ngay sau khi bạn kết nối một thiết bị là điều bình thường — hãy sử dụng `list_daq_sensors()` cho những thiết bị bạn đã nắm giữ. Các giao thức truyền tải mà quá trình quét không thể thực hiện (chưa cài đặt bleak / zeroconf) sẽ bị bỏ qua thay vì báo lỗi, do đó một máy không có Bluetooth vẫn nhận được kết quả từ USB và ETH.

### Danh sách

```python
for s in chloros_sdk.list_daq_sensors():
    print(s["sensor_id"], s["model"], s["transport"], s["wavelength_range"])
```

### Chia sẻ tài nguyên với GUI / CLI

Nếu GUI đã mở sẵn một cảm biến, việc gọi `connect_daq_sensor(port="COM3")` từ Python sẽ trả về một handle được đánh dấu là `already_connected=True`. Lúc này, phiên làm việc`disconnect()` sẽ không thực hiện thao tác nào, do đó tập lệnh SDK của bạn sẽ không làm mất cảm biến khỏi GUI khi thoát khỏi phạm vi.

### Các lớp phần cứng trực tiếp (Không có backend)

`daq_sdk` được tái xuất bởi `chloros_sdk`, do đó bạn cũng có thể điều khiển cảm biến từ đầu đến cuối trong quá trình xử lý mà không cần backend:

> **Tính khả dụng:**`daq_sdk` được cung cấp kèm theo bản cài đặt máy tính để bàn từ Chloros,**không** đi kèm với gói PyPI — `pip install chloros-sdk` cung cấp cho bạn `lattice_sdk` nhưng loại bỏ `chloros_sdk.DAQ_AVAILABLE == False`. Hãy kiểm tra cờ này trước khi sử dụng các lớp này; trên máy chủ chỉ dùng pip, hãy điều khiển cảm biến thông qua [`connect_daq_sensor()`](#daq-sensor-sessions), không cần thư viện truyền tải cục bộ.

```python
from chloros_sdk import DAQUSensor, DAQMSensor, DAQESensor, discover_all

# Discovery
for d in discover_all(timeout=3.0):
    print(d.model, d.display, d.address)   # USB serials: d.extra.get("serial_number")

# Direct DAQ-U
sensor = DAQUSensor(port="COM3")
sensor.connect()
sensor.start_streaming()
# ... use sensor.add_spectrum_callback(...) ...
sensor.stop()
```

Nên ưu tiên đường dẫn kết nối thông minh (`connect_daq_sensor`) khi bạn muốn chia sẻ quyền sở hữu với GUI; sử dụng các lớp trực tiếp cho các tập lệnh không có giao diện người dùng (headless) sở hữu cảm biến độc quyền.

---

## Tự động hóa dự án — `ChlorosProject`

Một dự án Chloros đã lưu là một thư mục chứa `cameras.json` + `sensors.json` + `project.json`. `open_project` tải bản kê khai, và `connect_all` đưa mọi thiết bị đã lưu vào trạng thái trực tuyến với các cài đặt đã lưu — trạng thái phần cứng giống hệt như khi thực hiện qua giao diện người dùng (GUI).

### Ví dụ tối thiểu

```python
import chloros_sdk

proj = chloros_sdk.open_project("/home/user/Chloros Projects/Field_A")
report = proj.connect_all(verbose=True)
print(report)  # {'cameras': {...}, 'arrays': {...}, 'sensors': {...}}

# Cameras and arrays are addressable by name OR serial / array_id
cam = proj.cameras["FrontLeft"]
cam.capture("./out", format="tiff", processing="reflectance")

arr = proj.arrays["main_rig"]
arr.capture("./out", format="tiff", processing="reflectance")

# Read a DAQ
spectrum = proj.sensors["Sky"].read()

# Trigger every device simultaneously
proj.capture_all("./out")

proj.disconnect_all()
```

Hoặc dưới dạng trình quản lý bối cảnh:

```python
with chloros_sdk.open_project("/path/to/proj") as proj:
    proj.connect_all()
    proj.arrays["main_rig"].capture("./out", processing="reflectance")
```

### Các phương thức của `ChlorosProject`

| Phương thức | Mô tả |
| --- | --- |
| `connect_all(cameras=True, arrays=True, sensors=True, verbose=False, align=None)` | Phát hiện và kết nối mọi thiết bị đã lưu. Trả về báo cáo kết nối theo từng lớp. Sử dụng backend đang chạy nếu có backend đang lắng nghe trên `127.0.0.1:5000`; nếu không, sẽ tự động chuyển sang điều khiển thiết bị trực tiếp (không cần backend) `lattice_sdk` — nó không bao giờ khởi tạo backend. |
| `disconnect_all()` | Ngắt kết nối mọi thứ. |
| `capture_all(output_dir=".")` | Một khung hình từ mỗi camera + mảng + phổ từ mỗi cảm biến. |
| `stream(camera, overlays=False, fps=10.0)` | Bộ tạo khung hình BGR `numpy` từ một camera (hoặc mảng) được chỉ định. `overlays=False` là một vòng lặp `lattice_sdk`X (mảng tạo ra các từ điển `{serial: frame}`). `overlays=True` chuyển tiếp qua `ChlorosLocal.camera_stream()` → luồng MJPEG của phần hậu kỳ `/api/camera/<serial>/stream-annotated`, với khối `ui.overlay` đã lưu của camera được truyền qua dưới dạng tham số truy vấn. Yêu cầu chế độ backend và một **camera độc lập**: camera ở chế độ trực tiếp sẽ gây ra lỗi `RuntimeError` (hệ thống backend không thể lấy camera mà tiến trình này đang sở hữu) và một mảng sẽ gây ra lỗi `NotImplementedError` (tạo lớp phủ tổng hợp cho từng camera — phát luồng thành viên theo tên). Tương đương với lệnh chạy một lần: `CameraHandle.capture(annotated=True)`. |
| `align_arrays(align=True, verbose=False)` | Chạy căn chỉnh trên mọi mảng hiện đang được kết nối. |
| `process(mode="parallel", wait=True, progress_callback=None, poll_interval=2.0)` | Chạy quy trình hiệu chuẩn / lập chỉ mục trên các hình ảnh của dự án (bao bọc `ChlorosLocal.process`; bốn tham số này là **duy nhất** được chấp nhận — `indices=` v.v. sẽ gây ra lỗi `TypeError`; thiết lập chỉ số thông qua `ChlorosLocal.configure()`). Tạo một đối tượng `ChlorosLocal()` theo cách lười biếng, đối tượng này sẽ tự động khởi động một backend. |

Thuộc tính:
- `proj.cameras` — `Dict[str, CameraHandle]` được lập chỉ mục theo tên VÀ số sê-ri.
- `proj.arrays` — `Dict[str, ArrayHandle]` được lập chỉ mục theo tên VÀ array_id.
- `proj.sensors` — `Dict[str, SensorHandle]` được lập chỉ mục theo tên VÀ slot_id.
- `proj.config` — `project.json["config"]` từ điển.

### `CameraHandle`

```python
cam = proj.cameras["FrontLeft"]

# Save a frame to disk (processing-aware)
filepath = cam.capture(
    output_dir="./out",
    format="tiff",
    processing="radiance",           # see the level table below
    apply_calibration=True,          # DSNU + flat + 3x3 unmix + NIST
    apply_white_balance=True,        # DLS-aware WB
    apply_index=False,
    index_expression=None,
)

# In-memory grab (numpy array)
frame = cam.grab(processing="debayered")
frame, header = cam.grab(processing="radiance", with_metadata=True)

# Frame iterator (generator)
for arr in cam.frame_stream(processing="debayered", fps=5, count=100):
    my_analysis(arr)
```

**Các cấp độ xử lý.** `capture()`, `grab()` và `frame_stream()` đều sử dụng cùng một mã thông báo `processing`
, và chuỗi này có tính tích lũy — mỗi cấp độ sẽ chạy tất cả các cấp độ ở trên nó:

| Cấp độ | Đầu ra | Ghi chú |
| --- | --- | --- |
| `raw` | Bayer 1 kênh, định dạng gốc của cảm biến | Không có khử mô-saic. Các lớp phủ không khả dụng ở cấp độ này. |
| `debayered` | BGR 3 kênh (**mặc định**) | Demosaic tuyến tính kép. Cấp độ duy nhất hoạt động mà không cần chế độ backend. |
| `radiance` | float32, W/m²/sr/nm | Chuỗi đo quang học đầy đủ: demosaic + tách màu 3×3 (đa phổ) + DSNU + hiệu chỉnh trường phẳng + quy mô NIST, với phơi sáng × độ khuếch đại được loại bỏ để các giá trị là tuyệt đối. |
| `reflectance` | uint16, 32768 = 1,0 | Độ sáng chia cho bức xạ xuống (ρ = π·L/E). Cần có giá trị đọc từ DLS/DAQ — xem chú thích bên dưới. |
| `display` | 8-bit tương tự sRGB | Hiển thị tương đương giao diện người dùng: CCM + cân bằng trắng + gamma thông qua hồ sơ màu hoạt động của máy ảnh. |

Bất kỳ giá trị nào khác ngoài `debayered` đều yêu cầu chế độ backend; máy ảnh chế độ trực tiếp sẽ tạo ra
`NotImplementedError`. `reflectance` cần một giá trị đo xuống (downwelling) có thể sử dụng — điểm cuối khung hình sẽ tự động kéo
dữ liệu DAQ đã gộp vào khe DLS của máy ảnh, nhưng nếu không có DAQ nào được liên kết, chuỗi xử lý sẽ từ chối
đầu ra phản xạ và ghi rõ việc hạ cấp trong metadata trả về thay vì im lặng
trả về một sản phẩm có chất lượng thấp hơn.

> **Thang đo DN phản xạ — không nên mã hóa cứng giá trị này.** Phản xạ LATTICE sử dụng `32768` = ρ 1.0 và ghi nhãn
> XMP `Chloros:PixelScale=32768`; Survey3 độ phản xạ sử dụng `65535` = ρ 1.0 và không chứa
> thẻ `Chloros:*`. Đọc thẻ và chia cho giá trị đó. Giá trị này được định nghĩa trong miền uint16, do đó nó vẫn giữ nguyên
> `32768` cho mọi định dạng thực hiện điều chỉnh tỷ lệ (TIFF 16-bit, PNG /JPG 8-bit, 32-bit theo phần trăm) — hãy chuẩn hóa
> kiểu dữ liệu được lưu trữ trở lại uint16 trước (nhân với 257 từ 8-bit, nhân với 65535 từ float). Trường hợp ngoại lệ duy nhất:
> dữ liệu chụp nguồn 8-bit được ghi dưới dạng 8-bit TIFF sẽ bị *cắt bớt*, không được điều chỉnh tỷ lệ, do đó không có tỷ lệ nào mô tả
> nó — Chloros sẽ bỏ qua hoàn toàn `PixelScale` và bộ tuple MicaSense trong trường hợp đó. Xử lý thẻ thiếu
> trên tệp phản xạ LATTICE là “không có tỷ lệ hợp lệ”, chứ không phải là giá trị mặc định.

> **Dữ liệu EXIF được chuyển sang tệp xuất.** `process()` sao chép khối GPS của dữ liệu nguồn
> **và ExifIFD của nó** vào mọi sản phẩm, do đó các tệp xuất sẽ chứa `FocalLength`, `FNumber`,
> `ExposureTime`, `ISO`, `DateTimeOriginal` và `CameraSerialNumber` cũng như
> thông tin tham chiếu địa lý. `FocalLength` là dữ liệu mà Pix4D sử dụng để tính toán khoảng cách mẫu mặt đất — nếu thiếu nó
> thì quá trình tái tạo sẽ quay về một tỷ lệ sai lệch nghiêm trọng (trong một trường hợp đo đạc, khu vực 411 m
> đã biến thành 47,8 km). Bản sao này cố ý không phải là `-all:all`: các thẻ cấu trúc của IFD0 làm gián đoạn
> đầu ra của LATTICE, và `ExifImageWidth`/`Height` bị loại trừ vì chúng mô tả quá trình
> thu thập nguồn thay vì raster đã xuất.

Các cờ phụ ở giai đoạn thu thập(áp dụng cho các mức đo bức xạ — `radiance`, `reflectance`, `display`):

| Cờ | Mặc định | Ý nghĩa |
| --- | --- | --- |
| `apply_calibration` | `True` | DSNU + hiệu chỉnh trường phẳng + tách thành phần 3x3 + thang đo phóng xạ NIST. |
| `apply_white_balance` | `True` | Bảng tra đổi (LUT) cân bằng trắng (WB). Hỗ trợ DLS khi thiết bị thu thập dữ liệu (DAQ) được kết nối với camera. |
| `apply_index` | `False` | Đánh giá chỉ số thực vật. |
| `index_expression` | `None` | Công thức ghi đè. Không trống → tự động kích hoạt chỉ số. |
| `annotated` | `False` | Lớp phủ các hiệu ứng giao diện người dùng (zebra/grid/peaking). Không khả dụng cho `raw`. |

### `ArrayHandle`

```python
arr = proj.arrays["main_rig"]

# Single synced capture group
files = arr.capture("./out", format="tiff", processing="reflectance")
# → {"213800234": "/path/to/x.tif", "214000533": "/path/to/y.tif", ...}

# Multi-level: each serial's value becomes an ordered LIST, not a str
files = arr.capture("./out", processing="all")
# → {"213800234": ["/raw.tif", "/debayered.tif", ...], "combined": "/idx.tif"}

# Smart capture (wait for AE to settle)
result = arr.capture_smart(
    "./out", processing="reflectance",
    settle_timeout_s=5.0,
    stability_window_s=1.5,
    exposure_tolerance_pct=5.0,
)
print(result["frames"], result["settle"])

# In-memory grab: {serial: numpy array}
frames = arr.grab(processing="debayered")
frames = arr.grab(processing="radiance", with_metadata=True)

# Stream-to-disk loop
arr.stream(count=60, output_dir="./stream", fps=5, processing="raw")

# Frame-iterator (tolerates per-cam drops; great for downstream analysis pipelines)
for frames in arr.frame_stream(processing="radiance", fps=5, count=100):
    if "213800234" in frames:
        my_analysis_pipeline(frames["213800234"])

# Preview iterator (live MJPEG-equivalent; tolerates partial cycles)
counts = arr.preview_stream("./preview", fps=3.0, duration=30.0)
print(counts)  # frames written per serial
```

> **Kiểu trả về là `CapturePathMap`, không phải là `Dict[str, str]`.**
> `chloros_sdk.CapturePathMap` là `Dict[str, Union[str, List[str]]]`: một cấp độ duy nhất
> `processing` gán cho mỗi chuỗi một đường dẫn, trong khi loại nhiều cấp (`"all"`, hoặc một
> danh sách `levels` rõ ràng) cung cấp cho nó **danh sách có thứ tự** của mọi sản phẩm được lưu cho
> máy ảnh đó. Một hình ảnh tổng hợp trực tiếp, nếu có đang phát trực tuyến, sẽ xuất hiện dưới khóa bổ sung
> `"combined"` thay vì dưới một số sê-ri. Mã nguồn giả định `str` sẽ gặp lỗi khi gặp
> dạng danh sách mà không có trình kiểm tra kiểu nào phản đối — chú thích đã ghi là `Dict[str, str]`
> trong một thời gian sau khi dạng danh sách được phát hành, đó là lý do tại sao biệt danh này tồn tại. Chuẩn hóa
> khi bạn muốn dạng phẳng:
>
> ```python
> paths = arr.capture(processing="all")
> flat = [p for v in paths.values()
>         for p in (v if isinstance(v, list) else [v])]
> ```

### Căn chỉnh mảng

`ArrayHandle` hiển thị toàn bộ bề mặt căn chỉnh. Các hồ sơ chỉ tồn tại trong phiên làm việc theo mặc định — hãy gọi `export_alignment()` một cách rõ ràng để lưu trữ.

```python
from chloros_sdk import AlignmentSpec

arr = proj.arrays["main_rig"]

# Defaults: ORB / affine / one synced snapshot — same as the GUI's auto-cal
result = arr.calibrate_alignment()
print(result["profile"]["rms_residual_px"])

# Custom spec for tough scenes (low-contrast canopy)
spec = AlignmentSpec(
    method="feature_orb",         # feature_orb / feature_akaze / phase_correlation / checkerboard / manual
    model="rigid",                # translation / rigid / affine / homography
    num_frames=5,
    max_features=8000,
    ratio_threshold=0.7,
    ransac_threshold_px=2.0,
    min_matches=30,
    max_reproj_err_px=2.0,
)
arr.calibrate_alignment(spec)

# Or tweak one knob at a time
arr.calibrate_alignment(num_frames=3, model="affine")

# Inspect / manipulate
status = arr.alignment_status()
arr.tweak_alignment("214701292", dx=2.5, dy=-1.0, rotation_deg=0.0, scale=1.0)
arr.export_alignment("/tmp/main_rig_alignment.json")
arr.import_alignment("/tmp/main_rig_alignment.json", validate=True)
arr.clear_alignment()
```

#### Căn chỉnh tại thời điểm kết nối

`connect_all(align=...)` có thể tự động căn chỉnh mọi mảng tại thời điểm kết nối:

```python
# Align every array with defaults
proj.connect_all(align=True)

# Per-array control
proj.connect_all(align={
    "main_rig": AlignmentSpec(num_frames=5, model="affine"),
    "side_rig": True,             # use defaults
    "verify_rig": False,          # skip
})
```

Sẽ chuyển sang sử dụng `project.json["config"]["auto_align_on_connect"]` nếu không được chỉ định.

### `SensorHandle`

```python
spectrum = proj.sensors["Sky"].read()
# (spectrum_list, is_saturated, integration_time, x, y, z) — matches the
# daq_sdk add_spectrum_callback signature.
```

---

## Phần cứng trực tiếp (Không cần backend)

Khi bạn muốn không phụ thuộc vào backend (CI, robot không giao diện người dùng, nhúng), hãy nhập trực tiếp `lattice_sdk` và `daq_sdk` — cả hai đều đượcbởi `chloros_sdk`. Lưu ý về `CAMERA_AVAILABLE` / `DAQ_AVAILABLE`: `lattice_sdk` có trong gói PyPI (nhưng cần có môi trường chạy Arena SDK), trong khi `daq_sdk` chỉ được cung cấp cùng với bản cài đặt trên máy tính để bàn.

```python
from chloros_sdk import (
    # cameras
    LatticeCamera, CameraSettings, PRESETS, CameraPool,
    Calibration, CalibrationCoefficients, FilterModel, list_filters,
    DLS, NetworkDiagnostics, gpu_info, gpu_available,
    # discovery
    discover_cameras, discover_cameras_via_backend,
    # exceptions
    LatticeError, CameraNotFoundError, StreamError, CaptureError,
    CalibrationError, NetworkError, DLSError,
)

# Find a camera and capture in one go
cams = discover_cameras(timeout_ms=3000)
print(cams)

settings = PRESETS["high_quality"]
with LatticeCamera(serial="213800234", settings=settings) as cam:
    result = cam.capture(output_dir="./out", format="tiff")
    print(result.filepath, result.width, result.height)
```

##### Các cài đặt sẵn và cơ chế kích hoạt

Ba trong số bốn cài đặt sẵn là chế độ **chạy tự do**: máy ảnh phơi sáng liên tục và
`capture()` trả về khung hình tiếp theo. `triggered` là trường hợp ngoại lệ — nó kích hoạt
máy ảnh khi có tín hiệu cạnh phần cứng trên Dòng 2, do đó sẽ không ghi lại gì cho đến khi tín hiệu đó xuất hiện.

| Cài đặt sẵn | Cơ chế kích hoạt | Sử dụng khi |
| --- | --- | --- |
| `default` | free-run | sử dụng chung |
| `high_speed` | chế độ tự do | 8-bit, giới hạn 60 khung hình/giây, phơi sáng ngắn |
| `high_quality` | chế độ tự do-run | 12-bit, không giới hạn fps — lựa chọn thông thường cho ảnh tĩnh |
| `triggered` | **armed, Line 2** | máy ảnh được kết nối qua cáp đồng bộ M8 và một thiết bị khác kích hoạt nó |

Nếu bạn chọn `triggered` (hoặc tự thiết lập `trigger_mode="On"`) mà không có gì
điều khiển Dòng 2, mọi `capture()` sẽ hết thời gian chờ — điều này là chính xác, vì bạn đã yêu cầu
máy ảnh chờ. Thông báo lỗi &quot;SDK&quot; sẽ giải thích điều này khi nó xảy ra; xem
[SC_ERR_TIMEOUT trong quá trình chụp](#direct-hardware-backend-free).

> **Lưu ý — &quot;Thông báo &#x27;GVSP probe&#x27; / `SC_ERR_TIMEOUT -1011` khi kết nối không phải là lỗi.**&gt; Khi kết nối, SDK sẽ cố gắng thương lượng**khung jumbo** (gói tin GVSP 9000 byte) để đạt thông lượng cao hơn. Trên kết nối NIC điểm-điểm trực tiếp (ví dụ: địa chỉ `169.254.x.x` cục bộ trên liên kết), mạng thường không hỗ trợ khung jumbo, do đó quá trình kiểm tra này sẽ hết thời gian chờ và ghi lại các dòng như:
>
> ```
> [Network] GVSP probe: unexpected error (TimeoutError: ... SC_ERR_TIMEOUT -1011)
> [Network] GVSP probe at 9000 did not deliver a complete buffer; reverting to ICMP-chosen size
> [Network] GVSP packet size: 1500 bytes (standard)
> ```
>
> Đây là **phương án dự phòng theo thiết kế**: Bộ điều khiển mạng (SDK) tự động chuyển về các gói tin tiêu chuẩn 1500 byte và camera tiếp tục kết nối bình thường (các dòng `[chunk-enable …]` tiếp theo là một phần của chuỗi kết nối thông thường). Chức năng chụp ảnh vẫn hoạt động bình thường.
>
> Bạn có thể bỏ qua lệnh kiểm tra này, nhưng **nó không chỉ đơn thuần là công cụ làm im lặng nhật ký — nó còn tắt hoàn toàn tính năng khung jumbo.** Camera chỉ trả lời các gói ping &quot;Don&#x27;t-Fragment&quot; có kích thước tối đa 1500 byte, bất kể mạng của bạn tốt đến đâu, do đó chỉ riêng thử nghiệm ping không bao giờ phát hiện được khung jumbo; lệnh kiểm tra này là cách duy nhất có thể làm được điều đó. Vô hiệu hóa nó và camera sẽ chạy các gói tin tiêu chuẩn 1500 byte vĩnh viễn, trên bất kỳ mạng nào:
>
> ```bash
> CHLOROS_GVSP_PROBE_FALLBACK=0   # gives up jumbo — see the warning it prints
> ```
>
> Chỉ đáng để sử dụng trên mạng mà bạn *biết chắc* không hỗ trợ gói jumbo, nơi nó giúp tiết kiệm khoảng một giây thời gian kết nối cho mỗi camera. Vì đây là sự đánh đổi thực sự chứ không phải chỉ mang tính hình thức, nên trang SDK hiện sẽ hiển thị thông báo này khi bạn sử dụng tính năng này:
>
> ```
> [Network] ⚠️ GVSP probe disabled (CHLOROS_GVSP_PROBE_FALLBACK=0) — staying at
> 1500 bytes, jumbo NOT tested. … if this network does carry it, you are giving
> up ~1.45x wire ceiling. Unset the variable to test for jumbo.
> ```
>
> **Đừng thay đổi cài đặt này trừ khi bạn có lý do chính đáng.** Nếu để chế độ này được bật, mỗi lần kết nối sẽ tự động đo lại mạng thực tế mà bạn đang sử dụng: khi kết nối với một bộ chuyển mạch hỗ trợ gói tin jumbo, lần kết nối tiếp theo sẽ tự động kích hoạt chế độ jumbo mà không cần cấu hình thêm hay khởi động lại.
>
> Nếu bạn *muốn* đạt thông lượng jumbo, hãy kích hoạt chế độ jumbo từ đầu đến cuối (MTU NIC 9000 + bộ chuyển mạch hỗ trợ gói jumbo), hoặc cố định kích thước bằng `CHLOROS_GVSP_PACKET_SIZE_FORCE=9000` khi bạn chắc chắn liên kết hỗ trợ nó — tuy nhiên, nên ưu tiên sử dụng lệnh `CHLOROS_GVSP_PACKET_SIZE_FORCE=9000 python …` cho từng lệnh thay vì thiết lập vĩnh viễn, vì kích thước được cố định sẽ bỏ qua quá trình kiểm tra và ngừng thích ứng với mạng phía trước. **Mọi** thiết bị trên đường truyền đều phải hỗ trợ gói jumbo — bao gồm cả bộ chia hoặc bộ cấp nguồn PoE, đây thường là lý do khiến một hệ thống vốn có khả năng hỗ trợ gói jumbo lạikhông thể truyền gói jumbo.

> **Lỗi `SC_ERR_TIMEOUT -1011` xảy ra trong quá trình `capture()` / `grab*()` là một vấn đề khác — đó là lỗi thực sự.**&gt; Lưu ý trên chỉ liên quan đến `-1011` được ghi lại bởi**cảm biến thời gian kết nối**. Nếu cùng một lỗi xuất hiện từ một**lệnh capture**, điều đó có nghĩa là camera đã kết nối thành công nhưng không gửi bất kỳ hình ảnh nào:
>
> ```
> File ".../lattice_sdk/camera.py", line ..., in grab_frame_with_metadata
>   buffer = self._get_buffer(timeout)
> lattice_sdk.exceptions.CaptureError: Capture failed: ... SC_ERR_TIMEOUT -1011
> ```
>
> Dấu hiệu nhận biết là camera có kênh *điều khiển* hoạt động bình thường — quá trình phát hiện thành công, các thiết lập và các lệnh ghi `[chunk-enable …]` đều thành công — trong khi *mọi* khung hình đều bị hết thời gian chờ.
>
> **Nguyên nhân thường gặp là camera được kích hoạt bằng tín hiệu kích hoạt phần cứng.** Với `trigger_mode="On"` và `trigger_source="Line2"`, camera sẽ không phát ra bất kỳ tín hiệu nào cho đến khi có tín hiệu điện xuất hiện trên cáp đồng bộ M8. Nếu không có cáp nào truyền tín hiệu trên đường dây đó, mọi lệnh grab sẽ chờ mãi mãi. Camera không bị hỏng và mạng hoạt động bình thường — nó đang thực hiện chính xác những gì được yêu cầu.
>
> `CameraSettings()` và các cài đặt sẵn `default` / `high_speed` / `high_quality` được cài đặt sẵn chế độ chạy tự do, và một lần chụp bị hết thời gian chờ trong khi đang ở chế độ sẵn sàng sẽ hiển thị thông báo giải thích thay vì chỉ hiển thị mã `-1011`. `PRESETS["triggered"]` kích hoạt Line2, theo thiết kế.
>
> Để buộc bất kỳ camera nào chạy tự do:
>
> ```python
> settings = PRESETS["high_quality"]
> settings.trigger_mode = "Off"        # free-run; don't wait for an M8 edge
> ```
>
> Nếu vẫn hết thời gian chờ với lệnh `trigger_mode="Off"`, có nghĩa là camera thực sự không truyền dữ liệu — hãy gửi cho chúng tôi tệp nhật ký và lệnh `ip link show`.

#### Cài đặt màu (xem trước trực tiếp RGB) — `set_color_profile`

`LatticeCamera.set_color_profile(profile, custom_cct_k=None)` chọn cấu hình màu hiển thị cho **xem trước trực tiếp** trên camera RGB (camera đa phổ sẽ bỏ qua cài đặt này):

| Hồ sơ | Ý nghĩa |
| --- | --- |
| `raw` | Bỏ qua hoàn toàn chuỗi đo quang. |
| `linear` | DSNU + flat + WB, không có CCM, không có gamma. |
| `natural` | Linear + CCM đo được + gamma sRGB, chỉ với hiệu ứng hoàn thiện cơ bản (làm mịn độ bão hòa màu + giảm bão hòa vùng sáng) — cài đặt mặc định thực tế. |
| `enhanced` | `natural` cộng với hiệu ứng hoàn thiện parity đầy đủ (loại bỏ viền màu, độ sống động, độ tương phản cục bộ CLAHE). Hình ảnh phong phú hơn với **chi phí xử lý mỗi khung hình gấp đôi**, do đó tốc độ khung hình trực tiếp (LIVE) sẽ thấp hơn. |
| `custom_temp` | `natural` nhưng cân bằng trắng (WB) được cố định ở `custom_cct_k` Kelvin (bỏ qua DLS; giới hạn trong khoảng 2000–10000 K ở phía sau). |

Cài đặt này là một công cụ điều chỉnh tốc độ/hình ảnh **chỉ dành cho chế độ xem trước trực tiếp**: các bản ghi được lưu luôn có chất lượng hoàn thiện đầy đủ và phong phú bất kể cài đặt nào được chọn, vì vậy việc chọn `natural` để tiết kiệm thời gian khung hình sẽ không làm giảm chất lượng của dữ liệu được lưu vào đĩa. Một cấu hình không xác định sẽ tăng giá trị `ValueError`; khi backend chloros có thể truy cập được, thay đổi này cũng sẽ được POST đến backend đó để khung hình xem trước tiếp theo phản ánh thay đổi này (người dùng direct-SDK không có backend vẫn nhận được sự thay đổi cài đặt).

```python
with LatticeCamera(serial="214701292") as cam:   # RGB cam
    cam.set_color_profile("enhanced")            # richer look, lower LIVE fps
    cam.set_color_profile("custom_temp", custom_cct_k=5600)
```

#### Máy ảnh đơn sắc (M3M) và `Calibration`

Máy ảnh đơn sắc **M3M** (`M3M-<lens>-F<wavelength>`) là loại đơn dải: một mặt phẳng xám, không có mô-zaic Bayer, không có ma trận nhiễu chéo quang phổ 3×3. `Calibration` nhận diện được điều này và cung cấp cờ `is_mono`. Độ phản xạ vẫn được áp dụng dưới dạng bản đồ phóng xạ theo từng dải (ma trận tách thành phần là ma trận đơn vị), nhưng các phép toán đa dải trên một máy ảnh sẽ cho kết quả chính xác thay vì trả về giá trị vô nghĩa:

```python
from chloros_sdk import Calibration, CalibrationError

calib = Calibration("M3M-L87-F685")
print(calib.is_mono)        # True  (False for any M3C / RGN Bayer cam)
print(calib.filter_type)    # 'mono'  (sentinel; not a real crosstalk key)

# NDVI needs two bands (Red + NIR); one mono band can't supply both.
try:
    calib.compute_ndvi(reflectance_frame)
except CalibrationError as e:
    print(e)   # "...single-band mono (M3M) camera. Combine multiple..."
```

Để xây dựng chỉ số thực vật từ phần cứng đơn sắc, hãy kết hợp nhiều camera M3M ở các bước sóng khác nhau thành một chồng hình đa dải đã được căn chỉnh (xem [Căn chỉnh mảng](#array-alignment)) và tính toán chỉ số trên chồng hình đó thay vì trên một camera duy nhất.

Chế độ trực tiếp DAQ:

```python
from chloros_sdk import (
    DAQUSensor, DAQMSensor, DAQESensor,
    SensorFleet, discover_all, DiscoveredSensor,
    apply_sensor_settings, SensorSettings,
)

for d in discover_all(timeout=3.0):
    print(d)

sensor = DAQUSensor(port="COM3")
sensor.connect()
apply_sensor_settings(sensor, settings={"integration_time_ms": 64, "frame_avg": 20})
sensor.start_streaming()
# ... sensor.add_spectrum_callback(your_callback) ...
sensor.stop()
```

> **Các khóa được chấp nhận cho `apply_sensor_settings`**— chính xác là `integration_time_ms`, `frame_avg`, `ae_enabled`, `sunshine_diffuser_installed` (DAQ-E; đã bị loại bỏ để chuyển sang sử dụng `cap_id`), `filter_model` (DAQ-M), và `cap_id` (tất cả các loại DAQ; `None`/`""`/`"none"` = cảm biến trần, không điều chỉnh nắp). Các khóa không xác định sẽ bị**bỏ qua một cách im lặng** — ví dụ: `{"integration_time": 64}` không thực hiện gì cả (phải là `integration_time_ms`). Trả về `{"applied": [...], "errors": {...}}` và không bao giờ gây ra ngoại lệ.

`chloros_sdk` chỉ tái xuất bề mặt lõi được sử dụng ở trên. Giao diện công khai đầy đủ `daq_sdk` API (22 tên) bổ sung các thành phần sau — hãy nhập chúng trực tiếp từ `daq_sdk`:

```python
from daq_sdk import (
    DAQULogger, DAQMLogger, DAQELogger,     # rotating-file recorders (the ones the GUI uses)
    ConnectResult, FleetRecordResult,       # SensorFleet result types
    discover_all_detailed, build_sensor,    # detailed discovery + build-by-descriptor
    scan_eth_devices, DaqEControl,          # DAQ-E Ethernet scan + control channel
    scan_ble_devices, detect_ble_device, list_ble_devices,   # DAQ-M BLE discovery
    detect_port, list_serial_ports,         # DAQ-U serial-port discovery
    TcpSerial,                              # serial-over-TCP transport shim
)
```

---

## Các trường hợp ngoại lệ

Bắt lớp cơ sở để xử lý &quot;bất kỳ sự cố nào xảy ra trong Chloros&quot;:

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

> `ChlorosAuthenticationError` và `ChlorosConfigurationError` được xuất ở cấp cao nhất cùng với các thành phần còn lại; chúng cũng có thể được nhập từ `chloros_sdk.exceptions` như đã trình bày.

Cấu trúc phân cấp:

```

ChlorosError
├── ChlorosBackendError           (backend failed to start / unreachable)
├── ChlorosConnectionError        (HTTP transport failure)
├── ChlorosLicenseError           (subscription / tier gate)
├── ChlorosAuthenticationError    (login required)
├── ChlorosConfigurationError     (bad configure() / open_project() inputs)
└── ChlorosProcessingError        (pipeline failed)

ChlorosConnectError                (raised by connect_camera / connect_array /
                                    connect_daq_sensor only — derives from
                                    plain Exception, NOT from ChlorosError,
                                    so `except ChlorosError` will not catch it)

lattice_sdk exceptions:
LatticeError
├── CameraNotFoundError
├── CameraConnectionError
├── StreamError
├── CaptureError
├── CalibrationError
├── NetworkError
└── DLSError
```

---

## Ví dụ từ đầu đến cuối

### 1. Xử lý một thư mục với thanh tiến trình tùy chỉnh

```python
from chloros_sdk import ChlorosLocal

def progress(percent, message):
    bar = "#" * (percent // 5)
    print(f"\r[{bar:<20s}] {percent:3d}% {message}", end="", flush=True)

with ChlorosLocal() as cl:
    cl.create_project("FieldA_2026-05-26")
    cl.import_images("C:/DroneImages/Flight001", recursive=True)
    cl.configure(
        debayer="High Quality (Faster)",
        vignette_correction=True,
        reflectance_calibration=True,
        indices=["NDVI", "NDRE", "GNDVI", "SAVI"],
        export_format="TIFF (16-bit)",
    )
    cl.process(progress_callback=progress)
print()
```

### 2. Mảng LATTICE trực tiếp → Độ phản xạ + Tham chiếu DAQ

```python
import chloros_sdk

# Open a paired sensor first so the array's reflectance step has an
# absolute reference. Smart-detect picks USB / BLE / ETH automatically.
with chloros_sdk.connect_daq_sensor() as daq:
    with chloros_sdk.connect_array([
            "213800234", "214000533", "214701288", "214701292"
    ]) as arr:
        # Smart capture: wait for AE to settle, then snap
        arr.capture("./out", processing="reflectance", smart=True)

        # Record the corresponding DAQ frames as ground truth
        daq.record_start(output_dir="./out", device_name="sky-reference")
        # ... do whatever capture campaign ...
        info = daq.record_stop()
        print(info["path"], info["rows"])
```

### 3. Chiến dịch thu thập dữ liệu theo dự án

```python
import time, chloros_sdk

with chloros_sdk.open_project("/home/user/Chloros Projects/Field_A") as proj:
    report = proj.connect_all(verbose=True, align=True)
    if report["arrays"]["errors"]:
        raise SystemExit(f"Array(s) failed to connect: {report['arrays']['errors']}")

    rig = proj.arrays["main_rig"]

    # Re-align right before the campaign
    rig.calibrate_alignment(num_frames=5)
    rig.export_alignment("./alignments/main_rig.json")

    # 50 sequential single-frame captures at 2 fps
    for i in range(50):
        frames = rig.capture(
            output_dir=f"./out/frame_{i:04d}",
            processing="reflectance",
            apply_calibration=True,
            apply_white_balance=True,
        )
        time.sleep(0.5)

    # End-of-day: process the captured folder. process() accepts only
    # mode/wait/progress_callback/poll_interval — indices come from the
    # project's saved config (or set them via ChlorosLocal.configure()).
    proj.process()
```

### 4. Dòng khung hình từ nhiều camera → Quy trình xử lý NumPy

```python
import chloros_sdk
import numpy as np

with chloros_sdk.open_project("/path/to/proj") as proj:
    proj.connect_all()
    rig = proj.arrays["main_rig"]

    for frames in rig.frame_stream(
            processing="radiance",
            fps=5.0, count=300,
            apply_calibration=True,
            apply_white_balance=True):
        # frames is {serial: numpy_array}; cams not delivering this tick are omitted
        for serial, frame in frames.items():
            print(serial, frame.shape, frame.dtype, frame.mean())
```

### 5. Tập lệnh thu thập dữ liệu trực tiếp trên phần cứng (không có hệ thống nền)

```python
from chloros_sdk import LatticeCamera, PRESETS, discover_cameras

cams = discover_cameras(timeout_ms=3000)
print(f"Found {len(cams)} cams")

settings = PRESETS["high_quality"]
for c in cams:
    with LatticeCamera(serial=c.serial, settings=settings) as cam:
        result = cam.capture(output_dir="./out", format="tiff")
        print(c.serial, result.filepath)
```

### 6. Kiểm tra khả năng trước khi kết nối mảng 4 camera

```python
import chloros_sdk

serials = ["214701288", "213800234", "214000533", "214701162"]

probe = chloros_sdk.analyze_array_network(
    master_serial=serials[0],
    slave_serials=serials[1:],
    width=2048, height=1536,
    pixel_format="BayerRG12",
)

if probe["status"] == "ok":
    arr = chloros_sdk.connect_array(
        serials, width=2048, height=1536, pixel_format="BayerRG12")
elif probe["status"] == "auto_capped_fps":
    r = probe["recommended"]
    print(f"Keeping resolution; capping trigger rate at "
          f"{r['recommended_target_fps']} fps")
    arr = chloros_sdk.connect_array(
        serials, width=2048, height=1536, pixel_format="BayerRG12",
        target_fps=r["recommended_target_fps"])
elif probe["status"] == "auto_shrunk":
    r = probe["recommended"]
    print(f"Auto-shrinking to {r['out_width']}x{r['out_height']} "
          f"binning={r['binning']} for sim-sync")
    arr = chloros_sdk.connect_array(
        serials,
        width=r["out_width"], height=r["out_height"],
        pixel_format=r["pixel_format"], binning=r["binning"])
elif probe["status"] == "needs_force_slip":
    print("Wire can't sustain sim-sync; falling back to slip mode")
    arr = chloros_sdk.connect_array(
        serials, force_tier="slip-emit-and-capture")
else:
    raise RuntimeError(f"Probe error: {probe.get('error')}")
```

### 7. Công thức ghi hình tương đương (Python thuần túy)

Ngôn ngữ kịch bản công thức (recipe DSL) của Bộ công cụ ghi hình thông minh (CLI) có phiên bản tương đương trực tiếp trên Python:

```python
import time, chloros_sdk

with chloros_sdk.open_project("/path/to/proj") as proj:
    proj.connect_all()
    cam = proj.cameras["FrontLeft"]
    rig = proj.arrays["main_rig"]
    sky = proj.sensors["Sky"]

    # apply
    # (CameraHandle has no direct apply method; use the underlying lattice_sdk
    #  helper or the backend's /api/camera/<sn>/apply-settings via requests)
    # For most cases just use cam.cam.set_exposure(...) in direct mode or
    # the GUI's saved settings via project.connect_all().

    # wait
    time.sleep(2)

    # capture
    cam.capture("pose_a/", format="tiff", processing="radiance")

    # stream
    rig.stream(count=60, fps=5, output_dir="stream/", processing="raw")

    # sensor read
    print(sky.read())
```

---

## Tự động khởi động backend

Các điểm vào smart-connect — `connect_camera`, `connect_array`, `connect_daq_sensor` và `discover_lattice_cameras` — là các máy khách mỏng HTTP giả định rằng một backend đang lắng nghe trên `127.0.0.1:5000` (URL mặc định của giao diện Smart-Connect). Khi giao diện người dùng đồ họa (GUI) hoặc CLI đã đang chạy, thì máy khách này cũng đang chạy. Từ một tập lệnh trống, có thể không có máy khách nào đang chạy — do đó, các hàm này **tự động khởi động tệp nhị phân máy chủ đi kèm** (chế độ không cửa sổ, giống như cách `ChlorosLocal` thực hiện) trước lần gọi đầu tiên, sau đó chờ tối đa `backend_startup_timeout` cho đến khi nó khởi động xong.

Quy tắc:

- **Chỉ có một URL cục bộ được khởi tạo.** Một `backend_url` trỏ đến `localhost` / `127.0.0.1` / `[::1]` là hợp lệ; bất kỳ máy chủ nào khác đều được coi là máy của người khác và sẽ không bao giờ được khởi tạo.
- **Backend sẽ được giữ chạy để tái sử dụng** (giống như CLI) — không có quá trình tắt máy ngầm định khi skript của bạn kết thúc. Chạy lại skript sẽ tái sử dụng backend đang hoạt động.
- **Từ chối tham gia bằng cách sử dụng `auto_start_backend=False`** trong bất kỳ lệnh gọi nào trong số đó (ví dụ: khi bạn trỏ đến một backend từ xa, hoặc bạn tự quản lý vòng đời của backend).

```python
import chloros_sdk

# Fresh shell, no backend running, no GUI open — this still works:
with chloros_sdk.connect_camera("213800234") as cam:   # spawns the backend
    cam.capture("output/")

# Remote backend (via tunnel — see Remote-Backend Mode): don't spawn one locally
arr = chloros_sdk.connect_array(serials,
                                backend_url="http://127.0.0.1:5000",
                                auto_start_backend=False)
```

Nếu không thể tìm thấy hoặc khởi động tệp nhị phân đi kèm, lệnh HTTP tiếp theo sẽ gây ra một lỗi `ChlorosConnectError` có thể xử lý được và **phù hợp với nền tảng** thay vì chỉ hiển thị thông báo kết nối bị từ chối — trên Windows, thông báo này sẽ hướng dẫn bạn đến ứng dụng trên máy tính để bàn hoặc lệnh `chloros-cli`; trên Linux (không có giao diện người dùng đồ họa), thông báo này sẽ hướng dẫn bạn đến lệnh `chloros-cli` hoặc `.deb`.

---

## Môi trường &amp; Đầu vào

SDK đánh dấu mọi cuộc gọi đến backend HTTP bằng `X-Chloros-Client: sdk`. Backend áp dụng các quy tắc cấp phép SDK / CLI (yêu cầu đăng nhập **và** gói trả phí Chloros+) thay vì lộ trình miễn phí của giao diện đồ họa. Cài đặt này được thiết lập tự động khi nhập dữ liệu — bạn không cần thực hiện bất kỳ thao tác nào.

`http://localhost` và `http://127.0.0.1` được phát hiện là backend cục bộ. Các yêu cầu đến các máy chủ khác (ví dụ: dịch vụ phân tích của riêng bạn) sẽ không bị ảnh hưởng.

Ghi đè backend URL bằng cách truyền `backend_url=` (hoặc `api_url=` trên `ChlorosLocal`):

```python
chloros_sdk.connect_camera("213800234", backend_url="http://127.0.0.1:5000")
chloros_sdk.connect_array(serials, backend_url="http://127.0.0.1:5000")
chloros_sdk.connect_daq_sensor(eth_host="daq-e-1.local",
                                backend_url="http://127.0.0.1:5000")
chloros_sdk.ChlorosLocal(backend_url="http://127.0.0.1:5000")
```

(Một địa chỉ `backend_url` không phải loopback chỉ kết nối đến backend nguồn/thiết bị — các backend được cung cấp sẵn chỉ liên kết với loopback; xem Chế độ Backend Từ xa để biết mẫu đường hầm.)

---

## Phiên bản &amp; Tương thích

- Phiên bản SDK được hiển thị dưới dạng `chloros_sdk.__version__`.
- SDK giới hạn hành vi theo phiên bản backend đi kèm. Việc kết hợp một SDK cũ hơn với một backend mới hơn thường hoạt động (các điểm cuối tương thích ngược), nhưng việc kết hợp một SDK mới hơn với một backend cũ hơn có thể gây ra lỗi `404` trên các điểm cuối mới — hãy nâng cấp ứng dụng máy tính để bàn cho phù hợp.
- Bề mặt smart-connect (`connect_camera` / `connect_array` / `connect_daq_sensor`) và điểm cuối phân tích mạng trả về các lược đồ JSON ổn định; các trường mới được bổ sung.

---

## Hướng dẫn khắc phục sự cố

- **`ChlorosAuthenticationError: Login required`** → Chạy `chloros-cli login EMAIL PASSWORD` một lần trên máy này, hoặc đăng nhập qua ứng dụng máy tính để bàn Chloros.
- **`ChlorosConnectError: No Chloros backend is running …`** → Các lệnh smart-connect tự động khởi động một backend cục bộ, do đó thông báo này chỉ xuất hiện khi không thể tìm thấy hoặc khởi động được tệp nhị phân đi kèm (ví dụ: máy chủ chỉ sử dụng pip mà không có gói ứng dụng desktop). Thông báo này tùy thuộc vào nền tảng: trên Windows, hãy mở ứng dụng máy tính để bàn hoặc chạy bất kỳ lệnh `chloros-cli` nào; trên Linux, hãy chạy lệnh `chloros-cli` (không có giao diện đồ họa) hoặc cài đặt `.deb`. Đối với máy chủ từ xa, truyền tham số `backend_url=` (và `auto_start_backend=False`).
- **`CAMERA_AVAILABLE == False`** khi nhập → `lattice_sdk` không tải được (thường là do các DLL thời gian chạy Arena SDK chưa được cài đặt). Bề mặt không phải camera vẫn hoạt động bình thường.
- **Array connect trả về độ phân giải thấp hơn độ phân giải gốc**→ Chức năng smart-prep của backend tự động thu nhỏ kích thước khung hình để phù hợp với đường dây. Sử dụng `analyze_array_network()` để tìm hiểu nguyên nhân, sau đó nâng cấp liên kết, chấp nhận việc thu nhỏ hoặc chuyển sang `force_tier="slip-emit-and-capture"` để ghi hình tuần tự. Cơ chế an toàn thu nhỏ****không** bao gồm tình trạng quá tải tổng hợp (`oversubscribed: true`, các trường fps bằng 0): số lượng camera quá nhiều so với băng thông không thể khắc phục bằng cách gộp khung hình/ROI — hãy giảm số lượng camera, bật khung hình jumbo hoặc chuyển sang NIC nhanh hơn (xem [Quá tải](#over-subscription-the-per-cam-floor)).
- **`analyze_array_network()` báo cáo vòng nhận (RX ring) của NIC rất nhỏ (~0,26 MB) / các cổng kết nối hiển thị thông báo &quot;FRAMES WILL DROP&quot;** → Vòng nhận (receive ring) của NIC máy chủ đang ở mức mặc định (thường được đặt lại về 32 sau khi cập nhật trình điều khiển NIC). Trên bộ điều hợp Realtek USB 10GbE, hãy đặt `ReceiveBufferLen=256` và `PendingReceives=64` (mức cao), sau đó khởi động lại hệ thống nền để nó đọc lại vòng nhận. Quy trình đầy đủ: [CLI Tham khảo → Cài đặt &amp; Tối ưu hóa NIC chủ](cli-reference.md#host-nic-setup--tuning-lattice-arrays).
- **Máy chủ bị treo khi khởi động lại/tắt máy, sau đó xuất hiện lỗi WMI `Invalid class` / NIC không thể kích hoạt** → Trình điều khiển USB 10GbE lỗi thời gây ra lỗi `DRIVER_POWER_STATE_FAILURE` (màn hình xanh `0x9F`). Cập nhật trình điều khiển bộ điều hợp lên phiên bản mới nhất (≥ 2026) và áp dụng lại cài đặt vòng nhận. Xem [CLI Tài liệu tham khảo → Cài đặt và Tối ưu hóa NIC máy chủ](cli-reference.md#host-nic-setup--tuning-lattice-arrays).
- **Phản xạ bị từ chối** → Phải liên kết một DAQ đang hoạt động với camera (hoặc mảng cảm biến) để có được giá trị phản xạ theo thang đo tuyệt đối. Có thể liên kết qua giao diện người dùng (GUI) hoặc sử dụng `processing="radiance"` (W/m²/sr/nm), phương pháp này không yêu cầu cảm biến ghép đôi.
- **Quá trình ghi dữ liệu của `smart=True` mất nhiều thời gian hơn dự kiến** → Sự hội tụ AE phụ thuộc vào động lực học của cảnh; hãy tăng độ chặt chẽ của `exposure_tolerance_pct` hoặc giảm thời gian của `stability_window_s` nếu bạn muốn kích hoạt nhanh hơn (nhưng kém ổn định hơn).

---

## Xem thêm

- [Tham khảo CLI](cli-reference.md) — mỗi lệnh con CLI tương ứng với một lệnh gọi SDK.
- [Hướng dẫn cảm biến DAQ](../daq/README.md) — quy tắc kết nối, hiệu chuẩn và ghi dữ liệu cụ thể cho từng cảm biến.
- Tài liệu trực tuyến: `https://mapir.gitbook.io/chloros/api-python-sdk`</id></sn>
