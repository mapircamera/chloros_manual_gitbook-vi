# Sử dụng Chloros với các trợ lý AI

Hướng dẫn này được viết dành cho hai đối tượng: con người và các trợ lý AI mà con người ngày càng phụ thuộc vào trong công việc. Mỗi trang đều cung cấp các giá trị chính xác, giá trị mặc định và các lệnh có thể sao chép-dán, để một trợ lý (Claude, ChatGPT, Copilot, một trợ lý lập trình, …) có thể viết mã tự động hóa Chloros hoạt động ngay từ lần thử đầu tiên.

Chloros phiên bản: **

1.2.0**. CLI / SDK nền tảng: Windows 10/11 x64 và Linux (x86\_64 / Jetson aarch64).

## Những gì cần cung cấp cho trợ lý của bạn

| Tài nguyên | URL | Mục đích |
| --- | --- | --- |
| **llms.txt** | `https://mapir.gitbook.io/chloros/llms.txt` | Chỉ mục có thể đọc được bằng máy của mọi trang trong tài liệu hướng dẫn này. |
| **Tham chiếu CLI** | `https://mapir.gitbook.io/chloros/reference/cli-reference` | Bề mặt lệnh  hoàn chỉnh: mọi lệnh, cờ, giá trị mặc định, mã thoát và quy tắc thư mục đầu ra. Được viết dành cho mô hình ngôn ngữ lớn (LLM). |
| **Tham chiếu SDK** | `https://mapir.gitbook.io/chloros/reference/sdk-reference` | Python API đầy đủ của `chloros_sdk`: các lớp, chữ ký, ngoại lệ và ví dụ minh họa. Được viết để sử dụng cho LLM. |
| **Bất kỳ trang nào dưới dạng Markdown thô** | thêm `.md` vào cuối trang URL | ví dụ: `https://mapir.gitbook.io/chloros/reference/sdk-reference.md` trả về trang dưới dạng Markdown thô — lý tưởng để dán vào cửa sổ bối cảnh hoặc lấy từ một tác nhân. |

Liên kết trong tài liệu hướng dẫn: [CLI Tham khảo](reference/cli-reference.md) · [SDK Tham khảo](reference/sdk-reference.md).

{% hint style="info" %}
Hai trang tham khảo này là các đơn vị độc lập: một trợ lý đã đọc một trong hai trang này sẽ không cần phần còn lại của tài liệu hướng dẫn để viết một kịch bản chính xác.
{% endhint %}

## Các mẫu lệnh

Sao chép, điền vào `<placeholders>`, rồi dán vào công cụ hỗ trợ của bạn.

### 1. Xử lý một thư mục dữ liệu bay thành tệpNDVI

```

Read https://mapir.gitbook.io/chloros/reference/cli-reference.md.
Then write a script for <Windows PowerShell | bash> that:
1. logs in with `chloros-cli login <email> '<password>'` (only needed once per machine),
2. processes the folder <path/to/flight_001> with reflectance and the NDVI index,
3. prints where each output product landed, using the reference's
   "Where the outputs land" folder rules.
```

### 2. Giám sát theo lô một thư mục dữ liệu thu thập

```

Read https://mapir.gitbook.io/chloros/reference/sdk-reference.md (sections
"Quickstart" and "Post-Run Summary & Hints"). Write a Python script that
watches <path/to/captures> for new flight subfolders and runs
chloros_sdk.process_folder() with indices=["NDVI"] on each new one.
After each run, print every hint from result["summary"]["hints"] and treat
a run with zero image products as a failure for that folder.
```

### 3. Kết nối mảng LATTICE và thu thập dữ liệu

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
Việc viết kịch bản DAQ từ dòng lệnh luôn được thực hiện thông qua dòng lệnh `daq pool-*` (`pool-connect`, `pool-list`, `pool-latest`, `pool-stream`, `pool-record`, `pool-set-cap`, `pool-disconnect`). Các lệnh con `daq` khác mà trợ lý của bạn có thể tạo ra không có sẵn trong các bản phát hành chính thức và sẽ kết thúc với một lỗi.
{% endhint %}

## Tại sao các kịch bản do AI viết lại hoạt động tốt với Chloros

Mỗi điều này đều là hành vi thực tế và đã được xác minh của Chloros 1.2.0 — chúng loại bỏ các chế độ thất bại cổ điển của tự động hóa do máy viết:

* **Không cần các bước thiết lập phức tạp.**Các công cụ hỗ trợ kết nối thông minh của trình xử lý giao diện người dùng (SDK) (`connect_camera`, `connect_array`, `connect_daq_sensor`) và các điểm vào xử lý (`ChlorosLocal`, `process_folder`)**tự động khởi động backend cục bộ**. Một kịch bản được tạo ra không cần giao diện người dùng (GUI) đang mở hay máy chủ được khởi động thủ công — nó chỉ cần gói phần mềm desktop/CLIđược cài đặt.
* **Toàn bộ quy trình là một lệnh gọi duy nhất.** `chloros_sdk.process_folder("path", indices=["NDVI"])` thực thi các bước nhập dữ liệu → hiệu chuẩn → đo phản xạ → xuất chỉ số từ đầu đến cuối. Diện tích bề mặt ít hơn, ít điểm có thể gây lỗi cho tập lệnh được tạo ra.
* **Các lần chạy không có đầu ra sẽ tự chẩn đoán.** Sau `process()`, bản tóm tắt quá trình chạy sẽ được đính kèm vào kết quả, và mọi gợi ý xử lý (ví dụ: *lý do* tại sao một lần chạy không tạo ra đầu ra) cũng được phát lại dưới dạng Python `UserWarning` — do đó, ngay cả một skript không bao giờ kiểm tra kết quả cũng hiển thị chẩn đoán.
* **Lỗi &quot;CLI&quot; được báo cáo rõ ràng.**Một lần chạy `chloros-cli process` yêu cầu sản phẩm nhưng không ghi ra kết quả nào sẽ in ra `Processing finished but wrote no image products.` và**thoát với mã không bằng 0**, do đó các skript shell và CI có thể phát hiện lỗi này thông qua kiểm tra mã thoát đơn giản. Các lần chạy thành công báo cáo mã lỗi `Image products written: N`.

Một điểm bất đối xứng mà trợ lý cần lưu ý: mã lỗi `process()` của SDK được thiết kế để **không** kích hoạt trong trường hợp chạy không tạo ra sản phẩm nào — thay vào đó, nó sẽ báo cáo thông qua phần tóm tắt/gợi ý. Nếu một đường ống Python phải dừng lại khi chạy không tạo ra sản phẩm, hãy kiểm tra phần tóm tắt (công thức 2 thực hiện điều này).

## Lưu ý

* **Yêu cầu đăng nhập tại Chloros+.**Các lệnh CLI và SDK yêu cầu gói dịch vụ**trả phí** tại Chloros+, được kiểm soát ở phía máy chủ: các yêu cầu sẽ thất bại với lỗi `401 AUTH_REQUIRED` khi chưa đăng nhập và `403 PLAN_UPGRADE_REQUIRED` trên gói miễn phí. Chạy lệnh `chloros-cli login` một lần trên mỗi máy trước khi chạy các tập lệnh được tạo ra. Xem [Chloros+ Đăng nhập](chloros+-login.md).
* **Các lệnh ghi hình điều khiển phần cứng thực.** Các lệnh `lattice` / `daq` / `project` và các đối tượng phiên làm việc SDK kết nối, truyền phát và kích hoạt các camera và cảm biến vật lý. Kiểm tra lại tập lệnh đã tạo trước khi chạy lần đầu tiên và chạy tập lệnh đó khi có người giám sát phần cứng.
* **Kiểm tra ngẫu nhiên các kết quả đầu ra.** Xác minh các thư mục sản phẩm và một vài giá trị pixel trước khi công bố kết quả. Đặc biệt, các tệp TIFF phản xạ được chia tỷ lệ theo từng nguồn — hãy đọc thẻ XMP của `Chloros:PixelScale` (LATTICE: 32768 = độ phản xạ 1,0; Survey3: 65535) thay vì giả định một hệ số chia. Cả hai trang tài liệu tham khảo đều ghi rõ điều này trong phần &quot;Đọc các pixel phản xạ&quot;.
* **Một số điểm cần lưu ý có thể gây lỗi cho mã được tạo ra:**`pool-record` ghi vào hệ thống tệp của**máy chủ backend** (mặc định là `~/Documents/DAQ Live View/`); trên các máy có nhiều giao diện mạng, nên ưu tiên sử dụng `daq pool-connect --eth-host <ip-or-hostname>` thay vì tính năng tự động phát hiện; và sử dụng `http://127.0.0.1:5000` (không bao giờ dùng `localhost`) ở bất kỳ đâu khi xuất hiện địa chỉ URL của máy chủ backend.
