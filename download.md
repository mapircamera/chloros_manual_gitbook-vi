---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/o044KN3Ws0uIDvOmSkcR/download
---

# Tải xuống

Tải xuống phiên bản mới nhất của Chloros
để bắt đầu xử lý hình ảnh đa quang phổ.

### Yêu cầu hệ thống

#### Windows


| Yêu cầu          | Tối thiểu                                              | Đề xuất                                          |
| -------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| **Hệ điều hành** | Windows
10 (64-bit)                                  | Windows
11 (64-bit)                                  |
| **Bộ xử lý**        | Intel Core i5 hoặc tương đương                          | Intel Core i7 hoặc cao hơn                              |
| **Bộ nhớ (RAM)**     | 8GB                                                  | 16GB trở lên                                         |
| **Card đồ họa**    | Tương thích DirectX 11                                | GPU NVIDIA với VRAM từ 4GB trở lên                            |
| **Dung lượng lưu trữ**          | 6GB dung lượng trống                                       | SSD với dung lượng trống từ 10GB trở lên                            |
| **Màn hình**          | 1920x1080                                            | 2560x1440 trở lên                                  |
| **Internet**         | Cần thiết để kích hoạt giấy phép \[tùy chọn] Chloros
+ | Cần thiết để kích hoạt giấy phép \[tùy chọn] Chloros
+ |

#### Linux
amd64 (x86\_64)

| Yêu cầu       | Tối thiểu                    | Khuyến nghị               |
| ----------------- | -------------------------- | ------------------------- |
| **Hệ điều hành**  | Ubuntu 22.04 LTS+ / Debian 12+ | Ubuntu 24.04 LTS      |
| **Bộ xử lý**     | x86\_64 (Intel/AMD)        | Intel Core i7 hoặc cao hơn   |
| **Bộ nhớ (RAM)**  | 8GB                        | 16GB trở lên              |
| **Card đồ họa** | Không cần (xử lý bằng CPU)      | GPU NVIDIA với 4GB+ VRAM |
| **Dung lượng lưu trữ**       | 2GB dung lượng trống             | SSD có 10GB+ dung lượng trống       |
| **Python**        | Python
3.7+ (cho SDK
)      | Python
3.10+              |

#### Linux
arm64 (NVIDIA Jetson)

| Yêu cầu      | Tối thiểu                      | Khuyến nghị                     |
| ---------------- | ---------------------------- | ------------------------------- |
| **Nền tảng**     | NVIDIA Jetson với JetPack 6 | Jetson Orin NX 16GB hoặc AGX Orin |
| **Bộ nhớ (RAM)** | 8GB (chia sẻ giữa GPU/CPU)         | 16GB+ chia sẻ                    |
| **Dung lượng lưu trữ**      | 2GB dung lượng trống               | SSD NVMe có 10GB+ dung lượng trống        |
| **Python**       | Python
3.7+ (cho SDK
)        | Python
3.10+                    |

{% hint style="info" %}
**Tăng tốc GPU**: Người dùng Chloros
+ có GPU NVIDIA có thể sử dụng tính năng tăng tốc CUDA để xử lý nhanh hơn đáng kể. Tính năng này hoạt động với các GPU máy tính để bàn NVIDIA trên Windows
và Linux
amd64, cũng như trên NVIDIA Jetson với JetPack 6. Người dùng Chloros
+ còn được hưởng lợi từ khả năng xử lý đa luồng để đạt tốc độ tối đa.
{% endhint %}

***

## Tải xuống Chloros


### Phiên bản ổn định mới nhất: Phiên bản 1.3.0 (ngày 11 tháng 11 năm 2026)

### <a href="https://drive.google.com/uc?export=download&#x26;id=1HjwrUY4M7HGxDbMybO7iPe_6JoHnUGr4" class="button primary">Tải xuống Chloros cho Windows (.exe)</a>


### <a href="https://drive.google.com/uc?export=download&#x26;id=1dB8-ke3wxNXpw_e1qJ4BhwBpCoNd4kLS" class="button primary">Tải xuống Chloros cho Linux amd64 (.deb)</a>


### <a href="https://drive.google.com/uc?export=download&#x26;id=1d1OwdcYA4Rf4jkuPi2IBeWT2772_HnyO" class="button primary">Tải xuống Chloros cho Linux arm64 / Jetson (.deb)</a>


#### Trình cài đặt Windows
(GUI + CLI
+ Backend)

* **Loại tệp**: .exe (Trình cài đặt Windows
)

**Các bước cài đặt:**

1. Tải xuống tệp .exe ở trên
2. Nhấp đúp vào trình cài đặt để bắt đầu cài đặt
3. Làm theo các hướng dẫn của trình hướng dẫn cài đặt
4. Chọn thư mục cài đặt (mặc định: `C:\Program Files\Chloros\`)
5. Trên trang **Chọn tác vụ bổ sung**, giữ nguyên cả hai tùy chọn**Cài đặt hệ thống** được chọn (được khuyến nghị — xem bên dưới)
6. Hoàn tất cài đặt và khởi chạy Chloros
hoặc Chloros
CLI
7. Đăng nhập bằng [tài khoản MAPIR
Cloud Chloros
+](https://cloud.mapir.camera/pricing) (hoặc tiếp tục sử dụng phiên bản miễn phí)

**Các tùy chọn cài đặt hệ thống** (cả hai đều được chọn theo mặc định):

| Tùy chọn, theo cách diễn đạt trong trình cài đặt | Chức năng | Tùy chọn từ chối cài đặt im lặng |
| --- | --- | --- |
| **Bật đường dẫn dài của Windows** (cho phép các thư mục dự án có độ sâu vượt quá 260 ký tự; được khuyến nghị) | Đặt cài đặt `LongPathsEnabled` trên toàn hệ thống Windows
thành 1. Windows
được cài đặt sẵn với tùy chọn này tắt, do đó đường dẫn tệp không được dài hơn 260 ký tự; các sản phẩm Chloros
thường nằm sâu hơn khoảng 130 ký tự so với thư mục dự án, vì vậy nếu tắt tùy chọn đường dẫn dài, hệ thống sẽ từ chối thư mục dự án nằm sâu hơn. Cài đặt này có hiệu lực đối với các chương trình được khởi chạy sau khi cài đặt. Cài đặt này vẫn được duy trì sau khi gỡ cài đặt, vì các chương trình khác có thể phụ thuộc vào nó. | `/MERGETASKS="!longpaths"` |
| **Loại trừ backend Chloros
khỏi quá trình quét của Microsoft Defender** (quá trình ghi hình từ camera nhanh hơn nhiều; được khuyến nghị) | Thêm quy tắc loại trừ quá trình cho `chloros-backend.exe` trong thư mục cài đặt vào Microsoft Defender, để tính năng bảo vệ thời gian thực không quét mọi tệp mà backend ghi ra, bất kể tệp đó được ghi ở đâu. Ở tốc độ chụp cao, quá trình quét chiếm khoảng 30% tốc độ chụp (77,7 so với 52,7 lần chụp mỗi giây trong chế độ chụp liên tục với 9 camera). Không có chương trình nào khác bị loại trừ, và quy tắc loại trừ sẽ bị xóa khi gỡ cài đặt. Trong trường hợp Defender không có sẵn hoặc bị khóa bởi chính sách, bước này sẽ thất bại mà không có thông báo và quá trình cài đặt vẫn tiếp tục. | `/MERGETASKS="!defenderexclusion"` |

Quá trình cài đặt im lặng (`/SILENT` hoặc `/VERYSILENT`) sẽ áp dụng cả hai tùy chọn trừ khi bạn chọn không tham gia bằng giá trị `/MERGETASKS` được hiển thị.

{% hint style="success" %}
Trình cài đặt sẽ thêm thư mục CLI
(`C:\Program Files\Chloros\cli\`) vào hệ thống của bạn `PATH` chỉ một lần — việc cài đặt lại hoặc nâng cấp sẽ không thêm mục trùng lặp nữa, đồng thời xóa các mục trùng lặp do các trình cài đặt trước đó đã thêm. Mở một cửa sổ terminal mới sau khi cài đặt để sử dụng `chloros-cli`. Trình cài đặt cũng thêm hai quy tắc Tường lửa Windows
cho phần backend, trên mọi cấu hình mạng: **Chloros
Backend (UDP-In)**, dành cho luồng video camera, PTP, mDNS và luồng dữ liệu DAQ-E, và**Chloros
Backend (TCP-In)**, giới hạn trong mạng con cục bộ, dành cho việc cập nhật firmware DAQ-E, trong đó cảm biến kết nối trở lại máy tính này để thu thập hình ảnh. Cả hai đều bị xóa khi gỡ cài đặt.
{% endhint %}

#### Linux
amd64 (Gói .deb — Giao diện dòng lệnh CLI
+ Backend)

* **Loại tệp**: .deb (gói Debian/Ubuntu)
* **Kiến trúc**: x86\_64 (amd64)

```bash
sudo apt install ./chloros_1.3.0_amd64.deb
chloros-cli --version     # Verify installation: "Chloros CLI 1.3.0"
```

#### Linux
arm64 — NVIDIA Jetson (Gói .deb — CLI
+ Backend)

* **Loại tệp**: .deb (JetPack 6)
* **Kiến trúc**: aarch64 (arm64)

```bash
sudo apt install ./chloros_1.3.0_arm64_jp6.deb
chloros-cli --version     # Verify installation: "Chloros CLI 1.3.0"
```

Sử dụng tên tệp mà bạn đã tải xuống thực tế, và giữ nguyên `./` để `apt` cài đặt tệp đó thay vì tìm kiếm gói theo tên. Cài đặt bằng `apt install`, chứ không phải `dpkg -i`: apt cũng cài đặt các yêu cầu của Python
SDK
, mà gói này khuyến nghị thay vì bắt buộc, và `dpkg -i` sẽ bỏ qua chúng. Kể từ phiên bản 1.3.0, backend và CLI
được cài đặt dưới dạng đã giải nén trong thư mục `/usr/lib/chloros/`, do đó chúng khởi động mà không cần tự giải nén trước.

Gói Jetson sử dụng phiên bản PyTorch được biên dịch bằng CUDA của JetPack 6, do đó quá trình xử lý, bao gồm cả thuật toán Texture Aware debayer, sẽ chạy trên GPU của Jetson. Nó sử dụng các thư viện CUDA và cuDNN mà JetPack 6 cài đặt (`sudo apt install nvidia-jetpack` trên hình ảnh không có các thư viện này).

Xem [Cài đặt trên Linux
](linux/linux-installation.md) để biết hướng dẫn thiết lập chi tiết và [Hướng dẫn NVIDIA Jetson](linux/nvidia-jetson-guide.md) để có hướng dẫn cụ thể cho Jetson.

#### Python
SDK
(Tất cả nền tảng)

Mỗi trình cài đặt đều đi kèm với một gói wheel `chloros_sdk` tương ứng, do đó phiên bản SDK
luôn khớp với phiên bản GUI/CLI
/backend đã cài đặt. Trên Windows
, trình cài đặt sẽ tự động cài đặt nó vào Python
hệ thống của bạn. Trên Linux
, `.deb` sẽ cài đặt SDK
cho chính hệ thống `python3` (vào `/usr/lib/python3/dist-packages`), do đó `import chloros_sdk` hoạt động ngay sau khi cài đặt, mà không cần pip hay kết nối mạng. Chỉ có môi trường ảo (virtualenv) — vốn không truy cập được các gói phần mềm của hệ thống — mới cần cài đặt gói wheel đi kèm vào đó:

```bash
pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl
```

Đối với các máy chủ chỉ sử dụng pip (không cài đặt gói Chloros
; lúc này SDK
sẽ điều khiển một backend trên máy khác), SDK
cũng có sẵn trên PyPI:

```bash
pip install chloros-sdk
```

Xem [API
: Python
SDK
](api-python-sdk.md) và [Tài liệu tham khảo SDK
](reference/sdk-reference.md) để biết thêm thông tin.

{% hint style="info" %}
**Người dùng Linux**: Gói `.deb` cài đặt giao diện dòng lệnh (CLI
), phần backend và SDK
Python
. Không có giao diện đồ họa (GUI) cho Linux
— mọi tương tác đều thông qua CLI
hoặc SDK
.
{% endhint %}

***

## Tài nguyên bổ sung

### SDK
Python


Dành cho các nhà phát triển và quy trình tự động hóa, Chloros
Python
SDK
được tích hợp sẵn trong mọi trình cài đặt: trình cài đặt Windows
sẽ cài đặt nó vào môi trường Python
của bạn, còn `.deb` trên Linux
sẽ cài đặt nó cho hệ thống `python3`. Trên máy không có gói Chloros
(máy điều khiển backend trên một máy khác), hãy cài đặt từ PyPI:

```bash
pip install chloros-sdk
```

**Tài liệu**: [API
: Python
SDK
](api-python-sdk.md)

**Yêu cầu**: một backend Chloros
để điều khiển (trình cài đặt Windows
hoặc gói `.deb` trên Linux
sẽ cài đặt một backend), và tài khoản đăng nhập Chloros
+ ở gói dịch vụ trả phí

***

## Gói bao gồm những gì

### Trình cài đặt Windows


* ✅ **Giao diện người dùng Chloros** - Giao diện đồ họa đầy đủ tính năng
* ✅ **Chloros
CLI
** - Giao diện dòng lệnh (yêu cầu giấy phép Chloros
+)
* ✅ **Hệ thống nền Chloros** - Bộ xử lý
* ✅ **Python
SDK
** - được cài đặt vào môi trường Python
của bạn nếu có sẵn (yêu cầu giấy phép Chloros
+)
* ✅ **Camera Profiles** - Các mẫu camera MAPIR
được cấu hình sẵn

### Gói .deb cho Linux


* ✅ **Chloros
CLI
** - Giao diện dòng lệnh (yêu cầu giấy phép Chloros
+)
* ✅ **Chloros
Backend** - Bộ xử lý
* ✅ **Python
SDK
** - được cài đặt cho hệ thống `python3` (yêu cầu giấy phép Chloros
+)
* ✅ **Cấu hình camera** - Các mẫu camera MAPIR
được cấu hình sẵn
* ❌ Không có GUI — Linux
chỉ hỗ trợ CLI
/SDK
không giao diện đồ họa

### Python
SDK
(PyPI, dành cho các máy không có gói Chloros
)

* ✅ **Chloros
SDK
** - Python
API
(yêu cầu giấy phép Chloros
+)

***

## Nâng cấp lên Chloros
+

Mở khóa các tính năng nâng cao với gói đăng ký Chloros
+:

* 🚀 **Xử lý đa luồng** - Xử lý hình ảnh song song
* ⚡ **Tăng tốc GPU (CUDA)** - Tận dụng sức mạnh GPU của NVIDIA
* 💻 **Truy cập CLI** - Tự động hóa bằng các công cụ dòng lệnh
* 🐍 **Python
SDK
** - Truy cập API
theo lập trình
* 📱 **Nhiều thiết bị** - Sử dụng trên 2–10+ thiết bị (tùy thuộc vào gói đăng ký)
* **🐻 Phương pháp loại bỏ hiệu ứng debayer nâng cao dựa trên kết cấu** - phương pháp loại bỏ hiệu ứng debayer chất lượng cao, nhận diện cạnh kết hợp với mô hình khử nhiễu AI/ML giúp loại bỏ gần như toàn bộ nhiễu do hiệu ứng debayer gây ra.
* 🧮 **Công thức tùy chỉnh** - Tạo chỉ số đa quang phổ tùy chỉnh

<p align="center"><a href="https://cloud.mapir.camera/pricing" class="button primary">Xem các gói và giá của Chloros+</a></p>***

## Hỗ trợ cài đặt

### Khắc phục sự cố

**Quá trình cài đặt thất bại với thông báo lỗi:**

* Đảm bảo bạn có quyền quản trị viên
* Tạm thời tắt phần mềm diệt virus
* Kiểm tra xem hệ thống có đáp ứng yêu cầu tối thiểu hay không

**Ứng dụng không khởi động được (hệ điều hành Windows

):**

* Xác minh rằng Windows

10/11 (64-bit) đã được cài đặt
* Cập nhật trình điều khiển đồ họa
* Kiểm tra Trình xem sự kiện Windows

để xem chi tiết lỗi
* Liên hệ bộ phận hỗ trợ kèm theo nhật ký lỗi

**CLI

không khởi động được (Linux

):**

* Kiểm tra xem gói `.deb` đã được cài đặt đúng cách chưa: `dpkg -l | grep chloros`
* Kiểm tra quyền truy cập: `sudo chmod +x /usr/bin/chloros-cli`
* Chạy chẩn đoán: `chloros-cli selftest`
* Kiểm tra xem có thiếu thư viện nào không: `ldd /usr/lib/chloros/backend/chloros-backend | grep "not found"` (từ phiên bản 1.3.0 trở đi, `/usr/lib/chloros/chloros-backend` là một tập lệnh khởi chạy nhỏ; chương trình chính nằm trong `/usr/lib/chloros/backend/`)

**Vấn đề kích hoạt giấy phép:**

* Đảm bảo kết nối internet đang hoạt động
* Xác minh thông tin đăng nhập tại [https://cloud.mapir.camera](https://cloud.mapir.camera)
* Kiểm tra xem tường lửa có chặn Chloros

không* Xem [Chloros

+ Đăng nhập](chloros+-login.md) để biết hướng dẫn chi tiết

### Nhận hỗ trợ

Cần trợ giúp về cài đặt hoặc thiết lập?

* 📧 **Email**: info@mapir.camera
* 🌐 **Trang web**: [https://www.mapir.camera/community/contact](https://www.mapir.camera/community/contact)
* 📚 **Tài liệu**: [Bắt đầu sử dụng](./)
* ❓ **Câu hỏi thường gặp**: [Câu hỏi thường gặp](faq.md)***

## Cập nhật phần mềm

Chloros

sẽ kiểm tra các bản cập nhật, thông báo khi có phiên bản mới và cung cấp liên kết đến trang tải xuống này — bạn cập nhật bằng cách chạy trình cài đặt mới đã được ký. Các cài đặt, dự án và thông tin đăng nhập của bạn sẽ được giữ nguyên sau khi cập nhật. Trên Linux

và Jetson, `chloros-cli update` sẽ kiểm tra xem có phiên bản mới hơn không và đề xuất tải xuống và cài đặt phiên bản tương ứng là `.deb` (lệnh này chỉ dành cho Linux

).

***

## Nhật ký thay đổi**Phiên bản 1.3.0 (Mới nhất, ngày 11 tháng 11 năm 2026)**— xem**Có gì mới trong Chloros

1.3.0** trên trang [Bắt đầu](./) để xem danh sách đầy đủ.**Phiên bản 1.2.0**— xem**Có gì mới trong Chloros

1.2.0** trên trang [Bắt đầu](./).

<details>

<summary>Phiên bản 1.0.5</summary>

**Ngày phát hành: 10 tháng 2 năm 2026**

**Tính năng mới*** **Phương pháp khử hiệu ứng debayer nhận diện kết cấu \[Chỉ dành cho Chloros

+] -** Phương pháp nhận diện kết cấu sử dụng thuật toán khử hiệu ứng debayer chất lượng cao, nhận diện cạnh kết hợp với mô hình khử nhiễu AI/ML, giúp loại bỏ gần như toàn bộ nhiễu do quá trình khử hiệu ứng debayer gây ra.
* **Hỗ trợ các mục tiêu hiệu chuẩn T4P*** **Xử lý GPU của Chloros

+ nhanh hơn, quản lý bộ nhớ tốt hơn**

**Sửa lỗi*** Giao diện người dùng (GUI) hoàn toàn mới, hiện đã hoạt động trên tất cả các máy tính Windows

.

</details>

<details>

<summary>Phiên bản 1.0.4</summary>

**Ngày phát hành: 5 tháng 1 năm 2026**

**Tính năng mới*** **Chuyển đổi giữa hình ảnh và siêu dữ liệu**: Đã thêm nút chuyển đổi trong Trình duyệt tệp để xem siêu dữ liệu của hình ảnh đã chọn dưới dạng bảng thay vì lưới hình ảnh
* **Thanh trượt thu phóng lưới hình ảnh**: Thanh trượt giao diện người dùng mới để điều chỉnh kích thước hình thu nhỏ (cũng hỗ trợ tổ hợp phím CTRL + bánh xe chuột)
* **Nút xuất lưới hình ảnh**: Các nút ở hàng trên cùng để chuyển đổi hình thu nhỏ từ định dạng JPG sang các định dạng xuất đã xử lý (Targets, Reflectance, Index, LUT)
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

**Phiên bản miễn phí**: Có sẵn cho mục đích sử dụng cá nhân và thương mại với các giới hạn về tính năng**Chloros+**: Giấy phép dựa trên đăng ký để sử dụng các tính năng nâng cao và triển khai thương mại
