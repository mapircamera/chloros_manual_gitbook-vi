# Hướng dẫn sử dụng NVIDIA Jetson

Chloros trên NVIDIA Jetson cho phép xử lý hình ảnh đa quang phổ ngay tại thiết bị đầu cuối — tại hiện trường, trên máy bay không người lái (UAV) và tại các cơ sở từ xa. Chloros 1.3.0 tự động nhận diện mẫu Jetson của bạn khi khởi động và tối ưu hóa chiến lược xử lý phù hợp với phần cứng được phát hiện. **Không cần điều chỉnh thủ công.**
{% hint style="info" %}
**Gói Jetson 1.3.0 xử lý trên GPU của Jetson.** `chloros_1.3.0_arm64_jp6.deb` được phát triển cho JetPack 6 và sử dụng phiên bản PyTorch tích hợp CUDA của JetPack 6, do đó quá trình xử lý — bao gồm cả thuật toán Texture Aware debayer — sẽ chạy trên GPU của Orin. Gói này không chứa CUDA: nó sử dụng các thư viện CUDA và cuDNN mà JetPack 6 cài đặt (`sudo apt install nvidia-jetpack` trên hình ảnh không có các thư viện này). Trong trường hợp không có GPU CUDA nào có thể sử dụng, quá trình xử lý sẽ chạy trên CPU và Texture Aware sẽ sử dụng thuật toán debayer tiêu chuẩn.
{% endhint %}

***

## Các mẫu Jetson được hỗ trợ

| Mẫu                | RAM            | Chiến lược xử lý                                     | Mục đích sử dụng khuyến nghị                                          |
| -------------------- | -------------- | ------------------------------------------------------- | -------------------------------------------------------- |
| **Jetson AGX Orin**  | 32–64GB chia sẻ | `GPU_PARALLEL` (2 worker)                              | Hiệu suất tối đa, tập dữ liệu lớn                      |
| **Jetson Orin NX**   | 8–16GB chia sẻ  | `GPU_PARALLEL` (2 worker, 16GB) / `GPU_SINGLE` (8GB)   | Khuyến nghị chính cho triển khai tại hiện trường              |
| **Jetson Orin Nano** | 8GB chia sẻ     | `GPU_SINGLE` (1 worker, tuần tự)                     | Tính toán biên cấp cơ bản                                 |

{% hint style="info" %}
Gói Linux arm64 yêu cầu **JetPack 6**, hiện có sẵn trên dòng sản phẩm Jetson Orin. Các mẫu cũ hơn (Nano, TX2, Xavier NX) không thể chạy JetPack 6 và không được hỗ trợ bởi gói hiện tại.
{% endhint %}

***

## Yêu cầu

* **JetPack 6.x** (khuyến nghị dùng phiên bản mới nhất)
* **NVIDIA CUDA và cuDNN** từ JetPack 6 (`sudo apt install nvidia-jetpack` nếu hình ảnh của bạn thiếu các thành phần này)
* **Gói Chloros+ trả phí** — Cấp độ Copper trở lên (bắt buộc để truy cập tất cả các dịch vụ CLI/SDK; được áp dụng ở phía máy chủ)

## Cài đặt

```bash
# Install the arm64 .deb package (JetPack 6) -- apt, not dpkg -i, so the
# Python SDK's requirements (python3-numpy, python3-requests) come too
sudo apt install ./chloros_1.3.0_arm64_jp6.deb

# Verify installation
chloros-cli --version    # prints "Chloros CLI 1.3.0"
python3 -c "import chloros_sdk; print(chloros_sdk.__version__)"   # the .deb installs the SDK

# Run system diagnostics
chloros-cli selftest
```

`.deb` cài đặt SDK Python cho hệ thống `python3`, tương thích với bản dựng này. Chỉ môi trường ảo (virtualenv) mới cần cài đặt gói wheel đi kèm vào đó: `pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl` (hoàn toàn ngoại tuyến). Để biết chi tiết chung về cài đặt trên Linux, vị trí tệp và khắc phục sự cố, hãy xem [Cài đặt trên Linux](linux-installation.md).

{% hint style="info" %}
**Không còn giải nén khi khởi động.** Kể từ phiên bản 1.3.0, phần backend và giao diện dòng lệnh (CLI) được cài đặt ở dạng đã giải nén trong thư mục `/usr/lib/chloros/`, do đó chúng không còn tự động giải nén vào một thư mục tạm thời mỗi khi khởi động. Các tệp tạm thời vẫn được tự động lưu vào `/mnt/ssd/tmp` khi thư mục đó tồn tại; nếu không, bạn có thể thiết lập `TMPDIR` thành một đường dẫn trên NVMe của bạn (`export TMPDIR=/mnt/nvme/tmp`).
{% endhint %}

***

## Tính năng Điều chỉnh Tính toán Động trên Jetson

### Cách Hoạt động

Khi khởi động, Chloros sẽ phân tích cấu hình hệ thống của bạn:

1. **Phát hiện mẫu Jetson** thông qua `/proc/device-tree/model`
2. **Đọc dung lượng bộ nhớ GPU/CPU dùng chung hiện có** (Jetson sử dụng bộ nhớ thống nhất)
3. **Kiểm tra xem PyTorch có thể sử dụng CUDA hay không** — nếu đã cài đặt CUDA và cuDNN của JetPack 6, thì có thể
4. **Chọn chiến lược xử lý** (`GPU_PARALLEL`, `GPU_SINGLE` hoặc `CPU_PARALLEL`) và số lượng worker tự động

Quyết định này dựa trên **tổng dung lượng RAM chia sẻ**, không phải tên mô hình:

* **Dưới 12GB RAM tổng cộng**(tất cả các Jetson 8GB):**1 worker — xử lý tuần tự có chủ đích**. Dung lượng bộ nhớ quá hạn chế để chạy nhiều worker đồng thời, do đó các hình ảnh được xử lý lần lượt từng hình một. Trên các thiết bị Jetson có**8GB hoặc ít hơn**, Thread 3 bỏ qua hoàn toàn nhóm worker và thực thi công việc cho từng hình ảnh ngay trong quá trình xử lý. Hệ thống ghi nhật ký phía sau gọi chiến lược này là `GPU_SINGLE`.
* **12GB trở lên**(Orin NX 16GB, AGX Orin): bộ nhớ thống nhất đáp ứng điều kiện cho `GPU_PARALLEL`, nhưng số lượng worker**bị giới hạn ở mức 2 trên Jetson** — GPU, RAM của các tiến trình công việc và bối cảnh CUDA cho từng công việc đều sử dụng cùng một nhóm bộ nhớ chia sẻ, do đó, nếu có nhiều công việc hơn, sẽ có nguy cơ xảy ra lỗi hết bộ nhớ.
* **Không sử dụng được CUDA** (thiếu thư viện CUDA của JetPack): `CPU_PARALLEL` với dung lượng 12GB trở lên, mỗi lần chỉ áp dụng cho một hình ảnh.

Bạn có thể ghi đè lựa chọn tự động bằng biến môi trường `CHLOROS_STRATEGY` — xem [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md#manual-strategy-override).

### Hành vi theo từng mô hình

| Mô hình Jetson                | Chiến lược       | Số công nhân | Quá trình thực thi                                      |
| --------------------------- | -------------- | ------- | ---------------------------------------------- |
| **Jetson Orin Nano 8GB**    | `GPU_SINGLE`   | 1       | Vòng lặp tuần tự trong quá trình (`tiled_gpu` khi áp lực bộ nhớ cao) |
| **Jetson Orin NX 8GB**      | `GPU_SINGLE`   | 1       | Vòng lặp tuần tự trong quá trình                     |
| **Jetson Orin NX 16GB**     | `GPU_PARALLEL` | 2       | Các tiến trình làm việc đồng thời, đường dẫn `fused_gpu`  |
| **Jetson AGX Orin 32-64GB** | `GPU_PARALLEL` | 2       | Các tiến trình làm việc đồng thời, đường dẫn `fused_gpu`  |

Sự khác biệt chính giữa các nền tảng là **bộ nhớ**. Một thiết bị Jetson 8GB phải xử lý hình ảnh từng cái một bằng phương pháp chia ô tiết kiệm bộ nhớ khi tải cao, trong khi Orin 16GB+ có thể chạy 2 hình ảnh qua GPU đồng thời bằng đường ống xử lý hợp nhất có thông lượng cao hơn.

### Ngân sách GPU cho từng mô hình

Mỗi mô hình Jetson cũng đi kèm với một cấu hình phần cứng giới hạn lượng tài nguyên mà quá trình xử lý từ vùng bộ nhớ chung có thể chiếm dụng, đồng thời điều chỉnh kích thước lô:

| Mô hình | Giới hạn ngân sách GPU | Hệ số nhân kích thước lô | Dành riêng cho hệ thống/màn hình |
| --- | --- | --- | --- |
| **Jetson Orin Nano** | 70% | ×0.8 | 2.0 GB |
| **Jetson Orin NX** | 75% | ×1.0 | 3.0 GB |
| **Jetson AGX Orin** | 80% | ×1.5 | 4.0 GB |

Dung lượng RAM được phát hiện sẽ điều chỉnh cấu hình: một thiết bị Jetson báo cáo có **16 GB trở lên** sẽ được tăng hệ số nhân lô lên ×1.2. Kích thước lô cơ bản trước khi áp dụng hệ số nhân là 8 hình ảnh.

Để tham khảo đầy đủ về điều chỉnh tính toán, xem [Điều chỉnh tính toán động](../processing-architecture/dynamic-compute-adaptation.md).

***

## Giới hạn tần số GPU cho Texture Aware trên Nano và Orin Nano

Quá trình debayer của Texture Aware thực hiện suy luận mạng nơ-ron trên GPU, điều này có thể gây ra **cảnh báo quá dòng**trên các mẫu Jetson công suất thấp (loại 10-15W) khi GPU chạy ở tốc độ xung nhịp tối đa. Khi bạn truyền tham số `--debayer texture-aware` trên một**Jetson Nano hoặc Orin Nano**, CLI sẽ kiểm tra tần số tối đa của GPU và giới hạn nó ở mức **510 MHz** (510000000) nếu tần số hiện tại cao hơn:

* Nếu CLI có thể ghi vào nút sysfs tần số GPU, giới hạn sẽ được **áp dụng tự động** và một thông báo xác nhận sẽ được hiển thị.
* Nếu không (cần quyền root), CLI sẽ hiển thị chính xác lệnh `sudo` để áp dụng giới hạn thủ công, chờ một lát để bạn có thể đọc lệnh, sau đó tiếp tục — quá trình xử lý vẫn diễn ra nhưng có thể hiển thị cảnh báo quá dòng.

Để tự áp dụng giới hạn trước khi xử lý:

```bash
echo 510000000 | sudo tee /sys/devices/platform/bus@0/17000000.gpu/devfreq/17000000.gpu/max_freq
```

Các mẫu có công suất cao hơn (Orin NX 25W, AGX Orin 60W) chạy ở tốc độ GPU tối đa; không áp dụng giới hạn. Chế độ Standard debayer không bao giờ kích hoạt giới hạn trên bất kỳ mẫu nào.

{% hint style="info" %}
**Chế độ Texture Aware trên Jetson luôn xử lý từng hình ảnh một.** Mỗi worker cần có bối cảnh CUDA riêng (~1GB) cùng với bản sao riêng của mô hình khử nhiễu, điều mà bộ nhớ thống nhất không thể đáp ứng — do đó trên Jetson, đường dẫn Texture Aware được gán cố định cho một worker duy nhất với quyền truy cập GPU được tuần tự hóa. Dự kiến Texture Aware sẽ chậm hơn đáng kể so với Standard trên bất kỳ thiết bị Jetson nào. Chế độ Texture Aware áp dụng cho hình ảnh Survey3: Các bản chụp LATTICE luôn sử dụng thuật toán khử pixel (debayer) theo chế độ Standard. Nếu không có GPU CUDA khả dụng, quá trình xử lý sẽ sử dụng chế độ Standard, và hệ thống ghi lại một dòng thông báo `[DEBAYER] … needs a CUDA GPU …` giải thích lý do (được hiển thị cùng với `chloros-cli --verbose`).
{% endhint %}

***

## Quản lý nhiệt

Các thiết bị Jetson có dung sai nhiệt hạn chế, đặc biệt trong các triển khai trong không gian kín hoặc trên không. Chloros theo dõi nhiệt độ SoC và tự động điều chỉnh kích thước lô:

| Nhiệt độ         | Hành động                                            |
| ------------------- | ------------------------------------------------- |
| **< 70°C**          | Hoạt động bình thường — tốc độ xử lý tối đa          |
| **70°C** (Cảnh báo)  | Kích thước lô được thu nhỏ dần (từ 100% xuống 50% trong khoảng từ 70°C đến 80°C) |
| **80°C** (Nguy cấp) | Giảm tốc độ mạnh (từ 50% xuống 0% trong khoảng từ 80°C đến 90°C) |
| **90°C** (Tắt nguồn) | Dừng hoàn toàn xử lý GPU — cần làm mát |

{% hint style="warning" %}
**Đảm bảo thông gió và tản nhiệt đầy đủ** để duy trì xử lý liên tục, đặc biệt trong các vỏ bảo vệ kín hoặc hệ thống trên không. Việc giới hạn nhiệt độ sẽ giảm thông lượng xử lý để bảo vệ phần cứng.
{% endhint %}

***

## Quản lý bộ nhớ

Các thiết bị Jetson sử dụng **bộ nhớ thống nhất** — GPU và CPU chia sẻ cùng một RAM vật lý. VRAM được báo cáo (ví dụ: ~15,3GB trên Orin NX 16GB) không phải là bộ nhớ GPU chuyên dụng; đó là cùng một RAM mà hệ điều hành và mọi quy trình khác đang sử dụng.

### Cảnh báo và khuyến nghị về bộ nhớ ảo

Trước khi xử lý trên Jetson, CLI sẽ đếm số lượng hình ảnh RAW trong thư mục đầu vào của bạn (`.tif`, `.tiff`, `.raw`, `.dng` — các bản xem trước JPG không được tính), ước tính dung lượng bộ nhớ tối đa mà quá trình chạy cần, và **cảnh báo trước khi bắt đầu** nếu RAM cộng với vùng swap có khả năng không đủ. Cảnh báo có tiêu đề `LOW MEMORY WARNING - Jetson Detected`, hiển thị số lượng hình ảnh, RAM, dung lượng swap hiện tại và mức đỉnh ước tính, sau đó cung cấp các lệnh chính xác `fallocate` / `chmod` / `mkswap` / `swapon` được điều chỉnh phù hợp với dự án của bạn (không bao giờ nhỏ hơn 8GB). Hệ thống sẽ tạm dừng vài giây để thông báo không bị mất trong vùng cuộn lại, sau đó quá trình xử lý tiếp tục.**Ước tính bộ nhớ được sử dụng trong cảnh báo:**

| Chế độ Debayer | Cơ bản | Mỗi hình ảnh |
| --- | --- | --- |
| Tiêu chuẩn | ~1,5 GB | ~10 MB |
| Nhận diện kết cấu | ~2,5 GB (mô hình + thời gian chạy Python) | ~15 MB |

Cảnh báo sẽ xuất hiện khi mức đỉnh ước tính vượt quá dung lượng RAM + bộ nhớ ảo trừ đi biên độ an toàn 1GB, và cảnh báo chỉ tính đến bộ nhớ ảo **được hỗ trợ bởi tệp** — cấu hình chỉ sử dụng zram vẫn sẽ bị báo lỗi.

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


### Xử lý OOM (Hết bộ nhớ)

Trong quá trình xử lý, Chloros theo dõi bộ nhớ GPU và giảm hiệu suất một cách êm ái thay vì bị treo:

1. Khi mức sử dụng bộ nhớ GPU vượt quá **85%**, kích thước lô sẽ được giảm trước một cách chủ động
2. Nếu sự cố hết bộ nhớ vẫn xảy ra, kích thước lô sẽ bị **giảm một nửa**, và tiếp tục giảm một nửa trong mỗi lần OOM liên tiếp; mỗi lô xử lý thành công tiếp theo sẽ giảm mức phạt đó xuống một bậc
3. Dưới áp lực kéo dài, đường ống xử lý sẽ chuyển từ `fused_gpu` sang đường dẫn tiết kiệm bộ nhớ `tiled_gpu`, và cuối cùng chuyển sang xử lý trên CPU như phương án cuối cùng

Vì GPU chia sẻ RAM của Jetson, nên tổng dung lượng RAM cộng với vùng swap vẫn là giới hạn cho mỗi lần chạy — cảnh báo về vùng swap ở trên là thông báo cần lưu ý.

***

## Triển khai tại hiện trường

### Các yếu tố cần xem xét về nguồn điện

| Mô hình Jetson     | Công suất tiêu thụ điển hình | Ghi chú                   |
| ---------------- | ------------------ | ----------------------- |
| Jetson Orin Nano | 7–15W              | Cổng nguồn DC dạng barrel          |
| Jetson Orin NX   | 10–25W             | Cổng nguồn DC dạng barrel          |
| Jetson AGX Orin  | 15–60W             | USB-C PD hoặc cổng nguồn DC dạng barrel |

Lập kế hoạch ngân sách điện năng cho quá trình xử lý liên tục — mức tiêu thụ điện năng đỉnh xảy ra trong Thread 3 (Processing) khi sử dụng GPU nhiều.

### Khuyến nghị về bộ nhớ lưu trữ

* **SSD NVMe** được khuyến nghị mạnh mẽ cho các triển khai arm64
* Thẻ SD quá chậm để xử lý — chỉ nên sử dụng làm phương tiện khởi động
* Dự trù dung lượng gấp 2–3 lần kích thước dữ liệu hình ảnh thô cho đầu ra đã xử lý

### Vận hành không màn hình qua SSH Giao diện dòng lệnh (

Chloros) của SSH là lựa chọn lý tưởng cho các triển khai Jetson không màn hình:

```bash
# SSH into the Jetson
ssh user@jetson-hostname

# Process a dataset
chloros-cli process /data/datasets/flight001 --format "TIFF (32-bit, Percent)"

# Monitor export progress
chloros-cli export-status
```

### Hệ thống nền luôn hoạt động cho đồng bộ thời gian LATTICE / DAQ-E

Nếu Jetson của bạn điều khiển camera LATTICE hoặc cảm biến ánh sáng DAQ-E ở chế độ không màn hình, hãy kích hoạt dịch vụ systemd của hệ thống nền **dưới tài khoản người dùng của bạn** để máy chủ PTP chính hoạt động liên tục và các tập lệnh SDK có thể tìm thấy hệ thống nền đang chạy (thiết bị đã được cài đặt nhưng không được kích hoạt theo mặc định):

```bash
sudo systemctl enable --now chloros-backend@$USER.service    # $USER expands to your user name
chloros-cli time-sync status
```

Khi đó, hệ thống backend sẽ chạy dưới quyền của bạn: các dữ liệu ghi lại sẽ thuộc về bạn và hệ thống sẽ sử dụng `chloros-cli login` của bạn. Xem [Cài đặt trên Linux](linux-installation.md#always-on-ptp-for-headless-hosts) để biết chi tiết, bao gồm tài khoản root `chloros-backend.service` và cách gói phần mềm cho phép các cổng PTP 319/320 có thể gán địa chỉ mà không cần quyền root.

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

`chloros-cli process` sẽ trả về giá trị không bằng 0 khi một lần chạy yêu cầu sản phẩm nhưng không ghi được hình ảnh nào, do đó trạng thái lỗi của systemd có ý nghĩa trong việc giám sát.

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
    --indices NDVI GNDVI OSAVI
```

### SDK Python trên Jetson

```python
from chloros_sdk import ChlorosLocal

with ChlorosLocal() as chloros:
    chloros.create_project("field_survey_042")
    chloros.import_images("/data/flights/flight_042")
    chloros.configure(
        indices=["NDVI", "GNDVI", "OSAVI"],
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
        --indices NDVI OSAVI
    echo "Completed $name"
done
```

***

## Các hệ thống Jetson được khuyến nghị cho ứng dụng thực địa

Đối với các triển khai thực địa và trên không, hãy xem xét các tùy chọn bo mạch chủ Jetson Orin NX 16GB sau đây:

* **Trên không/máy bay không người lái**: Hệ thống có khả năng chống rung (MIL-STD), trọng lượng nhẹ (dưới 300g), làm mát thụ động
* **Môi trường thực địa khắc nghiệt**: Vỏ bảo vệ chống nước IP67/IP69K với kết nối camera GigE qua PoE
* **Tối giản/tiết kiệm**: Bộ công cụ phát triển kèm vỏ bảo vệ mở rộng

Liên hệ [Bộ phận Hỗ trợ MAPIR](https://www.mapir.camera/community/contact) để nhận các đề xuất phần cứng cụ thể cho kịch bản triển khai của bạn.

***

## Các bước tiếp theo

* [Cài đặt Linux](linux-installation.md) — Chi tiết chung về cài đặt Linux

* [Thích ứng tính toán động](../processing-architecture/dynamic-compute-adaptation.md) — Tài liệu tham khảo đầy đủ về chiến lược tính toán
* [Quy trình xử lý](../processing-architecture/processing-pipeline.md) — Hiểu về quy trình xử lý 4 luồng
* [CLI: Dòng lệnh](../CLI.md) — Hướng dẫn sử dụng CLI

* [API: Python SDK](../api-python-sdk.md) — Hướng dẫn sử dụng SDK

* [Tham khảo CLI](../reference/cli-reference.md) và [Tham khảo SDK](../reference/sdk-reference.md) — Danh sách đầy đủ các lệnh/API cho phiên bản 1.3.0
