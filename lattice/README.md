# Máy ảnh LATTICE

LATTICE là hệ thống camera đa quang phổ mô-đun củMAPIR dành cho chụp ảnh nông nghiệp và khoa học. Mỗi camera LATTICE đều được trang bị cảm biến global-shutter Sony IMX265 (**3,1 MP, điểm ảnh 3,45 µm**) và kết nối qua Ethernet dưới dạng thiết bị**GigE Vision**.

ChlorosPhiên bản 1.2.0 cho phép điều khiển các máy ảnh LATTICE theo thời gian thực — phát hiện, xem trước trực tiếp, chụp ảnh và mảng máy ảnh đa kênh đồng bộ — từ ba giao diện:

| Giao diện    | Nơi                                                          | Nền tảng                                                |
| ---------- | -------------------------------------------------------------- | -------------------------------------------------------- |
| Giao diện người dùng (GUI)    | Tab **Camera** trong thanh bên của Chloros                         | Windows 10/11 x64                                        |
| CLI        | Bộ lệnh `chloros-cli lattice`                           | Windows 10/11 x64, Linux x86\_64, Linux aarch64 (Jetson) |
| Python SDK | `chloros_sdk.connect_camera()` / `chloros_sdk.connect_array()` | Windows 10/11 x64, Linux x86\_64, Linux aarch64 (Jetson) |

> **Bạn đang tìm kiếm phần cứng?**Các mô-đun camera, ống kính, bộ lọc và dải tần, khung và giá đỡ, cáp, PoE và dây kích hoạt được mô tả chi tiết trong [**Hướng dẫn sử dụng LATTICE**](https://mapir.gitbook.io/lattice-camera). Chương này hướng dẫn cách điều khiển camera từ Chloros.

Các tệp ghi hình của LATTICE là các tệp tiêu chuẩn `.tif`/`.tiff`, và Chloros luôn xử lý chúng bắt đầu từ dữ liệu thô. Xem [Tài liệu tham khảo CLI](../reference/cli-reference.md) và [Tài liệu tham khảo SDK](../reference/sdk-reference.md) để biết đầy đủ các lệnh và giao diện API.

## Hai cấu hình cảm biến

| Cấu hình | Cảm biến       | Bộ lọc                                | Hình ảnh do một camera ghi lại                                          |
| ------------- | ------------ | ------------------------------------- | ----------------------------------------------------------------- |
| **M3C**| Màu Bayer | bộ lọc ba dải thông                |**Ba dải đã hiệu chuẩn từ một lần phơi sáng**                 |
| **M3M**| Đơn sắc   | bộ lọc giao thoa dải hẹp đơn |**Một dải đã hiệu chuẩn**; kết hợp nhiều máy ảnh M3M để tính chỉ số |

Vì máy ảnh M3M là máy ảnh đơn sắc với một bộ lọc duy nhất, mỗi dải sẽ có phơi sáng riêng. Máy ảnh M3C bao phủ cả ba dải của nó chỉ với một lần phơi sáng cảm biến.

## Chuỗi mô hình và cách đặt tên

Mỗi máy ảnh lưu trữ thông tin nhận dạng của mình trong GenICam `DeviceUserID` dưới dạng chuỗi mô hình:

```
<sensor>-<lens>-F<filter>       e.g.  M3C-L41-FRGN,  M3M-L87-F450
```

Phần mềm Chloros hiển thị nó với tiền tố `LATT-` (ví dụ: `LATT-M3M-L87-F450`). Chuỗi `LATT-…` này cũng được ghi vào thẻ EXIF `Model` của mỗi tệp xuất ra và được sử dụng làm tên thư mục đầu ra của máy ảnh trong các dự án đã xử lý.

| Thành phần | Giá trị                                                   | Ý nghĩa                                                                                            |
| --------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Cảm biến    | `M3C` / `M3M`                                            | Màu Bayer / đơn sắc                                                                          |
| Ống kính      | `L41` / `L87`                                            | Số này là **góc nhìn ngang tính bằng độ**: L41 = hẹp (41°), L87 = rộng (87°)    |
| Bộ lọc    | `FRGB` / `FRGN` / `FOCN` / `FNGB` (M3C) hoặc `F<nm>` (M3M) | Xem [Bộ lọc &amp; Dải quang phổ](https://mapir.gitbook.io/lattice-camera/hardware/filters-and-bands) |

Chuỗi mô hình điều khiển tất cả các bước tiếp theo: Chloros xác định cấu hình cảm biến, bố cục dải phổ và hiệu chuẩn nhà máy từ `DeviceUserID` + `DeviceSerialNumber`. Không cần cấu hình gì cho từng camera — xem [Kết nối camera](connecting.md).

## Bộ lọc và dải phổ

Tâm dải phổ, rìa dải phổ (FWHM) và danh mục đầy đủ 23 SKU của M3M là các thông số kỹ thuật sản phẩm, do đó chúng được ghi trong tài liệu hướng dẫn phần cứng: [**Bộ lọc &amp; Dải phổ**](https://mapir.gitbook.io/lattice-camera/hardware/filters-and-bands).

Điều quan trọng ở phía phần mềm: mã bộ lọc trong chuỗi mô hình quyết định các sản phẩm mà Chloros có thể tạo ra. Các camera có bộ lọc RGB (`FRGB`) chỉ phát ra các sản phẩm đã loại bỏ hiệu ứng Bayer và bản xem trước — độ rạng rỡ và độ phản xạ theo từng dải phổ không có ý nghĩa đối với cảm biến băng rộng, do đó Chloros bỏ qua chúng và thông báo điều này. Mọi bộ lọc khác đều tạo ra chuỗi đầy đủ từ độ rạng rỡ → độ phản xạ → chỉ số.

## Tổng quan về hiệu chuẩn quang phổ

Mỗi camera LATTICE đều được hiệu chuẩn riêng lẻ tại nhà máy dựa trên chuỗi hiệu chuẩn có thể truy xuất nguồn gốc theo tiêu chuẩn NIST và được cung cấp kèm theo chứng chỉ riêng cho từng camera. Nội dung được bao phủ, phương pháp đo lường và độ chính xác có thể trích dẫn được nêu trong tài liệu hướng dẫn phần cứng: [**Hiệu chuẩn bức xạ tại nhà máy**](https://mapir.gitbook.io/lattice-camera/calibration/factory-radiometric-calibration).

Về mặt phần mềm, điều quan trọng là Chloros xác định đúng thông số hiệu chuẩn khi camera kết nối và cố định các hệ số đã áp dụng vào mọi tệp xuất — xem [Kết nối camera](connecting.md).

## Trong chương này

* [Kết nối camera](connecting.md) — tính năng tự động phát hiện, hộp thoại kết nối trên giao diện người dùng (GUI), các lệnh tương đương CLI / SDK, và cách xác định hiệu chuẩn nhà máy (gói trên camera so với đám mây) khi camera kết nối.

Các chủ đề khác về LATTICE — cài đặt camera và điều khiển trực tiếp, chế độ ghi hình, mảng nhiều camera, cùng với xử lý mono (M3M) và chỉ số — được trình bày trong các phần riêng biệt của tài liệu này, và danh sách lệnh đầy đủ có tại [CLI Reference](../reference/cli-reference.md) và [SDK Reference](../reference/sdk-reference.md).
