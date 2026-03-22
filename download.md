---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/download
---

# Tải xuống

Tải xuống phiên bản mới nhất của Chloros để bắt đầu xử lý hình ảnh đa phổ.

### Yêu cầu hệ thống

#### Windows

| Yêu cầu | Tối thiểu | Được đề xuất |
| -------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Hệ điều hành** | Windows 10 (64-bit) | Windows 11 (64-bit) |
| **Bộ xử lý** | Intel Core i5 hoặc tương đương | Intel Core i7 trở lên |
| **Bộ nhớ (RAM)** | 8GB | 16GB trở lên |
| **Card đồ họa** | Tương thích DirectX 11 | GPU NVIDIA với 4GB+ VRAM |
| **Lưu trữ** | Dung lượng trống 6GB | SSD có hơn 10GB dung lượng trống |
| **Hiển thị** | 1920x1080 | 2560x1440 trở lên |
| **Internet** | Cần thiết để kích hoạt giấy phép \[tùy chọn] Chloros+ | Cần thiết để kích hoạt giấy phép \[tùy chọn] Chloros+ |

#### Linux AMD64 (x86\_64)

| Yêu cầu | Tối thiểu | Được đề xuất |
| ----------------- | -------------------------- | ------------------------- |
| **Phân phối** | Ubuntu 20.04+ / Debian 11+ | Ubuntu 22.04+ |
| **Bộ xử lý** | x86\_64 (Intel/AMD) | Intel Core i7 trở lên |
| **Bộ nhớ (RAM)** | 8GB | 16GB trở lên |
| **Card đồ họa** | Không có (xử lý CPU) | GPU NVIDIA với 4GB+ VRAM |
| **Lưu trữ** | Dung lượng trống 2GB | SSD có 10GB+ miễn phí |
| **Python** | Python 3.7+ (dành cho SDK) | Python 3.10+ |

#### Linux arm64 (NVIDIA Jetson)

| Yêu cầu | Tối thiểu | Được đề xuất |
| ---------------- | ---------------------------- | ------------------------------- |
| **Nền tảng** | NVIDIA Jetson với JetPack 6 | Jetson Orin NX 16GB hoặc AGX Orin |
| **Bộ nhớ (RAM)** | 8GB (GPU/CPU dùng chung) | 16GB+ được chia sẻ |
| **Lưu trữ** | Dung lượng trống 2GB | SSD NVMe với 10GB+ miễn phí |
| **Python** | Python 3.7+ (dành cho SDK) | Python 3.10+ |

{% hint style="info" %}
**Tăng tốc GPU**: Người dùng Chloros+ có GPU NVIDIA có thể sử dụng khả năng tăng tốc CUDA để xử lý nhanh hơn đáng kể. Tính năng này hoạt động trên cả Windows (GPU máy tính để bàn) và Linux (GPU máy tính để bàn và NVIDIA Jetson). Người dùng Chloros+ cũng có thể xử lý đa luồng để có tốc độ tối đa.
{% endhint %}

***

## Tải xuống Chloros

###Bản phát hành ổn định mới nhất (23/03/2026): Phiên bản 1.1.0

### <a href="https://drive.google.com/uc?export=download&#x26;id=1HjwrUY4M7HGxDbMybO7iPe_6JoHnUGr4" class="button Primary">Tải xuống Chloros cho Windows (.exe)</a>

### <a href="https://drive.google.com/uc?export=download&#x26;id=1dB8-ke3wxNXpw_e1qJ4BhwBpCoNd4kLS" class="button Primary">Tải xuống Chloros cho Linux amd64 (.deb)</a>

### <a href="https://drive.google.com/uc?export=download&#x26;id=1d1OwdcYA4Rf4jkuPi2IBeWT2772_HnyO" class="button Primary">Tải xuống Chloros cho Linux arm64 / Jetson (.deb)</a>

#### Trình cài đặt Windows (GUI + CLI + Phần cuối)

* **Loại tệp**: .exe (Trình cài đặt Windows)**Các bước cài đặt:**

1. Tải xuống tệp .exe ở trên
2. Bấm đúp vào trình cài đặt để bắt đầu cài đặt
3. Làm theo lời nhắc của trình hướng dẫn cài đặt
4. Chọn thư mục cài đặt (mặc định: `C:\Program Files\[USER]\Chloros\`)
5. Hoàn tất cài đặt và khởi chạy Chloros hoặc Chloros CLI
6. Đăng nhập bằng [tài khoản MAPIR Cloud Chloros+](https://cloud.mapir.camera/pricing) (hoặc tiếp tục với phiên bản miễn phí)

{% hint style="success" %}
Trình cài đặt tự động thêm `chloros-cli` vào PATH hệ thống của bạn để truy cập dòng lệnh.
{% endhint %}

#### Linux amd64 (Gói .deb — CLI + Phần cuối)

* **Loại tệp**: .deb (gói Debian/Ubuntu)
* **Kiến trúc**: x86\_64 (AMD64)

```bash
sudo dpkg -i chloros-amd64.deb
chloros-cli --version  # Verify installation
```

#### Linux arm64 — NVIDIA Jetson (Gói .deb — CLI + Phần cuối)

* **Loại tệp**: .deb (JetPack 6)
* **Kiến trúc**: aarch64 (arm64)

```bash
sudo dpkg -i chloros-arm64-jp6.deb
chloros-cli --version  # Verify installation
```

Xem [Cài đặt Linux](linux/linux-installation.md) để biết hướng dẫn thiết lập chi tiết và [Hướng dẫn NVIDIA Jetson](linux/nvidia-jetson-guide.md) để biết hướng dẫn dành riêng cho Jetson.

#### Python SDK (Tất cả nền tảng)

```bash
pip install chloros-sdk
```

Xem [API : Python SDK](api-python-sdk.md) để biết tài liệu.

{% hint style="info" %}
**Người dùng Linux**: Gói `.deb` cài đặt CLI và chương trình phụ trợ. Python SDK được cài đặt riêng qua pip. Không có GUI cho Linux - tất cả tương tác đều thông qua CLI hoặc SDK.
{% endhint %}

***

## Tài nguyên bổ sung

### Python SDK

Đối với các nhà phát triển và quy trình tự động hóa, hãy cài đặt Chloros Python SDK:

```bash
pip install chloros-sdk
```

**Tài liệu**: [API: Python SDK](api-python-sdk.md)**Yêu cầu**: Phải cài đặt Chloros (trình cài đặt Windows hoặc gói Linux `.deb`), yêu cầu đăng nhập giấy phép Chloros+***

## Bao gồm những gì

### Trình cài đặt Windows

* ✅ **Chloros GUI** - Giao diện đồ họa đầy đủ tính năng
* ✅ **Chloros CLI** - Giao diện dòng lệnh (yêu cầu giấy phép Chloros+)
* ✅ **Phần cuối Chloros** - Công cụ xử lý
* ✅ **Hồ sơ máy ảnh** - Mẫu máy ảnh MAPIR được cấu hình sẵn

### Gói Linux .deb

* ✅ **Chloros CLI** - Giao diện dòng lệnh (yêu cầu giấy phép Chloros+)
* ✅ **Phần cuối Chloros** - Công cụ xử lý
* ✅ **Hồ sơ máy ảnh** - Mẫu máy ảnh MAPIR được cấu hình sẵn
* ❌ Không có GUI - Linux không có đầu chỉ CLI/SDK

### Python SDK (pip, tất cả nền tảng)

* ✅ **Chloros SDK** - Python API (yêu cầu giấy phép Chloros+)***

## Nâng cấp lên Chloros+

Mở khóa các tính năng nâng cao bằng đăng ký Chloros+:

* 🚀 **Xử lý đa luồng** - Xử lý hình ảnh song song
* ⚡ **Tăng tốc GPU (CUDA)** - Tận dụng sức mạnh GPU NVIDIA
* 💻 **Truy cập CLI** - Tự động hóa bằng các công cụ dòng lệnh
* 🐍 **Python SDK** - Truy cập API có lập trình
* 📱 **Nhiều thiết bị** - Sử dụng trên 2-10+ thiết bị (tùy thuộc vào gói)
* **🐻 Phương pháp gỡ lỗi nhận biết kết cấu nâng cao** - một công cụ gỡ lỗi nhận biết cạnh chất lượng cao kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như tất cả tiếng ồn gây nhiễu.
* 🧮 **Công thức tùy chỉnh** - Tạo các chỉ mục đa phổ tùy chỉnh

<palign="center"><a href="https://cloud.mapir.camera/pricing" class="button Primary">Xem các gói Chloros+ &#x26; Định giá</a></p>

***

## Trợ giúp cài đặt

### Khắc phục sự cố

**Cài đặt không thành công với thông báo lỗi:**

* Đảm bảo bạn có quyền quản trị viên
* Tạm thời vô hiệu hóa phần mềm diệt virus
* Kiểm tra xem bạn có đáp ứng các yêu cầu hệ thống tối thiểu không

**Ứng dụng không khởi động (Windows):**

* Xác minh Windows 10/11 (64-bit) đã được cài đặt
* Cập nhật trình điều khiển đồ họa
* Kiểm tra Trình xem sự kiện Windows để biết chi tiết lỗi
* Liên hệ hỗ trợ với nhật ký lỗi

**CLI không khởi động (Linux):**

* Xác minh gói `.deb` được cài đặt chính xác: `dpkg -l | grep chloros`
* Kiểm tra quyền: `sudo chmod +x /usr/bin/chloros-cli`
* Chạy chẩn đoán: `chloros-cli selftest`
* Kiểm tra các thư viện còn thiếu: `ldd /usr/lib/chloros/chloros-backend | grep "not found"`

**Vấn đề kích hoạt giấy phép:**

* Đảm bảo kết nối internet đang hoạt động
* Xác minh thông tin đăng nhập tại [https://cloud.mapir.camera](https://cloud.mapir.camera)
* Kiểm tra tường lửa không chặn Chloros
* Xem [Chloros+ Đăng nhập](chloros+-login.md) để biết hướng dẫn chi tiết

### Nhận hỗ trợ

Cần trợ giúp về cài đặt hoặc thiết lập?

* 📧 **Email**: info@mapir.camera
* 🌐 **Trang web**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Tài liệu**: [Bắt đầu](./)
* ❓ **Câu hỏi thường gặp**: [Câu hỏi thường gặp](faq.md)***

## Nhật ký thay đổi

<chi tiết>

<summary>Phiên bản 1.1.0 (Mới nhất)</summary>

**Ngày phát hành: Tháng 3 năm 2026**

**Tính năng mới*** **Hỗ trợ Linux** — CLI và SDK gốc dành cho Linux amd64 (x86\_64) và arm64 (NVIDIA Jetson JetPack 6). Cài đặt qua gói `.deb`.
* **Hỗ trợ NVIDIA Jetson** — Xử lý được tối ưu hóa cho các thiết bị Jetson Nano, Orin Nano, Orin NX và AGX Orin edge.
* **Thích ứng điện toán động** — Tối ưu hóa chiến lược xử lý và phát hiện phần cứng tự động. Chloros thích ứng với phần cứng của bạn từ Jetson Nano đến máy trạm đa GPU.
* **Đường ống xử lý 4 luồng** — Phát hiện, hiệu chỉnh, xử lý và xuất các luồng đồng thời với khả năng phân bổ bộ nhớ GPU động.
* **Các lệnh CLI mới** — `selftest` (chẩn đoán hệ thống) và `update` (quản lý cập nhật Linux).
* **Cờ xử lý CLI mới** — `--debayer` (tiêu chuẩn/nhận biết kết cấu), `--indices` (chỉ định chỉ mục), `--target` (thư mục con tìm kiếm mục tiêu trước để phát hiện nhanh hơn).
* **Các mục menu GUI mới** — Thêm tệp, thêm thư mục và Bắt đầu/Dừng xử lý hiện có thể truy cập được từ menu thả xuống chính.**Cải tiến**

* Tự động phát hiện phụ trợ đa nền tảng (đường dẫn Windows và Linux)
* SDK `get_status()` nâng cao với tính năng theo dõi tiến trình trên mỗi luồng
* Ngoại lệ SDK mới: `ChlorosConfigurationError`, `ChlorosAuthenticationError`
* Quản lý nhiệt và điều chỉnh thích ứng cho NVIDIA Jetson
* Quản lý bộ nhớ tự động với dự phòng OOM để xử lý GPU xếp lớp

</ chi tiết>

<chi tiết>

<tóm tắt>Phiên bản 1.0.5</tóm tắt>

**Ngày phát hành: 10 tháng 2 năm 2026**

**Tính năng mới*** **Phương pháp gỡ lỗi nhận biết kết cấu \[Chỉ Chloros+] -** Kết cấu Aware sử dụng trình gỡ lỗi nhận biết cạnh chất lượng cao kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ nhiễu gây nhiễu.
* **Hỗ trợ cho các mục tiêu hiệu chuẩn T4P*** **Xử lý GPU Chloros+ nhanh hơn, quản lý bộ nhớ tốt hơn**

**Sửa lỗi*** Giao diện người dùng (GUI) hoàn toàn mới, hiện sẽ hoạt động trên tất cả các máy tính Windows.

</ chi tiết>

<chi tiết>

<tóm tắt>Phiên bản 1.0.4</tóm tắt>

**Ngày phát hành: 5 tháng 1 năm 2026**

**Tính năng mới*** **Chuyển đổi hình ảnh/siêu dữ liệu**: Đã thêm nút chuyển đổi trong Trình duyệt tệp để xem siêu dữ liệu của hình ảnh đã chọn trong bảng thay vì lưới hình ảnh
* **Thanh trượt thu phóng lưới hình ảnh**: Thanh trượt giao diện người dùng mới để điều chỉnh kích thước hình thu nhỏ (cũng hỗ trợ CTRL + con lăn chuột)
* **Nút xuất lưới hình ảnh**: Các nút ở hàng trên cùng để chuyển hình thu nhỏ từ JPG sang xuất đã xử lý (Mục tiêu, Phản xạ, Chỉ mục, LUT)
* **Tab Bản đồ**: Bản đồ 2D tương tác mới hiển thị hình ảnh Điểm đánh dấu vị trí GPS
  * Hỗ trợ các ô bản đồ Google Maps và ESRI (tự động chọn dịch vụ ô tốt nhất dựa trên tính khả dụng của mức thu phóng)
  * Di chuột xem trước hình thu nhỏ trên điểm đánh dấu bản đồ

**Sửa lỗi*** Cải thiện hỗ trợ cài đặt Chloros trên các máy tính không nói tiếng Anh

</ chi tiết>

<chi tiết>

<tóm tắt>Phiên bản 1.0.3</tóm tắt>

**Ngày phát hành: 20 tháng 12 năm 2025**

**Tính năng mới*** Ra mắt lần đầu

**Cải tiến*** Ra mắt lần đầu

**Sửa lỗi*** Ra mắt lần đầu

**Vấn đề đã biết*** Ra mắt lần đầu

</ chi tiết>

***

## Thỏa thuận cấp phép**Phần mềm độc quyền** - Bản quyền (c) 2026 MAPIR Inc.

Việc sử dụng, phân phối hoặc sửa đổi trái phép đều bị cấm.

**Phiên bản miễn phí**: Có sẵn cho mục đích sử dụng cá nhân và thương mại với những hạn chế về tính năng**Chloros+**: Giấy phép dựa trên đăng ký cho các tính năng nâng cao và triển khai thương mại