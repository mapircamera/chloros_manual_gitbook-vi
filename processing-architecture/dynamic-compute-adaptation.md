# Tự động điều chỉnh khả năng tính toán

Chloros 1.3.0 sử dụng tính năng phát hiện phần cứng và tự động lựa chọn chiến lược xử lý. Bộ xử lý sẽ tự động điều chỉnh phù hợp với phần cứng của bạn — từ Jetson Orin Nano đến máy trạm đa GPU — mà không cần bất kỳ cấu hình thủ công nào.

***

## Cách thức hoạt động

Khi Chloros khởi động, nó sẽ phân tích hệ thống của bạn:

1. **Phát hiện hệ điều hành** — Windows hoặc Linux
2. **Xác định số lõi CPU và tổng dung lượng RAM**

3.**Phát hiện sự hiện diện của GPU** — khả năng NVIDIA CUDA, VRAM, mô hình
4. **Xác định mẫu Jetson** (nếu có) — thông qua `/proc/device-tree/model`
5. **Kiểm tra cảm biến nhiệt độ** (Jetson) — để xử lý có tính đến nhiệt độ
6. **Lựa chọn chiến lược tính toán** — dựa trên toàn bộ phần cứng đã phát hiện
7. **Tự động cấu hình số lượng worker, loại pipeline và phân bổ bộ nhớ**

Hồ sơ được phát hiện sẽ được lưu vào bộ nhớ đệm cho phiên làm việc cả trong bộ nhớ và trên đĩa, giúp các lần chạy sau khởi động nhanh hơn:

| Nền tảng | Hồ sơ được lưu vào bộ nhớ đệm |
| --- | --- |
| **Linux / Jetson** | `~/.config/chloros/system_config.json` (ưu tiên `XDG_CONFIG_HOME`) |
| **Windows** | `%LOCALAPPDATA%\Chloros\config\system_config.json` |

Xóa tệp đó để buộc thực hiện phát hiện lại — hữu ích sau khi thêm GPU hoặc tăng dung lượng RAM. Chloros cũng tự động phát hiện lại khi bộ nhớ đệm được ghi bởi một phiên bản cũ không tương thích.

***

## Chiến lược tính toán

Chloros chọn một trong ba chiến lược tính toán dựa trên phần cứng của bạn:

| Chiến lược | Được chọn khi | Công nhân | Bộ thực thi | Đường ống |
| --- | --- | --- | --- | --- |
| **`GPU_PARALLEL`**| GPU CUDA báo cáo**12GB+ VRAM**(trên bộ nhớ thống nhất Jetson, cũng yêu cầu tổng RAM chia sẻ từ 12GB trở lên) | `min(4, ⌊(VRAM − 0.5GB) × 0.9 ÷ 3.5GB⌋)`, tối thiểu 2 —**giới hạn tối đa 2 trên Jetson** | Các tiến trình công việc (bối cảnh CUDA riêng biệt) | `fused_gpu` |
| **`GPU_SINGLE`**| GPU CUDA với**2–12GB VRAM**| 3 (trùng lặp I/O; truy cập GPU được tuần tự hóa bằng semaphore).**1 (tuần tự) trên các thiết bị Jetson có RAM dưới 12GB** | Các tiến trình làm việc (các bối cảnh CUDA riêng biệt); tuần tự trong cùng tiến trình trên Jetson có RAM thấp | `fused_gpu` / `tiled_gpu` |
| **`CPU_PARALLEL`** | Không có GPU CUDA, hoặc dưới 2GB VRAM | `max(2, physical cores − 1)` | `ThreadPoolExecutor` | `cpu_fallback` |

Ví dụ minh họa về công thức xác định số worker của `GPU_PARALLEL`: 12GB VRAM → 2 worker, 16GB → 3 worker, 20GB+ → 4 worker. Mỗi worker được phân bổ 3,5GB, tương ứng với nhu cầu thực tế của tác vụ Texture Aware, trong phạm vi 90% dung lượng VRAM còn lại sau khi trừ đi 0,5GB dự trữ.

{% hint style="info" %}
**Các chiến lược GPU yêu cầu PyTorch có thể sử dụng CUDA.** Một máy tính có PyTorch không có thiết bị CUDA khả dụng sẽ nhận được `CPU_PARALLEL`, bất kể GPU của nó là gì. Gói Jetson 1.3.0 sử dụng phiên bản PyTorch được biên dịch với CUDA của JetPack 6, do đó một thiết bị Jetson sẽ đáp ứng yêu cầu ngay khi các thư viện CUDA và cuDNN của JetPack được cài đặt. Một thiết bị Jetson có RAM dưới 12GB luôn xử lý một hình ảnh tại một thời điểm, trong quá trình xử lý (được ghi lại là `GPU_SINGLE`).
{% endhint %}

Các chiến lược GPU chạy một nhóm các tiến trình công nhân, mỗi tiến trình là một tiến trình hệ điều hành riêng biệt với bối cảnh CUDA riêng, mà Chloros khởi động bằng cách khởi chạy lại chương trình backend đã cài đặt của chính nó (không thông qua `multiprocessing` của Python, vì việc khởi động tiến trình công nhân của nó không hoạt động bên trong ứng dụng được đóng gói). Chiến lược CPU sử dụng một nhóm luồng (`ThreadPoolExecutor` của Python). Chloros không sử dụng bất kỳ khung phân tán của bên thứ ba nào (chẳng hạn như Ray).

### Các loại đường ống xử lý

* **`fused_gpu`** — Đường ống xử lý hoàn toàn trên GPU. Các thao tác loại bỏ hiệu ứng Bayer, hiệu chỉnh và lập chỉ mục được thực hiện trên GPU trong một lần xử lý hợp nhất. Có thông lượng cao nhất, nhưng cần nhiều VRAM nhất.
* **`tiled_gpu`** — Đường dẫn GPU tiết kiệm bộ nhớ. Xử lý hình ảnh theo các ô (tiles) để phù hợp với bộ nhớ GPU hạn chế. Thông lượng thấp hơn nhưng hoạt động trên các thiết bị có bộ nhớ hạn chế.
* **`cpu_fallback`** — Xử lý chỉ bằng CPU sử dụng song song đa luồng. Được sử dụng khi không có GPU NVIDIA và là phương án dự phòng cuối cùng khi cả hai đường dẫn GPU đều thất bại.

Chuỗi giải pháp dự phòng trong thời gian chạy luôn là `fused_gpu` → `tiled_gpu` → `cpu_fallback`.

***

## Ghi đè chiến lược thủ công

Đặt biến môi trường `CHLOROS_STRATEGY` để ép sử dụng một chiến lược cụ thể — một giải pháp dự phòng chuyên sâu dành cho trường hợp phát hiện tự động chọn một chiến lược không phù hợp với tình huống của bạn (ví dụ: giữ GPU trống để dành cho các tác vụ khác):

```bash
# Valid values: CPU_PARALLEL, GPU_SINGLE, GPU_PARALLEL
CHLOROS_STRATEGY=CPU_PARALLEL chloros-cli process ~/datasets/flight001
```

Biến này được so khớp mà không phân biệt chữ hoa chữ thường; bất kỳ giá trị nào không phải là một trong ba tên trên sẽ bị bỏ qua và tính năng tự động phát hiện sẽ tiếp tục hoạt động bình thường. Khi áp dụng tùy chỉnh, Chloros vẫn sẽ tự động chọn số lượng công nhân cho bạn:

| Tùy chỉnh | Số lượng công nhân được sử dụng |
| --- | --- |
| `CPU_PARALLEL` | `max(2, physical cores − 1)` |
| `GPU_SINGLE` | 3 |
| `GPU_PARALLEL` | `min(4, physical cores)` |

Việc ghi đè bằng `GPU_SINGLE` hoặc `GPU_PARALLEL` trên một máy không có thiết bị CUDA khả dụng sẽ bị từ chối với dòng `[STRATEGY]` trong nhật ký, và quá trình chạy sẽ sử dụng `CPU_PARALLEL`.

Nên thiết lập theo từng lệnh thay vì thiết lập vĩnh viễn, để các lần chạy thông thường vẫn tiếp tục tự động điều chỉnh.

***

## Hành vi cụ thể theo nền tảng

| Nền tảng | Chiến lược | Công nhân | Đường ống | Ghi chú |
| --- | --- | --- | --- | --- |
| **Jetson Orin Nano 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` (từng bước) | Chế độ tiết kiệm bộ nhớ, xử lý từng hình ảnh một |
| **Jetson Orin NX 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` (từng bước) | RAM chia sẻ dưới 12GB buộc phải xử lý từng bước |
| **Jetson Orin NX 16GB** | `GPU_PARALLEL` | 2 | `fused_gpu` (đồng thời) | Thiết bị biên được khuyến nghị — Jetson giới hạn ở 2 công nhân |
| **Jetson AGX Orin 32–64GB** | `GPU_PARALLEL` | 2 | `fused_gpu` (đồng thời) | Hiệu năng biên tối đa (cũng bị giới hạn ở 2 worker trên Jetson) |
| **Máy tính để bàn với GPU 8GB** | `GPU_SINGLE` | 3 | `fused_gpu` / `tiled_gpu` | 3 worker chồng chéo I/O trong khi semaphore tuần tự hóa quyền truy cập GPU |
| **Máy tính để bàn với GPU 12GB+** | `GPU_PARALLEL` | 2-4 | `fused_gpu` (đồng thời) | Hiệu suất máy tính để bàn tối ưu: 12GB → 2 công nhân, 16GB → 3, 20GB+ → 4 |
| **Hệ thống chỉ dùng CPU** | `CPU_PARALLEL` | số lõi vật lý − 1 (tối thiểu 2) | `cpu_fallback` | Không cần GPU, sử dụng nhóm luồng |

{% hint style="info" %}
**Bộ nhớ thống nhất Jetson**: Các thiết bị Jetson chia sẻ bộ nhớ giữa GPU và CPU. Một thiết bị Jetson Orin NX 16GB báo cáo khoảng 15,3GB VRAM, nhưng đó chính là bộ nhớ RAM vật lý được hệ điều hành và các tiến trình CPU sử dụng. Đó là lý do tại sao các thiết bị Jetson 16GB+ đủ điều kiện cho `GPU_PARALLEL` giống như GPU máy tính để bàn 12GB+, nhưng lại bị giới hạn ở 2 worker — GPU, các tiến trình worker và bối cảnh CUDA cho từng worker đều sử dụng chung cùng một bể bộ nhớ chia sẻ.
{% endhint %}

### Ngân sách GPU theo VRAM (GPU rời)

Trên các máy chủ x86_64 có GPU rời NVIDIA, dung lượng VRAM được phát hiện cũng quyết định lượng tài nguyên xử lý mà card có thể sử dụng và kích thước tối đa của các lô dữ liệu:

| VRAM được phát hiện | Giới hạn ngân sách GPU | Hệ số nhân kích thước lô |
| --- | --- | --- |
| **8GB trở lên** | 90% | ×2.0 |
| **6–8GB** | 85% | ×1.75 |
| **3,5–6GB** | 80% | ×1,5 |
| **2–3,5GB** | 75% | ×1,25 |
| **Dưới 2GB** | 70% | ×1,0 |

Các GPU rời chỉ dành 0,5GB cho hệ thống, vì chúng không chia sẻ RAM hệ thống. Các cấu hình Jetson dành nhiều bộ nhớ hơn và giới hạn thấp hơn — xem [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md#per-model-gpu-budget).

***

## Phân bổ bộ nhớ GPU động

Chloros sử dụng [đường ống xử lý 4 luồng](processing-pipeline.md):

* **Luồng 1** (Phát hiện) — Tải hình ảnh, phân tích EXIF, phát hiện mục tiêu
* **Luồng 2** (Hiệu chuẩn) — Tính toán hiệu chỉnh độ phản xạ
* **Luồng 3** (Xử lý) — Loại bỏ hiệu ứng debayer bằng GPU, chỉnh sửa hiệu ứng vignette, tính toán chỉ số
* **Luồng 4** (Xuất) — Ghi tệp, nhúng siêu dữ liệu

Các luồng 1, 2 và 4 tiêu thụ ít tài nguyên GPU; Luồng 3 là luồng tiêu tốn nhiều tài nguyên nhất. Khi các luồng ở giai đoạn trước hoàn thành, ngân sách GPU của chúng sẽ được **phân bổ lại cho các luồng còn hoạt động**, do đó Luồng 3 sẽ nhận được ngày càng nhiều bộ nhớ hơn khi quá trình xử lý tiến triển.

### Các giai đoạn phân bổ

| Giai đoạn | Luồng đang hoạt động | Phân bổ bộ nhớ GPU |
| --- | --- | --- |
| **Giai đoạn đầu** | 1, 2, 3, 4 | Phân bổ đều cho tất cả các luồng, phần lớn dành cho Luồng 3 |
| **Giai đoạn giữa-đầu** | 2, 3, 4 | Phần của Luồng 1 được phân bổ lại |
| **Giữa-Cuối** | 3, 4 | Phần của luồng 1 và 2 được chuyển sang luồng 3 và 4 |
| **Cuối** | 3 hoặc 4 | Luồng hoạt động cuối cùng nhận được mức phân bổ tối đa |

Có hai quy tắc chi phối các con số này:

* Một luồng là luồng **duy nhất** đang hoạt động sẽ được cấp mức phân bổ tối đa theo cấu hình của nó.
* Khi có nhiều hơn một tác vụ GPU *nặng* đang hoạt động, mức phân bổ cơ bản của mỗi tác vụ nặng sẽ được chia đều cho các luồng đó (không bao giờ thấp hơn mức tối thiểu đã cấu hình).

Giá trị thực tế được sử dụng trong thời gian chạy là giá trị **thấp hơn** giữa mức phân bổ của cấu hình nền tảng và khuyến nghị thời gian thực từ trình giám sát bộ nhớ GPU, do đó một card đang bận rộn luôn được ưu tiên hơn so với một cấu hình quá lạc quan.

***

## Xử lý nhận biết kết cấu

Bộ khử răng cưa nhận biết kết cấu (**chỉ Chloros+** — `--debayer texture-aware`) chạy mô hình khử nhiễu AI/ML cần khoảng 1,75GB VRAM ở định dạng FP16 cho mỗi bản sao, do đó nó sử dụng nhiều bộ nhớ GPU hơn so với phương pháp Tiêu chuẩn:

* Các hệ thống có **dưới 7GB VRAM**xử lý Texture Aware trong một**vòng lặp đồng bộ, từng hình ảnh một** — không thể chứa nhiều bản sao mô hình, và việc sử dụng nhóm công nhân (worker pool) chỉ làm tăng xung đột
* Các hệ thống có **VRAM từ 7GB trở lên** có thể xử lý Texture Aware đồng thời, mặc dù số lượng worker sẽ ít hơn so với phương pháp Standard
* Trên **Jetson**, Texture Aware luôn được gán cố định cho một công nhân duy nhất
* Nếu **không có GPU CUDA khả dụng**, quá trình chạy sẽ sử dụng thuật toán debayer Standard, và hệ thống ghi lại một dòng thông báo `[DEBAYER] … needs a CUDA GPU …` giải thích lý do
* Trên các thiết bị Jetson công suất thấp (Nano, Orin Nano), giao diện dòng lệnh (CLI) sẽ áp dụng giới hạn tần số GPU khi bạn yêu cầu chế độ Texture Aware — xem [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md#gpu-frequency-cap-for-texture-aware-on-nano-and-orin-nano)

***

## Quản lý nhiệt (Jetson)

Các thiết bị Jetson có giới hạn về nhiệt độ, đặc biệt là trong các môi trường triển khai kín hoặc trên không. Chloros theo dõi các cảm biến nhiệt độ tích hợp trên Jetson và tự động điều chỉnh kích thước lô:

| Nhiệt độ | Phản ứng |
| --- | --- |
| **< 70°C** | Hoạt động bình thường — tốc độ tối đa |
| **70°C** (Cảnh báo) | Kích thước lô được thu nhỏ dần (từ 100% xuống 50% trong khoảng từ 70°C đến 80°C) |
| **80°C** (Nguy cấp) | Giảm tốc độ mạnh (từ 50% xuống 0% trong khoảng từ 80°C đến 90°C) |
| **90°C** (Tắt nguồn) | Dừng hoàn toàn quá trình xử lý GPU |

Trên các hệ thống máy tính để bàn có hệ thống làm mát đủ tốt, việc giới hạn nhiệt độ hiếm khi được kích hoạt.

***

## Xử lý áp lực bộ nhớ

Chloros liên tục theo dõi bộ nhớ GPU trong quá trình xử lý và phản ứng ở ba mức độ.

**Kích thước lô.** Một lô bắt đầu với 8 hình ảnh nhân với hệ số nhân của nền tảng từ các bảng trên. Sau đó, Chloros kiểm tra VRAM trống, dành riêng 20% trong số đó cho chi phí hoạt động của chính PyTorch và giả định khoảng 100MB bộ nhớ GPU cho mỗi hình ảnh 12MP — kích thước lô sẽ là giá trị nhỏ hơn giữa giới hạn tính từ bộ nhớ và giá trị cơ sở của nền tảng. Nó không bao giờ giảm xuống dưới 1.

**Giảm kích thước lô chủ động.**Khi**tỷ lệ sử dụng VRAM vượt quá 85%**, kích thước lô sẽ được giảm xuống trước khi xảy ra lỗi.**Cắt giảm phân bổ theo luồng.** Khi mức sử dụng thực tế tăng lên, ngân sách GPU của mỗi luồng sẽ bị thu hẹp: ×0,75 khi sử dụng vượt quá 80%, ×0,5 khi vượt quá 90%. Các ngưỡng theo dõi là 70% (thận trọng), 85% (giới hạn hoạt động bình thường) và 95% (nguy cơ hết bộ nhớ).**Thoái lui và phục hồi khi hết bộ nhớ (OOM).** Nếu sự cố hết bộ nhớ vẫn xảy ra:

* kích thước lô sẽ bị **giảm một nửa**, và tiếp tục giảm một nửa nữa ở mỗi lần hết bộ nhớ liên tiếp — mỗi lô thành công tiếp theo sẽ giảm mức phạt này xuống một bậc
* việc phân bổ cho các luồng đang hoạt động sẽ bị cắt giảm xuống 70% so với giá trị hiện tại và bộ phân bổ sẽ chuyển sang chiến lược thận trọng, sau đó nới lỏng trở lại sau một chuỗi các lần phân bổ thành công
* khi gặp áp lực nghiêm trọng, đường ống xử lý sẽ chuyển từ `fused_gpu` sang `tiled_gpu`, và sang `cpu_fallback` như phương án cuối cùng

**Bộ nhớ RAM máy chủ (Jetson).** Trước khi xử lý, giao diện dòng lệnh (CLI) sẽ ước tính dung lượng bộ nhớ tối đa của máy chủ dựa trên số lượng hình ảnh và chế độ debayer, đồng thời cảnh báo nếu dung lượng RAM cộng với vùng hoán đổi dựa trên tệp có khả năng không đủ, đồng thời hiển thị các lệnh chính xác để thêm vùng hoán đổi — xem [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md#swap-warning-and-recommendations).

***

## Giám sát quá trình điều chỉnh tính toán

### Chẩn đoán hệ thống

`chloros-cli selftest` là cách nhanh nhất để xác nhận những gì lớp tính toán nhận diện được:

```bash
chloros-cli selftest
```

7 kiểm tra này bao gồm phiên bản, tính khả dụng của cổng, quá trình khởi động backend, `/api/test`, thông tin hệ thống, sự hiện diện của mô hình khử nhiễu và trạng thái sẵn sàng của CUDA cùng mô hình khử nhiễu. Kiểm tra số 5 hiển thị trực tiếp thông tin phần cứng:

```
      GPU: NVIDIA RTX A4000, CUDA: True, PyTorch: 2.7.0
```

Kiểm tra 7 hiển thị `CUDA: <bool>, Denoiser: <bool>` — cả hai điều kiện này đều phải đúng thì Texture Aware mới có thể sử dụng được.

### Nhật ký backend

Chiến lược và số lượng worker được chọn bên trong backend khi bắt đầu mỗi lần chạy — không có thông báo trên giao diện dòng lệnh (CLI) nào hiển thị các thông tin này. Khi xảy ra sự cố bất thường (đường dẫn GPU chuyển sang phương án dự phòng, lỗi OOM, bộ lọc giảm nhiễu không thể tải), nhật ký backend của phiên làm việc đó sẽ ghi lại thông tin này:

| Nền tảng | Vị trí nhật ký |
| --- | --- |
| **Hệ điều hành Linux/Jetson** | `~/.cache/chloros/logs/backend_<YYYYMMDD_HHMMSS>.log` (một tệp cho mỗi lần khởi chạy) |
| **Linux, backend khởi chạy qua CLI** | cũng là `~/.chloros/backend.log` |
| **Windows** | `%LOCALAPPDATA%\Chloros\logs\` |

### Tiến trình thời gian thực

Trong quá trình chạy, giao diện dòng lệnh (CLI) hiển thị tiến trình theo thời gian thực cho từng luồng (Phát hiện, Phân tích, Xử lý, Xuất) thông qua Server-Sent Events — đây là cách thực tế để xác định liệu Luồng 3 có phải là điểm nghẽn hay không. Xem [Quy trình xử lý](processing-pipeline.md).

***

## Các bước tiếp theo

* [Quy trình xử lý](processing-pipeline.md) — Hiểu về kiến trúc quy trình 4 luồng
* [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md) — Triển khai và tối ưu hóa dành riêng cho Jetson
* [CLI: Dòng lệnh](../CLI.md) — Hướng dẫn về CLI

* [Tham chiếu CLI](../reference/cli-reference.md) — Danh sách đầy đủ các lệnh cho phiên bản 1.3.0
