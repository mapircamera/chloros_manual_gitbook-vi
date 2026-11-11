# CLI: Dòng lệnh

> **Tài liệu tham khảo đầy đủ:**[Tài liệu tham khảo CLI](reference/cli-reference.md) ghi lại**tất cả các tham số của mọi lệnh con** và được tối ưu hóa cho các trợ lý AI — hãy dán URL của tài liệu này vào trợ lý của bạn và yêu cầu một lệnh hoạt động: `https://mapir.gitbook.io/chloros/reference/cli-reference`
>
> **Mẹo cho các công cụ AI:** bất kỳ trang nào trong tài liệu hướng dẫn này đều có sẵn dưới dạng Markdown thô bằng cách thêm `.md` vào URL của trang đó (ví dụ: `https://mapir.gitbook.io/chloros/reference/cli-reference.md`), và `https://mapir.gitbook.io/chloros/llms.txt` lập chỉ mục toàn bộ tài liệu hướng dẫn để các mô hình ngôn ngữ lớn (LLM) sử dụng.

> **Bạn muốn trợ lý của mình tự chạy Chloros?** `chloros-mcp`, một máy chủ MCP được cài đặt cùng với Python SDK, hoạt động với Claude Desktop, Claude Code, Cursor và các ứng dụng khách MCP khác: xem [Sử dụng Chloros với các trợ lý AI](ai-assistants.md).

<figure><img src=".gitbook/assets/cli.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: banner shows CLI 1.1.0; reshoot the CLI welcome/banner output on the 1.3.0 build so the version line reads "Chloros CLI 1.3.0" -->


## CLI
là gì

`chloros-cli` là giao diện dòng lệnh cho cùng một công cụ xử lý mà ứng dụng máy tính để bàn Chloros
sử dụng. Đây là một ứng dụng khách HTTP
mỏng trên nền tảng Chloros
(một máy chủ cục bộ trên `127.0.0.1:5000`) — hầu hết các lệnh đều tự động khởi động nền tảng, do đó, một lệnh `chloros-cli process …` duy nhất là tất cả những gì một tập lệnh cần. Sau đó, máy chủ phía sau sẽ tiếp tục chạy cho lệnh tiếp theo: nó sẽ dừng lại khi nhận lệnh `chloros-cli logout` (trừ khi ứng dụng đang mở hoặc đang thực thi hoặc đang xây dựng), khi nhận lệnh `chloros-cli backend stop`, hoặc tự động dừng sau 30 phút nếu không có client, tác vụ đang chạy hoặc thiết bị kết nối. `chloros-cli backend status` hiển thị điều này.

Nó chạy trên **Windows
10/11 (x64)**và**Linux
(x86_64, và NVIDIA Jetson arm64 trên JetPack 6)**, trong bất kỳ terminal nào, không cần giao diện đồ họa (GUI). Kiểm tra cài đặt của bạn bằng:

```bash
chloros-cli --version    # prints "Chloros CLI 1.3.0"
```

Tổng quan về các nhóm lệnh:

* **Xử lý &amp; tài khoản** — `process`, `login`, `logout`, `status`, `export-status`, `language` (38 ngôn ngữ — xem [Các ngôn ngữ được hỗ trợ](supported-languages.md)), `set-project-folder` / `get-project-folder` / `reset-project-folder`, `selftest`, `update` (Chỉ dành cho Linux
/Jetson)
* **Phần cứng hoạt động** — `lattice` (điều khiển camera LATTICE, hơn 45 lệnh con), `daq pool-*` (cảm biến ánh sáng DAQ), `time-sync` (PTP)
* **Tự động hóa** — `project` (chạy dự án Chloros
đã lưu ở chế độ không giao diện người dùng, bao gồm các công thức ghi lại YAML)

Các tùy chọn toàn cục cần biết: `--port N` (cổng backend, mặc định là `5000`), `-v/--verbose`, `--restart` (buộc khởi động lại backend), `--backend-exe PATH`. Xem [Tham chiếu CLI
](reference/cli-reference.md) để xem danh sách đầy đủ.

***

## Cài đặt

CLI
**được tích hợp sẵn trong trình cài đặt Chloros** trên mọi nền tảng — không có bản tải xuống CLI
riêng biệt. Tải trình cài đặt từ trang [Tải xuống](download.md).

### Windows


Trình cài đặt sẽ đặt CLI
tại:

```

C:\Program Files\Chloros\cli\chloros-cli.exe
```

và thêm thư mục đó vào hệ thống của bạn `PATH` một lần (việc nâng cấp cũng sẽ xóa các mục trùng lặp do các trình cài đặt trước đó để lại) — **mở một cửa sổ terminal mới**sau khi cài đặt để hệ thống nhận diện được phiên bản cập nhật `PATH`. Trình cài đặt cũng đặt các tập lệnh khởi chạy (`Chloros_CLI.bat` / `Chloros_CLI.ps1`) trong thư mục gốc cài đặt cùng với một**Chloros
CLI
** trong menu Bắt đầu, mỗi phím tắt sẽ mở một cửa sổ terminal với `chloros-cli` sẵn sàng sử dụng.

### Linux


Cài đặt phiên bản `.deb` phù hợp với kiến trúc hệ thống của bạn bằng lệnh `apt` (không phải `dpkg -i`, vì phiên bản này bỏ qua các yêu cầu được khuyến nghị của Python
SDK
):

```bash
# Linux x86_64
sudo apt install ./chloros_1.3.0_amd64.deb

# NVIDIA Jetson (arm64, JetPack 6)
sudo apt install ./chloros_1.3.0_arm64_jp6.deb
```

Việc này sẽ cài đặt `chloros-cli` đến `/usr/bin/chloros-cli` (đã có sẵn trong `PATH`), các chương trình backend và giao diện dòng lệnh (CLI
) được giải nén trong `/usr/lib/chloros/backend/` và `/usr/lib/chloros/cli/` (`/usr/lib/chloros/chloros-backend` là trình khởi chạy để khởi động phần nền), môi trường chạy Arena SDK
mà phần nền cần cho camera LATTICE, và Python
SDK
cho hệ thống `python3`. Xem [Cài đặt trên Linux
](linux/linux-installation.md) để biết chi tiết.

### Xác minh

```bash
chloros-cli --version    # "Chloros CLI 1.3.0"
chloros-cli selftest     # 7-step diagnostic: backend, API, GPU/CUDA, denoiser models
chloros-cli status       # license tier + logged-in user
```

***

## Đăng nhập &amp; Giấy phép

Việc truy cập CLI
(và Python
SDK
) yêu cầu **gói Chloros
+ trả phí**— bất kỳ gói trả phí nào cũng có quyền truy cập này; gói miễn phí thì không. Giới hạn này được áp dụng**từ phía máy chủ** bởi phần nền, không phải bởi tệp nhị phân CLI
: một yêu cầu khi chưa đăng nhập sẽ bị từ chối với mã lỗi `401 AUTH_REQUIRED`, và yêu cầu đã đăng nhập trên gói miễn phí sẽ bị từ chối với mã lỗi `403 PLAN_UPGRADE_REQUIRED`, bất kể yêu cầu đó đến từ `chloros-cli`, SDK
hay một client HTTP
tự phát triển. Nâng cấp tại [https://cloud.mapir.camera/pricing](https://cloud.mapir.camera/pricing).

Đăng nhập **một lần trên mỗi máy**:

```bash
chloros-cli login user@example.com 'YourPassword'
chloros-cli status
```

<figure><img src=".gitbook/assets/cli login_w.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: login success output predates 1.2.0; reshoot `chloros-cli login` followed by `chloros-cli status` on the 1.3.0 build showing the license tier line -->


{% hint style="warning" %}
**Mật khẩu chứa ký tự đặc biệt**(`$`, `!`, spaces): wrap the password in**single quotes**, as shown above. In PowerShell double quotes, `$$` bị biến dạng bởi vỏ lệnh (CLI) (CLI
phát hiện điều này khi gặp lỗi 401 và tự động thử lại, nhưng việc sử dụng dấu nháy đơn sẽ tránh hoàn toàn vấn đề này).
{% endhint %}

Phiên làm việc được lưu trong bộ nhớ đệm tại `~/.chloros/user_session.json` và tiếp tục hoạt động ngoại tuyến trong thời gian ân hạn của gói dịch vụ (30 ngày đối với gói hàng tháng, đến khi hết hạn đối với gói hàng năm). `chloros-cli status` vẫn hoạt động ngay cả khi không có gói trả phí, do đó lý do từ chối luôn hiển thị.

{% hint style="danger" %}
**Đang lên lịch công việc không giao diện người dùng? Vui lòng đăng nhập trước.**Các lệnh khởi tạo tiến trình nền (`process`, `status`, `export-status`, …) được chạy**không có phiên làm việc được lưu trong bộ nhớ đệm**sẽ không báo lỗi ngay lập tức — nó sẽ chuyển sang chế độ nhắc lệnh tương tác `Email:` / `Password:` trên stdin. Do đó, một tác vụ cron tự động hoặc bước CI sẽ**bị treo khi chờ nhập liệu**. Hãy chạy lệnh `chloros-cli login EMAIL 'PASSWORD'` một lần trên máy trước khi lên lịch bất kỳ tác vụ nào.
{% endhint %}

***

## Lần Chạy Xử Lý Đầu Tiên

Chỉ định `process` vào một thư mục chứa dữ liệu thu thập — nó sẽ tự động phát hiện Survey3
(`.raw` + `.jpg`), LATTICE (`.tif`/`.tiff`), `.dng`, hoặc kết hợp cả hai:

```bash
chloros-cli process "C:\Images\flight_001"          # Windows
chloros-cli process ~/images/flight_001              # Linux
```

Dòng tiến trình được truyền trực tiếp theo từng luồng xử lý (Phát hiện, Phân tích, Xử lý, Xuất), và một lần chạy thành công kết thúc bằng việc báo cáo số lượng sản phẩm hình ảnh đã được ghi: `Image products written: N`, tiếp theo là `(plus M clamp masks)` khi quá trình chạy cũng ghi [mặt nạ kẹp](output-image-formats.md#clamp-masks) (chúng không được tính là sản phẩm).

<!-- SCREENSHOT-NEEDED: terminal capture of a `chloros-cli process` run on a LATTICE captures folder completing successfully — per-thread progress lines visible and the final "Image products written: N (plus M clamp masks)" summary line -->


Nếu backend Chloros
đã đang chạy (ví dụ: của ứng dụng máy tính để bàn), `process` sẽ sử dụng nó và mở dự án của mình trong đó. Nó sẽ từ chối nếu ứng dụng đang mở một dự án mà ứng dụng đã mở: hãy xử lý từ ứng dụng hoặc đóng dự án đó. Nếu một lần chạy trước đó vẫn đang hoàn tất, nó sẽ đợi tối đa 15 giây trước khi từ chối. `--restart` buộc phải khởi động một backend mới. Trên Linux
, một backend được chạy dưới dạng dịch vụ systemd sẽ không bao giờ được khởi động lại: `process` sử dụng nó nguyên trạng, và trong trường hợp bình thường nó sẽ khởi động lại (`--restart`, hoặc một lệnh import không tìm thấy gì), nó sẽ hiển thị thông báo `sudo systemctl restart <unit>` và thoát khỏi chương trình `1`.

### Vị trí lưu trữ kết quả

`process` ghi kết quả vào **thư mục dự án**, không phải vào thư mục đầu vào của bạn:

* Nếu không có `-o`: dự án sẽ được tạo trong thư mục dự án mặc định của bạn (được chia sẻ với giao diện người dùng; quản lý nó bằng `get-project-folder` / `set-project-folder`; khi không được thiết lập, `Documents/MAPIR/Chloros Projects` trong thư mục người dùng của bạn), được đặt tên theo `-n/--project-name` hoặc dấu thời gian (`YYYYMMDD_HHMMSS`) khi không được chỉ định.
* Với `-o PATH`: thư mục đó **chính là** thư mục dự án. Nếu thư mục đó đã chứa một tệp `project.json`, thay vì ghi đè, hệ thống sẽ tạo một tệp anh em có hậu tố là `_1`/`_2`… thay vì ghi đè lên tệp hiện có. Đường dẫn tương đối `PATH` được xác định dựa trên thư mục mà bạn chạy `chloros-cli` từ đó.

{% hint style="info" %}
**Đường dẫn tương đối (đã thay đổi trong phiên bản 1.3.0).** Mọi đường dẫn mà phần nền mở ra — `-o`/`--output` khi thực thi bất kỳ lệnh nào (bao gồm lệnh mặc định `output` của các lệnh ghi lại), `--input`, `--burst-dir`, `--profile`, `--save` và `--mask` — sẽ được giải quyết dựa trên thư mục mà bạn chạy lệnh `chloros-cli` từ đó, do đó lệnh `-o captures/` sẽ ghi vào thư mục `<current folder>/captures/`. Trong phiên bản 1.2.0, backend giải quyết các đường dẫn này trong thư mục cài đặt riêng của nó, điều này dẫn đến lỗi &quot;Quyền truy cập bị từ chối&quot;. Đối với một backend từ xa (`CHLOROS_BACKEND_URL`), các đường dẫn được gửi nguyên văn và được giải quyết trên máy đó.
{% endhint %}

{% hint style="warning" %}
**Windows
: hãy giữ thư mục dự án ngắn trừ khi tính năng đường dẫn dài được bật.** Khi tính năng đường dẫn dài của Windows
bị tắt (mặc định của Windows
), không đường dẫn tệp nào được dài hơn 260 ký tự, và các sản phẩm nằm cách thư mục dự án tối đa khoảng 130 ký tự. `process` kiểm tra điều này trước khi bắt đầu: thư mục dự án quá sâu sẽ bị từ chối với thông báo **&quot;Thư mục dự án quá sâu đối với máy tính này&quot;**, thư mục dự án dài nhất phù hợp, và cách bật đường dẫn dài — không có thao tác nào được thực hiện. Hãy sử dụng phiên bản `-o` (hoặc `set-project-folder`) ngắn hơn, hoặc bật tính năng đường dẫn dài của Windows
(tùy chọn **Bật đường dẫn dài của Windows** trong trình cài đặt, được chọn mặc định) và khởi động lại Chloros
.
{% endhint %}

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

Thư mục máy ảnh là `LATT-<sensor>-<lens>-F<filter>` cho LATTICE (trùng khớp với trường EXIF của ảnh chụp là `Model`) và `<model>_<filter>` (ví dụ: `Survey3N_RGN`) dành cho Survey3
. Thư mục định dạng tiếp theo là `--format`: `tiff16`, `tiff8`, `png8`, `jpg8` hoặc `tiff32` cho `TIFF (32-bit, Percent)`.

Đối với máy ảnh đa quang phổ M3C, thư mục `clamp_masks/` cũng có thể xuất hiện trong thư mục định dạng, bên cạnh các thư mục sản phẩm (ví dụ: `tiff16/clamp_masks/Reflectance_Calibrated_Images/` và `tiff32/clamp_masks/Radiance_Images/`). Thư mục này chứa các mặt nạ theo từng pixel của các sản phẩm bức xạ và phản xạ, trong đó quá trình tách dải đã giới hạn giá trị âm xuống 0 — xem [Mặt nạ giới hạn](output-image-formats.md#clamp-masks). Không có thư mục sản phẩm nào chứa mặt nạ.

{% hint style="info" %}
**Mọi sản phẩm được xuất đều giữ nguyên tên tệp NGUỒN.**Một tệp xuất độ rạng rỡ từ `capture_..._raw.tif` vẫn được đặt tên là `capture_..._raw.tif` — chỉ là nó nằm trong thư mục `tiff32/Radiance_Images/`.**Thư mục xác định sản phẩm, không phải tên tệp**, vì vậy hãy sử dụng cú pháp glob cho thư mục, không phải cho phần mở rộng `*radiance*`.
{% endhint %}

### Các tùy chọn bạn sẽ thực sự sử dụng

| Cờ | Mặc định | Chức năng |
| --- | --- | --- |
| `-o, --output PATH` | thư mục dự án mặc định | Vị trí thư mục dự án (xem ở trên). |
| `-n, --project-name NAME` | dấu thời gian | Tên dự án. |
| `--format FMT` | `TIFF (16-bit)` | Một trong các giá trị: `TIFF (16-bit)`, `TIFF (8-bit)`, `TIFF (32-bit, Percent)`, `PNG (8-bit)`, `JPG (8-bit)`. Chloros
không nâng cấp độ phân giải: trên tất cả các hình ảnh 8-bit, định dạng sâu hơn được ghi dưới dạng `TIFF (8-bit)`, và điều này được ghi rõ một lần. |
| `--indices NAME [NAME ...]` | không | Các chỉ số thực vật cần xuất (xem [Chỉ số thực vật](#vegetation-indices)). |
| `--debayer {standard,texture-aware}` | `standard` | `texture-aware` = thuật toán khử hiệu ứng Bayer bằng mạng nơ-ron, chậm hơn, chất lượng cao nhất (Chloros
+, cần GPU CUDA: nếu không có, quá trình chạy sẽ sử dụng `standard`; bị từ chối đối với các bản chụp LATTICE, vốn luôn sử dụng `standard`). |
| `--vignette / --no-vignette` | bật | Chỉnh sửa hiệu ứng tối góc. |
| `--reflectance / --no-reflectance` | bật | Hiệu chuẩn độ phản xạ; đối với LATTICE, đây cũng là công tắc bật/tắt sản phẩm độ phản xạ. |
| `--input-level {auto,raw,debayered,processed}` | `auto` | Buộc điểm vào của quy trình xử lý cho các tệp TIFF của LATTICE. |

Đối với các tùy chọn khác — điều chỉnh phát hiện mục tiêu, PPK, điểm phơi sáng, cờ căn chỉnh mảng — hãy tham khảo [phần `process` trong Tài liệu tham khảo CLI
](reference/cli-reference.md).

***

## Chọn nội dung cần xuất (Sản phẩm LATTICE)

Quá trình xử lý LATTICE được phân tán sang **tất cả các sản phẩm áp dụng trong một lần chạy**. Bốn tùy chọn bật/tắt cho từng sản phẩm đều**được bật theo mặc định**; sử dụng biểu mẫu `--no-` để tắt một tùy chọn:

| Công tắc | Sản phẩm |
| --- | --- |
| `--debayered` | Demosaic tuyến tính → `Debayered_Images/`: loại bỏ độ lệch tối của gói hiệu chuẩn, sau đó là quá trình khử mô-saic nhận diện cạnh, không có hiệu chỉnh tuyến tính, trường phẳng hoặc tách kênh — cùng tệp mà quá trình chụp lưu ở mức `debayered`. Giữ nguyên hướng gốc của cảm biến (được ghi trong XMP). |
| `--preview` | Xem trước trên màn hình (cân bằng trắng + gamma; kéo dãn màu giả cho đa quang phổ) → `Preview_Images/` |
| `--radiance` | Độ sáng (radiance) dạng float32, W/m²/sr/nm → `Radiance_Images/` (luôn là `tiff32/`) |
| `--reflectance` | độ phản xạ uint16, tương thích với Pix4D → `Reflectance_Calibrated_Images/` |

Các camera chính RGB
chỉ phát ra dữ liệu đã loại bỏ hiệu ứng Bayer và bản xem trước — độ rạng rỡ/độ phản xạ theo từng dải tần không có ý nghĩa đối với cảm biến băng rộng, do đó các tùy chọn bật/tắt này không có tác dụng đối với chúng. Survey3
`.raw` bỏ qua các tùy chọn bật/tắt và tuân theo đường dẫn tiêu chuẩn về độ phản xạ/mục tiêu.

```bash
# Radiance only — no DAQ downwelling needed
chloros-cli process ~/captures/lattice_flight --no-debayered --no-preview --no-reflectance
```

**`--reflectance-source {auto,target,daq}`** (mặc định là `auto`) chọn tham chiếu độ phản xạ: `auto` lấy [mục tiêu hiệu chuẩn](calibration-targets.md) nằm trong khung hình và đã vượt qua kiểm tra chất lượng (QA) làm tham chiếu tuyệt đối, và chuyển sang sử dụng tỷ lệ phân chia ánh sáng xuống từ cảm biến ánh sáng của hệ thống thu thập dữ liệu (DAQ) (ρ = π·L/E) khi không có mục tiêu nào hiện diện (một mục tiêu vượt qua kiểm tra chất lượng với cờ đánh dấu sẽ cạnh tranh với DAQ về điểm số đo được); `target` áp dụng quy tắc nghiêm ngặt (không thay thế bằng DAQ); `daq` là tiêu chuẩn có thẩm quyền của DAQ. Các lần quét mục tiêu đo lường theo đơn vị có thể được cung cấp bằng `--target-reflectance-dir`.

{% hint style="info" %}
**Đọc các pixel phản xạ:**giá trị DN tương ứng với ρ = 1.0 là**theo từng nguồn** — các tệp LATTICE gắn thẻ `Chloros:PixelScale=32768` trong XMP; Các tệp Survey3
sử dụng giá trị 65535 và không chứa thẻ `Chloros:PixelScale` (nhưng chúng vẫn chứa các thẻ xử lý `Chloros:*` khác). Hãy đọc thẻ này và chia cho giá trị đó thay vì giả định một hằng số. Chi tiết và trường hợp ngoại lệ duy nhất không có tỷ lệ được đề cập trong [Tài liệu tham khảo CLI
](reference/cli-reference.md).
{% endhint %}

**Quá trình xử lý luôn bắt đầu từ `raw`.** Các sản phẩm phái sinh (dữ liệu xuất sau khi loại bỏ hiệu ứng debayering, độ rạng rỡ hoặc độ phản xạ) không bao giờ được đưa trở lại qua đường ống xử lý — việc nhập lại và xử lý chúng sẽ áp dụng hai lần các phép tính hiệu chuẩn, do đó Chloros
sẽ bỏ qua chúng và thông báo điều này. `--input-level` là lối thoát có chủ đích khi bạn thực sự cần ép buộc một điểm vào.

***

## Khi một lần chạy thất bại

Kể từ phiên bản 1.2.0, `process` sẽ báo lỗi rõ ràng thay vì “thành công” mà không hiển thị kết quả nào:

* Một lần chạy **yêu cầu các sản phẩm nhưng không ghi ra sản phẩm nào**— chỉ có `project.json` và `calibration_data.json` — sẽ hiển thị thông báo `Processing finished but wrote no image products.` và**thoát với giá trị không bằng 0**, do đó các skript có thể phát hiện được. Các nguyên nhân thường gặp: thư mục đầu vào không được nhận diện là dữ liệu thu thập (kiểm tra bố cục và `--input-level`), hoặc mọi sản phẩm được yêu cầu đều không áp dụng được cho các camera đó (ví dụ: yêu cầu độ sáng/độ phản xạ từ các camera chỉ hỗ trợ RGB
).
* Một **lần chạy chỉ lấy metadata có chủ đích** (tắt tất cả các sản phẩm, không có `--indices`) vẫn được coi là thành công — kết quả hình ảnh trống là kết quả chính xác trong trường hợp này.
* Chạy lại với `--verbose` và kiểm tra nhật ký hệ thống để tìm các dòng `[LATTICE-EXPORT]` / `[EXPORT-CHECK]`, những dòng này giải thích lý do bỏ qua từng camera.
* Trên hệ điều hành Windows
khi tính năng đường dẫn dài bị tắt, thư mục dự án có độ sâu vượt quá giới hạn 260 ký tự sẽ bị từ chối **trước khi** quá trình xử lý bắt đầu, và hệ thống sẽ sử dụng thư mục dự án dài nhất vẫn nằm trong giới hạn (xem [Nơi lưu trữ các tệp đầu ra](#where-the-outputs-land)).
* Các lệnh ghi hình (`lattice capture`, `lattice array-capture` và các nhóm lệnh ghi hình khác) sẽ liệt kê tên từng tệp không thể ghi được và thoát với mã lỗi `1`; quá trình ghi hình lặp lại sẽ dừng lại khi ổ đĩa đầy.

Mã thoát: `0` thành công · `1` lỗi chung · `2` lỗi tham số · `130` bị ngắt bởi Ctrl+C.

***

## Chỉ số Thực vật

Chạy lệnh `--indices` với một hoặc nhiều tên cài đặt sẵn; mỗi chỉ số sẽ được lưu vào thư mục `<INDEX>_Index_Images/` riêng:

```bash
chloros-cli process ~/images/flight_001 --indices NDVI GNDVI OSAVI
```

22 tên cài đặt sẵn mà `process --indices` chấp nhận:

`NDVI` `GNDVI` `NDRE` `OSAVI` `SAVI` `MSAVI2` `EVI` `MSR` `TDVI` `LAI` `GCI` `GRVI` `GSAVI` `GOSAVI` `NLI` `MNLI` `RDVI` `WDRVI` `CVI` `ENDVI` `GLI` `VARI`

Một tên mà dự án chưa liệt kê sẽ được thêm vào. Một tên không phải là tên có sẵn, hoặc tên mà các dải tần của camera không thể tạo ra, sẽ bị bỏ qua kèm theo cảnh báo nêu rõ tên đó. Nếu Giao diện dòng lệnh (CLI
) không thể đọc được danh sách chỉ mục hiện tại của dự án, nó sẽ giữ nguyên danh sách đó và hiển thị cảnh báo thay vì thay thế nó.

{% hint style="warning" %}
**Có ba danh sách chỉ mục — đừng nhầm lẫn chúng.**Menu thả xuống &quot;Cài đặt Dự án&quot; trong GUI có 27 công thức (bao gồm `FCI1`, `FCI2`, `GARI`, `GEMI`, `LCI` — năm công thức này chỉ dành cho GUI và**không** áp dụng cho `--indices`). Lệnh trực tiếp/ngoại tuyến `lattice index --preset` sử dụng danh sách 22 cài đặt sẵn riêng biệt của chính nó. Các công thức và phép tính dải quang phổ được ghi chép trong [Công thức chỉ số đa quang phổ](project-settings/multispectral-index-formulas.md).
{% endhint %}

***

## Cảm biến ánh sáng DAQ: Giới thiệu nhanh

Dòng sản phẩm `daq pool-*` điều khiển các cảm biến quang phổ DAQ của MAPIR
(DAQ-U qua USB, DAQ-M qua BLE, DAQ-E qua Ethernet) thông qua nhóm kết nối liên tục của hệ thống nền — giao diện người dùng (GUI), giao diện dòng lệnh (CLI
) và bộ công cụ phát triển phần mềm (SDK
) đều chia sẻ chung một đối tượng kết nối trực tiếp. **`pool-*` là đường dẫn DAQ được hỗ trợ trong phiên bản CLI
đi kèm**; các lệnh con `daq` khác mà bạn có thể thấy được đề cập là bề mặt chỉ dành cho nguồn nội bộ của MAPIR
và sẽ thoát ra với một lỗi rõ ràng hướng dẫn bạn đến `pool-*`.

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

`pool-record` không có `--duration` sẽ chạy cho đến khi gặp `pool-record --stop`; thư mục đầu ra mặc định là `~/Documents/DAQ Live View/` **trên máy của backend**. Hồ sơ hiệu chỉnh nắp được chọn tại thời điểm kết nối (`--cap-id`, mặc định của máy chủ là `sunshine_cosine`) và có thể được thay đổi trực tiếp bằng `pool-set-cap` — các cấu hình giới hạn và phạm vi hiệu chuẩn của cảm biến được đề cập trong các chương về DAQ của tài liệu hướng dẫn này.

{% hint style="warning" %}
**DAQ-E trên máy chủ có nhiều NIC:** lần tự động phát hiện `pool-connect --eth` đầu tiên sau khi khởi động có thể thất bại ngay cả khi cảm biến hoạt động bình thường. `--eth-host <ip-or-hostname>` là phương án đáng tin cậy — hãy sử dụng nó mỗi khi quá trình phát hiện không thành công.
{% endhint %}

***

## Camera LATTICE, PTP &amp; Tự động hóa dự án

Dòng lệnh `lattice` (hơn 45 lệnh con) bao quát toàn bộ quy trình làm việc với camera LATTICE từ đầu đến cuối: phát hiện, chụp ảnh đơn lẻ, mảng đồng bộ liên tục thông qua quy trình kết nối &quot;smart-prep&quot; của giao diện người dùng (GUI), xem trước trực tiếp trên trình duyệt, căn chỉnh, tính toán chỉ số và chẩn đoán NIC của máy chủ. Một số ví dụ:

```bash
chloros-cli lattice info                                          # discover cameras
chloros-cli lattice capture -o output/                            # one frame, all export types
chloros-cli lattice array-connect --serials SN1,SN2,SN3,SN4       # persistent synced array
chloros-cli lattice array-capture --processing reflectance -o out/
```

Bên cạnh đó: `chloros-cli time-sync` báo cáo về máy chủ PTP chính (grandmaster) mà máy chủ Chloros
đang chạy (các camera LATTICE và cảm biến DAQ-E hoạt động như thiết bị phụ thuộc vào nó để đồng bộ dấu thời gian giữa các thiết bị), và `chloros-cli project` mở một dự án Chloros
đã lưu và điều khiển các camera, mảng cảm biến và cảm biến của nó mà không cần giao diện đồ họa — bao gồm cả các kịch bản ghi hình YAML được lập trình sẵn.

Ba nhóm này (`lattice`, `project`, `daq pool-*`) cũng là những dòng duy nhất hỗ trợ `CHLOROS_BACKEND_URL` để điều khiển một backend **từ xa**; các lệnh cốt lõi luôn nhắm đến máy cục bộ.

Các hướng dẫn chi tiết được trình bày trong các chương về LATTICE của tài liệu này; mọi tham số đều có trong [Tham chiếu CLI
](reference/cli-reference.md).

***

## Khắc phục sự cố: 5 vấn đề hàng đầu

| Triệu chứng | Cách khắc phục |
| --- | --- |
| `Login required` hoặc một tác vụ được lên lịch bị treo tại dấu nhắc `Email:` | Chạy `chloros-cli login EMAIL 'PASSWORD'` một lần trên máy này — các lệnh không có phiên làm việc được lưu trong bộ nhớ đệm sẽ tương tác thay vì báo lỗi ngay lập tức. |
| `backend unreachable` | Khởi động ứng dụng máy tính để bàn Chloros
, hoặc tự khởi động phần backend: `"C:\Program Files\Chloros\backend\chloros-backend.exe"` trên Windows
(thư mục `backend` trong thư mục cài đặt của bạn; thư mục này không tồn tại trên `PATH`), `chloros-backend` trên Linux
. `chloros-cli backend status` hiển thị xem có phiên nào đang chạy hay không; `lattice`/`project`/`daq pool-*` không bao giờ khởi động dịch vụ. Nếu bạn chỉ định chúng trỏ đến một backend từ xa, hãy kiểm tra `CHLOROS_BACKEND_URL`. |
| Kết nối mảng bị chặn: `FRAMES WILL DROP` / `Reduce ROI to enable` | Vòng nhận của NIC máy chủ được đặt lại về mặc định — nguyên nhân hàng đầu khiến một hệ thống từng hoạt động bình thường từ chối kết nối, thường xảy ra sau khi cập nhật trình điều khiển NIC. Chạy lệnh `chloros-cli lattice network --fix` từ một cửa sổ terminal **có quyền quản trị** (hoặc thiết lập các tùy chọn `ReceiveBufferLen=256`, `PendingReceives=64`); xem phần *Cài đặt và Tối ưu hóa NIC Máy chủ* trong tài liệu tham khảo. |
| Lệnh con `daq` báo lỗi: &quot;yêu cầu gói DAQ đầy đủ…&quot; | Sử dụng các lệnh `daq pool-*`, các lệnh này điều khiển cùng một cảm biến thông qua hệ thống nền: kết nối, truyền dữ liệu, ghi dữ liệu, chọn bộ lọc, và đối với DAQ-E là cài đặt mạng, khởi động lại và cập nhật firmware (hoặc `chloros_sdk.connect_daq_sensor()` từ Python
). |
| Jetson hiển thị cảnh báo về bộ nhớ ảo trước khi xử lý các thư mục lớn | Thêm bộ nhớ ảo dựa trên tệp — giao diện dòng lệnh (CLI
) sẽ hiển thị chính xác các lệnh `fallocate`/`swapon` cần thực thi. |

***

## Truy cập Trợ giúp

```bash
chloros-cli --help              # top-level help
chloros-cli process --help      # per-command help
chloros-cli lattice --help
chloros-cli daq --help          # lists the pool-* subcommands
```

* **Mọi tham số, mọi lệnh con:** [Tham chiếu CLI
](reference/cli-reference.md)
* **Tương đương Python
:** [Python
SDK
](api-python-sdk.md) và [Tham chiếu SDK
](reference/sdk-reference.md)
* **Hỗ trợ:** info@mapir.camera · [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
