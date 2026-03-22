# Xử lý đường ống

Chloros 1.1.0 sử dụng bộ xử lý đường ống 4 hoạt động như một dây leo nhanh theo giai đoạn. Mỗi quá trình xử lý luồng một giai đoạn riêng biệt của quá trình xử lý quy trình, được phép xử lý đồng thời nhiều hình ảnh ở các giai đoạn khác nhau.

***

##Đường ống kiến ​​trúc

```

Images In → [Thread 1: Detection] → [Thread 2: Calibration] → [Thread 3: Processing] → [Thread 4: Export] → Files Out
```

Mỗi hình ảnh được thực hiện theo thứ tự của cả bốn luồng. Với bộ xử lý đa luồng Chloros+, nhiều hình ảnh có thể ở các luồng khác cùng một lúc — trong khi Thread 3 xử lý một hình ảnh, Thread 1 có thể phát hiện hình ảnh tiếp theo, Thread 2 có thể hiệu chỉnh một hình ảnh khác và Thread 4 có thể ghi một hình ảnh đã được xử lý trước đó vào đĩa.

***

## Chi tiết chủ đề

### Chủ đề 1: Phát hiện

**Mục tiêu**: Tải hình ảnh và phát hiện tiêu chuẩn mục tiêu.

* Đọc các hình ảnh tập tin từ đĩa (RAW, JPG)
* Trích xuất EXIF ​​siêu dữ liệu (GPS, máy ảnh, dấu thời gian, độ phơi sáng)
* Phát hiện chuẩn hiệu tiêu chuẩn ArUco trong tiêu chuẩn hình ảnh được đánh dấu
* Đầu ra: hình ảnh dữ liệu + siêu dữ liệu + mục tiêu phát hiện kết quả

Đây chủ yếu là một I/O và CPU liên kết luồng.

### Chủ đề 2: Hiệu chuẩn

**Mục tiêu**: Tính toán các tiêu chuẩn thông số từ các mục tiêu được phát hiện.

* Tính toán hệ số điều chỉnh phản xạ từ tiêu điểm hình ảnh
* Tính toán các chi tiết điều chỉnh tham số
* Xác định chuẩn đường truyền định nghĩa trên mỗi băng tần
* Đầu ra: chỉnh sửa thông số cho từng hình ảnh

Đây là một CPU giới hạn tính toán luồng.

### Chủ đề 3: Xử lý (GPU)

**Mục đích**: Áp dụng các hiệu chỉnh và tính toán các thực vật chỉ số.**Đây là chủ đề Đòi hỏi nhiều tính toán nhất.*** **Debayering**: Chuyển đổi dữ liệu mẫu RAW Bayer thành hình ảnh đa kênh
  * Tiêu chuẩn (Chất lượng nhanh, trung bình) - mặc định
  * Nhận cấu hình đã biết (Chậm, Chất lượng cao nhất) — chỉ Chloros+, sử dụng tính năng khử nhiễu AI/ML
* **Chỉ họa tiết**: Áp dụng hiệu chỉnh ống kính chi tiết trên toàn bộ hình ảnh
* **Hiệu chuẩn phản xạ**: Áp dụng hệ thống chuẩn số để chuyển đổi thành phản xạ giá trị
* **Tính toán chỉ số**: Tính toán các chỉ số thực vật (NDVI, NDRE, GNDVI, v.v.)
* Đầu ra: hình ảnh dữ liệu đã được xử lý sẵn sàng để xuất

Chủ đề này được hưởng nhiều lợi ích nhất nhờ GPU tăng tốc độ khả năng. Hệ thống [Thích ứng điện toán động](dynamic-compute-adaptation.md) chủ yếu tối ưu hóa hoạt động của luồng này.

### Chủ đề 4: Xuất khẩu

**Mục đích**: Ghi hình ảnh đã được xử lý vào đĩa.

* Ghi các tệp đầu ra ở dạng đã chọn (TIFF 16-bit, TIFF 32-bit %, PNG, JPG)
* Nhúng EXIF ​​hyperdata vào đầu tệp ra (GPS, dấu thời gian, tham số xử lý)
* Sắp xếp đầu ra vào các loại máy ảnh thư mục
* Đầu ra: tập tin cuối cùng trên đĩa

Đây chủ yếu là một I/O giới hạn luồng. Bộ lưu trữ SSD cải thiện đáng kể hiệu suất của Thread 4.

***

## Xử lý tuần tự tương tự như xử lý theo đường ống

### Chế độ miễn phí (Tuần tự)

Trong bản miễn phí của Chloros, hình ảnh được xử lý **ngừng hình ảnh phiên bản**, tuần tự qua cả hai giai đoạn:

```

Image 1: [Detect] → [Calibrate] → [Process] → [Export]
                                                         Image 2: [Detect] → [Calibrate] → [Process] → [Export]
```

GUI tiến trình này hiển thị 2 giai đoạn: Phát hiện và xử lý tiêu điểm.

### Chế độ Chloros+ (Đường ống)

Với giấy phép Chloros+, cả bốn luồng đều hoạt động **đồng thời** trên các hình ảnh khác nhau:

```

Thread 1: [Image 1] [Image 2] [Image 3] [Image 4] ...
Thread 2:           [Image 1] [Image 2] [Image 3] ...
Thread 3:                     [Image 1] [Image 2] ...
Thread 4:                               [Image 1] ...
```

GUI tiến trình hiển thị 4 giai đoạn: Phát hiện, Phân tích, Hiệu chỉnh, Xuất. Di chuột qua thanh tiến trình để xem tiến trình trên mỗi luồng.

{% hint style="success" %}
**Xử lý đường dẫn với Chloros++** có thể nhanh hơn 3-5 lần so với quy trình xử lý tuần tự, tùy thuộc vào phần cứng và dữ liệu kích thước của bạn. Tốc độ tăng nhanh nhất trên hệ thống có GPU và SSD nhanh hơn.
{% endhint %}

***

## Thread xuất cấp độ 4

Trong Chloros 1.1.0, luồng xuất (Chủ đề 4) có tính năng theo dõi chuyên dụng tiến trình. Bạn có thể theo dõi tiến trình xuất riêng biệt:**CLI:**
```bash
chloros-cli export-status
```

**SDK:**
```python
status = chloros.get_status()
print(f"Export: {status['export']['percent']}% - Phase: {status['export']['phase']}")
```

Quá trình xử lý hoàn tất khi Thread 4 đạt 100%.

***

## Mối quan hệ với khả năng thích ứng tính toán

Hệ thống [Thích ứng điện toán động](dynamic-compute-adaptation.md) chủ yếu ảnh hưởng đến **Chủ đề 3 (Đang xử lý)**:

* **Chiến lược `GPU_PARALLEL`**: Thread 3 chạy đồng thời nhiều hình ảnh thông qua GPU bằng cách sử dụng đường dẫn `fused_gpu`
* **Chiến lược `GPU_SINGLE`**: Thread 3 xử lý mỗi lần một hình ảnh bằng cách sử dụng quy trình `tiled_gpu` tiết kiệm bộ nhớ
* **Chiến lược `CPU_PARALLEL`**: Thread 3 sử dụng bộ xử lý dựa trên CPU với tính năng bài hát đa luồng

Việc phân tích GPU bổ sung của Chủ đề 3 cũng thay đổi hoạt động khi Chủ đề 1 và 2 hoàn thành thành công — xem [Phân tích GPU bổ sung bộ nhớ](dynamic-compute-adaptation.md#dynamic-gpu-memory-allocation).

***

## Các bước tiếp theo

* [Điều chỉnh điện toán động](dynamic-compute-adaptation.md) — Cách Chloros lựa chọn chiến lược tối ưu cho phần cứng của bạn
* [NVIDIA Jetson Guide](../linux/nvidia-jetson-guide.md) — Hành động dành riêng cho đường dẫn cho nền tảng trên Jetson
* [Giám sát quá trình xử lý](../processing-images-gui/monitoring-the-processing.md) - giám sát quá trình GUI