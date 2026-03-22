# Thích ứng tính toán

Chloros 1.1.0 giới thiệu lựa chọn chiến lược xử lý tự động và phát hiện phần cứng thông minh. Công cụ xử lý thích hợp với phần cứng của bạn — từ Jetson Nano đến máy trạm đa GPU — mà không cần bất kỳ cấu hình thủ công nào.

***

## Nó hoạt động như thế nào

Khi Chloros khởi động, nó sẽ tự động cấu hình hệ thống của bạn:

1. **Phát hiện hệ điều hành** — Windows hoặc Linux
2. **Xác định CPU lõi và tổng RAM**

3.**Phát hiện diện của GPU** — Khả năng NVIDIA CUDA, VRAM, loại máy chủ
4. **Xác định loại máy Jetson** (nếu có) - qua `/proc/device-tree/model`
5. **Kiểm tra cảm biến nhiệt** (Jetson) — để xử lý nhận biết nhiệt độ
6. **Chọn tối ưu hóa chiến lược điện** — dựa trên tất cả phần cứng được phát hiện
7. **Tự động định cấu hình số lượng nhân viên, loại quy trình và bộ nhớ bổ sung**Kết quả được lưu vào bộ nhớ đệm để các lần chạy tiếp theo bắt đầu nhanh hơn. Nếu thay đổi phần cứng (ví dụ: thêm GPU), Chloros sẽ cấu hình lại lần khởi động tiếp theo.***

##Chiến lược tính toán

Chloros chọn một trong ba chiến lược điện toán dựa trên phần cứng của bạn:

| Chiến lược | GPU yêu cầu | Công nhân | Đường ống | Tốt nhất cho |
| --- | --- | --- | --- | --- |
| **`GPU_PARALLEL`** | Có (12GB+ VRAM hoặc 16GB+ chia sẻ) | 3-4 | `fused_gpu` | GPU máy tính để bàn có 12GB+, Jetson Orin NX 16GB, AGX Orin |
| **`GPU_SINGLE`** | Có (< 12GB VRAM) | 1-3 | `tiled_gpu` | GPU cấp đầu vào, Jetson Nano, Orin Nano |
| **`CPU_PARALLEL`** | Không | lõi - 1 | `cpu_fallback` | System no GPU NVIDIA |

### Các loại đường ống

* **`fused_gpu`** — Đầy đủ GPU xử lý đường dẫn. Tất cả các lỗi gỡ lỗi, sửa lỗi và cài đặt đều chạy trên GPU trong thời gian hợp lý nhất. Thông lượng cao nhất nhưng yêu cầu nhiều VRAM hơn.
* **`tiled_gpu`** — GPU tiết kiệm bộ nhớ đường dẫn. Xử lý hình ảnh từng ô để phù hợp với chế độ giới hạn GPU bộ nhớ. Thông lượng thấp hơn nhưng vẫn hoạt động trên các thiết bị có chế độ giới hạn bộ nhớ.
* **`cpu_fallback`**— Xử lý CPU chỉ bằng cách sử dụng đa luồng bài hát. Được sử dụng khi không có GPU NVIDIA.***

## Hành động dành riêng cho nền tảng

| Nền tảng | Chiến lược | Công nhân | Đường ống | Ghi chú |
| --- | --- | --- | --- | --- |
| **Jetson Nano 8GB** | `GPU_SINGLE` | 1 | `tiled_gpu` (tự động hóa tuần) | Chế độ tiết kiệm bộ nhớ, xử lý từng hình ảnh một |
| **Jetson Orin NX 16GB** | `GPU_PARALLEL` | 3 | `fused_gpu` (đồng thời) | Thiết bị biên dịch được xuất - xử lý bài hát GPU thực sự |
| **Jetson AGX Orin 64GB** | `GPU_PARALLEL` | 4 | `fused_gpu` (đồng thời) | Performance tối đa viền |
| **Máy tính để bàn có GPU 8GB** | `GPU_SINGLE` | 3 | `tiled_gpu` | Hiệu suất máy tính để bàn tốt với các ô tiết kiệm bộ nhớ |
| **Máy tính để bàn có GPU 12GB+** | `GPU_PARALLEL` | 3-4 | `fused_gpu` | Performance máy tính để bàn tối ưu |
| **Hệ thống CPU chỉ sử dụng** | `CPU_PARALLEL` | lõi - 1 | `cpu_fallback` | Không cần GPU, hãy sử dụng ThreadPool |

{% hint style="info" %}
**Jetson hợp nhất bộ nhớ**: Các thiết bị Jetson chia sẻ bộ nhớ GPU và CPU. Báo cáo Jetson Orin NX 16GB ~ 15,3 GB VRAM, nhưng đây là một RAM tương tự được sử dụng bởi hệ điều hành Windows và CPU. Chloros giải quyết vấn đề này khi thiết lập bộ nhớ bổ sung ngưỡng.
{% endhint %}

***

## Phân chia GPU động bộ nhớ bổ sung

Chloros sử dụng [luồng xử lý đường dẫn 4](processing-pipeline.md):

* **Chủ đề 1** (Phát hiện) — Tải hình ảnh, EXIF ​​cú pháp phân tích, phát hiện mục tiêu
* **Luồng 2** (Hiệu chỉnh) — Tính toán hiệu chuẩn Phản xạ
* **Chủ đề 3** (Đang xử lý) — GPU gỡ lỗi trình duyệt, hiệu chỉnh chi tiết, chỉ số tính toán
* **Chủ đề 4** (Xuất) — Ghi tệp, nhúng siêu dữ liệu

Khi chúng tôi hoàn thành công việc của các luồng quy trình trước đó (ví dụ: tất cả hình ảnh đã được phát hiện), GPU bổ sung bộ nhớ của chúng tôi sẽ được giải nén và **được phân phối lại cho các hoạt động luồng còn lại**. Điều này có nghĩa là Chủ đề 3 (giai đoạn sử dụng nhiều GPU) sẽ nhận được nhiều bộ nhớ hơn khi quy trình phát triển, cải thiện chất lượng cho công việc Yêu cầu nhiều tính toán nhất.

### Bổ sung phân đoạn giai đoạn

| Sân khấu | Chủ đề hoạt động | GPU phân phối bộ nhớ |
| --- | --- | --- |
| **Sớm** | 1, 2, 3, 4 | Chia thành tất cả các chủ đề |
| **Trung tâm** | 2, 3, 4 | Chủ đề 1 bộ nhớ được phân phối lại |
| **Giữa-Cuối** | 3, 4 | Chủ đề 1+2 bộ nhớ chuyển tới 3+4 |
| **Muộn**| 3 hoặc 4 | Tối đa bộ nhớ cho chuỗi còn lại |***

## Xử lý cấu hình nhận dạng

Phương pháp gỡ lỗi nhận biết cấu hình (chỉ Chloros+) use GPU bộ nhớ nhiều hơn đáng kể so với phương pháp tiêu chuẩn do mô hình khử nhiễu AI/ML:

* Hệ thống có **< 7GB VRAM** buộc phải được đưa vào đồng bộ xử lý vòng cho chế độ Nhận cấu hình đã biết (mỗi lần một hình ảnh)
* Hệ thống có **7GB+ VRAM**có thể xử lý đồng thời kết cấu Aware, mặc định giảm số lượng nhân viên so với tiêu chuẩn***

## Quản lý nhiệt độ (Jetson)

Các thiết bị Jetson có những hạn chế về nhiệt độ, đặc biệt là khi phát triển trong phạm vi kín hoặc trên không. Chloros theo dõi nhiệt độ GPU và CPU và điều chỉnh tự động trong quá trình xử lý:

| Nhiệt độ | Phản hồi |
| --- | --- |
| *****70°C** | Hoạt động bình thường - tốc độ tối đa |
| **70°C** (Cảnh báo) | Lô kích thước nhỏ |
| **80°C** (Nghiêm trọng) | Điều chỉnh mạnh mẽ — số lượng nhân viên và đồng thời thấp hơn |
| **90°C** (Tắt máy) | Dừng hoàn toàn quá trình xử lý GPU |

Giám sát nhiệt độ sử dụng `tegrastats` trên nền tảng Jetson. Trên các hệ thống máy tính có khả năng làm mát phù hợp, điều tiết nhiệt độ khi được kích hoạt.

***

## Bộ nhớ áp dụng xử lý

Chloros hệ thống bộ nhớ giám sát giám sát trong quá trình xử lý:

* **Ngưỡng bộ nhớ**: Mức sử dụng 85% Kích hoạt vi tính cẩn thận
* **Giảm OOM**: Nếu xảy ra sự cố hết bộ nhớ, phân tích sẽ giảm 25% (hệ số 0,75x)
* **Dự phòng đường ống**: dưới áp dụng sức mạnh nghiêm trọng, đường ống sẽ tự động chuyển từ `fused_gpu` xuống `tiled_gpu`
* **Đề xuất sản phẩm đổi**: Trên Jetson, Chloros cảnh báo bạn nếu dung lượng sản phẩm không đủ cho dữ liệu kích thước của bạn***

## Giám sát công việc thích hợp tính toán

### Đầu ra trạng thái CLI

Khi quá trình xử lý bắt đầu, CLI hiển thị phần cứng cấu hình được phát hiện:

```

Chloros CLI 1.1.0
Platform: Linux aarch64 (Jetson Orin NX 16GB)
Strategy: GPU_PARALLEL | Workers: 3 | Pipeline: fused_gpu
CUDA: Available | GPU Memory: 15.3 GB (shared)
```

### Chẩn đoán hệ thống

Chạy `chloros-cli selftest` để xem đầy đủ phần cứng cấu hình và khả năng tính toán xác thực:

```bash
chloros-cli selftest
```

Công việc này sẽ kiểm tra khả năng sử dụng của CUDA, GPU bộ nhớ, nhiễu nhiễu mô hình và hỗ trợ kết nối.

***

## Các bước tiếp theo

* [Xử lý đường ống](processing-pipeline.md) — Tìm hiểu cấu trúc đường ống 4 luồng
* [ Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md) — Triển khai và dành riêng cho tối ưu cho Jetson
* [CLI : Dòng lệnh](../CLI.md) — Tham khảo đầy đủ về CLI