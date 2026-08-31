# Tính năng Điều chỉnh Tính toán Động

Chloros
Phiên bản 1.2.0 sử dụng tính năng phát hiện phần cứng và tự động lựa chọn chiến lược xử lý. Bộ xử lý sẽ tự động điều chỉnh phù hợp với phần cứng của bạn — từ Jetson Orin Nano đến máy trạm đa GPU — mà không cần bất kỳ cấu hình thủ công nào.

***

## Cách thức hoạt động

KhiChloros
khởi động, nó sẽ phân tích hệ thống của bạn:

1. **Phát hiện hệ điều hành** —Windows
hoặcLinux
2. **Xác định số lõi CPU và tổng dung lượng RAM**

3.**Phát hiện sự hiện diện của GPU** — khả năng NVIDIA CUDA, VRAM, mô hình
4. **Xác định mẫu Jetson** (nếu có) — thông qua `/proc/device-tree/model`
5. **Kiểm tra cảm biến nhiệt** (Jetson) — để xử lý có tính đến nhiệt độ
6. **Chọn chiến lược tính toán** — dựa trên tất cả phần cứng được phát hiện
7. **Tự động cấu hình số lượng worker, loại pipeline và phân bổ bộ nhớ**

Hồ sơ được phát hiện sẽ được lưu vào bộ nhớ đệm cho phiên làm việc cả trong bộ nhớ và trên đĩa, giúp các lần chạy sau khởi động nhanh hơn:

| Nền tảng | Hồ sơ được lưu vào bộ nhớ đệm |
| --- | --- |
| **Linux
/ Jetson** | `~/.config/chloros/system_config.json` (ưu tiên `XDG_CONFIG_HOME`) |
| **Windows** | `%LOCALAPPDATA%\Chloros\config\system_config.json` |

Xóa tệp đó để buộc thực hiện phát hiện lại — hữu ích sau khi thêm GPU hoặc RAM.Chloros
cũng tự động phát hiện lại khi bộ nhớ đệm được ghi bởi một phiên bản cũ không tương thích.

***

## Chiến lược tính toán

Chloros
chọn một trong ba chiến lược tính toán dựa trên phần cứng của bạn:

| Chiến lược | Được chọn khi | Công nhân | Trình thực thi | Đường ống |
| --- | --- | --- | --- | --- |
| **`GPU_PARALLEL`**| GPU CUDA báo cáo**12GB+ VRAM**(trên bộ nhớ thống nhất Jetson, cũng yêu cầu tổng RAM chia sẻ từ 12GB trở lên) | `min(4, VRAM ÷ 4GB)`, tối thiểu 2 —**giới hạn tối đa 2 trên Jetson** | `ProcessPoolExecutor` (spawn) | `fused_gpu` |
| **`GPU_SINGLE`**| GPU CUDA có**2–12GB VRAM**| 3 (trùng lặp I/O; truy cập GPU được tuần tự hóa bằng semaphore).**1 (tuần tự) trên các thiết bị Jetson có RAM dưới 12GB** | `ProcessPoolExecutor` (spawn); tính toán tuần tự trong cùng một tiến trình trên các thiết bị Jetson có RAM thấp | `fused_gpu` / `tiled_gpu` |
| **`CPU_PARALLEL`** | Không có GPU CUDA, hoặc VRAM dưới 2GB | `max(2, physical cores − 1)` | `ThreadPoolExecutor` | `cpu_fallback` |

Ví dụ minh họa về công thức xác định số worker của `GPU_PARALLEL`: 12GB VRAM → 3 worker, 16GB trở lên → 4 worker, bất kỳ Jetson nào → 2 worker.

Tính song song được triển khai bằng cách sử dụng tiêu chuẩn `concurrent.futures` củPython
: Các chiến lược GPU sử dụng `ProcessPoolExecutor` với phương thức bắt đầu **spawn** (mỗi worker là một tiến trình riêng biệt với bối cảnh CUDA riêng — `fork` sẽ sao chép trạng thái CUDA đã được khởi tạo sẵn và làm hỏng các tiến trình con), còn chiến lược CPU sử dụng `ThreadPoolExecutor`.Chloros
không sử dụng bất kỳ khung công tác phân tán của bên thứ ba nào (chẳng hạn như Ray).

### Các loại đường ống

* **`fused_gpu`** — Đường ống xử lý hoàn toàn trên GPU. Các thao tác khử hiệu ứng Bayer, hiệu chỉnh và lập chỉ mục được thực hiện trên GPU trong một lần xử lý hợp nhất. Thông lượng cao nhất, nhưng cần nhiều VRAM nhất.
* **`tiled_gpu`** — Đường dẫn GPU tiết kiệm bộ nhớ. Xử lý hình ảnh theo các ô (tiles) để phù hợp với bộ nhớ GPU hạn chế. Thông lượng thấp hơn nhưng hoạt động trên các thiết bị có bộ nhớ hạn chế.
* **`cpu_fallback`** — Xử lý chỉ trên CPU bằng cách sử dụng song song đa luồng. Được sử dụng khi không có GPU NVIDIA và là phương án dự phòng cuối cùng khi cả hai đường dẫn GPU đều thất bại.

Chuỗi dự phòng trong thời gian chạy luôn là `fused_gpu` → `tiled_gpu` → `cpu_fallback`.

***

## Ghi đè chiến lược thủ công

Đặt biến môi trường `CHLOROS_STRATEGY` để ép buộc sử dụng một chiến lược cụ thể — đây là giải pháp dự phòng chuyên sâu dành cho trường hợp phát hiện tự động chọn một chiến lược không phù hợp với tình huống của bạn (ví dụ: giữ GPU trống để thực hiện các tác vụ khác):

```bash
# Valid values: CPU_PARALLEL, GPU_SINGLE, GPU_PARALLEL
CHLOROS_STRATEGY=CPU_PARALLEL chloros-cli process ~/datasets/flight001
```

Biến này được so khớp mà không phân biệt chữ hoa chữ thường; bất kỳ tên nào không nằm trong ba tên này sẽ bị bỏ qua và tính năng tự động phát hiện sẽ tiếp tục hoạt động bình thường. Khi áp dụng tùy chỉnh,Chloros
vẫn sẽ tự động chọn số lượng công nhân cho bạn:

| Tùy chỉnh | Số lượng công nhân được sử dụng |
| --- | --- |
| `CPU_PARALLEL` | `max(2, physical cores − 1)` |
| `GPU_SINGLE` | 3 |
| `GPU_PARALLEL` | `min(4, physical cores)` |

Nên thiết lập theo từng lệnh thay vì thiết lập vĩnh viễn, để các lần chạy thông thường tiếp tục tự động điều chỉnh.

***

## Hành vi cụ thể theo nền tảng

| Nền tảng | Chiến lược | Worker | Ống dẫn | Ghi chú |
| --- | --- | --- | --- | --- |
| **Jetson Orin Nano 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` (tuần tự) | Chế độ tiết kiệm bộ nhớ, xử lý từng hình ảnh một |
| **Jetson Orin NX 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` (tuần tự) | RAM chia sẻ dưới 12GB buộc phải xử lý tuần tự |
| **Jetson Orin NX 16GB** | `GPU_PARALLEL` | 2 | `fused_gpu` (đồng thời) | Thiết bị biên được khuyến nghị — Jetson giới hạn tối đa 2 công nhân |
| **Jetson AGX Orin 32–64GB** | `GPU_PARALLEL` | 2 | `fused_gpu` (đồng thời) | Hiệu suất biên tối đa (cũng bị giới hạn ở 2 worker trên Jetson) |
| **Máy tính để bàn với GPU 8GB** | `GPU_SINGLE` | 3 | `fused_gpu` / `tiled_gpu` | 3 worker chồng chéo I/O trong khi semaphore tuần tự hóa quyền truy cập GPU |
| **Máy tính để bàn với GPU 12GB+** | `GPU_PARALLEL` | 3-4 | `fused_gpu` (đồng thời) | Hiệu suất máy tính để bàn tối ưu: 12GB → 3 công nhân, 16GB+ → 4 |
| **Hệ thống chỉ dùng CPU** | `CPU_PARALLEL` | số lõi vật lý − 1 (tối thiểu 2) | `cpu_fallback` | Không cần GPU, sử dụng nhóm luồng |

{% hint style="info" %}
**Bộ nhớ thống nhất Jetson**: Các thiết bị Jetson chia sẻ bộ nhớ giữa GPU và CPU. Một thiết bị Jetson Orin NX 16GB báo cáo khoảng 15,3GB VRAM, nhưng đó chính là bộ nhớ RAM vật lý được hệ điều hành và các tiến trình CPU sử dụng. Đó là lý do tại sao các thiết bị Jetson có dung lượng 16GB trở lên đủ điều kiện cho `GPU_PARALLEL` giống như GPU máy tính để bàn 12GB trở lên, nhưng lại bị giới hạn ở 2 worker — GPU, các tiến trình worker và bối cảnh CUDA cho từng worker đều sử dụng chung cùng một bể bộ nhớ chia sẻ.
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

Các GPU rời chỉ dành 0,5GB cho hệ thống, vì chúng không chia sẻ RAM hệ thống. Các cấu hình Jetson dành nhiều dung lượng hơn và giới hạn thấp hơn — xem [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md#per-model-gpu-budget).

***

## Phân bổ bộ nhớ GPU động

Chloros
sử dụng [đường ống xử lý 4 luồng](processing-pipeline.md):

* **Luồng 1** (Phát hiện) — Tải hình ảnh, phân tích EXIF, phát hiện mục tiêu
* **Luồng 2** (Hiệu chuẩn) — Tính toán hiệu chuẩn phản xạ
* **Luồng 3** (Xử lý) — Loại bỏ hiệu ứng debayer bằng GPU, hiệu chỉnh hiện tượng tối góc, tính toán chỉ số
* **Luồng 4** (Xuất) — Ghi tệp, nhúng siêu dữ liệu

Các luồng 1, 2 và 4 tiêu tốn ít tài nguyên GPU; luồng 3 là luồng tiêu tốn nhiều nhất. Khi các luồng trước đó trong quy trình hoàn thành, ngân sách GPU của chúng sẽ được **phân bổ lại cho các luồng còn hoạt động**, do đó luồng 3 sẽ nhận được ngày càng nhiều bộ nhớ hơn khi quá trình xử lý tiến triển.

### Các giai đoạn phân bổ

| Giai đoạn | Các luồng đang hoạt động | Phân bổ bộ nhớ GPU |
| --- | --- | --- |
| **Giai đoạn đầu** | 1, 2, 3, 4 | Phân bổ đều cho tất cả các luồng, phần lớn dành cho luồng 3 |
| **Giữa-Sớm** | 2, 3, 4 | Phần của luồng 1 được phân phối lại |
| **Giữa-Muộn** | 3, 4 | Phần của các luồng 1 và 2 được chuyển sang 3 và 4 |
| **Giai đoạn cuối** | 3 hoặc 4 | Luồng hoạt động cuối cùng nhận được mức phân bổ tối đa |

Có hai quy tắc chi phối các con số này:

* Một luồng là luồng **duy nhất** đang hoạt động sẽ được cấp mức phân bổ tối đa theo cấu hình của nó.
* Khi có nhiều hơn một tác vụ GPU *nặng* đang hoạt động, mức phân bổ cơ bản của mỗi tác vụ nặng sẽ được chia đều cho các luồng đó (không bao giờ thấp hơn mức tối thiểu đã cấu hình).

Giá trị thực sự được sử dụng trong thời gian chạy là mức **thấp hơn** giữa mức phân bổ của cấu hình nền tảng và khuyến nghị thời gian thực từ trình giám sát bộ nhớ GPU, do đó một GPU đang bận rộn luôn được ưu tiên hơn so với một cấu hình quá lạc quan.***

## Xử lý Nhận biết Texture

Bộ khử răng cưa Texture Aware (**chỉ dành choChloros
trở lên** — `--debayer texture-aware`) chạy mô hình khử nhiễu AI/ML cần khoảng 1,75GB VRAM ở định dạng FP16 cho mỗi bản sao, do đó nó sử dụng nhiều bộ nhớ GPU hơn so với phương pháp Chuẩn:

* Các hệ thống có **dưới 7GB VRAM**xử lý Texture Aware trong một**vòng lặp đồng bộ, từng hình ảnh một** — không thể chứa nhiều bản sao mô hình, và việc sử dụng nhóm công nhân (worker pool) chỉ làm tăng xung đột
* Các hệ thống có **VRAM từ 7GB trở lên** có thể xử lý Texture Aware đồng thời, mặc dù số lượng worker sẽ ít hơn so với phương pháp Standard
* Trên **Jetson**, Texture Aware luôn được gán cố định cho một công nhân duy nhất, và trên các mẫu tiêu thụ điện năng thấp (Nano, Orin Nano), nó cũng tự động áp dụng giới hạn tần số GPU — xem [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md#gpu-frequency-cap-for-texture-aware-on-nano-and-orin-nano)***

## Quản lý nhiệt (Jetson)

Các thiết bị Jetson có các giới hạn về nhiệt, đặc biệt là trong các triển khai trong không gian kín hoặc trên không.Chloros
theo dõi các cảm biến nhiệt độ tích hợp trên Jetson và tự động điều chỉnh kích thước lô:

| Nhiệt độ | Phản ứng |
| --- | --- |
| **&lt; 70°C** | Hoạt động bình thường — tốc độ tối đa |
| **70°C** (Cảnh báo) | Kích thước lô được giảm dần (từ 100% xuống 50% trong khoảng từ 70°C đến 80°C) |
| **80°C** (Nguy hiểm) | Giảm tốc độ mạnh (từ 50% xuống 0% trong khoảng từ 80°C đến 90°C) |
| **90°C** (Tắt máy) | Dừng hoàn toàn quá trình xử lý GPU |

Trên các hệ thống máy tính để bàn có hệ thống làm mát đủ tốt, việc giới hạn nhiệt độ hiếm khi được kích hoạt.

***

## Xử lý áp lực bộ nhớ

Chloros
liên tục theo dõi bộ nhớ GPU trong quá trình xử lý và phản ứng ở ba mức độ.

**Định cỡ lô.** Một lô bắt đầu với 8 hình ảnh nhân với hệ số nhân của nền tảng từ các bảng trên.Chloros
sau đó kiểm tra VRAM trống, dành ra 20% trong đó cho chi phí hoạt động của PyTorch, và giả định khoảng 100MB bộ nhớ GPU cho mỗi hình ảnh 12MP — kích thước lô sẽ là giá trị nhỏ hơn giữa giới hạn tính từ bộ nhớ và giá trị cơ sở của nền tảng. Kích thước lô không bao giờ giảm xuống dưới 1.

**Giảm kích thước lô chủ động.**Khi**tỷ lệ sử dụng VRAM vượt quá 85%**, kích thước lô sẽ được giảm trước khi xảy ra lỗi.**Cắt giảm phân bổ theo luồng.** Khi tỷ lệ sử dụng thực tế tăng lên, ngân sách GPU của mỗi luồng sẽ được thu hẹp: ×0,75 khi tỷ lệ sử dụng vượt quá 80%, ×0,5 khi vượt quá 90%. Các ngưỡng giám sát là 70% (thận trọng), 85% (giới hạn hoạt động bình thường) và 95% (nguy cơ hết bộ nhớ).**Thoái lui và phục hồi khi hết bộ nhớ (OOM).** Nếu sự cố hết bộ nhớ vẫn xảy ra:

* kích thước lô sẽ bị **giảm một nửa**, và tiếp tục giảm một nửa nữa ở mỗi lần hết bộ nhớ liên tiếp — mỗi lô thành công tiếp theo sẽ giảm mức phạt này xuống một bậc
* việc phân bổ cho các luồng đang hoạt động sẽ bị cắt giảm xuống 70% so với giá trị hiện tại và bộ phân bổ sẽ chuyển sang chiến lược thận trọng, sau đó nới lỏng trở lại sau một chuỗi các lần phân bổ thành công
* khi chịu áp lực nghiêm trọng, đường ống xử lý sẽ chuyển từ `fused_gpu` sang `tiled_gpu`, và sang `cpu_fallback` như phương án cuối cùng

**Bộ nhớ RAM máy chủ (Jetson).** Trước khi xử lý, công cụ phân bổ bộ nhớ (CLI
) sẽ ước tính dung lượng bộ nhớ tối đa của máy chủ dựa trên số lượng hình ảnh và chế độ debayer, đồng thời cảnh báo nếu dung lượng RAM cộng với vùng hoán đổi dựa trên tệp có khả năng không đủ, đồng thời hiển thị các lệnh chính xác để thêm vùng hoán đổi — xem [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md#swap-warning-and-recommendations).

***

## Giám sát quá trình thích ứng tính toán

### Chẩn đoán hệ thống

`chloros-cli selftest` là cách nhanh nhất để xác nhận những gì lớp tính toán nhận diện được:

```bash
chloros-cli selftest
```

7 kiểm tra này bao gồm phiên bản, tính khả dụng của cổng, quá trình khởi động backend, `/api/test`, thông tin hệ thống, sự hiện diện của mô hình khử nhiễu và trạng thái sẵn sàng của CUDA cùng mô hình khử nhiễu. Kiểm tra số 5 hiển thị trực tiếp dòng thông tin phần cứng:

```
      GPU: NVIDIA RTX A4000, CUDA: True, PyTorch: 2.7.0
```

Kiểm tra 7 hiển thị `CUDA: <bool>, Denoiser: <bool>` — cả hai điều kiện này đều phải đúng thì Texture Aware mới có thể sử dụng được.

### Nhật ký backend

Chiến lược và số lượng công nhân (worker) được chọn bên trong backend khi bắt đầu mỗi lần chạy — không có thông báo nào trong phần “CLI
” để thông báo về chúng. Khi có sự cố bất thường xảy ra (đường dẫn GPU chuyển sang chế độ dự phòng, lỗi OOM, bộ lọc giảm nhiễu không thể tải), nhật ký backend của phiên đó sẽ hiển thị thông tin này:

| Nền tảng | Vị trí nhật ký |
| --- | --- |
| **Linux
/ Jetson** | `~/.cache/chloros/logs/backend_<YYYYMMDD_HHMMSS>.log` (một tệp cho mỗi lần khởi chạy) |
| **Linux
,CLI
-backend đã khởi động** | cũng là `~/.chloros/backend.log` |
| **Windows** | `%LOCALAPPDATA%\Chloros\logs\` |

### Tiến trình thời gian thực

Trong quá trình chạy,CLI
hiển thị tiến trình theo từng luồng (Phát hiện, Phân tích, Xử lý, Xuất) được truyền qua Server-Sent Events — đây là cách thực tế để xác định liệu Luồng 3 có phải là điểm nghẽn hay không. Xem [Đường ống xử lý](processing-pipeline.md).

***

## Các bước tiếp theo

* [Quy trình xử lý](processing-pipeline.md) — Hiểu về kiến trúc quy trình 4 luồng
* [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md) — Triển khai và tối ưu hóa dành riêng cho Jetson
* [CLI
: Command Line](../CLI.md) — Hướng dẫn về lệnhCLI

* [CLI
Reference](../reference/cli-reference.md) — Danh sách đầy đủ các lệnh cho phiên bản 1.2.0
