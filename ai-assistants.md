# Sử dụng Chloros với các trợ lý AI

Hướng dẫn này được viết dành cho hai đối tượng: con người và các trợ lý AI mà con người ngày càng sử dụng nhiều hơn. Mỗi trang đều cung cấp các giá trị chính xác, giá trị mặc định và các lệnh có thể sao chép và dán để trợ lý (Claude, ChatGPT, Copilot, trợ lý lập trình, …) có thể viết mã tự động hóa Chloros hoạt động ngay từ lần thử đầu tiên.

Phiên bản Chloros: **

1.3.0**. Nền tảng CLI/SDK: Windows 10/11 x64 và Linux (x86_64 / Jetson aarch64).

## Những gì cần cung cấp cho trợ lý của bạn

| Tài nguyên | URL | Mục đích |
| --- | --- | --- |
| **llms.txt** | `https://mapir.gitbook.io/chloros/llms.txt` | Chỉ mục có thể đọc được bằng máy của mọi trang trong tài liệu hướng dẫn này. |
| **Tài liệu tham khảo CLI** | `https://mapir.gitbook.io/chloros/reference/cli-reference` | Bảng tổng hợp đầy đủ các lệnh `chloros-cli`: mọi lệnh, tham số, giá trị mặc định, mã thoát và quy tắc thư mục đầu ra. Được viết dành cho các mô hình ngôn ngữ lớn (LLM). |
| **Tham chiếu SDK** | `https://mapir.gitbook.io/chloros/reference/sdk-reference` | Python API `chloros_sdk` đầy đủ: các lớp, chữ ký, ngoại lệ và ví dụ minh họa. Được viết dành cho các mô hình ngôn ngữ lớn (LLM). |
| **Bất kỳ trang nào dưới dạng Markdown thô** | thêm `.md` vào URL của trang | ví dụ: `https://mapir.gitbook.io/chloros/reference/sdk-reference.md` trả về trang dưới dạng Markdown thô — lý tưởng để dán vào cửa sổ bối cảnh hoặc lấy từ một tác nhân. |

Liên kết trong tài liệu hướng dẫn: [Tham khảo CLI](reference/cli-reference.md) · [Tham khảo SDK](reference/sdk-reference.md).

{% hint style="info" %}
Hai trang tham khảo này là độc lập: một trợ lý đã đọc một trong hai trang này sẽ không cần phần còn lại của tài liệu hướng dẫn để viết một tập lệnh chính xác.
{% endhint %}

## Cho phép trợ lý của bạn chạy Chlorostrực tiếp: `chloros-mcp`

Tất cả các nội dung trên đều yêu cầu trợ lý của bạn *viết* một kịch bản hoặc một lệnh. Với `chloros-mcp`, một trợ lý hỗ trợ [Giao thức Bối cảnh Mô hình](https://modelcontextprotocol.io) (MCP), chẳng hạn như Claude Desktop, Claude Code hoặc Cursor, sẽ trực tiếp điều khiển Chloros. Bạn chỉ cần yêu cầu bằng ngôn ngữ thông thường và nó sẽ gọi các công cụ của máy chủ:

* **Xử lý hình ảnh:** tạo dự án, nhập thư mục chứa các ảnh chụp, chọn cài đặt, xử lý và báo cáo kết quả xuất ra từ quá trình chạy.
* **Vận hành phần cứng:** kết nối camera LATTICE, mảng cảm biến và cảm biến ánh sáng DAQ, xem trước, thiết lập độ phơi sáng và màu sắc, chụp ảnh, ghi lại và cập nhật firmware.

```text
You:  Process the flight in D:\Flights\2026-10-17 to reflectance and NDVI.

You:  Show me what the array sees, then capture reflectance every 10 seconds
      for 5 minutes.
```

Các công cụ của máy chủ bao gồm các chức năng mà [Python SDK](api-python-sdk.md) thực hiện. Nó được cài đặt cùng với SDK, do đó thường không cần cài đặt thêm gì khác: trên Windows, trình cài đặt Chloros sẽ đặt `chloros-mcp.exe` vào thư mục `Scripts` của Python, còn trên Linux, trình cài đặt `.deb` sẽ đặt tệp `chloros-mcp` vào thư mục `/usr/bin`.

**Mã Claude:**

```bash
claude mcp add chloros -- chloros-mcp
```

**Claude Desktop:**thêm mục này vào `claude_desktop_config.json` (**Cài đặt > Nhà phát triển > Chỉnh sửa cấu hình**để mở), sau đó khởi động lại Claude Desktop.**Cursor** cũng sử dụng mục tương tự trong `~/.cursor/mcp.json`.

```json
{
  "mcpServers": {
    "chloros": {"command": "chloros-mcp"}
  }
}
```

Nếu máy khách báo lỗi máy chủ không khởi động được, có thể `chloros-mcp` không nằm trong danh sách `PATH` mà máy khách nhìn thấy: hãy cung cấp đường dẫn đầy đủ thay thế (`where chloros-mcp` trên Windows, `which chloros-mcp` trên Linux).

Bạn cần cài đặt Chloros trên cùng một máy tính và đăng nhập tại đó, trong ứng dụng hoặc thông qua `chloros-cli login`, trên bất kỳ gói Chloros+ trả phí nào (Copper trở lên). Máy chủ không thể đăng nhập thay cho bạn.

{% hint style="info" %}
Cài đặt máy khách cho từng ứng dụng, tùy chọn, từng công cụ và cách thức hoạt động của mỗi lần chạy: [MCP Server (Trợ lý AI)](reference/sdk-reference.md#mcp-server-ai-assistants) trong Tài liệu tham khảo SDK.
{% endhint %}

## Mẫu lời nhắc

Sao chép, điền vào `<placeholders>`, rồi dán vào trợ lý của bạn.

### 1. Chuyển đổi thư mục dữ liệu bay thành NDVI

```

Read https://mapir.gitbook.io/chloros/reference/cli-reference.md.
Then write a script for <Windows PowerShell | bash> that:
1. logs in with `chloros-cli login <email> '<password>'` (only needed once per machine),
2. processes the folder <path/to/flight_001> with reflectance and the NDVI index,
3. prints where each output product landed, using the reference's
   "Where the outputs land" folder rules.
```

### 2. Giám sát hàng loạt thư mục ảnh chụp

```

Read https://mapir.gitbook.io/chloros/reference/sdk-reference.md (sections
"Quickstart" and "Post-Run Summary & Hints"). Write a Python script that
watches <path/to/captures> for new flight subfolders and runs
chloros_sdk.process_folder() with indices=["NDVI"] on each new one.
After each run, print every hint from result["summary"]["hints"] and treat
a run with zero image products as a failure for that folder.
```

### 3. Kết nối mảng LATTICE và chụp ảnh

```

Read https://mapir.gitbook.io/chloros/reference/sdk-reference.md (section
"connect_array"). Write a Python script that connects my LATTICE cameras
with serials <213800234, 214000533, ...> as one synchronized array, captures
a reflectance image set into <output/> every 10 seconds for one hour, and
disconnects cleanly when done (use the context-manager form).
```

### 4. Ghi lại phổ cảm biến ánh sáng DAQ

```

Read https://mapir.gitbook.io/chloros/reference/cli-reference.md (section
"chloros-cli daq" — use only the pool-* commands). Write a script that:
1. connects my DAQ-E sensor with `chloros-cli daq pool-connect --eth-host <daq-e-xxxxxx.local>`,
2. lists the pool with `pool-list` to get the sensor id,
3. records a 10-minute calibrated .daq file named "<field-A>" with `pool-record`,
4. disconnects with `pool-disconnect`.
```

{% hint style="warning" %}
Việc viết kịch bản DAQ từ dòng lệnh luôn thông qua họ lệnh `daq pool-*` (`pool-connect`, `pool-list`, `pool-latest`, `pool-stream`, `pool-record`, `pool-set-cap`, `pool-disconnect`). Các lệnh con `daq` khác mà trợ lý của bạn có thể tạo ra không có sẵn trong các bản phát hành chính thức và sẽ kết thúc với lỗi.
{% endhint %}

## Tại sao các kịch bản do AI viết lại hoạt động tốt với Chloros

Mỗi trường hợp này đều là hành vi thực tế và đã được xác minh của Chloros 1.3.0 — chúng loại bỏ các chế độ thất bại truyền thống của tự động hóa do máy viết:

* **Không cần các bước thiết lập phức tạp.**Các hàm trợ giúp kết nối thông minh của SDK (`connect_camera`, `connect_array`, `connect_daq_sensor`) và các điểm vào xử lý (`ChlorosLocal`, `process_folder`)**tự động khởi động backend cục bộ**. Một tập lệnh được tạo ra không cần giao diện người dùng (GUI) đang mở hay máy chủ được khởi động thủ công — nó chỉ cần gói phần mềm dành cho máy tính để bàn hoặc CLI được cài đặt.
* **Toàn bộ quy trình chỉ là một lệnh gọi duy nhất.** `chloros_sdk.process_folder("path", indices=["NDVI"])` thực hiện các bước nhập dữ liệu → hiệu chuẩn → đo phản xạ → xuất chỉ số từ đầu đến cuối. Diện tích bề mặt xử lý ít hơn, do đó giảm thiểu các điểm có thể gây lỗi cho tập lệnh được tạo tự động.
* **Các lần chạy không có đầu ra sẽ tự chẩn đoán.** Sau `process()`, bản tóm tắt của lần chạy sẽ được đính kèm vào kết quả, và mọi gợi ý xử lý (ví dụ: *lý do* tại sao một lần chạy không tạo ra đầu ra) cũng được phát lại dưới dạng Python `UserWarning` — do đó, ngay cả một skript không bao giờ kiểm tra đối tượng dict kết quả cũng sẽ hiển thị chẩn đoán.
* **Giao diện dòng lệnh (CLI) báo lỗi rõ ràng.**Một phiên chạy `chloros-cli process` yêu cầu sản phẩm nhưng không ghi được gì sẽ hiển thị mã lỗi `Processing finished but wrote no image products.` và**thoát với mã thoát khác 0**, do đó các skript shell và hệ thống CI có thể phát hiện lỗi này chỉ bằng cách kiểm tra mã thoát đơn giản. Các lần chạy thành công báo cáo mã lỗi `Image products written: N`, kèm theo `(plus M clamp masks)` khi các tệp mặt nạ được ghi. Các lệnh ghi lại cũng liệt kê tên mọi tệp mà chúng không thể ghi và thoát với mã lỗi `1`.
* **Đường dẫn tương đối được hiểu theo cách mà skript mong đợi.** Với backend cục bộ, đường dẫn đầu ra tương đối (`-o out/` trong giao diện dòng lệnh (CLI), `output_dir="out/"` trong bộ công cụ phát triển phần mềm (SDK)) sẽ được giải quyết dựa trên thư mục mà lệnh hoặc skript đang chạy.

Một điểm bất đối xứng mà trợ lý cần lưu ý: lệnh `process()` trong SDK cố ý **không** gây ra lỗi khi chạy không có sản phẩm — thay vào đó, nó báo cáo thông qua phần tóm tắt/gợi ý. Nếu một đường ống Python phải dừng lại khi chạy trống, hãy kiểm tra phần tóm tắt (công thức 2 làm như vậy).

## Lưu ý

* **Yêu cầu đăng nhập Chloros+.**CLI và SDK yêu cầu gói Chloros+**trả phí**, được áp dụng ở phía máy chủ: các yêu cầu sẽ thất bại với lỗi `401 AUTH_REQUIRED` khi chưa đăng nhập và `403 PLAN_UPGRADE_REQUIRED` trên gói miễn phí. Chạy lệnh `chloros-cli login` một lần trên mỗi máy trước khi chạy các tập lệnh được tạo ra. Xem [Đăng nhập Chloros+](chloros+-login.md).
* **Các lệnh Capture điều khiển phần cứng thực tế.** Các lệnh `lattice` / `daq` / `project`, các đối tượng phiên SDK và các công cụ của `chloros-mcp` sẽ kết nối, truyền dữ liệu và kích hoạt các camera và cảm biến vật lý. Kiểm tra lại tập lệnh đã tạo trước khi chạy lần đầu tiên và chạy tập lệnh đó khi có người giám sát phần cứng.
* **Kiểm tra ngẫu nhiên các kết quả đầu ra.** Xác minh các thư mục sản phẩm và một vài giá trị pixel trước khi công bố kết quả. Đặc biệt, các tệp TIFF phản xạ được quy mô hóa theo từng nguồn — hãy đọc thẻ XMP của `Chloros:PixelScale` (các tệp LATTICE chứa thông tin này: 32768 = 1.0 phản xạ; các tệp Survey3 thì không, và sử dụng 65535 = 1.0) thay vì giả định một hệ số chia. Cả hai trang tài liệu tham khảo đều ghi rõ điều này trong phần "Đọc các pixel phản xạ".
* **Một số điểm cần lưu ý có thể gây lỗi cho mã được tạo tự động:**`pool-record` ghi vào hệ thống tệp của**máy chủ backend** (mặc định là `~/Documents/DAQ Live View/`); trên các máy có nhiều giao diện mạng, nên ưu tiên `daq pool-connect --eth-host <ip-or-hostname>` thay vì tự động phát hiện; sử dụng `http://127.0.0.1:5000` (không bao giờ dùng `localhost`) ở bất kỳ đâu có URL máy chủ phía sau xuất hiện; một skript duyệt qua cây thư mục đầu ra của dự án nên bỏ qua các thư mục `clamp_masks/` (các tệp `*_clampmask.tif` trong đó là các mặt nạ, không phải hình ảnh — xem [Định dạng hình ảnh đầu ra](output-image-formats.md#clamp-masks)); và trên hệ điều hành Windows khi tính năng đường dẫn dài bị tắt, hãy giữ cho tên thư mục dự án ngắn — thư mục dự án có độ sâu vượt quá khoảng 130 ký tự sẽ bị từ chối trước khi quá trình xử lý bắt đầu.
