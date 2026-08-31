---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/download
---

# Tải xuống

Tải xuống phiên bản mới nhất củaChloros

để bắt đầu xử lý hình ảnh đa quang phổ.

### Yêu cầu hệ thống

####Windows



| Yêu cầu          | Tối thiểu                                              | Được khuyến nghị                                          |
| -------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| **Hệ điều hành** | Windows 10 (64-bit)                                  | Windows 11 (64-bit)                                  |
| **Bộ xử lý**        | Intel Core i5 hoặc tương đương                          | Intel Core i7 hoặc cao hơn                              |
| **Bộ nhớ (RAM)**     | 8GB                                                  | 16GB trở lên                                         |
| **Card đồ họa**    | Tương thích DirectX 11                                | GPU NVIDIA với VRAM 4GB trở lên                            |
| **Dung lượng lưu trữ**          | 6GB dung lượng trống                                       | SSD với 10GB+ dung lượng trống                            |
| **Màn hình**          | 1920x1080                                            | 2560x1440 trở lên                                  |
| **Internet**         | Cần thiết để kích hoạt giấy phép \[tùy chọn\]Chloros

trở lên | Cần thiết để kích hoạt giấy phép \[tùy chọn\]Chloros

trở lên |

####Linux

amd64 (x86\_64)

| Yêu cầu       | Tối thiểu                    | Đề xuất               |
| ----------------- | -------------------------- | ------------------------- |
| **Hệ điều hành**  | Ubuntu 22.04 LTS+ / Debian 12+ | Ubuntu 24.04 LTS      |
| **Bộ xử lý**     | x86\_64 (Intel/AMD)        | Intel Core i7 trở lên   |
| **Bộ nhớ (RAM)**  | 8GB                        | 16GB trở lên              |
| **Card đồ họa** | Không cần (xử lý bằng CPU)      | GPU NVIDIA với 4GB+ VRAM |
| **Dung lượng lưu trữ**       | 2GB dung lượng trống             | SSD có ít nhất 10GB dung lượng trống       |
| **Hệ điều hànhPython**        |Python

3.7+ (dành choSDK

)      |Python

3.10+              |

####Linux

arm64 (NVIDIA Jetson)

| Yêu cầu      | Tối thiểu                      | Khuyến nghị                     |
| ---------------- | ---------------------------- | ------------------------------- |
| **Nền tảng**     | NVIDIA Jetson với JetPack 6 | Jetson Orin NX 16GB hoặc AGX Orin |
| **Bộ nhớ (RAM)** | 8GB (chia sẻ giữa GPU và CPU)         | 16GB+ chia sẻ                    |
| **Dung lượng lưu trữ** | 2GB dung lượng trống               | SSD NVMe với 10GB+ dung lượng trống        |
| **Hệ điều hànhPython**       |Python

3.7 trở lên (dành choSDK

)        |Python

3.10 trở lên                    |

{% hint style="info" %}
**Tăng tốc GPU**: Người dùngChloros

trở lên có GPU NVIDIA có thể sử dụng tính năng tăng tốc CUDA để xử lý nhanh hơn đáng kể. Tính năng này hoạt động trên cảWindows

(GPU máy tính để bàn) vàLinux

(GPU máy tính để bàn và NVIDIA Jetson).Chloros

+ người dùng còn được hưởng lợi từ khả năng xử lý đa luồng để đạt tốc độ tối đa.
{% endhint %}

***

## Tải xuốngChloros

### Phiên bản ổn định mới nhất: Phiên bản 1.2.0

<!-- NOLAN: replace installer links + release date for 1.2.0 — the three download buttons below still point at the 1.1.0 Google Drive files, and the release date needs to be added to the heading above. -->



### <a href="https://drive.google.com/uc?export=download&#x26;id=1HjwrUY4M7HGxDbMybO7iPe_6JoHnUGr4" class="button primary">Tải xuống Chloros cho Windows (.exe)</a>



### <a href="https://drive.google.com/uc?export=download&#x26;id=1dB8-ke3wxNXpw_e1qJ4BhwBpCoNd4kLS" class="button primary">Tải xuống Chloros cho Linux amd64 (.deb)</a>



### <a href="https://drive.google.com/uc?export=download&#x26;id=1d1OwdcYA4Rf4jkuPi2IBeWT2772_HnyO" class="button primary">Tải xuống Chloros cho Linux arm64 / Jetson (.deb)</a>



#### Trình cài đặtWindows

(GUI +CLI

+ Backend)

* **Loại tệp**: .exe (Trình cài đặtWindows

)

**Các bước cài đặt:**

1. Tải xuống tệp .exe ở trên
2. Nhấp đúp vào trình cài đặt để bắt đầu cài đặt
3. Làm theo các hướng dẫn của trình hướng dẫn cài đặt
4. Chọn thư mục cài đặt (mặc định: `C:\Program Files\MAPIR\Chloros\`)
5. Hoàn tất cài đặt và khởi chạyChloros

hoặcChloros

CLI

6. Đăng nhập bằng tài khoản [MAPIR

CloudChloros

+](https://cloud.mapir.camera/pricing) (hoặc tiếp tục sử dụng phiên bản miễn phí)

{% hint style="success" %}
Trình cài đặt sẽ tự động thêm `chloros-cli` vào biến môi trường PATH của hệ thống để truy cập từ dòng lệnh.
{% endhint %}

####Linux

amd64 (Gói .deb —CLI

+ Backend)

* **Loại tệp**: .deb (gói Debian/Ubuntu)
* **Kiến trúc**: x86_64 (amd64)

```bash
sudo dpkg -i chloros-amd64.deb
chloros-cli --version  # Verify installation
```

####Linux

arm64 — NVIDIA Jetson (Gói .deb —CLI

+ Backend)

* **Loại tệp**: .deb (JetPack 6)
* **Kiến trúc**: aarch64 (arm64)

```bash
sudo dpkg -i chloros-arm64-jp6.deb
chloros-cli --version  # Verify installation
```

Xem [Cài đặtLinux

](linux/linux-installation.md) để biết hướng dẫn cài đặt chi tiết và [Hướng dẫn NVIDIA Jetson](linux/nvidia-jetson-guide.md) để biết hướng dẫn cụ thể cho Jetson.

####Python

SDK

(Tất cả nền tảng)

Mỗi trình cài đặt đều đi kèm với gói `chloros_sdk` tương ứng, do đó phiên bảnSDK

luôn khớp với GUI/CLI

/backend đã cài đặt. TrênWindows

, trình cài đặt sẽ tự động cài đặt nó vào thư mục hệ thốngPython

; trênLinux

, `.deb` sẽ đặt gói wheel tại `/usr/lib/chloros/sdk/` và hiển thị lệnh cài đặt:

```bash
pip install --user /usr/lib/chloros/sdk/chloros_sdk-*.whl
```

Đối với các máy chủ chỉ sử dụng pip (không cài đặt góiChloros

),SDK

cũng có trên PyPI:

```bash
pip install chloros-sdk
```

Xem [API

:Python

SDK

](api-python-sdk.md) và [SDK

Tài liệu tham khảo](reference/sdk-reference.md) để biết thêm thông tin.

{% hint style="info" %}
**Người dùngLinux**: Gói `.deb` cài đặtCLI

và backend. Không có giao diện người dùng đồ họa (GUI) choLinux

— tất cả tương tác đều thông quaCLI

hoặcSDK

.
{% endhint %}

***

## Tài nguyên bổ sung

###Python

SDK



Đối với các nhà phát triển và quy trình làm việc tự động hóa, hãy cài đặtChloros

Python

SDK

:

```bash
pip install chloros-sdk
```

**Tài liệu**: [API

:Python

SDK

](api-python-sdk.md)

**Yêu cầu**: Phải cài đặtChloros

(trình cài đặtWindows

hoặc góiLinux

`.deb`), yêu cầu đăng nhập bằng giấy phépChloros

+

***

## Gói sản phẩm bao gồm

### Trình cài đặtWindows



* ✅ **Giao diện người dùng đồ họaChloros** - Giao diện đồ họa đầy đủ tính năng
* ✅ **Giao diện dòng lệnhChloros

CLI

** - Giao diện dòng lệnh (yêu cầu giấy phépChloros

+)
* ✅ **Hệ thống xử lý nềnChloros** - Hệ thống xử lý
* ✅ **Camera Profiles** - Các mẫu camera được cấu hình sẵnMAPIR



### Gói .deb củaLinux



* ✅ **Chloros

CLI

** - Giao diện dòng lệnh (yêu cầu giấy phépChloros

+)
* ✅ **Chloros

Backend** - Bộ xử lý
* ✅ **Camera Profiles** - Các mẫu máy ảnh được cấu hình sẵnMAPIR


* ❌ Không có GUI —Linux

là phiên bản không giao diện người dùng (headless), chỉ hỗ trợCLI

/SDK



###Python

SDK

(pip, tất cả nền tảng)

* ✅ **Chloros

SDK

** -Python

API

(yêu cầu giấy phépChloros

+)

***

## Nâng cấp lênChloros

+

Mở khóa các tính năng nâng cao với gói đăng kýChloros

+:

* 🚀 **Xử lý đa luồng** - Xử lý hình ảnh song song
* ⚡ **Tăng tốc GPU (CUDA)** - Tận dụng sức mạnh GPU của NVIDIA
* 💻 **Truy cậpCLI** - Tự động hóa bằng các công cụ dòng lệnh
* 🐍 **Python

SDK

** - Truy cậpAPI

thông qua lập trình
* 📱 **Nhiều thiết bị** - Sử dụng trên 2-10+ thiết bị (tùy thuộc vào gói đăng ký)
* **🐻 Phương pháp loại bỏ hiệu ứng debayering nâng cao nhận diện kết cấu** - Phương pháp loại bỏ hiệu ứng debayering chất lượng cao, nhận diện cạnh, kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ nhiễu do hiệu ứng debayering gây ra.
* 🧮 **Công thức tùy chỉnh** - Tạo các chỉ số đa quang phổ tùy chỉnh

<p align="center"><a href="https://cloud.mapir.camera/pricing" class="button primary">Xem Chloros+ Gói dịch vụ &amp; Giá cả</a></p>***

## Hỗ trợ cài đặt

### Khắc phục sự cố

**Quá trình cài đặt thất bại với thông báo lỗi:**

* Đảm bảo bạn có quyền quản trị viên
* Tạm thời tắt phần mềm diệt virus
* Kiểm tra xem máy tính có đáp ứng yêu cầu hệ thống tối thiểu hay không

**Ứng dụng không khởi động được (Windows

):**

* Kiểm tra xemWindows

10/11 (64-bit) đã được cài đặt chưa
* Cập nhật trình điều khiển đồ họa
* Kiểm tra Trình xem sự kiện (Event Viewer) tạiWindows

để xem chi tiết lỗi
* Liên hệ bộ phận hỗ trợ kèm theo nhật ký lỗi

**CLI

không khởi động được (Linux

):**

* Xác minh gói `.deb` đã được cài đặt đúng cách: `dpkg -l | grep chloros`
* Kiểm tra quyền truy cập: `sudo chmod +x /usr/bin/chloros-cli`
* Chạy chẩn đoán: `chloros-cli selftest`
* Kiểm tra xem có thư viện nào bị thiếu không: `ldd /usr/lib/chloros/chloros-backend | grep "not found"`

**Vấn đề kích hoạt giấy phép:**

* Đảm bảo kết nối internet đang hoạt động
* Xác minh thông tin đăng nhập tại [https://cloud.mapir.camera](https://cloud.mapir.camera)
* Kiểm tra xem tường lửa có chặnChloros

không * Xem [Chloros

+ Đăng nhập](chloros+-login.md) để biết hướng dẫn chi tiết

### Nhận hỗ trợ

Cần trợ giúp về cài đặt hoặc thiết lập?

* 📧 **Email**: info@mapir.camera
* 🌐 **Trang web**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Tài liệu**: [Bắt đầu sử dụng](./)
* ❓ **Câu hỏi thường gặp**: [Câu hỏi thường gặp](faq.md)***

## Cập nhật phần mềm

Chloros

kiểm tra các bản cập nhật, thông báo khi có phiên bản mới và cung cấp liên kết đến trang tải xuống này — bạn cập nhật bằng cách chạy trình cài đặt mới đã được ký. Cài đặt và dự án của bạn sẽ được giữ nguyên sau khi cập nhật. TrênLinux

và Jetson, `chloros-cli update` sẽ kiểm tra xem có phiên bản mới hơn không và đề nghị tải xuống và cài đặt phiên bản tương ứng `.deb` (lệnh này chỉ dành choLinux

).

***

## Nhật ký thay đổi**Phiên bản 1.2.0 (Mới nhất)**— xem**Có gì mới trongChloros

1.2.0** trên trang [Bắt đầu](./) để biết danh sách đầy đủ các tính năng.

<details>

<summary>Phiên bản 1.0.5</summary>

**Ngày phát hành: 10 tháng 2 năm 2026**

**Tính năng mới*** **Phương pháp loại bỏ hiệu ứng debayer dựa trên kết cấu \[Chloros

+ Chỉ] -** Phương pháp dựa trên kết cấu sử dụng thuật toán loại bỏ hiệu ứng debayer chất lượng cao, nhận diện cạnh kết hợp với mô hình khử nhiễu AI/ML, giúp loại bỏ gần như toàn bộ nhiễu do quá trình debayer gây ra.
* **Hỗ trợ mục tiêu hiệu chuẩn T4P*** **Chloros

nhanh hơn + xử lý GPU, quản lý bộ nhớ tốt hơn**

**Sửa lỗi*** Giao diện người dùng (GUI) hoàn toàn mới, hiện đã hoạt động trên tất cả các máy tínhWindows

.

</details>

<details>

<summary>Phiên bản 1.0.4</summary>

**Ngày phát hành: 5 tháng 1 năm 2026**

**Tính năng mới*** **Chuyển đổi hình ảnh/siêu dữ liệu**: Đã thêm nút chuyển đổi trong Trình duyệt tệp để xem siêu dữ liệu của hình ảnh đã chọn dưới dạng bảng thay vì lưới hình ảnh
* **Thanh trượt thu phóng lưới hình ảnh**: Thanh trượt giao diện người dùng mới để điều chỉnh kích thước hình thu nhỏ (cũng hỗ trợ phím CTRL + bánh xe chuột)
* **Nút xuất lưới hình ảnh**: Các nút ở hàng trên cùng để chuyển hình thu nhỏ từ định dạng JPG sang các định dạng xuất đã xử lý (Targets, Reflectance, Index, LUT)
* **Tab Bản đồ**: Bản đồ 2D tương tác mới hiển thị các điểm đánh dấu vị trí GPS trên hình ảnh
  * Hỗ trợ Google Maps và các ô bản đồ ESRI (tự động chọn dịch vụ ô bản đồ tốt nhất dựa trên mức độ phóng to có sẵn)
  * Di chuột qua các điểm đánh dấu trên bản đồ để xem trước hình thu nhỏ

**Sửa lỗi*** Cải thiện hỗ trợ cài đặt Chloros trên các máy tính không sử dụng tiếng Anh

</details>

<details>

<summary>Phiên bản 1.0.3</summary>

**Ngày phát hành: 20 tháng 12 năm 2025**

**Tính năng mới*** Phiên bản đầu tiên

**Cải tiến*** Phiên bản đầu tiên

**Sửa lỗi*** Phiên bản đầu tiên

**Vấn đề đã biết*** Phiên bản đầu tiên

</details>

***

## Thỏa thuận cấp phép**Phần mềm độc quyền** - Bản quyền (c) 2026 MAPIR Inc.

Nghiêm cấm sử dụng, phân phối hoặc sửa đổi trái phép.

**Phiên bản miễn phí**: Dành cho mục đích sử dụng cá nhân và thương mại với các hạn chế về tính năng**Chloros+**: Giấy phép dựa trên đăng ký để sử dụng các tính năng nâng cao và triển khai thương mại
