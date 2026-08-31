# Tài liệu tham khảo về Chloros CLI

**Phiên bản:**

1.2.0**Ngày tạo:**29/07/2026 19:19 ·**Ngày sửa đổi:** 30/08/2026**Đối tượng:** Được tối ưu hóa cho việc sử dụng bởi các mô hình ngôn ngữ lớn (LLM); dễ đọc đối với con người.**Phạm vi:** Mọi lệnh con dành cho người dùng của `chloros-cli`, kèm theo các tùy chọn và ví dụ có thể sao chép và dán.

Tài liệu này là tài liệu tham khảo đầy đủ cho công cụ dòng lệnh `chloros-cli` đi kèm với MAPIR Chloros. Tài liệu này được thiết kế để bao quát toàn diện nhằm giúp LLM (hoặc con người) có thể tạo ra bất kỳ quy trình làm việc nào được hỗ trợ từ các danh sách dưới đây mà không cần kiểm tra mã nguồn.

Nếu bạn chỉ cần những điểm chính, hãy chuyển đến:
- [Hướng dẫn bắt đầu nhanh trong 5 phút](#five-minute-quickstart)
- [Quy trình kết nối lần đầu với camera LATTICE](#lattice-camera-first-connect-workflow)
- [Quy trình kết nối lần đầu với cảm biến DAQ](#daq-sensor-first-connect-workflow)
- [Smart-AE / Smart-Capture](#smart-ae--smart-capture)
- [Chế độ ghi, thiết bị ghi và xử lý lại ngoại tuyến](#capture-modes-recorders--offline-reprocess)

---

## Quy ước

- Tất cả các lệnh đều có tiền tố `chloros-cli`. Trên Windows, tệp nhị phân là `chloros-cli.exe`; trên Linux /Jetson, tệp nhị phân là `chloros-cli`.
- Các tham số tùy chọn được hiển thị dưới dạng `--flag`. Các tham số vị trí bắt buộc được hiển thị mà không có dấu ngoặc vuông.
- Khi có giá trị mặc định, việc bỏ qua cờ sẽ sử dụng giá trị đó.
- CLI là một client HTTP nhẹ chạy trên backend Chloros (máy chủ Flask trên `127.0.0.1:5000`). Hệ thống nền được khởi động tự động bởi hầu hết các lệnh. `CHLOROS_BACKEND_URL=<url>` trỏ đến **`lattice`**,**`project`**và**`daq pool-*`** đến một backend từ xa — các lệnh cốt lõi (`process`, `login`, `logout`, `status`, `export-status`, `time-sync`, `selftest`) cố ý khóa `http://127.0.0.1:<port>` và bỏ qua nó (giá trị IPv4 cố định giúp tránh mức phạt khoảng Windows&#x27; `localhost`→`::1` ~2 giây phạt cho mỗi yêu cầu). Xem [Biến môi trường](#environment-variables).
- Cần đăng nhập bằng tài khoản Chloros+ cho tất cả các lệnh gọi SDK / CLI (chạy `chloros-cli login` một lần trên mỗi máy; được lưu trong bộ nhớ đệm tại `~/.chloros/`).
- Các ví dụ sử dụng đường dẫn Linux; trên Windows, hãy thay thế `/home/user/...` bằng `C:/Users/.../...`.

---

## Tóm tắt cấp cao nhất

```
chloros-cli [global options] COMMAND [command options]
```

### Tùy chọn toàn cục

| Cờ | Mô tả |
| --- | --- |
| `--backend-exe PATH` | Ghi đè tệp thực thi backend được tự động phát hiện. |
| `--port N` | Cổng HTTP của backend (mặc định: `5000`). |
| `-v, --verbose` | Bật chế độ hiển thị chi tiết. |
| `--restart` | Buộc khởi động lại backend (kết thúc mọi tiến trình `backend_server.py` đang chạy). |
| `--version` | Hiển thị phiên bản (`Chloros CLI 1.2.0`). |
| `--help` | Hiển thị hướng dẫn sử dụng cấp cao nhất. |

### Chỉ mục lệnh

| Lệnh | Mục đích |
| --- | --- |
| [`process`](#chloros-cli-process) | Xử lý một thư mục chứa các bản ghi Survey3 hoặc LATTICE từ đầu đến cuối. |
| [`login`](#chloros-cli-login) | Xác thực máy này bằng tài khoản Chloros+. |
| [`logout`](#chloros-cli-logout) | Xóa thông tin đăng nhập được lưu trong bộ nhớ đệm. |
| [`status`](#chloros-cli-status) | Hiển thị trạng thái giấy phép / xác thực hiện tại. |
| [`export-status`](#chloros-cli-export-status) | Hiển thị tiến trình xuất dữ liệu Thread-4 theo thời gian thực trong quá trình chạy `process`. |
| [`language`](#chloros-cli-language) | Đặt hoặc liệt kê ngôn ngữ hiển thị củCLI (hỗ trợ 38 ngôn ngữ). |
| [`set-project-folder`](#project-folder-commands) / [`get-project-folder`](#project-folder-commands) / [`reset-project-folder`](#project-folder-commands) | Thư mục dự án mặc định (chia sẻ với GUI). |
| [`update`](#chloros-cli-update) | Kiểm tra và cài đặt các bản cập nhật CLI (Linux /Jetson). |
| [`selftest`](#chloros-cli-selftest) | Chẩn đoán hệ thống + kiểm tra hoạt động cơ bản. |
| [`time-sync`](#chloros-cli-time-sync) | Trạng thái / kiểm soát grandmaster PTP. |
| [`lattice`](#chloros-cli-lattice) | Điều khiển và ghi hình camera LATTICE (hơn 45 lệnh con). |
| [`daq`](#chloros-cli-daq) | Điều khiển cảm biến quang phổ DAQ (DAQ-U / DAQ-M / DAQ-E). |
| [`project`](#chloros-cli-project) | Mở và điều khiển dự án đã lưu trên Chloros (máy ảnh + DAQ). |

---

## Cài đặt

`chloros-cli` được tích hợp sẵn trong trình cài đặt máy tính để bàn Chloros trên mọi nền tảng được hỗ trợ — không có bản tải xuống riêng biệt tại CLI. Việc cài đặt gói nền tảng sẽ thêm `chloros-cli` vào hệ thống của bạn cùng với ứng dụng máy tính để bàn và tệp nhị phân nền tảng mà nó điều khiển.

Tải xuống mới nhất: [`https://mapir.gitbook.io/chloros/download`](https://mapir.gitbook.io/chloros/download)

> Trình cài đặt cũng đi kèm với các tập lệnh khởi chạy tiện lợi (`Chloros_CLI.bat` / `Chloros_CLI.ps1`, `Launch_CLI.*`, `chloros-cli.sh`) để mở giao diện dòng lệnh CLI sẵn sàng sử dụng; các tập lệnh này được đề cập trong [CLI Hướng dẫn sử dụng](../CLI.md) và không được lặp lại ở đây.

### Windows (.exe)

1. Tải xuống trình cài đặt Windows từ trang tải xuống.
2. Chạy `Chloros-Setup-x.y.z.exe` và làm theo hướng dẫn của trình hướng dẫn. Đường dẫn cài đặt mặc định là `C:\Program Files\Chloros\` (thư mục &quot;CLI&quot; sẽ được đặt tại `C:\Program Files\Chloros\cli\`, mà trình cài đặt sẽ thêm vào biến môi trường PATH).
3. Mở một cửa sổ terminal mới (`cmd.exe`, PowerShell hoặc Terminal củWindows) để hệ thống tự động chọn thư mục `PATH` đã được cập nhật .

```powershell
chloros-cli --version
```

Trình cài đặt sẽ tự động thêm `chloros-cli.exe` vào đường dẫn hệ thống `PATH` của bạn và gói kèm runtime Arena SDK cần thiết cho các camera LATTICE.

### Linux amd64 (.deb)

Dành cho Ubuntu 22.04 LTS trở lên / các máy trạm x86_64 dựa trên Debian.

> **Ubuntu 20.04 không được hỗ trợ.** Danh sách phụ thuộc của gói phần mềm được xác định dựa trên
> những gì mà phần mềm nền tảng thực sự liên kết đến, và điều đó bao gồm `libc6 (>= 2.34)`;
> Focal cung cấp glibc 2.31. `apt` từ chối cài đặt thay vì để nó thất bại tại
> thời điểm chạy.

```bash
sudo dpkg -i chloros-amd64.deb
sudo apt-get install -f         # only if dpkg reports missing dependencies
chloros-cli --version
```

Gói .deb cài đặt:
- `chloros-cli` thành `/usr/bin/chloros-cli`
- Phần backend đã biên dịch thành `/usr/lib/chloros/chloros-backend`
- Môi trường chạy Arena SDK (dành cho camera LATTICE)
- Các mô hình khử nhiễu, gói hiệu chuẩn và cấu hình kênh cập nhật

### Linux arm64 — Jetson (JetPack 6)

```bash
sudo dpkg -i chloros-arm64-jp6.deb
sudo apt-get install -f
chloros-cli --version
```

Cấu trúc tương tự như tệp .deb, với bản dựng CUDA được tối ưu hóa cho Jetson Orin / Orin NX / Orin Nano.

### Xác thực một lần cho mỗi máy

Mỗi nền tảng yêu cầu đăng nhập một lần tại Chloros+ trước khi các lệnh SDK / CLI có thể hoạt động:

```bash
chloros-cli login user@example.com 'YourPassword'
```

Thông tin đăng nhập được lưu trong bộ nhớ đệm tại `~/.chloros/user_session.json`.

### Xác minh cài đặt

```bash
chloros-cli --version           # prints "Chloros CLI 1.2.0"
chloros-cli selftest            # full 7-step diagnostic (backend, GPU, models, CUDA)
chloros-cli status              # shows license tier + logged-in user
```

> **Yêu cầu đăng ký Chloros+.**Dịch vụ CLI yêu cầu gói Chloros+ đang hoạt động.**Copper**là gói cơ bản Chloros+ — mọi gói trả phí Chloros+ đều có quyền truy cập vào CLI / SDK; chỉ có gói miễn phí**Iron** là không có. (Bản đồ-id: `0`=Iron/free, `1`=Copper, `2`=Bronze, `3`=Silver, `4`=Gold.) Nâng cấp tại [`https://cloud.mapir.camera/pricing`](https://cloud.mapir.camera/pricing).
>
> Giới hạn này được áp dụng bởi hệ thống backend, không chỉ bởi CLI: một yêu cầu có cờ SDK / CLI mà không có gói dịch vụ trả phí sẽ bị từ chối với mã lỗi `403 PLAN_UPGRADE_REQUIRED`, bất kể yêu cầu đó đến từ `chloros-cli`, Python SDK, hay một client HTTP tự phát triển. Người gọi đã đăng xuất sẽ nhận được mã lỗi `401 AUTH_REQUIRED`. Quyền truy cập vẫn hoạt động ngoại tuyến trong thời gian ân hạn của gói cước (30 ngày đối với gói hàng tháng, đến ngày hết hạn đối với gói hàng năm) và sẽ ngừng khi thời gian này kết thúc; `chloros-cli status` vẫn tiếp tục hoạt động để lý do được hiển thị (đây là đường dẫn duy nhất SDK / CLI được miễn trừ khỏi cơ chế giới hạn cấp độ — `GET /api/license-status`).

---

## Hướng dẫn nhanh trong 5 phút

```bash
# 1. Sign in once on this machine
chloros-cli login user@example.com 'YourPassword'

# 2. Survey3 / LATTICE folder → finished radiance + NDVI in one call
chloros-cli process "/home/user/captures/flight_001" \
  --vignette --reflectance --indices NDVI NDRE GNDVI

# 3. Take a single LATTICE photo with the first camera found
chloros-cli lattice capture -o output/

# 4. Connect a 4-cam LATTICE array with the GUI's smart-prep flow
chloros-cli lattice array-connect \
  --serials 213800234,214000533,214701288,214701292

# 5. Read a spectrum from a connected DAQ-U
chloros-cli daq pool-connect --port COM3
chloros-cli daq pool-latest --sensor-id CB-7C-A8-2E-5F   # id from 'daq pool-list'
```

---

## `chloros-cli process`

Xử lý một thư mục hình ảnh qua toàn bộ quy trình Chloros (phát hiện mục tiêu → hiệu chỉnh → hiệu ứng viền → phản xạ → xuất chỉ số).

### Tóm tắt

```
chloros-cli process INPUT [OPTIONS]
```

### Đối số vị trí

| Đối số | Mô tả |
| --- | --- |
| `INPUT` | Đường dẫn đến thư mục đầu vào chứa các tệp `.raw + .jpg` (Survey3), `.tif/.tiff` (LATTICE), hoặc các tệp `.dng`. |

### Tùy chọn chung

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `-o, --output PATH` | một thư mục mới có dấu thời gian dưới đường dẫn dự án mặc định của bạn (`~/Chloros Projects` trừ khi được cấu hình khác) | Thư mục dự án để tạo mới hoặc tái sử dụng. Nếu thư mục đó đã chứa tệp `project.json`, một thư mục cùng cấp có tên `_1`/`_2` sẽ được tạo thay vì ghi đè. |
| `-n, --project-name NAME` | tự động (dấu thời gian) | Tên dự án. |
| `--debayer {standard,texture-aware}` | `standard` | `texture-aware` sử dụng thuật toán khử hiệu ứng debayer thần kinh Chloros+; chậm hơn nhưng chất lượng cao hơn. |
| `--vignette / --no-vignette` | `--vignette` | Chỉnh sửa hiện tượng tối góc. |
| `--reflectance / --no-reflectance` | `--reflectance` | Hiệu chuẩn độ phản xạ (sử dụng mục tiêu bảng nếu tìm thấy, hiệu chuẩn theo số sê-ri của NIST cho LATTICE). Đối với LATTICE đa quang phổ, tính năng này cũng đóng vai trò là công tắc độ phản xạ **sản phẩm** — xem [Công tắc xuất theo từng sản phẩm](#per-product-export-toggles-lattice-multispectral). |
| `--ppk` | tắt | Áp dụng các hiệu chỉnh PPK GNSS từ các tệp sidecar. |
| `--exposure-pin-1 MODEL` | tắt | Khóa mô hình “pin-1” của bộ thiết bị camera kép Survey3. |
| `--exposure-pin-2 MODEL` | tắt | Khóa mô hình “pin-2”. |
| `--recal-interval SECONDS` | 0 | Buộc thực hiện lại các phép tính hiệu chuẩn cứ sau N giây trong thời gian ghi hình. |
| `--timezone-offset HOURS` | local | Ghi đè độ lệch múi giờ được tích hợp sẵn trong siêu dữ liệu đầu ra. |
| `--format FORMAT` | `TIFF (16-bit)` | Một trong các giá trị sau: `TIFF (16-bit)`, `TIFF (32-bit, Percent)`, `PNG (8-bit)`, `JPG (8-bit)`. |
| `--indices NAME [NAME ...]` | không | Các chỉ số thảm thực vật (`NDVI`, `NDRE`, `GNDVI`, `EVI`, `SAVI`, `OSAVI`, `CIG`, …). |
| `--input-level {auto,raw,debayered,processed}` | `auto` | Buộc điểm vào của đường ống xử lý cho các tệp TIFF LATTICE (tệp .raw trong thư mục Survey3 không bị ảnh hưởng). Ngoài ra, cơ chế dự phòng cho phép xử lý hoàn toàn một bản chụp **không có tệp raw** — xem [Thư mục captures trông như thế nào](#what-a-captures-folder-looks-like). |
| `--debayered / --no-debayered` | bật | Xuất sản phẩm đã khử hiệu ứng Bayer tuyến tính (`Debayered_Images`). Xem [Các tùy chọn xuất theo từng sản phẩm](#per-product-export-toggles-lattice-multispectral). |
| `--preview / --no-preview` | bật | Xuất bản xem trước trên màn hình (`Preview_Images`): RGB = cân bằng trắng (nguồn sáng DAQ nếu có, nếu không thì chế độ gray-world) + gamma; multispec = kéo dãn màu giả. |
| `--radiance / --no-radiance` | bật | Xuất giá trị radiance dạng float32 (`Radiance_Images`, W/m²/sr/nm). |
| `--reflectance-source {daq,target,auto}` | `auto` | Tham chiếu cho sản phẩm phản xạ LATTICE: `auto` = mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) là tham chiếu tuyệt đối, dự phòng DAQ hướng xuống (ρ = π·L/E); `target` = nghiêm ngặt (không thay thế DAQ); `daq` = DAQ có thẩm quyền. Xem [Công tắc xuất theo từng sản phẩm](#per-product-export-toggles-lattice-multispectral). |
| `--target-reflectance-dir DIR` | none | Thư mục chứa các bản quét độ phản xạ mục tiêu **đo được** theo từng đơn vị (`<serial>.csv`); nếu không tìm thấy, sẽ chuyển sang sử dụng phổ T3/T4P danh nghĩa. |
| `--array-alignment / --no-array-alignment` | bật | Mảng LATTICE: áp dụng căn chỉnh giữa các mô-đun được ghi trong mỗi tệp`Chloros:Alignment*` XMP vào mọi sản phẩm đã xử lý (loại bỏ hiệu ứng debayering / xem trước / độ rạng rỡ / độ phản xạ / chỉ số). Không thực hiện thao tác nào đối với các hình ảnh không có thẻ. |
| `--array-alignment-crop / --no-array-alignment-crop` | cắt | Cắt các tệp xuất đã được căn chỉnh vào vùng chồng chéo chung của mảng để tất cả các mô-đun chia sẻ cùng một vùng phủ sóng; `--no-…` giữ nguyên khung hình cảm biến đầy đủ (điền màu đen bên ngoài nguồn). |
| `--array-alignment-interp {bilinear,nearest,cubic}` | `bilinear` | Lấy mẫu lại cho sự biến dạng căn chỉnh. `nearest` giữ nguyên các giá trị DN nguồn chính xác (không trộn lẫn các giá trị đo quang giữa các điểm ảnh). |

### Tùy chọn phát hiện mục tiêu

| Cờ | Mô tả |
| --- | --- |
| `--min-target-size PIXELS` | Kích thước tối thiểu của bảng mục tiêu (px) cho bộ phát hiện. |
| `--target-clustering 0-100` | Độ nhạy phân cụm. |
| `--target / --targets` | Xử lý thư mục đầu vào như bảng mục tiêu(bỏ qua phát hiện khảo sát). |

### Ví dụ

```bash
# Simplest: defaults are good for most surveys
chloros-cli process "/home/user/images/survey_001"

# Multi-index with explicit format
chloros-cli process "/home/user/images/survey_001" \
  --vignette \
  --reflectance \
  --format "TIFF (32-bit, Percent)" \
  --indices NDVI NDRE GNDVI OSAVI

# Texture-aware debayer for highest quality (Chloros+ only)
chloros-cli process "/home/user/images/survey_001" \
  --debayer texture-aware \
  --indices NDVI

# Process LATTICE captures explicitly (auto-detects from EXIF normally)
chloros-cli process "/home/user/captures/lattice_flight" \
  --input-level processed

# LATTICE multispectral → float32 radiance only (no DAQ downwelling needed)
chloros-cli process "/home/user/captures/lattice_flight" \
  --no-debayered --no-preview --no-reflectance

# LATTICE reflectance anchored to an in-frame target (strict, no DAQ fallback),
# with per-unit measured target scans looked up by serial
chloros-cli process "/home/user/captures/lattice_flight" \
  --reflectance-source target --target-reflectance-dir "/home/user/target_scans"

# LATTICE array capture: keep native geometry (ignore stamped alignment)
chloros-cli process "/home/user/captures/array_flight" \
  --no-array-alignment

# Aligned, uncropped, value-preserving resampling
chloros-cli process "/home/user/captures/array_flight" \
  --no-array-alignment-crop --array-alignment-interp nearest

# Save to a custom output location with a project name
chloros-cli process "C:/input" -o "C:/output" -n "Field_A_2026-05-26"
```

### Công tắc xuất theo từng sản phẩm (LATTICE đa phổ)

Quá trình xử lý LATTICE được phân tách thành **tất cả các sản phẩm áp dụng trong một lần chạy**. Bốn công tắc theo từng loại — `--debayered`, `--preview`, `--radiance`, `--reflectance` — đều**được bật theo mặc định**; sử dụng biểu mẫu `--no-<type>` để tắt một tùy chọn. Các camera chính RGB chỉ phát ra dữ liệu đã loại bỏ hiệu ứng Bayer và bản xem trước (không có độ sáng/độ phản xạ theo từng dải), do đó `--radiance`/`--reflectance` sẽ không có tác dụng đối với chúng. Các tùy chọn bật/tắt này bị bỏ qua đối với Survey3 `.raw` (theo đường dẫn phản xạ/mục tiêu tiêu chuẩn). *(Cờ `--radiometric-output {reflectance,radiance,sensor-response}` cũ đã bị **loại bỏ** và thay thế bằng các tùy chọn bật/tắt này; không còn mức `sensor-response` nữa.)*

| Sản phẩm | Đầu ra | Cần thu thập dữ liệu hướng xuống (DAQ downwelling)? |
| --- | --- | --- |
| `--debayered` | Demosaic tuyến tính (`Debayered_Images`). | Không. |
| `--preview` | Xem trước hiển thị (`Preview_Images`): RGB = cân bằng trắng (WB) + gamma; đa phổ (multispec) = kéo dãn màu giả. | Không. |
| `--radiance` | float32 W/m²/sr/nm từ chuỗi đo quang học đầy đủ (`Radiance_Images`). | Số |
| `--reflectance` | uint16 hệ số phản xạ ρ (`32768` = 1.0), sẵn sàng cho Pix4D. | **Có**, trừ khi có mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) làm điểm neo (xem bên dưới). |

`--reflectance-source` chọn tham chiếu độ phản xạ:**`auto`**(mặc định) lấy mục tiêu trong khung hình đã vượt qua kiểm tra chất lượng (QA) làm**điểm tham chiếu tuyệt đối**— các chuỗi đường thực nghiệm được neo bởi mục tiêu sẽ được so sánh chéo trên các bảng dữ liệu được giữ lại và giá trị đo được cao nhất sẽ được áp dụng — nếu không có mục tiêu hoặc kiểm tra chất lượng (QA) không đạt, hệ thống sẽ chuyển sang sử dụng giá trị chia theo hướng xuống của dữ liệu thu thập (DAQ) (ρ = π·L/E) khi không có mục tiêu hoặc kiểm tra chất lượng (QA) thất bại;**`target`**áp dụng chế độ nghiêm ngặt (không thay thế bằng dữ liệu DAQ);**`daq`**chuyển sang chế độ ưu tiên dữ liệu DAQ. Hình học mục tiêu (ArUco / ROI cố định / dải) lấy từ cấu hình mục tiêu của dự án; `--target-reflectance-dir DIR` lưu giữ các**kết quả quét** (`<serial>.csv`) được tra cứu theo số sê-ri/mã QR của thiết bị đích, với phổ T3/T4P danh định làm phương án dự phòng.

Đường dẫn phản xạ DAQ tự động xác định **tia sáng chiếu xuống khớp với dấu thời gian**từ tệp**`.daq`**(DAQ-U/M/E)**hoặc một tệp `.csv` gốc của DAQ-M**được tìm thấy cùng với hình ảnh. Nếu gói hiệu chuẩn cho từng camera hoặc DAQ không được lưu trong bộ nhớ đệm cục bộ, quy trình xử lý sẽ**tự động tải xuống từ AWS** khi sử dụng lần đầu (cần kết nối internet một lần; được lưu trong bộ nhớ đệm dưới dạng `~/.chloros/`).

#### Đọc các pixel phản xạ (Pix4D / Metashape / các tập lệnh của riêng bạn)

Phản xạ được lưu trữ dưới dạng số nguyên DN, và **giá trị DN tương ứng với ρ = 1.0 phụ thuộc vào camera nguồn**:

| Nguồn | ρ = 1.0 là | Cách xác định |
| --- | --- | --- |
| LATTICE (M3C / M3M) | `32768` (dải động đến ρ 2.0) | XMP `Chloros:PixelScale=32768` được đóng dấu trên tệp. |
| Survey3 | `65535` (bị cắt tại ρ 1.0) | Không có thẻ XMP `Chloros:*` — chính sự vắng mặt đó *chính là* tín hiệu. |

**Đọc giá trị `Chloros:PixelScale` và chia cho nó** thay vì giả định một hằng số. Thẻ được định nghĩa trong miền uint16, do đó nó vẫn giữ nguyên giá trị `32768` trên các định dạng đầu ra thực hiện quy đổi tỷ lệ — `TIFF (16-bit)`, `PNG (8-bit)`, `JPG (8-bit)` và `TIFF (32-bit, Percent)` đều tự mô tả (trước tiên hãy chuẩn hóa kiểu dữ liệu được lưu trữ trở lại uint16: nhân với 257 từ 8-bit, nhân với 65535 từ float).

> **Một trường hợp không mang theo tỷ lệ, theo thiết kế.** Khi một bản chụp nguồn 8-bit (BayerRG8) được ghi dưới dạng 8-bit TIFF, quy trình xử lý sẽ *cắt bớt* giá trị trong khoảng 0..255 thay vì điều chỉnh tỷ lệ, do đó mọi giá trị lớn hơn ρ≈0.008 sẽ được làm phẳng thành 255 và tệp không chứa thông tin tỷ lệ. Chloros cố ý loại bỏ cả `Chloros:PixelScale` và cặp `MicaSense:RadiometricCalibration` ở đó, đồng thời ghi lại lý do. **Nếu thẻ này vắng mặt trên tệp phản xạ LATTICE, đừng giả định có tỷ lệ — hãyxuất ở định dạng 16-bit hoặc 32-bit** thay vì chia các pixel vốn không thể chia được.

#### Thông tin EXIF được giữ nguyên khi xuất

`process` sao chép **khối GPS và ExifIFD** của ảnh gốc vào mọi sản phẩm, do đó
tệp xuất sẽ chứa `FocalLength`, `FNumber`, `ExposureTime`, `ISO`, `DateTimeOriginal` và
`CameraSerialNumber` cùng với thông tin tham chiếu địa lý.

**`FocalLength` là bắt buộc đối với photogrammetry.** Pix4D tính toán khoảng cách mẫu mặt đất (GSD) dựa trên
tiêu cự cộng với độ cao; nếu thiếu thẻ này, phần mềm sẽ tự động điều chỉnh theo tỷ lệ hoàn toàn sai lệch. Trong một
chuyến bay chụp 49 khung hình tại vườn cam, việc thiếu thẻ này đã biến khu vực 411 m × 160 m thành một khu vực tái tạo
47,8 km × 13 km — một ảnh chính quy 455 MP chủ yếu là dữ liệu trống, sau đó được chẩn đoán nhầm là vấn đề ghép ảnh và
vấn đề BigTIFF trước khi ai đó kiểm tra GSD. Nếu bản đồ chính quy của bạn xuất ra ở một tỷ lệ không hợp lý
, hãy chạy lệnh `exiftool -FocalLength` trên sản phẩm đã xuất trước tiên.

Bản sao này cố ý **không** phải là `-all:all`: các thẻ cấu trúc của IFD0&#x27;sẽ làm hỏng đầu ra LATTICE khi
sao chép, và `ExifImageWidth` / `ExifImageHeight` bị loại trừ vì chúng mô tả
bản chụp *nguồn* — một tệp xuất đã từng được thay đổi kích thước sẽ mang theo các kích thước
mâu thuẫn với raster của chính nó. XMP được ghi trực tiếp thay vì sao chép, vì ExifTool
sẽ loại bỏ các thẻ XMP cùng phiên gọi khi khối XMP được sao chép (điều này sẽ làm mất các thẻ hiệu chuẩn MAPIR
).

### Vị trí lưu trữ kết quả

Các tệp đầu ra được ghi **trong thư mục dự án, được nhóm theo máy ảnh và sau đó theo định dạng tệp**:

```
<project>/
└── LATT-M3M-L41-F550/                  # one folder per camera model+lens+filter
    ├── tiff16/
    │   ├── Reflectance_Calibrated_Images/
    │   ├── Debayered_Images/
    │   ├── Preview_Images/
    │   └── <INDEX>_Index_Images/        # e.g. NDVI_Index_Images
    └── tiff32/
        └── Radiance_Images/             # float32 radiance always lands here
```

Thư mục máy ảnh là `LATT-<sensor>-<lens>-F<filter>` cho LATTICE (phù hợp với EXIF của ảnh chụp
`Model`) và `<model>_<filter>` cho Survey3 — hai máy ảnh dùng chung cảm biến và bộ lọc nhưng khác nhau
về ống kính sẽ được lưu trong các cây thư mục riêng biệt, vì hiện tượng tối góc, trường nhìn và độ méo khác nhau. Thư mục định dạng
theo sau `--format`: `tiff16`, `tiff8`, `png8`, `jpg8`, hoặc `tiff32` cho
`TIFF (32-bit, Percent)`.

> **Mọi sản phẩm được xuất ra đều giữ nguyên tên tệp NGUỒN.** Một tệp xuất radiance của
> `capture_…_raw.tif` vẫn được gọi là `capture_…_raw.tif` — nó chỉ nằm trong
> `tiff32/Radiance_Images/`. **Thư mục xác định sản phẩm, không phải tên tệp**, vì vậy việc tìm kiếm theo mẫu
> cho `*radiance*.tif` sẽ không tìm thấy gì; thay vào đó, hãy so khớp với thư mục.

### Bản ghi cảm biến ánh sáng — đã hiệu chuẩn `.daq` + `.csv`

`process` cũng xử lý các bản ghi `.daq` trong thư mục đầu vào của bạn, và nó **không**
cần bất kỳ hình ảnh nào để thực hiện việc này: một thiết bị DAQ-U / DAQ-M / DAQ-E được vận hành độc lập đã là một
quá trình ghi dữ liệu hoàn chỉnh, và một thư mục chỉ chứa các tệp `.daq` là đầu vào hợp lệ.

Một thiết bị DAQ có thể được ghi lại **mà không cần** hiệu chuẩn — đó chính là cách các thiết bị ghi dữ liệu công khai
[`chloros_scripts`](https://github.com/mapircamera/chloros_scripts)
(`record_daq.py`) hoạt động theo mặc định: chúng ghi lại số đếm thô từ cảm biến và gắn dấu thời gian vào tệp để
Chloros lấy dữ liệu hiệu chuẩn nhà máy của cảm biến đó **theo số sê-ri** (trước tiên từ bộ nhớ đệm cục bộ,
sau đó từ đám mây MAPIR) và áp dụng nó. `process` ghi kết quả trở lại:

```
<project>/
└── Light Sensor/
    ├── <name>_calibrated.daq        # reprocessable archive, declares its bundle
    └── <name>_calibrated.csv        # W/m^2/nm per reading + photometric columns
```

`.csv` chứa một hàng cho mỗi lần đọc: dấu thời gian UTC, thời gian tích hợp, công suất tổng,
độ lux quang giác/tối giác, PPFD (và phân tách màu xanh dương/xanh lá/đỏ), bước sóng đỉnh, sau đó là
phổ đầy đủ trên lưới bước sóng riêng của cảm biến. `.daq` được nhập lại mà không cần
hiệu chuẩn lần thứ hai.

Khi thành công, quá trình chạy sẽ báo cáo `Light-sensor products written: N (calibrated .daq + .csv)`.
Phần trong ngoặc đơn mô tả những gì thực sự đã được ghi ra, do đó nội dung như sau
`(RAW COUNTS — this sensor has no calibration bundle)` đối với cảm biến không có gói và
`(N calibrated, M raw counts)` đối với thư mục chứa cả hai. Các tiêu đềriêng
các tiêu đề `[DAQ-EXPORT]` và `[RUN-SUMMARY]` cũng được đặt tên theo cách tương tự — không có
cái nào trong ba cái này có thể gọi một bản xuất thô là đã được hiệu chuẩn.

Một bản ghi DAQ-U / DAQ-M / DAQ-E mà gói hiệu chuẩn không thể lấy được — bạn đang
ngoài mạng, hoặc cảm biến đó không có tệp hiệu chuẩn — sẽ bị **bỏ qua kèm lý do** trên dòng
`[DAQ-EXPORT]`, và tuyệt đối không được ghi ra dưới dạng tệp tệp “đã hiệu chuẩn” chứa số đếm thô.
Kết nối internet và chạy lại. Lý do là lý do mà trình đọc thực sự
xác định cho tệp đó (cấu trúc không đọc được, không có gói, lỗi ghi), và bản tóm tắt
chạy liệt kê các lý do **riêng biệt** — hai mươi tệp bị bỏ qua vì một nguyên nhân được ghi nhận là một
nguyên nhân duy nhất, chứ không phải hai mươi lần lặp lại của nó.

#### Các bản ghi DAQ-A được xuất dưới dạng số đếm thô

Dòng sản phẩm **DAQ-A** ra đời trước hệ thống gói theo số sê-ri và không có gói hiệu chuẩn
để lấy — thay vào đó, nó được hiệu chuẩn tại hiện trường so với một mục tiêu phản xạ, đó là
lý do tại sao nó không bao giờ cần đến gói hiệu chuẩn. Việc từ chối các bản ghi này khiến họ không có cách nào để lấy
dữ liệu ra, vì vậy họ xuất dưới một **tên tệp khác**:

```
<project>/
└── Light Sensor/
    ├── <name>_raw.daq        # NOT _calibrated
    └── <name>_raw.csv        # raw spectral sensor counts, NOT irradiance
```

Một tên tệp khác thay vì một cờ bên trong tệp, vì yêu cầu này phải duy trì
khi được gửi qua email dưới dạng tên tệp thuần túy. Tiêu đề `.csv` ghi rõ
`raw spectral sensor counts (NOT irradiance)` và cảnh báo rằng các giá trị chỉ có thể so sánh
**trong phạm vi** tệp — đây chính xác là mục đích mà hiệu chuẩn dựa trên mục tiêu sử dụng chúng — chứ
không phải giữa các cảm biến. Các cột quang độ phụ thuộc vào công suất (công suất tổng, lux quang giác và
lux tối giác, PPFD) được ghi là **NULL** thay vì được tích hợp từ số đếm, và phần tóm tắt
tóm tắt cho biết `RAW COUNTS` nên dữ liệu “xuất” trong nhật ký không thể được đọc như bức xạ.

Các bản ghi **v1.01 / v1.02** cũ (được ghi bởi DAQ-A-SD) không chứa thời điểm cho từng lần đọc,
chỉ có thời gian ghi của tệp. Công cụ khớp hình ảnh↔ánh sáng chiếu xuống vẫn từ chối chúng — việc khớp một
khung hình với thời gian ghi sẽ dẫn đến sai sót không thể nhận biết — nhưng trình xuất dữ liệu đọc được chúng, và
CSV in ra `clock=daq_created_on` để sản phẩm chỉ ra nó đang chạy trên đồng hồ nào.

### Ghi chú

- `process` tự động phát hiện xem thư mục của bạn thuộc loại Survey3, LATTICE hay hỗn hợp.
- Tiến trình được truyền qua Server-Sent Events; CLI hiển thị tiến trình theo từng luồng thời gian thực (Phát hiện, Phân tích, Xử lý, Xuất).
- Đối với Linux /Jetson, CLI sẽ kiểm tra bộ nhớ ảo (swap) và có thể hiển thị cảnh báo trước khi xử lý các thư mục lớn. Chức năng loại bỏ hiệu ứng debayer nhận diện kết cấu cũng tự động áp dụng giới hạn tần số GPU trên các thiết bị Jetson tiết kiệm năng lượng (Nano, Orin Nano).
- Khi thành công, quá trình chạy sẽ báo cáo số lượng sản phẩm hình ảnh đã được ghi (`Image products written: N`).

#### Một quá trình chạy không ghi được hình ảnh nào sẽ bị lỗi

Nếu bạn yêu cầu các sản phẩm nhưng quá trình chạy không ghi **bất kỳ sản phẩm nào** — chỉ ghi `project.json` và
`calibration_data.json` — thì `process` sẽ coi đó là lỗi: nó sẽ hiển thị
`Processing finished but wrote no image products.` và **thoát với giá trị không bằng 0**, do đó một skript có thể
phát hiện được lỗi này. Thông báo này nêu tên thư mục dự án và các nguyên nhân thường gặp:

- thư mục đầu vào không được nhận diện là dữ liệu chụp (kiểm tra bố cục và `--input-level`), hoặc
- mọi sản phẩm được yêu cầu đều bị bỏ qua vì không áp dụng được cho các camera đó (ví dụ: yêu cầu
  độ bức xạ/độ phản xạ từ các camera chỉ hỗ trợ chế độ chụp ảnh toàn cảnh (RGB)).

Chạy lại với `--verbose` và kiểm tra nhật ký backend để tìm các dòng `[LATTICE-EXPORT]` / `[EXPORT-CHECK]`,
giải thích từngviệc bỏ qua từng máy ảnh mà nếu không sẽ không xuất hiện trong kết quả đầu ra của trình xử lý hình ảnh (CLI).

Một lần chạy chỉ metadata được thực hiện có chủ đích — tắt tất cả các tùy chọn sản phẩm và không có `--indices` — vẫn là một
**thành công**, vì đầu ra hình ảnh trống là kết quả chính xác trong trường hợp này.

Tương tự với **quá trình chạy chỉ sử dụng cảm biến ánh sáng**: một thư mục chứa các bản ghi `.daq` không có hình ảnh để xuất
theo định nghĩa, và quá trình chạy được đánh giá dựa trên tệp `.daq`X / `.csv` đã được ghi thay thế.

---

## `chloros-cli login`

Xác thực máy này bằng tài khoản đám mây Chloros+. Thông tin đăng nhập được lưu trữ an toàn trong `~/.chloros/user_session.json`.

```
chloros-cli login EMAIL PASSWORD
```

### Ví dụ

```bash
chloros-cli login user@example.com 'YourPassword'

# Passwords containing $ should use SINGLE quotes
chloros-cli login user@example.com 'my$ecret$pass'
```

> **PowerShell `$$` mangling is auto-corrected.** In double quotes PowerShell expands `$$` (loại bỏ hoặc sao chép một phần mật khẩu). Khi gặp lỗi 401, CLI sẽ tự động thử lại bằng cách thêm lại `$$`, sau đó với-bản sao của một nửa mật khẩu; nếu lần thử lại thành công, hệ thống sẽ đăng nhập cho bạn và hiển thị cú pháp dấu nháy đơn chính xác để sử dụng trong lần tiếp theo.

> **Sử dụng ở chế độ không giao diện/tự động hóa: không có phiên làm việc được lưu trong bộ nhớ đệm có nghĩa là sẽ xuất hiện lời nhắc tương tác, chứ không phải lỗi nhanh.** Bất kỳ lệnh nào khởi tạo backend (`process`, `status`, `export-status`, `time-sync`, …) chạy mà không có giấy phép/phiên làm việc được lưu trong bộ nhớ đệm sẽ chuyển sang dấu nhắc tương tác `Email:` / `Password:` trên stdin trước khi tiếp tục. Do đó, một tác vụ không cần giám sát mà không có phiên làm việc được lưu trong bộ nhớ đệm sẽ bị treo khi chờ nhập liệu — hãy chạy `chloros-cli login EMAIL PASSWORD` một lần trên mỗi máy trước khi lên lịch công việc không cần giao diện người dùng.

---

## `chloros-cli logout`

Xóa phiên làm việc được lưu trong bộ nhớ đệm và buộc đăng nhập lại trong lần gọi tiếp theo.

```bash
chloros-cli logout
```

---

## `chloros-cli status`

Hiển thị cấp độ giấy phép hiện tại (Iron/Copper/Bronze/Silver/Gold), người dùng đã xác thực và số lần liên kết thiết bị.

```bash
chloros-cli status
```

---

## `chloros-cli export-status`

Kiểm tra tiến trình xuất Thread-4 đang diễn ra. Có thể gọi an toàn **trong khi** đang chạy lệnh `process` từ một cửa sổ lệnh khác.

```bash
chloros-cli export-status
```

---

## `chloros-cli language`

Đặt ngôn ngữ hiển thị của Trình quản lý xuất (CLI) (hỗ trợ 38 ngôn ngữ, bao gồm CJK, RTL, và Indic). Tự động chuyển sang tiếng Anh một cách mượt mà trên các giao diện điều khiển cũ không thể hiển thị kịch bản.

```
chloros-cli language [LANG_CODE] [--list]
```

### Ví dụ

```bash
# List all available languages
chloros-cli language --list

# Switch to Spanish
chloros-cli language es

# Show the currently-active language
chloros-cli language
```

---

## Lệnh liên quan đến thư mục dự án

Các lệnh này quản lý vị trí thư mục dự án mặc định (được chia sẻ với giao diện người dùng).

```bash
chloros-cli set-project-folder "/home/user/Chloros Projects"
chloros-cli get-project-folder
chloros-cli reset-project-folder
```

---

## `chloros-cli update`

Linux/ Chỉ dành cho Jetson. Kiểm tra `version_url` từ `/etc/chloros/update.conf` và đề xuất tải xuống + cài đặt phiên bản tương ứng `.deb`.

```bash
chloros-cli update            # check + install
chloros-cli update --check    # check only
```

Trên Linux /Jetson, CLI cũng thực hiện **kiểm tra cập nhật tự động mỗi khi khởi động** (không chặn, không bao giờ làm chậm lệnh): nó đọc `/etc/chloros/update.conf`, lưu kết quả vào bộ nhớ đệm trong 1 giờ tại `~/.chloros/update_cache.json`, và hiển thị `Update available: vX.Y.Z / Run: chloros-cli update` khi có phiên bản mới hơn. Tự động bỏ qua mà không hiển thị thông báo trong trường hợp có lỗi hoặc khi gặp Windows.

---

## `chloros-cli selftest`

Thực hiện kiểm tra nhanh gồm 7 bước: phiên bản, tính khả dụng của cổng, khởi động backend, `/api/test`, `/api/system-info` (GPU/CUDA/PyTorch), sự hiện diện của mô hình khử nhiễusự hiện diện của mô hình khử nhiễu, sự sẵn sàng của CUDA+mô hình khử nhiễu.

```bash
chloros-cli selftest
```

---

## `chloros-cli time-sync`

Trạng thái và điều khiển grandmaster PTP. Máy chủ Chloros chạy grandmaster PTP; các thiết bị LATTICE cams và DAQ-E hoạt động như thiết bị phụ thuộc vào nó để đồng bộ hóa dấu thời gian giữa các thiết bị.

| Lệnh con | Mô tả |
| --- | --- |
| `status` | Hiển thị trạng thái máy chủ chính, mức ưu tiên BMCA, mã định danh đồng hồ. |
| `peers` | Liệt kê các thiết bị phụ được phát hiện qua Delay_Req (máy ảnh + cảm biến DAQ-E). |
| `cameras` | Tình trạng PTP theo từng camera (`PtpStatus`, `PtpOffsetFromMaster`, `PtpMeanPathDelay`). |
| `restart` | Khởi động lại quá trình grandmaster. |
| `set-priority --priority1 N --priority2 N` | Ghi đè mức ưu tiên BMCA. |

### Ví dụ

```bash
chloros-cli time-sync status
chloros-cli time-sync peers
chloros-cli time-sync cameras
chloros-cli time-sync restart
chloros-cli time-sync set-priority --priority1 1 --priority2 1
```

---

## `chloros-cli lattice`

Điều khiển camera LATTICE. Mỗi lệnh con đều được chuyển qua backend Chloros; backend này quản lý nhóm camera (cam pool) nên các lệnh gọi CLI tiếp theo sẽ tái sử dụng cùng một handle đã mở.

### Các tùy chọn chung (được chia sẻ bởi hầu hết các lệnh con)

| Cờ | Mô tả |
| --- | --- |
| `-d, --device N` | Chỉ số camera (mặc định: 0). |
| `-s, --serial SN` | Số sê-ri cụ thể; ghi đè lên `--device`. |
| `--serials SN1,SN2,…` | Các số sê-ri được phân tách bằng dấu phẩy cho hoạt động đa camera. |
| `--all` | Hoạt động trên mọi camera được phát hiện. |
| `--exposure US` | Thời gian phơi sáng tính bằng microgiây. |
| `--gain DB` | Độ khuếch đại (gain) tính bằng dB. |
| `--pixel-format FMT` | Ví dụ: `BayerRG8`, `BayerRG12`. |
| `--width N` / `--height N` | Kích thước hình ảnh. |
| `--preset {default,high_quality,high_speed,triggered}` | Áp dụng cài đặt sẵn. Tất cả đều hoạt động tự do trừ `triggered`, chức năng này kích hoạt camera khi phát hiện tín hiệu cạnh phần cứng trên Dòng 2 — nếu không có tín hiệu nào tác động lên dòng đó, camera sẽ chờ mãi mãi thay vì ghi hình. |
| `-o, --output DIR` | Thư mục đầu ra (mặc định: `output`). |
| `--packet-size {auto,jumbo,standard,N}` | Kích thước gói tin GVSP. `auto` chạy các gói thăm dò ICMP+GVSP; `jumbo` = 9000; `standard` = 1500. |

### Camera LATTICE Quy trình làm việc First-Connect

```bash
# 1. Discover cameras on the network
chloros-cli lattice info

# 2. Single-cam smoke test: capture one frame.
#    By default this saves EVERY export type applicable to the cam
#    (raw, debayered, radiance, reflectance, preview). Pass e.g.
#    `--processing debayered` to save just one.
chloros-cli lattice capture -o output/

# 3. Connect a synchronized array (RECOMMENDED ENTRY POINT for arrays).
#    This is the same "smart-prep" flow the Chloros GUI uses:
#      - Network capability probe (ICMP DF ping + GVSP probe)
#      - Tier auto-pick (sim-emit / ftd-stagger / slip)
#      - Auto-shrink frame size to fit the wire
#      - PTP enabled by default
#      - Per-cam pixel format auto-pick
#      - AE seeding from the cam's saved state
#      - GPIO trigger config on Line2
chloros-cli lattice array-connect \
  --serials 213800234,214000533,214701288,214701292

# 4. Capture one synced frame group from the live array.
#    Defaults to --processing all (one file per export type per cam);
#    pass a single level to narrow it, e.g. --processing reflectance.
chloros-cli lattice array-capture --processing reflectance -o output/

# 5. Live-preview one cam in your browser
chloros-cli lattice viewer --serial 213800234

# 6. Tear down when done
chloros-cli lattice array-disconnect
```

### Tham chiếu lệnh con

#### Khám phá &amp; Thông tin

| Lệnh con | Mục đích |
| --- | --- |
| `lattice info` | Liệt kê các camera đã kết nối (nhà sản xuất, kiểu máy, số sê-ri, IP, MAC). |
| `lattice probe [--pixel-format FMT] [--json] [--no-discover]` | Phân tích hệ thống máy chủ để có cấu hình camera tối ưu. `--no-discover` bỏ qua bước phát hiện camera (nhanh hơn, chỉ phân tích NIC). |
| `lattice network [--fix] [--estimate] [--cameras N]` | Kiểm tra/sửa cài đặt NIC; ước tính băng thông/FPS. |
| `lattice network-analysis --master SN --slaves SN1,SN2,… [--width N] [--height N] [--pixel-format FMT] [--binning N] [--force-tier TIER] [--backend-url URL] [--json]` | Khả năng mạng của backend theo lược đồ ổn định + đề xuất mảng (trả về `status` ∈ `ok` / `auto_shrunk` / `auto_capped_fps` / `needs_force_slip` / `error`). `auto_capped_fps` giữ nguyên độ phân giải được yêu cầu nhưng giới hạn tốc độ khung hình mục tiêu — hãy đọc `recommended.recommended_target_fps` và truyền nó làm mục tiêu kết nối; coi đó là thành công, không phải là lỗi. |
| `lattice analyze-array [--models M1,M2,…] [--binning N] [--n-active N] [--width N] [--height N] [--pixel-format FMT] [--force-tier TIER] [--json]` | Phân tích giả định mà không cần mở camera. **`--n-active` là tổng số camera trên mạng, không chỉ riêng mảng này**— tăng giá trị này khi các camera độc lập truyền dữ liệu đồng thời, hoặc ngân sách đường truyền được tính toán dựa trên nhu cầu không tính đủ số lượng camera (mặc định: `len(--models)`). Luôn in ra giá trị tổng hợp `Wire budget:` (MB/s yêu cầu so với giới hạn an toàn va chạm) và các dòng `Max cameras:`, đồng thời đánh dấu cảnh báo `** OVER-SUBSCRIBED**` khi mảng sử dụng quá mức băng thông — xem [Mô hình fps &amp; burst của mảng](#array-fps--burst-model). |
| `lattice gpu` | Hiển thị trạng thái GPU. |
| `lattice firmware [--update] [--force] [-y\|--yes]` | Kiểm tra hoặc cập nhật phần mềm camera. Lựa chọn cục bộ `.fwa` được cố định: tệp trong `firmware/<MODEL_PREFIX>/` phù hợp với phiên bản `MIN_FIRMWARE_VERSION` của bản dựng sẽ được nạp khi có sẵn (chỉ phiên bản cao nhất được dùng làm phương án dự phòng), do đó hình ảnh nhà cung cấp mới hơn được lưu trữ trên đĩa sẽ không hoạt động cho đến khi ghim đó được thay đổi — các bản phát hành mới hơn được đưa vào thiết bị thông qua bản kê khai AWS đã ký, điều này được ưu tiên khi có phiên bản mới hơn. |
| `lattice presets [--apply NAME]` | Hiển thị danh sách hoặc áp dụng cài đặt sẵn của camera. |
| `lattice status` | Trạng thái camera trực tiếp. |

#### Ghi hình

| Lệnh con | Mục đích |
| --- | --- |
| `lattice capture [--format tiff\|png\|jpg] [--jpeg-quality N] [--processing LEVEL] [--levels L1,L2,…] [--force-daq]` | Khung hình đơn. **Lưu mọi định dạng xuất theo mặc định** (`--processing all`); xem [Các mức xuất dữ liệu ghi hình](#capture-export-levels-the-all-default). `--levels` lưu một tập con cụ thể (ghi đè lên `--processing`); `--force-daq` ghi giá trị đọc DAQ được chỉ định dưới dạng tệp phụ `.daq` ngay cả khi ghi dữ liệu thô thuần túy. `--jpeg-quality` = JPEG chất lượng 1–100 (mặc định 95). |
| `lattice continuous [--format tiff\|png\|jpg] [--jpeg-quality N] [--queue-depth N]` | Truyền dữ liệu vào đĩa cho đến khi nhấn Ctrl+C. |
| `lattice viewer [--brightness N] [--ae-damping F] [--frame-rate FPS]` | Xem trước MJPEG trực tiếp trên trình duyệt. `--ae-damping` thiết lập độ giảm phơi sáng tự động (0,4–100). |

#### Điều chỉnh cảm biến

| Lệnh con | Mục đích |
| --- | --- |
| `lattice configure [--get N1 N2…] [--set N=V N=V…] [--dump] [--json]` | Đọc/ghi bất kỳ nút GenICam nào. |
| `lattice exposure [--auto] [--auto-once] [--off] [--set US] [--brightness N] [--damping F] [--upper-limit US]` | Thời gian phơi sáng &amp; AE. |
| `lattice gain [--auto] [--off] [--set DB]` | Độ khuếch đại và tự động điều chỉnh độ khuếch đại. |
| `lattice resolution [--set WxH] [--offset X,Y] [--binning N] [--binning-mode Sum\|Average]` | Vùng quan tâm (ROI) của cảm biến và gộp pixel. |
| `lattice format [--set FMT] [--list]` | Định dạng pixel. |
| `lattice trigger [--mode On\|Off] [--source SRC] [--delay-us US] [--activation EDGE] [--list-sources] [--software]` | Kích hoạt phần cứng/phần mềm. |
| `lattice white-balance [--auto] [--off] [--red R] [--blue B]` (không có cờ = cân bằng trắng một lần) | Các thao tác cân bằng trắng.RGB /Chỉ dành cho máy ảnh Bayer; không thực hiện (bỏ qua) trên M3M đơn sắc. |
| `lattice color-profile [--set raw\|linear\|natural\|enhanced\|custom_temp] [--cct K] [--get]` | Quy trình xử lý màu hiển thịRGB. `natural` (mặc định) là chế độ hoàn thiện trực tiếp tiết kiệm; `enhanced` bổ sung loại bỏ viền màu + độ sống động + độ tương phản cục bộ CLAHE để đạt được hiệu ứng tương đương hub-parity với chi phí hoàn thiện mỗi khung hình gấp khoảng 2 lần, do đó tốc độ khung hình **trực tiếp** sẽ thấp hơn— các bản ghi được lưu lại luôn nhận được hiệu ứng hoàn thiện đầy đủ trong cả hai trường hợp. Chỉ dành cho máy ảnh RGB/Bayer; bị bỏ qua trên máy ảnh đơn sắc M3M. |
| `lattice color [--saturation N] [--contrast N] [--reset] [--get]` | Hiển thị độ bão hòa/độ tương phản (máy ảnh có bộ lọc RGB). Bỏ qua trên M3M đơn sắc. |
| `lattice filter [--set NAME] [--list]` | Đặt mô hình bộ lọc của máy ảnh (`RGN-IMX265`, `OCN`, `NGB`, …). |
| `lattice power [--sleep]` | Kiểm tra các nút nguồn/nhiệt; bật/tắt chế độ chờ tiết kiệm năng lượng. |

#### Hiệu chuẩn &amp; Cảm biến

| Lệnh con | Mục đích |
| --- | --- |
| `lattice calibrate [--filter NAME] [--attempts N] [--save PATH]` | Hiệu chuẩn từ mục tiêu phản xạ. |
| `lattice dls [--connect] [--spectrum] [--irradiance] [--mac MAC] [--filter NAME] [--json]` | Lệnh cảm biến ánh sáng chiếu xuống tích hợp sẵn. |
| `lattice vignette --input DIR --output DIR [--lens-model KEY]` | Áp dụng hiệu chỉnh hiệu ứng vignette cho các hình ảnh hiện có. |

#### Nhiều camera (Phiên làm việc tạm thời)

| Lệnh con | Mục đích |
| --- | --- |
| `lattice multi-info` | Liệt kê tất cả các camera có vai trò đồng bộ. |
| `lattice multi-capture [--format FMT] [--jpeg-quality N] [--processing LEVEL]` | Một khung hình đồng bộ từ mỗi camera. Lưu **tất cả các định dạng xuất theo mặc định**khi kết nối với mảng dữ liệu liên tục; chế độ dự phòng tạm thời không dùng mảng chỉ**chỉ được khử hiệu ứng debayering** (chạy lệnh `array-connect` trước cho phần còn lại). |
| `lattice multi-stream [--fps F] [--count N] [--format FMT] [--jpeg-quality N]` | Truyền phát các khung hình đồng bộ (tạm thời). |
| `lattice multi-test [--count N]` | Kiểm tra thời gian đồng bộ GPIO. |
| `lattice multi-detect [--line LINE] [--json]` | Tự động phát hiện cách kết nối GPIO chủ/nô. |

#### Căn chỉnh

| Lệnh con | Mục đích |
| --- | --- |
| `lattice align-calibrate [--method orb\|akaze\|phase\|checkerboard\|manual] [--model translation\|rigid\|affine\|homography] [--frames N] [--checkerboard RxC] [--points PATH] [--reference SN] [--save PATH] [--preview] [--vignette] [--prefilter none\|gradient\|clahe\|blur\|hist_match] [--rms-threshold-px N]` — cùng với các nút điều chỉnh bộ phát hiện/khớp `[--max-features N] [--ratio-threshold F] [--matcher bf\|flann] [--knn-k N]`, các nút điều chỉnh RANSAC `[--ransac-threshold-px F] [--ransac-iters N] [--ransac-confidence F]`, kết hợpkhung hình `[--averaging mean\|median\|inlier_weighted]`, ràng buộc hình học `[--lock-rotation] [--lock-scale] [--lock-axis x\|y]`, hạn chế không gian `[--roi X0,Y0,X1,Y1] [--mask PATH]` và các tùy chỉnh riêng cho từng thiết bị phụ `[--per-cam-override SN:KEY=VALUE]` (có thểlặp lại) | Tính toán hồ sơ căn chỉnh từ camera trực tiếp. `--prefilter` mặc định là `gradient` (bản đồ cạnh; tương thích với công cụ căn chỉnh GUI/mảng — các cạnh được duy trì qua các dải phổ). `--matcher flann` mang lại hiệu quả khi có trên ~5000 đặc trưng; `--averaging median` có khả năng chống chịu tốt trước một lần chụp hình không đạt yêu cầu, `inlier_weighted` cân bằng theo số lần khớp; `--lock-scale` chiếu lên góc xoay gần nhất (không thay đổi tỷ lệ), `--lock-axis` đặt thành 0 một thành phần dịch chuyển; `--mask` áp dụng cho mọi camera (sử dụng `--per-cam-override` cho cài đặt riêng từng camera, ví dụ: `--per-cam-override 214701292:method=phase`). `--rms-threshold-px` từ chối lưu bản hiệu chuẩn có giá trị RMS tái chiếu vượt quá ngưỡng. |
| `lattice align-apply --profile PATH [--format tiff\|png] [--bit-depth 8\|12\|16] [--bands NAMES] [--order NAMES] [--gpu\|--no-gpu] [--no-crop] [--per-camera] [--per-band] [--vignette] [--interpolation nearest\|linear\|cubic\|lanczos] [--border-mode constant\|replicate\|reflect\|wrap] [--border-value N]` | Chụp một khung hình đa dải đã được căn chỉnh. `--bit-depth` mặc định sẽ khớp với máy ảnh; `--no-crop` giữ nguyên khung hình đầy đủ (lấp đầy bằng màu đen); `--interpolation` (mặc định `linear`) và `--border-mode`/`--border-value` (mặc định là `constant`/0) điều khiển biến dạng CPU — đường dẫn GPU luôn là tuyến tính hai chiều. |
| `lattice align-stream --profile PATH [--fps F] [--count N] [--bit-depth 8\|12\|16] [--bands NAMES] [--order NAMES] [--gpu\|--no-gpu] [--no-crop] [--per-band] [--vignette] [--interpolation nearest\|linear\|cubic\|lanczos] [--border-mode MODE] [--border-value N]` | Truyền các khung hình đa(các thông số điều chỉnh warp giống như `align-apply`). |
| `lattice align-info --profile PATH [--json]` | Hiển thị chi tiết cấu hình. |
| `lattice align-reorder --profile PATH [--order NAMES] [--enable SERIALS] [--disable SERIALS]` | Thay đổi thứ tự lớp. |

#### Chỉ mục / Toán học thảm thực vật

```bash
# Offline: compute NDVI from an aligned multi-band TIFF
chloros-cli lattice index --input aligned.tif --preset NDVI \
  --output ndvi.tif --colorize --gradient RdYlGn

# Live: discover array, calibrate alignment, capture, compute index, in one go
chloros-cli lattice index --live --profile align.json --preset NDVI \
  --save-multiband -o output/
```

Bộ cờ đầy đủ: `--input PATH | --live --profile PATH`, `--preset NAME` (NDVI / NDRE / EVI / SAVI / GNDVI /…), `--formula EXPR`, `--channel SYM=BAND` (có thể lặp lại), `--capture-level raw|debayered|radiance|reflectance|unknown` (ghi đè mức ghi lại được lưu trong tệp nguồn TIFF; mặc định: đọc từ siêu dữ liệu của TIFF), `--output PATH`, `--output-format all|raw|tif|colorized|lut|png`, `--gradient NAME|JSON`, `--vmin/--vmax/--percentile LO,HI`, `--bg-mode clip|transparent|indexColor|backgroundColor`, `--colorize`, `--list-presets`, `--list-gradients`. Với `--live`, các núm điều chỉnh độ lệch căn chỉnh cũng được áp dụng: `--save-multiband`, `--gpu/--no-gpu`, `--no-crop`, `--bit-depth 8|12|16`, `--vignette`, `--interpolation nearest|linear|cubic|lanczos`, `--border-mode constant|replicate|reflect|wrap`, `--border-value N`.

> **Các ký hiệu `--channel` phân biệt chữ hoa và chữ thường.** Phần ký hiệu phải trùng khớp chính xác với tên kênh của cài đặt sẵn (các cài đặt sẵn sử dụng chữ thường, ví dụ: NDVI = `red`, `nir` — kiểm tra `--list-presets`), và phần dải tần phải trùng khớp với tên dải tần trong ngăn xếp được căn chỉnh (hoặc là chỉ số dải tần bắt đầu từ 0 ở chế độ ngoại tuyến). `--channel red=Red_660 --channel nir=NIR_850` hoạt động; `--channel RED=660` sẽ thất bại với lỗi `channel_map missing entries` .

#### Kết nối liên tục (Smart-Prep, quy trình tương đương GUI)

Các lệnh này giữ cho các camera luôn mở trong nhóm máy chủ phía sau qua các lần gọi hàm `CLI`.

| Lệnh con | Mục đích |
| --- | --- |
| `lattice cam-connect [--serial SN]` | Thêm một camera vào nhóm (một camera duy nhất, không phải mảng). |
| `lattice cam-disconnect [--serial SN] [--all]` | Giải phóng. |
| `lattice cam-list` | Liệt kê các camera trong nhóm. |
| **`lattice array-connect`**|**Kết nối một mảng đồng bộ hóa liên tục (điểm vào được khuyến nghị).** Chạy toàn bộ quy trình chuẩn bị thông minh (smart-prep) trên giao diện người dùng (GUI). |
| `lattice array-disconnect [--array-id ID] [--all]` | Giải phóng một mảng. |
| `lattice array-list` | Liệt kê các mảng đã kết nối. |
| `lattice array-status [--array-id ID]` | Tốc độ khung hình trực tiếp (fps), PTP, lỗi gần nhất. |
| `lattice array-capture [--processing LEVEL\|all] [--levels L1,L2,…] [--aligned\|--no-aligned] [--index\|--no-index] [--force-daq] [--smart] [--fastest] [--compression deflate\|none] [--continuous\|--interval S] [--count N] [--duration S]` | Một lần ghi đồng bộ từ mảng trực tiếp — Đơn lẻ / Liên tục / Theo khoảng thời gian / Nhanh nhất. **Mặc định là `all`** (một tệp cho mỗi loại xuất áp dụng trên mỗi camera). Các camera bị bỏ qua (ví dụ: RGB bị loại trừ khỏi độ sáng/độ phản xạ) được báo cáo với `Skipped: SN:<serial> (<reason>)`; giá trị đọc DAQ được sử dụng cho độ phản xạ được lưu cùng và báo cáo với `DAQ: <path>`. Xem [Chế độ ghi hình, Thiết bị ghi &amp; Xử lý lại ngoại tuyến](#capture-modes-recorders--offline-reprocess). |
| `lattice array-record [--fps F] [--duration S] [--gif] [--gif-only]` | Ghi lại chế độ xem chỉ số kết hợp trực tiếp thành video/GIF (dành cho giám sát; cần mở luồng kết hợp). |
| `lattice array-burst [--duration S] [--max-frames N] [--build] [--products …]` | Chụp liên tiếp Bayer thô với tốc độ khung hình cao (chất lượng phân tích; xử lý lại ngoại tuyến). |
| `lattice array-build-video --burst-dir DIR [--products …] [--fps F] [--save-tiffs] [--gif]` | Xử lý lại loạt ảnh thô đã lưu thành video đã hiệu chuẩn. |

##### Tùy chọn `array-connect`

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--serials SN1,SN2,…` | Tự động phát hiện tất cả camera LATTICE (cần ≥2) | Thiết bị có số sê-ri đầu tiên là MASTER. Khi bỏ qua, quá trình phát hiện sẽ lọc các mẫu LATTICE (`TRI032*`) và kết nối tất cả chúng. |
| `--line {Line0,Line2,Line3}` | `Line2` | Dòng đồng bộ GPIO. |
| `--target-fps F` | tự động | Tần suất kích hoạt của MASTER. |
| `--force-tier {sim-capture-sim-emit, sim-capture-ftd-stagger, slip-emit-and-capture}` | tự động | Ghi đè bộ chọn cấp độ. |
| `--wire-ceiling-mbps MB_PER_S` | tự động phát hiện | **Dung lượng băng thông liên tục của máy chủ, tính bằng MB/s — con số này quyết định việc phân bổ toàn bộ mảng.** Hãy giảm giá trị này khi mảng báo cáo các khung dữ liệu bị hỏng GVSP: giá trị tự động được tính toán dựa trên tốc độ kết nối được NIC công bố, vốn thường đánh giá quá cao đối với các bộ điều hợp USB, các làn PCIe mỏng và các mạng chia sẻ đang bận. Giá trị này được lưu trữ trong khối ghi lại mảng của dự ánkhối ghi lại mảng của dự án, do đó việc mở lại / CLI / SDK kết nối lại sẽ khôi phục giá trị này. Xem [Tình trạng mảng](#array-health--which-subsystem-is-losing-frames). |
| `--binning {1,2,4}` | tự động | Phân nhóm phần cứng. |
| `--no-recommend` | tắt | Bỏ qua bước phân tích mạng. |
| `--no-ptp` | tắt | Vô hiệu hóa PTP (lúc đó, các dấu thời gian giữa các camera **sẽ không** thể so sánh được). |

### Smart-AE / Smart-Capture

Các mảng LATTICE sẽ chạy AE liên tục ở chế độ nền ngay khi được kết nối, nhưng một cảnh vừa được định hướng sẽ mất một chút thời gian để hội tụ. `array-capture --smart` là **tính năng tiện lợi được tích hợp sẵn**: nó chờ cho đến khi AE ổn định trên mọi camera trong mảng, sau đó kích hoạt quá trình chụp. Sử dụng tính năng này khi bạn thay đổi cảnh giữa phiên làm việc.

```bash
# Connect once, then take settled captures whenever you re-point the rig
chloros-cli lattice array-connect --serials SN1,SN2,SN3,SN4
chloros-cli lattice array-capture --smart --processing reflectance -o pose_a/
# (move the rig)
chloros-cli lattice array-capture --smart --processing reflectance -o pose_b/
```

Chính sách ổn định mặc định khá thận trọng: thời gian chờ 5 giây, khoảng thời gian ổn định 1,5 giây, dung sai độ phơi sáng ±5%. Điều chỉnh thông qua Giao diện Điều khiển Tự động (SDK) (`ArrayHandle.capture_smart(settle_timeout_s=…, stability_window_s=…, exposure_tolerance_pct=…)`) nếu bạn cần hành vi khác so với chế độ tự động.

### Cấp độ xuất ảnh (mặc định là `all`)

Kể từ phiên bản này, `lattice capture`, `lattice multi-capture` và `lattice array-capture` **mặc định là `--processing all`** — một tệp lưu cho mỗi loại xuất áp dụng cho từng camera, phù hợp với hành vi hành vi &quot;Capture All&quot; (Chụp tất cả). Các cấp độ là:

| Cấp độ | Đầu ra | Áp dụng cho |
| --- | --- | --- |
| `raw` | Bayer một kênh (camera đơn sắc: dải đơn) trực tiếp từ cảm biến. | Tất cả các camera. |
| `debayered` | Demosaic BGR 3 kênh (camera đơn sắc: 1 kênh thang xám). | Tất cả các camera. |
| `radiance` | float32 W/m²/sr/nm thông qua chuỗi đo quang học đầy đủ. | Chỉ dành cho đa phổ (M3C/M3M) — **bỏ qua đối với các camera có bộ lọc RGB**. |
| `reflectance` | uint16 ρ (`32768` = 1.0), tương thích với Pix4D. | Chỉ đa quang phổ, và **chỉ khi có DAQ được liên kết + camera đã được hiệu chuẩn**; nếu không thì sẽ bị bỏ qua. |
| `preview` / `display` | Chuỗi xem trước đầy đủ trên GUI (CCM + WB + gamma theo cấu hình của camera). `lattice capture` đặt tên cho `preview`; `array-capture`/`multi-capture` sử dụng `display`. | Tất cả các camera. |

Chỉ định một mức duy nhất để chỉ lưu mức đó (`--processing debayered`). Khi bạn yêu cầu `all`, các mức không áp dụng cho cam cụ thể sẽ bị bỏ qua (và được báo cáo), không gây lỗi — một camera không kết nối hoặc chưa được hiệu chuẩn vẫn nhận được `raw` / `debayered` / `preview`.

Đối với bất kỳ khung hình phản xạ nào, giá trị đo hướng xuống từ DAQ thực sự được sử dụng sẽ được ghi vào tệp phụ **`.daq`** bên cạnh hình ảnh (để có thể xử lý lại dữ liệu thu thập sau này) và được báo cáo trên dòng `DAQ:`.

### Cấu trúc thư mục dữ liệu thu thập

Mỗi loại xuất dữ liệu được lưu vào **thư mục con riêng** dưới `-o`, do đó một bản chụp đa cấp độ sẽ không bao giờ trộn lẫn các loại:

```
output/
├── raw/           capture_<ts>_SN<serial>_raw.tif
├── debayered/     capture_<ts>_SN<serial>_debayered.tif
├── radiance/      capture_<ts>_SN<serial>_radiance.tif
├── reflectance/   capture_<ts>_SN<serial>_reflectance.tif
├── preview/       capture_<ts>_SN<serial>_display.tif
├── index/         per-camera vegetation-index (LUT) render, when --index is on
├── composite/     array foreground/background live-view composite, when produced
└── *.daq          the downwelling reading matched to the capture
```

`<ts>` là dấu thời gian chụp và `<serial>` là số sê-ri máy ảnh, do đó một nhóm đồng bộ sẽ chia sẻ
dấu thời gian chung trên các máy ảnh. **Lưu ý điểm bất đối xứng duy nhất:** cấp độ `display` được lưu trữ trong một thư mục
có tên `preview/` trong khi các tệp tin vẫn giữ `_display` trong tên — thư mục và hậu tố chỉ khác nhau
ở cấp độ đó. Các cấp độ không xác định sẽ được lưu vào một thư mục mang tên của chính chúng, và nếu thư mục con
không thể được tạo ra, tệp sẽ được ghi vào thư mục gốc đầu ra thay vì bị mất.

**Xử lý lại thư mục captures:**chỉ định `chloros-cli process` vào**thư mục gốc captures**
(`output/`). `process` thông thường chỉ nhập thư mục mà bạn chỉ định, nhưng khi thư mục đó không chứa
hình ảnh và có các thư mục con, nó sẽ tự động đi sâu vào các thư mục con — do đó, các thư mục con cùng cấp với thư mục gốc và chính
thư mục gốc `.daq` đều được nhập cùng một lúc. Mỗi cấp độ của một bản chụp sẽ được nhập dưới dạng một hình ảnh duy nhất, trong khi
các cấp độ còn lại có sẵn dưới dạng chế độ, thay vì mỗi cấp độ là một hình ảnh riêng biệt.

Đặt tên cho một **thư mục con cấp độ** trực tiếp (ví dụ: `output/raw/`) cũng hoạt động. Làm như vậy sẽ bỏ lại thư mục gốc
`.daq`, do đó hãy sao chép hoặc chỉ định dữ liệu đọc từ DAQ cùng lúc khi bạn tái tính toán sản phẩm đo xạ
từ `raw/` — nếu không, việc khớp dấu thời gian sẽ không có cơ sở để giải quyết.

**Quá trình xử lý luôn bắt đầu từ `raw`.** Trong mỗi lần thu thập, khung hình thô là nguồn của đường ống xử lý;
`debayered`, `radiance`, `reflectance` và `preview` xuất hiện dưới dạng các chế độ xemnhưng không bao giờ được đưa
trở lại qua đường ống xử lý. Việc xử lý lại một sản phẩm dẫn xuất sẽ áp dụng lại hiệu ứng vignette, CCM và
các phép tính radiance đã được tích hợp sẵn vào các pixel của nó, vì vậy Chloros sẽ từ chối thay vì
xử lý hai lần. Hai hệ quả đáng lưu ý:

- Các bản render `index/` và `composite/` **không bao giờ** được xử lý. Chúng là đầu ra, không phải dữ liệu chụp —
  một bản render LUT từ NDVI không có thông tin radiance có ý nghĩa.
- Thư mục captures được xuất **không** chứa `raw` (ví dụ: `array-capture --processing reflectance`) sẽ
  không có nguồn hợp lệ trong quy trình xử lý. Các dữ liệu ghi lại này vẫn được nhập và hiển thị bình thường, nhưng `process` sẽ bỏ qua
  chúng và thông báo như sau:

  ```
  [IMPORT-LEVEL] Skipping 4 already-processed file(s) with no raw source: capture_…_reflectance.tif
  [IMPORT-LEVEL] Processing starts from raw. Re-capture with --processing raw, or force an entry
                 point with --input-level.
  ```

  Nếu bạn thực sự cần đẩy một sản phẩm phái sinh qua — một phiên hub được ghi lại với
  `demosaic` được bật, hoặc một thư mục cũ — `--input-level {raw,debayered,processed}` sẽ ép buộc điểm vào
  và vô hiệu hóa lệnh bỏ qua. Cờ này là lối thoát được thiết kế có chủ đích; `auto` (mặc định)
  không bao giờ xử lý bản ghi nào không có dữ liệu thô.

### Các bản ghi bị bỏ qua trong mảngMảng lọc hỗn hợp

Khi bạn kết hợp các camera đơn phổ (RGB) và đa phổ (multispectral) trong cùng một mảng, `array-capture --processing radiance` (hoặc `reflectance`) sẽ lưu các khung hình đa phổ và **bỏ qua** các camera RGB — độ rạng rỡ theo từng điểm Bayer không có ý nghĩa đối với cảm biến băng thông rộng. Tệp CLI in ra từng tệp đã lưu (kèm mức xuất), từng dòng `.daq` được ghi và từng lần bỏ qua một cách rõ ràng, do đó số lượng tệp không gây ngạc nhiên:

```
  Saved: output/sync_…_SN213800234.tif [reflectance] (SN:213800234, fid:1)
  Saved: output/sync_…_SN214000533.tif [reflectance] (SN:214000533, fid:1)
  Saved: output/sync_…_SN214701288.tif [reflectance] (SN:214701288, fid:1)
  DAQ:   output/sync_…_daq-e-54b5e0.daq
  Skipped: SN:214701292 (reflectance-not-applicable-to-rgb-cam filter=RGB)

  3 synchronized frames captured. (1 skipped)
```

Các mã lý do bỏ qua tuân theo mẫu `<level>-not-applicable-to-rgb-cam`. Độ phản xạ cũng có thể bị bỏ qua với `reflectance-skipped-no-fresh-dls` / `reflectance-skipped-bound-daq-unavailable (…)`, và với `dls-uncalibrated-band-<nm>` khi dải sóng chủ yếu nằm ngoài phạm vi hiệu chuẩn quang phổ của cảm biến ánh sáng DAQ (~374–974 nm) — trong số các mã sản phẩm (SKU) đang được phân phối, chỉ có F988 hỗ trợ quy trình làm việc với bảng đo độ phản xạ.

Sử dụng `--processing debayered` (hoặc `display`) để bao gồm mọi camera bất kể loại bộ lọc, hoặc sử dụng giá trị mặc định `all` để thu được mọi mức độ áp dụng cho từng camera chỉ trong một lần.

---

## Chế độ chụp, bộ ghi và xử lý lại ngoại tuyến

Tất cả các chức năng này đều hoạt động trên một **mảng lưu trữ** (chạy lệnh `array-connect` trước tiên). Chúng phản ánh bảng điều khiển chụp trên giao diện người dùng (GUI).

### Các chế độ `array-capture`

`array-capture` là một lệnh duy nhất với bốn chế độ màn trập cùng một bộ tùy chọn xuất:

| Chế độ | Cờ | Hành vi |
| --- | --- | --- |
| **Đơn lẻ** *(mặc định)* | (không có) | Một nhóm chụp đồng bộ, sau đó thoát. |
| **Liên tục** | `--continuous` | Các lần quét liên tiếp cho đến khi gặp `Ctrl+C`, `--count N` hoặc `--duration S`. |
| **Khoảng thời gian** | `--interval S` | Một lần quét mỗi `S` giây (được tính từ khi bắt đầu mỗi lần quét), cùng phạm vi. |
| **Nhanh nhất** | `--fastest` | Chỉ dữ liệu thô + giá trị đọc DAQ được gán + thành phần chỉ số kết hợp; bỏ qua các phép tính về độ rạng rỡ/độ phản xạ/hiển thị để khung hình được hiển thị nhanh. Ngụ ý `--processing raw --force-daq`. Xử lý lại `.daq` đã lưu thành các sản phẩm đã hiệu chuẩn sau này. |

Các tùy chọn xuất (kết hợp với bất kỳ chế độ nào; tất cả đều sử dụng chung giao diện người dùng (GUI) và điểm cuối SDK):

| Cờ | Hiệu ứng |
| --- | --- |
| `--processing LEVEL` | Mức xuất duy nhất, hoặc `all` (mặc định). |
| `--levels L1,L2,…` | Tập con rõ ràng của các loại xuất (ví dụ: `raw,radiance,reflectance`); **ghi đè lên `--processing`**. |
| `--aligned` / `--no-aligned` | Điều chỉnh (warp) mọi xuất không thô (non-raw) của từng thành viên theo [hồ sơ căn chỉnh](#alignment) của mảng (được đăng ký chung). Dữ liệu thô (raw) vẫn giữ nguyên nhưng mang theo phép biến đổi trong siêu dữ liệu. Sẽ chuyển về chế độ không căn chỉnh (kèm cảnh báo) nếu mảng không có cấu hình. |
| `--index` / `--no-index` | Lưu / bỏ qua lớp phủ chỉ số thực vật cho từng camera khi lớp phủ này đã được cấu hình. Mặc định: hiển thị lớp phủ. |
| `--force-daq` | Lưu giá trị đo DAQ/DLS đã gán dưới dạng tệp sidecar `.daq` ngay cả khi không có mức nào được chọn yêu cầu (ví.g. chụp chỉ dữ liệu thô), để các khung hình có thể được xử lý lại thành phản xạ/chỉ số ngoại tuyến. |
| `--smart` | Chờ cho AE ổn định trên tất cả các camera trước khi kích hoạt (xem [Smart-AE / Smart-Capture](#smart-ae--smart-capture)). |
| `--compression {deflate,none}` | Nén pixel theo phương pháp &quot;TIFF&quot;. `deflate` (mặc định) = nén zlib L1 không mất dữ liệu + bộ dự đoán theo chiều ngang, ~4,1 MB cho mỗi khung hình độ phân giải đầy đủ; `none` = không nén, tốc độ ghi nhanh hơn ~5 lần với ~6,3 MB mỗi khung hình — sử dụng để đạt tốc độ duy trì tối đa khi dung lượng đĩa cho phép. Cả hai đều không mất dữ liệu và được đọc giống hệt nhau khi nhập. |

> **Ghi một lần TIFF + mô hình tốc độ duy trì.**Các bản ghi được ghi trong**một**lần xử lý tệp TIFF chứa pixel + XMP + IFD0 Make/Model (đo trên Mono12 độ phân giải đầy đủ: 36 ms (nén) / 6,5 ms (không nén), so với ~148 ms của phương pháp ghi trước rồiviết lại bằng ExifTool); công việc ExifTool duy nhất còn lại (hoàn thiện IFD con EXIF) chạy trên một tiến trình nền không đồng bộ, và một khung hình đã hoàn tất và sẵn sàng nhập ngay cả khi tiến trình này không bao giờ chạy. Lưu ý rằng nén DEFLATE giữ GIL của Python, do đó các thao tác ghi nén**không**song song trên các luồng ghi riêng cho từng camera — việc ghi liên tục ở độ phân giải đầy đủ với 8 camera ở tốc độ cảm biến (~10,4 khung hình/giây) yêu cầu `--compression none`**và** ổ đĩa loại NVMe (~500 MB/s tốc độ ghi liên tục). Cùng một tùy chọn này được cung cấp dưới tên `compression` trên `POST /api/camera/array/capture`.

```bash
# Interval timelapse: one reflectance pass every 10 s for 5 minutes
chloros-cli lattice array-capture --interval 10 --duration 300 \
  --processing reflectance -o timelapse/

# Fastest grab for a moving rig — raw + .daq now, calibrate later
chloros-cli lattice array-capture --fastest -o flightline/

# Co-registered multi-band export (drop the index overlay)
chloros-cli lattice array-capture --processing reflectance --aligned --no-index -o out/
```

### `array-record` — video/GIF chỉ mục kết hợpvideo/GIF chỉ mục kết hợp (cấp độ giám sát)

Ghi lại bất kỳ nội dung nào mà **chế độ xem chỉ mục kết hợp trực tiếp** đang hiển thị vào tệp `.avi` (và tùy chọn vào tệp `.gif`). Vì nó lấy dữ liệu từ luồng tổng hợp trực tiếp, luồng kết hợp phải đang mở (ví dụ: mảng đang được xem trước trong GUI) để các khung hình được ghi lại. Nó kiểm tra tiến trình cứ sau 2 giây và dừng lại khi gặp `--duration`, `Ctrl+C`, hoặc khi trình ghi tự động kết thúc.

```bash
# 30-second combined-index clip at 10 fps, plus a GIF
chloros-cli lattice array-record --duration 30 --fps 10 --gif -o monitoring/
```

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--array-id ID` | chỉ mảng | Mảng mục tiêu (bỏ qua nếu chỉ có một mảng được kết nối). |
| `-o, --output DIR` | `output` | Thư mục đầu ra (trên máy chủ). |
| `--fps F` | `10` | Tốc độ khung hình ghi hình. |
| `--duration S` | cho đến khi nhấn Ctrl+C | Tự động dừng sau `S` giây. |
| `--gif` | tắt | Cũng ghi tệp GIF động. |
| `--gif-only` | tắt | Chỉ ghi tệp GIF (không có `.avi`). |

### `array-burst` — chế độ chụp liên tiếp raw-Bayer tốc độ caofps (chất lượng phân tích)

Đọc trực tiếp bộ đệm nhóm đồng bộ của vòng lặp chụp — **không cần chuỗi hiệu chuẩn, không cần exiftool, không cần chế độ xem trực tiếp** — do đó nó chạy ở tốc độ chụp tối đa của máy ảnh. Ghi các khung hình thô + bản kê khai cho từng khung hình + một `.daq` cho mỗi lần đọc DLS riêng biệt dưới `<output>/bursts/<base>/`. Xử lý lại ngoại tuyến (lệnh tiếp theo) hoặc truyền `--build` để thực hiện ngay lập tức khi dừng.

```bash
# 5-second raw burst, then build the combined index video in one shot
chloros-cli lattice array-burst --duration 5 --build \
  --products combined:index --fps 10 -o capture/
```

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--array-id ID` | chỉ mảng | Mảng đích. |
| `-o, --output DIR` | `output` | Thư mục đầu ra (dữ liệu đợt sẽ được lưu vào `<DIR>/bursts/<base>/`). |
| `--duration S` | cho đến khi nhấn Ctrl+C | Tự động dừng sau `S` giây. |
| `--max-frames N` | không giới hạn | Tự động dừng sau khi xử lý xong `N` khung hình thô. |
| `--build` | tắt | Sau khi dừng, ngay lập tức xử lý lại loạt ảnh (giống như `array-build-video`). |
| `--products …` | `combined:index` | Khi sử dụng `--build`: chọn video nào để tạo (xem bên dưới). |
| `--fps F` | `10` | Với `--build`: tốc độ khung hình (fps) của video đầu ra. |
| `--save-tiffs` | tắt | Với `--build`: cũng lưu các tệp TIFF đã hiệu chỉnh cho từng khung hình. |
| `--gif` | tắt | Khi sử dụng `--build`: cũng ghi tệp GIF động. |

### `array-build-video` — xử lý lại ngoại tuyến một loạt ảnh đã lưu

Đồng bộ hóa thời gian từng khung hình thô với giá trị `.daq` đã lưu gần nhất và đưa nó qua **chuỗi radiance / reflectance / index giống như trong quy trình nhập**, từ đó tạo ra một hoặc nhiều video.

`--products` là danh sách các mục `kind:level` được phân tách bằng dấu phẩy, trong đó `kind` ∈ `per_cam` | `combined` và `level` ∈ `radiance` | `reflectance` | `index`. Một `level` (không có `kind:`) sẽ mặc định là `per_cam`. Giá trị mặc định là `combined:index`.

```bash
# Per-cam reflectance video for every member + one combined NDVI video
chloros-cli lattice array-build-video \
  --burst-dir "capture/bursts/2026-06-24_141500" \
  --products per_cam:reflectance,combined:index \
  --fps 10 --save-tiffs
```

| Cờ | Giá trị mặc định | Mô tả |
| --- | --- | --- |
| `--burst-dir DIR` | (bắt buộc) | Đường dẫn đến thư mục burst (`…/bursts/<base>/`). |
| `--products …` | `combined:index` | Danh sách `kind:level`, như trên. |
| `--fps F` | `10` | Tốc độ khung hình video đầu ra (fps). |
| `--save-tiffs` | tắt | Cũng lưu các tệp TIFF đã hiệu chỉnh từng khung hình cùng với video(s). |
| `--gif` | tắt | Cũng ghi các tệp GIF động. |

> **Chọn trình ghi phù hợp.** `array-record` là *cấp độ giám sát* — nó ghi lại hình ảnh tổng hợp trực tiếp như hiển thị và cần luồng dữ liệu mở. `array-burst` → `array-build-video` là *cấp độ phân tích* — nó lưu dữ liệu cảm biến thô ở tốc độ đầy đủ và tái tạo các video độ sáng/độ phản xạsau đó, không cần xem trực tiếp.

### Máy ảnh đơn dải (M3M)

Dòng **M3M**là phiên bản đơn dải tương đương với dòng Bayer**M3C**: mỗi máy ảnh có một bộ lọc nhiễu dải hẹp (`M3M-<lens>-F<wavelength>`, ví dụ: `M3M-L87-F685`), do đó cảm biến cung cấp**một dải xám duy nhất** mà không có mô-zaic Bayer. Không cần thực hiện quá trình khử mô-zaic, không có hiện tượng nhiễu chéo giữa các kênh cần tách, và không cần thiết lập cân bằng trắng — toàn bộ quy trình xử lý màu từ cảm biến đến màn hình (RGB -quy trình xử lý màu hiển thị hoàn toàn không áp dụng.

Điều đó có nghĩa là trên CLI:

- **`lattice white-balance`, `lattice color-profile`, `lattice color`**phát hiện camera đơn sắc và**bỏ qua với một thông báo một dòng** thay vì áp dụng các thiết lập vô nghĩa. Chúng vẫn hoạt động bình thường với camera RGB /Bayer M3C trong cùng một phiên làm việc.
- **`lattice calibrate` / `process --reflectance` / `array-capture --processing radiance`** vẫn hoạt động — độ rạng rỡ và độ phản xạ là các bản đồ đo quang *theo từng dải* và được định nghĩa hoàn hảo cho một dải. Các khung hình đơn sắc mang ma trận phản hồi cảm biến **bản sắc** (không có tách màu 3×3), do đó mặt phẳng đi qua các phép toán hiệu chuẩn mà không bị thay đổi.
- **Một camera đơn sắc duy nhất không thể tạo ra chỉ số thực vật.**NDVI / NDRE / v.v. cần ít nhất hai dải (ví dụ: Red + NIR). Để tính chỉ số từ thiết bị đơn sắc, hãy hướng**nhiều** camera M3M vào các bước sóng khác nhau, ghép chúng thành một chồng dữ liệu đa dải, và tính chỉ số *từ chồng dữ liệu đó*:

```bash
# Red (660) + NIR (850) mono pair -> aligned 2-band stack -> NDVI
chloros-cli lattice array-connect --serials SN_RED,SN_NIR
chloros-cli lattice index --live --profile align.json \
  --preset NDVI --channel red=Red_660 --channel nir=NIR_850 \
  --save-multiband -o output/
```

`--channel` các ký hiệu phải khớp **chính xác** với tên kênh của cài đặt sẵn (phân biệt chữ hoa chữ thường; các giá trị trong NDVI là chữ thường `red`,`nir` — xem `--list-presets`), và tên phía băng tần phải trùng khớp với tên một băng tần trong ngăn xếp được căn chỉnh (chế độ ngoại tuyến cũng chấp nhận chỉ số băng tần bắt đầu từ 0, ví dụ: `--channel red=0 --channel nir=1`).

Yếu tố phân biệt trong toàn bộ ngăn xếp là mã `M3M` trong chuỗi mô hình (mã này không bao giờ xuất hiện trong chuỗi `M3C`), được hiển thị trên GUI/SDK dưới dạng `is_mono`.

---

## Cài đặt và Tối ưu hóa NIC Máy chủ (Mảng LATTICE)

Các camera LATTICE truyền dữ liệu GVSP qua bộ điều hợp Ethernet của máy chủ, do đó đối với các mảngmáy ảnh, **trình điều khiển**và**kích thước vòng nhận** của bộ điều hợp quan trọng không kém tốc độ kết nối. Các thiết lập sai sẽ hiển thị dưới dạng cổng `FRAMES WILL DROP` / `Reduce ROI to enable` trong bảng Cài đặt Mảng (và trong `lattice network-analysis` / `analyze_array_network()` của SDK), ngay cả khi chính các camera vẫn hoạt động bình thường.

### Bộ điều hợp USB 10GbE — Realtek RTL8157 (&quot;Realtek USB 10GbE Family Controller&quot;)

| Mục | Giá trị bắt buộc | Tại sao điều này quan trọng |
| --- | --- | --- |
| **Phiên bản trình điều khiển**|**≥ v10.67 (tháng 1 năm 2026)**, INF `rtump64x64sta.inf` | Trình điều khiển cũ**năm 2016**(v10.65, `rtump64x64.inf`) xử lý không đúng cách việc tắt nguồn và gây ra lỗi**`DRIVER_POWER_STATE_FAILURE` (BSOD `0x9F`)**khi tắt máy/khởi động lại/chế độ ngủ. Quá trình chuyển đổi bị treo (~5 phút hết thời gian chờ), người dùng phải tắt nguồn cưỡng bức, và các lần tắt máy không an toàn lặp đi lặp lại**sẽ làm hỏng kho lưu trữ WMI**(PowerShell/các công cụ bắt đầu gặp lỗi với mã `Invalid class`) và**làm treo hệ thống USB** trong lần khởi động tiếp theo (NIC không kích hoạt được; các thiết bị USB ngừng nhận diện). Hãy liên hệ với realtek.com (hoặc nhà cung cấp thiết bị USB) trước khi thực hiện khởi động lại an toàn. |
| **Bộ đệm nhận**— từ khóa `ReceiveBufferLen` |**256**(giá trị tối đa của trình điều khiển) | Vòng đệm nhận (RX ring) của NIC. Giá trị mặc định**32**của trình điều khiển chỉ để lại khoảng ~0,26 MB dung lượng vòng đệm có thể sử dụng — quá nhỏ để xử lý luồng dữ liệu từ nhiều camera — do đó bảng điều khiển mảng báo lỗi `Sim-emit burst … exceeds NIC RX ring usable capacity 0.26 MB` và chặn kết nối. Ở mức**256**, vòng đệm này rất lớn (**~13,5 MB được đo trên máy chủ 10GbE trong phòng thí nghiệm**), cung cấp cho đường ống nhận (RX pipeline) dung lượng dự trữ thực sự cho các đợt truyền dữ liệu đađợt truyền dữ liệu GVSP từ nhiều camera. (Việc một cấu hình cụ thể có thực sự *kết nối* được hay không được quyết định bởi hai kiểm tra — kiểm tra khả năng tiếp nhận **nhận biết mức xả**và kiểm tra**tổng mức đăng ký vượt quá** — chứ không phải so sánh trực tiếp giữa đợt truyền dữ liệu và vòng đệm; xem [Mô hình fps &amp; đợt dữ liệu của mảng](#array-fps--burst-model).) |
| **URB nhận**— từ khóa `PendingReceives` |**64** (tối đa) | Các khối yêu cầu USB đang truyền; tăng cùng với Bộ đệm nhận để hấp thụ các đợt truyền dữ liệu đột biến. |
| **Khung Jumbo** — từ khóa `*JumboPacket` | **9014** | Cần thiết cho các gói GVSP 9000 byte (số gói/khung ít hơn 6 lần so với 1500). |

> ⚠️ **Việc cập nhật trình điều khiển NIC sẽ ĐẶT LẠI các thuộc tính nâng cao này về giá trị mặc định.**Sau khi cập nhật hoặc thay thế trình điều khiển bộ điều hợp,**hãy áp dụng lại** `ReceiveBufferLen=256` và `PendingReceives=64`, nếu không bảng điều khiển mảng sẽ lại bị khóa dù &quot;không có gì thay đổi về phần cứng&quot;. Đây là nguyên nhân hàng đầu khiến một hệ thống từng hoạt động bình thường bỗng dưng từ chối kết nối.

Áp dụng từ **quyền quản trị** (thay thế bằng tên bộ điều hợp của bạn, ví dụ: `"Ethernet 5"`):

```powershell
Set-NetAdapterAdvancedProperty -Name "Ethernet 5" -RegistryKeyword ReceiveBufferLen -RegistryValue 256
Set-NetAdapterAdvancedProperty -Name "Ethernet 5" -RegistryKeyword PendingReceives  -RegistryValue 64
Get-NetAdapterAdvancedProperty  -Name "Ethernet 5" -RegistryKeyword ReceiveBufferLen,PendingReceives   # verify
```

> **`lattice network --fix` áp dụng cho các bộ điều hợp USB 10GbE.** Giờ đây, hệ thống sẽ phát hiện loại bộ điều hợp và điều chỉnh từ khóa receive-ring phù hợp: `*ReceiveBuffers`→2048 cho các NIC PCIe (Intel I219, v.v.), hoặc `ReceiveBufferLen`→256 + `PendingReceives`→64 cho bộ điều khiển **USB** 10GbE của Realtek (không hỗ trợ `*ReceiveBuffers`). Các giá trị mục tiêu được giới hạn ở mức tối đa do trình điều khiển báo cáo (`NumericParameterMaxValue`), do đó không bao giờ ghi giá trị ngoài phạm vi. Chạy lệnh này từ một cửa sổ terminal **có quyền quản trị**; giống như bất kỳ tùy chỉnh dựa trên registry nào, thay đổi sẽ có hiệu lực sau khi khởi động lại bộ điều hợp hoặc khởi động lại hệ thống. Các lệnh `Set-NetAdapterAdvancedProperty` thủ công ở trên vẫn là một lựa chọn tốt — chúng áp dụng ngay lập tức (gán lại bộ điều hợp) mà không cần khởi động lại.

### Kiến thức cơ bản về mạng (tất cả các liên kết LATTICE)

- **Địa chỉ:** địa chỉ liên kết cục bộ `169.254.0.0/16` (GigE Vision LLA). Máy chủ sử dụng địa chỉ tĩnh `169.254.x.x/16`; các camera và DAQ-E tự gán địa chỉ trong cùng dải. Không cần DHCP/cổng mạng.
- **Kích thước gói tin:**ưu tiên jumbo (9000), nhưng hãy để tính năng tự động dò tìm xác định — tính năng này sẽ đo lại mỗi khi kết nối và đã bỏ qua giới hạn ICMP 1500 byte của cameragiới hạn ICMP 1500 byte của camera thông qua lệnh dò GVSP, do đó sẽ sử dụng kích thước jumbo bất cứ khi nào đường truyền thực sự hỗ trợ. Chỉ nên cố định giá trị với `CHLOROS_GVSP_PACKET_SIZE_FORCE=9000` khi bạn chắc chắn hơn so với kết quả dò, và ưu tiên thiết lập theo từng lệnh thay vì cố định vĩnh viễn: việc cố định sẽ bỏ qua quá trình dò, do đó nếu đường truyền thực tế không thể truyền 9000**mỗi** lần ghi lại sẽ hết thời gian chờ với `SC_ERR_TIMEOUT -1011` (xem [Biến môi trường](#environment-variables)).
- **Vòng RX mở rộng theo giá trị `ReceiveBufferLen`:**ở giá trị mặc định `32`, dung lượng vòng có thể sử dụng là ~0,26 MB (quá nhỏ cho bất kỳ đợt truyền dữ liệu đa camera nào); ở giá trị tối đa `256` , dung lượng này rất lớn (~13,5 MB khi đo trên máy chủ 10GbE trong phòng thí nghiệm), mang lại dung lượng dự phòng thực sự. Việc một cấu hình có kết nối được hay không sau đó được quyết định bởi kiểm tra khả năng chấp nhận dựa trên lưu lượng thoát**và** kiểm tra tổng mức đăng ký vượt quá dưới đây — chứ không phải so sánh trực tiếp giữa tốc độ bùng nổ và dung lượng vòng.

### Tốc độ khung hình mảng &amp; mô hình chụp liên tiếp

Cách đọc bảng Cài đặt Mảng (và `lattice analyze-array` / `analyze_array_network` của SDK):

- **Dữ liệu chụp liên tiếp được cộng dồn theo từng camera ở định dạng pixel thực của camera đó.**Camera đơn sắc**M3M**phát luồng**Mono12 (2 B/px)**; camera Bayer**M3C**phát luồng 8- hoặc 12-bit (TRI032S tự động phát ra BayerRG12 ngay cả khi được yêu cầu BayerRG8). Do đó, một khung hình độ phân giải đầy đủ từ 4 camera có dung lượng**~12,6 MB nếu tất cả đều là 8-bit nhưng ~25 MB với ba camera đơn sắc 12-bit**. Hệ thống xử lý xác định định dạng của từng camera dựa trên mẫu (bộ nhớ đệm nhận dạng), do đó dữ liệu burst khớp với những gì thực tế được truyền qua đường truyền — chứ không phải dựa trên giả định chung chung là BayerRG8.
- **Bộ chuyển đổi Ethernet qua USB bị giới hạn ở tốc độ tối đa 200 MB/s bất kể thông số kỹ thuật ghi trên nhãn.** Bảng hiệu suất chuyển đổi tốc độ kết nối thành giá trị duy trì được lấy từ PCIe; một NIC USB công bố tốc độ kết nối *Ethernet* của nó nhưng bị giới hạn bởi bus USB và trình điều khiển của nó. Một bộ chuyển đổi USB 10GbE từng đạt ~1063 MB/s &quot;ổn định” — con số này chưa bao giờ được kiểm chứng — và cơ chế điều tiết tốc độ đã làm hỏng 6–18 % các khung dữ liệu trong khi vẫn báo cáo tốc độ khung hình mục tiêu (fps) ở mức ổn định. Các bộ điều hợp mạng (NIC) kết nối qua USB hiện bị giới hạn tuyệt đối ở mức **200 MB/s** (giới hạn này do bus quy định, nên không phụ thuộc vào thông số kỹ thuật ghi trên nhãn; một bộ chuyển đổi USB 1 GbE đạt ~80 MB/s và không bị ảnh hưởng). `wire_ceiling_source` trong bản ghi khả năng nêu rõ điều này bằng văn bản, và `nic_is_usb` đánh dấu nó. Bỏ qua cả hai cách bằng `--wire-ceiling-mbps`.
- **Khả năng chấp nhận dữ liệu phụ thuộc vào dòng dữ liệu (drain-aware), không phải so sánh giữa toàn bộ đợt truyền (whole-burst) và vòng lặp (ring).** Một đợt truyền đồng thời chỉ cần phù hợp với *dữ liệu tồn đọng tạm thời* = `max(0, Σ per-cam arrival − host drain) × emit_window`, chứ không phải toàn bộ đợt truyền. Trên kiến trúc máy chủ nhanh / cam chậm (một máy chủ **PCIe**10G + 4× cam 1 GbE: tốc độ đến ≈ 320 MB/s, tốc độ xả ≈ 1063 MB/s), máy chủ xả dữ liệu nhanh hơn tốc độ lấp đầy của các camera, lượng tồn đọng ≈ 0, do đó chế độ phát mô phỏng độ phân giải đầy đủ (full-res sim-emit)**vẫn được chấp nhận**mặc dù gói dữ liệu 25 MB vượt quá dung lượng vòng đệm 13,5 MB. Đặt cùng bốn camera này phía sau một**USB**10GbE thì tốc độ xả dữ liệu là 200 MB/s, chứ không phải 1063 — tốc độ đến vượt quá tốc độ xả, và sự mất mát thể hiện dưới dạng các khung hình bị hỏng thay vì tốc độ khung hình thấp hơn. Trên máy chủ 1 GbE, ngưỡng DLThr tối thiểu 31,25 MB/s của các camera khiến tốc độ đến vượt quá tốc độ xả → hệ thống sẽ**chặn** đúng cách (đối với *loại* chặn này, hãy giảm ROI hoặc sử dụng binning ≥ 2). Điều kiện cho phép là một trong **hai** cổng kết nối — cổng còn lại là kiểm tra quá tải tổng hợp ở phần dưới.
- **Tốc độ khung hình dự kiến (fps) là giới hạn tối đa thận trọng khi truy xuất tuần tự.**Vòng lặp lấy dữ liệu của máy chủ hiện đang kéo bộ đệm của từng camera**theo thứ tự**(~một cửa sổ phát cho mỗi camera), do đó chu kỳ này bị giới hạn bởi `max(readout+emit, N × emit)` với tốc độ phát của mỗi camera bị giới hạn ở**kết nối truy cập**của camera (1 GbE ≈ 80 MB/s), chứ không phải đường truyền lên của máy chủ. Đối với mảng 4 camera độ phân giải đầy đủ 12-bit, con số này là**~2,8 fps**, khớp với giá trị đo được ~2,7–3,0 fps và được thiết kế**không phụ thuộc vào thời gian phơi sáng**, do đó trong các cảnh thiếu sáng, tốc độ thực tế có thể giảm nhẹ xuống dưới mức trần khi thời gian phơi sáng kéo dài. Quá trình truy xuất nối tiếp mới là yếu tố thực sự giới hạn fps; việc song song hóa quá trình này sẽ nâng mức trần lên gần với tốc độ phát tín hiệu đơn.
- **Tình trạng quá tải tổng hợp là yếu tố cản trở kết nối nghiêm trọng.**Mức phân bổ băng thông tối thiểu cho mỗi camera là**8 MB/s**(`ARRAY_PER_CAM_FLOOR_BPS`), do đó khi mức tối thiểu này được cố định, nhu cầu tổng hợp (`per_cam × N`) có thể vượt quá**giới hạn an toàn va chạm trên đường truyền**(`sustained × sim_emit_factor`). Giới hạn thực tế ở độ phân giải đầy đủ trên 1 GbE:**6 camera ở 1500 MTU, 9 camera khi dùng gói jumbo**. Giới hạn này chỉ phụ thuộc vào đường truyền và mức sàn — nó**không phụ thuộc vào kích thước khung**, do đó**việc gộp khung và thu nhỏ vùng quan tâm (ROI) KHÔNG giúp ích** (chúng chỉ giảm số byte trên mỗi *khung*, chứ không phải số byte theo nhịp GevSCPD trên mỗi *giây*); các giải pháp duy nhất là giảm số lượng camera, sử dụng khung jumbo từ đầu đến cuối, hoặc dùng NIC nhanh hơn. Triệu chứng sẽ là mất gói tin GVSP, chứ không phải giảm fps một cách từ từ, do đó `analyze-array` đặt các giá trị-fps có thể đạt được về 0 và in ra `**OVER-SUBSCRIBED**`, còn `array-connect` với độ phân giải cố định **sẽ từ chối kết nối** (nếu không, chế độ walk-down sẽ gộp khung hình lại, điều này cũng không giải quyết được loại lỗi này). `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED=1` hạ cấp tình trạng từ chối này thành một cảnh báo rõ ràng dành cho công việc thử nghiệm — xem [Biến môi trường](#environment-variables).

### Tình trạng mảng — hệ thống con nào đang mất khung hình

Mảng đã kết nối có mã lỗi `GET /api/camera/array/<array_id>/capability` chứa một khối
`health` đang hoạt động, được đánh giá lại theo cửa sổ **10 giây** lăn. Nó phân chia tình trạng mất khung
thành hai nguyên nhân cần các biện pháp khắc phục trái ngược nhau, thay vì báo cáo một tỷ lệ &quot;không hoàn chỉnh&quot;
mà không chỉ ra cụ thể nguyên nhân nào:

| Trường | Ý nghĩa | Hệ thống con nào |
| --- | --- | --- |
| `gvsp_corrupt_rate_pct` (theo cổng nối tiếp) | Khung dữ liệu **đã đến nhưng có cấu trúc sai**— mất gói tin GVSP. |**Mạng**: băng thông đường truyền, tốc độ truyền, vòng RX của NIC, MTU |
| `never_arrived_rate_pct` (theo số sê-ri) | Khung dữ liệu **chưa bao giờ đến**— máy ảnh không kích hoạt, hoặc không có dữ liệu nào được gửi đi. |**Kích hoạt / đồng bộ**: cáp M8, `--line`, `TriggerMode` |
| `worst_gvsp_corrupt_pct` / `worst_never_arrived_pct` | Tỷ lệ tệ nhất của từng camera. | — |
| `per_cam_rate_pct` | Tỷ lệ không hoàn chỉnh tổng hợp trên mỗi camera (cả hai nguyên nhân cộng lại). | — |
| `stable_for_seconds` | Thời gian mỗi camera duy trì ở mức dưới 0,01 %. | — |

Khi vượt quá 5%, hệ thống backend sẽ ghi lại một dòng `[array-health <id>] WARN` nêu rõ sự phân chia — khi
xảy ra vi phạm lần đầu, khi thay đổi mức độ nghiêm trọng, mỗi phút một lần trong suốt thời gian tình trạng này tồn tại, và một lần khi
tình trạng này được khắc phục. Nửa bị lỗi in ra `[gvsp-corrupt <SN>]` khi lần đầu tiên phát hiện vi phạm cho mỗi camera và
lý do, sau đó tổng hợp lại mỗi 60 giây. Mọi đánh giá vẫn được ghi vào tệp nhật ký phía sau;
các bộ đếm tiếp tục tăng lên với mỗi đợt xử lý, bất kể nội dung được in ra là gì.

Cùng một bản ghi báo cáo số liệu mà toàn bộ phân bổ phụ thuộc vào:

| Trường | Ý nghĩa |
| --- | --- |
| `wire_ceiling_mbps` | Ngân sách băng thông liên tục hiện tại của máy chủ, MB/s. |
| `wire_ceiling_source` | Nguồn gốc của con số đó, bằng văn bản — ví dụ: `USB-capped 200 MB/s (was theoretical 1062; PnPDeviceID=USB\VID_0BDA&PID_815A)` hoặc `user override 120 MB/s (auto said 200)`. |
| `wire_ceiling_is_user_set` | `true` khi `--wire-ceiling-mbps` (hoặc trường **Wire Budget**) trên giao diện người dùng (GUI) thiết lập giá trị đó. |
| `nic_is_usb` | `true` đối với bộ chuyển đổi Ethernet qua USB — xem giới hạn 200 MB/s ở trên. |

**Cách đọc:** Giá trị `gvsp_corrupt_rate_pct` khác 0 trong khi `never_arrived_rate_pct` bằng 0
có nghĩa là kích hoạt và đồng bộ cáp hoàn hảo, và 100% mất mát nằm trên đường truyền mạng
— giảm giá trị `--wire-ceiling-mbps` và kết nối lại. Mô hình ngược lại cho thấy vấn đề nằm ở
cáp đồng bộ hoặc đường tín hiệu kích hoạt.

> **`--target-fps` không phải là yếu tố gây ra các khung dữ liệu bị hỏng.** Tốc độ điều chỉnh GevSCPD được thiết lập
> một lần khi kết nối, do đó việc giảm tần số kích hoạt sẽ thay đổi chu kỳ làm việc chứ không phải
> tốc độ phát đồng thời theo đợt. Việc cắt giảm 5 lần nhu cầu theo đo lường không mang lại cải thiện;
> việc giảm giới hạn băng thông từ 240 xuống 200 MB/s đã đưa cùng hệ thống từ 10,4 %
> xuống 0,00%.

> **Tính năng tự động thu hẹp giữa luồng không khả dụng trên firmware TRI032S.** Một mảng đang chạy
> không thể tự khắc phục sự cố này; hãy ngắt kết nối và kết nối lại để công cụ chọn thời gian kết nối có thể
> lập kế hoạch lại với giới hạn mới.

### Triệu chứng → cách khắc phục

| Triệu chứng (Cài đặt mảng / kết nối / `analyze_array_network`) | Nguyên nhân | Cách khắc phục |
| --- | --- | --- |
| `FRAMES WILL DROP … exceeds NIC RX ring usable capacity 0.26 MB`, `Reduce ROI to enable` | `ReceiveBufferLen` được đặt lại về 32 (thường xảy ra sau khi cập nhật trình điều khiển) | Đặt `ReceiveBufferLen`→256, `PendingReceives`→64; mở lại bảng điều khiển (khởi động lại hệ thống nền nếu nó đã lưu kích thước vòng cũ vào bộ nhớ đệm) |
| Khởi động lại/tắt máy bị treo; sau đó xuất hiện lỗi WMI `Invalid class`, NIC không kích hoạt được, thiết bị USB bị mất | Trình điều khiển Realtek USB 10GbE cũ năm 2016 → Màn hình xanh (BSOD) `0x9F` → phải tắt nguồn cưỡng bức | Cập nhật trình điều khiển bộ điều hợp lên phiên bản ≥ v10.67 (2026), sau đó áp dụng lại các thiết lập vòng nhận ở trên |
| Kết nối thành công nhưng trả về độ phân giải thấp hơn tiêu chuẩn | Chế độ Smart-prep tự động thu nhỏ khung dữ liệu để phù hợp với đường truyền | Nâng cấp kết nối / chấp nhận việc thu nhỏ / `--force-tier slip-emit-and-capture` |
| Mảng báo cáo tốc độ khung hình mục tiêu bình thường nhưng chỉ truyền được một phần nhỏ của giá trị đó; `health.gvsp_corrupt_rate_pct` khác 0, `never_arrived_rate_pct` bằng 0 | Ngân sách băng thông đường truyền được suy luận của máy chủ cao hơn mức thực tế mà nó có thể duy trì (thường gặp trên bộ điều hợp USB Ethernet, làn PCIe hẹp hoặc cấu trúc mạng chia sẻ) | Kết nối lại với giá trị `--wire-ceiling-mbps` thấp hơn và kiểm tra lại khối trạng thái. **Không phải** `--target-fps` — Tốc độ điều chỉnh GevSCPD được cố định khi kết nối |
| Thiếu camera trong các nhóm đã công bố; `health.never_arrived_rate_pct` khác 0, `gvsp_corrupt_rate_pct` bằng 0 | Đường dẫn kích hoạt/đồng bộ — các camera không hoạt động, không phải là vấn đề mạng | Kiểm tra cáp đồng bộ M8 và `--line`; xác nhận mọi thành viên đã được kích hoạt (`TriggerMode=On`) |
| `**OVER-SUBSCRIBED**` / `Wire budget` vượt quá giới hạn trong `analyze-array`, hoặc từ chối kết nối với độ phân giảiđộ phân giải đã ghim (`array over-subscribes the wire`) | Tổng nhu cầu trên mỗi camera (mức tối thiểu 8 MB/s × N camera) vượt quá giới hạn an toàn va chạm của đường truyền — 6 camera ở độ phân giải đầy đủ trên 1 GbE @1500 MTU, 9 camera với khung jumbo | Sử dụng ít camera hơn, khung jumbo từ đầu đến cuối, hoặc card mạng (NIC) nhanh hơn. **ROI/binning SẼ KHÔNG GIÚP ĐƯỢC** (giới hạn trên không phụ thuộc vào kích thước khung). `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED=1` ghi đè trên bàn thử nghiệm (chấp nhận mất gói tin) |

---

## `chloros-cli daq`

Lệnh cảm biến quang phổ. Hai loại:
- **`pool-*`**— các client &quot;HTTP&quot; nhẹ điều khiển cảm biến thông qua pool liên tục của backend.**Đây là đường dẫn được hỗ trợ và là đường dẫn duy nhất có trong CLI được cung cấp sẵn.** Hệ thống backend quản lý kênh truyền tải, do đó giao diện người dùng (GUI), các tập lệnh CLI và SDK đều chia sẻ chung một kết nối hoạt động thay vì cạnh tranh để sử dụng cổng nối tiếp.
- **Tất cả các trường hợp khác**(`test`, `record`, `live`, `stream`, `connect`, `info`, `net`, `ota`, `sample-rate`, `calibrate`, `serve`, `ws`, `udp`, `mqtt`, `reflectance`, `login`, `logout`, `status`) — truy cập trực tiếp vào phần cứng, được ghi chép dưới đây để đảm bảo tính đầy đủ. Các thành phần này cần gói `daq` Python, gói này**không được bao gồm trong bất kỳ bản phát hành nào**: tệp đã biên dịch CLI ex(`scripts/Build-CLI.ps1` thiết lập `--nofollow-import-to=daq`, và các giao thức truyền tải `pyserial` / `bleak` / `zeroconf` cùng với nó), và gói PyPI SDK cũng không chứa nó. Chúng chỉ chạy từ bản sao mã nguồn, vì vậy hãy coi chúng như một đường dẫn phát triển nội bộ của MAPIR thay vì một thứ để tìm kiếm.
- **`discover` / `list`** nằm giữa hai trường hợp: chúng là các lệnh điều khiển trực tiếp phần cứng từ mã nguồn, nhưng trên bản dựng đã phát hành, chúng sẽ chuyển sang sử dụng `pool-discover` và phần backend sẽ thực hiện quá trình quét. Do đó, chức năng quét hoạt động ở mọi nơi — điều này rất quan trọng vì đây là cách duy nhất để xác định địa chỉ MAC BLE của thiết bị DAQ-M.

> **`chloros-cli daq --help`** (và `-h` / `help`) liệt kê các lệnh con của `pool-*` — thông tin trợ giúp được chuyển hướng có chủ đích đến client pool để phản ánh chính xác các lệnh thực sự được thực thi. Nếu bạn gọi một lệnh con truy cập trực tiếp vào phần cứng trên bản dựng đã phát hành, chương trình sẽ thoát với thông báo lỗi cụ thể chỉ ra gói thiếu và hướng dẫn bạn quay lại `pool-*`; không có lỗi nào xảy ra mà không có thông báo. (`discover` / `list` là trường hợp ngoại lệ — chúng sẽ chuyển hướng đến `pool-discover` và hoạt động bình thường.)
>
> **Tất cả những gì khách hàng cần đều có thể truy cập thông qua `pool-*`** — kết nối, truyền dữ liệu, ghi lại các tệp `.daq` đã được hiệu chuẩn và chuyển đổi cấu hình cap. Bộ thu thập dữ liệu (DAQ) cũng có thể được điều khiển từ Python bằng `chloros_sdk.connect_daq_sensor()`, sử dụng cùng một đường dẫn chung.

### Quy trình kết nối lần đầu với cảm biến DAQ

```bash
# 1. Smart-detect any DAQ on this machine (Ethernet → BLE → USB precedence)
chloros-cli daq connect

# 2. Detailed scan: every transport, showing the address to connect with.
#    This is how you find a DAQ-M's BLE MAC — unlike a DAQ-E hostname or a
#    DAQ-U COM port, a MAC isn't printed on the device or listed by the OS.
chloros-cli daq discover                      # or: daq pool-discover
chloros-cli daq discover --only ble           # BLE only
chloros-cli daq discover --json               # machine-readable

# 3. Open a persistent pool session (handle stays alive across CLI calls)
chloros-cli daq pool-connect           # smart-detect
chloros-cli daq pool-connect --port COM3                       # DAQ-U on a specific COM port
chloros-cli daq pool-connect --mac AA:BB:CC:DD:EE:FF           # DAQ-M by BLE MAC
chloros-cli daq pool-connect --eth-host daq-e-xxx.local        # DAQ-E by hostname

# 4. List what's in the pool, including the sensor_id you'll use next
#    (DAQ-U ids look like 'CB-7C-A8-2E-5F'; DAQ-E ids like 'daq-e-def330')
chloros-cli daq pool-list

# 5. Read the latest spectrum frame
chloros-cli daq pool-latest --sensor-id CB-7C-A8-2E-5F

# 6. Record a calibrated .daq file for 60s
chloros-cli daq pool-record --sensor-id CB-7C-A8-2E-5F --duration 60 \
  -o ~/Documents/spectra --device-name "field-A"

# 7. Release
chloros-cli daq pool-disconnect --sensor-id CB-7C-A8-2E-5F
```

### Tài liệu tham khảo `pool-*`

| Lệnh con | Mục đích |
| --- | --- |
| `daq pool-connect` (smart-detect) | Mở một cảm biến trong nhóm phía sau. |
| `daq pool-connect --port PORT` | DAQ-U trên cổng nối tiếp cụ thể. |
| `daq pool-connect --ble` | DAQ-M qua BLE, quét MAC tự động. |
| `daq pool-connect --mac MAC` | DAQ-M qua BLE tại địa chỉ MAC đã biết (ngụ ý `--ble`). |
| `daq pool-connect --eth-host HOST` | DAQ-E qua Ethernet tại một máy chủ đã biết. |
| `daq pool-connect --eth` | DAQ-E qua Ethernet, máy chủ được tự động phát hiện (mDNS + dự phòng ARP; hoạt động ngay cả khi bộ nhớ đệm ARP trống trên Windows và Linux). |
| `daq pool-connect --integration-time MS --frame-avg N --no-ae` | Điều chỉnh cửa sổ tích hợp / trạng thái AE. |
| `daq pool-connect --no-stream` | Kết nối nhưng chưa bắt đầu truyền dữ liệu (tiếp tục với `pool-stream --start`). |
| `daq pool-connect --cap-id {none, fov_15, fov_30, fov_45, fov_60, fov_90, sunshine_cosine}` | Hồ sơ hiệu chỉnh giới hạn. Giá trị mặc định ở phía máy chủ là `sunshine_cosine`. |
| `daq pool-discover [--only usb,ble,eth] [--timeout SEC] [--json]` | Quét mọi kênh truyền tải để tìm các cảm biến có thể kết nối, mà không kết nối. **Đây là cách để tìm địa chỉ MAC BLE của DAQ-M.** `daq discover` / `daq list` sẽ tự động chuyển hướng đến đây trong các bản dựng đã phát hành. Các cảm biến đã được mở trong nhóm sẽ không được liệt kê — một thiết bị DAQ-M đã kết nối sẽ ngừng phát tín hiệu quảng cáo — vì vậy hãy sử dụng lệnh `pool-list` cho những thiết bị đó. |
| `daq pool-list` | Hiển thị mọi cảm biến trong nhóm cảm biến phía máy chủ. |
| `daq pool-disconnect --sensor-id ID [--all]` | Giải phóng. |
| `daq pool-latest --sensor-id ID [--recent N] [--json]` | Các khung phổ N mới nhất. |
| `daq pool-stream --sensor-id ID [--start \| --stop]` | Tiếp tục / tạm dừng truyền dữ liệu. |
| `daq pool-record --sensor-id ID [--duration SEC] [--output DIR] [--device-name NAME] [--stop]` | Bắt đầu / dừng ghi tệp .daq. |
| `daq pool-set-cap --sensor-id ID --cap-id CAP` | Đổi cấu hình hiệu chỉnh mức trần trong khi chạy. |

### Các lệnh con trực tiếp với phần cứng (chỉ có trong bản kiểm tra mã nguồn — không có trong các bản dựng đã phát hành)

> Được liệt kê để đảm bảo tính đầy đủ. Các lệnh này yêu cầu gói `daq` Python cùng với `pyserial` / `bleak` / `zeroconf`, không có lệnh nào trong số này được bao gồm trong bản dựng đã biên dịch CLI hoặc trên PyPI SDK — chúng chỉ chạy từ bản sao mã nguồn MAPIR. **Nếu bạn đang sử dụng bản dựng đã phát hành Chloros, hãy sử dụng các lệnh `pool-*` ở trên thay thế**; các lệnh này bao gồm các chức năng kết nối, truyền dữ liệu, ghi lại và chọn bộ lọc.

```bash
chloros-cli daq test --port COM3                           # Verify connection
chloros-cli daq connect --eth                              # Smart-detect over ETH
chloros-cli daq info --eth-host daq-e-xxx.local            # Device summary as JSON
chloros-cli daq discover --only usb,ble --timeout 5        # Scan local interfaces
chloros-cli daq list                                       # Alias of discover
# ^ discover/list are the exception in this section: in a shipped build they
#   fall back to `pool-discover` (the backend does the scan), so they work
#   without a source checkout. The only difference is that the fallback needs
#   the Chloros backend running, as all pool-* commands do.

# Streaming JSON Lines to stdout (pipeable)
chloros-cli daq stream --port COM3 --format jsonl --photometrics

# Record to .daq for 60 seconds
chloros-cli daq record --port COM3 --duration 60 -o ~/Documents/spectra/

# Live spectrum visualization in a window
chloros-cli daq live --port COM3 --record

# Dual-sensor reflectance (ambient + object) → JSON Lines
chloros-cli daq reflectance \
  --ambient-eth-host daq-e-field.local \
  --object-eth-host daq-e-canopy.local \
  --record -o ~/Documents/reflectance/

# Convenience: pick integration_time + frame_avg for a target rate
chloros-cli daq sample-rate --port COM3 --target-hz 5

# Calibration profile management
chloros-cli daq calibrate --port COM3 --list
chloros-cli daq calibrate --port COM3 --set field_calibration_2026_05

# DAQ-E network config (mDNS auto-discovers the host)
chloros-cli daq net --eth-host daq-e-xxx.local set-ip --mode static --ip 192.168.2.20
chloros-cli daq net --eth-host daq-e-xxx.local set-name "sky-sensor"
chloros-cli daq net --eth-host daq-e-xxx.local set-ptp --enabled true --domain 0
chloros-cli daq net --eth-host daq-e-xxx.local set-auto-stream true          # auto-stream on boot
chloros-cli daq net --eth-host daq-e-xxx.local set-require-signature         # require factory-signed cal (fw v1.6.0+; refused while the held cal is unsigned)
chloros-cli daq net --eth-host daq-e-xxx.local set-time                      # push host clock (refused when PTP SLAVE)
chloros-cli daq net --eth-host daq-e-xxx.local set-auth-token --current "" --new "s3cret"   # control-channel auth ("" new = disable)
chloros-cli daq net --eth-host daq-e-xxx.local set-ota-password "newpass"    # change OTA password (min 4 chars)
chloros-cli daq net --eth-host daq-e-xxx.local factory-reset                 # clear all NVS settings and reboot
chloros-cli daq net --eth-host daq-e-xxx.local reboot

# OTA firmware update
chloros-cli daq ota --eth-host daq-e-xxx.local \
  --firmware daq_e_1.21.bin --password mapir-daq-e

# Bridge spectra to other protocols
chloros-cli daq serve --port COM3 --tcp-port 9000           # TCP JSON-lines
chloros-cli daq ws    --port COM3 --ws-port 9001            # WebSocket
chloros-cli daq udp   --port COM3 --udp-port 9002           # UDP broadcast
chloros-cli daq mqtt  --port COM3 --broker mqtt.example.com --topic daq/spectrum
```

---

## `chloros-cli project`

Mở, kết nối và điều khiển một dự án đã lưu Chloros (một thư mục chứa `cameras.json` + `sensors.json` + `project.json`). Tất cả dữ liệu đều được xử lý qua hệ thống nền (backend) nên giao diện người dùng (GUI) và tệp ghi nhật ký (CLI) sẽ hiển thị trạng thái phần cứng giống hệt nhau.

### Tham chiếu lệnh con

| Lệnh con | Mục đích |
| --- | --- |
| `project open PATH` | In danh sách thiết bị của dự án(máy ảnh, mảng, cảm biến). |
| `project devices PATH [--reconnect]` | Liệt kê hoặc chạy lại quá trình phát hiện. |
| `project connect PATH [--cameras-only] [--sensors-only]` | Kết nối mọi máy ảnh / mảng / cảm biến đã lưu. |
| `project capture PATH NAME [-o DIR] [--format FMT] [--exposure US] [--gain DB] [--prefix P]` | Chụp một khung hình từ một camera hoặc mảng được chỉ định. |
| `project burst PATH NAME [-n N] [-i S] [-o DIR] [--format FMT] [--exposure US] [--gain DB] [--prefix P]` | Chụp liên tiếp N khung hình từ một camera hoặc mảng được chỉ định (`-n/--count` mặc định là 5; `-i/--interval` số giây giữa các khung hình, mặc định là 0). Chế độ chụp liên tiếp của mảng sẽ loại bỏ các nhóm đồng bộ lặp lại (cơ chế giám sát độ cũ) để một mảng hoạt động theo chu kỳ một phần có thểtrả lại N bản sao của một khung hình; in kết quả theo từng lần lặp. |
| `project stream PATH NAME [-n N] [--fps F] [-o DIR] [--format FMT] [--exposure US] [--gain DB] [--poll-interval S]` | Truyền luồng vào đĩa qua tác vụ backend. `--poll-interval` = số giây giữa các lần thăm dò `/stats` (mặc định 2,0). |
| `project sensor read PATH NAME [--json]` | Khung phổ mới nhất. |
| `project sensor log PATH NAME --seconds SEC [-o DIR] [--device-name NAME]` | Ghi tệp .daq. |
| `project run PATH RECIPE.yaml` | Thực thi công thức ghi dữ liệu YAML/JSON. `--dry-run` xác thực mà không cần chạy. |
| `project align calibrate PATH NAME [--method M] [--model M] [--frames N] [--reference SN] [--max-features N] [--ratio-threshold F] [--ransac-threshold-px F] [--min-matches N] [--max-reproj-err-px F] [--checkerboard RxC] [--name PROFILE]` | Tính toán căn chỉnh cho một mảng — xem [bảng cờ bên dưới](#project-align-calibrate-options). |
| `project align status PATH NAME [--json]` | Hiển thị cấu hình căn chỉnh hiện tại. |
| `project align clear PATH NAME` | Xóa cấu hình được lưu trong bộ nhớ đệm. |
| `project align tweak PATH NAME --serial SN --dx N --dy N --rotation-deg N --scale N` | Điều chỉnh biến đổi của một máy con. |
| `project align export PATH NAME --to FILE` | Lưu cấu hình vàJSON |
| `project align import PATH NAME --from FILE [--no-validate]` | Tải cấu hình đã lưu. |

#### Tùy chọn `project align calibrate`

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--method {feature_orb, feature_akaze, phase_correlation, checkerboard, manual}` | `feature_orb` | Phương pháp căn chỉnh. **Cách viết này khác với `lattice align-calibrate`**, vốn chấp nhận các dạng viết tắt `orb` / `akaze` / `phase`; hai lệnh này không thể thay thế cho nhau trên cờ này. |
| `--model {translation, rigid, affine, homography}` | `affine` | Chuyển đổi mô hình để vừa khít. |
| `--frames N` | `1` | Đồng bộ hóa các ảnh chụp khung hình với giá trị trung bình. |
| `--reference SN` | máy chủ | Số sê-ri camera tham chiếu; mọi thành viên khác đều được biến dạng để khớp với nó. |
| `--max-features N` | `5000` | Giới hạn số lượng đặc trưng ORB. |
| `--ratio-threshold F` | `0.75` | Kiểm tra tỷ lệ Lowe. |
| `--ransac-threshold-px F` | `3.0` | Ngưỡng điểm nội tại RANSAC. |
| `--min-matches N` | `15` | **Ngưỡng chất lượng** — từ chối kết quả giải nếu số điểm nội tại khớp ít hơn mức này. |
| `--max-reproj-err-px F` | `4.0` | **Ngưỡng chất lượng** — từ chối kết quả giải nếu sai số tái chiếu RMS vượt quá giá trị này. |
| `--checkerboard RxC` | — | Hình học bảng cho `--method checkerboard`, ví dụ: `9x6`. |
| `--name PROFILE` | trống | Tên cấu hình được nhúng trong tệp JSON đã lưu. **Không phải tên mảng** — đó là vị trí `NAME`. |

Hai tiêu chí chất lượng là lý do tại sao quá trình hiệu chuẩn có thể giải quyết thành công nhưng vẫn
từ chối lưu: một cấu hình không đáp ứng bất kỳ tiêu chí nào trong số đó sẽ gây ra lỗi đăng ký ngầm cho mọi
lần chụp sau đó, do đó nó bị từ chối thay vì được lưu lại.

### Ví dụ

```bash
# Open a project and see what it knows about
chloros-cli project open "/home/user/Chloros Projects/Field_A"

# Connect everything saved in the project
chloros-cli project connect "/home/user/Chloros Projects/Field_A"

# Capture from a named camera (defined in cameras.json)
chloros-cli project capture "/home/user/Chloros Projects/Field_A" FrontLeft \
  -o output/ --format tiff

# Capture from a named array
chloros-cli project capture "/home/user/Chloros Projects/Field_A" main_rig \
  -o output/ --format tiff

# Capture with overrides
chloros-cli project capture "/home/user/Chloros Projects/Field_A" main_rig \
  --exposure 5000

# Read a spectrum
chloros-cli project sensor read "/home/user/Chloros Projects/Field_A" Sky --json

# Record a DAQ log
chloros-cli project sensor log "/home/user/Chloros Projects/Field_A" Sky \
  --seconds 120 -o ~/Documents/spectra/

# Align an array (live)
chloros-cli project align calibrate "/home/user/Chloros Projects/Field_A" main_rig
chloros-cli project align status "/home/user/Chloros Projects/Field_A" main_rig

# Run a recipe
chloros-cli project run "/home/user/Chloros Projects/Field_A" recipe.yaml
```

### Ngôn ngữ kịch bản (Recipe DSL)

`project run RECIPE.yaml` chấp nhận tệp YAML hoặc JSON mô tả một chuỗi các hành động:

```yaml
# recipe.yaml
overrides:
  cameras:
    FrontLeft:
      exposure_us: 5000
      target_brightness: 80

stop_on_error: true
actions:
  - apply:
      name: FrontLeft
      settings:
        exposure_auto: "Off"
        gain: 6.0
        gain_auto: "Off"
  - wait: 2s
  - capture:
      name: FrontLeft
      output: pose_a/
      format: tiff
  - stream:
      name: main_rig
      count: 60
      fps: 5
      output: stream/
  - burst:
      name: main_rig
      count: 10
      interval: 0.5
      output: burst_a/
      format: tiff
  - sensor:
      name: Sky
      action: read
```

Các hành động được hỗ trợ: `apply`, `wait`, `capture`, `stream`, `burst`, `sensor`. Hành động `burst` yêu cầu `name` (bắt buộc), `count` (mặc định là 5), `interval` (giây, mặc định là 0), `output`, `format` và `settings` (cấu hình cài đặt cho từng camera giống như `apply`); các chuỗi chụp liên tiếp (array bursts) sử dụng cùng cơ chế giám sát nhóm đồng bộ mới (fresh-synced-group watchdog) như `project burst`.

Chạy lệnh:

```bash
chloros-cli project run "/path/to/project" recipe.yaml

# Dry-run to validate without firing hardware
chloros-cli project run "/path/to/project" recipe.yaml --dry-run
```

---

## Biến môi trường

| Biến | Tác dụng |
| --- | --- |
| `CHLOROS_BACKEND_URL` | Ghi đè lên giá trị mặc định của backend URL (mặc định là `http://127.0.0.1:5000`) — **chỉ được các nhóm lệnh `lattice`, `project` và `daq pool-*` tuân thủ.** Các lệnh (`process`, `login`, `logout`, `status`, `export-status`, `time-sync`, `selftest`) khóa `http://127.0.0.1:<port>` và bỏ qua biến này (giá trị IPv4 cố định giúp tránh được lỗi &quot;Windows&quot; `localhost`→`::1` ~2 giây phạt mỗi yêu cầu), do đó chúng luôn nhắm mục tiêu vào máy cục bộ. |
| `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED` | `1` hạ cấp việc từ chối kết nối do quá tải mảng (tổng nhu cầu trên mỗi CAM &gt; giới hạn an toàn va chạm trên đường truyền với `pin_resolution`) thành cảnh báo và tiếp tục, chấp nhận mất gói tin GVSP. Chỉ dùng cho mục đích thử nghiệm — xem [Mô hình fps &amp; burst của mảng](#array-fps--burst-model). |
| `CHLOROS_CLI_MODE` | Được thiết lập bởi chính bộ điều khiển mạng (CLI); thông báo cho hệ thống phía sau kích hoạt xử lý song song. |
| `CHLOROS_GVSP_PROBE_FALLBACK` | `0` bỏ qua quá trình kiểm tra dự phòng GVSP (chỉ hiển thị kết quả ICMP ). **Thiết lập này tắt tính năng jumbo, không chỉ đơn thuần là giảm bớt thông báo trong nhật ký** — camera chỉ trả lời các gói ping DF tối đa 1500 trên mỗi đường dẫn, do đó lệnh kiểm tra này là cách duy nhất có thể phát hiện gói jumbo. Tiết kiệm khoảng 1 giây cho mỗi camera trên mỗi lần kết nối; tiêu tốn khoảng 1,45 lần giới hạn băng thông nếu mạng *có thể* truyền được gói jumbo. SDK sẽ cảnh báo khi bạn thiết lập tùy chọn này. |
| `CHLOROS_GVSP_PACKET_SIZE_FORCE` | Khóa kích thước gói GVSP ở mức N byte; bỏ qua hoàn toàn quá trình kiểm tra. Nên sử dụng tùy chọn theo từng lệnh (`CHLOROS_GVSP_PACKET_SIZE_FORCE=9000 chloros-cli …`) thay vì thiết lập vĩnh viễn: kích thước cố định sẽ ngừng thích ứng với mạng phía trước, và việc cố định 9000 trên một đường dẫn không thể truyền gói jumbo sẽ khiến **mọi** lần ghi dữ liệu bị hết thời gian chờ với `SC_ERR_TIMEOUT -1011`. |
| `TMPDIR` (Linux) | Ghi đè thư mục trích xuất tệp đơn của Nuitka. Tùy chọn &quot;CLI&quot; sẽ tự động sử dụng `/mnt/ssd/tmp` nếu có. |

---

## Mã thoát

| Mã | Ý nghĩa |
| --- | --- |
| `0` | Thành công. |
| `1` | Lỗi chung (hầu hết các lỗi của lệnh con). |
| `2` | Lỗi tham số. |
| `130` | Bị ngắt bởi Ctrl+C. |

---

## Hướng dẫn khắc phục sự cố

- **&quot;Yêu cầu đăng nhập&quot;** → Chạy lệnh `chloros-cli login EMAIL PASSWORD` một lần trên máy này.
- **&quot;không thể kết nối với backend&quot;** → Khởi động ứng dụng desktop Chloros, hoặc chạy trực tiếp tệp nhị phân backend (`chloros-backend`), hoặc kiểm tra `CHLOROS_BACKEND_URL` nếu kết nối từ xa.
- **Các lệnh `lattice` thất bại với thông báo &quot;Không tìm thấy trình điều khiển camera LATTICE&quot;** → Môi trường chạy Arena SDK chưa được cài đặt; CLI được cung cấp kèm theo `win32api` trên Windows nhưng thư viện thời gian chạy C là một phần của trình cài đặt GUI.
- **Array connect / Array Settings hiển thị &quot;FRAMES WILL DROP&quot; hoặc &quot;Giảm ROI để kích hoạt&quot;** → Vòng nhận của NIC máy chủ quá nhỏ (thường được đặt lại thành 32 sau khi cập nhật trình điều khiển NIC). Xem [Cài đặt &amp; Tối ưu hóa NIC máy chủ](#host-nic-setup--tuning-lattice-arrays) — thiết lập `ReceiveBufferLen=256`, `PendingReceives=64`.
- **Máy bị treo khi khởi động lại/tắt máy, sau đó WMI `Invalid class` / NIC không thể kích hoạt / thiết bị USB bị thiếu** → Trình điều khiển bộ điều hợp USB 10GbE lỗi thời gây ra lỗi `DRIVER_POWER_STATE_FAILURE` (màn hình xanh `0x9F`). Cập nhật trình điều khiển bộ điều hợp — xem [Cài đặt &amp; Tối ưu hóa NIC máy chủ](#host-nic-setup--tuning-lattice-arrays).
- **Cảnh báo về vùng hoán đổi trên Jetson** → Thêm vùng swap dựa trên tệp; tệp CLI hiển thị chính xác các lệnh `fallocate` / `swapon`.
- **Thiếu các lệnh trực tiếp DAQ** → Dự kiến: gói `chloros-cli` được cung cấp sẵn đã loại trừ có chủ đích gói `daq`, do đó chỉ có `pool-*` hiện diện (SDK trên PyPI cũng không cung cấp gói này). Hãy sử dụng `pool-*`, gói này điều khiển cùng một cảm biến thông qua phần mềm nền, hoặc `chloros_sdk.connect_daq_sensor()` từ Python.

---

## Xem thêm

- [Python SDK Tài liệu tham khảo](sdk-reference.md) — tương đương lập trình của mọi lệnh CLI.
- [Hướng dẫn cảm biến DAQ](../daq/README.md) — cách đấu dây và hiệu chuẩn dành riêng cho từng cảm biến.
- Tài liệu trực tuyến: `https://mapir.gitbook.io/chloros/cli`
