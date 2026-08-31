# CLI : Dòng lệnh

> **Tài liệu tham khảo đầy đủ:**[CLI Reference](reference/cli-reference.md) ghi chép**mọi tham số của mọi lệnh con** và được tối ưu hóa cho các trợ lý AI — hãy dán liên kết URL vào trợ lý của bạn và yêu cầu một lệnh hoạt động: `https://mapir.gitbook.io/chloros/reference/cli-reference`
>
> **Mẹo cho các công cụ AI:** bất kỳ trang nào trong tài liệu hướng dẫn này đều có sẵn dưới dạng Markdown thô bằng cách thêm `.md` vào URL của trang đó (ví dụ: `https://mapir.gitbook.io/chloros/reference/cli-reference.md`), và `https://mapir.gitbook.io/chloros/llms.txt` sẽ lập chỉ mục toàn bộ tài liệu hướng dẫn để các mô hình ngôn ngữ lớn (LLM) có thể sử dụng.

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: banner shows CLI 1.1.0; reshoot the CLI welcome/banner output on the 1.2.0 build so the version line reads "Chloros CLI 1.2.0" -->


##CLI
là gì

`chloros-cli` là giao diện dòng lệnh (CLI) cho cùng một bộ xử lý mà ứng dụng máy tính để bànChloros
sử dụng. Đây là một client mỏngHTTP
chạy trên backendChloros
(một máy chủ cục bộ trên `127.0.0.1:5000`) — hầu hết các lệnh sẽ tự động khởi động backend, do đó một skript chỉ cần một lệnh gọi `chloros-cli process …` duy nhất.

Ứng dụng này chạy trên **Windows
10/11 (x64)**và**Linux
(x86_64, và NVIDIA Jetson arm64 trên JetPack 6)**, trong bất kỳ terminal nào, không cần giao diện đồ họa (GUI). Kiểm tra cài đặt của bạn bằng lệnh:

```bash
chloros-cli --version    # prints "Chloros CLI 1.2.0"
```

Tổng quan về các nhóm lệnh:

* **Xử lý &amp; tài khoản** — `process`, `login`, `logout`, `status`, `export-status`, `language` (38 ngôn ngữ — xem [Các ngôn ngữ được hỗ trợ](supported-languages.md)), `set-project-folder` / `get-project-folder` / `reset-project-folder`, `selftest`, `update` (chỉ dành choLinux
/Jetson)
* **Phần cứng trực tiếp** — `lattice` (điều khiển camera LATTICE, hơn 45 lệnh con), `daq pool-*` (cảm biến ánh sáng DAQ), `time-sync` (PTP)
* **Tự động hóa** — `project` (chạy dự ánChloros
đã lưu ở chế độ không giao diện người dùng, bao gồm các công thức ghi lại YAML)

Các tùy chọn toàn cục cần biết: `--port N` (cổng backend, mặc định là `5000`), `-v/--verbose`, `--restart` (buộc khởi động lại backend), `--backend-exe PATH`. Xem [CLI
Tài liệu tham khảo](reference/cli-reference.md) để xem danh sách đầy đủ.

***

## Cài đặt

CLI
**được tích hợp sẵn trong trình cài đặtChloros** trên mọi nền tảng — không có bản tải xuống riêng biệt tạiCLI
. Tải trình cài đặt từ trang [Tải xuống](download.md).

###Windows


Trình cài đặt sẽ đặt thư mụcCLI
tại:

```

C:\Program Files\Chloros\cli\chloros-cli.exe
```

và thêm thư mục đó vào hệ thống của bạn `PATH` — **mở một cửa sổ terminal mới**sau khi cài đặt để hệ thống nhận diện được phiên bản cập nhật `PATH`. Trình cài đặt cũng đặt các tập lệnh khởi chạy (`Chloros_CLI.bat` / `Chloros_CLI.ps1`) trong thư mục gốc cài đặt cùng với một**Chloros
CLI
** trong menu Bắt đầu, mỗi phím tắt sẽ mở một cửa sổ terminal với `chloros-cli` sẵn sàng để sử dụng.

###Linux


Cài đặt phiên bản `.deb` phù hợp với kiến trúc hệ thống của bạn:

```bash
# Linux x86_64
sudo dpkg -i chloros-amd64.deb

# NVIDIA Jetson (arm64, JetPack 6)
sudo dpkg -i chloros-arm64-jp6.deb
```

Việc này sẽ cài đặt `chloros-cli` đến `/usr/bin/chloros-cli` (hiện đã ở phiên bản `PATH`) và phần backend lên phiên bản `/usr/lib/chloros/chloros-backend`, cùng với môi trường chạy ArenaSDK
cần thiết cho camera LATTICE. Xem [Linux
Cài đặt](linux/linux-installation.md) để biết chi tiết.

### Xác minh

```bash
chloros-cli --version    # "Chloros CLI 1.2.0"
chloros-cli selftest     # 7-step diagnostic: backend, API, GPU/CUDA, denoiser models
chloros-cli status       # license tier + logged-in user
```

***

## Đăng nhập &amp; Giấy phép

CLI
(vàPython
SDK
) yêu cầu **gói trả phíChloros
+**— bất kỳ gói trả phí nào cũng có tính năng này; gói miễn phí thì không. Giới hạn này được áp dụng**từ phía máy chủ** bởi hệ thống backend, không phải bởi tệp nhị phânCLI
: một cuộc gọi khi chưa đăng nhập sẽ bị từ chối với mã lỗi `401 AUTH_REQUIRED`, và yêu cầu từ tài khoản đã đăng nhập trên gói miễn phí sẽ bị từ chối với mã lỗi `403 PLAN_UPGRADE_REQUIRED`, bất kể yêu cầu đó đến từ `chloros-cli`,SDK
hay một clientHTTP
tự phát triển. Nâng cấp tại [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing).

Đăng nhập **một lần trên mỗi máy**:

```bash
chloros-cli login user@example.com 'YourPassword'
chloros-cli status
```

<figure><img src=".gitbook/assets/cli login_w.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: login success output predates 1.2.0; reshoot `chloros-cli login` followed by `chloros-cli status` on the 1.2.0 build showing the license tier line -->


{% hint style="warning" %}
**Mật khẩu có ký tự đặc biệt**(`$`, `!`, spaces): wrap the password in**single quotes**, as shown above. In PowerShell double quotes, `$$` bị lỗi do vỏ lệnh (CLI
) phát hiện lỗi này khi gặp mã trạng thái 401 và tự động thử lại, nhưng việc sử dụng dấu nháy đơn sẽ tránh được vấn đề này hoàn toàn).
{% endhint %}

Phiên làm việc được lưu trong bộ nhớ đệm tại `~/.chloros/user_session.json` và vẫn hoạt động ngoại tuyến trong thời gian ân hạn của gói cước (30 ngày đối với gói hàng tháng, đến khi hết hạn đối với gói hàng năm). `chloros-cli status` vẫn hoạt động ngay cả khi không có gói trả phí, do đó lý do từ chối luôn hiển thị.

{% hint style="danger" %}
**Đang lên lịch công việc không cần giao diện người dùng? Vui lòng đăng nhập trước.**Các lệnh khởi tạo nền (`process`, `status`, `export-status`, …) được chạy**không có phiên làm việc được lưu trong bộ nhớ đệm**sẽ không báo lỗi ngay lập tức — nó sẽ chuyển sang chế độ nhắc lệnh tương tác `Email:` / `Password:` trên stdin. Do đó, một tác vụ cron hoặc bước CI chạy tự động sẽ**bị treo khi chờ nhập liệu**. Hãy chạy `chloros-cli login EMAIL 'PASSWORD'` một lần trên máy trước khi lên lịch bất kỳ tác vụ nào.
{% endhint %}

***

## Lần Chạy Xử Lý Đầu Tiên

Chỉ định `process` vào một thư mục chứa các tệp ghi lại — nó sẽ tự động phát hiệnSurvey3
(`.raw` + `.jpg`), LATTICE (`.tif`/`.tiff`), `.dng`, hoặc kết hợp cả hai:

```bash
chloros-cli process "C:\Images\flight_001"          # Windows
chloros-cli process ~/images/flight_001              # Linux
```

Dòng tiến trình được hiển thị trực tiếp theo từng luồng xử lý (Phát hiện, Phân tích, Xử lý, Xuất), và một lần chạy thành công kết thúc bằng việc báo cáo số lượng sản phẩm hình ảnh đã được ghi (`Image products written: N`).



<!-- SCREENSHOT-NEEDED: terminal capture of a `chloros-cli process` run on a LATTICE captures folder completing successfully — per-thread progress lines visible and the final "Image products written: N" summary line -->
### Vị trí lưu trữ đầu ra

`process` ghi vào một **thư mục dự án**, không phải vào thư mục đầu vào của bạn:

* Nếu không có `-o`: dự án sẽ được tạo trong thư mục dự án mặc định của bạn (được chia sẻ với giao diện người dùng; quản lý nó bằng `get-project-folder` / `set-project-folder`, tùy chọn dự phòng `~/Chloros Projects`), được đặt tên theo `-n/--project-name` hoặc dấu thời gian (`YYYYMMDD_HHMMSS`) khi không chỉ định.
* Với `-o PATH`: thư mục đó **chính là** thư mục dự án. Nếu thư mục đó đã chứa một tệp `project.json`, một tệp có hậu tố `_1`/`_2`… sẽ được tạo ra thay vì ghi đè lên tệp cũ.

Bên trong dự án, các sản phẩm được nhóm **theo máy ảnh, sau đó theo định dạng tệp**:

```
<project>/
├── project.json
├── calibration_data.json
└── LATT-M3M-L41-F550/                  # one folder per camera model+lens+filter
    ├── tiff16/
    │   ├── Reflectance_Calibrated_Images/
    │   ├── Debayered_Images/
    │   ├── Preview_Images/
    │   └── NDVI_Index_Images/           # one folder per requested index
    └── tiff32/
        └── Radiance_Images/             # float32 radiance always lands here
```

Thư mục máy ảnh là `LATT-<sensor>-<lens>-F<filter>` dành cho LATTICE (phù hợp với thông tin EXIF của ảnh chụp `Model`) và `<model>_<filter>` (ví dụ: `Survey3N_RGN`) dành choSurvey3
. Thư mục định dạng tuân theo cấu trúc `--format`: `tiff16`, `tiff8`, `png8`, `jpg8` hoặc `tiff32` cho `TIFF (32-bit, Percent)`.

{% hint style="info" %}
**Mọi sản phẩm được xuất ra đều giữ nguyên tên tệp NGUỒN.**Một tệp xuất radiance từ `capture_..._raw.tif` vẫn được đặt tên là `capture_..._raw.tif` — chỉ là nó nằm trong thư mục `tiff32/Radiance_Images/`.**Thư mục xác định sản phẩm, không phải tên tệp**, vì vậy hãy sử dụng cú pháp glob cho thư mục, không phải cho phần mở rộng `*radiance*`.
{% endhint %}

### Các tùy chọn bạn sẽ thực sự sử dụng

| Cờ | Mặc định | Chức năng |
| --- | --- | --- |
| `-o, --output PATH` | thư mục dự án mặc định | Vị trí thư mục dự án (xem ở trên). |
| `-n, --project-name NAME` | dấu thời gian | Tên dự án. |
| `--format FMT` | `TIFF (16-bit)` | Một trong các giá trị sau: `TIFF (16-bit)`, `TIFF (32-bit, Percent)`, `PNG (8-bit)`, `JPG (8-bit)`. |
| `--indices NAME [NAME ...]` | không có | Các chỉ số thảm thực vật cần xuất (xem [Chỉ số thảm thực vật](#vegetation-indices)). |
| `--debayer {standard,texture-aware}` | `standard` | `texture-aware` = thuật toán khử hiệu ứng Bayer bằng mạng nơ-ron, chậm hơn, chất lượng cao nhất (hệ điều hànhChloros
trở lên, GPU NVIDIA). |
| `--vignette / --no-vignette` | bật | Chỉnh sửa hiệu ứng tối góc. |
| `--reflectance / --no-reflectance` | bật | Hiệu chỉnh độ phản xạ; đối với LATTICE, đây cũng là công tắc bật/tắt sản phẩm độ phản xạ. |
| `--input-level {auto,raw,debayered,processed}` | `auto` | Buộc điểm vào của chuỗi xử lý cho các tệp TIFF của LATTICE. |

Đối với các tùy chọn khác — tinh chỉnh phát hiện mục tiêu, PPK, điểm phơi sáng, cờ căn chỉnh mảng — hãy tham khảo [phần `process` trong Tài liệu tham khảoCLI
](reference/cli-reference.md).

***

## Chọn nội dung cần xuất (Sản phẩm LATTICE)

Quy trình xử lý LATTICE được phân tán sang **tất cả các sản phẩm tương ứng trong một lần chạy**. Bốn công tắc cho từng sản phẩm đều**được bật theo mặc định**; sử dụng biểu mẫu `--no-` để tắt một công tắc:

| Công tắc | Sản phẩm |
| --- | --- |
| `--debayered` | Demosaic tuyến tính → `Debayered_Images/` |
| `--preview` | Xem trước hiển thị (cân bằng trắng + gamma; kéo dãn màu giả cho đa phổ) → `Preview_Images/` |
| `--radiance` | Độ sáng (radiance) dạng float32, W/m²/sr/nm → `Radiance_Images/` (luôn là `tiff32/`) |
| `--reflectance` | uint16 độ phản xạ, tương thích với Pix4D → `Reflectance_Calibrated_Images/` |

RGB
các camera chính chỉ phát ra dữ liệu đã loại bỏ hiệu ứng Bayer + bản xem trước — độ rạng rỡ/độ phản xạ theo từng dải tần không có ý nghĩa đối với cảm biến băng thông rộng, do đó các tùy chọn này không có tác dụng đối với chúng.Survey3
`.raw` bỏ qua các tùy chọn này và tuân theo đường dẫn phản xạ/mục tiêu tiêu chuẩn.

```bash
# Radiance only — no DAQ downwelling needed
chloros-cli process ~/captures/lattice_flight --no-debayered --no-preview --no-reflectance
```

**`--reflectance-source {auto,target,daq}`** (mặc định là `auto`) chọn tham chiếu độ phản xạ: `auto` tạo một [mục tiêu hiệu chuẩn](calibration-targets.md) trong khung hình đạt tiêu chuẩn QA làm tham chiếu tuyệt đối và chuyển về giá trị chia theo hướng xuống của cảm biến ánh sáng DAQ (ρ = π·L/E) khi không có mục tiêu; `target` áp dụng nghiêm ngặt (không thay thế bằng dữ liệu DAQ); `daq` là có thẩm quyền theo DAQ. Các lần quét mục tiêu được đo theo đơn vị có thể được cung cấp bằng `--target-reflectance-dir`.

{% hint style="info" %}
**Đọc các pixel phản xạ:**giá trị DN tương ứng với ρ = 1,0 là**theo từng nguồn** — Các tệp LATTICE đóng dấu `Chloros:PixelScale=32768` trong XMP; các tệpSurvey3
sử dụng giá trị 65535 (và không chứa thẻ `Chloros:*`). Hãy đọc thẻ và chia cho giá trị đó thay vì giả định một hằng số. Chi tiết và trường hợp ngoại lệ duy nhất không có tỷ lệ được đề cập trong [CLI
Tài liệu tham khảo](reference/cli-reference.md).
{% endhint %}

**Quá trình xử lý luôn bắt đầu từ `raw`.** Các sản phẩm phái sinh (dữ liệu xuất sau khi loại bỏ hiệu ứng debayering/độ rạng rỡ/độ phản xạ) không bao giờ được đưa trở lại qua đường ống xử lý — việc nhập lại và xử lý chúng sẽ khiến các phép tính hiệu chuẩn được áp dụng hai lần, do đóChloros
sẽ bỏ qua chúng và thông báo điều này. `--input-level` là lối thoát có chủ ý khi bạn thực sự cần ép buộc một điểm vào.

***

## Khi một lần chạy thất bại

Kể từ phiên bản 1.2.0, `process` sẽ báo lỗi rõ ràng thay vì “thành công” mà không hiển thị kết quả nào:

* Một lần chạy **yêu cầu các sản phẩm nhưng không ghi lại sản phẩm nào**— chỉ có `project.json` và `calibration_data.json` — sẽ hiển thị thông báo `Processing finished but wrote no image products.` và**thoát với giá trị không bằng 0**, do đó các skript có thể phát hiện được. Các nguyên nhân thường gặp: thư mục đầu vào không được nhận diện là vùng ghi hình (kiểm tra bố cục và `--input-level`), hoặc mọi sản phẩm được yêu cầu đều không áp dụng được cho các camera đó (ví dụ: yêu cầu độ sáng/độ phản xạ từ các camera chỉ hỗ trợ chế độ &quot;RGB
&quot;).
* Một **lần chạy chỉ lấy metadata có chủ đích** (tắt tất cả các sản phẩm, không có `--indices`) vẫn được coi là thành công — kết quả đầu ra là hình ảnh trống là kết quả chính xác trong trường hợp này.
* Chạy lại với `--verbose` và kiểm tra nhật ký hệ thống để tìm các dòng liên quan đến `[LATTICE-EXPORT]` / `[EXPORT-CHECK]`, những dòng này giải thích lý do bỏ qua từng camera cụ thể.

Mã thoát: `0` thành công · `1` lỗi chung · `2` lỗi tham số · `130` bị ngắt bởi Ctrl+C.

***

## Chỉ số thảm thực vật

Chạy `--indices` với một hoặc nhiều tên cài đặt sẵn; mỗi chỉ số sẽ được lưu trong thư mục `<INDEX>_Index_Images/` riêng:

```bash
chloros-cli process ~/images/flight_001 --indices NDVI NDRE GNDVI
```

22 tên cài đặt sẵn mà `process --indices` chấp nhận:

`NDVI` `GNDVI` `NDRE` `OSAVI` `SAVI` `MSAVI2` `EVI` `MSR` `TDVI` `LAI` `GCI` `GRVI` `GSAVI` `GOSAVI` `NLI` `MNLI` `RDVI` `WDRVI` `CVI` `ENDVI` `GLI` `VARI`

{% hint style="warning" %}
**Có ba danh sách chỉ mục — đừng nhầm lẫn chúng.**Menu thả xuống “Project Settings” (Cài đặt Dự án) trên giao diện người dùng (GUI) có 27 công thức (bao gồm `FCI1`, `FCI2`, `GARI`, `GEMI`, `LCI` — năm công thức này chỉ dành riêng cho giao diện người dùng (GUI) và**không** áp dụng cho `--indices`). Lệnh trực tiếp/ngoại tuyến `lattice index --preset` sử dụng danh sách 22 cài đặt sẵn riêng biệt. Các công thức và phép toán dải phổ được ghi chép trong [Công thức chỉ số đa phổ](project-settings/multispectral-index-formulas.md).
{% endhint %}

***

## Cảm biến ánh sáng DAQ: Giới thiệu nhanh

Dòng sản phẩm `daq pool-*` điều khiển các cảm biến phổ DAQ (MAPIR
) (DAQ-U qua USB, DAQ-M qua BLE, DAQ-E qua Ethernet) thông qua bể lưu trữ liên tục của hệ thống nền — giao diện người dùng (GUI),CLI
vàSDK
đều chia sẻ chung một đối tượng kết nối trực tiếp. **`pool-*` là đường dẫn DAQ được hỗ trợ trong bản phát hànhCLI
**; các lệnh con `daq` khác mà bạn có thể thấy được đề cập làMAPIR
— một bề mặt chỉ dành cho nguồn nội bộ — và sẽ thoát với một lỗi rõ ràng chỉ dẫn bạn đến `pool-*`.

```bash
# 1. Open a pooled session (pick the line matching your sensor)
chloros-cli daq pool-connect                              # smart-detect
chloros-cli daq pool-connect --port COM3                  # DAQ-U on a specific COM port
chloros-cli daq pool-connect --mac AA:BB:CC:DD:EE:FF      # DAQ-M by BLE MAC
chloros-cli daq pool-connect --eth-host daq-e-xxx.local   # DAQ-E by hostname (reliable)

# 2. List pooled sensors and their ids
#    (DAQ-U ids look like 'CB-7C-A8-2E-5F'; DAQ-E ids like 'daq-e-def330')
chloros-cli daq pool-list

# 3. Read the latest calibrated spectrum (W/m²/nm)
chloros-cli daq pool-latest --sensor-id CB-7C-A8-2E-5F

# 4. Record a calibrated .daq file for 60 s
chloros-cli daq pool-record --sensor-id CB-7C-A8-2E-5F --duration 60 \
  -o ~/Documents/spectra --device-name "field-A"

# 5. Release
chloros-cli daq pool-disconnect --sensor-id CB-7C-A8-2E-5F
```

`pool-record` mà không có `--duration` sẽ chạy cho đến khi gặp `pool-record --stop`; thư mục đầu ra mặc định là `~/Documents/DAQ Live View/` **trên máy của backend**. Hồ sơ hiệu chỉnh giới hạn được chọn tại thời điểm kết nối (`--cap-id`, mặc định của máy chủ là `sunshine_cosine`) và có thể được thay đổi trực tiếp bằng `pool-set-cap` — các cấu hình cap và phạm vi hiệu chuẩn của cảm biến được đề cập trong các chương về DAQ của tài liệu hướng dẫn này.

{% hint style="warning" %}
**DAQ-E trên máy chủ có nhiều NIC:** quá trình tự động phát hiện `pool-connect --eth` đầu tiên sau khi khởi động có thể thất bại ngay cả khi cảm biến hoạt động bình thường. `--eth-host <ip-or-hostname>` là phương án đáng tin cậy — hãy sử dụng nó mỗi khi quá trình phát hiện không thành công.
{% endhint %}

***

## Camera LATTICE, PTP &amp; Tự động hóa dự án

Dòng lệnh `lattice` (hơn 45 lệnh con) bao quát toàn bộ quy trình làm việc với máy ảnh LATTICE: phát hiện, chụp ảnh đơn lẻ, mảng đồng bộ liên tục thông qua quy trình kết nối &quot;smart-prep&quot; của giao diện người dùng (GUI), xem trước trực tiếp trên trình duyệt, căn chỉnh, tính toán chỉ số và chẩn đoán NIC của máy chủ. Một số ví dụ:

```bash
chloros-cli lattice info                                          # discover cameras
chloros-cli lattice capture -o output/                            # one frame, all export types
chloros-cli lattice array-connect --serials SN1,SN2,SN3,SN4       # persistent synced array
chloros-cli lattice array-capture --processing reflectance -o out/
```

Bên cạnh đó: `chloros-cli time-sync` báo cáo về máy chủ PTP chính (grandmaster) mà máy chủChloros
đang chạy (các camera LATTICE và cảm biến DAQ-E hoạt động như thiết bị phụ thuộc vào nó để đồng bộ dấu thời gian giữa các thiết bị), và `chloros-cli project` mở một dự án đã lưu từChloros
và điều khiển các camera, mảng và cảm biến của nó mà không cần giao diện đồ họa — bao gồm cả các kịch bản thu thập dữ liệu YAML được lập trình sẵn.

Ba dòng sản phẩm này (`lattice`, `project`, `daq pool-*`) cũng là những dòng duy nhất hỗ trợ `CHLOROS_BACKEND_URL` để điều khiển một backend **từ xa**; các lệnh cốt lõi luôn nhắm đến máy cục bộ.

Các hướng dẫn chi tiết được trình bày trong các chương về LATTICE của tài liệu này; mọi tham số đều có trong [CLI
Tài liệu tham khảo](reference/cli-reference.md).

***

## Khắc phục sự cố: 5 vấn đề hàng đầu

| Triệu chứng | Cách khắc phục |
| --- | --- |
| `Login required` hoặc một tác vụ được lên lịch bị treo tại dấu nhắc `Email:` | Chạy `chloros-cli login EMAIL 'PASSWORD'` một lần trên máy này — các lệnh không có phiên được lưu trong bộ nhớ đệm sẽ tương tác thay vì báo lỗi ngay lập tức. |
| `backend unreachable` | Khởi động ứng dụng máy tính để bànChloros
hoặc chạy trực tiếp tệp nhị phân phía máy chủ (`chloros-backend`). Nếu bạn chỉ định `lattice`/`project`/`daq pool-*` đến một backend từ xa, hãy kiểm tra `CHLOROS_BACKEND_URL`. |
| Kết nối mảng bị chặn: `FRAMES WILL DROP` / `Reduce ROI to enable` | Vòng nhận của NIC máy chủ được đặt lại về mặc định — nguyên nhân hàng đầu khiến một hệ thống từng hoạt động bình thường từ chối kết nối, thường xảy ra sau khi cập nhật trình điều khiển NIC. Chạy lệnh `chloros-cli lattice network --fix` từ một cửa sổ terminal **có quyền quản trị** (hoặc thiết lập các tham số `ReceiveBufferLen=256`, `PendingReceives=64`); xem phần *Cài đặt và Tối ưu hóa NIC máy chủ* trong tài liệu tham khảo. |
| Lệnh con `daq` báo lỗi: &quot;yêu cầu gói DAQ đầy đủ…&quot; | Điều này là bình thường trên các bản dựng được phân phối sẵn — phiên bản biên dịch sẵnCLI
chỉ bao gồm dòng lệnh `daq pool-*`, bao quát các chức năng kết nối, truyền dữ liệu, ghi dữ liệu và chọn tệp CAP. Sử dụng `pool-*` (hoặc `chloros_sdk.connect_daq_sensor()` từPython
). |
| Jetson hiển thị cảnh báo về vùng nhớ ảo trước khi xử lý các thư mục lớn | Thêm vùng nhớ ảo dựa trên tệp — tệpCLI
in ra chính xác các lệnh `fallocate`/`swapon` cần chạy. |

***

## Nhận trợ giúp

```bash
chloros-cli --help              # top-level help
chloros-cli process --help      # per-command help
chloros-cli lattice --help
chloros-cli daq --help          # lists the pool-* subcommands
```

* **Mọi tham số, mọi lệnh con:** [CLI
Tài liệu tham khảo](reference/cli-reference.md)
* **Tương đươngPython
:** [Python
SDK
](api-python-sdk.md) và [SDK
Tài liệu tham khảo](reference/sdk-reference.md)
* **Hỗ trợ:** info@mapir.camera · [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
