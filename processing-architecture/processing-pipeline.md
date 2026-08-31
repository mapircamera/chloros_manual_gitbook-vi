# Quy trình xử lý

ChlorosPhiên bản 1.2.0 sử dụng quy trình xử lý gồm 4 luồng, hoạt động giống như một dây chuyền lắp ráp theo từng giai đoạn. Mỗi luồng xử lý một giai đoạn riêng biệt trong quy trình làm việc, do đó nhiều hình ảnh có thể đang được xử lý ở các giai đoạn khác nhau cùng một lúc.

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

***

## Kiến trúc quy trình

```

Images In → [Thread 1: Detection] → [Thread 2: Calibration] → [Thread 3: Processing] → [Thread 4: Export] → Files Out
```

Mỗi hình ảnh sẽ lần lượt đi qua cả bốn luồng theo thứ tự. Với cơ chế xử lý đa luồng của Chloros+, nhiều hình ảnh có thể được xử lý đồng thời trên các luồng khác nhau — trong khi Luồng 3 đang xử lý một hình ảnh, Luồng 1 có thể đang phát hiện hình ảnh tiếp theo, Luồng 2 đang hiệu chỉnh một hình ảnh khác, và Luồng 4 đang ghi hình ảnh đã hoàn thành lên đĩa.

Tiến trình được báo cáo theo từng luồng và truyền qua Server-Sent Events (hệ thống backend xuất bản chúng trên `/api/events`). Trên màn hình hiển thị tiến trình trực tiếp của CLI, bốn giai đoạn được đánh dấu là **Phát hiện, Phân tích, Xử lý, Xuất**.***

## Chi tiết luồng

### Luồng 1: Phát hiện

**Mục đích**: Tải hình ảnh và phát hiện các mục tiêu hiệu chuẩn.

* Đọc các tệp hình ảnh từ đĩa — cặp tệp Survey3 `.raw`+`.jpg`, các ảnh chụp LATTICE `.tif`/`.tiff`, và `.dng`
* Trích xuất siêu dữ liệu EXIF (GPS, mẫu máy ảnh, dấu thời gian, độ phơi sáng)
* Phát hiện các mục tiêu hiệu chuẩn: hình học mục tiêu được đánh dấu ArUco cho các ảnh chụp từ LATTICE, và bảng cảm biến cổ điển cho các ảnh mục tiêu hiệu chuẩn từ Survey3
* Kết quả đầu ra: dữ liệu hình ảnh + siêu dữ liệu + kết quả phát hiện mục tiêu

Chủ yếu là luồng liên quan đến I/O và CPU.

### Luồng 2: Hiệu chuẩn

**Mục đích**: Tính toán các tham số hiệu chuẩn từ các mục tiêu đã phát hiện.

* Tính toán hệ số hiệu chuẩn phản xạ từ hình ảnh mục tiêu
* Tính toán các thông số hiệu chỉnh hiện tượng tối góc
* Xác định các đường cong hiệu chuẩn cho từng dải phổ
* Đầu ra: các thông số hiệu chuẩn cho từng hình ảnh

Một luồng tính toán phụ thuộc vào CPU. Luồng 3 sẽ chờ luồng này khi tính năng hiệu chuẩn phản xạ được bật, để các hệ số hiệu chuẩn sẵn sàng trước khi bất kỳ hình ảnh nào được xử lý.

### Luồng 3: Xử lý (GPU)

**Mục đích**: Áp dụng các hiệu chỉnh và tính toán các chỉ số thực vật.**Đây là luồng tiêu tốn nhiều tài nguyên tính toán nhất.*** **Debayering**: chuyển đổi dữ liệu RAW Bayer thành hình ảnh đa kênh
  * Tiêu chuẩn (Nhanh, Chất lượng trung bình) — mặc định, `--debayer standard`
  * Nhận diện kết cấu (Chậm, Chất lượng cao nhất) — chỉ dành cho Chloros+, `--debayer texture-aware`, sử dụng mô hình khử nhiễu AI/ML
  * Các ảnh chụp LATTICE mono (M3M) là ảnh đơn dải: các bước khử môzaic và cân bằng trắng sẽ bị bỏ qua đối với chúng (kèm theo một dòng thông báo nhật ký), trong khi bất kỳ ảnh M3C/Bayer nào trong cùng một đợt chụp vẫn được thực hiện các bước này
* **Chỉnh sửa hiện tượng tối góc**: áp dụng chỉnh sửa hiện tượng tối góc của ống kính trên toàn bộ hình ảnh
* **Hiệu chuẩn phản xạ**: áp dụng các hệ số hiệu chuẩn để chuyển đổi sang giá trị phản xạ
* **Tính toán chỉ số**: tính toán các chỉ số thực vật (NDVI, NDRE, GNDVI, …)
* Đầu ra: dữ liệu hình ảnh đã xử lý sẵn sàng để xuất

Luồng này được hưởng lợi nhiều nhất từ khả năng tăng tốc GPU, và đây chính là luồng mà [Dynamic Compute Adaptation](dynamic-compute-adaptation.md) điều chỉnh.

### Luồng 4: Xuất

**Mục đích**: Ghi hình ảnh đã xử lý ra đĩa.

* Ghi các tệp đầu ra theo định dạng đã chọn — `TIFF (16-bit)`, `TIFF (32-bit, Percent)`, `PNG (8-bit)`, `JPG (8-bit)`
* Nhúng siêu dữ liệu vào các tệp đầu ra (GPS, dấu thời gian, thông số xử lý)
* Sắp xếp các tệp đầu ra trong thư mục dự án theo định dạng `<camera>/<format>/<Product>_Images/` — ví dụ: `LATT-M3M-L41-F550/tiff16/Reflectance_Calibrated_Images/`. **Các tệp xuất ra giữ nguyên tên tệp nguồn; thư mục xác định sản phẩm.**
* Đối với các bản chụp LATTICE, một khung hình nguồn có thể được chia thành nhiều sản phẩm (Debayered, Preview, Radiance, Reflectance, Index), mỗi sản phẩm nằm trong thư mục riêng của mình
* Kết quả đầu ra: các tệp cuối cùng trên đĩa

Chủ yếu là luồng bị giới hạn bởi I/O — lưu trữ SSD cải thiện hiệu suất đáng kể.

***

## Cơ chế hoạt động: Các trình thực thi

Trong luồng 3, công việc trên từng hình ảnh được song song hóa bằng các trình thực thi tiêu chuẩn của Python: `concurrent.futures`:

* **Chiến lược GPU**(`GPU_SINGLE`, `GPU_PARALLEL`) sử dụng phương thức**spawn** — mỗi công nhân là một tiến trình riêng biệt với bối cảnh CUDA riêng (`fork` sẽ kế thừa trạng thái CUDA đã được khởi tạo của tiến trình cha và làm hỏng các tiến trình con)
* **`CPU_PARALLEL`** sử dụng `ThreadPoolExecutor` — NumPy và OpenCV giải phóng GIL, do đó chỉ cần các luồng là đủ
* Các thiết bị Jetson có RAM chia sẻ 8GB trở xuống sẽ bỏ qua hoàn toàn trình thực thi và xử lý trong cùng một tiến trình, theo thứ tự tuần tự
* Chế độ Texture Aware trên GPU có VRAM dưới 7GB cũng chạy theo thứ tự tuần tự — mô hình khử nhiễu không thể được tải vào hơn một lần

Chloroskhông sử dụng bất kỳ khung công tác phân tán của bên thứ ba nào (chẳng hạn như Ray). Xem [Dynamic Compute Adaptation](dynamic-compute-adaptation.md) để biết cách lựa chọn chiến lược và số lượng worker.

***

## Xử lý tuần tự so với xử lý theo đường ống

### Chế độ Tự do (Tuần tự)

Trong phiên bản miễn phí của Chloros, hình ảnh được xử lý **mỗi lần một hình**, tuần tự qua tất cả bốn giai đoạn:

```

Image 1: [Detect] → [Calibrate] → [Process] → [Export]
                                                         Image 2: [Detect] → [Calibrate] → [Process] → [Export]
```

Giao diện người dùng (GUI) hiển thị thanh tiến trình đơn giản hóa trong chế độ miễn phí; các giai đoạn tuần tự được hiển thị là **Phát hiện mục tiêu**và sau đó là**Xử lý**.

### Chế độ Chloros+ (Xử lý theo luồng)

Với giấy phép Chloros+, cả bốn luồng hoạt động **đồng thời** trên các hình ảnh khác nhau:

```

Thread 1: [Image 1] [Image 2] [Image 3] [Image 4] ...
Thread 2:           [Image 1] [Image 2] [Image 3] ...
Thread 3:                     [Image 1] [Image 2] ...
Thread 4:                               [Image 1] ...
```

Thanh tiến trình trên giao diện người dùng (GUI) hiển thị bốn giai đoạn; di chuột qua thanh này để xem tiến trình của từng luồng. Trong trình phát trực tiếp (CLI), bốn giai đoạn này được phát trực tiếp dưới dạng **Phát hiện, Phân tích, Xử lý, Xuất**.

{% hint style="info" %}
**Một nhãn, hai tên.** Giao diện người dùng (CLI) gọi giai đoạn 3 là _Processing_. Dòng tiến trình chế độ cao cấp của hệ thống nền — dòng mà thanh tiến trình trên GUI hiển thị — lại gọi cùng giai đoạn đó là _Calibrating_. Đây là cùng một luồng đang thực hiện cùng một công việc (Luồng 3: debayer, hiệu chỉnh, chỉ số).
{% endhint %}

{% hint style="success" %}
**Xử lý theo luồng (pipelined processing) với Chloros+** có thể nhanh hơn 3-5 lần so với xử lý tuần tự, tùy thuộc vào phần cứng và kích thước tập dữ liệu của bạn. Tốc độ tăng cao nhất trên các hệ thống có GPU và SSD nhanh.
{% endhint %}

***

## Tiến trình xuất của luồng 4

Luồng xuất có hệ thống theo dõi tiến trình riêng, mà bạn có thể kiểm tra riêng biệt:**CLI:**

```bash
chloros-cli export-status
```

**SDK:**

```python
status = chloros.get_status()
print(f"Export: {status['export']['percent']}% - Phase: {status['export']['phase']}")
```

Quá trình xử lý hoàn tất khi luồng 4 đạt 100%.

{% hint style="info" %}
**Một lần chạy không ghi được hình ảnh nào được coi là thất bại.**Khi thành công, `chloros-cli process` sẽ báo cáo số lượng sản phẩm hình ảnh đã ghi (`Image products written: N`). Nếu có yêu cầu sản phẩm nhưng**không có**sản phẩm nào được ghi — chỉ có `project.json` và `calibration_data.json` — thì CLI sẽ in ra `Processing finished but wrote no image products.` và**thoát với giá trị không bằng 0**, đồng thời nêu tên thư mục dự án và các nguyên nhân thường gặp (thư mục đầu vào không được nhận diện là vùng ghi — hãy kiểm tra bố cục và `--input-level` — hoặc tất cả các sản phẩm được yêu cầu đều không áp dụng được cho các camera đó). Các tập lệnh có thể dựa vào mã thoát.
{% endhint %}

***

## Mối quan hệ với Tính năng Điều chỉnh Tính toán Động

[Tính năng Điều chỉnh Tính toán Động](dynamic-compute-adaptation.md) chủ yếu ảnh hưởng đến **Luồng 3 (Xử lý)**:

* **`GPU_PARALLEL`**: Luồng 3 xử lý đồng thời nhiều hình ảnh qua GPU bằng cách sử dụng đường ống `fused_gpu`
* **`GPU_SINGLE`**: Luồng 3 tuần tự hóa việc truy cập GPU bằng semaphore trong khi các tiến trình công việc thực hiện I/O chồng chéo, sử dụng `fused_gpu` hoặc đường ống `tiled_gpu` tiết kiệm bộ nhớ
* **`CPU_PARALLEL`**: Luồng 3 sử dụng xử lý dựa trên CPU với song song đa luồng

Việc phân bổ bộ nhớ GPU của luồng 3 cũng tăng lên khi các luồng 1 và 2 hoàn thành — xem [Phân bổ bộ nhớ GPU động](dynamic-compute-adaptation.md#dynamic-gpu-memory-allocation).

***

## Các bước tiếp theo

* [Thích ứng tính toán động](dynamic-compute-adaptation.md) — Cách Chloros chọn chiến lược tối ưu cho phần cứng của bạn
* [Hướng dẫn NVIDIA Jetson](../linux/nvidia-jetson-guide.md) — Hành vi đường ống xử lý cụ thể cho nền tảng trên Jetson
* [Giám sát quá trình xử lý](../processing-images-gui/monitoring-the-processing.md) — Giám sát tiến trình qua giao diện người dùng (GUI)
* [Tham khảo CLI](../reference/cli-reference.md) — `process`, `export-status`, mã thoát và bố cục đầu ra
