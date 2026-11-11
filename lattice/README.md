# Máy ảnh LATTICE

LATTICE là hệ thống camera đa quang phổ mô-đun củMAPIR dành cho chụp ảnh nông nghiệp và khoa học. Mỗi camera LATTICE đều được trang bị cảm biến global-shutter Sony IMX265 (**3,1 MP, điểm ảnh 3,45 µm**) và kết nối qua Ethernet dưới dạng thiết bị**GigE Vision**.

Chloros 1.3.0 điều khiển camera LATTICE theo thời gian thực — phát hiện, xem trước trực tiếp, chụp ảnh và mảng camera đa kênh đồng bộ — từ ba giao diện:

| Giao diện    | Nơi                                                          | Nền tảng                                                |
| ---------- | -------------------------------------------------------------- | -------------------------------------------------------- |
| Giao diện người dùng (GUI) | Tab **Camera** trong thanh bên Chloros                         | Windows 10/11 x64                                        |
| Giao diện dòng lệnh (CLI)        | Bộ lệnh `chloros-cli lattice`                           | Windows 10/11 x64, Linux x86\_64, Linux aarch64 (Jetson) |
| Python SDK | `chloros_sdk.connect_camera()` / `chloros_sdk.connect_array()` | Windows 10/11 x64, Linux x86\_64, Linux aarch64 (Jetson) |

> **Bạn đang tìm kiếm phần cứng?**Các mô-đun camera, ống kính, bộ lọc và dải tần, khung và giá đỡ, cáp, PoE và dây kích hoạt được mô tả chi tiết trong [**Hướng dẫn sử dụng LATTICE**](https://mapir.gitbook.io/lattice-camera). Chương này hướng dẫn cách điều khiển camera từ Chloros.

Các tệp ghi hình của LATTICE là các tệp tiêu chuẩn `.tif`/`.tiff`, và Chloros luôn xử lý chúng bắt đầu từ dữ liệu thô. Xem [Tài liệu tham khảo CLI](../reference/cli-reference.md) và [Tài liệu tham khảo SDK](../reference/sdk-reference.md) để biết đầy đủ các lệnh và giao diện API.

## Hai cấu hình cảm biến

| Cấu hình | Cảm biến       | Bộ lọc                                | Dữ liệu mà một camera cung cấp                                          |
| ------------- | ------------ | ------------------------------------- | ----------------------------------------------------------------- |
| **M3C**| Màu Bayer | bộ lọc ba dải thông                |**Ba dải đã hiệu chuẩn từ một lần phơi sáng duy nhất**                 |
| **M3M**| Đơn sắc   | bộ lọc nhiễu dải hẹp đơn |**Một dải đã hiệu chuẩn**; kết hợp nhiều camera M3M để tạo chỉ số |

Vì camera M3M là camera đơn sắc phía sau một bộ lọc duy nhất, mỗi dải sẽ có lần phơi sáng riêng. Máy ảnh M3C bao phủ cả ba dải quang phổ của nó chỉ với một lần phơi sáng cảm biến.

## Chuỗi mô hình và cách đặt tên

Mỗi máy ảnh lưu trữ thông tin nhận dạng của mình trong GenICam dưới dạng chuỗi mô hình `DeviceUserID`:

```
<sensor>-<lens>-F<filter>       e.g.  M3C-L41-FRGN,  M3M-L87-F450
```

Chloros hiển thị nó với tiền tố `LATT-` (ví dụ: `LATT-M3M-L87-F450`). Chuỗi `LATT-…` này cũng được ghi vào thẻ EXIF `Model` của mọi tệp xuất và được sử dụng làm tên thư mục đầu ra của máy ảnh trong các dự án đã xử lý.

| Thành phần | Giá trị                                                   | Ý nghĩa                                                                                            |
| --------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Cảm biến    | `M3C` / `M3M`                                            | Màu Bayer / đơn sắc                                                                          |
| Ống kính      | `L41` / `L87`                                            | Số này là **góc nhìn ngang tính bằng độ**: L41 = hẹp (41°), L87 = rộng (87°)    |
| Bộ lọc    | `FRGB` / `FRGN` / `FOCN` / `FNGB` (M3C) hoặc `F<nm>` (M3M) | Xem [Bộ lọc & Dải quang phổ](https://mapir.gitbook.io/lattice-camera/hardware/filters-and-bands) |

Chuỗi ký tự mô hình điều khiển mọi thứ ở các bước tiếp theo: Chloros xác định cấu hình cảm biến, bố cục dải quang phổ và hiệu chuẩn nhà máy từ `DeviceUserID` + `DeviceSerialNumber`. Không cần cấu hình gì cho từng camera — xem [Kết nối camera](connecting.md).

## Bộ lọc và dải

Tâm dải, rìa FWHM và danh mục M3M đầy đủ 23 SKU là các thông số kỹ thuật của sản phẩm, do đó chúng được nêu trong hướng dẫn sử dụng phần cứng: [**Bộ lọc & Dải quang phổ**](https://mapir.gitbook.io/lattice-camera/hardware/filters-and-bands).

Điều quan trọng về mặt phần mềm: mã bộ lọc trong chuỗi mô hình xác định các sản phẩm mà Chloros có thể tạo ra. Các máy ảnh bộ lọc RGB (`FRGB`) chỉ tạo ra các sản phẩm đã loại bỏ hiệu ứng Bayer và bản xem trước — độ rạng rỡ và độ phản xạ theo từng dải phổ không có ý nghĩa đối với cảm biến băng rộng, do đó Chloros bỏ qua chúng và thông báo điều này. Mọi bộ lọc khác đều tạo ra chuỗi đầy đủ từ độ rạng rỡ → độ phản xạ → chỉ số.

## Tổng quan về hiệu chuẩn quang phổ

Mỗi máy ảnh LATTICE đều được hiệu chuẩn riêng lẻ tại nhà máy dựa trên chuỗi hiệu chuẩn có thể truy xuất nguồn gốc theo NIST và được cung cấp kèm theo chứng chỉ riêng cho từng máy. Nội dung hiệu chuẩn, phương pháp đo lường và độ chính xác có thể trích dẫn được nêu trong tài liệu hướng dẫn phần cứng: [**Hiệu chuẩn bức xạ tại nhà máy**](https://mapir.gitbook.io/lattice-camera/calibration/factory-radiometric-calibration).

Về mặt phần mềm, điều quan trọng là Chloros sẽ xác định đúng thông số hiệu chuẩn khi camera kết nối và cố định các hệ số đã áp dụng vào mọi tệp xuất — xem [Kết nối camera](connecting.md).

## Trong chương này

* [Kết nối camera](connecting.md) — tính năng tự động phát hiện, hộp thoại kết nối trên giao diện người dùng (GUI), các phương thức tương đương trên giao diện dòng lệnh (CLI) và bộ công cụ phát triển phần mềm (SDK), cũng như cách xác định hiệu chuẩn nhà máy (gói trên camera so với đám mây) khi camera kết nối.
* [Cài đặt máy ảnh](camera-settings.md) — mọi điều khiển trong tab Máy ảnh: danh sách máy ảnh, khung cài đặt cho từng máy ảnh và khung cài đặt mảng (bao gồm trình chỉnh sửa ROI và căn chỉnh).
* [Cài đặt & Chế độ chụp](capture.md) — Chụp tất cả, chụp đơn lẻ / liên tục / theo khoảng thời gian, các loại xuất và bố cục thư mục ảnh chụp.
* [Mảng đa camera](arrays.md) — trình hướng dẫn kết nối mảng, đầu dò cáp đồng bộ, băng thông, PTP và chế độ xem mảng trực tiếp.
* [Camera đơn & Chỉ số thực vật](mono-indices.md) — Camera M3M và quy trình làm việc căn chỉnh → ghép chồng → tính chỉ số.

Toàn bộ giao diện lệnh được trình bày trong [Tham chiếu CLI](../reference/cli-reference.md) và [Tham chiếu SDK](../reference/sdk-reference.md).
