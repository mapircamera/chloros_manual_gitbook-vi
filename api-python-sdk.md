# API: Python SDK

{% hint style="info" %}
**Bạn đang tìm kiếm API đầy đủ?** Trang này là một hướng dẫn thực hành. Mọi lớp công khai, phương thức, chữ ký chính xác và ví dụ có thể sao chép-dán đều có trong [Tài liệu tham khảo SDK](reference/sdk-reference.md), được tối ưu hóa cho các trợ lý AI.**Đang làm việc với trợ lý AI?** Hãy dán URL này vào khung trò chuyện để trợ lý có được Chloros API đầy đủ và mới nhất của Chloros 1.3.0:

`https://mapir.gitbook.io/chloros/reference/sdk-reference.md`

Mỗi trang của tài liệu hướng dẫn này đều có sẵn dưới dạng markdown thô tại đường dẫn slug viết thường + `.md`, và toàn bộ tài liệu được lập chỉ mục tại `https://mapir.gitbook.io/chloros/llms.txt`.
{% endhint %}

**Chloros Python SDK** (`chloros-sdk` trên PyPI) cho phép thực hiện mọi chức năng mà ứng dụng desktop có thể làm từ Python: xử lý hình ảnh hàng loạt, điều khiển camera và mảng LATTICE thời gian thực, các phiên thu thập dữ liệu (DAQ) từ cảm biến ánh sáng, và tự động hóa dự án đã lưu. Đây là một lớp mỏng phủ lên cùng một backend cục bộ mà GUI và CLI sử dụng (HTTP trên `127.0.0.1:5000`), do đó hành vi hoạt động giống hệt nhau trên cả ba giao diện.

## Cài đặt

SDK này đi kèm với gói Chloros dành cho máy tính để bàn, gói này cũng cung cấp nền tảng xử lý và các runtime phần cứng, đồng thời luôn tương thích về phiên bản.

**Bước 1 — Cài đặt Chloros.** Windows: chạy trình cài đặt máy tính để bàn (đường dẫn mặc định `C:\Program Files\Chloros\`) từ trang [Tải xuống](download.md). Linux: cài đặt gói `.deb` bằng `sudo apt install ./chloros_1.3.0_<arch>.deb` ([Cài đặt trên Linux](linux/linux-installation.md)).**Bước 2 — Kiểm tra SDK** (Python 3.7+). Thông thường không cần cài đặt gì thêm:

* **Windows:** trình cài đặt sẽ cài đặt gói SDK đi kèm vào hệ thống Python của bạn (nếu chưa cài đặt Python, bước này sẽ bị bỏ qua).
* **Linux:** `.deb` sẽ cài đặt SDK cho hệ thống `python3` vào `/usr/lib/python3/dist-packages`. Các yêu cầu của nó, `python3-numpy` và `python3-requests`, được gói phần mềm khuyến nghị: `apt install` cung cấp chúng, trong khi `dpkg -i` thì không.
* **Môi trường ảo (virtualenv)** không nhận diện được các gói phần mềm của hệ thống; hãy cài đặt tệp wheel đi kèm vào môi trường đó: `pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl` trên Linux, hoặc `chloros_sdk-*.whl` trong thư mục `sdk` của thư mục cài đặt Windows (mặc định là `C:\Program Files\Chloros\sdk\`).

```bash
python3 -c "import chloros_sdk; print(chloros_sdk.__version__)"   # Windows: py -c "…"
```

Trên máy không có gói Chloros, mà SDK của nó điều khiển một backend trên máy khác (xem [Những gì một cài đặt chỉ dùng pip nhận được](#what-a-pip-only-install-gets)), hãy cài đặt nó từ PyPI: `pip install chloros-sdk`. PyPI được cập nhật trên các bản dựng phát hành, do đó nó luôn khớp với phiên bản ổn định mới nhất.

**Bước 3 — Đăng nhập một lần trên mỗi máy:**

```bash
chloros-cli login user@example.com 'YourPassword'
```

Thông tin đăng nhập được lưu trong bộ nhớ đệm tại `~/.chloros/` (cả hai nền tảng). Trên Windows, bạn có thể đăng nhập tương tự thông qua tab Người dùng <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> của ứng dụng máy tính để bàn. SDK yêu cầu gói Chloros+ trả phí — xem [Yêu cầu về giấy phép](#license-requirement) bên dưới.

| Yêu cầu | Chi tiết |
| --- | --- |
| **Đã cài đặt Chloros** | Windows: trình cài đặt máy tính để bàn; Linux: gói `.deb` (cung cấp tệp nhị phân backend và SDK) |
| **Ngôn ngữ Python** | 3.7 trở lên (được phát triển/kiểm thử trên phiên bản 3.10) |
| **Hệ điều hành** | Windows 10/11 64-bit, Ubuntu 22.04 LTS trở lên, hoặc NVIDIA Jetson (JetPack 6) |
| **Giấy phép** | Tài khoản Chloros+ đang hoạt động, bất kỳ gói trả phí nào (Copper trở lên) |

## Hoàn thành trong 60 giây

Chỉ với một lệnh, bạn có thể tạo dự án, nhập thư mục, cấu hình xử lý và chạy quy trình — tự động khởi động phần nền nếu chưa có phiên bản nào đang chạy:

```python
import chloros_sdk

results = chloros_sdk.process_folder(
    "C:/DroneImages/Flight001",
    indices=["NDVI", "GNDVI", "OSAVI"],
)
```

(Trên Linux, sử dụng đường dẫn Linux: `/home/user/drone_images/flight001`. SDK hoạt động giống hệt nhau trên cả hai nền tảng.)

Đang xử lý thư mục dữ liệu chụp từ LATTICE? Hãy sử dụng trình bao bọc tương thích với LATTICE — nó sẽ áp dụng các thiết lập mặc định phù hợp (không phát hiện mục tiêu bảng điều khiển, thuật toán debayer tiêu chuẩn):

```python
results = chloros_sdk.process_lattice_capture(
    folder_path="C:/Captures/2026-05-13_Field",
    indices=["NDVI"],
)
```

## `ChlorosLocal` — kiểm soát toàn bộ quy trình xử lý

Đối với bất kỳ tác vụ nào phức tạp hơn một dòng lệnh, hãy sử dụng `ChlorosLocal`. Công cụ này sử dụng backend đang chạy sẵn (của ứng dụng desktop) hoặc khởi động backend đã cài đặt khi sử dụng lần đầu (`auto_start_backend=True`; trên Windows từ thư mục `backend` trong thư mục cài đặt Chloros, mặc định là `C:\Program Files\Chloros\backend\`), tạo và cấu hình các dự án, theo dõi tiến trình, và trả về bản tóm tắt sau khi chạy.

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
Giữ nguyên giá trị mặc định `http://127.0.0.1:5000` thay vì thay thế bằng `localhost` — trên hệ điều hành Windows, `localhost` sẽ được giải quyết thành `::1` trước tiên và tốn khoảng 2 giây cho mỗi yêu cầu khi truy cập vào backend chỉ hỗ trợ IPv4.
{% endhint %}

Sử dụng nó như một trình quản lý bối cảnh. Khi thoát khỏi khối, việc sử dụng máy chủ phía sau của phiên bản này sẽ kết thúc, chứ không phải máy chủ phía sau: máy chủ phía sau vẫn tiếp tục chạy cho tập lệnh tiếp theo và tự động dừng sau 30 phút không được sử dụng. `shutdown_backend()` sẽ dừng phiên bản này khi nó được khởi động.

```python
import chloros_sdk

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("FieldA_2026-05-26", camera="Survey3N_RGN")
    cl.import_images("C:/DroneImages/Flight001")
    cl.configure(
        vignette_correction=True,
        reflectance_calibration=True,
        indices=["NDVI", "GNDVI", "OSAVI"],
        export_format="TIFF (16-bit)",
    )
    results = cl.process(mode="parallel", wait=True)
print(results["summary"])
```

`configure()` chấp nhận các từ khóa sau: `debayer`, `vignette_correction`, `reflectance_calibration`, `indices`, `export_format`, `ppk`, `daq_log_path`, `input_level`, `radiometric_output`, `array_alignment`, `array_alignment_crop`, `array_alignment_interpolation` và `custom_settings`. Các giá trị chính:

```python
# export_format
"TIFF (16-bit)"           # default, recommended
"TIFF (8-bit)"            # 8-bit TIFF; what all-8-bit images get for a deeper format
"TIFF (32-bit, Percent)"  # reflectance percentage as float32
"PNG (8-bit)"
"JPG (8-bit)"

# debayer
"High Quality (Faster)"                  # standard, default
"Texture Aware (Slow, Highest Quality)"  # neural debayer, Chloros+ only
```

Các thông số điều chỉnh dành riêng cho LATTICE (`input_level`, `radiometric_output`, và nhóm `array_alignment*`) được ghi chép kèm theo bảng giá trị đầy đủ trong [Tài liệu tham khảo SDK](reference/sdk-reference.md#supported-values).

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

Khi hoàn tất, `process()` sẽ đính kèm bản tóm tắt xử lý của backend dưới dạng `result["summary"]`. Mỗi mục trong `summary["hints"]` là một câu hoàn chỉnh giải thích bất kỳ điều gì đáng chú ý — ví dụ: tại sao một lần chạy lại không tạo ra kết quả nào — và mọi gợi ý cũng được phát lại dưới dạng Python `UserWarning`, do đó các lần chạy trống có thể tự chẩn đoán ngay cả khi bạn không bao giờ kiểm tra từ điển:

```python
result = cl.process()
for hint in result.get("summary", {}).get("hints", []):
    print("HINT:", hint)
# hints also arrive on the warnings channel:
#   python -W always::UserWarning your_script.py
```

{% hint style="warning" %}
**`process()` không phát sinh ngoại lệ khi một lần chạy không tạo ra hình ảnh nào.** Đây là điểm duy nhất mà SDK và CLI có sự khác biệt có chủ đích: `chloros-cli process` coi &quot;đã yêu cầu sản phẩm nhưng không có sản phẩm nào được ghi&quot; là lỗi và thoát với giá trị không bằng 0, trong khi SDK trả về kết quả bình thường và báo cáo tình trạng này thông qua `summary` / các gợi ý. Nếu quy trình của bạn cần dừng lại khi có lần chạy trống, hãy tự kiểm tra — kiểm tra `summary` (hoặc đếm số tệp trong thư mục dự án) thay vì dựa vào ngoại lệ.
{% endhint %}

## Smart Connect — phần cứng trực tiếp

Ba hàm trợ giúp mở các phiên làm việc liên tục trong nhóm phần cứng của backend — cùng nhóm mà CLI, các skript khác và các tab camera của GUI sử dụng, do đó các skript SDK có thể hoạt động song song với chúng mà không phải cạnh tranh về cổng nối tiếp hay băng thông mạng. Cả ba đều tự động khởi động một backend cục bộ nếu chưa có backend nào đang chạy. Bảng điều khiển DAQ của GUI không thuộc nhóm này: hãy đóng cảm biến ánh sáng trong GUI trước khi mở nó từ tập lệnh.

### Máy ảnh LATTICE đơn — `connect_camera`

```python
import chloros_sdk

# Open by serial; reuses existing pool entry if one exists
with chloros_sdk.connect_camera("213800234") as cam:
    cam.set_settings(exposure_time=10000, gain=0.0)   # microseconds, dB
    cam.capture("output/")
```

{% hint style="info" %}
**Đường dẫn tương đối sẽ trỏ đến vị trí chạy skript của bạn (đã thay đổi trong phiên bản 1.3.0).** Với backend cục bộ, đường dẫn tương đối `output_dir` (bao gồm cả đường dẫn mặc định `"output"`) — và các đường dẫn khác mà backend mở (`burst_dir`, `profile_path`, `save_path`, `mask_path`) — sẽ được giải quyết dựa trên thư mục làm việc của tập lệnh của bạn, do đó `cam.capture("output/")` ghi vào `<current folder>/output/`. Trong phiên bản 1.2.0, một backend đã cài đặt giải quyết các đường dẫn này trong thư mục cài đặt riêng của nó và báo lỗi &quot;Truy cập bị từ chối&quot;. Đối với backend từ xa, các đường dẫn được gửi nguyên văn như đã nhập.
{% endhint %}

### Mảng đồng bộ — `connect_array`

`connect_array` là điểm vào được khuyến nghị cho các hệ thống nhiều camera. Nó chạy cùng quy trình chuẩn bị thông minh như giao diện người dùng (GUI): phân tích mạng, tự động chọn cấp độ đồng bộ, đồng bộ thời gian PTP, lựa chọn định dạng pixel cho từng camera, gieo giá trị AE và kích hoạt tín hiệu GPIO. **Cổng nối tiếp đầu tiên là thiết bị chủ** (nó phát xung kích hoạt phần cứng); các cổng còn lại là thiết bị phụ.

```python
with chloros_sdk.connect_array(
        ["213800234", "214000533", "214701288", "214701292"]) as arr:
    arr.capture("output/", processing="reflectance")
```

Thêm `smart=True` vào bất kỳ mảng chụp nào để chờ cho đến khi phơi sáng tự động ổn định trên tất cả các camera trước khi kích hoạt.

Mới trong phiên bản 1.3.0, `ArraySession` cũng cung cấp trực tiếp các khung hình đồng bộ mà không cần ghi tệp:

* `arr.grab()` trả về một khung hình cho mỗi camera, tất cả từ một lần kích hoạt, dưới dạng mảng numpy (`{serial: array}`); `.group_id` là lệnh kích hoạt chung và `.errors` chỉ định bất kỳ camera nào bị bỏ qua.
* `arr.frame_stream(fps=5.0)` trả về một bộ như vậy cho mỗi tick, để xử lý trực tiếp trong quy trình xử lý của riêng bạn.
* `arr.continuous("output/")` chạy chế độ ghi liên tục của chính backend (chế độ Liên tục trên GUI) và trả về một `ContinuousHandle` cùng với `status()`, `wait()` và `stop()` — hãy sử dụng nó thay cho vòng lặp ghi hình khi bạn cần lưu lại mọi tín hiệu kích hoạt với tốc độ cao.

Đối với các chế độ thu thập (Đơn lẻ / Liên tục / Theo khoảng thời gian / Nhanh nhất), bộ ghi, chế độ burst-to-video và căn chỉnh mảng, hãy tham khảo [Tài liệu tham khảo SDK](reference/sdk-reference.md#synchronized-array--arraysession-smart-prep).

### Cảm biến ánh sáng DAQ — `connect_daq_sensor`

Nếu không có đối số nào, `connect_daq_sensor()` sẽ tự động phát hiện giao thức truyền tải (thứ tự ưu tiên: Ethernet → BLE → USB):

```python
with chloros_sdk.connect_daq_sensor() as daq:    # smart-detect USB / BLE / ETH
    for frame in daq.latest(n=5):
        print(frame["spectrum"][:10])
```

Mỗi khung dữ liệu chứa giá trị 135 điểm `spectrum` (W/m²/nm khi đã hiệu chuẩn), cờ `is_saturated` và CIE `x`, `y`, `z`. Để chỉ định cụ thể một cảm biến hoặc giao thức truyền tải, hãy truyền một gợi ý rõ ràng:

```python
daq = chloros_sdk.connect_daq_sensor(transport="usb", port="COM3")
daq = chloros_sdk.connect_daq_sensor(mac="AA:BB:CC:DD:EE:FF")        # implies BLE
daq = chloros_sdk.connect_daq_sensor(eth_host="daq-e-xxx.local")     # implies Ethernet
```

Lưu ý rằng các cấu hình hiệu chỉnh độ sáng tối đa (`cap_id`) **không** phải là một tùy chọn trong SDK — thay vào đó, hãy chọn chúng thông qua `chloros-cli daq pool-connect --cap-id …` / `pool-set-cap`.

### Các dự án đã lưu — `open_project`

Một dự án Chloros đã lưu sẽ duy trì phần cứng được kết nối (`cameras.json` + `sensors.json` cùng với `project.json`), và `chloros_sdk.open_project(path)` có thể kết nối lại các camera, mảng cảm biến và cảm biến ánh sáng cùng lúc, đồng thời điều khiển việc chụp ảnh theo tên thiết bị. Các cảm biến ánh sáng kết nối lại thông qua một backend Chloros đang chạy, cùng với các cài đặt và giới hạn đã lưu của chúng. Xem [Tự động hóa dự án](reference/sdk-reference.md#project-automation--chlorosproject) trong tài liệu tham khảo.

## Những gì bạn nhận được khi cài đặt chỉ có pip

Kiểm tra các cờ khả dụng ở cấp mô-đun trước khi sử dụng các bề mặt phần cứng:

```python
import chloros_sdk
print(chloros_sdk.__version__)
print("CAMERA_AVAILABLE =", chloros_sdk.CAMERA_AVAILABLE)    # True iff lattice_sdk imported cleanly
print("PROJECT_AVAILABLE =", chloros_sdk.PROJECT_AVAILABLE)  # True iff ChlorosProject deps available
```

Trên một máy chủ **chỉ** có `pip install chloros-sdk` và không có gói máy tính để bàn Chloros:

* `ChlorosLocal`, `process_folder` và `process_lattice_capture` **sẽ không** hoạt động — chúng cần tệp nhị phân backend được cung cấp trong trình cài đặt máy tính để bàn.
* Các công cụ hỗ trợ kết nối thông minh (`connect_camera`, `connect_array`, `connect_daq_sensor`) là các máy khách HTTP thuần túy, do đó chúng hoạt động với máy chủ phía sau trên một máy khác — nhưng các backend đi kèm chỉ kết nối với loopback, do đó bạn phải tự chuyển tiếp cổng (ví dụ: `ssh -N -L 5000:127.0.0.1:5000 user@chloros-host`) và truyền `backend_url="http://127.0.0.1:5000"` cùng với `auto_start_backend=False`. Xem [Chế độ máy chủ phía sau từ xa](reference/sdk-reference.md#remote-backend-mode-pip-only-host-via-tunnel).
* Các lớp LATTICE truy cập trực tiếp vào phần cứng (`LatticeCamera`, `CameraPool`, …) có thể được nhập, nhưng cần có môi trường chạy Arena SDK từ gói phần mềm dành cho máy tính để bàn — nếu không có nó, `CAMERA_AVAILABLE` sẽ là `False`.
* Các cảm biến DAQ chỉ được điều khiển thông qua một backend (được chuyển tiếp) — `connect_daq_sensor()`, hoặc `proj.sensors` của dự án; backend sẽ mở cảm biến trên chính máy của nó.

## Trợ lý AI — `chloros-mcp`

SDK bao gồm `chloros-mcp`, một máy chủ MCP cho phép Claude Desktop, Claude Code, Cursor và các khách hàng MCP khác điều khiển Chloros: xử lý hình ảnh, và vận hành camera LATTICE, mảng cảm biến và cảm biến ánh sáng DAQ. Các công cụ của nó bao quát các chức năng mà SDK này thực hiện. Nó được cài đặt cùng với SDK, do đó việc thiết lập cho Claude Code chỉ cần một dòng lệnh:

```bash
claude mcp add chloros -- chloros-mcp
```

Các ứng dụng khách khác: [Sử dụng Chloros với Trợ lý AI](ai-assistants.md). Các tùy chọn và mọi công cụ: [MCP Server (Trợ lý AI)](reference/sdk-reference.md#mcp-server-ai-assistants).

## Yêu cầu về giấy phép

Để truy cập SDK, cần có tài khoản Chloros+ đang hoạt động ở bất kỳ gói trả phí nào — **Copper trở lên**(Copper / Bronze / Silver / Gold); gói miễn phí Iron không có quyền truy cập SDK/CLI. Việc thực thi được thực hiện**từ phía máy chủ**: mọi yêu cầu SDK phải kèm theo cả phiên hoạt động và gói trả phí, nếu không, hệ thống backend sẽ trả về mã lỗi `403` / `PLAN_UPGRADE_REQUIRED` (được nâng cấp thành `ChlorosLicenseError` bởi `ChlorosLocal`, và thành `ChlorosConnectError` bởi các hàm trợ giúp `connect_*`). Người gọi đã đăng xuất nhận được `401` / `AUTH_REQUIRED` (`ChlorosAuthenticationError`) — việc chạy lại `chloros-cli login` sẽ khắc phục trường hợp đầu tiên nhưng không khắc phục được trường hợp thứ hai.

Chế độ ngoại tuyến hoạt động trong thời gian ân hạn của gói dịch vụ: cấp độ dịch vụ được đọc từ bộ nhớ đệm xác thực máy chủ (5 phút) hoặc bộ nhớ đệm giấy phép đã ký và gắn với máy (30 ngày đối với gói hàng tháng; đến khi hết hạn đăng ký đối với gói hàng năm). Khi thời gian ân hạn hết hiệu lực, gói dịch vụ sẽ chuyển sang chế độ miễn phí và quyền truy cập SDK sẽ bị ngừng cho đến khi máy kết nối với máy chủ ít nhất một lần. `chloros-cli status` vẫn có thể truy cập được ở cấp độ miễn phí nên lý do luôn hiển thị rõ ràng. Xem [Đăng nhập Chloros+](chloros+-login.md).

## Các trường hợp ngoại lệ

Bắt lớp cơ sở để xử lý “bất kỳ sự cố nào của Chloros”:

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

Tất cả các ngoại lệ trong quy trình xử lý (`ChlorosBackendError`, `ChlorosConnectionError`, `ChlorosLicenseError`, `ChlorosAuthenticationError`, `ChlorosConfigurationError`, `ChlorosProcessingError`) đều kế thừa từ `ChlorosError`. Một điểm cần lưu ý: `ChlorosConnectError` — chỉ được tạo ra bởi `connect_camera` / `connect_array` / `connect_daq_sensor` — được thừa kế từ `Exception` thông thường, **không** từ `ChlorosError`, do đó `except ChlorosError` sẽ không phát hiện được lỗi này. Cấu trúc phân cấp đầy đủ có trong [Tham chiếu SDK](reference/sdk-reference.md#exceptions).

## Xem thêm

* [Tham chiếu SDK](reference/sdk-reference.md) — bề mặt API hoàn chỉnh, được tối ưu hóa cho các trợ lý AI.
* [Tham khảo CLI](reference/cli-reference.md) — mỗi lệnh con CLI tương ứng với một lệnh SDK.
* [Sử dụng Chloros với Trợ lý AI](ai-assistants.md) — hướng dẫn thiết lập và mẫu lời nhắc.
* [Tải xuống](download.md) — trình cài đặt cho Windows và Linux.
