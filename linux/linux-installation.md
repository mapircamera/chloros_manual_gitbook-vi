# Cài đặt Linux

Chloros được phân phối trên Linux dưới dạng các gói `.deb`, dùng để cài đặt CLI và máy chủ backend. Python SDK là một gói pip riêng biệt (cũng được đóng gói bên trong `.deb` dưới dạng tệp wheel phù hợp với phiên bản).

Tên tệp gói chứa thông tin về phiên bản và kiến trúc: `chloros_1.2.0_amd64.deb` dành cho x86_64 và `chloros_1.2.0_arm64_jp6.deb` dành cho các bản dựng JetPack 6 Jetson. Hãy thay thế bằng tên tệp bạn đã tải xuống thực tế trong các lệnh dưới đây.

***

## Linux amd64 (x86_64)

### Yêu cầu hệ thống

| Yêu cầu | Tối thiểu | Khuyến nghị |
| --- | --- | --- |
| **Hệ điều hành** | Ubuntu 22.04 LTS+ / Debian 12+ | Ubuntu 24.04 LTS |
| **Bộ xử lý** | x86_64 (Intel/AMD) | Intel Core i7 trở lên |
| **Bộ nhớ (RAM)** | 8GB | 16GB trở lên |
| **Card đồ họa** | Không cần (xử lý bằng CPU) | GPU NVIDIA có 4GB+ VRAM (12GB+ mở khóa `GPU_PARALLEL`, 7GB+ giữ cho Texture Aware không hoạt động trên đường dẫn hình ảnh đơn) |
| **Dung lượng lưu trữ** | 2GB dung lượng trống | SSD có 10GB+ dung lượng trống |
| **Hệ điều hành Python** | Python 3.7+ (dành cho SDK) | Python 3.10+ |

> **Ubuntu 20.04 và Debian 11 không được hỗ trợ.** Danh sách phụ thuộc của `.deb` được
> xác định dựa trên những gì mà backend Chloros thực sự liên kết, và điều đó bao gồm
> `libc6 (>= 2.34)`. Cả Focal và bullseye đều đi kèm với glibc 2.31, do đó `apt` từ chối
> cài đặt ngay từ đầu thay vì để nó thất bại sau này trong quá trình chạy.

### Cài đặt

```bash
sudo dpkg -i chloros_1.2.0_amd64.deb
sudo apt-get install -f    # pulls the declared dependencies (libibverbs1, libcap2-bin)
```

{% hint style="info" %}
`dpkg -i` không giải quyết được các phụ thuộc. Nếu hệ thống báo cáo thiếu gói phần mềm, `sudo apt-get install -f` (hoặc `sudo apt --fix-broken install`) sẽ hoàn tất quá trình cài đặt — đây là quy trình bình thường, không phải lỗi.
{% endhint %}

Kiểm tra cài đặt:

```bash
chloros-cli --version    # prints "Chloros CLI 1.2.0"
```



<!-- SCREENSHOT-NEEDED: Terminal on Ubuntu 22.04 immediately after `sudo dpkg -i chloros_1.2.0_amd64.deb`, showing the full postinst output: the "Chloros installed successfully!" banner, the Usage lines, the "Python SDK:" block naming the bundled wheel path under /usr/lib/chloros/sdk/, any "GPU Acceleration:" detection line, and the closing "Systemd Service (optional): sudo systemctl enable --now chloros-backend.service" hint -->***

## Linux arm64 (NVIDIA Jetson)

### Yêu cầu hệ thống

| Yêu cầu | Tối thiểu | Khuyến nghị |
| --- | --- | --- |
| **Nền tảng** | NVIDIA Jetson với JetPack 6 | Jetson Orin NX 16GB hoặc AGX Orin |
| **JetPack** | JetPack 6.x | Phiên bản JetPack 6 mới nhất |
| **Bộ nhớ (RAM)** | 8GB (chia sẻ giữa GPU và CPU) | 16GB+ chia sẻ (12GB+ là ngưỡng tối thiểu để chạy các công việc GPU song song) |
| **Dung lượng lưu trữ** | 2GB dung lượng trống | SSD NVMe với 10GB+ dung lượng trống |
| **Hệ điều hành Python** | Python 3.7+ (cho SDK) | Python 3.10+ |

### Cài đặt

```bash
sudo dpkg -i chloros_1.2.0_arm64_jp6.deb
sudo apt-get install -f
chloros-cli --version
```

Cấu trúc tương tự như phiên bản amd64 `.deb`, với bản dựng CUDA được tối ưu hóa cho Jetson Orin / Orin NX / Orin Nano. Để biết thông tin về bộ nhớ, nhiệt độ và hành vi triển khai thực tế của Jetson, hãy tham khảo [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md).

***

## Cài đặt Python SDK (Tất cả Linux)

SDK là một client thuần túy Python HTTP cho backend, do đó cùng một gói phần mềm hoạt động trên cả amd64 và arm64. Có hai nguồn:**Từ PyPI** — phiên bản ổn định đã được phát hành:

```bash
pip install chloros-sdk
```

**Từ gói wheel đi kèm** — đảm bảo tương thích với phiên bản CLI/backend mà bạn vừa cài đặt (sử dụng gói này khi phiên bản `.deb` của bạn mới hơn phiên bản trên PyPI):

```bash
pip install --user /usr/lib/chloros/sdk/chloros_sdk-*.whl
```

{% hint style="warning" %}
**Các bản phân phối tuân thủ PEP 668** (Ubuntu 23.10+, Debian 12+) không cho phép cài đặt pip trên toàn hệ thống. Hãy sử dụng `pip install --user …`, một môi trường ảo, hoặc `sudo pip install --break-system-packages …`. Trình cài đặt gói phần mềm không bao giờ tự động cài đặt SDK vào hệ thống của bạn Python — quyết định này hoàn toàn thuộc về bạn.
{% endhint %}

Các thành phần bổ sung tùy chọn:

| Thành phần | Lệnh | Thêm |
| --- | --- | --- |
| `progress` | `pip install chloros-sdk[progress]` | `sseclient-py` để truyền phát tiến trình trực tiếp |
| `camera` | `pip install chloros-sdk[camera]` | `bleak` cho giao thức truyền tải BLE (DAQ-M) |

Xác minh tệp cấu hình &quot;SDK&quot;:

```bash
python -c "import chloros_sdk; print(chloros_sdk.__version__)"
```

{% hint style="info" %}
`.deb` cài đặt Chloros CLI và backend. Python SDK giao tiếp với backend đó qua một HTTP API (`http://127.0.0.1:5000`) cục bộ và tự động khởi động nó khi cần thiết. Luôn sử dụng địa chỉ IPv4 cụ thể thay vì `localhost` — `localhost` có thể được giải quyết thành `::1` và tốn khoảng hai giây cho mỗi yêu cầu.
{% endhint %}

***

## Cài đặt lần đầu

### 1. Đăng nhập

CLI và quyền truy cập SDK yêu cầu gói dịch vụ trả phí Chloros+ (**Copper** trở lên), được áp dụng ở phía máy chủ: người gọi chưa đăng nhập sẽ nhận được `401 AUTH_REQUIRED`, còn người gọi sử dụng gói miễn phí (Iron) sẽ nhận được `403 PLAN_UPGRADE_REQUIRED`.

```bash
chloros-cli login your@email.com 'your-password'
```

Thông tin đăng nhập được lưu trong bộ nhớ đệm tại `~/.chloros/user_session.json`.

{% hint style="warning" %}
**Bạn phải đăng nhập lại sau mỗi lần cài đặt hoặc nâng cấp.** Tập lệnh `prerm` của gói phần mềm sẽ cố ý xóa `~/.chloros/user_session.json` và giấy phép được lưu trong bộ nhớ đệm cho mọi người dùng trên máy, do đó mỗi bản dựng mới sẽ luôn xác thực lại giấy phép thay vì tin tưởng vào bộ nhớ đệm đã lỗi thời.
{% endhint %}

### 2. Kiểm tra trạng thái giấy phép

```bash
chloros-cli status
```

`chloros-cli status` hoạt động trên mọi gói dịch vụ (kể cả gói miễn phí), do đó bạn luôn có thể biết lý do tại sao quyền truy cập có hoặc không khả dụng.

### 3. Chạy chẩn đoán hệ thống

```bash
chloros-cli selftest
```

Bảy bước kiểm tra được thực hiện theo thứ tự, và lệnh sẽ trả về giá trị khác 0 nếu bất kỳ bước nào thất bại:

| # | Kiểm tra | Kết quả |
| --- | --- | --- |
| 1 | **Phiên bản** | Dịch vụ &quot;CLI&quot; báo cáo phiên bản của nó (`v1.2.0`). |
| 2 | **Cổng khả dụng** | Cổng 5000 đang trống, *hoặc* đã được phản hồi bởi một máy chủ nền Chloros đang hoạt động bình thường (điều này được coi là đạt yêu cầu). |
| 3 | **Khởi động máy chủ phía sau** | Tệp nhị phân máy chủ phía sau được khởi chạy. |
| 4 | **Kiểm tra API (`/api/test`)** | Máy chủ phía sau trả lời `status: ok`. |
| 5 | **Thông tin hệ thống** | Hiển thị `GPU: <name>, CUDA: <bool>, PyTorch: <version>` từ `/api/system-info`. |
| 6 | **Các mô hình khử nhiễu** | Tìm thấy các mô hình `*.pth.enc` (trên Linux: `/usr/lib/chloros/models`). |
| 7 | **CUDA + Bộ khử nhiễu**| Chức năng Texture Aware thực sự có thể sử dụng được — cần có CUDA**và** ít nhất một tệp mô hình. |

Quá trình chạy kết thúc với `N/7 checks passed`, liệt kê các lỗi theo tên.

### 4. Xử lý tập dữ liệu đầu tiên của bạn

```bash
chloros-cli process ~/datasets/flight001
```

***

## Tệp và thư mục

### Mỗi người dùng

Chloroslưu trữ thông tin đăng nhập và cấu hình CLI trong một thư mục duy nhất tương thích đa nền tảng, **`~/.chloros/`** (trên Windows, `%USERPROFILE%\.chloros\`). Hai bộ nhớ đệm dành riêng cho Linux tuân theo các quy ước XDG — các bộ nhớ đệm này sẽ sử dụng `XDG_CONFIG_HOME` / `XDG_CACHE_HOME` khi được thiết lập.

| Đường dẫn | Mục đích |
| --- | --- |
| `~/.chloros/user_session.json` | Bộ nhớ đệm phiên đăng nhập được ghi bởi `chloros-cli login` (được xóa sau mỗi lần cài đặt/nâng cấp gói phần mềm) |
| `~/.chloros/working_directory.txt` | Ghi đè thư mục dự án mặc định (`chloros-cli set-project-folder` / `get-project-folder` / `reset-project-folder`) |
| `~/.chloros/cli_language.json` | Tùy chọn ngôn ngữ của CLI (`chloros-cli language <code>`) |
| `~/.chloros/user.json` | Cài đặt ngôn ngữ được chia sẻ với giao diện người dùng (GUI) của Windows — giá trị `language` ở đây có ưu tiên cao hơn `cli_language.json` |
| `~/.chloros/update_cache.json` | Bộ nhớ đệm một giờ cho việc kiểm tra cập nhật khởi động của Linux /Jetson |
| `~/.chloros/backend.log` | Nhật ký backend khi backend được khởi chạy bởi CLI |
| `~/.chloros/camera_cal/<serial>/<bundle_sha>/` | Các gói hiệu chuẩn LATTICE được lưu trong bộ nhớ đệm cho từng camera, được chỉ định bằng số sê-ri và băm gói |
| `~/.chloros/daq_cap_profiles/<u\|m\|e>/<cap_id>.json` | Các thiết lập ghi đè tùy chọn của người dùng cho các cấu hình hiệu chỉnh giới hạn DAQ |
| `~/.config/chloros/system_config.json` | Hồ sơ phần cứng được lưu trong bộ nhớ đệm từ Dynamic Compute Adaptation — xóa hồ sơ này để buộc thực hiện phát hiện phần cứng mới |
| `~/.cache/chloros/logs/backend_<YYYYMMDD_HHMMSS>.log` | Nhật ký máy chủ backend, một tệp cho mỗi lần khởi chạy |
| `~/Chloros Projects/` | Thư mục dự án mặc định khi không có thiết lập tùy chỉnh nào |

### Toàn hệ thống

| Đường dẫn | Mục đích |
| --- | --- |
| `/usr/bin/chloros-cli` | Tập lệnh bọc — thiết lập `LD_LIBRARY_PATH` cho các thư viện gốc được gói sẵn, sau đó thực thi tệp nhị phân thực tế |
| `/usr/bin/chloros-backend` | Tập lệnh bọc — tương tự, cộng thêm `CHLOROS_PRODUCTION=1` để cổng xác thực của phần nền không bao giờ tự vô hiệu hóa một cách im lặng |
| `/usr/lib/chloros/chloros-cli`, `/usr/lib/chloros/chloros-backend` | Các tệp nhị phân đã biên dịch |
| `/usr/lib/chloros/arena_runtime/` | Môi trường chạy Arena SDK cần thiết cho các camera LATTICE |
| `/usr/lib/chloros/models/*.pth.enc` | Các mô hình khử nhiễu được mã hóa được sử dụng bởi thuật toán Texture Aware debayer |
| `/usr/lib/chloros/sdk/chloros_sdk-*.whl` | Python SDK phiên bản tương ứng chính xác với bản dựng này |
| `/usr/lib/chloros/exiftool` | exiftool được gói kèm (chỉ được liên kết tượng trưng đến `/usr/local/bin/exiftool` nếu không có exiftool hệ thống) |
| `/etc/chloros/update.conf` | Cấu hình kênh cập nhật được `chloros-cli update` đọc |
| `/etc/sysctl.d/60-chloros-ptp.conf` | Đặt `net.ipv4.ip_unprivileged_port_start = 319` để phần nền có thể gán các cổng PTP mà không cần quyền root |
| `/etc/ld.so.conf.d/Arena_SDK.conf` | Chỉ định trình tải động đến `/usr/lib/chloros/arena_runtime` |
| `/lib/udev/rules.d/70-chloros-daq.rules` | Cấp quyền truy cập cho người dùng đã đăng nhập vào cầu nối nối tiếp USB DAQ-U (CP2102N, `10c4:ea60`) |
| `/lib/systemd/system/chloros-backend.service` | Kích hoạt dịch vụ nền luôn chạy (đã cài đặt, **chưa được bật**) |
| `/usr/share/applications/chloros-cli.desktop` | Mục menu ứng dụng &quot;Chloros CLI&quot; để mở cửa sổ terminal |

## Vị trí tệp thực thi nền

Các tệp CLI và SDK tự động phát hiện dịch vụ nền:

| Thành phần | Đường dẫn |
| --- | --- |
| CLI | `/usr/bin/chloros-cli` |
| Dịch vụ nền | `/usr/lib/chloros/chloros-backend` |

Ghi đè đường dẫn backend bằng cờ CLI với giá trị `--backend-exe` hoặc tham số hàm tạo SDK với giá trị `backend_exe`, và ghi đè cổng bằng `--port` (mặc định là `5000`).

{% hint style="info" %}
`CHLOROS_BACKEND_URL` trỏ đến các nhóm lệnh **`lattice`**,**`project`**, và**`daq pool-*`** tại một máy chủ phía sau từ xa. Các lệnh cốt lõi (`process`, `login`, `logout`, `status`, `export-status`, `time-sync`, `selftest`) cố ý bỏ qua các mã này và luôn nhắm đến `http://127.0.0.1:<port>`.
{% endhint %}

***

## Camera LATTICE và cảm biến ánh sáng DAQ trên Linux

Tất cả các nhóm lệnh live-hardware đều hoạt động trên Linux (amd64 và Jetson):

* **`chloros-cli lattice`** — phát hiện, kết nối, cấu hình và thu thập dữ liệu từ camera LATTICE và các mảng đồng bộ. `.deb` gói sẵn môi trường chạy Arena SDK cần thiết và đăng ký nó với trình tải động.
* **`chloros-cli daq pool-*`** — kết nối các cảm biến ánh sáng DAQ-U/M/E thông qua nhóm backend, truyền dữ liệu quang phổ đã hiệu chuẩn và ghi lại các tệp `.daq`. CLI đã biên dịch chỉ cung cấp dòng sản phẩm `pool-*`: `pool-connect`, `pool-disconnect`, `pool-list`, `pool-latest`, `pool-stream`, `pool-record`, `pool-set-cap`.
* **`chloros-cli project`** — chạy một dự án đã lưu (bao gồm các camera, cảm biến và cài đặt xử lý) ở chế độ không giao diện.
* **`chloros-cli time-sync`** — kiểm tra máy chủ PTP chính mà hệ thống nền tảng Chloros chạy cho các camera LATTICE và cảm biến DAQ-E.

```bash
# DAQ-E at a known address — the reliable path on multi-homed hosts
chloros-cli daq pool-connect --eth-host 192.168.2.50

# DAQ-U over USB serial
chloros-cli daq pool-connect --port /dev/ttyUSB0

# What is connected, then the latest calibrated spectrum as JSON
chloros-cli daq pool-list
chloros-cli daq pool-latest --sensor-id daq-e-a1b2c3 --json
```

`--sensor-id` là yêu cầu tiên quyết cho `pool-latest`, `pool-stream`, `pool-record` và `pool-set-cap`; `pool-list` hiển thị các ID hiện có trong nhóm.

{% hint style="info" %}
**Nên ưu tiên sử dụng `--eth-host` cho lần kết nối DAQ-E đầu tiên trên một máy có nhiều giao diện mạng.** Tính năng tự động phát hiện sẽ quét mDNS và có thể bỏ sót giao diện của cảm biến do bộ nhớ đệm ARP chưa được cập nhật, do đó lần kết nối `pool-connect --eth` đầu tiên sau khi khởi động có thể thất bại ngay cả khi cảm biến hoàn toàn hoạt động bình thường. Việc truyền địa chỉ IP hoặc tên máy chủ của cảm biến sẽ bỏ qua hoàn toàn quá trình phát hiện.
{% endhint %}

**Quyền truy cập cổng nối tiếp DAQ-U** được quản lý bởi quy tắc udev đã cài đặt (`uaccess` + nhóm `dialout`). Nếu một cảm biến đã được cắm sẵn mà vẫn không thể truy cập được, hãy tải lại các quy tắc hoặc cắm lại cảm biến:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger --subsystem-match=tty
```

Xem [CLI tài liệu tham khảo](../CLI.md) để biết đầy đủ bộ lệnh.

### PTP luôn hoạt động cho các máy chủ không có giao diện đồ họa

Khi cài đặt lần đầu, đơn vị systemd `chloros-backend.service` sẽ được tạo ra nhưng **chưa được kích hoạt**. Trên thiết bị Jetson không màn hình hoặc máy chủ cần duy trì đồng bộ thời gian PTP liên tục cho các cảm biến DAQ-E và camera LATTICE, hãy kích hoạt nó:

```bash
sudo systemctl enable --now chloros-backend.service
sudo systemctl status chloros-backend.service
```

Nếu không có đơn vị này, PTP chỉ chạy khi backend Chloros đang hoạt động — tức là trong suốt phiên làm việc CLI / SDK đang diễn ra.

Thiết bị này liên kết backend với `127.0.0.1:5000` (các thiết lập môi trường `CHLOROS_HOST` / `CHLOROS_PORT` bên trong thiết bị; ghi đè bằng `sudo systemctl edit chloros-backend.service`) và khởi động lại nó sau 5 giây nếu gặp lỗi.

**Cách PTP lấy các cổng của mình.** PTP sử dụng UDP 319/320, cả hai đều nằm dưới ngưỡng 1024 cổng ưu tiên. Tệp `postinst` trong gói này ghi giá trị `/etc/sysctl.d/60-chloros-ptp.conf` cùng với `net.ipv4.ip_unprivileged_port_start = 319`, cho phép phần mềm nền (backend) liên kết các cổng này khi chạy dưới quyền người dùng của bạn. Nó cũng áp dụng giá trị `setcap cap_net_bind_service,cap_net_raw=+ep` cho tệp nhị phân backend như một biện pháp phòng ngừa — đó là lý do tại sao `libcap2-bin` được khai báo là phụ thuộc của gói.***

## Ví dụ về kịch bản Bash

{% hint style="info" %}
**Mã thoát thân thiện với kịch bản.**`chloros-cli process` trả về mã thoát `0` khi thành công và**không bằng 0 khi thất bại — bao gồm cả trường hợp yêu cầu sản phẩm hình ảnh nhưng không ghi được sản phẩm nào** (nó hiển thị mã `Processing finished but wrote no image products.` và nêu tên thư mục dự án cùng các nguyên nhân thường gặp). Các lần chạy thành công báo cáo số lượng sản phẩm hình ảnh đã được ghi (`Image products written: N`). Mã thoát: `0` thành công, `1` thất bại, `2` lỗi tham số, `130` bị gián đoạn.
{% endhint %}

### Xử lý nhiều tập dữ liệu

```bash
#!/bin/bash
for dataset in ~/datasets/2026/*/; do
    echo "Processing $(basename "$dataset")..."
    if chloros-cli process "$dataset" --format "TIFF (32-bit, Percent)"; then
        echo "Done: $(basename "$dataset")"
    else
        echo "FAILED: $(basename "$dataset")" >&2
    fi
done
```

### Xử lý với cài đặt tùy chỉnh

```bash
#!/bin/bash
chloros-cli process ~/datasets/field_a \
    --output ~/output/field_a \
    --format "TIFF (32-bit, Percent)" \
    --indices NDVI NDRE GNDVI \
    --debayer texture-aware \
    --no-vignette
```

Có chính xác bốn giá trị `--format` hợp lệ, và chúng chứa khoảng trắng — luôn phải đặt chúng trong dấu ngoặc kép:

| Giá trị `--format` | Thư mục đầu ra |
| --- | --- |
| Giá trị `TIFF (16-bit)` *(mặc định)* | `tiff16` |
| `TIFF (32-bit, Percent)` | `tiff32` |
| `PNG (8-bit)` | `png8` |
| `JPG (8-bit)` | `jpg8` |

`--debayer` chấp nhận `standard` (mặc định) hoặc `texture-aware` (Chloros+).

### Xử lý tự động bằng Cron

```cron
# Process any new datasets at 2 AM daily
0 2 * ** /usr/bin/chloros-cli process /data/incoming --output /data/processed >> /var/log/chloros.log 2>&1
```

### Ví dụ về Python SDK

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

## Khắc phục sự cố

### Không tìm thấy tệp CLI sau khi cài đặt

```bash
# Check if the binary exists
which chloros-cli
ls -la /usr/bin/chloros-cli

# List everything the package installed
dpkg -L chloros

# Reload your shell
source ~/.bashrc
```

### Quyền truy cập bị từ chối

```bash
sudo chmod +x /usr/bin/chloros-cli
sudo chmod +x /usr/lib/chloros/chloros-backend
```

### &quot;setcap failed&quot; trong quá trình cài đặt

`.deb` áp dụng `cap_net_bind_service` cho `/usr/lib/chloros/chloros-backend` để nó có thể gán các cổng PTP 319/320 mà không cần quyền root. Nếu `libcap2-bin` thiếu tại thời điểm cài đặt, lệnh này sẽ bị bỏ qua. Hãy cài đặt nó và cài đặt lại gói phần mềm:

```bash
sudo apt install libcap2-bin
sudo apt reinstall chloros
```

### PTP không khởi động được / Không thể gán cổng 319

Xác nhận rằng ngưỡng cổng không có đặc quyền đã được hạ xuống, và áp dụng lại cho lần khởi động hiện tại nếu chưa thực hiện:

```bash
sysctl net.ipv4.ip_unprivileged_port_start     # expect 319
sudo sysctl -w net.ipv4.ip_unprivileged_port_start=319
```

Sau đó kiểm tra máy chủ chính (grandmaster):

```bash
chloros-cli time-sync status
chloros-cli time-sync peers
```

### &quot;Không tìm thấy trình điều khiển camera LATTICE&quot;

Môi trường chạy Arena SDK không được giải quyết. Hãy xác nhận rằng cấu hình trình tải mà gói phần mềm ghi vào đã có mặt và được cập nhật:

```bash
cat /etc/ld.so.conf.d/Arena_SDK.conf     # expect /usr/lib/chloros/arena_runtime
sudo ldconfig
ls /usr/lib/chloros/arena_runtime | head
```

### Backend không khởi động được

```bash
# Check if port 5000 is already in use
lsof -i :5000

# Kill any existing process on port 5000
kill $(lsof -t -i :5000)

# Try starting with a different port
chloros-cli --port 5001 process ~/datasets/flight001
```

Nhật ký backend cho lần khởi động thất bại có trong `~/.cache/chloros/logs/`.

### Không phát hiện CUDA

```bash
# Check NVIDIA driver installation
nvidia-smi

# Check CUDA availability
nvcc --version

# On Jetson, check JetPack version
cat /etc/nv_tegra_release
```

`chloros-cli selftest` báo cáo cùng một thông tin trong một dòng: `GPU: <name>, CUDA: <bool>, PyTorch: <version>`.

### Thiếu thư viện chia sẻ

```bash
sudo apt-get update
sudo apt-get install -f

# Check for missing libraries
ldd /usr/lib/chloros/chloros-backend | grep "not found"
```

### Khởi động chậm trên hệ thống sử dụng thẻ SD

Các tệp nhị phân đã biên dịch sẽ tự giải nén vào một thư mục tạm thời mỗi khi khởi động. Nếu tệp `/mnt/ssd/tmp` tồn tại, Chloros sẽ tự động sử dụng nó; nếu không, hãy thiết lập `TMPDIR` thành một hệ thống tệp nhanh:

```bash
export TMPDIR=/mnt/nvme/tmp
```

***

## Cập nhật Chloros trên Linux

Lệnh `update` chỉ dành riêng cho Linux /Jetson. Lệnh này kiểm tra phiên bản được phát hành trong kênh cập nhật được cấu hình tại `/etc/chloros/update.conf` và đề xuất tải xuống và cài đặt phiên bản tương ứng `.deb`:

```bash
# Check for updates without installing
chloros-cli update --check

# Check for and install updates
chloros-cli update
```

Trên Linux /Jetson, CLI cũng thực hiện kiểm tra cập nhật không chặn mỗi khi khởi động (kết quả được lưu trong bộ nhớ đệm trong một giờ tại `~/.chloros/update_cache.json`) và hiển thị `Update available: vX.Y.Z` khi có phiên bản mới hơn. Các cài đặt và dự án của bạn sẽ được giữ nguyên sau khi cập nhật; bạn sẽ cần đăng nhập lại sau đó.

## Gỡ cài đặt

```bash
sudo apt remove chloros
```

Việc gỡ cài đặt sẽ dừng `chloros-backend.service`, khôi phục giới hạn cổng không có đặc quyền mặc định (1024), xóa liên kết tượng trưng exiftool đi kèm và cấu hình trình tải Arena, đồng thời xóa thông tin đăng nhập được lưu trong bộ nhớ đệm. Các dự án và tệp dữ liệu `~/.chloros/` của bạn sẽ không bị ảnh hưởng.

***

## Các bước tiếp theo

* [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) — Tối ưu hóa và triển khai dành riêng cho Jetson
* [CLI : Command Line](../CLI.md) — Hướng dẫn về &quot;CLI&quot;
* [API : Python SDK](../api-python-sdk.md) — Hướng dẫn về &quot;SDK&quot;
* [Tham khảo CLI](../reference/cli-reference.md) và [Tham khảo SDK](../reference/sdk-reference.md) — danh sách đầy đủ các lệnh/API cho phiên bản 1.2.0
* [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md) — cách Chloros thích ứng với phần cứng của bạn
