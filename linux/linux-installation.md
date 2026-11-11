# Cài đặt trên Linux

Chloros được phân phối cho Linux dưới dạng các gói `.deb`, bao gồm giao diện dòng lệnh (CLI), máy chủ nền và bộ công cụ phát triển phần mềm Python (SDK) (đối với hệ thống `python3`, kèm theo một tệp wheel tương thích về phiên bản dành cho môi trường ảo virtualenv).

Tên tệp gói chứa thông tin về phiên bản và kiến trúc: `chloros_1.3.0_amd64.deb` dành cho x86_64, và `chloros_1.3.0_arm64_jp6.deb` dành cho arm64 (NVIDIA Jetson, JetPack 6). Hãy thay thế bằng tên tệp bạn đã tải xuống thực tế trong các lệnh dưới đây.

{% hint style="info" %}
**Mới trong phiên bản 1.3.0: các chương trình được phân phối dưới dạng đã giải nén.** Phần lõi (backend) và giao diện dòng lệnh (CLI) được cài đặt dưới dạng các thư mục, lần lượt là `/usr/lib/chloros/backend/` và `/usr/lib/chloros/cli/`, thay vì các tệp tự giải nén đơn lẻ. Chúng không còn tự giải nén vào một thư mục tạm thời mỗi khi khởi động, do đó khởi động nhanh hơn nhiều (trên x86_64, phần backend trước đây mất khoảng 100 giây để giải nén mỗi khi khởi động) và không còn cần vài GB dung lượng trống trong thư mục `/tmp`. Phiên bản amd64 `.deb` cũng chỉ còn khoảng một nửa kích thước so với trước đây.
{% endhint %}

***

## Linux amd64 (x86_64)

### Yêu cầu hệ thống

| Yêu cầu | Tối thiểu | Khuyến nghị |
| --- | --- | --- |
| **Hệ điều hành** | Ubuntu 22.04 LTS+ / Debian 12+ | Ubuntu 24.04 LTS |
| **Bộ xử lý** | x86_64 (Intel/AMD) | Intel Core i7 trở lên |
| **Bộ nhớ (RAM)** | 8GB | 16GB trở lên |
| **Card đồ họa** | Không cần (xử lý bằng CPU) | GPU NVIDIA có VRAM 4GB trở lên (12GB trở lên sẽ mở khóa `GPU_PARALLEL`, 7GB trở lên sẽ giữ chế độ Texture Aware tắt trên đường dẫn hình ảnh đơn) |
| **Dung lượng lưu trữ** | 2GB dung lượng trống | SSD có 10GB+ dung lượng trống |
| **Python** | Python 3.7+ (cho SDK) | Python 3.10+ |

> **Ubuntu 20.04 và Debian 11 không được hỗ trợ.** Danh sách phụ thuộc của `.deb` được
> xác định dựa trên những thành phần mà backend Chloros thực sự liên kết, và điều này bao gồm một
> phiên bản `libc6` mới hơn so với glibc 2.31 được cung cấp trong các bản phát hành focal và bullseye, do đó `apt` sẽ từ chối
> cài đặt ngay lập tức thay vì để quá trình cài đặt thất bại sau này khi chạy chương trình.

### Cài đặt

```bash
sudo apt install ./chloros_1.3.0_amd64.deb
```

{% hint style="info" %}
**Hãy sử dụng `apt install ./…`, không phải `dpkg -i`.** `apt` giải quyết các phụ thuộc của gói và cũng cài đặt những gì nó khuyến nghị: các yêu cầu của Python SDK, `python3-numpy` và `python3-requests`. `dpkg -i` không cài đặt cả hai. CLI và backend là các thành phần độc lập và có thể chạy mà không cần chúng, nhưng `import chloros_sdk` cần chúng; kết quả cài đặt sẽ liệt kê bất kỳ thành phần nào còn thiếu. Hãy giữ lại `./`, để `apt` có thể cài đặt tệp này thay vì tìm kiếm gói có tên tương ứng. Gói này cũng khai báo `libgl1`, thành phần mà OpenCV đi kèm sẽ tải và có thể thiếu trong hình ảnh máy chủ tối thiểu.
{% endhint %}

Kiểm tra cài đặt:

```bash
chloros-cli --version    # prints "Chloros CLI 1.3.0"
```

<!-- SCREENSHOT-NEEDED: Terminal on Ubuntu 22.04 immediately after `sudo apt install ./chloros_1.3.0_amd64.deb`, showing the full postinst output: the "Chloros installed successfully!" banner, the Usage lines, the "Python SDK:" block ("chloros_sdk 1.3.0 for python3" and the "In a virtualenv: pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl" line), any "GPU Acceleration:" detection line, and the closing "Systemd Service (optional): sudo systemctl enable --now chloros-backend@<user>.service" hint -->

***

## Linux arm64 (NVIDIA Jetson)

### Yêu cầu hệ thống

| Yêu cầu | Tối thiểu | Khuyến nghị |
| --- | --- | --- |
| **Nền tảng** | NVIDIA Jetson với JetPack 6 | Jetson Orin NX 16GB hoặc AGX Orin |
| **JetPack** | JetPack 6.x | Phiên bản JetPack 6 mới nhất |
| **Bộ nhớ (RAM)** | 8GB (chia sẻ giữa GPU và CPU) | 16GB+ chia sẻ (12GB+ là ngưỡng tối thiểu cho xử lý song song) |
| **Dung lượng lưu trữ** | 2GB dung lượng trống | SSD NVMe có 10GB+ dung lượng trống |
| **Python** | Python 3.7+ (cho SDK) | Python 3.10+ |

### Cài đặt

```bash
sudo apt install ./chloros_1.3.0_arm64_jp6.deb
chloros-cli --version
```

Cấu trúc tương tự như phiên bản amd64 `.deb` (bao gồm Python SDK, bộ đệm nhận và các dịch vụ backend), dành cho Jetson Orin / Orin NX / Orin Nano trên **JetPack 6** (L4T R36). PyTorch của gói này là phiên bản CUDA của JetPack 6 dành cho Orin, do đó việc xử lý GPU, bao gồm cả thuật toán Texture Aware debayer, sẽ chạy trên GPU của Jetson. Gói này không chứa CUDA: nó sử dụng các thư viện CUDA và cuDNN mà JetPack 6 cài đặt (`sudo apt install nvidia-jetpack` trên hình ảnh không có các thư viện này). Trong trường hợp không có GPU CUDA nào có thể sử dụng được, quá trình xử lý sẽ diễn ra trên CPU và `--debayer texture-aware` sẽ sử dụng thuật toán khử pixel tiêu chuẩn (backend sẽ ghi lại một dòng thông báo `[DEBAYER] … needs a CUDA GPU …` giải thích lý do). Để biết thông tin về bộ nhớ, nhiệt độ và hành vi triển khai thực địa của Jetson, hãy tham khảo [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md).***

## Cài đặt Python SDK (Tất cả các hệ điều hành Linux)**`.deb` sẽ tự động cài đặt SDK cho bạn.** Cả hai gói (amd64 và Jetson) đều cài đặt SDK cho hệ thống `python3`, do đó `import chloros_sdk` hoạt động ngay sau khi cài đặt, mà không cần pip hay kết nối mạng. Các gói của nó (`chloros_sdk`, `lattice_sdk`, `lattice_cam`, `arena_api` và các trình tải dự án) được đặt trong `/usr/lib/python3/dist-packages`, thuộc sở hữu của gói: việc nâng cấp sẽ thay thế chúng, và việc gỡ bỏ gói sẽ xóa chúng. Phiên bản luôn khớp với giao diện dòng lệnh (CLI) và backend mà bạn đã cài đặt.

Các yêu cầu riêng của SDK, `python3-numpy` và `python3-requests`, là **được khuyến nghị, không bắt buộc** bởi gói phần mềm: `sudo apt install ./chloros_<version>_<arch>.deb` sẽ cài đặt chúng, trong khi `dpkg -i` thì không. Kết quả cài đặt sẽ báo cáo phiên bản SDK mà `python3` nhập vào, bất kỳ yêu cầu nào còn thiếu, và bất kỳ bản `chloros_sdk` nào khác trên đường dẫn của `python3` mà che lấp bản sao của gói (chẳng hạn như bản đã được cài đặt trước đó bằng `pip install --user`).**Trong môi trường virtualenv**, vốn không truy cập được các gói của hệ thống, hãy cài đặt tệp wheel đi kèm vào đó (hoàn toàn ngoại tuyến):

```bash
pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl
```

**Từ PyPI** — chỉ dành cho máy tính chưa cài đặt gói Chloros, trong đó SDK điều khiển một backend trên máy tính khác (xem [Chế độ Backend Từ Xa](../reference/sdk-reference.md#remote-backend-mode-pip-only-host-via-tunnel)):

```bash
pip install chloros-sdk
```

Các tùy chọn bổ sung, dành cho cài đặt qua pip (trong môi trường ảo hoặc từ PyPI):

| Tùy chọn | Lệnh | Thêm |
| --- | --- | --- |
| `progress` | `pip install chloros-sdk[progress]` | `sseclient-py` để truyền phát tiến trình trực tiếp |
| `camera` | `pip install chloros-sdk[camera]` | `bleak` dành cho giao thức truyền tải BLE (DAQ-M) |

Xác minh SDK:

```bash
python3 -c "import chloros_sdk; print(chloros_sdk.__version__)"
```

{% hint style="info" %}
SDK Python giao tiếp với phần backend mà `.deb` cài đặt thông qua HTTP API cục bộ (`http://127.0.0.1:5000`). `ChlorosLocal` và các công cụ hỗ trợ kết nối sẽ tự động khởi động nó khi cần thiết; `connect_all()` của một dự án đã lưu thì không bao giờ làm vậy, vì vậy trên máy không có giao diện đồ họa, hãy chạy phần backend dưới dạng [dịch vụ](#always-on-ptp-for-headless-hosts). Luôn sử dụng địa chỉ IPv4 nguyên văn thay vì `localhost` — `localhost` có thể được giải quyết thành `::1` và tốn khoảng hai giây cho mỗi yêu cầu.
{% endhint %}

***

## Cài đặt lần đầu

### 1. Đăng nhập

Việc truy cập CLI và SDK yêu cầu gói Chloros+ trả phí (**Copper** trở lên), được áp dụng ở phía máy chủ: người gọi chưa đăng nhập sẽ nhận được `401 AUTH_REQUIRED`, còn người gọi sử dụng gói miễn phí (Iron) sẽ nhận được `403 PLAN_UPGRADE_REQUIRED`.

```bash
chloros-cli login your@email.com 'your-password'
```

Thông tin đăng nhập được lưu trong bộ nhớ đệm tại `~/.chloros/user_session.json`.

{% hint style="info" %}
**Việc nâng cấp sẽ giữ nguyên thông tin đăng nhập; việc gỡ bỏ sẽ xóa thông tin này.** Cài đặt phiên bản mới hơn (`.deb`) lên bản cài đặt hiện có (bao gồm cả qua `chloros-cli update`) sẽ giữ lại `~/.chloros/user_session.json` và bản lưu trữ giấy phép, do đó các tác vụ CLI/SDK không giao diện người dùng vẫn hoạt động bình thường. Việc gỡ bỏ gói (`sudo apt remove chloros`) sẽ xóa thông tin đăng nhập đã lưu và bộ nhớ đệm giấy phép cho mọi người dùng trên máy, do đó việc cài đặt mới sẽ cần `chloros-cli login` một lần nữa.
{% endhint %}

### 2. Kiểm tra trạng thái giấy phép

```bash
chloros-cli status
```

`chloros-cli status` hoạt động trên mọi gói dịch vụ (bao gồm cả gói miễn phí), do đó bạn luôn có thể xác định lý do tại sao quyền truy cập có hoặc không khả dụng.

### 3. Chạy chẩn đoán hệ thống

```bash
chloros-cli selftest
```

Bảy bước kiểm tra được thực hiện theo thứ tự, và lệnh sẽ trả về giá trị không bằng 0 nếu bất kỳ bước nào thất bại:

| # | Kiểm tra | Xác nhận điều gì |
| --- | --- | --- |
| 1 | **Phiên bản** | Giao diện dòng lệnh (CLI) báo cáo phiên bản của nó (`v1.3.0`). |
| 2 | **Cổng khả dụng** | Cổng 5000 đang trống, *hoặc* đã được phản hồi bởi một backend Chloros hoạt động bình thường (điều này được coi là đỗ). |
| 3 | **Khởi động backend** | Tệp nhị phân backend được khởi chạy. |
| 4 | **Kiểm tra API (`/api/test`)** | Backend trả lời `status: ok`. |
| 5 | **Thông tin hệ thống** | Hiển thị `GPU: <name>, CUDA: <bool>, PyTorch: <version>` từ `/api/system-info`. |
| 6 | **Mô hình khử nhiễu** | Tìm thấy các mô hình `*.pth.enc` (trên Linux: `/usr/lib/chloros/models`). |
| 7 | **CUDA + Bộ lọc nhiễu**| Chế độ Texture Aware thực sự có thể sử dụng được — cần CUDA**và** ít nhất một tệp mô hình. |

Quá trình chạy kết thúc với `N/7 checks passed`, liệt kê các lỗi theo tên.

### 4. Xử lý bộ dữ liệu đầu tiên của bạn

```bash
chloros-cli process ~/datasets/flight001
```

***

## Tệp và thư mục

### Dành cho từng người dùng

Chloros lưu trữ thông tin đăng nhập và cấu hình CLI trong một thư mục duy nhất tương thích đa nền tảng, **`~/.chloros/`** (trên Windows, `%USERPROFILE%\.chloros\`). Hai bộ nhớ đệm dành riêng cho Linux tuân theo các quy ước XDG — các thư mục này sẽ sử dụng `XDG_CONFIG_HOME` / `XDG_CACHE_HOME` khi được thiết lập.

| Đường dẫn | Mục đích |
| --- | --- |
| `~/.chloros/user_session.json` | Bộ nhớ đệm phiên đăng nhập được ghi bởi `chloros-cli login` (được giữ lại qua các lần nâng cấp gói; bị xóa khi gói được gỡ bỏ) |
| `~/.chloros/working_directory.txt` | Ghi đè thư mục dự án mặc định (`chloros-cli set-project-folder` / `get-project-folder` / `reset-project-folder`) |
| `~/.chloros/cli_language.json` | Tùy chọn ngôn ngữ giao diện dòng lệnh (CLI) (`chloros-cli language <code>`) |
| `~/.chloros/user.json` | Cài đặt ngôn ngữ được chia sẻ với giao diện người dùng đồ họa (GUI) của Windows — giá trị `language` ở đây có ưu tiên cao hơn `cli_language.json` |
| `~/.chloros/update_cache.json` | Bộ nhớ đệm trong một giờ cho việc kiểm tra cập nhật khởi động trên Linux/Jetson |
| `~/.chloros/backend.log` | Nhật ký backend khi backend được khởi chạy bởi CLI |
| `~/.chloros/camera_cal/<serial>/<bundle_sha>/` | Các gói hiệu chuẩn LATTICE được lưu trong bộ nhớ đệm cho từng camera, được chỉ định bằng số sê-ri và băm gói |
| `~/.chloros/daq_cap_profiles/<u\|m\|e>/<cap_id>.json` | Các tùy chỉnh tùy chọn của người dùng cho các cấu hình hiệu chỉnh giới hạn DAQ |
| `~/.config/chloros/system_config.json` | Hồ sơ phần cứng được lưu trong bộ nhớ đệm từ Dynamic Compute Adaptation — xóa hồ sơ này để buộc thực hiện phát hiện phần cứng mới |
| `~/.cache/chloros/logs/backend_<YYYYMMDD_HHMMSS>.log` | Nhật ký máy chủ backend, mỗi lần khởi chạy tương ứng với một tệp |
| `~/Documents/MAPIR/Chloros Projects/` | Thư mục dự án mặc định khi không có thiết lập thay thế nào |

### Toàn hệ thống

| Đường dẫn | Mục đích |
| --- | --- |
| `/usr/bin/chloros-cli` | Tập lệnh bọc — thiết lập `LD_LIBRARY_PATH` cho các thư viện gốc đi kèm (và `TMPDIR` thành `/mnt/ssd/tmp` khi thư mục đó tồn tại), sau đó thực thi tệp nhị phân thực |
| `/usr/bin/chloros-backend` | Tập lệnh bọc — tương tự đối với phần backend, mà nó đánh dấu là bản dựng sản xuất đã cài đặt |
| `/usr/lib/chloros/cli/`, `/usr/lib/chloros/backend/` | Giao diện dòng lệnh (CLI) và phần backend đã biên dịch, được cài đặt ở dạng giải nén (tính năng mới trong phiên bản 1.3.0); các chương trình là `cli/chloros-cli` và `backend/chloros-backend` |
| `/usr/lib/chloros/chloros-cli`, `/usr/lib/chloros/chloros-backend` | Các tập lệnh khởi chạy hai dòng để khởi động các chương trình thực tế ở trên — được giữ tại các đường dẫn này vì SDK, CLI và các tập lệnh cũ tìm kiếm chúng tại đây |
| `/usr/lib/chloros/arena_runtime/` | Môi trường chạy SDK của Arena cần thiết cho các camera LATTICE |
| `/usr/lib/chloros/models/*.pth.enc` | Các mô hình khử nhiễu được mã hóa được sử dụng bởi bộ giải mã pixel có nhận biết kết cấu (Texture Aware debayer) |
| `/usr/lib/python3/dist-packages/chloros_sdk/` (và `lattice_sdk`, `lattice_cam`, `arena_api`, các trình tải dự án) | Bộ công cụ phát triển phần mềm Python (SDK), được cài đặt cho hệ thống `python3` và thuộc về gói |
| `/usr/lib/chloros/sdk/chloros_sdk-*.whl` | Tệp wheel của bộ công cụ phát triển phần mềm Python (SDK) khớp chính xác với bản dựng này, dành cho môi trường ảo |
| `/usr/lib/chloros/exiftool` | Trình khởi chạy ExifTool đi kèm (chỉ được liên kết tượng trưng đến `/usr/local/bin/exiftool` nếu không có exiftool hệ thống). Kể từ phiên bản 1.3.0, tập lệnh ExifTool được cung cấp kèm theo thư viện Perl riêng trong `/usr/lib/chloros/exiftool-dist/`, do đó nó chạy mà không cần gói hệ thống `libimage-exiftool-perl` |
| `/etc/chloros/update.conf` | Cấu hình kênh cập nhật được `chloros-cli update` đọc |
| `/etc/sysctl.d/60-chloros-ptp.conf` | Đặt `net.ipv4.ip_unprivileged_port_start = 319` để phần hậu kỳ có thể gán các cổng PTP mà không cần quyền root |
| `/etc/sysctl.d/60-chloros-gige.conf` | Tăng kích thước bộ đệm nhận socket của kernel (`net.core.rmem_max`, `net.core.rmem_default`) lên 32 MB cho luồng dữ liệu từ camera LATTICE, và áp dụng các thiết lập này khi cài đặt (nếu giá trị đã được thiết lập cao hơn thì sẽ giữ nguyên). Theo mặc định của kernel, các gói tin từ camera sẽ bị kernel loại bỏ và mảng sẽ mất các khung hình cũng như tốc độ khung hình; `chloros-cli lattice network` sẽ báo cáo khi bộ đệm còn ít. Xem [Máy chủ Linux — bộ đệm nhận socket](../reference/cli-reference.md#linux-hosts--socket-receive-buffers) |
| `/etc/ld.so.conf.d/Arena_SDK.conf` | Chỉ định trình tải động đến `/usr/lib/chloros/arena_runtime` |
| `/lib/udev/rules.d/70-chloros-daq.rules` | Cấp quyền truy cập cho người dùng đã đăng nhập vào cầu nối nối tiếp USB DAQ-U (CP2102N, `10c4:ea60`) |
| `/lib/systemd/system/chloros-backend@.service` | Dịch vụ nền luôn hoạt động (opt-in) chạy nền dưới danh nghĩa người dùng mà bạn chỉ định (đã cài đặt, **chưa được kích hoạt**) — đây là dịch vụ cần kích hoạt, xem [Always-On PTP](#always-on-ptp-for-headless-hosts) |
| `/lib/systemd/system/chloros-backend.service` | Dịch vụ tương tự chạy phần nền dưới quyền **root**(đã cài đặt,**chưa được kích hoạt**), được giữ lại để máy tính đã kích hoạt nó tiếp tục hoạt động |
| `/var/log/chloros/` | Nhật ký chi tiết của dịch vụ root, có thể được đọc bởi nhóm `adm` (bị xóa khi gói phần mềm bị xóa hoàn toàn) |
| `/usr/share/applications/chloros-cli.desktop` | Mục menu ứng dụng &quot;Chloros CLI&quot; dùng để mở terminal |

**Tường lửa (mới trong phiên bản 1.3.0).** Nếu `ufw` hoặc `firewalld` đang hoạt động tại thời điểm cài đặt, gói phần mềm sẽ mở các cổng UDP `5002` (dữ liệu DAQ-E), `5353` (phát hiện mDNS), `319` và `320` (PTP), và xóa các quy tắc đó khi gói phần mềm bị gỡ bỏ. Nếu tường lửa đã được cài đặt nhưng không hoạt động, nó sẽ không thêm bất kỳ quy tắc nào và hiển thị danh sách các cổng cần mở trong trường hợp bạn kích hoạt tường lửa sau này. Nó không mở bất kỳ cổng TCP nào: việc cập nhật firmware DAQ-E, trong đó cảm biến kết nối trở lại với máy tính này qua TCP để thu thập hình ảnh, sẽ thất bại với mã lỗi `… device never connected back on TCP <port> …` khi tường lửa đang hoạt động chặn kết nối đó.

## Vị trí tệp thực thi backend

CLI và SDK tự động phát hiện backend:

| Thành phần | Đường dẫn |
| --- | --- |
| CLI | `/usr/bin/chloros-cli` |
| Backend | `/usr/lib/chloros/chloros-backend` (trình khởi chạy cho `/usr/lib/chloros/backend/chloros-backend`) |

Ghi đè đường dẫn backend bằng cờ CLI `--backend-exe` hoặc tham số hàm tạo SDK `backend_exe`, và cổng bằng `--port` (mặc định là `5000`).

{% hint style="info" %}
`CHLOROS_BACKEND_URL` chỉ định các nhóm lệnh **`lattice`**,**`project`**và**`daq pool-*`** tại một backend từ xa. Các lệnh cốt lõi (`process`, `login`, `logout`, `status`, `export-status`, `time-sync`, `selftest`) sẽ cố ý bỏ qua thông số này và luôn nhắm đến `http://127.0.0.1:<port>`.
{% endhint %}

***

## Camera LATTICE và cảm biến ánh sáng DAQ trên Linux

Tất cả các nhóm lệnh live-hardware đều hoạt động trên Linux (amd64 và Jetson):

* **`chloros-cli lattice`** — phát hiện, kết nối, cấu hình và thu thập dữ liệu từ camera LATTICE và các mảng đồng bộ. `.deb` gói sẵn môi trường chạy Arena SDK mà chúng yêu cầu và đăng ký nó với trình tải động.
* **`chloros-cli daq pool-*`** — kết nối các cảm biến ánh sáng DAQ-U/M/E thông qua nhóm backend, truyền dữ liệu quang phổ đã hiệu chuẩn, ghi tệp `.daq` và thay đổi cấu hình cap, đồng thời đối với DAQ-E, có thể thay đổi cài đặt mạng, khởi động lại và cập nhật phần mềm hệ thống của thiết bị. `chloros-cli daq discover` quét các cảm biến mà bạn có thể kết nối, và `chloros-cli daq --help` liệt kê các lệnh con `pool-*`.
* **`chloros-cli project`** — chạy một dự án đã lưu (bao gồm camera, cảm biến và cài đặt xử lý) ở chế độ không giao diện.
* **`chloros-cli time-sync`** — kiểm tra máy chủ chính PTP mà backend Chloros chạy cho các camera LATTICE và cảm biến DAQ-E.

```bash
# DAQ-E at a known address — the reliable path on multi-homed hosts
chloros-cli daq pool-connect --eth-host 192.168.2.50

# DAQ-U over USB serial
chloros-cli daq pool-connect --port /dev/ttyUSB0

# What is connected, then the latest calibrated spectrum as JSON
chloros-cli daq pool-list
chloros-cli daq pool-latest --sensor-id daq-e-a1b2c3 --json
```

`--sensor-id` là điều kiện tiên quyết cho `pool-latest`, `pool-stream`, `pool-record` và `pool-set-cap`; `pool-list` hiển thị các ID hiện có trong nhóm.

{% hint style="info" %}
**`--eth-host` bỏ qua quá trình phát hiện.** `pool-connect --eth` tìm thấy một DAQ-E thông qua mDNS, với cơ chế dự phòng ARP hoạt động từ bộ nhớ đệm ARP trống; việc truyền địa chỉ IP hoặc tên máy chủ của cảm biến cùng với `--eth-host` sẽ kết nối trực tiếp với nó.
{% endhint %}

**Quyền truy cập cổng nối tiếp DAQ-U** được quản lý bởi quy tắc udev đã cài đặt (`uaccess` + nhóm `dialout`). Nếu một cảm biến đã được cắm vào nhưng vẫn không thể truy cập được, hãy tải lại các quy tắc hoặc cắm lại cảm biến:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger --subsystem-match=tty
```

Xem [tài liệu tham khảo CLI](../CLI.md) để biết đầy đủ các lệnh.

### PTP luôn hoạt động cho các máy chủ không có giao diện đồ họa

Gói phần mềm này cài đặt hai dịch vụ systemd cho phần backend, cả hai đều chưa được kích hoạt. Trên một thiết bị Jetson không màn hình hoặc máy chủ cần duy trì đồng bộ thời gian PTP liên tục cho các cảm biến DAQ-E và camera LATTICE, hoặc trong trường hợp các tập lệnh SDK kết nối các dự án đã lưu (`connect_all()` không bao giờ khởi động phần nền), hãy kích hoạt dịch vụ **với tư cách người dùng của bạn**:

```bash
sudo systemctl enable --now chloros-backend@$USER.service    # $USER expands to your user name
systemctl status chloros-backend@$USER.service
```

`chloros-backend@<user>.service` chạy phần backend dưới danh nghĩa người dùng đó: các dữ liệu ghi lại do nó tạo ra thuộc về người dùng đó, nhật ký chi tiết của nó nằm trong tệp `~/.cache/chloros/logs` của người dùng đó, và nó sử dụng tệp `chloros-cli login` (`~/.chloros`), vì vậy hãy đăng nhập một lần với tài khoản đó trước. Quá trình cài đặt gói phần mềm sẽ hiển thị lệnh này với tên người dùng của bạn được điền sẵn.

Nếu không có dịch vụ, phần backend (và cùng với đó là máy chủ chính PTP) chỉ chạy khi có thứ gì đó khởi động nó — một lệnh `chloros-cli` hoặc một tập lệnh SDK.

Thiết bị này liên kết phần backend với các thiết lập môi trường `127.0.0.1:5000` (các thiết lập môi trường `CHLOROS_HOST` / `CHLOROS_PORT` bên trong đơn vị; ghi đè bằng `sudo systemctl edit chloros-backend@.service`) và khởi động lại nó sau 5 giây nếu xảy ra lỗi.

**Dịch vụ root.**`chloros-backend.service` chạy backend với quyền**root**. Nó vẫn được cài đặt để đảm bảo máy đã kích hoạt dịch vụ này tiếp tục hoạt động, nhưng mọi dữ liệu mà phần nền root ghi vào đều thuộc về root, do đó người dùng không thể xóa các bản ghi của chính mình; nhật ký của nó nằm trong tệp `/var/log/chloros`, có thể đọc được bởi nhóm `adm`. Để chuyển sang tài khoản người dùng của bạn:

```bash
sudo systemctl disable --now chloros-backend.service
sudo systemctl enable --now chloros-backend@$USER.service
```

**Backend của một dịch vụ là chung, không bao giờ bị khách hàng dừng lại.** Các tập lệnh `chloros-cli` và SDK sử dụng nó khi tìm thấy: `process` chạy trên đó, tải dự án của chính nó thay thế cho dự án cuối cùng. Trong trường hợp bình thường, giao diện dòng lệnh (CLI) sẽ khởi động lại phần nền (`--restart`, hoặc một `process` mà quá trình nhập không tìm thấy gì), nó sẽ hiển thị tên đơn vị, in ra `sudo systemctl restart <unit>` và thoát khỏi `1`, vì một backend bị dừng theo cách đó sẽ không khởi động lại. Hãy tự khởi động lại bằng lệnh `sudo systemctl restart chloros-backend@$USER.service`. Backend của một dịch vụ không bao giờ tự dừng khi ở trạng thái nhàn rỗi, và `chloros-cli backend stop` từ chối điều đó, đồng thời hiển thị tên lệnh `systemctl stop`.**Cách PTP lấy cổng của mình.** PTP sử dụng UDP 319/320, cả hai đều nằm dưới ngưỡng 1024 cổng ưu tiên thông thường. Gói phần mềm `postinst` ghi `/etc/sysctl.d/60-chloros-ptp.conf` cùng với `net.ipv4.ip_unprivileged_port_start = 319`, cho phép phần nền (backend) gán các cổng này khi chạy dưới quyền người dùng của bạn. Kể từ phiên bản 1.3.0, gói phần mềm không thiết lập bất kỳ quyền truy cập tệp nào (`setcap`) trên phần mềm nền, và `libcap2-bin` không còn là yêu cầu phụ thuộc: trên phần mềm nền đã giải nén, quyền truy cập tệp sẽ ngăn không cho nó khởi động, vì vậy đừng tự thêm quyền này.***

## Ví dụ về kịch bản Bash

{% hint style="info" %}
**Mã thoát thân thiện với kịch bản.**`chloros-cli process` sẽ trả về mã thoát `0` khi thành công và**mã không bằng 0 khi thất bại — bao gồm cả trường hợp yêu cầu sản phẩm hình ảnh nhưng không ghi được sản phẩm nào** (nó hiển thị mã `Processing finished but wrote no image products.` cùng tên thư mục dự án và các nguyên nhân thường gặp). Các lần chạy thành công báo cáo số lượng sản phẩm hình ảnh đã được ghi (`Image products written: N`, cộng với `(plus M clamp masks)` khi các mặt nạ được ghi). Mã thoát: `0` (thành công), `1` (thất bại), `2` (lỗi tham số), `130` (bị gián đoạn).
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
    --indices NDVI GNDVI OSAVI \
    --debayer texture-aware \
    --no-vignette
```

Các giá trị `--format` hợp lệ chỉ có đúng bốn giá trị và chúng chứa khoảng trắng — luôn phải đặt chúng trong dấu ngoặc kép:

| Giá trị `--format` | Thư mục đầu ra |
| --- | --- |
| Giá trị `TIFF (16-bit)` *(mặc định)* | `tiff16` |
| `TIFF (8-bit)` | `tiff8` |
| `TIFF (32-bit, Percent)` | `tiff32` |
| `PNG (8-bit)` | `png8` |
| `JPG (8-bit)` | `jpg8` |

`--debayer` chấp nhận `standard` (mặc định) hoặc `texture-aware` (Chloros+).

### Xử lý tự động với Cron

```cron
# Process any new datasets at 2 AM daily
0 2 * ** /usr/bin/chloros-cli process /data/incoming --output /data/processed >> /var/log/chloros.log 2>&1
```

### Ví dụ về PythonSDK

```python
from chloros_sdk import process_folder

# One-line processing
result = process_folder(
    "/home/user/datasets/flight001",
    indices=["NDVI", "OSAVI"],
    export_format="TIFF (32-bit, Percent)"
)
```

***

## Khắc phục sự cố

### Không tìm thấy CLI sau khi cài đặt

```bash
# Check if the binary exists
which chloros-cli
ls -la /usr/bin/chloros-cli

# List everything the package installed
dpkg -L chloros

# Reload your shell
source ~/.bashrc
```

### Bị từ chối quyền truy cập

```bash
sudo chmod +x /usr/bin/chloros-cli
sudo chmod +x /usr/lib/chloros/chloros-backend /usr/lib/chloros/backend/chloros-backend
```

### Backend không khởi động được sau khi thêm quyền truy cập tệp

Không chạy `setcap` trên backend. Từ phiên bản 1.3.0 trở đi, gói phần mềm không thiết lập bất kỳ khả năng tệp nào, và các cổng 319/320 của PTP được xác định bởi cài đặt `sysctl` được mô tả trong phần [Cách PTP lấy các cổng của mình](#always-on-ptp-for-headless-hosts). Một khả năng tệp sẽ đặt backend đã giải nén vào chế độ thực thi an toàn, trong đó nó không thể tìm thấy các thư viện của chính mình, do đó nó sẽ không khởi động. Nếu bạn đã thêm một khả năng tệp, hãy xóa nó:

```bash
sudo setcap -r /usr/lib/chloros/backend/chloros-backend
```

### PTP không khởi động / Không thể gán cổng 319

Xác nhận rằng ngưỡng cổng không đặc quyền đã được hạ xuống, và áp dụng lại cho lần khởi động hiện tại nếu chưa thực hiện:

```bash
sysctl net.ipv4.ip_unprivileged_port_start     # expect 319
sudo sysctl -w net.ipv4.ip_unprivileged_port_start=319
```

Sau đó, kiểm tra grandmaster:

```bash
chloros-cli time-sync status
chloros-cli time-sync peers
```

### Các lệnh `lattice` thất bại với thông báo &quot;Không tìm thấy trình điều khiển camera LATTICE&quot;

Backend không thể tải runtime camera LATTICE đi kèm khi cài đặt. Mọi lệnh `lattice` đều chạy trên backend; chính giao diện dòng lệnh (CLI) không chứa trình điều khiển camera nào. Cài đặt lại Chloros (`sudo apt install --reinstall ./chloros_1.3.0_<arch>.deb`): lệnh `.deb` sẽ cài đặt runtime bên cạnh phần backend, trong thư mục `/usr/lib/chloros/arena_runtime`, và đăng ký nó với trình tải động. Để kiểm tra việc đăng ký này:

```bash
cat /etc/ld.so.conf.d/Arena_SDK.conf     # expect /usr/lib/chloros/arena_runtime
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

Nhật ký backend cho lần khởi động thất bại nằm trong tệp `~/.cache/chloros/logs/`. Nếu cổng 5000 thuộc về một dịch vụ backend (`systemctl status chloros-backend@$USER.service`), hãy khởi động lại dịch vụ thay vì kết thúc quá trình của nó.

### Không phát hiện thấy CUDA

```bash
# Check NVIDIA driver installation (x86_64)
nvidia-smi

# Check CUDA availability
nvcc --version

# On Jetson, check the L4T release (JetPack 6 is R36) and that JetPack's CUDA libraries are installed
cat /etc/nv_tegra_release
sudo apt install nvidia-jetpack
```

`chloros-cli selftest` báo cáo cùng một thông tin trong một dòng: `GPU: <name>, CUDA: <bool>, PyTorch: <version>`.

### Thiếu thư viện chia sẻ

```bash
sudo apt-get update
sudo apt-get install -f

# Check for missing libraries (the program itself, not the launcher script)
ldd /usr/lib/chloros/backend/chloros-backend | grep "not found"
```

### Khởi động chậm trên hệ thống sử dụng thẻ SD

Từ phiên bản 1.3.0 trở đi, dịch vụ nền và giao diện dòng lệnh (CLI) được cài đặt ở dạng chưa giải nén, do đó chúng không còn tự giải nén vào thư mục tạm thời mỗi khi khởi động. Các tệp tạm thời sẽ tự động được lưu vào thư mục `/mnt/ssd/tmp` nếu thư mục đó tồn tại; nếu không, bạn có thể chỉ định chúng vào một hệ thống tệp nhanh:

```bash
export TMPDIR=/mnt/nvme/tmp
```

***

## Cập nhật Chloros trên Linux

Lệnh `update` chỉ dành cho Linux/Jetson. Lệnh này kiểm tra phiên bản được công bố trong kênh cập nhật được cấu hình tại `/etc/chloros/update.conf` và đề xuất tải xuống và cài đặt phiên bản tương ứng `.deb`:

```bash
# Check for updates without installing
chloros-cli update --check

# Check for and install updates
chloros-cli update
```

Trên Linux/Jetson, giao diện dòng lệnh (CLI) cũng thực hiện kiểm tra cập nhật không chặn mỗi khi khởi động (kết quả được lưu trong bộ nhớ đệm trong một giờ tại `~/.chloros/update_cache.json`) và hiển thị thông báo `Update available: vX.Y.Z` khi có phiên bản mới hơn. Các cài đặt, dự án và thông tin đăng nhập của bạn sẽ được giữ nguyên sau khi cập nhật.

Việc nâng cấp gói phần mềm sẽ tạm dừng mọi dịch vụ nền trong quá trình cài đặt, do đó một skript đang chạy sẽ mất kết nối với các camera và cảm biến, sau đó khởi động lại từng dịch vụ đã được kích hoạt (một dịch vụ được khởi động thủ công nhưng chưa bao giờ được kích hoạt sẽ vẫn bị dừng, giống như sau khi khởi động lại hệ thống).

## Gỡ cài đặt

```bash
sudo apt remove chloros
```

Quá trình gỡ cài đặt sẽ dừng và vô hiệu hóa mọi dịch vụ nền, khôi phục ngưỡng cổng không đặc quyền mặc định (1024), xóa tệp cấu hình bộ đệm nhận (các giá trị đang chạy sẽ được giữ lại cho đến khi khởi động lại), liên kết tượng trưng exiftool đi kèm, cấu hình trình tải Arena, các quy tắc tường lửa mà gói phần mềm đã thêm vào và Python SDK mà nó đã cài đặt, đồng thời xóa các thông tin đăng nhập được lưu trong bộ nhớ đệm. Các dự án và tệp dữ liệu `~/.chloros/` của bạn sẽ không bị ảnh hưởng. `sudo apt purge chloros` cũng xóa `/var/log/chloros`.

***

## Các bước tiếp theo

* [Hướng dẫn NVIDIA Jetson](nvidia-jetson-guide.md) — Tối ưu hóa và triển khai dành riêng cho Jetson
* [CLI: Dòng lệnh](../CLI.md) — hướng dẫn về CLI
* [API: Python SDK](../api-python-sdk.md) — hướng dẫn về SDK
* [Tham chiếu CLI](../reference/cli-reference.md) và [Tham chiếu SDK](../reference/sdk-reference.md) — danh sách đầy đủ các lệnh/API cho phiên bản 1.3.0
* [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md) — cách Chloros thích ứng với phần cứng của bạn
