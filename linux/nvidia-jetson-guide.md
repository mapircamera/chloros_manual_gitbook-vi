# Hướng dẫn NVIDIA Jetson Tính năng “

Chloros

” trên NVIDIA Jetson cho phép xử lý hình ảnh đa quang phổ tại thiết bị đầu cuối — tại hiện trường, trên máy bay không người lái (UAV) và tại các cơ sở từ xa. Phiên bảnChloros

1.2.0 tự động nhận diện mẫu Jetson của bạn khi khởi động và tối ưu hóa chiến lược xử lý phù hợp với phần cứng được phát hiện. **Không cần điều chỉnh thủ công.**

***

## Các mẫu Jetson được hỗ trợ

| Mẫu                | RAM            | Chiến lược xử lý                                     | Mục đích sử dụng khuyến nghị                                          |
| -------------------- | -------------- | ------------------------------------------------------- | -------------------------------------------------------- |
| **Jetson AGX Orin**  | 32–64 GB chia sẻ | `GPU_PARALLEL` (2 worker)                              | Hiệu suất tối đa, tập dữ liệu lớn                      |
| **Jetson Orin NX**   | 8-16GB chia sẻ  | `GPU_PARALLEL` (2 worker, 16GB) / `GPU_SINGLE` (8GB)   | Khuyến nghị chính cho triển khai trên không và tại hiện trường |
| **Jetson Orin Nano** | 8GB chia sẻ     | `GPU_SINGLE` (1 worker, tuần tự)                     | Giải pháp tính toán biên cấp cơ bản                                 |

{% hint style="info" %}
Gói phần mềmLinux

arm64 yêu cầu **JetPack 6**, hiện có sẵn trên dòng sản phẩm Jetson Orin. Các mẫu cũ hơn (Nano, TX2, Xavier NX) không thể chạy JetPack 6 và không được hỗ trợ bởi gói phần mềm hiện tại.
{% endhint %}

***

## Yêu cầu

* **JetPack 6.x** (khuyến nghị sử dụng phiên bản mới nhất)
* **NVIDIA CUDA** (đi kèm với JetPack)
* **GóiChloros

+ trả phí** — Gói Copper trở lên (bắt buộc để truy cập tất cả các dịch vụCLI

/SDK

; được áp dụng ở phía máy chủ)

## Cài đặt

```bash
# Install the JetPack 6 .deb package
sudo dpkg -i chloros_1.2.0_arm64_jp6.deb
sudo apt-get install -f

# Verify installation
chloros-cli --version    # prints "Chloros CLI 1.2.0"

# Install Python SDK (optional) — the bundled wheel always matches this build
pip install --user /usr/lib/chloros/sdk/chloros_sdk-*.whl

# Run system diagnostics
chloros-cli selftest
```

Để biết chi tiết chung về cài đặtLinux

, vị trí tệp và khắc phục sự cố, hãy xem [Cài đặtLinux

](linux-installation.md).

{% hint style="info" %}
**Đặt thư mục giải nén trên thiết bị lưu trữ tốc độ cao.** Các tệp nhị phân đã biên dịch sẽ tự giải nén vào một thư mục tạm thời mỗi khi khởi chạy — quá trình này diễn ra cực kỳ chậm khi thực hiện từ thẻ SD.Chloros

sẽ tự động sử dụng `/mnt/ssd/tmp` nếu thư mục này tồn tại; nếu không, hãy đặt `TMPDIR` thành đường dẫn trên NVMe của bạn (`export TMPDIR=/mnt/nvme/tmp`).
{% endhint %}

***

## Tự động điều chỉnh tính toán trên Jetson

### Cách thức hoạt động

Khi khởi động,Chloros

sẽ phân tích cấu hình hệ thống của bạn:

1. **Phát hiện mẫu Jetson** thông qua `/proc/device-tree/model`
2. **Đọc bộ nhớ GPU/CPU dùng chung có sẵn** (Jetson sử dụng bộ nhớ thống nhất)
3. **Chọn chiến lược xử lý** (`GPU_PARALLEL`, `GPU_SINGLE` hoặc `CPU_PARALLEL`)
4. **Tự động thiết lập số lượng worker, loại pipeline và phân bổ bộ nhớ**Quyết định này dựa trên**tổng dung lượng RAM chia sẻ**, không phải tên mô hình:

* **Dưới 12GB RAM tổng cộng**(tất cả các Jetson 8GB): `GPU_SINGLE` với**1 công nhân — xử lý tuần tự có chủ đích**. Bộ nhớ quá hạn chế để chạy nhiều worker đồng thời, do đó các hình ảnh được xử lý lần lượt từng hình một. Trên các thiết bị Jetson có**8GB hoặc ít hơn**, Thread 3 bỏ qua hoàn toàn nhóm worker và thực thi công việc cho từng hình ảnh trực tiếp trong quá trình xử lý.
* **12GB trở lên**(Orin NX 16GB, AGX Orin): bộ nhớ thống nhất đáp ứng yêu cầu cho `GPU_PARALLEL`, nhưng số lượng worker**bị giới hạn ở mức 2 trên Jetson** — GPU, bộ nhớ RAM của các tiến trình công việc và bối cảnh CUDA cho từng tiến trình đều sử dụng chung một vùng nhớ chia sẻ, do đó việc tăng số lượng tiến trình công việc có thể dẫn đến lỗi hết bộ nhớ.

Bạn có thể ghi đè lựa chọn tự động bằng biến môi trường `CHLOROS_STRATEGY` — xem [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md#manual-strategy-override).

### Hành vi theo từng mô hình

| Mô hình Jetson                | Chiến lược       | Số tiến trình làm việc | Quá trình thực thi                                      |
| --------------------------- | -------------- | ------- | ---------------------------------------------- |
| **Jetson Orin Nano 8GB**    | `GPU_SINGLE`   | 1       | Vòng lặp tuần tự trong quá trình (`tiled_gpu` khi áp lực bộ nhớ cao) |
| **Jetson Orin NX 8GB**      | `GPU_SINGLE`   | 1       | Vòng lặp tuần tự trong quá trình                     |
| **Jetson Orin NX 16GB**     | `GPU_PARALLEL` | 2       | Các tiến trình làm việc đồng thời, đường dẫn `fused_gpu`  |
| **Jetson AGX Orin 32-64GB** | `GPU_PARALLEL` | 2       | Các tiến trình làm việc đồng thời, đường dẫn `fused_gpu`  |

Sự khác biệt chính giữa các nền tảng là **bộ nhớ**. Một thiết bị Jetson 8GB phải xử lý hình ảnh từng cái một bằng cách sử dụng phương pháp chia ô tiết kiệm bộ nhớ khi áp lực xử lý cao, trong khi Orin 16GB+ có thể chạy 2 hình ảnh qua GPU đồng thời bằng cách sử dụng đường ống hợp nhất có thông lượng cao hơn.

### Ngân sách GPU theo từng mô hình

Mỗi mô hình Jetson cũng đi kèm với một cấu hình phần cứng giới hạn lượng bộ nhớ chung mà quá trình xử lý có thể chiếm dụng, đồng thời điều chỉnh kích thước lô:

| Mô hình | Giới hạn ngân sách GPU | Hệ số nhân kích thước lô | Dành riêng cho hệ thống/màn hình |
| --- | --- | --- | --- |
| **Jetson Orin Nano** | 70% | ×0,8 | 2,0 GB |
| **Jetson Orin NX** | 75% | ×1,0 | 3,0 GB |
| **Jetson AGX Orin** | 80% | ×1.5 | 4.0 GB |

Dung lượng RAM được phát hiện sẽ điều chỉnh cấu hình: một thiết bị Jetson báo cáo **16 GB trở lên** sẽ được tăng hệ số nhân lô lên ×1.2. Kích thước lô cơ bản trước khi áp dụng hệ số nhân là 8 hình ảnh.

Để tham khảo tài liệu đầy đủ về điều chỉnh tính toán, xem [Điều chỉnh Tính toán Động](../processing-architecture/dynamic-compute-adaptation.md).

***

## Giới hạn tần số GPU cho Texture Aware trên Nano và Orin Nano

Quá trình khử hiệu ứng debayer của Texture Aware thực hiện suy luận mạng nơ-ron trên GPU, điều này có thể gây ra **cảnh báo quá dòng**trên các mẫu Jetson công suất thấp (loại 10-15W) khi GPU chạy ở tốc độ xung nhịp tối đa. Trước khi xử lý Texture Aware trên**Jetson Nano hoặc Orin Nano**, hàm `Chloros

` sẽ kiểm tra tần số tối đa của GPU và giới hạn nó ở mức **510 MHz** (510000000) nếu tần số hiện tại cao hơn:

* NếuCLI

có thể ghi vào nút sysfs tần số GPU, giới hạn sẽ được **áp dụng tự động** và một thông báo xác nhận sẽ được hiển thị.
* Nếu không (cần quyền root),CLI

sẽ hiển thị chính xác lệnh `sudo` để áp dụng giới hạn thủ công, chờ một lát để bạn có thể đọc lệnh, sau đó tiếp tục — quá trình xử lý vẫn diễn ra nhưng có thể hiển thị cảnh báo quá dòng.

Để tự áp dụng giới hạn trước khi xử lý:

```bash
echo 510000000 | sudo tee /sys/devices/platform/bus@0/17000000.gpu/devfreq/17000000.gpu/max_freq
```

Các mẫu có công suất cao hơn (Orin NX 25W, AGX Orin 60W) chạy ở tốc độ GPU tối đa; không áp dụng giới hạn nào. Chế độ debayer tiêu chuẩn không bao giờ kích hoạt giới hạn trên bất kỳ mẫu nào.

{% hint style="info" %}
**Chế độ Texture Aware trên Jetson luôn xử lý từng hình ảnh một.** Mỗi worker sẽ cần bối cảnh CUDA riêng (~1GB) cùng với bản sao riêng của mô hình khử nhiễu, điều mà bộ nhớ thống nhất không thể đáp ứng — do đó, trên Jetson, đường dẫn Texture Aware được gán cố định cho một worker duy nhất với quyền truy cập GPU được tuần tự hóa. Dự kiến Texture Aware sẽ chậm hơn đáng kể so với Standard trên bất kỳ thiết bị Jetson nào.
{% endhint %}

***

## Quản lý nhiệt

Các thiết bị Jetson có dung sai nhiệt hạn chế, đặc biệt là trong các triển khai trong môi trường kín hoặc trên không. Chế độ &quot;Chloros

&quot; theo dõi nhiệt độ SoC và tự động điều chỉnh kích thước lô:

| Nhiệt độ         | Hành động                                            |
| ------------------- | ------------------------------------------------- |
| **&lt; 70°C**          | Hoạt động bình thường — tốc độ xử lý tối đa          |
| **70°C** (Cảnh báo)  | Kích thước lô được thu nhỏ dần (từ 100% xuống 50% trong khoảng từ 70°C đến 80°C) |
| **80°C** (Nguy hiểm) | Giảm tốc mạnh (từ 50% xuống 0% trong khoảng từ 80°C đến 90°C) |
| **90°C** (Tắt máy) | Dừng hoàn toàn xử lý GPU — cần làm mát |

{% hint style="warning" %}
**Đảm bảo thông gió và tản nhiệt đầy đủ** để duy trì xử lý liên tục, đặc biệt trong các vỏ bảo vệ kín hoặc hệ thống trên không. Giới hạn nhiệt sẽ giảm thông lượng xử lý để bảo vệ phần cứng.
{% endhint %}

***

## Quản lý bộ nhớ

Các thiết bị Jetson sử dụng **bộ nhớ thống nhất** — GPU và CPU chia sẻ cùng một RAM vật lý. Dung lượng VRAM được báo cáo (ví dụ: ~15,3GB trên Orin NX 16GB) không phải là bộ nhớ GPU chuyên dụng; đó chính là RAM mà hệ điều hành và mọi quy trình khác đang sử dụng.

### Cảnh báo và khuyến nghị về bộ nhớ ảo

Trước khi xử lý trên Jetson, CLI sẽ đếm số lượng ảnh RAW trong thư mục đầu vào của bạn (`.tif`, `.tiff`, `.raw`, `.dng` — các bản xem trước JPG không được tính), ước tính dung lượng bộ nhớ tối đa mà quá trình chạy cần, và **cảnh báo trước khi bắt đầu** nếu RAM + vùng hoán đổi có khả năng không đủ. Cảnh báo có tiêu đề `LOW MEMORY WARNING - Jetson Detected`, hiển thị số lượng ảnh, dung lượng RAM, dung lượng swap hiện tại và mức đỉnh ước tính, sau đó cung cấp các lệnh chính xác `fallocate` / `chmod` / `mkswap` / `swapon` được điều chỉnh phù hợp với dự án của bạn (không bao giờ nhỏ hơn 8GB). Chương trình sẽ tạm dừng vài giây để thông báo không bị mất trong vùng cuộn lại, sau đó quá trình xử lý tiếp tục.**Các ước tính bộ nhớ được sử dụng bởi cảnh báo:**

| Chế độ Debayer | Cơ sở | Mỗi hình ảnh |
| --- | --- | --- |
| Tiêu chuẩn | ~1,5 GB | ~10 MB |
| Nhận diện kết cấu | ~2,5 GB (mô hình + thời gian chạy Python) | ~15 MB |

Cảnh báo sẽ xuất hiện khi mức đỉnh ước tính vượt quá dung lượng RAM + vùng hoán đổi trừ đi 1 GB dự phòng, và nó chỉ tính đến vùng hoán đổi **dựa trên tệp** — một cấu hình chỉ sử dụng zram vẫn sẽ bị báo lỗi.

Để thêm vùng hoán đổi thủ công (ví dụ: 8 GB):

```bash
# Check current memory and swap
free -h

# Create a swap file
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make persistent across reboots
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```



<!-- SCREENSHOT-NEEDED: Terminal on a Jetson Orin (SSH session) showing the full "LOW MEMORY WARNING - Jetson Detected" block printed by `chloros-cli process` on a large folder: the image count and debayer mode line, RAM / current swap / estimated peak figures, and the fallocate/chmod/mkswap/swapon command block it recommends -->

### Xử lý tình trạng OOM (Hết bộ nhớ)

Trong quá trình xử lý,Chloros

theo dõi bộ nhớ GPU và giảm hiệu suất một cách êm ái thay vì bị treo:

1. Khi mức sử dụng bộ nhớ GPU vượt quá **85%**, kích thước lô sẽ được giảm trước một cách chủ động
2. Nếu sự cố hết bộ nhớ vẫn xảy ra, kích thước lô sẽ bị **giảm một nửa**, và tiếp tục giảm một nửa trong mỗi lần OOM liên tiếp; mỗi lô xử lý thành công tiếp theo sẽ giảm mức phạt đó đi một bậc
3. Khi chịu áp lực kéo dài, đường ống xử lý sẽ chuyển từ `fused_gpu` sang đường dẫn `tiled_gpu` tiết kiệm bộ nhớ, và cuối cùng chuyển sang xử lý bằng CPU như phương án cuối cùng

***

## Triển khai thực tế

### Các yếu tố liên quan đến công suất

| Mô hình Jetson     | Công suất tiêu thụ điển hình | Ghi chú                   |
| ---------------- | ------------------ | ----------------------- |
| Jetson Orin Nano | 7–15W              | Cổng cắm DC dạng ống          |
| Jetson Orin NX   | 10–25W             | Cổng cắm DC dạng ống          |
| Jetson AGX Orin  | 15–60W             | USB-C PD hoặc cổng cắm DC dạng ống |

Lập kế hoạch ngân sách điện năng cho quá trình xử lý liên tục — mức tiêu thụ điện năng đỉnh xảy ra trong Thread 3 (Xử lý) sử dụng GPU nhiều.

### Khuyến nghị về bộ nhớ lưu trữ

* **Ổ SSD NVMe** được khuyến nghị mạnh mẽ cho các triển khai arm64
* Thẻ SD quá chậm để xử lý — chỉ nên sử dụng làm phương tiện khởi động
* Dự trù dung lượng gấp 2–3 lần kích thước dữ liệu hình ảnh thô cho đầu ra đã xử lý

### Vận hành không màn hình quaSSH



Chloros

CLI

là giải pháp lý tưởng cho các triển khai Jetson không màn hình:

```bash
# SSH into the Jetson
ssh user@jetson-hostname

# Process a dataset
chloros-cli process /data/datasets/flight001 --format "TIFF (32-bit, Percent)"

# Monitor export progress
chloros-cli export-status
```

### Hệ thống nền luôn hoạt động cho đồng bộ thời gian LATTICE / DAQ-E

Nếu Jetson của bạn điều khiển camera LATTICE hoặc cảm biến ánh sáng DAQ-E ở chế độ không màn hình, hãy kích hoạt dịch vụ systemd nền để máy chủ PTP grandmaster chạy liên tục (dịch vụ này đã được cài đặt nhưng không được kích hoạt theo mặc định):

```bash
sudo systemctl enable --now chloros-backend.service
chloros-cli time-sync status
```

Xem [Linux

Hướng dẫn cài đặt](linux-installation.md#always-on-ptp-for-headless-hosts) để biết chi tiết, bao gồm cách gói phần mềm này cho phép gán các cổng PTP 319/320 mà không cần quyền root.

### Xử lý tự động với systemd

Tạo dịch vụ systemd để xử lý tự động:

```ini
# /etc/systemd/system/chloros-process.service
[Unit]
Description=Chloros Automated Processing
After=network.target

[Service]
Type=oneshot
User=chloros
ExecStart=/usr/bin/chloros-cli process /data/incoming --output /data/processed
StandardOutput=append:/var/log/chloros-process.log
StandardError=append:/var/log/chloros-process.log

[Install]
WantedBy=multi-user.target
```

`chloros-cli process` trả về giá trị khác 0 khi một lần chạy yêu cầu sản phẩm nhưng không ghi hình ảnh nào, do đó trạng thái lỗi của systemd có ý nghĩa trong việc giám sát.

Kết hợp với bộ hẹn giờ systemd để xử lý theo lịch trình:

```ini
# /etc/systemd/system/chloros-process.timer
[Unit]
Description=Run Chloros Processing Every Hour

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
sudo systemctl enable chloros-process.timer
sudo systemctl start chloros-process.timer
```

***

## Ví dụ về quy trình làm việc

### Xử lý cơ bản trên Jetson

```bash
#!/bin/bash
# Process a drone flight dataset on Jetson
chloros-cli process /data/flights/flight_042 \
    --output /data/processed/flight_042 \
    --format "TIFF (32-bit, Percent)" \
    --indices NDVI NDRE GNDVI
```

###Python

SDK

trên Jetson

```python
from chloros_sdk import ChlorosLocal

with ChlorosLocal() as chloros:
    chloros.create_project("field_survey_042")
    chloros.import_images("/data/flights/flight_042")
    chloros.configure(
        indices=["NDVI", "NDRE", "GNDVI"],
        export_format="TIFF (32-bit, Percent)",
        reflectance_calibration=True
    )
    chloros.process(mode="parallel")

print("Processing complete!")
```

### Xử lý hàng loạt nhiều chuyến bay

```bash
#!/bin/bash
# Process all flight datasets in a directory
for flight in /data/flights/*/; do
    name=$(basename "$flight")
    echo "Processing $name..."
    chloros-cli process "$flight" \
        --output "/data/processed/$name" \
        --format "TIFF (32-bit, Percent)" \
        --indices NDVI NDRE
    echo "Completed $name"
done
```

***

## Các hệ thống Jetson được khuyến nghị cho sử dụng ngoài thực địa

Đối với các triển khai ngoài thực địa và trên không, hãy xem xét các tùy chọn bo mạch chủ Jetson Orin NX 16GB sau đây:

* **Trên không/máy bay không người lái**: Hệ thống có tiêu chuẩn chống rung (MIL-STD), trọng lượng nhẹ (dưới 300g), làm mát thụ động
* **Môi trường ngoài trời khắc nghiệt**: Vỏ bảo vệ chống nước IP67/IP69K với kết nối camera GigE qua PoE
* **Tối giản/tiết kiệm chi phí**: Bộ kit phát triển kèm vỏ bảo vệ bổ sung

Liên hệ với [Bộ phận Hỗ trợMAPIR

](https://www.mapir.camera/community/contact) để nhận các khuyến nghị cụ thể về phần cứng phù hợp với kịch bản triển khai của bạn.

***

## Các bước tiếp theo

* [Cài đặtLinux

](linux-installation.md) — Chi tiết chung về cài đặtLinux


* [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md) — Tài liệu tham khảo đầy đủ về chiến lược tính toán
* [Dòng xử lý](../processing-architecture/processing-pipeline.md) — Hiểu về dòng xử lý 4 luồng
* [CLI

: Command Line](../CLI.md) — Hướng dẫn vềCLI


* [API

:Python

SDK

](../api-python-sdk.md) — Hướng dẫn vềSDK


* [Tài liệu tham khảoCLI

](../reference/cli-reference.md) và [Tài liệu tham khảoSDK

](../reference/sdk-reference.md) — Danh sách đầy đủ các lệnh/API

cho phiên bản 1.2.0
