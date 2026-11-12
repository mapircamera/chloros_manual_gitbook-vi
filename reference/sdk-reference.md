# Tài liệu tham khảo Chloros Python SDK

**Phiên bản:**

1.3.0**Ngày tạo:**29/07/2026 19:19 ·**Ngày cập nhật:** 06/11/2026**Gói:** `chloros-sdk` (PyPI)**Đối tượng:** Được tối ưu hóa cho việc sử dụng LLM; dễ đọc cho con người.**Phạm vi:** Mọi lớp, hàm và hàm trợ giúp công khai được `import chloros_sdk` cung cấp, kèm theo các ví dụ có thể sao chép và dán bao gồm xử lý hình ảnh, điều khiển camera đơn, mảng đồng bộ, cảm biến DAQ và tự động hóa dự án, cùng với máy chủ `chloros-mcp` dành cho trợ lý AI.

Nếu bạn chỉ cần xem những điểm nổi bật, hãy chuyển đến:
- [Cài đặt & Bắt đầu nhanh](#cài_đặt)
- [Smart-Connect cho mảng camera LATTICE](#smart-connect-for-lattice-cameras)
- [Phiên cảm biến DAQ](#daq-sensor-sessions)
- [Công cụ hỗ trợ camera](#camera-utilities)
- [Mảng tạm thời, hồ sơ căn chỉnh & chỉ số](#transient-arrays-alignment-profiles--indices)
- [Thư mục dự án & công thức](#project-folder--recipes)
- [Tự động hóa dự án](#project-automation--chlorosproject)
- [Smart-AE / Smart-Capture](#smart-ae--smart-capture)
- [Khung hình trực tiếp trong quy trình xử lý của riêng bạn](#live-frames-in-your-own-pipeline)
- [Máy chủ MCP (Trợ lý AI)](#mcp-server-ai-assistants)

> **Thay đổi trong phiên bản 1.3.0** (những gì một kịch bản 1.2.0 hiện có sẽ phát hiện).
>
> **Các thay đổi mà kịch bản 1.2.0 có thể cần xử lý:**
>
> - **Một bản phát hành mà hệ thống nền không xác nhận hiện sẽ gây ra lỗi.** `CameraSession.disconnect()`, `ArraySession.disconnect()` / `force_disconnect()` trên một mảng mà phiên làm việc đã mở, `DAQSensorSession.disconnect()` / `force_disconnect()` trên cảm biến mà phiên làm việc đã mở, và `ChlorosProject.disconnect_all()` sẽ gây ra lỗi `ChlorosConnectError` khi phần nền từ chối việc giải phóng hoặc không xác nhận kịp thời, vì thiết bị có thể vẫn đang bị giữ. Trong phiên bản 1.2.0, các sự kiện này được trả về một cách im lặng. Việc thoát khỏi khối `with` vẫn không bao giờ gây ra sự kiện.
> - **Một thiết bị được mượn sẽ không bao giờ bị giải phóng.** `with attach_array(...)` và lệnh `disconnect()` rõ ràng trên thiết bị đó sẽ giữ cho mảng tiếp tục hoạt động (trong phiên bản 1.2.0, việc thoát khỏi khối sẽ giải phóng thiết bị, ngay cả khi GUI đang sở hữu nó); hãy gọi `force_disconnect()` khi bạn muốn kết thúc nó. Tương tự, lệnh `connect_daq_sensor()` khi phát hiện cảm biến đã có trong nhóm (bao gồm cả tính năng phát hiện thông minh với cờ 0) sẽ để nó mở trên `disconnect()` và khi thoát khỏi khối `with` của nó.
> - **Các định dạng PNG và JPEG chỉ hỗ trợ các sản phẩm `display` và chỉ mục.** `capture()` với `format="png"` / `"jpg"` (`ext=".png"` / `".jpg"`) và mức `radiance` hoặc `reflectance`, hoặc `processing="all"`, sẽ kích hoạt `ValueError` trước bất kỳ yêu cầu nào; một giá trị 12-bit `raw` / `debayered` sẽ bị hệ thống phía sau từ chối. Xem [PNG / JPEG](#png--jpeg-take-the-display-and-index-products-only).
> - **`connect_daq_sensor(sensor_id=...)` mở cảm biến đó hoặc kích hoạt** `ChlorosConnectError` liệt kê những gì nó tìm thấy; nó không bao giờ chuyển sang cảm biến khác. Xem [Pinned Transport](#pinned-transport).
> - **`frame_stream()` phát tín hiệu thay vì im lặng:** `FrameFetchError` khi bị từ chối, ở lần tick tiếp theo sẽ lặp lại (một lần đọc độ phản xạ mà không bao gồm bất kỳ dữ liệu DAQ nào), sau 60 giây không có khung dữ liệu, và sau 60 giây không có tín hiệu kích hoạt mới. Xem [Một tín hiệu kích hoạt cho mỗi lần đọc](#one-trigger-per-read--group_id).
> - **Thời gian chờ đọc hết hạn hoặc kết nối bị ngắt sẽ gây ra lỗi `ChlorosConnectError`** từ mọi cuộc gọi smart-connect và `analyze_array_network()`, trong đó phiên bản 1.2.0 đã báo lỗi thời gian chờ hoặc lỗi kết nối cơ bản.
> - **`connect_array()` chờ 600 giây theo mặc định (`timeout`, 180 giây trong phiên bản 1.2.0), và kết nối mà nó từ bỏ sẽ được hủy bỏ.** Lệnh gọi gửi mã lỗi `timeout` đến hệ thống backend, hệ thống này sẽ khôi phục trạng thái ban đầu của mọi camera và không thực hiện kết nối nào nếu quá trình kết nối không hoàn tất kịp thời; lệnh gọi sẽ gây ra mã lỗi `ChlorosConnectError` (lỗi HTTP 504 `CLIENT_DEADLINE_PASSED`). Trong phiên bản 1.2.0, một kết nối được thực hiện ngay sau khi camera được bật nguồn có thể kéo dài hơn thời gian thực thi của lệnh và kết nối mảng sau khi lỗi đã được kích hoạt. `connect_all()` chờ đợi từng mảng đã lưu theo cách tương tự (120 giây trong phiên bản 1.2.0). Xem [`connect_array()` Signature](#connect_array-signature).
> - **`connect_array()` giữ nguyên cảm biến đầy đủ.** Nếu không có `width` / `height` / `binning`, mọi camera đều được thiết lập ở chế độ cảm biến đầy đủ và tốc độ khung hình sẽ giảm xuống mức mà máy chủ và mạng cung cấp; phiên bản 1.2.0 sẽ thu nhỏ hoặc gộp các khung hình để duy trì tốc độ. Xem [Mảng đồng bộ](#synchronized-array--arraysession-smart-prep).
> - **Định dạng pixel Bayer trên camera M3M (mono) chính là độ sâu của nó.** Camera M3M chỉ hỗ trợ các định dạng Mono, và các cài đặt sẵn được đặt tên theo định dạng Bayer: trong phiên bản 1.2.0, máy ảnh từ chối định dạng này và giữ nguyên định dạng của riêng mình, trong khi `settings.pixel_format` đặt tên cho định dạng Bayer. `LatticeCamera(preset=...)`, `apply_preset()`, `set_format()`, `CameraSettings(pixel_format=...)`, `connect_camera(preset=...)` và một mảng'định dạng chia sẻ hiện đã thiết lập định dạng Mono có cùng độ sâu trên M3M: `"BayerRG8"` và `high_speed` tạo thành `Mono8`; `"BayerRG12"`, `high_quality` và `triggered` tạo thành `Mono12`; `cam.settings.pixel_format` đặt tên cho nó. Ngược lại cũng vậy: M3C không bao giờ được đặt ở định dạng Mono, vì vậy `"Mono12"` — từ bất kỳ cái nào trong số này, `connect_array(pixel_format=...)`, `set_settings(pixel_format=...)` hoặc định dạng lưu trữ của mảng — sẽ tạo ra `BayerRG12` trên M3C, bất kể máy ảnh nào là máy chủ của mảng. Một đối tượng `CameraSettings` mà bạn truyền vào sẽ giữ nguyên tên của nó, do đó cùng một đối tượng vẫn yêu cầu từng máy ảnh cung cấp độ sâu đó trong chính nhóm của nó. `connect_camera(preset=...)` giữ nguyên định dạng của preset (phiên bản 1.2.0 thiết lập máy ảnh ở chế độ 12-bit sau khi áp dụng cài đặt sẵn), và một máy ảnh được mở dựa trên cài đặt sẵn sẽ không còn ghi dữ liệu đã thu được vào `PRESETS` nữa. Bất kỳ định dạng nào khác mà máy ảnh từ chối (một định dạng không có trong danh sách `PixelFormat` của nó, chẳng hạn như `RGB8` trên M3M hoặc `BayerRG16` trên firmware không hỗ trợ định dạng đó) cũng sẽ không được đặt tên riêng: sau khi kết nối với `LatticeCamera`, `apply_settings()` hoặc `set_format()`, `cam.settings.pixel_format` và `packed_transport` chỉ định định dạng luồng video của camera, và một dòng in ra chỉ định định dạng mà hệ thống từ chối.
> - **`analyze_array_network()` dự đoán kết nối nào được áp dụng:** `pixel_format=None` (trước đây là `"BayerRG8"`) chỉ định các định dạng mà kết nối thiết lập (Mono12 / BayerRG12), còn `packed="auto"` thể hiện quyết định về phương thức truyền tải nén, do đó giá trị fps được báo cáo có thể khác với 1.2.0'. Một dự án `pixel_format` mà bạn truyền qua sẽ tạo ra một tệp M3M ở độ sâu đó. Xem [Trợ giúp Phân tích Mạng](#network-analysis-helper).
> - **`CameraSession.capture()` trả về một `CaptureResult`** (vẫn là danh sách các từ điển khung) chứa `.skipped`, `.failed`, `.daq_files` và `.daq_matches`. Kết quả của hàm `ArraySession.capture()` cũng chứa `.alignment` (liệu hiệu ứng biến dạng căn chỉnh có được áp dụng hay không, và tại sao không) và `.oriented`.
> - **Việc thu thập mảng được căn chỉnh theo mặc định, giống như các thành phần của ứng dụng.** `ArraySession.capture()`, `continuous()`, `capture_repeated()` và `ArrayHandle.capture()` / `capture_smart()` thực hiện hiệu chỉnh biến dạng cho các tệp `debayered`, `display`, `radiance`, `reflectance` và các tệp chỉ mục vào khung hình của camera chính và cắt chúng theo vùng chồng chéo (nhỏ hơn cảm biến do độ lệch của máy ảnh, `XMP-mapir:AlignmentJSON` `"applied": true`), do đó chúng là các tệp giống như ảnh chụp cùng cảnh của ứng dụng Chloros; `raw` vẫn giữ nguyên định dạng gốc. Trước đây, khi chụp mà không có `aligned=True`, mọi tệp đều được lưu ở kích thước gốc, chưa được chỉnh sửa biến dạng. `aligned=False` giữ nguyên điều đó. `chloros-mcp`'`capture_array` và `start_capture_run` sử dụng cùng một giá trị mặc định. Xem [Các phương thức `ArraySession`](#arraysession-methods).
> - **`connect_all()` chỉ định một backend từ chối SDK**. Một backend đang chạy nhưng đã đăng xuất hoặc sử dụng gói dịch vụ thấp hơn Copper sẽ được báo cáo dưới mã lỗi `errors` kèm lý do riêng (HTTP 401 / 403), chứ không phải là "không có backend Chloros nào đang chạy", và mỗi lần gọi `connect_all()` sẽ kiểm tra lại backend, do đó việc gọi lại sau khi đăng nhập sẽ hoạt động trên cùng một đối tượng dự án.
> - **Một đường dẫn tương đối `output_dir` hoạt động với backend cục bộ.** Các đường dẫn tương đối sẽ được chuyển đổi thành đường dẫn tuyệt đối so với thư mục làm việc của skript trước khi được gửi đi. Trong phiên bản 1.2.0, backend giải quyết các đường dẫn này trong thư mục cài đặt của chính nó, dẫn đến lỗi "Quyền truy cập bị từ chối".
> - **`numpy` hiện là yêu cầu bắt buộc** của SDK (các lệnh gọi live-frame trả về mảng numpy).
> - **Tên cấp độ xuất không phải là số sẽ bị từ chối.** `processing=` và `levels=` nhận các giá trị `raw`, `debayered`, `radiance`, `reflectance` hoặc `display` (hoặc một tên gọi khác như `preview`; `processing=` cũng như `"all"`). Bất kỳ tên nào khác đều bị từ chối trước khi chạm vào camera (HTTP 400 `UNKNOWN_LEVEL`: `capture()` gây ra lỗi `ChlorosConnectError`, `grab()` / `frame_stream()` `FrameFetchError`); phiên bản 1.2.0 đã lưu hoặc trả về `raw` cho trường hợp này.
> - **Chức năng Texture Aware bị từ chối đối với các bản chụp LATTICE.** Trên một dự án chứa các bản chụp LATTICE, `configure(debayer="Texture Aware (Slow, Highest Quality)")` gây ra lỗi `ChlorosConfigurationError`, và `process()` cũng gây ra lỗi tương tự sau khi có một `configure()` yêu cầu trước khi nhập. Giá trị Texture Aware mà dự án đã lưu trước đó được đặt thành Standard, kèm theo lỗi `UserWarning`. Các bản chụp LATTICE luôn sử dụng thuật toán demosaic tiêu chuẩn, do đó phiên bản 1.2.0 đã chấp nhận cài đặt này và bỏ qua nó; ứng dụng Chloros cũng không cung cấp tùy chọn này cho chúng. Xem [Các giá trị được hỗ trợ](#supported-values).
> - **Các máy ảnh sẽ trở về trạng thái ban đầu — trừ khi có thông báo lỗi `keep_settings=True`.** `ArraySession.disconnect()` / `force_disconnect()`, phần cuối của mọi hàm tạm thời (`capture_cameras_transient()`, `start_multi_camera_stream()`, `test_array_sync()`, `calibrate_alignment_profile()`, `capture_aligned_multiband()`, `start_aligned_stream()`, `capture_live_index()`) và việc dừng `start_camera_continuous()` sẽ đưa mỗi máy ảnh trở lại trạng thái ban đầu: kết nối dây kích hoạt và dây đồng bộ, PTP, cùng mọi cài đặt máy ảnh đã được thay đổi trong phiên (thời gian phơi sáng và độ khuếch đại, mục tiêu AE, giới hạn và độ giảm chấn, cân bằng trắng, mức đen, định dạng pixel, ghép pixel và vùng quan tâm (ROI), lật hình, tốc độ khung hình). `keep_settings=True` thay vào đó lưu các cài đặt của từng máy ảnh vào bộ nhớ riêng của máy ảnh, để máy ảnh khởi động với các cài đặt đó; `clear_settings=True` đặt lại bộ nhớ đó về cài đặt gốc. Dù bằng cách nào, các camera vẫn được để chạy tự do. Một dự án sẽ thiết lập các camera của mình từ các cài đặt đã lưu sẵn khi kết nối chúng. Xem [Bộ nhớ camera](#camera-memory--keep_settings--clear_settings).
> - **Mọi chế độ đều lưu dự án theo cùng một cách.** Bất kỳ thay đổi nào đối với camera, mảng hoặc cảm biến ánh sáng được kết nối trong một dự án — từ SDK, ứng dụng, CLI hoặc máy chủ MCP — đều được lưu vào tệp `cameras.json` / `sensors.json`, và việc mở dự án trong bất kỳ chế độ nào sẽ thiết lập các thiết bị dựa trên dự án đó; một `ChlorosProject` đặt tên cho thư mục riêng của nó trên hệ thống backend. Trước đây, chỉ có ứng dụng mới lưu dự án. Xem [Một bản ghi dự án, mọi chế độ](#one-project-record-every-mode).
> - **Các tệp được tạo từ nhiều camera sẽ ghi rõ tên từng camera.** Một chồng ảnh đa dải, một chỉ mục trực tiếp được tạo từ các dải của nhiều camera và hình ảnh chụp mảng `composite/` chứa số sê-ri, mẫu máy và gói hiệu chuẩn của từng camera nguồn dưới dạng danh sách (`XMP-mapir:SourceCameraSerials`, `SourceCameraModels`, `SourceCalBundleSha256s`, …), và danh sách EXIF `SerialNumber` liệt kê các số sê-ri, được phân tách bằng dấu phẩy. Trước đây, chúng không chứa thông tin về máy ảnh nào, hoặc chỉ chứa số sê-ri của máy ảnh đầu tiên. Xem [Các tệp được tạo từ nhiều máy ảnh](#files-made-from-several-cameras).
> - **Chỉ tệp `raw` mới chứa các thẻ hiệu chuẩn của máy ảnh.** Bộ tương thích với MicaSense (`BlackLevel`, `RadiometricCalibration`, các bộ cài đặt-kênh `Band{R,G,B}RadiometricCalibration`, các bộ `MicaSenseVignetting*`) và phần dữ liệu `CalibrationJSON` mô tả các giá trị thô của cảm biến, do đó chúng chỉ được chứa trong tệp ghi hình `raw`. Tất cả các tệp khác — các tệp ghi dữ liệu`debayered`, `display`, `radiance`, `reflectance` và các tệp chỉ mục, các khung hình ghi liên tục và đa camera, cũng như mọi tệp sản phẩm `process()` — đều mang thông tin nhận dạng của camera (`XMP-mapir:CameraSerial`, `CameraModel`, `FilterType`, `LensModel`, `CalBundleSha256`, ...) và, trong mỗi lần chụp, thông tin ghi hình riêng của khung hình (`XMP-mapir:FrameJSON`: độ phơi sáng, độ khuếch đại, nhiệt độ, định dạng pixel, thời gian chụp). Trong phiên bản 1.2.0, chúng cũng chứa bộ dữ liệu thô (raw set), mà công cụ áp dụng (Pix4D, Metashape) sẽ áp dụng cho các pixel không phải là giá trị thô. Quá trình xử lý xác định thông số hiệu chỉnh của tệp này dựa trên số seri và `CalBundleSha256`.
>
> **Các thay đổi khác:**
>
> - **Tốc độ khung hình tự động của mảng sẽ ổn định theo giá trị mà mảng cung cấp.** Tốc độ nào mất hơn 1% số lần kích hoạt sẽ giảm xuống, được đánh giá qua nhiều lần quan sát kéo dài thay vì chỉ một lần, và tốc độ mà nó đã giảm xuống từ đó sẽ được thử lại thỉnh thoảng. Tốc độ do bạn tự thiết lập sẽ không bị thay đổi.
> - **Chế độ phơi sáng tự động ổn định hơn.** Chế độ này duy trì ở mức ánh sáng không đổi, ổn định trong vòng vài giây sau khi có thay đổi, bỏ qua các vùng sáng nhỏ và khôi phục hoạt động của máy ảnh khi máy ngừng phản hồi dưới ánh nắng chói chang. Xem [Cách hoạt động của chế độ phơi sáng tự động](#how-auto-exposure-behaves).
> - **`import_images(recursive=True)` chỉ tính các hình ảnh và bản ghi của bạn.** Một thư mục chứa tệp `camera_cal/` của dự án sẽ không còn tính các tệp hiệu chuẩn đó là hình ảnh và bản ghi, và một dự án đã nhập chúng trước đó sẽ loại bỏ chúng.
> - **`configure(radiometric_output=...)` được áp dụng.** Giờ đây, nó thiết lập các sản phẩm `Export radiance` / `Export reflectance` của dự án; trước đây, cài đặt sản phẩm riêng của từng dự án sẽ được ưu tiên hơn, do đó `radiance`, `sensor-response` và `all` đều xuất dữ liệu độ rạng rỡ và độ phản xạ.
> - **`test_array_sync()` khởi động từ trạng thái bật nguồn của các camera.** Nó bật PTP của các camera cho quá trình chạy, chờ đồng hồ của chúng khóa và ổn định trước lần kích hoạt đầu tiên (`ptp_lock`) — mỗi camera khóa trong 120 giây, khoảng 2 phút kể từ khi bật nguồn (`ptp_settle_s=` thiết lập), vì một đồng hồ vừa khóa có thể lệch tới ~200 us trong khoảng 2 phút — và khôi phục cài đặt PTP của từng camera về trạng thái ban đầu. Một camera vẫn đang đồng bộ từ lần thử nghiệm trước sẽ không bị chờ đợi thêm. Trước đây, hệ thống thường tắt PTP trên mạng chỉ có một bộ chuyển mạch khi không có thiết bị DAQ-E nào phản hồi, và đọc đồng hồ chạy tự do của từng camera như một đồng hồ PTP không thể đồng bộ ("PTP chưa được khóa", trong mỗi lần chạy).
> - **Một bản ghi `.daq` cho biết nắp bảo vệ của nó đã được chọn hay chỉ được giả định.** DAQ-U, DAQ-M và DAQ-E được xuất xưởng mà không có nắp, và Chloros không thể xác định liệu có nắp được lắp hay không, do đó một cảm biến được kết nối mà không có tên nắp sẽ được gán mã `sunshine_cosine` theo mặc định, đây là một *giả định*. Mỗi bản ghi Chloros (sơ đồ 1.30) hiện lưu trữ `cap_id_source` bên cạnh `cap_id`: `operator` khi bạn đặt tên cho nắp (bao gồm cả `none`), `auto_default` khi đó là giá trị mặc định. Khi xử lý một bản ghi mà tên nắp được giả định, với cài đặt nắp DAQ của dự án để ở chế độ Tự động, hệ thống sẽ in ra một `[daq-dls] WARNING` để đặt tên cho tệp: một DAQ-U không có bộ lọc dưới chế độ hiệu chỉnh ánh sáng mặt trời sẽ hiển thị giá trị cao gấp khoảng 30 lần. **Đặt tên cho bộ lọc bạn đã lắp đặt** khi kết nối (`connect_daq_sensor(cap_id=...)` hoặc `DAQSensorSession.set_cap()`, `chloros-cli daq pool-connect --cap-id` / `daq pool-set-cap`, hoặc tùy chọn tụ điện trong menu thả xuống của GUI), hoặc thiết lập tụ điện DAQ cho dự án, và cảnh báo sẽ biến mất.
> - **Không còn cửa sổ console pip trên Windows nữa.** Trên máy tính có cài đặt Python, backend và từng image worker của nó trước đây đều chạy lệnh ``pip`` một lần khi khởi động, và mỗi quá trình đều mở một cửa sổ console. Hiện tại, điều này không còn xảy ra nữa.
> - **Các bản ghi `.daq` trực tiếp vẫn tồn tại sau khi mất điện.**Một bản ghi được ghi lại kèm theo nhật ký và đồng bộ hóa toàn bộ đĩa sau mỗi lần cam kết, phía sau cảm biến, do đó đĩa chậm sẽ làm chậm tệp nhưng không bao giờ làm chậm các giá trị đo. Một bản ghi bị dừng giữa chừng khi commit (do mất điện, bị ngắt đột ngột) có thể để lại tệp `<name>.daq-journal` bên cạnh:**hãy sao chép cả hai tệp này lại với nhau.** Chloros sẽ khôi phục nhật ký khi lần đầu tiên đọc tệp, do đó bản ghi sẽ mở ra với toàn bộ dữ liệu đã được cam kết trước khi dừng (khoảng 3 giây cuối cùng bị mất); trong phiên bản 1.2.0, việc mất điện vào thời điểm không thích hợp có thể khiến toàn bộ tệp không thể đọc được ("hình ảnh đĩa cơ sở dữ liệu bị lỗi").
> - **`DAQSensorSession.record_start()` trên đĩa đang bận không còn làm chậm cảm biến nữa**: các giá trị đo được xếp hàng và ghi trên một luồng riêng, và giá trị nào không thể ghi được sẽ được tính vào `status()` của `record_errors` (cùng với `record_last_error`).
> - **`LatticeCamera` trực tiếp:**một khung hình không có thông số phơi sáng/độ khuếch đại theo khối giờ đây báo cáo giá trị**thực** về phơi sáng và độ khuếch đại của máy ảnh (trước đây, cơ chế dự phòng sẽ đọc giá trị được lưu trong bộ nhớ đệm, có thể bị "đóng băng" trong suốt quá trình phơi sáng tự động), và `apply_settings(trigger_source='LineN')` / `set_trigger_source('LineN')` áp dụng cùng cơ chế loại bỏ nhiễu đầu vào 5 µs mà các thiết bị nô lệ qua cáp của Chloros nhận được (không qua bộ lọc, khung hình được kích hoạt bởi thiết bị nô lệ mà không có tín hiệu kích hoạt được gửi đi).
> - **Các máy ảnh kích hoạt bằng phần mềm không còn bỏ qua mỗi tín hiệu kích hoạt thứ hai.** Một máy ảnh ở chế độ kích hoạt bằng phần mềm hiện sẽ xếp hàng tín hiệu kích hoạt đến trong quá trình đọc dữ liệu (hoặc Chloros sẽ đợi cho đến khi máy ảnh sẵn sàng), giống như các máy ảnh đồng bộ qua cáp đã làm từ trước.
> - **`discover_daq_sensors()` đặt tên cho DAQ-E-S là `DAQ-E-S`.** `model` trước đây là `DAQ-E` cho mọi cảm biến Ethernet; cảm biến nào không trả lời kiểm tra danh tính trong khoảng 2 giây vẫn được liệt kê, với tên `DAQ-E`.
> - **Mới: `chloros-mcp`, một máy chủ MCP dành cho trợ lý AI.** Claude Desktop, Claude Code, Cursor và các ứng dụng khách MCP khác có thể xử lý hình ảnh (tạo dự án, nhập ảnh chụp, chọn cài đặt, xử lý, báo cáo kết quả xuất ra) và điều khiển camera LATTICE, mảng camera và cảm biến ánh sáng DAQ: kết nối, xem trước, thiết lập độ phơi sáng và màu sắc, chụp ảnh, ghi lại. Các công cụ của nó bao quát các chức năng mà SDK này thực hiện. Nó được cài đặt cùng với SDK, và gói .deb cho Linux sẽ đặt nó vào thư mục `/usr/bin`. Xem [Máy chủ MCP (Trợ lý AI)](#mcp-server-ai-assistants).
> - **Mới: tiện ích camera**, các lệnh `chloros-cli lattice` dưới dạng các lệnh gọi SDK: cấu hình màu, độ bão hòa / độ tương phản, vùng sáng, cân bằng trắng (một lần, tự động, tỷ lệ cố định), cài đặt sẵn, mô hình bộ lọc, trạng thái và cập nhật firmware, nguồn (kiểm tra, ngủ, thức), hiệu chuẩn mục tiêu, bộ điều hợp mạng (kiểm tra, ước tính, sửa), hiệu chỉnh hiệu ứng viền tối trên tệp, đọc DLS và kiểm tra máy chủ. Xem [Công cụ máy ảnh](#camera-utilities).
> - **Mới: phần còn lại của CLI dưới dạng các lệnh SDK**, do đó CLI, SDK và `chloros-mcp` cung cấp các tính năng giống nhau (CLI chỉ giữ lại chức năng đăng nhập, cấu hình DAQ-E và các máy chủ DAQ trực tiếp, vì lý do bảo mật): mở camera bằng một lệnh duy nhất (`start_camera_continuous()`, `start_multi_camera_stream()`, `capture_cameras_transient()`, `test_array_sync()`, `detect_array_master()`, `analyze_array_models()`), các tệp cấu hình căn chỉnh (`calibrate_alignment_profile()`, `capture_aligned_multiband()`, `start_aligned_stream()`, `alignment_profile_info()`, `reorder_alignment_profile()`), các chỉ mục (`index_catalog()`, `compute_index()`, `capture_live_index()`), tệp PTP grandmaster (`time_sync_status()` và các tệp liên quan), thư mục dự án (`project_folder()`, `set_project_folder()`, `reset_project_folder()`) và các công thức (`run_project_recipe()`, trình chạy riêng của CLI). Đối với cảm biến ánh sáng: `connect_daq_sensor(cap_id=...)`, `DAQSensorSession.set_cap()`, `daq_bindings()` / `set_daq_bindings()`, `daq_band_presets()` và `light_sensor_reflectance()` (độ phản xạ và chỉ số Môi trường xung quanh + Đối tượng của hai cảm biến); đối với camera: `CameraSession.status()` và `capture_repeated()`; đối với một mảng: `ArraySession.capability()` và `set_member_settings()` (thông số phơi sáng, hệ số khuếch đại, v.v. của một thành viên). Xem [Mảng tạm thời, Hồ sơ căn chỉnh & Chỉ số](#transient-arrays-alignment-profiles--indices), [Thư mục dự án & Công thức](#project-folder--recipes) và [Phiên cảm biến DAQ](#daq-sensor-sessions).
> - **Các sản phẩm chỉ số LATTICE được cải thiện** (chỉ số NDVI và các chỉ số khác) từ `process()`, trong mọi chế độ xử lý.
> - **Camera LATTICE OCN: các sản phẩm độ sáng, độ phản xạ và chỉ số được cải thiện dưới ánh sáng mặt trời.** Sửa lỗi đội hình được áp dụng sau khi tách dải — trực tiếp, trong các lần thu thập và trong `process()`; camera RGN không thay đổi. Xem [Đầu ra đo quang](#radiometric-output-lattice-multispectral-pipeline).
> - **Mỗi lần chụp đều lưu lại giá trị DAQ đã sử dụng, giống như giao diện người dùng (GUI).** `CameraSession.capture()` và `ArraySession.capture()` / `continuous()` ghi tệp `.daq` và tệp `.match.json` mỗi khi một DAQ được liên kết hoặc gộp vào nhóm, bao gồm cả bản ghi chỉ dữ liệu thô; trong phiên bản 1.2.0, chỉ có một bản chụp thô hoặc đã loại bỏ hiệu ứng Bayer được lưu với `force_daq=True`. `daq=False` tắt tính năng này và `daq_tolerance_ms=` thiết lập dung sai khớp; cả hai kết quả đều cho biết những gì đã được lưu trong `.daq_files` và `.daq_matches`. Hiện tại, quá trình ghi hình sẽ chờ đến lần đọc tiếp theo của DAQ để có thể kết hợp các giá trị đọc ở cả hai bên của khoảng thời gian phơi sáng. Xem [Giá trị đọc từ DAQ được lưu trong quá trình ghi hình](#the-daq-reading-a-capture-saves--daq_files-and-daq_matches).
> - **`ArraySession` có các khung hình trực tiếp và chế độ ghi liên tục phía máy chủ.** `grab()` và `frame_stream()` trả về các khung hình numpy từ một tín hiệu kích hoạt (cùng một thao tác đọc như các hàm `ArrayHandle.grab()` / `frame_stream()` trong API của dự án), và `continuous()` lưu mọi nhóm đồng bộ hóa trong hệ thống nền và trả về một [`ContinuousHandle`](#continuoushandle). Xem [Khung hình trực tiếp trong quy trình xử lý của riêng bạn](#live-frames-in-your-own-pipeline).
> - **Mảng `grab()` được ghim vào một tín hiệu kích hoạt** (`ArrayHandle.grab()` và `ArraySession.grab()` mới). Trong phiên bản 1.2.0, 12 trong số 20đọc dữ liệu từ hai bộ kích hoạt. Hiện tại, mỗi khung hình đều chứa `group_id`, và một mảng `frame_stream()` chỉ trả về kết quả cho mỗi bộ kích hoạt một lần. Xem [Một tín hiệu kích hoạt cho mỗi lần đọc](#one-trigger-per-read--group_id).
> - **Khung hình theo dải tần và đã căn chỉnh:** `grab()` / `frame_stream()` lấy `band=`, `align=`, `bit_depth=` và `refl_max=` trên các đối tượng camera và mảng. Xem [Xuất một dải tần duy nhất](#exporting-a-single-band--band).
> - **Các phương thức và tham số mới của `ArraySession`:** `set_target_fps()`, `force_disconnect()`, `capture(after_seq=..., orient=...)` (cùng với `CaptureResult.group_seq` và `.failed`), `continuous()`, `connect_array(packed=...)`; `capture_repeated()` nối các nhóm liên tiếp và dừng khi đã hoàn tất.
> - **`ArraySession.record()` ghi dữ liệu khi không có GUI nào đang mở.** Khi chế độ xem kết hợp của GUI bị đóng, phần nền sẽ tự hiển thị chế độ xem: mảngNDVI của mảng, hoặc chỉ số mà bạn truyền vào như `expr=`. `RecorderHandle.stop()` sẽ kích hoạt `ChlorosConnectError` cho một bản ghi bị lỗi, tức là bản ghi không ghi được khung hình nào, và giữ kết quả dừng lại tại `.result` (việc để lại khối `with` sẽ không gây ra ngoại lệ: hãy kiểm tra `rec.result`). Xem [`RecorderHandle`](#recorderhandle).
> - **Các phương thức `DAQSensorSession` mới:** `enable_ptp()`, `force_disconnect()`, và đối với các cài đặt riêng của DAQ-E là `device_info()`, `set_network()`, `set_auto_stream()`, `reboot()` và `update_firmware()`. `record_stop()` trả về `warning` và `gaps`. Việc khởi động lại đã đóng một bản ghi sẽ đặt tên cho bản ghi đó trong phản hồi của nó hoặc, khi việc khởi động lại thất bại, trên `ChlorosConnectError` (`recording_closed`; xem [Cài đặt Thiết bị DAQ-E](#daq-e-device-settings)). Xem [Các phương pháp `DAQSensorSession`](#daqsensorsession-methods).
> - **Một DAQ-E được gộp chung mà ngừng truyền dữ liệu sẽ được kết nối lại bởi hệ thống nền**, với các cài đặt ban đầu của nó, và bản ghi trên đó sẽ tiếp tục trong cùng một phiên `.daq` với khoảng trống được đánh dấu trong tệp; `record_stop()` trả về các khoảng trống dưới dạng `gaps`. Trong phiên bản 1.2.0, cảm biến sẽ im lặng cho đến khi bị ngắt kết nối và kết nối lại, điều này sẽ kết thúc quá trình ghi dữ liệu. Xem phần tham khảo CLI [Một DAQ-E ngừng truyền dữ liệu](cli-reference.md#a-daq-e-that-stops-delivering).
> - **`open_project()` hoạt động dựa trên SDK đã cài đặt.** Trình cài đặt (các tệp cài đặtvà PyPI) hiện đã chứa các trình tải dự án cần thiết; trước đây, `open_project()` đã gây ra lỗi `ImportError` liên quan đến `project_cameras` / `project_sensors` trong bất kỳ SDK nào được cài đặt qua pip. `connect_all()` kết nối các camera, mảng và cảm biến ánh sáng đã lưu thông qua một backend đang chạy (cảm biến ánh sáng thông qua nhóm DAQ của nó), và một thiết bị đã lưu mà nó không thể tìm thấy sẽ được liệt kê dưới `errors` kèm theo lý do thay vì bị bỏ qua mà không có thông báo. Một mảng đã lưu đang hoạt động trên nền (của GUI, hoặc của `chloros-cli project connect`) sẽ được kết nối và tiếp tục chạy, trong khi trước đây nó được báo lỗi (“Camera  <sn>đã có trong mảng <id>”). Xem [Các phương thức của `ChlorosProject`](#chlorosproject-methods).
> - **`ChlorosLocal()`, `process_folder()` và `process_lattice_capture()` tìm Chloros trên Windows.** Chúng tìm kiếm vị trí mà trình cài đặt đặt backend (mặc định là `<install folder>\backend\`, `C:\Program Files\Chloros\backend\`, một thư mục tùy chỉnh từ bản ghi của chính trình cài đặt), và kết nối với một backend đang chạy mà không cần tìm kiếm thêm. Trong phiên bản 1.2.0, chúng sẽ báo lỗi "Không tìm thấy backend Chloros" trên máy đã cài đặt trừ khi bạn truyền tham số `backend_exe=`.
> - **Một backend bị treo ngay khi khởi động sẽ được báo cáo ngay lập tức.** Khi `ChlorosLocal()` hoặc một lệnh smart-connect khởi động một backend mà backend đó thoát ra trước khi trả lời, lệnh gọi sẽ báo lỗi trong vòng vài giây, với mã lỗi `ChlorosBackendError` từ `ChlorosLocal` và `ChlorosConnectError` từ các lệnh gọi smart--connect, kèm theo mã thoát, lệnh đã chạy và các dòng cuối cùng mà backend đã in ra. Phiên bản 1.2.0 chỉ chờ hết thời gian chờ khởi động 60 giây mà không giải thích lý do. Xem [Khởi động tự động backend](#backend-auto-start).
> - **Backend tiếp tục chạy giữa các kịch bản.** Khi thoát khỏi khối `with ChlorosLocal(...)`, `process_folder()` / `process_lattice_capture()` trả về và `chloros-mcp` thoát ra sẽ không còn làm dừng backend mà SDK đã khởi động; kịch bản tiếp theo sẽ tái sử dụng nó. Nó sẽ dừng lại tại `logout()` (trừ khi ứng dụng Chloros đang mở hoặc có phiên chạy đang diễn ra), tại `shutdown_backend()`, hoặc tự động sau 30 phút nếu không có gì sử dụng nó (`CHLOROS_BACKEND_IDLE_MIN` trong môi trường của backend sẽ thay đổi điều này; `0`: không bao giờ).
> - **Một backend do SDK khởi chạy sẽ được yêu cầu tắt, không bị buộc dừng.** `shutdown_backend()` cho phép backend giải phóng các camera và cảm biến ánh sáng của mình cũng như hoàn tất bất kỳ quá trình ghi hình nào; nó chỉ bị chấm dứt nếu vẫn đang chạy sau 10 giây. Phiên bản 1.2.0 đã chấm dứt nó ngay lập tức (trên Windows, là việc buộc dừng hoàn toàn), điều này có thể khiến cảm biến ánh sáng DAQ kết nối với backend đó không phản hồi. Một backend mà SDK phát hiện đang chạy vẫn được để nguyên.
> - **Trên Linux, tính năng tự khởi động (auto-start) không bao giờ chạy lệnh `backend_server.py` từ thư mục hiện tại.** Khi không có backend nào được cài đặt, một tập lệnh được khởi chạy trong thư mục chứa backend sẽ chạy nó; hiện tại chỉ backend đã cài đặt mới được khởi chạy (hoặc, đối với `ChlorosLocal`, backend được `backend_exe=` chỉ định).
> - **`configure(indices=...)`**thêm từng cài đặt sẵn vào dự án cho các dải tần của camera tương ứng, bao gồm cả dự án mới, và hiển thị cảnh báo (`UserWarning`) nếu không thể thêm tên đó.**`import_images()`** báo cáo các bản ghi `.daq` dưới dạng `scan_count` và không còn cảnh báo về thư mục lưu trữ LATTICE nữa.
> - **`read_image_audit_tags()` hoạt động khi cài đặt qua pip.** Nó tự đọc metadata của hình ảnh; trong phiên bản 1.2.0, nó trả về `{}` mà không cần các thành phần nội bộ của bản cài đặt trên máy tính để bàn trong đường dẫn Python.
> - **`capture()` ngay sau `connect_array()`** sẽ chờ tối đa 20 giây cho nhóm khung hình đầu tiên của mảng thay vì báo lỗi HTTP 503 "Mảng vẫn đang khởi động".
> - **`status()` báo cáo tình trạng bị treo.** Trường mới `stalled` có giá trị True trong khi vòng lặp thu thập đang chạy nhưng không xuất bản dữ liệu nào, và `actual_fps` sau đó bằng 0. Trong phiên bản 1.2.0, `actual_fps` giữ nguyên giá trị cuối cùng trong suốt thời gian bị đình trệ, do đó mảng bị đình trệ hiển thị `actual_fps > 0` trong khi quá trình chụp thất bại với lỗi 503 “Không có nhóm khung hình đồng bộ mới”.
> - **Giảm số lượng bộ khung hình không hoàn chỉnh và lỗi 503 khi ghi hình:** đầu vào kích hoạt của mỗi camera phụ hiện đã được loại bỏ nhiễu.
> - **Truyền tải 12-bit nén** (`connect_array(packed="auto")`) giúp tăng tốc độ khung hình của mảng cảm biến toàn phần lên khoảng một phần ba. Xem [Truyền tải 12-bit nén](#packed-12-bit-transport-packed).
> - **Thời gian phơi sáng mảng trong vùng ROI:** một vùng ROI nhỏ không còn giới hạn thời gian phơi sáng của thành viên ở mức chu kỳ hoạt động tự do của cảm biến (4,9 ms ở độ phân giải 768 × 304). Xem [Thời gian phơi sáng trên các thành viên mảng](#exposure-on-array-members).
> - **Chế độ phơi sáng tự động không còn làm giảm tốc độ khung hình tự động của mảng cảm biến.** Một mảng được đồng bộ hóa qua cáp sẽ chạy ở tốc độ mà mạng và tốc độ đọc dữ liệu của các camera cho phép; chế độ phơi sáng tự động sẽ lấp đầy khoảng thời gian mà tốc độ đó cung cấp, sau đó tăng độ khuếch đại. Một thời gian phơi sáng thủ công dài hơn khoảng thời gian đó sẽ làm chậm tín hiệu kích hoạt để phù hợp với nó thay vì bị từ chối. Xem [Thời gian phơi sáng trên các thành viên mảng](#exposure-on-array-members).
> - **`read_nodes()`**thêm `frame_values` và `notes` trong khi chế độ phơi sáng tự động đang hoạt động (xem [`read_nodes()` trong chế độ phơi sáng tự động](#read_nodes-under-auto-exposure--frame_values-and-notes)). Khi máy ảnh từ chối một mức phơi sáng hoặc độ khuếch đại,**`set_settings()`** sẽ thông báo phạm vi mà máy ảnh chấp nhận và khôi phục chế độ tự động mà trước đó đã bị tắt cho giá trị đó; `set_settings(target_brightness_auto=True)` là mã lỗi mới.
> - **`orient=`** trên `ArraySession.capture()` / `capture_fastest()` / `continuous()` / `burst()` và `ArrayHandle.capture()` / `capture_smart()` xoay các sản phẩm theo hướng của camera. Xem [Các phương thức của `ArraySession`](#arraysession-methods).
> - **`DLS()` cần có `mac_address` riêng của cảm biến.** Không còn địa chỉ MAC mặc định nữa (trước đây là địa chỉ của một đơn vị); việc sử dụng `DLS()` mà không có địa chỉ MAC sẽ gây ra lỗi `DLSError`. Xem [Độ lớn dải tần DAQ-M](#daq-m-band-magnitudes--dlsband_magnitudes).
> - **Trình cài đặt Windows** cung cấp các tùy chọn "Bật đường dẫn dài của Windows" và "Loại trừ backend Chloros khỏi quá trình quét của Microsoft Defender", cả hai đều được chọn mặc định. Xem [Bước 1](#step-1--install-the-chloros-platform-package).
> - **Gói .deb cho Linux** cài đặt Python SDK cho hệ thống `python3` (trước đây, nó chỉ đặt gói wheel lên đĩa), tăng dung lượng bộ đệm nhận socket của kernel mà các luồng camera cần, và thêm một dịch vụ backend chạy dưới quyền người dùng của bạn. Gói Jetson là `chloros_1.3.0_arm64_jp6.deb`, dành cho JetPack 6, kèm theo PyTorch CUDA của JetPack. Xem [Bước 1](#step-1--install-the-chloros-platform-package) và [Bước 2](#linux-deb).

---

## Kiến trúc trong 60 giây

SDK là một lớp Python mỏng phủ lên phần nền Chloros (cùng máy chủ Flask mà giao diện người dùng trên máy tính để bàn và CLI sử dụng). Để tự động hóa, bạn nhập `chloros_sdk` và gọi các phương thức cấp cao; bên trong, mỗi lệnh gọi sẽ trở thành một yêu cầu HTTP gửi đến backend cục bộ trên cổng 5000 — `http://127.0.0.1:5000/api/...` (cố ý không dùng `localhost`, vì trên Windows, tên này sẽ được giải quyết thành `::1` trước và tốn khoảng 2 giây cho mỗi yêu cầu khi kết nối với backend chỉ hỗ trợ IPv4). Backend quản lý nhóm phần cứng — camera, cảm biến DAQ, cấu hình căn chỉnh, bộ đệm khung hình — do đó các skript SDK có thể hoạt động song song với giao diện người dùng (GUI) mà không phải cạnh tranh về cổng nối tiếp hay băng thông NIC.

Có ba giao diện bạn sẽ sử dụng:

1. **`ChlorosLocal` + các hàm tự do** (`process_folder`, `process_lattice_capture`) — Quy trình xử lý hình ảnh. Chạy toàn bộ thư mục qua các bước hiệu chuẩn / loại bỏ hiệu ứng Bayer / xuất chỉ mục chỉ bằng một lệnh Python.
2. **Các công cụ kết nối thông minh** (`connect_camera`, `connect_array`, `connect_daq_sensor`) — Mở một phiên làm việc nền liên tục cho phần cứng trực tiếp. Quy trình "smart-prep" tương tự như trên GUI: kiểm tra mạng, tự động chọn cấp độ, PTP, gieo hạt AE, cấu hình kích hoạt GPIO.
3. **`ChlorosProject` / `open_project`** — Tải một dự án đã lưu (thư mục chứa `cameras.json` + `sensors.json` + `project.json`), kết nối các camera, mảng cảm biến và cảm biến ánh sáng của dự án đó cùng lúc, và điều khiển việc chụp ảnh thông qua các handle được đặt tên.

Các bề mặt 1 và 2 **tự động khởi động một backend cục bộ** nếu chưa có backend nào đang lắng nghe (cùng một tệp nhị phân đi kèm mà GUI/CLI khởi tạo) — do đó, một tập lệnh đơn giản có thể hoạt động từ một shell mới mà không cần bạn khởi động backend trước: `ChlorosLocal` (và `process_folder()` / `process_lattice_capture()`), `connect_camera()`, `connect_array()`, `connect_daq_sensor()`, `discover_lattice_cameras()` và `discover_daq_sensors()`. `list_cameras()`, `list_arrays()`, `list_daq_sensors()`, `attach_array()` và `analyze_array_network()` không bao giờ tự khởi động: chúng cần một backend đang chạy. Truyền tham số `auto_start_backend=False` để tắt tính năng này (ví dụ: khi trỏ đến một backend từ xa, vốn không bao giờ được khởi tạo). Xem [Tự động khởi động backend](#backend-auto-start). Surface 3 hoạt động khác biệt: `open_project()` không nhận tham số `auto_start_backend`, và `connect_all()` không bao giờ khởi tạo backend — nó sẽ kiểm tra `http://127.0.0.1:5000` một lần và nếu không có phản hồi, nó sẽ chuyển sang điều khiển thiết bị trực tiếp (không cần backend) `lattice_sdk`. Điều này yêu cầu môi trường chạy Arena SDK và thư viện OpenCV (`cv2`) có thể nhập vào Python của bạn, mà gói SDK không cài đặt; nếu thiếu chúng, mỗi camera và mảng đã lưu sẽ được báo lỗi kèm lý do. Các cảm biến ánh sáng của dự án không có đường dẫn không cần backend: khi không có backend nào đang chạy, mỗi cảm biến đã lưu sẽ được báo lỗi kèm lý do (xem [`SensorHandle`](#sensorhandle)). Chỉ có `proj.process()` và `stream(..., overlays=True)` mới tạo ra một `ChlorosLocal()` một cách lười biếng (điều này sẽ tự động khởi động).

Cả ba đều được xác thực-gated: chạy `chloros-cli login` một lần trên máy, hoặc đăng nhập qua GUI trên máy tính để bàn. Lỗi phát sinh khi gọi mà không có phiên hợp lệ phụ thuộc vào bề mặt (xem [Lỗi](#exceptions)): `ChlorosAuthenticationError` từ `ChlorosLocal` (và `process_folder()` / `process_lattice_capture()`), `ChlorosConnectError` từ các lệnh-connect và các phiên của chúng, `status: "error"` kèm theo `error_code` từ `analyze_array_network()`, và một mục dưới `errors` trong báo cáo `connect_all()`.

Yêu cầu:
- Python 3.7+ (theo thông báo của gói phần mềm; được phát triển/kiểm thử trên phiên bản 3.10)
- Đã cài đặt Chloros Desktop trên máy cục bộ (tệp nhị phân backend được tích hợp sẵn trong trình cài đặt)
- Tài khoản Chloros+ đang hoạt động trên **bất kỳ gói Chloros+ trả phí nào (Copper trở lên)**; gói miễn phí**Iron**không có quyền truy cập SDK/CLI. Quy định này được áp dụng**từ phía máy chủ**: mọi yêu cầu có gắn cờ SDK/CLI đều phải kèm theo cả phiên làm việc đang hoạt động và gói dịch vụ trả phí. Gói dịch vụ dưới mức Copper sẽ nhận được mã lỗi `403` kèm theo `error_code: PLAN_UPGRADE_REQUIRED` (`ChlorosLicenseError` từ các cuộc gọi `ChlorosLocal`, `ChlorosConnectError` từ các cuộc gọi smart-connect). Người gọi đã đăng xuất sẽ nhận được mã trạng thái `401` / `AUTH_REQUIRED` thay vì (`ChlorosAuthenticationError` từ `ChlorosLocal`, `ChlorosConnectError` từ các cuộc gọi smart-connect) — hai mã này khác nhau vì việc chạy lại `chloros-cli login` sẽ khắc phục lỗi `401` nhưng không thể khắc phục lỗi `403`, lỗi này cần nâng cấp gói dịch vụ.
- Việc sử dụng ngoại tuyến được hỗ trợ trong thời gian ân hạn của gói dịch vụ: cấp độ dịch vụ được đọc từ bộ nhớ đệm xác thực máy chủ (5 phút) hoặc bộ nhớ đệm giấy phép đã ký và gắn với máy (30 ngày đối với gói hàng tháng, đến khi hết hạn đăng ký đối với gói hàng năm). Khi thời gian ân hạn này hết hiệu lực, gói dịch vụ sẽ chuyển sang phiên bản miễn phí và quyền truy cập SDK/CLI sẽ bị ngừng cho đến khi thiết bị có thể kết nối với máy chủ ít nhất một lần. Lỗi `chloros-cli status` (`GET /api/license-status`) vẫn có thể truy cập được trên gói miễn phí nên nguyên nhân lỗi vẫn hiển thị.
- Windows 10/11 64-bit, **Ubuntu 22.04 LTS hoặc mới hơn**, hoặc Jetson (JetPack 6). Ubuntu 20.04**không** được hỗ trợ: các phụ thuộc của `.deb` được dẫn xuất từ những gì backend liên kết, bao gồm `libc6 (>= 2.35)`, và Focal đi kèm với glibc 2.31.

---

## Cài đặt

Python SDK là một lớp Python mỏng phủ lên backend Chloros. Trừ khi bạn chạy backend trên một máy khác (xem [Chế độ Backend Từ xa](#remote-backend-mode-pip-only-host-via-tunnel)), bạn cần **cài đặt gói Chloros dành cho máy tính để bàn tại địa phương** (trình cài đặt Windows hoặc Linux `.deb`), bao gồm cả các cảm biến ánh sáng DAQ. Gói này cung cấp tệp nhị phân backend, môi trường chạy Arena SDK cho camera LATTICE và các gói hiệu chuẩn.

Tải xuống phiên bản mới nhất: [`https://mapir.gitbook.io/chloros/download`](https://mapir.gitbook.io/chloros/download)

### Bước 1 — Cài đặt gói nền tảng Chloros

#### Windows (.exe)

1. Tải xuống `Chloros Installer 1.3.0.exe` từ trang tải xuống.
2. Chạy trình cài đặt và làm theo hướng dẫn của trình hướng dẫn. Đường dẫn cài đặt mặc định là `C:\Program Files\Chloros\`.
3. Khởi chạy Chloros ít nhất một lần và đăng nhập bằng tài khoản Chloros+ của bạn.

Trình cài đặt cho phép phần nền (`chloros-backend.exe`, không có gì khác) đi qua Tường lửa Windows trên mọi cấu hình mạng: UDP vào, dành cho luồng camera, PTP, mDNS và luồng DAQ-E, cùng với TCP vào chỉ từ mạng con cục bộ, dành cho kết nối mà DAQ-E thiết lập trở lại máy tính trong quá trình cập nhật firmware ([`update_firmware()`](#daq-e-device-settings)). Trình gỡ cài đặt sẽ xóa cả hai quy tắc này. API HTTP HTTP của phần nền vẫn hoạt động trên cổng `127.0.0.1` trong mọi trường hợp.

Nhóm **Cài đặt hệ thống** của trình hướng dẫn có hai tùy chọn, cả hai đều được chọn theo mặc định:

| Tùy chọn | Chức năng | Tắt cài đặt im lặng |
| --- | --- | --- |
| **Bật đường dẫn dài của Windows** | Cho phép các thư mục dự án nằm ở cấp độ sâu hơn giới hạn 260 ký tự của Windows. Khi tắt đường dẫn dài (mặc định của Windows), thư mục dự án nằm quá sâu so với các sản phẩm của nó sẽ bị từ chối trước khi bất kỳ thao tác nào được xử lý — xem [Khắc phục sự cố](#troubleshooting-pointers). | `/MERGETASKS="!longpaths"` |
| **Loại trừ backend Chloros khỏi quá trình quét của Microsoft Defender** | Ngăn chặn quá trình quét theo thời gian thựcmỗi tệp mà camera ghi lại. Ở tốc độ ghi cao, quá trình quét chiếm khoảng 30% tốc độ ghi: được đo tại ROI 512 trên mảng 9 camera, ghi liên tục ở định dạng raw đạt 52,7 khung hình/giây khi có quét và 77,7 khung hình/giây khi loại trừ backend. | `/MERGETASKS="!defenderexclusion"` |

Quá trình ghi hình liên tục báo cáo tải của Defender là `defender_cores` (xem [`ContinuousHandle`](#continuoushandle)).

#### Linux amd64 (.deb)

```bash
sudo apt install ./chloros_1.3.0_amd64.deb
chloros-cli --version
chloros-cli login user@example.com 'YourPassword'
```

Cài đặt bằng `apt install ./…`, không phải `dpkg -i`: apt cũng cài đặt các yêu cầu của Python SDK, mà gói này khuyến nghị chứ không bắt buộc (xem [Bước 2](#linux-deb)).

#### Linux arm64 — Jetson (JetPack 6)

```bash
sudo apt install ./chloros_1.3.0_arm64_jp6.deb
chloros-cli --version
chloros-cli login user@example.com 'YourPassword'
```

Dành cho Jetson Orin / Orin NX / Orin Nano trên JetPack 6 (L4T R36). PyTorch của nó là bản dựng CUDA của JetPack 6, do đó việc xử lý GPU diễn ra trên GPU của Orin; nó sử dụng các thư viện CUDA và cuDNN mà JetPack 6 cài đặt (`sudo apt install nvidia-jetpack` trên một hình ảnh thiếu các thư viện này).

#### Hệ điều hành Linux: bộ đệm nhận và dịch vụ nền

Cả hai gói .deb đều tăng kích thước bộ đệm nhận socket của kernel (cần thiết cho luồng dữ liệu từ camera LATTICE) lên 32 MB (`/etc/sysctl.d/60-chloros-gige.conf`). Ở mức mặc định của kernel, các gói dữ liệu từ camera sẽ bị kernel loại bỏ và mảng dữ liệu sẽ mất các khung hình cũng như tốc độ khung hình; `chloros-cli lattice network` sẽ thông báo khi dung lượng bộ đệm xuống thấp. Xem phần [Máy chủ Linux — bộ đệm nhận socket](cli-reference.md#linux-hosts--socket-receive-buffers) trong tài liệu tham khảo CLI.

Trên máy không có giao diện đồ họa, hãy chạy backend dưới dạng dịch vụ **với tư cách người dùng của bạn**, để skript có thể tìm thấy một tiến trình đang chạy (`connect_all()` không bao giờ khởi động backend):

```bash
sudo systemctl enable --now chloros-backend@$USER.service    # $USER expands to your user name
```

Sau đó, backend sẽ chạy dưới quyền của bạn: các dữ liệu ghi lại mà nó tạo ra thuộc về bạn, và nó sử dụng tài khoản `chloros-cli login` của bạn. Tài khoản root `chloros-backend.service` vẫn được cài đặt cho các máy đã kích hoạt tính năng này, nhưng mọi tệp mà backend chạy dưới quyền root tạo ra đều thuộc về root. Xem phần [Chạy backend dưới dạng dịch vụ](cli-reference.md#running-the-backend-as-a-service-headless) trong tài liệu tham khảo CLI.

Một backend do dịch vụ chạy là chung cho tất cả, không bao giờ bị khách hàng dừng lại: các tập lệnh SDK và `chloros-cli` sử dụng nó khi tìm thấy (`ChlorosLocal.shutdown_backend()` chỉ dừng backend mà chính `ChlorosLocal` đã khởi động), và `GET /api/status` đặt tên đơn vị là `service`. Khởi động lại bằng `sudo systemctl restart chloros-backend@$USER.service`. Việc nâng cấp gói phần mềm sẽ dừng các dịch vụ backend trong quá trình cài đặt, do đó, một skript đang chạy sẽ mất kết nối với các camera và cảm biến, sau đó khởi động lại tất cả các thiết bị đã được kích hoạt; việc gỡ bỏ gói phần mềm sẽ dừng các dịch vụ này và vô hiệu hóa các thiết bị đã được kích hoạt. Chỉ có một backend chạy tại một thời điểm, vì vậy hãy kích hoạt một thiết bị: thiết bị thứ hai sẽ từ chối khởi động và giữ trạng thái `failed` (trạng thái thoát 98, không khởi động lại), và thông báo `systemctl status` sẽ giải thích lý do. Để chuyển đổi giữa các đơn vị, hãy vô hiệu hóa đơn vị mà bạn không sử dụng.</id></sn>

### Bước 2 — Cài đặt Python SDK

**Trình cài đặt Chloros đi kèm với một gói SDK dạng wheel tương ứng.** Mỗi trình cài đặt dành cho Windows và gói .deb dành cho Linux sẽ tạo ra một tệp `chloros_sdk-X.Y.Z-py3-none-any.whl` trên đĩa, hoàn toàn tương thích với phiên bản GUI / CLI / backend. Bạn không cần phải theo dõi PyPI để đảm bảo sự đồng bộ.

#### Windows

Trình cài đặt sẽ tự động chạy tệp `pip install` với gói wheel đi kèm bằng phiên bản Python của hệ thống (ưu tiên trình khởi chạy `py.exe`, nếu không có sẽ chuyển sang `python -m pip`). Không cần thực hiện thao tác nào — `import chloros_sdk` sẽ hoạt động trong môi trường Python của bạn sau khi cài đặt thành công. Nếu máy không có Python, trình cài đặt sẽ tự động bỏ qua bước này và GUI + CLI vẫn tiếp tục hoạt động.

#### Linux (.deb)

Cả hai gói .deb (x86_64 và Jetson) đều cài đặt SDK cho hệ thống `python3`, do đó `import chloros_sdk` sẽ hoạt động ngay sau khi cài đặt, mà không cần pip hay kết nối mạng. Các gói (`chloros_sdk`, `lattice_sdk`, `lattice_cam`, `arena_api` và các trình tải dự án) được đưa vào `/usr/lib/python3/dist-packages`, và lệnh `chloros-mcp` của SDK ([máy chủ MCP](#mcp-server-ai-assistants)) vào `/usr/bin`, tất cả đều thuộc sở hữu của gói phần mềm: việc nâng cấp sẽ thay thế chúng và việc gỡ bỏ gói phần mềm sẽ xóa chúng.

Các yêu cầu riêng của SDK, `python3-numpy` và `python3-requests`, là **được khuyến nghị, không bắt buộc** (GUI, CLI và backend là các thành phần độc lập): `sudo apt install ./chloros_<version>_amd64.deb` (`…_arm64_jp6.deb` trên Jetson) sẽ cài đặt chúng, trong khi `dpkg -i` thì không. Kết quả cài đặt báo cáo phiên bản SDK mà `python3` nhập vào, bất kỳ yêu cầu nào còn thiếu, và một bản `chloros_sdk` ở vị trí khác trên đường dẫn của `python3` đang che lấp bản sao của gói (chẳng hạn như bản đã được cài đặt trước đó bằng `pip install --user`).

Trong môi trường virtualenv (không truy cập được các gói hệ thống), hãy cài đặt tệp wheel đi kèm vào đó:

```bash
pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl
```

Đối với các triển khai Jetson không kết nối mạng (air-gapped), quá trình này hoàn toàn diễn ra ngoại tuyến — SDK và tệp wheel đã có sẵn trên đĩa.

#### PyPI công khai

Đối với các máy chủ chỉ sử dụng pip (không cài đặt gói máy tính để bàn Chloros; lúc này SDK sẽ điều khiển một backend trên máy khác, xem [Chế độ Backend Từ Xa](#remote-backend-mode-pip-only-host-via-tunnel)):

```bash
pip install chloros-sdk
```

PyPI cung cấp bản phát hành ổn định mới nhất. Hãy đảm bảo phiên bản SDK trùng khớp với phiên bản backend Chloros mà nó điều khiển (xem [Quản lý phiên bản & Tương thích](#versioning--compatibility)). Trình trợ giúp DAQ-M `DLS` cũng cần `bleak`: `pip install "chloros-sdk[camera]"` (hoặc `pip install bleak`).

#### Xác minh

```python
import chloros_sdk
print(chloros_sdk.__version__)
print("CAMERA_AVAILABLE =", chloros_sdk.CAMERA_AVAILABLE)
print("PROJECT_AVAILABLE =", chloros_sdk.PROJECT_AVAILABLE)
```

> **Yêu cầu đăng ký Chloros+.** Tất cả các lệnh gọi SDK đều yêu cầu đăng nhập Chloros+ đang hoạt động. Chạy `chloros-cli login user@example.com 'YourPassword'` một lần trên mỗi máy; thông tin đăng nhập được lưu trong bộ nhớ đệm trong `~/.chloros/`.

### Tôi có cần Gói Desktop không?

Chỉ riêng gói pip là **không** đủ cho hầu hết các quy trình làm việc. Dưới đây là những gì mỗi bề mặt SDK cần:

| Bề mặt SDK | Có cần Gói Desktop không? | Tại sao |
| --- | --- | --- |
| `ChlorosLocal`, `process_folder`, `process_lattice_capture` | **Có**(để khởi động một máy chủ)**/ Không** (nếu backend đã có sẵn) | Sử dụng backend đang chạy (của ứng dụng máy tính để bàn hoặc một skript khác đã được khởi chạy); nếu không, tự động khởi động phiên bản đã cài đặt: `/usr/lib/chloros/chloros-backend` (hệ điều hành Linux) hoặc `<install folder>\backend\chloros-backend.exe` (Hệ điều hành Windows; mặc định là `C:\Program Files\Chloros\backend\`, thư mục cài đặt tùy chỉnh được đọc từ bản ghi của trình cài đặt). Truyền tham số `backend_exe=` để tự đặt tên cho nó. |
| `chloros-mcp` ([máy chủ MCP](#mcp-server-ai-assistants)) | **Có**(để khởi động một máy chủ)**/ Không** (đã có máy chủ nền sẵn sàng) | Được xây dựng trên `ChlorosLocal`, nên giống như dòng trên. |
| `read_image_audit_tags` | **Không** | Đọc metadata của chính hình ảnh đã xử lý (chỉ thư viện chuẩn Python). |
| `connect_camera`, `connect_array`, `connect_daq_sensor`, `analyze_array_network`, `list_*`, `discover_*` | **Có**(cục bộ)**/ Không**(từ xa) | Các client HTTP thuần túy qua backend. Backend cục bộ → yêu cầu gói phần mềm trên máy tính để bàn. Backend từ xa → `backend_url=`**qua đường hầm** (xem Chế độ Backend Từ Xa — các backend được cung cấp sẵn chỉ kết nối với loopback). |
| `ChlorosProject` / `open_project` | **Có** | Truy cập các dự án đã lưu qua máy chủ phía sau. |
| Các lớp LATTICE trực tiếp (`LatticeCamera`, `CameraPool`, `Calibration`, `DLS`, …) | **Có** | Cần có môi trường chạy Arena SDK (được cung cấp sẵn trong gói phần mềm máy tính để bàn) và OpenCV (`cv2`) trong môi trường Python của bạn, mà cả gói phần mềm máy tính để bàn lẫn SDK đều không cài đặt (`pip install opencv-python`). Nếu không, `CAMERA_AVAILABLE` sẽ được nhập dưới dạng `False`. `DLS` cũng cần `bleak`. |

### Chế độ Remote-Backend (máy chủ chỉ dùng pip, qua đường hầm)

> **Backend đã cài đặt không thể truy cập được qua mạng LAN.** Nó chỉ lắng nghe trên
> cổng loopback (`127.0.0.1` / `::1`), do đó `backend_url="http://<lan-ip>:5000"`
> **không thể kết nối với Chloros đã cài đặt**. Để điều khiển một backend trên một
> máy khác, hãy tự chuyển tiếp cổng loopback của nó và chỉ định SDK đến
> đường hầm:

```bash
# on the pip-only host: forward local 5000 to the Chloros machine's loopback
ssh -N -L 5000:127.0.0.1:5000 user@chloros-host
```

```python
import chloros_sdk

BACKEND = "http://127.0.0.1:5000"     # the tunnel endpoint
serials = ["213800234", "214000533"]   # master first

# auto_start_backend=False: the tunnel URL looks local, so without it a tunnel
# that is down would start this machine's own backend, if one is installed.
arr = chloros_sdk.connect_array(serials, backend_url=BACKEND,
                                auto_start_backend=False)
daq = chloros_sdk.connect_daq_sensor(eth_host="daq-e-1.local",
                                     backend_url=BACKEND,
                                     auto_start_backend=False)

# The backend opens every path on ITS machine: pass absolute paths valid there.
# (A relative path would be made absolute against this machine's working folder.)
arr.capture("/home/chloros/captures/run1", processing="raw")
```

Các máy chủ không màn hình / CI / robotics có thể duy trì một máy tính có cài đặt desktop đầy đủ làm "máy chủ Chloros" và `pip install chloros-sdk` ở mọi nơi khác — nhưng kết nối giữa chúng là đường hầm do người dùng thiết lập ở trên, không phải URL LAN trực tiếp.

> **`ChlorosLocal` trên một máy chủ chỉ chạy pip.** `ChlorosLocal(backend_url=BACKEND)` kết nối với một backend mà nó có thể truy cập mà không cần cài đặt máy tính để bàn: nó chỉ tìm kiếm tệp nhị phân backend cục bộ khi bắt buộc phải KHỞI ĐỘNG một backend (không có phản hồi và `auto_start_backend=True`). Cho đến phiên bản 1.2.0, nó tìm kiếm tệp nhị phân trước và báo lỗi `ChlorosBackendError` (“Không tìm thấy backend Chloros…”) trên máy chủ chỉ chạy pip ngay cả khi có backend có thể truy cập được. Các thư mục bạn truyền vào (`process_folder`, `import_images`) sẽ được kiểm tra trên máy này và được backend đọc, do đó khi sử dụng backend từ xa, tên thư mục trên cả hai máy phải trùng khớp, chẳng hạn như một ổ đĩa chia sẻ.

### Quy trình làm việc chỉ dành cho DAQ (máy chủ chỉ chạy pip)

`pip install chloros-sdk` một mình không thể điều khiển cảm biến DAQ: cảm biến DAQ luôn được điều khiển thông qua backend Chloros với `connect_daq_sensor()`:

- **Cảm biến trên máy này:** cài đặt gói phần mềm máy tính để bàn Chloros tại đây. `connect_daq_sensor()` sẽ khởi động backend của nó nếu chưa có backend nào đang chạy.
- **Cảm biến trên máy khác:** máy đó cần có gói phần mềm máy tính để bàn; kết nối với backend của máy đó qua đường hầm trong [Chế độ Backend Từ Xa](#remote-backend-mode-pip-only-host-via-tunnel). Backend sẽ mở cảm biến trên chính máy của nó, vì vậy một thiết bị DAQ-U được cắm vào máy đó, một thiết bị DAQ-M nằm trong phạm vi Bluetooth của nó, và một thiết bị DAQ-E nằm trên mạng mà nó có thể kết nối được.

```python
import chloros_sdk

# http://127.0.0.1:5000 is the local desktop install's backend, or the tunnel.
with chloros_sdk.connect_daq_sensor() as daq:   # smart-detect USB / BLE / ETH
    print(daq.model, daq.transport, daq.address)
    for frame in daq.latest(n=5):
        print(frame["spectrum"][:10])
```

Dù bằng cách nào, máy chủ cũng áp dụng gói Chloros+: đăng nhập vào máy chạy máy chủ (`chloros-cli login` hoặc ứng dụng máy tính để bàn) bằng bất kỳ gói trả phí nào (Copper trở lên). Hệ thống backend chưa đăng nhập và gói dịch vụ dưới mức Copper đều sẽ gây ra lỗi `ChlorosConnectError` kèm theo lý do từ hệ thống backend (`AUTH_REQUIRED` / `PLAN_UPGRADE_REQUIRED`).

---

## Hướng dẫn nhanh

```python
import chloros_sdk

# === Image processing ===
results = chloros_sdk.process_folder(
    "C:/DroneImages/Flight001",
    indices=["NDVI", "GNDVI", "OSAVI"],
)

# === Live LATTICE single-cam ===
with chloros_sdk.connect_camera("213800234") as cam:
    cam.set_settings(exposure_time=10000, gain=0.0)
    cam.capture("output/")

# === Live LATTICE synchronized array (GUI smart-prep flow) ===
with chloros_sdk.connect_array(
        ["213800234", "214000533", "214701288", "214701292"]) as arr:
    arr.capture("output/", processing="reflectance")

# === Live DAQ spectral sensor ===
with chloros_sdk.connect_daq_sensor() as daq:    # smart-detect USB / BLE / ETH
    for frame in daq.latest(n=5):
        print(frame["spectrum"][:10])

# === Drive a saved project end-to-end ===
proj = chloros_sdk.open_project("/path/to/project")
proj.connect_all()
proj.arrays["main_rig"].capture("output/", processing="reflectance")
proj.disconnect_all()
```

---

## Chỉ mục API cấp cao nhất

```python
import chloros_sdk

# === Image processing (full pipeline) ===
chloros_sdk.ChlorosLocal                          # class
chloros_sdk.process_folder(...)                   # one-shot helper
chloros_sdk.process_lattice_capture(...)          # LATTICE-friendly defaults
chloros_sdk.read_image_audit_tags(path)           # post-run audit

# === Live cameras (persistent backend pool) ===
chloros_sdk.connect_camera(serial, ...)           # → CameraSession
chloros_sdk.connect_array(serials, ...)           # → ArraySession (smart-prep)
chloros_sdk.attach_array(serials_or_id, ...)      # → ArraySession (attach without re-connecting)
chloros_sdk.list_cameras()
chloros_sdk.list_arrays()
chloros_sdk.discover_lattice_cameras()
chloros_sdk.analyze_array_network(...)            # network capability + recommendation
chloros_sdk.CaptureResult                         # list subclass returned by CameraSession.capture / ArraySession.capture
chloros_sdk.RecorderHandle                        # handle for an array record()/burst() job
chloros_sdk.ContinuousHandle                      # handle for an array continuous() capture
chloros_sdk.GrabResult                            # dict returned by grab() / yielded by frame_stream()
chloros_sdk.FrameFetchError                       # raised by grab()/frame_stream() on a refusal

# === Live DAQ sensors (persistent backend pool) ===
chloros_sdk.connect_daq_sensor(...)               # → DAQSensorSession
chloros_sdk.discover_daq_sensors()                # scan USB/BLE/ETH (finds a DAQ-M MAC)
chloros_sdk.list_daq_sensors()
chloros_sdk.daq_bindings() / set_daq_bindings(cameras=, arrays=)   # which DAQ feeds which camera
chloros_sdk.daq_band_presets(filter_code=None)    # band sets for Ambient + Object reflectance
chloros_sdk.light_sensor_reflectance(ambient_id, object_id, ...)   # two DAQs -> reflectance + indices
chloros_sdk.time_sync_status() / time_sync_peers() / time_sync_cameras()   # PTP grandmaster
chloros_sdk.restart_time_sync() / set_time_sync_priority(priority1=, priority2=)

# === Camera utilities (the CLI's lattice commands; by serial) ===
chloros_sdk.camera_color_profile(serial) / set_camera_color_profile(serial, ...)
chloros_sdk.camera_color_adjust(serial) / set_camera_color_adjust(serial, ...)
chloros_sdk.camera_highlights(serial) / set_camera_highlights(serial, value)
chloros_sdk.camera_white_balance_once(serial) / set_camera_white_balance(serial, ...)
chloros_sdk.camera_presets() / apply_camera_preset(serial, preset)
chloros_sdk.camera_filter_models() / camera_filter_model_info(name)
chloros_sdk.camera_firmware_status() / update_camera_firmware(serial)
chloros_sdk.camera_power_probe(serial) / sleep_camera(serial) / wake_camera(serial)
chloros_sdk.calibrate_camera_with_target(serial, ...)
chloros_sdk.network_adapters(...) / fix_network_settings()
chloros_sdk.apply_vignette_correction(input_dir, output_dir, ...)
chloros_sdk.read_dls(mac, ...) / probe_system(...) / system_info()

# === Transient cameras, alignment profiles, indices (cameras opened for one command) ===
chloros_sdk.start_camera_continuous(serial, ..., daq=None, daq_tolerance_ms=None)  # → CliJob; a DAQ bound to the camera records one .daq per frame (daq=False: none)
chloros_sdk.start_multi_camera_stream(serials, ...)   # → CliJob
chloros_sdk.capture_cameras_transient(serials, ...)
chloros_sdk.test_array_sync(serials, count, ptp_settle_s=None) / detect_array_master(line) / analyze_array_models(models)
chloros_sdk.calibrate_alignment_profile(serials, save_path, ...)
chloros_sdk.capture_aligned_multiband(profile_path, ...) / start_aligned_stream(profile_path, ...)  # → dict / CliJob
chloros_sdk.alignment_profile_info(path) / reorder_alignment_profile(path, ...)
chloros_sdk.index_catalog() / compute_index(input_path, ...) / capture_live_index(profile_path, ...)
chloros_sdk.CliJob                                # stats() / stop() / wait()

# === Project lifecycle ===
chloros_sdk.open_project(path, relink=None)       # → ChlorosProject
chloros_sdk.project_folder() / set_project_folder(path) / reset_project_folder()
chloros_sdk.run_project_recipe(recipe, project_path)  # `chloros-cli project run`
chloros_sdk.ChlorosProject                        # class
chloros_sdk.AlignmentSpec                         # dataclass
chloros_sdk.ArrayHandle, CameraHandle, SensorHandle

# === Direct-hardware (no-backend) classes (from lattice_sdk) ===
chloros_sdk.LatticeCamera, CameraSettings, PRESETS, CameraPool
chloros_sdk.Calibration, CalibrationCoefficients, FilterModel, list_filters
chloros_sdk.DLS, NetworkDiagnostics               # DLS: DAQ-M over Bluetooth; needs bleak

# === Exceptions ===
chloros_sdk.ChlorosError                          # base
chloros_sdk.ChlorosBackendError
chloros_sdk.ChlorosLicenseError
chloros_sdk.ChlorosConnectionError
chloros_sdk.ChlorosProcessingError
chloros_sdk.ChlorosProjectFilesMissingError      # a ChlorosProcessingError
chloros_sdk.ChlorosAuthenticationError
chloros_sdk.ChlorosConfigurationError
chloros_sdk.ChlorosConnectError                   # raised by smart-connect surface
chloros_sdk.LatticeError, CameraNotFoundError, ...  # from lattice_sdk

# === Availability flags ===
chloros_sdk.CAMERA_AVAILABLE     # True iff lattice_sdk imported cleanly
chloros_sdk.PROJECT_AVAILABLE    # True when chloros_sdk.project imported; open_project()
                                 # still raises ImportError without the project loaders
```

---

## Xử lý hình ảnh — `ChlorosLocal`

Lớp đường ống chính. Khởi động backend khi được khởi tạo (nếu không có backend nào phản hồi và `auto_start_backend=True`), tạo / cấu hình các dự án, theo dõi tiến trình, trả về bản tóm tắt sau khi chạy.

### Hàm khởi tạo

```python
ChlorosLocal(
    api_url="http://127.0.0.1:5000",   # backend URL (also: backend_url=)
    auto_start_backend=True,            # spawn backend if not running
    backend_exe=None,                   # override backend binary path
    timeout=30,                         # request timeout seconds
    backend_startup_timeout=60,         # backend boot timeout
    processing_timeout=14400,           # hard cap on process() (4 h)
    processing_stuck_timeout=1800,      # no-progress threshold (30 min)
)
```

### Phương thức

| Phương thức | Mô tả |
| --- | --- |
| `create_project(project_name, camera=None)` | Tạo một dự án mới (tùy chọn với mẫu camera như `"Survey3N_RGN"`). |
| `open_project(project_path, relink=None)` | Mở một dự án đã lưu (thư mục chứa dự án hoặc tên dự án trong thư mục dự án) và thiết lập nó làm dự án đang mở. Các đường dẫn của dự án đã di chuyển sẽ được điều chỉnh lại trỏ đến thư mục hiện tại của nó; `relink` chỉ định tên thư mục chứa các tệp còn thiếu khác hiện tại. Trả về kết quả từ backend với `source_files`; gây ra lỗi `ChlorosProjectFilesMissingError` khi không tìm thấy tệp nào. Xem phần **Dự án đã di chuyển** bên dưới. |
| `import_images(folder_path, recursive=False)` | Nhập hình ảnh RAW/TIF/JPG/DNG **và `.daq` bản ghi từ cảm biến ánh sáng**. Trả về `count` (hình ảnh) và `scan_count` (bản ghi). Chỉ cảnh báo nếu thư mục không chứa cả hai. |
| `export_light_sensor(daq=True, csv=True)` | Ghi các tệp đã hiệu chuẩn `.daq` + `.csv` cho mỗi bản ghi cảm biến ánh sáng trong dự án, vào tệp `<project>/Light Sensor/`. Xem [Bản ghi cảm biến ánh sáng](#light-sensor-recordings--calibrated-daq--csv). |
| `configure(debayer=..., vignette_correction=..., reflectance_calibration=..., indices=[...], export_format=..., ppk=..., daq_log_path=..., input_level=..., radiometric_output=..., array_alignment=..., array_alignment_crop=..., array_alignment_interpolation=..., custom_settings=None)` | Cài đặt các thông số xử lý. `indices` là các tên cài đặt sẵn (`NDVI`, `GNDVI`, `OSAVI`, …), mỗi cái được thêm vào dự án tương ứng với các dải phổ của camera đó; một tên không phải là cài đặt sẵn, hoặc các dải phổ không thể tạo ra tên đó (`NDRE` cần dải RE và NIR, mà không có camera nào cung cấp đầy đủ), sẽ được đặt tên là `UserWarning` và không có hình ảnh chỉ mục. `daq_log_path` chỉ định một bản ghi từ cảm biến ánh sáng (một tệp `.daq` hoặc `.csv`, hoặc một thư mục chứa các tệp này) mà mỗi khung hình sẽ được so khớp với, cả LATTICE lẫn Survey3, thay vì các bản ghi được nhập cùng với hình ảnh; nếu đường dẫn bị thiếu hoặc không chứa bản ghi nào, các bản ghi đã nhập sẽ được sử dụng. |
| `process(mode="parallel", wait=True, progress_callback=None, poll_interval=2.0)` | Chạy quy trình xử lý. Trả về `{"status": "complete", "async": False}`, cùng với khóa `summary` khi hệ thống nền cung cấp — xem [Tóm tắt sau khi chạy & Gợi ý](#post-run-summary--hints) — `export_check_failure` khi không thể xác minh các tệp xuất của lần chạy, và `image_failure` khi mất một hình ảnh. `wait=False` trả về `{"status": "started", "async": True}` cùng lúc. |
| `get_config()` / `get_status()` / `status()` | Kiểm tra trạng thái hệ thống nền. |
| `logout()` | Đăng xuất: xóa thông tin đăng nhập được lưu trong bộ nhớ đệm và giải phóng khe thiết bị của máy này (`device_deregistration` trong kết quả). Quá trình xử lý đang diễn ra sẽ không bị dừng: nó sẽ hoàn tất ở chế độ mà nó đã bắt đầu (`run_in_progress: true` và `run_mode` trong kết quả), và lần chạy tiếp theo sẽ sử dụng quy trình xử lý tiêu chuẩn. |
| `shutdown_backend()` | Dừng backend nếu phiên bản này đã khởi động nó; backend đang chạy sẽ được để nguyên. Việc để lại khối `with` hoặc phiên bản đang bị thu gom rác sẽ không dừng nó: backend sẽ tiếp tục chạy cho kịch bản tiếp theo và tự dừng sau 30 phút nếu không có gì sử dụng nó. Backend được yêu cầu tắt máy, do đó nó sẽ giải phóng các camera và cảm biến ánh sáng của mình và hoàn tất bất kỳ quá trình ghi hình nào trước tiên; nó chỉ bị chấm dứt nếu vẫn đang chạy sau 10 giây. |
| `discover_cameras()` | Phát hiện camera LATTICE **thông qua backend của phiên bản này** (`/api/camera/discover`). Trả về một danh sách các từ điển (`serial`, `model`, `ip`, …) — có cấu trúc giống như những gì GUI/CLI hiển thị. Danh sách trống nếu không tìm thấy camera nào hoặc không thể kết nối với backend. |
| `camera_capture(output_dir, format="tiff", **settings)` | Ghi lại một khung hình duy nhất**thông qua backend**(được tự động khởi chạy bởi handle này) để nó được chuẩn bị giống như trên GUI/CLI (mặc định 12-bit, tái sử dụng pool, metadata hiệu chuẩn nhúng). Xác định mục tiêu bằng `serial=` hoặc `device_index=`; chuyển `exposure`/`gain`/`pixel_format`/`preset` thành `**settings`. Trả về từ điển siêu dữ liệu cũ (`filepath`, `width`, `height`, `pixel_format`, `exposure_time`, `gain`, `timestamp`). |
| `camera_stream(serial, *, fps=10.0, overlay=None, decode=True, connect_timeout=10.0, read_timeout=15.0)` | Trả về các khung hình xem trước được ghép chồng từ một camera được gộp — client MJPEG nhẹ thông qua đường dẫn `/api/camera/<serial>/stream-annotated` của backend (zebra / lưới / thập tự / biểu đồ histogram / peaking / điểm được vẽ phía máy chủ). `decode=True` trả về mảng BGR và yêu cầu OpenCV (`cv2`), mà SDK không cài đặt; `False` trả về các byte JPEG thô. Cũng có thể truy cập theo từng dự án dưới dạng `ChlorosProject.stream(overlays=True)`. |

**Một phiên xử lý đang chạy sẽ giữ nguyên dự án của nó.** Trong khi một phiên xử lý đang diễn ra — của tập lệnh này, của ứng dụng, của `chloros-cli process` hoặc của `chloros-mcp` — hệ thống nền sẽ từ chối tạo, mở, nhập vào hoặc thay đổi cài đặt xử lý của một dự án (Lỗi HTTP 409 `PROCESSING_IN_PROGRESS`): `create_project()` và `import_images()` gây ra lỗi `ChlorosProcessingError`, và `configure()` gây ra lỗi `ChlorosConfigurationError`, còn `process()` với chế độ khác với chế độ đang chạy sẽ gây ra lỗi `ChlorosProcessingError` (một lần chạy sẽ giữ nguyên chế độ mà nó đã bắt đầu), mỗi lần đều kèm theo thông báo từ backend. Một bản ghi được lưu vào dự án sau đó sẽ được ghép vào khi lần chạy kết thúc (thông báo trả lời của nó là `project_add` cho biết điều này). Hãy đợi cho đến khi quá trình chạy kết thúc (`get_status()`), sau đó tiếp tục.**Một dự án đã được di chuyển.** `project.json` của dự án đặt tên cho từng tệp nguồn (mỗi hình ảnh và bản ghi cảm biến ánh sáng) theo đường dẫn đầy đủ của nó, do đó việc di chuyển thư mục dự án — hoặc toàn bộ thư mục dự án sang một ổ đĩa khác — sẽ khiến chúng vẫn giữ tên vị trí cũ. Mỗi lần mở lại sẽ tự động khắc phục những gì có thể: tệp từng nằm trong thư mục dự án sẽ được tìm kiếm trong thư mục mà dự án hiện đang nằm, và mỗi đường dẫn được tìm thấy ở đó sẽ được điều chỉnh lại và lưu lại (`project.json` trước tiên được sao lưu thành `project.json.before-relink-<date>-<time>`). Đường dẫn chỉ được điều chỉnh lại cho những tệp thực sự tồn tại tại vị trí mới. Các tệp nằm bên ngoài thư mục dự án — chẳng hạn như phiên làm việc trên hub hoặc tệp nhập từ "Add Folder" — chỉ được tìm thấy trong `relink=<the folder they are in now>` (hoặc bất kỳ thư mục nào ở cấp trên của nó). `open_project()` trả về `source_files`: `total`, `found`, `missing`, `missing_roots` (các thư mục chứa các tệp bị thiếu), `missing_outside_project` và `relink` (thông tin về việc thay đổi đường dẫn, từ đâu đến đâu). Một dự án có một số tệp bị thiếu sẽ được mở cùng với các tệp còn lại; một dự án không tìm thấy tệp nào sẽ không được mở, và `open_project()` gây ra lỗi `ChlorosProjectFilesMissingError` (`.missing_roots`, `.audit`; HTTP 409 `PROJECT_SOURCES_MISSING`). Các mã lỗi `project open --relink` và `chloros-mcp` của CLIcủa giao diện dòng lệnh (CLI) như `open_project` (`relink_folder`) cũng thực hiện tương tự.

Sử dụng làm trình quản lý bối cảnh để đảm bảo dọn dẹp:

```python
with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("FieldA_2026-05-26", camera="Survey3N_RGN")
    cl.import_images("C:/DroneImages/Flight001")
    cl.configure(
        vignette_correction=True,
        reflectance_calibration=True,
        indices=["NDVI", "GNDVI", "OSAVI"],
        export_format="TIFF (16-bit)",
    )
    results = cl.process(mode="parallel", wait=True)
print(results["summary"])
```

### Ghi dữ liệu cảm biến ánh sáng — đã hiệu chuẩn `.daq` + `.csv`

Một thiết bị DAQ-U / DAQ-M / DAQ-E có thể được ghi lại **mà không cần** gói hiệu chuẩn của nó. Đó chính là
cách các bộ ghi công khai [`chloros_scripts`](https://github.com/mapircamera/chloros_scripts)
(`record_daq.py`) thực hiện theo mặc định: chúng ghi lại số đếm thô từ cảm biến và đóng dấu
tệp để Chloros lấy dữ liệu hiệu chuẩn nhà máy của cảm biến đó **theo số sê-ri** — trước tiên từ bộ nhớ đệm cục bộ
trước, sau đó là MAPIR Cloud — và áp dụng nó khi nhập dữ liệu.

Chloros ghi kết quả trở lại dưới dạng hai sản phẩm cho mỗi bản ghi, trong
`<project>/Light Sensor/`:

| Sản phẩm | Mô tả |
| --- | --- |
| `<name>_calibrated.daq` | Kho lưu trữ có thể xử lý lại — cùng cấu trúc với bản ghi trực tiếp, nay khai báo gói dữ liệu đã tạo ra nó. Việc nhập lại tệp này **sẽ không** hiệu chuẩn lại lần thứ hai. |
| `<name>_calibrated.csv` | Cường độ bức xạ quang phổ tính bằng W/m²/nm trên lưới bước sóng riêng của cảm biến, mỗi hàng tương ứng với một giá trị đo, kèm theo các cột dữ liệu quang học (công suất tổng, lux quang giác/tối giác, PPFD và phân tách theo màu xanh dương/xanh lục/đỏ, bước sóng đỉnh). |
| `<name>_raw.daq` / `<name>_raw.csv` | **Chỉ dành cho cảm biến không có gói dữ liệu (DAQ-A).** Số đếm phổ thô của cảm biến — *không* phải cường độ bức xạ. Xem bên dưới. |

`<name>` là tên tệp của bản ghi. Hai bản ghi trong các thư mục khác nhau có thể chia sẻ
một — giá trị đo cho mỗi lần chụp (`<capture>/daq/daq_data_<sensor id>_<ts>.daq`) được lưu trong
cùng một mili giây bởi hai camera — và mỗi bản ghi vẫn nhận được các sản phẩm riêng của mình: bản ghi đầu tiên giữ
`<name>`, bản ghi tiếp theo thêm các thư mục phân biệt nó **sau** tên
(`<name>__<capture>_daq_calibrated.daq`), do đó ngày và giờ của bản ghi vẫn được đặt ở đầu.

`process()` thực hiện việc xuất dữ liệu này như một trong các giai đoạn của nó. Nó **không** yêu cầu hình ảnh:
một cảm biến ánh sáng được triển khai độc lập là một quy trình làm việc hoàn chỉnh, và dự án như vậy không có
hình ảnh nào theo thiết kế.

**Các bản ghi DAQ-A được xuất dưới dạng số đếm thô.** Dòng sản phẩm DAQ-A ra đời trước hệ thống gói theo số sê-ri
và không có gói nào để lấy — thay vào đó, nó được hiệu chuẩn tại hiện trường so với một
mục tiêu phản xạ, đó là lý do tại sao nó không bao giờ cần đến gói. Các bản ghi này được xuất
dưới dạng `_raw` thay vì `_calibrated`: tên tệp khác thay vì một cờ
bên trong tệp, vì thông tin này phải được giữ nguyên khi được gửi qua email dưới dạng tên tệp thuần túy. Tiêu đề
Tiêu đề `.csv` ghi là `raw spectral sensor counts (NOT irradiance)` và cảnh báo rằng các
giá trị chỉ có thể so sánh được **trong** cùng một tệp — chính xác là mục đích mà hiệu chuẩn dựa trên mục tiêu sử dụng
chúng — chứ không phải giữa các cảm biến. Các cột quang đo phụ thuộc vào công suất (công suất tổng,
lux quang giác/tối giác, PPFD) trả về giá trị **NULL** thay vì được tích hợp từ số đếm.

Một thiết bị DAQ-U / DAQ-M / DAQ-E mà gói dữ liệu của nó không thể lấy được vẫn sẽ bị **bỏ qua**,
không được ghi dưới dạng thô: trong trường hợp đó, gói dữ liệu vẫn tồn tại và “kết nối lại và xử lý lại” là lời khuyên thực tế.

Các bản ghi cũ **v1.01 / v1.02** (thiết bị DAQ-A-SD ghi lại các bản ghi này) không chứa thời điểm cho từng lần đọc,
chỉ có thời gian ghi của tệp. Trình khớp hình ảnh↔ánh sáng chiếu xuống vẫn từ chối chúng — việc khớp
khung hình với thời gian ghi sẽ dẫn đến sai sót không thể nhận biết — nhưng trình xuất dữ liệu vẫn đọc được chúng, và tệp
.CSV sẽ in ra `clock=daq_created_on` để chỉ ra sản phẩm đang sử dụng đồng hồ nào.

```python
import chloros_sdk

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("DAQ-U_2026-08-26")
    cl.import_images("C:/Flights/raw_daq")     # .daq only — no camera involved
    result = cl.export_light_sensor()          # or just cl.process()

for rec in result["exported"]:
    print(rec["csv"])
for rec in result["skipped"]:
    print("skipped", rec["source"], "--", rec["reason"])
```

Một bản ghi mà gói hiệu chuẩn không thể được lấy (chế độ ngoại tuyến, hoặc cảm biến không có
hiệu chuẩn trong tệp) sẽ được báo cáo dưới dạng `skipped` **kèm theo lý do**. Nó sẽ không bao giờ
được ghi ra dưới dạng tệp “đã hiệu chuẩn” chứa số đếm thô — hãy kết nối với internet và
chạy lại, quá trình xuất dữ liệu sẽ hoàn tất.

### Các hàm gọi lại tiến trình

```python
def show_progress(percent, message):
    print(f"[{percent:3d}%] {message}")

with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("FieldA")
    cl.import_images("C:/DroneImages/Flight001")
    cl.configure(indices=["NDVI"])
    cl.process(progress_callback=show_progress, poll_interval=1.0)
```

### Tóm tắt sau khi chạy & Gợi ý

Khi hoàn tất, `process()` sẽ lấy `GET /api/processing-summary` và đính kèm nội dung dưới dạng `result["summary"]`. Việc lấy dữ liệu được thực hiện theo nguyên tắc "cố gắng hết sức" và không bao giờ chặn kết quả trả về thành công — nếu bản tóm tắt không khả dụng, `process()` sẽ chuyển sang định dạng `{"status": "complete", "async": False}` cơ bản. Mỗi mục trong `summary["hints"]` — các câu hoàn chỉnh kèm theo đề xuất khắc phục, ví dụ: lý do tại sao một lần chạy không tạo ra đầu ra nào — cũng được phát lại dưới dạng Python `UserWarning`, do đó các lần chạy không có đầu ra có khả năng tự chẩn đoán ngay cả khi bạn không bao giờ kiểm tra từ điển:

```python
result = cl.process()
for hint in result.get("summary", {}).get("hints", []):
    print("HINT:", hint)
# hints also arrive on the warnings channel:
#   python -W always::UserWarning your_script.py
```

`summary["totals"]` là phần có thể đọc được bởi máy:

| Khóa | Điều được đếm |
| --- | --- |
| `models` | Các nhóm camera trong lần chạy. |
| `images_in_groups` | Hình ảnh nguồn trong các nhóm đó. |
| `targets_found` | Các mục tiêu phản xạ được phát hiện. |
| `images_calibrated` | Các hình ảnh mà quá trình chạy đã tạo sản phẩm phản xạ. |
| `images_failed` | Các hình ảnh bị lỗi và không được xuất. |
| `exported_files` | **Các tệp sản phẩm hình ảnh mà quá trình chạy đã tạo**: mọi sản phẩm (debayered, preview, radiance, reflectance, index, render mục tiêu), ở mọi định dạng. |
| `reflectance_files` | Trong số các sản phẩm phản xạ đó (`Reflectance_Calibrated_Images`), dù được hiệu chuẩn bằng mục tiêu hay bằng độ chiếu xạ từ cảm biến ánh sáng (DAQ). Một lần chạy không có mục tiêu nhưng vẫn ghi lại phản xạ sẽ hiển thị thông báo này dưới dạng gợi ý; chỉ những lần chạy không ghi lại bất kỳ tệp nào mới được thông báo rằng các đầu ra phản xạ của chúng bị thiếu. |
| `clamp_masks_written` | Các [mặt nạ giới hạn tách kênh](#unmix-clamp-mask--which-pixels-a-band-was-clamped-on) được ghi bên cạnh chúng — là các thành phần đi kèm, không phải sản phẩm, do đó không nằm trong `exported_files` (CLI cũng đếm chúng riêng biệt theo cách tương tự). |
| `captures_left_out` | Các khung hình thô trong thư mục `captures` của dự án mà lần chạy này không thu thập. Một lần chạy sẽ chờ dữ liệu được ghi lại trước đó khi bắt đầu, do đó giá trị này là 0 trừ khi có dữ liệu được lưu trong quá trình chạy hoặc thời gian chờ hết hạn. `summary["captures_left_out_note"]` chỉ dẫn cách xử lý chúng. |
| `daq_recordings_exported` / `daq_recordings_skipped` | Các bản ghi từ cảm biến ánh sáng, được tính riêng một cách có chủ đích — chúng đến từ một giai đoạn khác và tồn tại trong các lần chạy hoàn toàn không có hình ảnh, do đó việc gộp chúng vào sẽ khiến một lần chạy chỉ thu thập dữ liệu (DAQ) trông như thể đã xuất hình ảnh. |

Bên cạnh đó: `summary["output_dirs"]` (mọi thư mục đã được ghi vào),
`summary["light_sensor_export"]`, `summary["stopped"]` (có giá trị true khi người dùng ngắt
quá trình chạy, do đó số đếm một phần sẽ không được coi là một lần chạy hoàn thành nhưng sản lượng thấp), và
`summary["groups"]` (phân tích theo từng nhóm).

`exported_files` được hệ thống ghi lại **trong quá trình ghi**, không phải quét từ các
đối tượng hình ảnh của dự án sau đó. Các chiến lược song song và GPU tự tạo ra các đối tượng hình ảnh
của riêng mình (trong các tiến trình con của worker cho các đường dẫn GPU), do đó quá trình quét cũ đã báo cáo
`0 file(s) written` cho mỗi lần chạy như vậy và sau đó phát ra gợi ý “zero-exports” — ngay cả trong các lần chạy
mà mọi thứ đều hoạt động bình thường. Nếu bạn viết kịch bản dựa trên con số này, một lần chạy song song bình thường hiện nay
sẽ báo cáo số lượng không bằng không.

Các báo cáo bỏ qua cảm biến ánh sáng sẽ hiển thị lý do thực tế mà trình đọc xác định cho từng tệp — một
schema không thể đọc được, một gói thiếu, một lỗi ghi — **được loại trừ trùng lặp**, do đó hai mươi tệp
bị bỏ qua vì một nguyên nhân sẽ được hiển thị là một nguyên nhân duy nhất thay vì hai mươi lần lặp lại của nó.

> **`process()` không được kích hoạt khi một lần chạy không tạo ra hình ảnh nào.** Đây là một trong hai điểm mà SDK và
> CLI có sự khác biệt có chủ đích: `chloros-cli process` coi "đã yêu cầu sản phẩm, nhưng không có sản phẩm nào
> được ghi" là lỗi và thoát với giá trị không bằng 0, trong khi SDK trả về kết quả bình thường và báo cáo
> tình trạng thông qua `summary` / hints. Nếu quy trình xử lý của bạn cần dừng lại khi có lần chạy trống, hãy tự kiểm tra
> — kiểm tra `summary` (hoặc đếm số tệp trong thư mục dự án) thay vì dựa vào
> việc không có ngoại lệ. Các nguyên nhân thường gặp là thư mục đầu vào không được nhận diện là
> vùng ghi hình và các sản phẩm bị bỏ qua vì không áp dụng được cho các camera hiện có (ví dụ: độ sáng từ camera chỉ hỗ trợ RGB
> ).
>
> `process()` cũng không được kích hoạt khi không thể xác minh các tệp xuất của một lần chạy: nó sẽ hiển thị cảnh báo và đặt
> `result["export_check_failure"]`. Các sản phẩm vẫn được ghi ra. Để dừng tại đây, hãy kiểm tra khóa đó. Một lần chạy
> bị mất hình ảnh cũng tương tự: nó sẽ hiển thị cảnh báo và đặt `result["image_failure"]`, mà `chloros-cli process`
> sẽ thoát với giá trị không bằng 0 khi giá trị này được đặt.

### Các hàm tiện ích

```python
# One-call process: project + import + configure + process
results = chloros_sdk.process_folder(
    folder_path="C:/DroneImages/Flight001",
    project_name="FieldA_2026-05-26",
    camera="Survey3N_RGN",
    indices=["NDVI", "GNDVI", "OSAVI"],
    vignette_correction=True,
    reflectance_calibration=True,
    export_format="TIFF (16-bit)",
    mode="parallel",
    debayer="High Quality (Faster)",      # or "Texture Aware (Slow, Highest Quality)"
    ppk=False,
    recursive=False,
    processing_timeout=14400,
)

# LATTICE-friendly defaults (no panel-target detection, standard debayer)
results = chloros_sdk.process_lattice_capture(
    folder_path="C:/Captures/2026-05-13_Field",
    indices=["NDVI"],
)

# process_folder() / process_lattice_capture() also take input_level=,
# array_alignment=, array_alignment_crop=, array_alignment_interpolation=,
# progress_callback=, poll_interval=, timeout=, processing_stuck_timeout=,
# backend_exe= and api_url= (NOT backend_url=). indices defaults to ["NDVI"].

# Audit which calibration sources were applied to a processed image
tags = chloros_sdk.read_image_audit_tags("output/Reflectance_Calibrated/x.tif")
print(tags["CalibrationSource"])   # 'per_serial' / 'legacy_lookup' / 'none'
print(tags["VignetteSource"])      # 'per_serial' / 'legacy_polynomial' / 'none'
```

Mỗi lần chạy đều nằm trong `with ChlorosLocal(...)` riêng của nó; một backend mà nó phải khởi động sẽ tiếp tục chạy khi nó trả về kết quả, cho lần gọi tiếp theo, và tự dừng sau 30 phút nếu không có gì sử dụng nó. Cả hai đều không nhận các tham số `auto_start_backend`, `radiometric_output` hoặc `daq_log_path`; hãy sử dụng `ChlorosLocal` và `configure()` cho các trường hợp đó. `process_lattice_capture()` không chấp nhận các giá trị `camera`, `debayer` hoặc `ppk`: nó bỏ qua quá trình phát hiện bảng và sử dụng thuật toán debayer tiêu chuẩn.

### Các giá trị được hỗ trợ

```python
# export_format
"TIFF (16-bit)"           # default, recommended
"TIFF (32-bit, Percent)"  # reflectance percentage as float32
"TIFF (8-bit)"            # all-8-bit images are written as this; never raised
"PNG (8-bit)"
"JPG (8-bit)"

# debayer
"High Quality (Faster)"               # standard, default
"Texture Aware (Slow, Highest Quality)"  # neural debayer for Survey3 imagery, Chloros+ only.
                                         # Survey3 only: on a project holding LATTICE captures
                                         # (which always use the standard demosaic) configure()
                                         # or process() raises ChlorosConfigurationError; a
                                         # Texture Aware value the project saved earlier is set
                                         # to Standard with a UserWarning, as the app does. Needs a
                                         # CUDA GPU the backend's PyTorch can use (an NVIDIA
                                         # GPU, or a Jetson Orin's). Without one, process()
                                         # uses the standard debayer, and the backend prints
                                         # one "[DEBAYER] ... needs a CUDA GPU" line saying why.
"Standard (Fast, Medium Quality)"      # alias used internally for LATTICE

# input_level (LATTICE only; Survey3 .raw ignores)
"auto"        # default — infers from each file's XMP ProcessingLevel tag
"raw"         # force-treat as raw Bayer
"debayered"   # force-treat as Chloros's saved debayered (3-channel) product
"processed"   # force-treat as already-calibrated radiance

# array_alignment / array_alignment_crop (LATTICE arrays; None = keep saved setting)
True          # backend default — apply the module-to-module transform stamped
              # in each capture's Chloros:Alignment* XMP to every product
False         # export in native sensor geometry / skip the common-overlap crop

# array_alignment_interpolation (alignment warp resampling)
"bilinear"    # backend default
"nearest"     # preserves exact source DNs (no inter-pixel value mixing)
"cubic"
```

#### Đầu ra phóng xạ (dòng xử lý đa phổ LATTICE)

Cấp độ xuất đa phổ LATTICE (M3C/M3M) của quy trình xử lý `process` — `reflectance` (mặc định), `radiance`, `sensor-response` hoặc `all` (mỗi chế độ áp dụng cho từng hình ảnh) — xác định các sản phẩm **"Xuất radiance"** và **"Xuất phản xạ"**: `radiance` chỉ xuất radiance, `reflectance` chỉ xuất reflectance, `sensor-response` không xuất cả hai (chỉ các sản phẩm đã loại bỏ hiệu ứng Bayer và bản xem trước), `all` xuất cả hai. Các sản phẩm đã loại bỏ hiệu ứng Bayer và bản xem trước không thuộc phạm vi điều chỉnh của cấp độ này. `configure()` có từ khóa chuyên dụng cho mục đích này:

```python
with chloros_sdk.ChlorosLocal() as cl:
    cl.create_project("Field_A")
    cl.import_images("C:/Captures/lattice_flight")
    cl.configure(
        radiometric_output="radiance",   # reflectance (default) / radiance / sensor-response / all
        export_format="TIFF (32-bit, Percent)",
    )
    cl.process()
```

Cách thoát khẩn cấp nâng cao — ghi các sản phẩm thông qua `custom_settings` — vẫn hoạt động, nhưng hãy nhớ rằng nó sẽ thay thế toàn bộ khối cài đặt (xem cảnh báo bên dưới). Ghi các khóa `"Export <product>"`: các thiết lập riêng của dự án `"Export radiance"` / `"Export reflectance"` sẽ được ưu tiên hơn khóa `"Radiometric output"` cũ, do đó khóa đó khi đứng riêng lẻ sẽ không thay đổi gì cả.

```python
cl.configure(custom_settings={
    "Project Settings": {
        "Processing": {"Export radiance": True, "Export reflectance": False},
        "Export": {"Calibrated image format": "TIFF (32-bit, Percent)"},
    }
})
```

`reflectance` (mặc định) chia độ rạng rỡ của camera cho **dữ liệu DAQ hướng xuống khớp với dấu thời gian**, được xác định tự động từ một khóa `.daq` (DAQ-U/M/E)**hoặc một tệp `.csv` gốc của DAQ-M**được tìm thấy cùng với hình ảnh; bất kỳ gói hiệu chuẩn nào cho từng camera hoặc DAQ bị thiếu tại địa phương sẽ được**tự động tải về từ AWS** khi sử dụng lần đầu. Giao diện dòng lệnh (CLI) hiển thị tính năng này theocác tùy chọn bật/tắt sản phẩm trên `chloros-cli process`: `--radiance`/`--no-radiance`, `--reflectance`/`--no-reflectance`, `--debayered`, `--preview`.**Đây là dữ liệu đo của ai.**Dữ liệu được lưu trong một bản ghi `.daq` là dữ liệu đo của (các) máy ảnh mà nó mô tả — được nêu tên trong tệp sidecar `<file>.daq.cameras.json` đi kèm (bản ghi đơn lẻ, đồng bộ và dạng mảng; một loạt ảnh thô sẽ liệt kê dữ liệu đo của từng thành viên trong tệp manifest của nó) — và chỉ khớp**với hình ảnh từ những camera đó**. Do đó, một camera độc lập không có dữ liệu đo riêng sẽ không còn mượn DAQ của camera khác; hãy gán một DAQ cho nó hoặc ghi lại một bản ghi. Bản ghi riêng của DAQ (`chloros-cli daq pool-record`, hoặc bản ghi được khởi tạo trong GUI), hoặc bất kỳ tệp `.daq` nào không có tệp sidecar đó (bao gồm cả các bản ghi cũ), vẫn phục vụ cho mọi camera.**Camera OCN dưới ánh nắng mặt trời.** Mỗi gói hiệu chuẩn OCN đều được xây dựng dưới ánh sáng dải hẹp, mỗi dải sử dụng một đèn LED, trong khi ánh sáng mặt trời trộn lẫn các dải. Phiên bản Chloros 1.3.0 áp dụng hiệu chỉnh dựa trên ánh sáng mặt trời cho toàn bộ hệ thống sau khi tách dải, giúp cải thiện các sản phẩm độ sáng, độ phản xạ và chỉ số của OCN dưới ánh sáng mặt trời. Chức năng này hoạt động ở bất kỳ đâu mà quá trình tách dải được thực hiện — `process()`, các bản chụp, `grab()` / `frame_stream()` và chế độ xem trực tiếp — đối với các gói OCN hỗ trợ tính năng này; các camera RGN không có thay đổi. Các hình ảnh OCN được chụp bằng phiên bản trước đó cũng sẽ được áp dụng tính năng này khi được xử lý trên máy tính lưu trữ thông số hiệu chuẩn đã sử dụng khi chụp (Chloros lưu giữ thông số hiệu chuẩn của máy ảnh sau khi máy ảnh được kết nối); nếu không, chúng sẽ được xử lý như trước đây. Do đó, các giá trị độ sáng, độ phản xạ và chỉ số OCN sẽ khác so với phiên bản 1.2.0.

> `custom_settings` **thay thế** toàn bộ khối cài đặt tính toán (theo thiết kế, nó bỏ qua các từ khóa khác và quá trình xác thực của `configure()`). Khi sử dụng, hãy bao gồm mọi khóa `Project Settings` mà bạn quan tâm, như trong ví dụ trên.

#### Đọc các pixel chỉ số (chỉ số NDVI và các chỉ số thực vật khác)

**Một raster chỉ số nguyên là quá trình điều chỉnh độ tương phản cho từng hình ảnh, không phải là bản đồ trong khoảng −1…1.** `TIFF (16-bit)`, `TIFF (8-bit)`, `PNG (8-bit)` và `JPG (8-bit)` — bao gồm cả tệp TIFF 8-bit được tạo ra từ dữ liệu chụp 8-bit — lưu trữ

```

DN = (value − IndexMin) / (IndexMax − IndexMin) × full_scale
```

trong đó `IndexMin` / `IndexMax` là **các giá trị chỉ mục hữu hạn thấp nhất và cao nhất của chính khung hình đó**. DN 0 không phải là −1, và mức đầy đủ không phải là +1.

Hệ thống Chloros đánh dấu cả hai điểm cuối để phép ánh xạ vẫn có thể đảo ngược:

| Thẻ | Ý nghĩa |
| --- | --- |
| `Chloros:IndexScaling` | `linear_minmax` — quy ước dưới đây |
| `Chloros:IndexMin` | giá trị chỉ số được lưu trữ dưới dạng DN 0 |
| `Chloros:IndexMax` | giá trị chỉ mục được lưu trữ dưới dạng quy mô đầy đủ |

Đọc các giá trị này và đảo ngược, đồng thời chuẩn hóa giá trị DN đã lưu thành khoảng 0…1 dựa trên giá trị tối đa của kiểu dữ liệu (dtype) đã lưu trước đó, để một biểu thức duy nhất có thể áp dụng cho mọi định dạng số nguyên:

```python
import json, subprocess
import numpy as np, tifffile

tags = json.loads(subprocess.run(
    ["exiftool", "-j", "-XMP:all", path],
    capture_output=True, text=True).stdout)[0]

# float(): exiftool hands back a negative real as a quoted string, so
# IndexMin arrives as str on most real NDVI data while IndexMax is a number.
lo, hi = float(tags["IndexMin"]), float(tags["IndexMax"])

raw = tifffile.imread(path)                    # uint16 for TIFF (16-bit)
ndvi = lo + raw.astype(np.float64) / np.iinfo(raw.dtype).max * (hi - lo)
```

Việc đọc không cần `-config pix4d.config` — exiftool tự động đọc các thuộc tính XMP này. Cấu hình này chỉ cần thiết để *ghi* chúng.

> **`IndexMin == IndexMax`** có nghĩa là mọi pixel trong khung hình đó đều có cùng một giá trị chỉ số. Hình raster được lấp đầy ở mức trung bình và không có gì để chia — hãy lấy điểm cuối.

> **Không có thẻ `Chloros:Index*` có nghĩa là không có ánh xạ được chỉ định.** Xuất LUT là một bản hiển thị đã được tô màu, trong đó các pixel là các màu chuyển sắc được liên kết với cửa sổ ngưỡng LUT của bạn, và một khung hình không có pixel chỉ số hữu hạn sẽ không có phạm vi để xác định. Đừng giả định có bản đồ ánh xạ trong cả hai trường hợp — quy tắc này tương tự như trường hợp thiếu thẻ `PixelScale`.**Vì độ giãn được áp dụng cho từng hình ảnh, hai khung hình trong cùng một chuyến bay không thể so sánh với nhau dưới dạng DN**— mỗi khung hình có các điểm cuối riêng, do đó cùng một giá trị NDVI sẽ tương ứng với các giá trị DN khác nhau trong các hình ảnh khác nhau. Đối với công việc định lượng trên toàn chuyến bay (thống kê theo vùng, ngưỡng cố định, chuỗi thời gian), hãy xuất**`TIFF (32-bit, Percent)`**: các raster chỉ số được ghi ở đó dưới dạng float32**chính các giá trị chỉ số**, không cần điều chỉnh tỷ lệ và không cần thẻ. Mặc dù tên định dạng là vậy, các sản phẩm chỉ số không phải là ×100 — tỷ lệ đó chỉ áp dụng cho phản xạ có kiểu dữ liệu số nguyên.

#### Mặt nạ giới hạn tách phổ — các pixel mà dải phổ bị giới hạn

Quá trình tách phổ 3×3 có thể khiến một dải phổ trở thành âm — khi ánh sáng hầu như không chứa dải phổ đó, hoặc khi cảnh nằm ngoài phạm vi mà ma trận tách phổ của máy ảnh mô tả. Đó không phải là giá trị thực, do đó quá trình xuất sẽ cố định nó thành 0 — và chế độ xem trực tiếp cũng hiển thị giá trị 0 tương tự. Một dải phổ có thể bị giới hạn trên một vài pixel hoặc gần như toàn bộ các pixel; mặt nạ ghi lại cả hai trường hợp. `Chloros:Band{R,G,B}UnmixClampFraction` cho biết phần trăm khung hình bị ảnh hưởng; mặt nạ giới hạn chỉ ra **các pixel cụ thể nào**:

- **`clamp_masks/<product folder>/<product name>_clampmask.tif`**— trong thư mục `clamp_masks/`**bên cạnh** thư mục của sản phẩm, không bao giờ nằm bên trong thư mục đó (`radiance/x_radiance.tif` → `clamp_masks/radiance/x_radiance_clampmask.tif`) — dành cho mọi sản phẩm về độ sáng, độ phản xạ và chỉ số trong đó có ít nhất một pixel bị giới hạn. `uint8` dải đơn, trên lưới pixel riêng của sản phẩm (bao gồm hướng, ROI, binning và biến dạng căn chỉnh mảng được bao gồm). Mỗi mặt nạ ghi tên camera của sản phẩm đó (`Make` / `Model` / số sê-ri), thời gian chụp và thẻ ống kính, đồng thời liệt kê các dải tần mà các bit của nó đại diện (`XMP-Camera:BandName`, bit 0 đầu tiên); các bit `ImageDescription` và `Software` đánh dấu nó là mặt nạ giới hạn, và nó không chứa thông tin hiệu chuẩn.
- **Bit**: bit 0 = dải tần trong kênh R của sản phẩm (`Chloros:BandRName`), bit 1 = G (`BandGName`), bit 2 = B (`BandBName`). Một bit được đặt có nghĩa là giá trị của pixel trong dải đó đã bị giới hạn xuống 0 — hãy coi nó, cũng như bất kỳ chỉ số nào được xây dựng từ nó, là có độ tin cậy thấp.
- **`Chloros:UnmixClampMask`** được đóng dấu trên mọi sản phẩm đã chạy quá trình tách kênh: đường dẫn của mặt nạ so với thư mục của chính sản phẩm (`../clamp_masks/<product folder>/<product name>_clampmask.tif`, luôn được phân tách bằng `/`- được phân tách — ghép nó với thư mục của hình ảnh để mở mặt nạ), hoặc `none` khi không có pixel nào bị giới hạn. Không có thẻ nào có nghĩa là sản phẩm chưa bao giờ chạy quá trình tách màu (máy ảnh RGB hoặc đơn sắc, mức độ thô/đã loại bỏ hiệu ứng debayering).
- Kết quả chụp sẽ chứa thông tin này dưới dạng **`clamp_mask`** của khung hình (đường dẫn đến mặt nạ, hoặc `None` khi không có pixel nào bị giới hạn) — trong kết quả `CameraSession.capture()`, các khung hình `ArraySession.capture()`'các khung hình `CaptureResult` và các phản hồi ghi hình qua HTTP. Bản dựng ngoại tuyến liệt kê các mặt nạ của nó dưới dạng `clamp_masks`.
- Ngăn xếp đa dải tần của chỉ mục trực tiếp (`/api/camera/cli-array-index` với `save_multiband`) nhận được một mặt nạ cho mỗi camera nguồn, trong thư mục đầu ra riêng của `clamp_masks/` — xem tài liệu tham khảo CLI'`lattice index`. Video không có mặt nạ: kết quả của bản dựng ngoại tuyến thay vào đó báo cáo tỷ lệ kẹp của từng khung hình.

Vì không có thư mục sản phẩm nào chứa mặt nạ, ở bất kỳ cấp độ nào, nên thư mục sản phẩm có thể được chuyển trực tiếp cho phần mềm quang trắc học — bao gồm cả các thư mục con — và phần mềm này chỉ nhận hình ảnh. Hãy chuyển thư mục sản phẩm cho phần mềm, chứ không phải thư mục ở trên nó. Tính năng nhập liệu riêng của Chloros sẽ bỏ qua các mặt nạ.

```python
import os, numpy as np, tifffile

mask = tifffile.imread(
    "clamp_masks/radiance/capture_<ts>_SN<serial>-<filter>_radiance_clampmask.tif")
# or, from the image: os.path.join(os.path.dirname(image_path), tag), with
# tag = the image's Chloros:UnmixClampMask
r_clamped = (mask & 1) != 0      # the band Chloros:BandRName names
g_clamped = (mask & 2) != 0
b_clamped = (mask & 4) != 0
```

---

## Smart-Connect cho máy ảnh LATTICE

Các phiên làm việc nền liên tục cho phần cứng trực tiếp. Sử dụng cùng các điểm cuối (endpoints) mà giao diện người dùng (GUI) sử dụng, do đó hành vi hoạt động giống hệt nhau trên SDK, CLI và GUI.

### Máy ảnh đơn — `CameraSession`

```python
import chloros_sdk

# Open by serial; reuses existing pool entry if one exists
with chloros_sdk.connect_camera("213800234") as cam:
    # cam is a CameraSession; supports context manager + manual disconnect
    cam.set_settings(
        exposure_time=10000,    # microseconds
        gain=0.0,               # dB
        pixel_format="BayerRG12",
        target_brightness=80,
        ae_damping=8.0,
    )
    cam.capture("output/", ext=".tiff")
```

Các thông số phơi sáng, độ khuếch đại và mục tiêu được thiết lập tại đây sẽ được giữ nguyên: tính năng điều khiển phơi sáng tự động của Chloros (bao gồm cả phơi sáng được đo bằng DAQ) sẽ không can thiệp vào chúng. `cam.set_settings(exposure_auto="Continuous")` (hoặc `gain_auto=`) trả lại thông số phơi sáng (độ khuếch đại), và `cam.set_settings(target_brightness_auto=True)` trả lại mục tiêu cho chế độ mục tiêu tự động củChloros.

#### `connect_camera()` Chữ ký

```python
connect_camera(
    serial,
    *,
    preset=None,                       # "default" | "high_quality" | "high_speed" | "triggered"
    settings=None,                     # dict overlaid on the preset
    backend_url="http://127.0.0.1:5000",  # deliberately not 'localhost' (::1-first on Windows ≈ 2 s/request)
    timeout=60.0,
    auto_start_backend=True,           # spawn a local backend if none is running
) -> CameraSession
```

- **Nó trả về khi luồng dữ liệu đã bắt đầu.** Phần backend chờ đợi, trong một khoảng thời gian giới hạn (khoảng 15 giây để luồng bắt đầu, sau đó 10 giây cho bộ đệm hoàn chỉnh đầu tiên — không phải cho khung hình xem trước đầu tiên, vốn xuất hiện khoảng 1,5 giây sau đó), vì vậy một `status()` xuất hiện ngay sau khi nó báo `streaming`. Phần `connect_stream` của phiên là khối `stream` trong phản hồi: `state` `streaming`, `no_frames` (luồng dữ liệu đã bắt đầu nhưng không truyền tải được gì — khối `note` cho biết điều này và, đối với camera sử dụng gói tin jumbo, cần kết nối lại với kích thước gói tin 1500 byte), `starting` hoặc `start_failed`, với `waited_s`. `None` dành cho camera đã được kết nối. Một bản xem trước (`get_camera_preview`, `/api/camera/jpeg/<serial>`) được yêu cầu ngay sau khi chờ, trong khoảng thời gian giới hạn (4 giây), để nhận khung hình xem trước đầu tiên của camera thay vì trả lời "Chưa có khung hình nào"; một camera mà luồng dữ liệu chưa truyền tải gì (`no_frames`, hoặc đã được kích hoạt bằng tín hiệu kích hoạt phần cứng) vẫn trả lời ngay lập tức.
- **Kích thước gói tin do bạn quyết định.** `settings={"gev_packet_size": 1500, "gev_packet_size_auto": False}` mở camera với gói tin 1500 byte, không qua kiểm tra (`chloros-cli lattice cam-connect --packet-size standard` gửi tương tự).
- **Mỗi lần chỉ kết nối một camera để duy trì nhóm camera**: quá trình mở kết nối, kiểm tra kích thước gói tin, tải xuống dữ liệu hiệu chuẩn lần đầu (khoảng 70 giây đối với camera mới trên máy tính này), và bắt đầu luồng video. Một yêu cầu cần sử dụng camera trong thời gian đó (`read_nodes`, `set_settings`, ghi hình, kết nối lần thứ hai) sẽ chờ tối đa 5 giây, sau đó báo lỗi với mã trạng thái HTTP 503 (`CAMERA_POOL_BUSY`), kèm theo tên camera đang kết nối (`ChlorosConnectError`); hãy thử lại sau một lúc. Các mục trong danh sách không bao giờ phải chờ: `list_cameras()` (`GET /api/camera/pool`), `list_arrays()` và `CameraSession.status()` trả lời ngay lập tức. Một camera đang kết nối được liệt kê dưới mục trả lời của nhóm với tên `connecting` (`serial`, `phase` — `opening the camera`, `rebalancing the pool bandwidth`, `loading its calibration`, `starting its stream`, `waiting for its stream` — và `elapsed_s`), và giá trị `status()` của nó cho biết giá trị `connecting: True` cùng với giá trị `phase`; các camera còn lại được đọc trực tiếp khi bộ nhớ đệm trống, nếu không thì lấy giá trị từ lần đọc gần nhất (`live: False`).

#### Các phương thức `CameraSession`

| Phương thức | Mô tả |
| --- | --- |
| `status(timeout=15.0)` | Trạng thái trực tiếp của camera, như được `chloros-cli lattice status` đọc: `streaming`, `stream_state` (`starting`, `no_frames`, `streaming`, `start_failed` hoặc `stopped`), `device` (kiểu máy, số sê-ri, phiên bản phần mềm, nhiệt độ, `is_mono`) và các cài đặt hiện tại của nó. |
| `read_nodes(names, enum_names=(), timeout=30.0)` | Đọc các nút GenICam; trả về `{nodes, errors, enums, device, was_in_pool, in_array_id}`, cùng với `frame_values` và `notes` trong khi chế độ phơi sáng tự động đang chạy (xem bên dưới). |
| `set_settings(**kwargs)` | Ghi các nút theo tên thân thiện (`exposure_time`, `gain`, `pixel_format`, `width`, `height`, `target_brightness`, `target_brightness_auto`, `ae_damping`, `ae_upper_limit`, `trigger_mode`, `trigger_source`, …). Một `exposure_time` (`gain`) là chế độ phơi sáng thủ công (độ lợi): được ghi mà không có `exposure_auto` (`gain_auto`), chế độ tự động phải được tắt trước, để giá trị này không bị từ chối bởi chế độ tự động đang hoạt động. Kết quả là `applied` lưu lại giá trị mà máy ảnh đã ghi nhận; một giá trị ghi bị máy ảnh từ chối được lưu trong `errors` và không được lưu giữ. Lỗi này chỉ ra phạm vi mà máy ảnh hiện chấp nhận (giá trị tối đa `ExposureTime` tuân theo tốc độ khung hình của máy ảnh), và chế độ tự động đã bị tắt chỉ dành cho giá trị bị từ chối sẽ được khôi phục; `restored` liệt kê nó (ví dụ: `{"ExposureAuto": "Continuous"}`). |
| `capture(output_dir="output", ext=".tiff", jpeg_quality=95, processing=None, levels=None, force_daq=None, daq=None, daq_tolerance_ms=None, settings=None, timeout=None)` | Ghi lại **một** khung hình. Trả về một `CaptureResult`: danh sách các từ điển siêu dữ liệu khung hình (mỗi cấp độ lưu trữ một từ điển), bao gồm `.skipped`, `.failed`, `.daq_files` và `.daq_matches`. (Chức năng chụp liên tiếp/nhiều khung hình đã bị loại bỏ — hãy gọi `capture()` trong vòng lặp nếu bạn cần một chuỗi hình ảnh.) |
| `capture_repeated(output_dir="output", *, count=None, duration_s=None, interval_s=0.0, on_capture=None, **capture_kwargs)` | Chụp liên tiếp (`interval_s=0`) hoặc chụp theo khoảng thời gian từ camera này, đối với các lần chụp `count` và/hoặc `duration_s` (ít nhất một lần để kết thúc), tương tự như cách `ArraySession.capture_repeated()` thực hiện đối với một mảng. Trả về danh sách các `CaptureResult`; quá trình dừng lại sau khi có lần chụp báo ổ đĩa đầy. `on_capture(i, result)` chạy sau mỗi lần chụp; các từ khóa khác được chuyển đến `capture()`. |
| `disconnect(keep_settings=False, clear_settings=False)` | Giải phóng khỏi nhóm, camera được khôi phục về trạng thái ban đầu khi kết nối (cấu hình kích hoạt, PTP, cài đặt); `keep_settings=True` lưu cài đặt của nó vào bộ nhớ riêng thay vào đó, `clear_settings=True` đặt lại bộ nhớ đó (xem [Bộ nhớ camera](#camera-memory--keep_settings--clear_settings)); trả về `camera_memory` khi được yêu cầu thực hiện một trong hai thao tác trên. Một camera là thành viên của mảng sẽ giải phóng toàn bộ mảng. Không thực hiện thao tác nào nếu chúng ta đã kết nối với một phiên đã mở sẵn. Trả về khi backend xác nhận việc giải phóng, hoặc khi không còn gì để giải phóng (camera đã không còn tồn tại, hoặc không có backend nào đang chạy); việc giải phóng hết thời gian chờ hoặc bị backend từ chối sẽ gây ra lỗi `ChlorosConnectError`, vì camera có thể vẫn đang bị giữ và lần kết nối tiếp theo sẽ xung đột với nó. Việc thoát khỏi khối `with` không bao giờ gây ra lỗi. |

Thuộc tính: `serial`, `device` (bản ghi phát hiện camera) và `backend_url`.

Kiểm soát xuất khẩu `capture()` (cùng mô hình với mảng + GUI):

- `processing` / `levels` — `processing="all"` lưu mọi loại xuất dữ liệu áp dụng; `levels=["raw","radiance"]` chỉ lưu những loại đó (ghi đè lên `processing`). Bỏ qua cả hai để sử dụng giá trị mặc định của hệ thống nền.
- `daq` — dữ liệu DAQ được sử dụng để ghi lại quá trình thu thập sẽ được lưu dưới dạng tệp phụ `.daq` theo mặc định, bao gồm cả bản ghi thô, do đó khung hình có thể được xử lý lại thành dữ liệu phản xạ/chỉ số sau này. `daq=False` sẽ bỏ qua nó khi không có mức nào sử dụng nó. `force_daq=True` là cách viết cũ của giá trị mặc định. Không có DAQ, không có `.daq`.
- `daq_tolerance_ms` — khoảng cách tối đa từ thời điểm phơi sáng mà giá trị đo vẫn nằm trong giới hạn cho phép (mặc định là 500). Chỉ kết quả đánh giá là thay đổi.

#### `read_nodes()` trong chế độ phơi sáng tự động — `frame_values` và `notes`

Trong khi `ExposureAuto` (`GainAuto`) đang chạy, nút `ExposureTime` (`Gain`) sẽ đọc lại **giá trị cuối cùng được ghi**, chứ không phải giá trị hiện tại mà máy ảnh đang thực hiện. `nodes` vẫn giữ nguyên giá trị của nút đó. Khi bạn yêu cầu `ExposureTime` hoặc `Gain` trên một camera đã kết nối mà chế độ tự động không phải là `Off`, phản hồi sẽ bổ sung:

- `frame_values` — giá trị mà khung hình mới nhất của camera mang theo, dưới tên nút (`ExposureTime` tính bằng µs, `Gain` tính bằng dB);
- `notes` — mỗi nút như vậy sẽ có một dòng chứa cả hai giá trị.

```python
r = cam.read_nodes(["ExposureTime", "Gain"])
print(r["nodes"]["ExposureTime"])                  # last value written, e.g. 2000.448
print(r.get("frame_values", {}).get("ExposureTime"))  # what the newest frame used, e.g. 19600.0
for name, note in r.get("notes", {}).items():
    print(note)
# ExposureAuto is Continuous: ExposureTime holds the last value written (2000.448);
# the camera's newest frame was at 19600 us.
```

Cả hai khóa đều vắng mặt khi chế độ tự động là `Off` (lúc này nút chính là giá trị thực của camera) và đối với máy ảnh không được kết nối với hệ thống hậu kỳ. Đối với mảng, `ArraySession.status()["member_ae"]` cung cấp giá trị khung hình của từng thành viên cùng một lúc.

#### Giới hạn phơi sáng được đo bằng DAQ (HTTP)

Khi kết nối với cảm biến ánh sáng DAQ, Chloros sẽ giới hạn phơi sáng × độ khuếch đại của camera dựa trên ánh sáng ban ngày được đo để đảm bảo bảng tham chiếu màu trắng không bị cắt sáng. Giới hạn này chỉ áp dụng khi mục tiêu hiệu chuẩn nằm trong tầm nhìn của camera đó (xuất hiện trong khung hình của nó trong vòng 10 giây gần nhất); khi không có mục tiêu trong tầm nhìn, chế độ phơi sáng tự động thông thường của camera, kèm theo tính năng bảo vệ vùng sáng riêng, sẽ được kích hoạt thay thế, để phần còn lại của cảnh không bị làm tối hơn mức cần thiết. Không có lớp bọc SDK; hãy đọc hoặc chuyển đổi cài đặt này cho từng camera bằng các mã `GET` / `POST /api/camera/<serial>/daq-exposure-cap` (nội dung POST `{"enabled": false}` sẽ tắt tính năng này cho camera đó). Phản hồi chứa các giá trị `enabled`, `cap_us` (giới hạn trên trong chế độ trực tiếp, `None` khi không hoạt động, bao gồm cả khi không có mục tiêu hiệu chuẩn trong khung hình), `k_exp_g` (hằng số được sử dụng để tính toán giới hạn trên) và:

| Trường | Ý nghĩa |
| --- | --- |
| `k_exp_g_source` | Hằng số nào đang được sử dụng: `camera` — của chính camera này, được tính toán từ điểm chuẩn trong gói hiệu chuẩn của nó (máy ảnh RGB); `fleet` — hằng số của đội máy ảnh, được sử dụng khi gói hiệu chuẩn không có điểm lab; `override` — giá trị ghi đè trường được thiết lập trên hệ thống backend. |
| `k_exp_g_fleet` | Hằng số của đội, để so sánh. |

### Mảng đồng bộ — `ArraySession` (Smart-Prep)

`connect_array` là **điểm khởi đầu được khuyến nghị** cho các cấu hình nhiều camera. Hệ thống này vận hành toàn bộ quy trình Smart-Prep thông qua giao diện người dùng đồ họa (GUI) ở chế độ nền:

1. **Phân tích mạng**(`/api/camera/array/recommend`) — thiết lập**cảm biến đầy đủ** (không gộp pixel, không cắt vùng quan tâm) và cho phép tốc độ khung hình giảm xuống mức mà máy chủ và mạng có thể cung cấp, dựa trên định dạng pixel và cách đóng gói mà Connect sẽ sử dụng. Các khung hình không bao giờ bị thu nhỏ để duy trì tốc độ: hãy truyền một giá trị nhỏ hơn cho `width` / `height` hoặc `binning=2` / `4` để đánh đổi độ phân giải lấy tốc độ khung hình (điểm cuốiliệt kê các tùy chọn này cùng với fps dự kiến của chúng).
2. **Chọn tự động theo cấp độ** — `sim-capture-sim-emit` (mọi camera phơi sáng cùng lúc khi có tín hiệu kích hoạt, tốc độ phát tín hiệu được điều chỉnh để chia sẻ băng thông, tốc độ khung hình giảm khi băng thông chia sẻ thu hẹp) trên bất kỳ mạng nào mà các camera có thể được điều chỉnh để chia sẻ băng thông; `slip-emit-and-capture` (các camera lần lượt sử dụng đường truyền, không phơi sáng đồng thời) chỉ khi không thể chia sẻ.
3. **PTP được bật**theo mặc định — các dấu thời gian giữa các camera đồng bộ với một đồng hồ chung với độ chính xác**~1 ms**, [máy chủ chính PTP](#ptp-grandmaster--time_sync_status) của hệ thống hậu kỳ. Việc phơi sáng đồng thời đến từ bộ kích hoạt phần cứng M8 (**< 100 µs** giữa các mô-đun), không phải từ PTP: PTP đồng bộ hóa *dấu thời gian*, không phải thời điểm phơi sáng.
4. **Tự động chọn định dạng pixel cho từng camera** — đơn sắc → `Mono12`, màu → `BayerRG12`.
5. **Ghi nhớ giá trị AE** — chụp nhanh trạng thái AE hiện tại của từng camera để kết nối không làm đặt lại thời gian phơi sáng khi đang hoạt động.
6. **Cấu hình kích hoạt GPIO** — `connect_array` kích hoạt mọi camera (`TriggerMode=On`, `TriggerSource=Line2`) để xung từ camera chínhcó thể điều khiển các camera phụ qua cáp M8. Đây là bước chỉ áp dụng cho mảng camera: nếu chỉ mở một camera duy nhất bằng lệnh `LatticeCamera`, camera đó sẽ hoạt động tự do. Khi giải phóng mảng camera (`disconnect()` / `force_disconnect()`, `chloros-cli lattice array-disconnect`) sẽ đưa mọi camera trở lại chế độ chạy tự do (`TriggerMode=Off`), trong khi các thiết lập khác vẫn giữ nguyên như khi kết nối: các thiết lập tín hiệu kích hoạt và đường đồng bộ (`TriggerSource`, `TriggerActivation`, `TriggerOverlap`, `TriggerDelay`, `LineMode` / `LineSource`; đường đồng bộ của máy chủ được đặt lại về chế độ đầu vào), PTP và các cài đặt máy ảnh đã thay đổi trong phiên làm việc — trừ khi `keep_settings=True` / `clear_settings=True` (xem [Bộ nhớ máy ảnh](#camera-memory--keep_settings--clear_settings)).

```python
import chloros_sdk

# First serial is the MASTER (fires the trigger pulse); rest are slaves.
with chloros_sdk.connect_array(
        ["213800234", "214000533", "214701288", "214701292"]) as arr:
    print(arr.array_id, arr.sync_mode, arr.ptp_enabled)
    arr.capture("output/", processing="reflectance")
```

Thuộc tính: `array_id`, `master_serial`, `slave_serials`, `ptp_enabled`, `sync_mode`, `packed_transport` và `packed_reason` (bốn khối cuối cùng là `None` trên một handle từ `attach_array`), `connect_stream` và `backend_url`. `connect_stream` là khối `stream` của phản hồi kết nối, hình dạng của kết nối đơn cộng với `members` (`{serial: {state, why?}}`): `streaming` ngay khi nhóm đồng bộ đầu tiên của mảng đến (hệ thống phía sau chờ đợi nó, trong khoảng thời gian giới hạn khoảng 10 giây), hoặc `no_frames` với lần lấy dữ liệu thất bại cuối cùng của mỗi thành viên làm `why` và, đối với các gói dữ liệu jumbo, giá trị dự phòng 1500 byte trong `note`. `None` trên một handle `attach_array`.

#### Chữ ký `connect_array()`

```python
connect_array(
    serials,                              # list[str]; serials[0] = master
    *,
    line="Line2",                         # GPIO sync line: Line0 | Line2 | Line3
    target_fps=None,                      # pin the master trigger rate (auto if None); see set_target_fps
    force_tier=None,                      # override tier picker; see below
    wire_ceiling_mbps=None,               # host sustained wire budget, MB/s (auto if None)
    width=None,                           # explicit frame size; skips network analysis
    height=None,
    pixel_format=None,
    binning=None,
    recommend=True,                       # set False to skip the recommend step
    ptp_enable=True,                      # set False to disable PTP
    packed="auto",                        # packed 12-bit transport: "auto" | "on" | "off"
    backend_url="http://127.0.0.1:5000",  # same IPv6-avoidance default as connect_camera
    timeout=600.0,                        # s; also the backend's deadline (see below)
    auto_start_backend=True,              # spawn a local backend if none is running
) -> ArraySession
```

`timeout` là thời gian cuộc gọi chờ kết nối, và nó được gửi đến
hệ thống phía sau (backend) dưới dạng thời hạn. Nếu kết nối không hoàn tất trước thời hạn đó, quá trình sẽ bị hủy: mọi camera đã
được mở sẽ được khôi phục về trạng thái ban đầu và đóng lại, không có mảng nào được kết nối, và
cuộc gọi sẽ gây ra lỗi `ChlorosConnectError` (HTTP 504 `CLIENT_DEADLINE_PASSED`, chỉ định
bước mà quá trình kết nối đã đạt đến). Do đó, một cuộc gọi gây ra lỗi này sẽ không bao giờ để lại một mảng
đã kết nối phía sau nó. Một mảng 9 camera kết nối trong khoảng một phút; ngay sau khi
các camera bật nguồn, quá trình này có thể mất vài phút. Một mảng đã hoạt động nhưng chưa có
nhóm khung hình đồng bộ sẽ được trả về, kèm theo mã lỗi `connect_stream["state"]`
`no_frames`. Nhật ký của hệ thống nền có một dòng ghi thời gian cho mỗi bước kết nối
(`[array-connect <id>] <step>: <s> s …`) và dòng cuối cùng ghi tên bước chậm nhất.

Các giá trị `force_tier`:
- `"sim-capture-sim-emit"` — đồng bộ thực sự (tất cả các camera đều kích hoạt cùng một cạnh đồng hồ).
- `"sim-capture-ftd-stagger"` — lệch pha linh hoạt trong miền thời gian (các cam phát tín hiệu ở các thời điểm lệch nhau một chút để các gói tin được tuần tự hóa trên đường truyền).
- `"slip-emit-and-capture"` — chụp tuần tự theo từng cam (không đồng bộ thời gian). Chỉ được chọn tự động khi các camera không thể được điều chỉnh tốc độ đủ chậm để chia sẻ đường truyền đồng thời.

`wire_ceiling_mbps` ghi đè lên **ngân sách đường truyền liên tục của máy chủ** tính bằng MB/s — con số duy nhất
mà toàn bộ việc phân bổ mảng phụ thuộc vào. Giữ nguyên giá trị `None` để sử dụng giá trị được phát hiện tự động. Giảm giá trị này khi mảng báo cáo các khung dữ liệu bị hỏng do GVSP: giá trị tự động được tính toán
từ tốc độ kết nối được NIC công bố, vốn thường đánh giá quá cao đối với bộ điều hợp USB, các làn PCIe hẹp và
các mạng chia sẻ đang quá tải — và sự đánh giá quá cao này thể hiện dưới dạng các khung dữ liệu bị hỏng thay vì là một
kết nối chậm rõ rệt. Giá trị này được lưu trữ trong khối ghi dữ liệu mảng của dự án, do đó việc
mở lại hoặc thiết lập lại `connect_array` sau này sẽ khôi phục nó giống như bất kỳ cài đặt mảng nào khác.
Xem [Tình trạng mảng](#array-health--which-subsystem-is-losing-frames).

#### Truyền tải đóng gói 12-bit (`packed=`)

Khi hoạt động ở công suất tối đa, tốc độ khung hình của mảng được xác định bởi tốc độ mà mỗi camera có thể gửi
các khung hình của mình. Các định dạng 12-bit (`BayerRG12`, `Mono12`) thường được truyền dưới dạng các
từ 16-bit; truyền tải đóng gói gửi cùng 12 bit trong 1,5 byte mỗi pixel và
giải nén chúng trên máy chủ, do đó mỗi khung hình mất ít thời gian hơn 25% để gửi — khoảng
**tăng thêm 33% khung hình mỗi giây khi cảm biến hoạt động hết công suất** (đo được: 10,7 → 14,0 fps trên
mảng 9 camera). **Dữ liệu được lưu trữ là giống hệt nhau**: cùng giá trị, cùng tệp 16-bit
và cùng siêu dữ liệu.

- `packed="auto"` (mặc định) chỉ nén khi điều đó có lợi **và** máy chủ có
  đủ khả năng giải nén: khi kết nối, phần hậu kỳ sẽ đo tốc độ giải nén của chính nó,
  số lõi CPU, tải hiện tại và băng thông mạng. Một máy tính để bàn có đủ lõi CPU
  sẽ nén dữ liệu từ tất cả các cảm biến; một máy chủ bận rộn hoặc có công suất thấp (máy tính một bo mạch,
  bộ điều khiển nhúng) sẽ truyền dữ liệu ở dạng không nén. Các vùng quan tâm (ROI) nhỏ và các khung hình được gộp lại (binned frames) hầu như không được lợi gì.
- `packed="off"` (hoặc `False`) không bao giờ nén — để loại trừ tùy chọn này khi chẩn đoán,
  hoặc trên máy chủ phải dành CPU cho các tác vụ khác.
- `packed="on"` (hoặc `True`) luôn đóng gói các định dạng 12-bit.
- Bất kỳ giá trị nào khác sẽ kích hoạt `ValueError`. Các định dạng 8-bit và 16-bit không bao giờ
  được đóng gói.

`analyze_array_network(..., packed="auto")` đưa ra quyết định tương tự,
do đó fps của nó chính là tốc độ mà mảng sẽ chạy (phản hồi chứa
`packed_transport` và `packed_reason`); giá trị mặc định của `pixel_format` là giá trị mà
connect áp dụng (Mono12 / BayerRG12) thay vì 8-bit.

Quyết định và lý do của nó nằm trong phiên làm việc:

```python
arr = chloros_sdk.connect_array(serials)          # packed="auto"
print(arr.packed_transport, arr.packed_reason)
# True  auto: projected 11.0 -> 14.4 fps, unpack 0.16 of 1.50 budgeted cores

arr = chloros_sdk.connect_array(serials, packed="off")
```

`ChlorosProject.connect_all(packed=...)` áp dụng cùng một thiết lập ghi đè cho mọi
mảng trong một dự án (mặc định là `None`, có nghĩa là `"auto"`: dự án không lưu
thiết lập đóng gói riêng cho từng mảng).
Để thiết lập mặc định cho toàn hệ thống, hãy đặt `CHLOROS_ARRAY_PACKED12=off` (hoặc `on`) trong
môi trường của backend; một giá trị `packed="on"` / `"off"` được chỉ định rõ ràng vẫn có ưu tiên cao hơn.
`/api/camera/array/status` báo cáo `packed_transport` và `packed_decision`.

#### Tốc độ khung hình dự kiến

Tốc độ khung hình mảng đã đo (đồng bộ phần cứng, 12-bit; cảm biến đầy đủ = 2048 × 1536). Sử dụng các giá trị này để kiểm tra tính hợp lý của `status()["actual_fps"]`; `analyze_array_network()` dự đoán tốc độ khung hình cho máy chủ và mạng của bạn.

| Cấu hình | Tốc độ khung hình mảng |
| --- | --- |
| Cảm biến đầy đủ, NIC máy chủ PCIe 10 GbE, truyền tải đóng gói (`packed="auto"` trên máy tính để bàn) | ~14 khung hình/giây |
| Cảm biến đầy đủ, NIC máy chủ PCIe 10 GbE, không nén (`packed="off"`) | ~10,5 khung hình/giây |
| Cảm biến đầy đủ, 8 camera trên bộ điều hợp USB 10 GbE | ~3,8 khung hình/giây |
| Cảm biến đầy đủ, kết nối máy chủ 1 GbE | 1,2–1,7 khung hình/giây |
| Ghép 2 × 2 (`binning=2`), NIC máy chủ 10 GbE | ~30–33 khung hình/giây |

- Trên kết nối PCIe 10 GbE, **số lượng camera** là yếu tố giới hạn khi sử dụng toàn bộ cảm biến, không phải mạng: 4 và 8 camera đều chạy ở cùng tốc độ.
- **`target_fps` là tốc độ kích hoạt, và mảng camera không bao giờ chạy nhanh hơn mức này.** `target_fps=1` chạy mảng ở tốc độ 1 khung hình/giây; `None` (mặc định) chạy mảng ở tốc độ cao nhất mà máy chủ và mạng có thể hỗ trợ. Tốc độ vượt quá mức này sẽ được hạ xuống mức đó (xem [`set_target_fps`](#interval-capture-for-drone-surveys)).
- **`link_speed_mbps` của `4294` trong `status()` không phải là tốc độ kết nối.** Điều này có nghĩa là Windows không thể đọc được tốc độ của bộ điều hợp (trình điều khiển đã báo cáo giá trị 32-bit lớn nhất), điều này thường xảy ra với các bộ điều hợp Ethernet USB. Tốc độ có thể sử dụng của bộ điều hợp USB trên Windows bị giới hạn ở mức 200 MB/s bất kể trường hợp nào (xem [Array Health](#array-health--which-subsystem-is-losing-frames)); để tối ưu hóa, hãy tham khảo phần "Bộ điều hợp USB 10GbE" trong tài liệu tham khảo CLI tại mục [Cài đặt & Tối ưu hóa NIC máy chủ](cli-reference.md#host-nic-setup--tuning-lattice-arrays).
- **Giới hạn tốc độ USB cũng áp dụng trên Linux: 400 MB/s trên x86_64, 200 MB/s trên NVIDIA Jetson.** Bộ điều hợp được xác định thông qua sysfs (sử dụng mã `ethtool -i` làm phương án dự phòng), do đó bộ điều hợp USB 5 GbE hoặc 10 GbE sẽ bị giới hạn ở mức trần này thay vì tốc độ danh định 500 / ~1060 MB/s. Con số trên x86_64 được đo trên Linux: cùng một bộ điều hợp gây hỏng khung hình khi vượt quá 200 MB/s trên Windows lại hoạt động ổn định với mảng 4 camera ở mọi mức giới hạn lên đến 480 MB/s. Trên NVIDIA Jetson, cùng một bộ điều hợp chỉ hoạt động ổn định ở tốc độ 200 MB/s và bị mất khung hình ngắt quãng khi vượt quá mức này. Nếu máy chủ của bạn xử lý được nhiều hơn mà không gặp vấn đề với các khung hình `gvsp_corrupt`, hãy tăng giới hạn đó bằng `wire_ceiling_mbps`. Trên Linux, bộ điều hợp cắm vào cổng USB 2.0 chỉ được phân bổ 30 MB/s; hãy cắm nó vào cổng USB 3.

#### Quá tải (giới hạn tối thiểu cho mỗi camera)

Mảng camera hoạt động trên bất kỳ mạng nào; tốc độ khung hình phụ thuộc vào điều kiện mạng. Cơ chế điều tiết Sim-emit phân bổ cho mỗi camera một phần bằng nhau trong giới hạn băng thông an toàn va chạm, và tất cả vẫn truyền dữ liệu trên cùng một tín hiệu kích hoạt; do đó, trên đường truyền chậm, phần băng thông — và tốc độ khung hình — sẽ thấp: mỗi camera 8trên mạng 1 GbE không sử dụng khung jumbo sẽ nhận được 6,5 MB/s mỗi camera, tương đương khoảng **1 khung hình/giây**(khoảng 1,3 khung hình/giây khi nén). Phần chia này được giới hạn ở tốc độ chậm nhất mà các camera có thể được điều chỉnh (`per_cam_floor_bps`: đọc từ phạm vi GevSCPD của từng camera khi kết nối, và không bao giờ dưới 1 MB/s). Nếu đường truyền yêu cầu tốc độ chậm hơn mức đó, quá trình kết nối sẽ chuyển sang chế độ phát**nối tiếp**(`slip-emit-and-capture`): các camera sẽ luân phiên hoạt động ở tốc độ liên kết tối đa, do đó mọi khung dữ liệu đều được truyền đến nhưng các thành viên**không được hiển thị đồng thời**— cấp độ phản hồi và các thông báo chặn (`SERIAL: ...`) sẽ chỉ ra điều này; truyền `force_tier="sim-capture-sim-emit"` để từ chối chế độ dự phòng. Kích thước khung hình không ảnh hưởng đến việc chia sẻ, do đó**việc gộp khung hình (binning) và vùng quan tâm (ROI) sẽ tăng tốc độ khung hình nhưng không thay đổi việc mảng có vừa với đường truyền hay không**. Chỉ đường truyền không thể truyền mảng ở bất kỳ tốc độ nào, dù đồng thời hay luân phiên, mới bị**quá tải** — chế độ lỗi là mất gói tin GVSP, không phải do tốc độ khung hình thấp hơn. Các biện pháp khắc phục: giảm số camera, sử dụng khung jumbo từ đầu đến cuối, hoặc sử dụng NIC nhanh hơn.

- Các phản hồi `analyze_array_network()` và `/api/camera/array/connect` chứa `oversubscribed`, `aggregate_demand_bps`, `collision_safe_ceiling_bps`, `max_cams_collision_safe` và `per_cam_floor_bps`. Khi `oversubscribed` là true, quá trình chiếu sẽ **đặt các trường fps về 0** (`achievable_fps_max` / `fps_bright` / `fps_dark`) thay vì báo cáo một tốc độ chậm nhưng vẫn hoạt động, điều này có thể gây hiểu lầm.
- `POST /api/camera/array/connect` chấp nhận tham số nội dung `pin_resolution` (**chỉ dành cho HTTP — không phải là tham số từ SDK**; `connect_array` không hiển thị tham số này). Đây là chế độ nghiêm ngặt: một kết nối bị đăng ký quá mức với `pin_resolution` được thiết lập sẽ bị**từ chối cứng** kèm theo lỗi nêu rõ mọi biện pháp khắc phục. Nếu không có tham số này, quá trình kết nối sẽ tiếp tục với cảnh báo rằng mảng sẽ mất gói tin. Dù trong trường hợp nào, kết nối vẫn giữ nguyên kích thước khung được yêu cầu — nó không bao giờ tự động gộp hoặc cắt bớt.
- Chỉ dành cho mục đích thử nghiệm: thiết lập `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED=1` trong môi trường của backend để hạ cấp mức từ chối thành cảnh báo rõ ràng — bạn vẫn có thể kết nối và chấp nhận việc mất gói tin.

#### Tình trạng mảng — hệ thống con nào đang mất khung

`ArraySession.capability()` (`GET /api/camera/array/<array_id>/capability`) mang theo một khối `health` đang hoạt động trên một
mảng đã kết nối, được đánh giá lại theo chu kỳ **10 giây** . Nó phân chia tình trạng mất khung
thành hai nguyên nhân cần các biện pháp khắc phục trái ngược nhau, thay vì một tỷ lệ “không hoàn chỉnh”
không chỉ ra cụ thể nguyên nhân nào:

| Trường | Ý nghĩa | Hệ thống con nào |
| --- | --- | --- |
| `gvsp_corrupt_rate_pct` (theo số sê-ri) | Khung hình **đã đến nhưng có cấu trúc sai**— mất gói tin GVSP. |**Mạng**: dung lượng đường truyền, tốc độ truyền, vòng nhận NIC, MTU |
| `never_arrived_rate_pct` (theo số sê-ri) | Khung dữ liệu **chưa bao giờ đến**— camera không kích hoạt, hoặc không có dữ liệu nào được gửi đi. |**Kích hoạt / đồng bộ**: Cáp M8, `line=`, `TriggerMode` |
| `worst_gvsp_corrupt_pct` / `worst_never_arrived_pct` | Tỷ lệ tệ nhất của camera mỗi máy. | — |
| `per_cam_rate_pct` | Tỷ lệ không hoàn chỉnh tổng hợp trên mỗi máy ảnh (cả hai nguyên nhân cộng lại). | — |
| `stable_for_seconds` | Thời gian mỗi máy ảnh duy trì ở mức dưới 0,01 %. | — |

Bên cạnh `health`, bản ghi này cũng báo cáo số liệu về toàn bộ phân bổ bị treo:

| Trường | Ý nghĩa |
| --- | --- |
| `wire_ceiling_mbps` | Dung lượng băng thông dây duy trì hiện tại của máy chủ, MB/s. |
| `wire_ceiling_source` | Nguồn gốc của con số đó, bằng văn bản — ví dụ: `USB-capped 200 MB/s (was theoretical 1062; …)` hoặc `user override 120 MB/s (auto said 200)`. |
| `wire_ceiling_is_user_set` | `true` khi `wire_ceiling_mbps=` thiết lập giá trị này. |
| `nic_is_usb` | `true` dành cho bộ chuyển đổi Ethernet qua USB. |

```python
import chloros_sdk

serials = ["213800234", "214000533"]           # master first
arr = chloros_sdk.attach_array(serials)
h = arr.capability()

health = h.get("health", {})
print("wire ceiling:", h["wire_ceiling_mbps"], "MB/s", h["wire_ceiling_source"])
print("corrupt (network) :", health.get("worst_gvsp_corrupt_pct"), "%")
print("absent  (trigger) :", health.get("worst_never_arrived_pct"), "%")

if (health.get("worst_gvsp_corrupt_pct") or 0) > 1.0:
    # Network path. Reconnect with a lower budget -- NOT a lower target_fps.
    # force_disconnect(), not disconnect(): this handle came from attach_array,
    # and disconnect() never tears down an array the session only attached to.
    # It returns only once the backend confirms the release; a teardown that
    # timed out or was refused raises ChlorosConnectError instead, so the
    # reconnect below never races a half-released array.
    arr.force_disconnect()
    arr = chloros_sdk.connect_array(serials, wire_ceiling_mbps=120)
```

**Giải thích:** Giá trị `gvsp_corrupt_rate_pct` khác 0 và `never_arrived_rate_pct` bằng 0 có nghĩa là
tín hiệu kích hoạt và đồng bộ cáp hoàn hảo, và 100% mức mất mát nằm trên đường truyền mạng — giảm
`wire_ceiling_mbps` và kết nối lại. Mô hình ngược lại chỉ ra rằng vấn đề nằm ở cáp đồng bộ hoặc
dây kích hoạt.

> **`target_fps` không phải là yếu tố quyết định đối với các khung dữ liệu bị hỏng.** Tốc độ điều chỉnh GevSCPD được thiết lập một lần khi
> kết nối, do đó việc giảm tần số kích hoạt sẽ thay đổi chu kỳ làm việc chứ không phải
> tốc độ phát xung đồng thời. Việc giảm 5 lần nhu cầu đã không mang lại cải thiện, trong khi
> việc giảm giới hạn băng thông từ 240 xuống 200 MB/s đã đưa cùng một hệ thống từ tỷ lệ khung bị hỏng 10,4% xuống
> 0,00%.

> **Một mảng đang hoạt động không thể tự lập kế hoạch lại trên firmware LATTICE (M3C / M3M).** Nó không thể
> tự khắc phục vấn đề này; hãy ngắt kết nối và kết nối lại để công cụ chọn thời gian kết nối tự động lập kế hoạch lại dựa trên
> giới hạn mới.

Một **bộ chuyển đổi Ethernet USB bị giới hạn ở 200 MB/s** bởi thiết bị đo lường bất kể
nhãn hiệu: bảng hiệu suất chuyển đổi tốc độ kết nối thành giá trị ổn định được
dựa trên PCIe, và bộ điều hợp mạng USB (NIC) công bố tốc độ kết nối Ethernet của mình nhưng bị giới hạn bởi
bus USB và trình điều khiển của nó. Giới hạn này là tuyệt đối, không phải tỷ lệ phần trăm — một bộ chuyển đổi USB 1 GbE
đạt ~80 MB/s và không bị ảnh hưởng. Giới hạn này áp dụng cho Windows và Linux (bao gồm Jetson);
xem [Tốc độ khung hình dự kiến](#expected-frame-rates) để biết nơi đã đo được.

#### Các phương thức `ArraySession`

| Phương thức | Mô tả |
| --- | --- |
| `status(timeout=10.0)` | Trạng thái thời gian thực của mảng đang chạy: `target_fps` / `target_fps_auto` (tốc độ kích hoạt và liệu bộ điều khiển tốc độ có kiểm soát nó hay không), `target_fps_clamped_reason` (lý do tại sao tốc độ cố định được giữ ở mức thấp hơn, nếu không thì `None`), `actual_fps` (được đo; bằng 0 khi vòng lặp grab đã từ bỏ hoặc bị đình trệ), `recommended_fps`, `streaming` (False khi vòng lặp grab đã từ bỏ — hãy kiểm tra giá trị này, không chỉ `last_error`; giữ giá trị True sau khi khởi động lại cài đặt, điều mà `restarting` báo hiệu), `stalled` (True khi vòng lặp grab đang chạy nhưng chưa phát hành nhóm khung hình nào trong khoảng mười chu kỳ khung hình, 2–30 giây; `last_error` chỉ báo điều này sau 60 giây), `last_frame_age_s`, `last_error`, `serials`, `ptp_enabled` (`True` chỉ khi mảng được kết nối với PTP **và** mọi thành viên đã đọc lần cuối `PtpEnable` ở trạng thái bật; không giống như trạng thái bị khóa, mà `ptp_epoch` đo lường), `ptp_configured` (yêu cầu từ kết nối), `ptp_off_members` / `ptp_unread_members` (các thành viên lần cuối đọc trạng thái `PtpEnable` ở chế độ tắt / không trả lời), `member_recovery` (`None`, hoặc một thành viên bị mất nguồn hoặc mất kết nối: `state`, `waiting` trong khi nó không trả lời, `reopening`, sau đó là `recovered`, cùng với `members` và `why` — mảng sẽ mở lại kết nối khi thiết bị này phản hồi trở lại và khôi phục thời gian kết nối PTP, cấu hình dây tín hiệu kích hoạt và đồng bộ, tốc độ khung hình và cài đặt camera, sau đó tiếp tục truyền dữ liệu mà không có các giá trị mới như `connect_array`), và `ptp_epoch` (được đo), `sync_mode` / `sync_tier`, `packed_transport` / `packed_decision`, `per_cam_emit_mbps`, `link_speed_mbps` (`4294` có nghĩa là Windows không thể đọc được tốc độ của bộ điều hợp — xem [Tốc độ khung hình dự kiến](#expected-frame-rates)), `trigger_pipeline` / `pipeline_tune`, `radiometric_chain` (liệu chuỗi độ rạng rỡ theo từng dải tần có thể sử dụng được hay không, và thành phần nào đã làm hỏng chuỗi đó), và `band_clamp` — trong khi chế độ xem trực tiếp hiển thị mảng: tỷ lệ phần trăm các pixel được chiếu sáng mà quá trình tách màu đã giới hạn ở mức 0, cho từng thành phần (`members`: `{serial: [band0, band1, band2]}` theo thứ tự dải R, G, B của máy ảnh) và theo từng dải, hình ảnh tổng hợp hiển thị (`composite`: `{band name: share}`), cùng với `warn`, tỷ lệ phần trăm mà giao diện người dùng (GUI) đánh dấu là “Dữ liệu dải có độ tin cậy thấp” (được đo khoảng một lần mỗi giây; `None` khi không có chế độ xem trực tiếp nào đang chạy), và `member_ae` — cho từng thành viên, thời gian phơi sáng (`exposure_us`) và hệ số khuếch đại (`gain_db`) mà khung hình được công bố gần nhất của nó mang theo, lấy từ chính khung hình đó (`read_nodes` của `ExposureTime` / `Gain` trả về giá trị cuối cùng được ghi trong khi chế độ phơi sáng tự động đang chạy, không phải giá trị hiện tại). |
| `set_member_settings(serial, **kwargs)` | Ghi cài đặt của một camera thành viên, như `CameraSession.set_settings()` lấy chúng (phơi sáng, độ khuếch đại, độ sáng mục tiêu, ...); thành viên đó vẫn nằm trong mảng. Gây ra lỗi `ChlorosConnectError` nếu đối tượng không phải là thành viên của mảng. Xem [Phơi sáng trên các thành viên mảng](#exposure-on-array-members). |
| `capability(timeout=10.0)` | Các thông số mà smart-prep đã xác định (cấp độ ghi hình, kích thước khung hình, `pixel_format`), khối `health` đang hoạt động và ngân sách băng thông của máy chủ. Xem [Tình trạng mảng](#array-health--which-subsystem-is-losing-frames). |
| `set_target_fps(fps, timeout=10.0)` | Thay đổi tần suất kích hoạt trong khi mảng đang hoạt động, mà không cần kết nối lại. Một giá trị cố định nó (bộ điều khiển tần suất không can thiệp); `None` đưa nó về chế độ tự động. Trả về `{requested, target_fps, target_fps_auto, clamped}` — `clamped` là True khi tốc độ đã được hạ xuống mức mà máy chủ và mạng có thể xử lý. Xem [Chụp theo khoảng thời gian cho khảo sát bằng máy bay không người lái](#interval-capture-for-drone-surveys). |
| `grab(processing="debayered", with_metadata=False, apply_calibration=True, apply_white_balance=True, apply_index=False, index_expression=None, bit_depth=None, band=None, align=False, refl_max=None)` | Một khung hình từ mỗi camera, **tất cả từ một tín hiệu kích hoạt**, dưới dạng mảng numpy (không ghi tệp). Trả về một `GrabResult` (một `dict`): `{serial: array}`, hoặc `{serial: (array, metadata)}` với `with_metadata=True`. `.group_id` là tín hiệu kích hoạt chung cho các khung hình; `.errors` ánh xạ từng camera bị loại trừ vào lý do (một camera không thể được phục vụ tại tín hiệu kích hoạt đó sẽ bị bỏ qua, không bao giờ được bù đắp từ camera khác). `processing` là `raw` / `debayered` / `radiance` / `reflectance` / `display`. Cách đọc này tương tự như [`ArrayHandle.grab()`](#one-trigger-per-read--group_id) trong API của dự án — một triển khai chung — do đó, mọi từ khóa ở đó (`band`, `align`, `bit_depth`, `refl_max`) đều hoạt động ở đây. Cần có `numpy`. |
| `frame_stream(*, processing="debayered", fps=5.0, duration=None, count=None, apply_calibration=True, apply_white_balance=True, apply_index=False, index_expression=None, with_metadata=False, bit_depth=None, band=None, align=False, refl_max=None)` | Trình lặp trả về một kết quả `grab()` mỗi tick (chỉ chấp nhận tham số từ khóa), được điều chỉnh theo `fps`, giới hạn bởi `duration` (giây) và/hoặc `count` (`None` = không giới hạn). **Mỗi tập hợp được trả về là một tín hiệu kích hoạt mới:** một tick mà tín hiệu kích hoạt mới nhất là tập hợp mà nó đã tạo ra sẽ chờ tín hiệu kích hoạt tiếp theo thay vì lặp lại nó, do đó luồng dữ liệu chạy với tốc độ chậm hơn giữa `fps` và tần suất khung hình của chính mảng. Gây ra sự kiện `FrameFetchError` thay vì im lặng: ngay lập tức khi mọi camera đều từ chối vì lý do nào đó khiến tick tiếp theo sẽ lặp lại, sau 60 giây tick không có khung hình, và sau 60 giây không có trigger mới (“không có trigger mới trong 60 giây”). Cách triển khai tương tự như `ArrayHandle.frame_stream()`. Xem [Khung hình trực tiếp trong quy trình xử lý của riêng bạn](#live-frames-in-your-own-pipeline). |
| `continuous(output_dir="output", *, format="tiff", processing="debayered", levels=None, aligned=None, render_index=None, force_daq=None, orient=None, max_captures=None, duration_s=None, daq=None, daq_tolerance_ms=None, timeout=30.0)` | Khởi động chế độ ghi liên tục riêng của backend (chế độ Liên tục trên GUI) → [`ContinuousHandle`](#continuoushandle). Lưu **mọi** nhóm đồng bộ mà mảng xuất bản, giống như cách `capture()` sẽ lưu (các kiểm soát xuất tương tự), cho đến khi dừng lại hoặc cho đến khi `max_captures` / `duration_s` (`None` = cho đến khi dừng lại). Gây ra lỗi `ChlorosConnectError` nếu đã có một tác vụ đang chạy cho mảng này. |
| `capture(output_dir="output", format="tiff", processing="debayered", levels=None, aligned=None, render_index=None, force_daq=None, orient=None, smart=False, after_seq=None, daq=None, daq_tolerance_ms=None, timeout=300.0)` | Một nhóm ghi hình đồng bộ. Trả về một `CaptureResult` (danh sách các từ điển khung hình + `.skipped` + `.failed` + `.group_seq` + `.daq_files` + `.daq_matches` + `.alignment` + `.oriented`). Truyền kết quả trước đó`group_seq` thành `after_seq` để lấy nhóm đồng bộ tiếp theo sau nó thay vì chờ một nhóm mới. Các tùy chọn xuất dưới đây. |
| `capture(..., smart=True)` | **Chụp thông minh** — chờ AE ổn định trên tất cả các camera, sau đó kích hoạt. |
| `capture_fastest(output_dir="output", force_daq=True, render_index=True, orient=None, timeout=120.0)` | Chụp nhanh nhất: chỉ dữ liệu thô + giá trị đọc DAQ được gán (+ chỉ số kết hợp miễn phí). Tương ứng với nút "Ghi nhanh nhất" trên giao diện người dùng. |
| `capture_repeated(output_dir="output", count=None, duration_s=None, interval_s=0.0, on_capture=None, **capture_kwargs)` | Chế độ Đơn / Liên tục / Khoảng thời gian trong một vòng lặp có giới hạn. Trả về `list[CaptureResult]`.**Yêu cầu `count` và/hoặc `duration_s`** để kết thúc (SDK không hỗ trợ Ctrl+C). Cũng kết thúc sớm, sau lượt quét, khi một lần quét báo cáo ổ đĩa đầy (một mục `.failed` kèm theo `disk_full`); lần quét đó là kết quả cuối cùng. |
| `record(output_dir="output", fps=10.0, duration_s=None, video=True, gif=False, expr=None, timeout=30.0)` | Bắt đầu ghi lại chế độ xem chỉ mục kết hợp trực tiếp thành video/GIF → `RecorderHandle`. Video là định dạng Motion-JPEG `.avi` (không cần ffmpeg). Khi không có cửa sổ GUI nào mở, hệ thống nền sẽ hiển thị nó: `expr` (một biểu thức chỉ mục trong tên dải của mảng, ví dụ: `"(NIR_850_RGN-Red_660_RGN)/(NIR_850_RGN+Red_660_RGN)"`), hoặc giá trị NDVI của mảng theo mặc định. Mỗi mảng có một bộ ghi tổng hợp. |
| `burst(output_dir="output", duration_s=None, max_frames=None, index_config=None, serial_index_config=None, orient=None, timeout=30.0)` | Bắt đầu chụp liên tục Bayer thô với tốc độ khung hình cao → `RecorderHandle`. Xử lý lại ngoại tuyến với `build_video()`. `orient=True`: các sản phẩm do `build_video()` tạo ra được xoay theo hướng của máy ảnh (các sản phẩm riêng lẻ theo hướng của từng máy ảnh, chỉ số kết hợp theo hướng của máy ảnh chính); các khung hình thô giữ nguyên định dạng gốc và tệp manifest của chuỗi chụp ghi lại hướng của từng thành viên. |
| `build_video(burst_dir, products=None, fps=10.0, video=True, gif=False, save_tiffs=False, wait=True, poll_s=2.0, timeout=1800.0)` | Xử lý lại ngoại tuyến một loạt ảnh thô đã lưu thành video đã hiệu chỉnh. Chờ cho đến khi hoàn tất (`wait=True`) và trả về kết quả xây dựng: `{success, outputs, errors, skipped, substitutions, notes, degraded, combined}` — xem [kết quả xây dựng](#the-build_video-result) dưới đây. |
| `build_video_status(job_id, timeout=15.0)` | Kiểm tra trạng thái công việc xây dựng ngoại tuyến: `{running, result, error, burst_dir}`. |
| `disconnect(keep_settings=False, clear_settings=False)` | Giải phóng toàn bộ mảng — **nếu phiên làm việc này đã mở nó** (`connect_array`): lúc đó sẽ là `force_disconnect()`, và sẽ gây ra lỗi `ChlorosConnectError` khi việc giải phóng không được xác nhận (khối `with` không bao giờ gây ra ngoại lệ khi thoát). Khi nhận handle từ `attach_array`, nó để mảng tiếp tục chạy và in một dòng thông báo lên stderr: mảng này có thể thuộc về GUI hoặc một skript khác. Mỗi camera được để chạy tự do và giữ nguyên trạng thái như khi kết nối; `keep_settings=True` lưu cài đặt của nó vào bộ nhớ riêng của camera, `clear_settings=True` đặt lại bộ nhớ đó về cài đặt gốc (xem [Bộ nhớ camera](#camera-memory--keep_settings--clear_settings)). |
| `force_disconnect(timeout=60.0, keep_settings=False, clear_settings=False)` | Giải phóng toàn bộ mảng bất kể ai đã mở nó. Sử dụng lệnh này trên một handle đã kết nối khi mục đích của bạn là kết thúc mảng (ví dụ: để kết nối lại với các cài đặt khác). Hàm trả về ngay khi backend xác nhận việc giải phóng, hoặc khi không còn gì để giải phóng (mảng đã không còn tồn tại, hoặc không có backend nào đang chạy); việc giải phóng hết thời gian chờ hoặc bị backend từ chối sẽ gây ra lỗi `ChlorosConnectError`. Với bất kỳ cờ nào, hàm sẽ trả về dữ liệu mà bộ nhớ của từng camera đã nhận được (`camera_memory`), nếu không thì trả về `None`. |

`capture()` kiểm soát xuất (cùng điểm cuối mà GUI/CLI sử dụng):

- `processing` / `levels` — `processing="all"` (hoặc `levels=["raw","radiance",…]`) lưu mọi loại xuất dữ liệu áp dụng cho từng camera; một giá trị `processing` duy nhất chỉ lưu cấp độ đó.
- `aligned` — **mặc định là bật (`None`), vì việc ghi hình cùng mảng của ứng dụng Chloros**(Cài đặt chụp**Căn chỉnh** cho từng mảng). Dữ liệu xuất không thô của mỗi thành viên được căn chỉnh vào khung hình của camera chính theo [hồ sơ căn chỉnh](#array-alignment) và được cắt xén theo vùng chồng chéo chung của các thành viên, do đó các tệp được đăng ký theo pixel và có kích thước, vùng cắt xén cùng các thẻ `Alignment*` giống như ứng dụng thực hiện (trong vùng ROI, phép biến đổi được căn chỉnh lại theo cửa sổ); `XMP-mapir:AlignmentJSON` chỉ định `"applied": true` cùng với `crop_roi` của nó. Tệp thô vẫn giữ nguyên kích thước gốc mà không bị biến dạng nhưng mang theo thông tin biến đổi trong siêu dữ liệu. `aligned=False` lưu mọi tệp ở kích thước gốc, không biến dạng; `aligned=True` là cài đặt mặc định được nêu rõ. `continuous()`, `capture(smart=True)`, `capture_repeated()` và các tùy chọn cấp dự án `ArrayHandle.capture()` / `capture_smart()` đều sử dụng cùng một thiết lập mặc định. Cho dù có áp dụng biến dạng hay không, mỗi tệp thành viên sẽ ghi lại phép biến đổi `Alignment*` của máy ảnh khi mảng có cấu hình căn chỉnh — ngoại trừ máy ảnh mà cấu hình không căn chỉnh (bị bỏ qua, hoặc bị ép về ma trận đơn vị do giải quyết thất bại), và máy ảnh tham chiếu khi không có máy ảnh nào khác được căn chỉnh: những máy ảnh này không mang thẻ `Alignment*`. Nếu mảng không có hồ sơ, dữ liệu chụp sẽ được lưu ở trạng thái chưa căn chỉnh, và kết quả sẽ ghi rõ điều này: `result.alignment` là `{"requested": True, "applied": False, "warnings": [...]}`, mỗi lý do khiến hiệu ứng biến dạng không thể áp dụng sẽ tương ứng với một cảnh báo (không có hồ sơ, các thành viên có độ phân giải khác nhau, mảng không thể, khung hình không thể ánh xạ lên cấu hình). `applied` là `True` chỉ khi mọi tệp xuất yêu cầu hiệu chỉnh biến dạng đều nhận được hiệu chỉnh đó. Tính năng tự động căn chỉnh của mảng sẽ tạo cấu hình ngay khi độ phơi sáng ổn định; `ArrayHandle.calibrate_alignment()` / `align-calibrate` của CLI sẽ tạo cấu hình theo yêu cầu. `result.oriented` cho biết liệu `orient=True` đã xử lý các sản phẩm hay chưa.
- `render_index=False` — bỏ qua lớp phủ chỉ số thảm thực vật cho từng camera; mặc định sẽ hiển thị lớp phủ này ở vị trí đã cấu hình.
- `daq` / `daq_tolerance_ms` — giá trị đọc từ DAQ được lưu dưới dạng tệp phụ `.daq` theo mặc định, ngay cả khi không có cấp độ nào được chọn yêu cầu nó; `daq=False` sẽ bỏ qua việc này, và `daq_tolerance_ms` thiết lập độ dung sai khớp (mặc định là 500). `force_daq=True` là cách viết cũ của giá trị mặc định. Xem [Giá trị DAQ mà một bản ghi lưu lại](#the-daq-reading-a-capture-saves--daq_files-and-daq_matches).
- `orient=True` — chuyển đổi các **sản phẩm**của từng thành viên (`display`, `radiance`, `reflectance`, chỉ số từng camera và hình ảnh tổng hợp) theo hướng của camera, sau khi thực hiện biến dạng căn chỉnh: theo hướng của**máy ảnh chính**khi chúng được biến dạng vào khung hình của máy ảnh đó (giá trị mặc định là `aligned` — các thành viên vẫn được đăng ký chung), nếu không thì theo hướng của chính máy ảnh của chúng.**Mặc định: mỗi tệp thành viên giữ định dạng gốc**, như các bản chụp mảng luôn có; `raw` và `debayered` vẫn giữ định dạng gốc trong cả hai trường hợp. Mỗi tệp ghi lại hướng của khung hình mà các pixel của nó nằm trong (`XMP-Camera:ChlorosRotation` / `ChlorosFlip*` / `ChlorosOrientationApplied` — tệp RAW, theo khung hình gốc của chính máy ảnh đó), và tệp XMP `Alignment*` vẫn giữ biến đổi khung hình gốc, được áp dụng trước hướng; một tệp RAW chứa phép biến đổi đó cũng ghi lại `ChlorosAligned*` (của khung hình chính), tức là hướng của sản phẩm đã được biến dạng vào khung hình chính. Quá trình xử lý xoay từng sản phẩm theo phép biến đổi thuộc về các pixel của nó: phép biến đổi đã căn chỉnh khi biến dạng tệp RAW (Chế độ căn chỉnh mảng bật), và phép biến đổi của chính máy ảnh khi không căn chỉnh. `capture_repeated()` và `capture_fastest()` truyền nguyên vẹn thông tin này qua; các biến `ArrayHandle.capture()` / `capture_smart()` ở cấp độ dự án cũng giữ nguyên thông tin này.

**Nén TIFF (nút điều chỉnh HTTP duy nhất):** `ArraySession.capture()` không gửi khóa `compression`, do đó mặc định của backend sẽ được áp dụng — `POST /api/camera/array/capture` đọc tham số nội dung `compression`, `"none"` theo mặc định: không nén, các pixel của cảm biến được đọc chính xác như ban đầu (~6,3 MB cho mỗi khung hình độ phân giải đầy đủ). `"deflate"` (zlib L1 không mất dữ liệu + bộ dự đoán ngang, ~4,1 MB/khung hình) ghi các tệp nhỏ hơn nhưng chậm hơn — cả hai đều không mất dữ liệu và được đọc giống hệt nhau khi nhập. SDK không cung cấp tham số dòng lệnh (kwarg) cho tùy chọn này; giải pháp thay thế là `chloros-cli lattice array-capture --compression deflate` hoặc HTTP thô. DEFLATE chiếm dụng GIL của Python, do đó việc ghi dữ liệu nén không thể thực hiện song song trên các luồng ghi riêng lẻ cho từng camera — việc ghi liên tục ở độ phân giải đầy đủ với tốc độ cảm biến yêu cầu sử dụng chế độ mặc định không nén. Chi tiết: [Tham khảo CLI → array-capture](cli-reference.md).**Các nhóm liên tiếp (giao thức HTTP):** điểm cuối trả về `group_seq`; một vòng lặp gửi lại giá trị này dưới dạng `after_seq` trong yêu cầu tiếp theo sẽ nhận được nhóm đồng bộ tiếp theo — bao gồm cả nhóm được xuất bản trong khi quá trình ghi hình trước đó đang lưu trữ — thay vì phải chờ nhóm được xuất bản sau yêu cầu, và `skipped_groups` báo cáo bất kỳ nhóm nào mà vòng lặp đã bỏ lỡ. `capture_repeated`, `--continuous` của CLI và Chế độ Liên tục của GUI đều thực hiện điều này; các khoảng thời gian được cập nhật mới tại mỗi thời điểm đã lên lịch.**Các tùy chỉnh xuất theo từng thành viên (chỉ dành cho HTTP):**cùng một điểm cuối cũng chấp nhận `exclude_serials` (danh sách — loại bỏ các thành viên khỏi tập hợp đã lưu; mảng vẫn được kích hoạt như một nhóm đồng bộ và các thành viên bị loại trừ được trả về trong `excluded`), `serial_levels` (các tùy chỉnh cấp độ camera riêng lẻ `{serial: [level tokens]}`) và `serial_index` (các tùy chỉnh lớp phủ chỉ mục riêng lẻ cho từng camera `{serial: bool}`). Đây là các tham số cơ thể tương đương với GUI và**chưa phải là các tham số từ khóa (kwargs) củSDK**; các thành viên không có trong các bản đồ sẽ được thay thế bằng các giá trị chung cho toàn mảng `levels` / `render_index`.

##### Kiểm tra các camera bị bỏ qua — `CaptureResult.skipped`

`ArraySession.capture()` trả về một `CaptureResult`, đây là một lớp con của `list`: lặp qua nó, truy cập theo chỉ số, `len()` — mọi mẫu hiện có vẫn hoạt động bình thường. Mã mới có thể kiểm tra thuộc tính `.skipped` để xem những cam nào đã bị loại trừ và lý do tại sao. Trường hợp phổ biến nhất là các cảm biến RGB trong mảng bộ lọc hỗn hợp khi bạn yêu cầu `processing="radiance"` hoặc `"reflectance"` — vì độ sáng theo từng điểm Bayer không có ý nghĩa đối với cảm biến băng thông rộng, nên hệ thống xử lý phía sau sẽ bỏ qua các camera đó thay vì tạo ra kết quả vô nghĩa.

```python
with chloros_sdk.connect_array(serials) as arr:
    result = arr.capture("output/", processing="reflectance")

    # Back-compat: iterate as a plain list
    for frame in result:
        print(frame["filepath"], frame["serial"])

    # New: see why N-1 cams were saved
    for skip in result.skipped:
        print(f"skipped SN:{skip['serial']} reason={skip['reason']}")
        # e.g. {'serial': '214701292', 'level': 'reflectance',
        #       'reason': 'reflectance-not-applicable-to-rgb-cam',
        #       'filter': 'RGB'}
```

Các mã lý do tuân theo mẫu `<level>-not-applicable-to-rgb-cam` (một mục cho mỗi cấp độ bị bỏ qua, mỗi mục chứa `level`). Một camera không được tải gói hiệu chuẩn sẽ bỏ qua `radiance` / `reflectance` bằng `<level>-needs-calibration (connect the camera so its cal pack loads)`, và khi không có giá trị phản xạ từ DAQ có thể sử dụng được, nó sẽ bỏ qua với mã `reflectance-skipped-no-dls`. Các trường hợp bỏ qua khác liên quan đến phản xạ bao gồm `reflectance-skipped-no-fresh-dls` (không có giá trị phản xạ xuống mới), `reflectance-skipped-bound-daq-unavailable (…)` (không thể kết nối với DAQ đã được gán), và `dls-uncalibrated-band-<nm>` — dải sóng này chủ yếu nằm ngoài phạm vi hiệu chuẩn quang phổ của cảm biến ánh sáng DAQ (~374–974 nm), do đó việc phân chia độ phản xạ tuyệt đối dựa trên DAQ bị từ chối và khung hình sẽ chuyển xuống chế độ phản hồi cảm biến. Trong số các SKU đang bán, chỉ có F988 gây ra lỗi này; quy trình làm việc được hỗ trợ của máy ảnh đó là quy trình sử dụng bảng độ phản xạ.

##### Các tệp không thể ghi — `CaptureResult.failed`

Một lần chụp chỉ có thể ghi được một số tệp vẫn trả về: danh sách chứa mọi tệp **đã** được ghi, và `.failed` liệt kê phần còn lại, mỗi tệp tương ứng với một đối tượng với `serial` (`None` cho tệp tổng hợp kết hợp và tệp phụ `.daq` của mảng), `level` (`raw`, `radiance`, …, `index`, `composite`, `daq`, hoặc `match` — nguồn gốc của `.daq` và `.match.json`, khi `.daq` đã được ghi nhưng tệp sidecar của nó thì chưa) và `error`. Trên một ổ đĩa đã đầy, mục nhập này cũng chứa `disk_full: True`; mọi lần sao lưu sau đó vào ổ đĩa đó đều sẽ thất bại theo cách tương tự, đó là lý do tại sao `capture_repeated()` dừng lại ở đó. Không có ngoại lệ nào được kích hoạt đối với việc ghi dữ liệu một phần, và tệp trong `.failed` không bị bỏ lại trên đĩa ở trạng thái ghi dở dang. `.failed` trống rỗng khi ghi dữ liệu hoàn chỉnh.

```python
result = arr.capture("output/", processing="all")
for f in result.failed:
    print(f"SN:{f['serial']} {f['level']} not written: {f['error']}")
if any(f.get("disk_full") for f in result.failed):
    raise SystemExit("drive full -- free space and capture again")
```

##### DAQ đọc giá trị khi lưu bản ghi — `.daq_files` và `.daq_matches`

Mỗi khi một DAQ được liên kết với camera hoặc mảng, hoặc được gộp chung, mỗi bản ghi sẽ lưu lại giá trị đo hướng xuống mà nó đã sử dụng: một tệp `.daq` trong thư mục con `daq/` của thư mục bản ghi, cùng với các tệp `.match.json` và `.cameras.json` (đó là giá trị đo của nó). Điều này cũng áp dụng cho cả bản chụp thô (raw) hoặc đã tách màu (debayered), và tên tệp sẽ giống nhau dù bản chụp được thực hiện từ giao diện người dùng (GUI), dòng lệnh (CLI) hay bộ công cụ phát triển phần mềm (SDK). `daq=False` sẽ bỏ qua bước này đối với bản chụp có mức độ không sử dụng DAQ (mức `radiance`, `reflectance` hoặc `display` vẫn ghi thông tin này); `daq_tolerance_ms` điều chỉnh ngưỡng dung sai cho lần lần chụp đó chứ không bao giờ là giá trị đọc được sử dụng. DAQ nào: DAQ được liên kết với camera hoặc mảng (lựa chọn cảm biến ánh sáng trên GUI, liên kết của dự án, hoặc `chloros-cli daq pool-bind`), nếu không thì là DAQ mới nhất mà backend có. CLI định nghĩa các lệnh điều khiển `--no-daq` và `--daq-tolerance-ms` ([Tham chiếu CLI](cli-reference.md#the-daq-reading-a-capture-saves)).

```python
r = cam.capture("out", levels=["raw"])      # a CameraSession; ArraySession results match
for m in r.daq_matches:                      # one per .daq in r.daq_files
    print(m["daq_file"], m["gap_ms"], m["in_tolerance"])
```

Một lần ghi lại sẽ lưu giá trị đọc từ cảm biến ánh sáng được sử dụng dưới dạng `.daq` kèm theo một giá trị `<file>.daq.match.json` bên cạnh, mô tả cách giá trị đọc đó được liên kết với thời gian phơi sáng: `method`, `gap_ms` (từ thời điểm phơi sáng đến giá trị đọc DAQ gần nhất) và `in_tolerance` (trong vòng 500 ms, hoặc giá trị `daq_tolerance_ms` của lần ghi lại). Tệp XMP của tệp phản xạ chứa kết quả tương tự như `Chloros:DlsMatchMethod`, `Chloros:DlsMatchGapS` và `Chloros:DlsInTolerance`.

Một lần ghi dữ liệu sẽ chờ giá trị đo đầu tiên từ DAQ sau khi phơi sáng, để có thể kết hợp các giá trị đo ở hai bên của nó (`method` `interpolated`). Một DAQ-E gửi giá trị đo mỗi 0,4 giây vào ban ngày và mỗi 1–2 giây trong nhà, do đó `capture()` sẽ trả về sau khoảng một rưỡi khoảng thời gian đó; chính quá trình phơi sáng không bị trì hoãn. Một DAQ đã ngừng gửi dữ liệu sẽ không được chờ đợi. Giá trị đo của DAQ-E là giá trị trung bình của ánh sáng trong khoảng thời gian kể từ lần đo trước đó, do đó nó được đặt ở giữa khoảng thời gian đó: `gap_ms` được đo tại điểm giữa đó, và `.match.json`'cung cấp dấu thời gian riêng của các giá trị đo. `ArraySession.continuous()` chờ mà không làm chậm quá trình thu thập. Các lần truyền ngượckhông chờ đợi, do đó chúng duy trì tốc độ của mình và được khớp với giá trị gần nhất trước khi phơi sáng.

Một giá trị đo lường không bao giờ hết hạn, do đó không có khung hình nào bị mất do giá trị đến muộn — thay vào đó, nó được gắn nhãn:

- **Dòng dữ liệu của DAQ bị gián đoạn** — giá trị đo mới nhất của nó được sử dụng, và `gap_ms` cho biết độ trễ của nó (`in_tolerance: false` đã quá 500 ms).
- **Không có DAQ để đọc**— bị ngắt kết nối (`disconnect()`, `chloros-cli daq pool-disconnect`), hoặc không thể kết nối với DAQ đã được liên kết — giá trị đọc cuối cùng của máy ảnh sẽ được tái sử dụng và được mô tả tương ứng với**lần phơi sáng này** thời phơi sáng: `method`, `retained`, `gap_ms` từ lần phơi sáng này, `in_tolerance` được tính toán lại, và `retained_from_epoch_ns` ghi tên lần phơi sáng mà nó được khớp lần đầu tiên. Một camera chưa bao giờ có giá trị đo vẫn không có giá trị phản xạ, và kết quả sẽ giải thích lý do (`reflectance-skipped-no-fresh-dls` / `reflectance-skipped-bound-daq-unavailable (…)`, hoặc việc từ chối chụp).

Các tệp mà một lần chụp lưu trong thư mục của dự án đang mở trong Chloros sẽ được thêm vào danh sách tệp của dự án đó, giống như khi chụp trong ứng dụng (các tệp được lưu ở nơi khác sẽ không bị ảnh hưởng): `CameraSession.capture()`, `ArraySession.capture()` (cũng như `smart=True`) và `capture_cameras_transient()`. Kết quả được ghi lại trong `.project_add` (`capture_cameras_transient()`: `project_add` của từ điển) -- `held`, `files` và `message` của hệ thống nền; `held` là `True` trong khi quá trình xử lý đang diễn ra, và các tệp sẽ được hợp nhất sau khi quá trình kết thúc. `None` khi không có tệp nào được đưa vào dự án.

`CameraSession.capture()` và `ArraySession.capture()` trả về chúng dưới dạng kết quả là `.daq_files` và `.daq_matches`, các phản hồi ghi lại qua HTTP (`/api/camera/<serial>/cli-capture`, `/api/camera/capture`, `/api/camera/capture-all-synced`, `/api/camera/array/capture`) chứa các giá trị `daq_files` và `daq_matches`, mỗi giá trị tương ứng với một trong các giá trị `.daq`: `daq_file`, `cameras`, `method`, `gap_ms`, `in_tolerance`, `tolerance_ms` và `stream_fault` — lỗi luồng dữ liệu riêng của DAQ, chỉ được kiểm tra khi giá trị đo nằm ngoài giới hạn cho phép. Trước khi dựa vào giá trị phản xạ từ một lần ghi, hãy kiểm tra `in_tolerance` trong `.match.json` hoặc XMP của nó, hoặc các mức [`status()`](#daqsensorsession-methods).

Các mức `processing`:

| Mức | Đầu ra |
| --- | --- |
| `"raw"` | Dữ liệu Bayer một kênh (đối với camera đơn sắc: dải đơn) trực tiếp từ cảm biến. |
| `"debayered"` *(mặc định của SDK)* | Màu 3 kênh: giá trị bù tối (DSNU) từ gói hiệu chuẩn được trừ khỏi dữ liệu thô, sau đó là quá trình khử mô-saic nhận diện cạnh của OpenCV — không có bảng tra tuyến tính (LUT), hiệu chỉnh trường phẳng hoặc tách kênh (máy ảnh đơn sắc: 1 kênh xám). Tệp TIFF được lưu ở thứ tự kênh **RGB**(RGB quang học, do đó bất kỳ trình xem ảnh nào cũng hiển thị màu sắc chính xác); các mảng trong bộ nhớ mà `grab()` / `frame_stream()` trả về ở thứ tự**BGR** — đảo ngược thứ tự các kênh (`frame[..., ::-1]`) để so sánh với tệp đã lưu. `chloros-cli process` ghi ra kết quả giống như `Debayered_Images`. |
| `"radiance"` | float32 W/m²/sr/nm thông qua chuỗi đo quang học đầy đủ. Chỉ dành cho đa phổ — bỏ qua camera RGB. |
| `"reflectance"` | uint16 0..32768 (sẵn sàng cho Pix4D); yêu cầu ghép nối DAQ trực tiếp để tham chiếu tuyệt đối. Chỉ dành cho đa phổ. |
| `"display"` | Chuỗi xử lý đầy đủ khớp với bản xem trước trên giao diện người dùng (CCM + WB + gamma theo cấu hình của camera). |
| `"all"` | **Mỗi camera có một tệp cho mỗi mức áp dụng** (tương ứng với tùy chọn "Capture All" trên GUI / mặc định CLI). Tệp `CaptureResult` trả về sẽ chứa một từ điển khung hình cho mỗi `(cam, level)`, với mức tương ứng trong mỗi từ điển; các mức không áp dụng sẽ xuất hiện trong `.skipped`. Dữ liệu DAQ được sử dụng để ghi lại sẽ được lưu dưới dạng tệp phụ `.daq`. |

> **Lưu ý — giá trị mặc định khác với CLI.** `ArraySession.capture()` mặc định là `processing="debayered"`; lệnh `chloros-cli lattice array-capture` mặc định là `processing="all"`. Chuyển `processing="all"` một cách rõ ràng từ SDK để phản ánh tính năng lưu nhiều cấp độ của CLI/GUI.

##### Tên tệp ghi lại mảng

Mỗi cấp độ được lưu trong một thư mục con riêng của `output_dir` (`raw/`, `debayered/`, `radiance/`, `reflectance/`, `preview/` cho `display`, `index/`, `composite/`). Mỗi tệp đặt tên máy ảnh theo định dạng `SN<serial>-<filter>`: số sê-ri, sau đó là phần tên mã của mẫu máy ảnh — `SN214000533-FOCN` cho `M3C-L87-FOCN`, `SN261902142-F450` đối với `M3M-L87-F450` — kết hợp với `-`, vì vậy tên tệp có các trường được phân tách bằng `_` giống như trước đây. Một camera không thể đọc được mẫu mã chỉ có tên là `SN<serial>`, và các bản ghi từ các phiên bản trước không có bộ lọc trong tên. Tên tệp phụ thuộc vào số cấp độ mà thành viên được yêu cầu:

| Thành viên được yêu cầu | Tên tệp | Ví dụ (`reflectance`, mặc định là `format="tiff"`) |
| --- | --- | --- |
| **Một cấp độ** (`processing="reflectance"`, hoặc `levels` chỉ có một mục) | `sync_<timestamp>_SN<serial>-<filter>.<format>` — không có hậu tố cấp độ, giống như trong các phiên bản trước, do đó các tập lệnh hiện có vẫn hoạt động | `reflectance/sync_<ts>_SN214000533-FOCN.tiff` |
| **Nhiều cấp độ** (`processing="all"`, hoặc `levels` với nhiều hơn một mục) | `sync_<timestamp>_SN<serial>-<filter>_<level>.<format>` | `reflectance/sync_<ts>_SN214000533-FOCN_reflectance.tiff` |

[Mặt nạ giới hạn](#unmix-clamp-mask--which-pixels-a-band-was-clamped-on) đi kèm với tên sản phẩm, `clamp_masks/<product folder>/<product name>_clampmask.tif`: việc ghi lại độ phản xạghi lại độ phản xạ một cấp sẽ tạo ra `clamp_masks/reflectance/sync_<ts>_SN<serial>-<filter>_clampmask.tif`, trong khi `processing="all"` sẽ tạo ra `clamp_masks/reflectance/sync_<ts>_SN<serial>-<filter>_reflectance_clampmask.tif`. Quy tắc hậu tố được xác định cho từng thành viên dựa trên các mức được yêu cầu, do đó một mức bị bỏ qua (ví dụ: độ phản xạ trên camera RGB) sẽ không làm thay đổi nó. Đọc các đường dẫn từ các khung hình được trả về thay vì tự xây dựng chúng. Việc ghi lại bằng một camera duy nhất (`CameraSession.capture()`) luôn thêm cấp độ: `capture_<ts>_SN<serial>-<filter>_<level>.tiff`.

##### Chỉ chấp nhận các tệp PNG/JPEG có thông tin hiển thị và chỉ mục

PNG hoặc JPEG chứa 8 bit trên mỗi kênh. Yêu cầu một định dạng có mức độ mà nó không thể chứa sẽ bị từ chối trước khi bất kỳ dữ liệu nào được ghi lại, kèm theo thông báo nêu rõ mức độ đó và TIFF là định dạng có thể chứa mức độ đó:

- **`radiance`**(float32) và**`reflectance`**(uint16, `32768` = 1.0) — và do đó là `processing="all"`, bao gồm cả hai mức này. SDK sẽ kiểm tra điều này trên các đối số: `capture()` sẽ gây ra lỗi**`ValueError`** mà không có yêu cầu (`CameraSession`, `ArraySession`, `CameraHandle`, `ArrayHandle`, `capture_smart`).
- **`raw` / `debayered` từ camera truyền phát hơn 8 bit** (12-bit M3C, Mono12 M3M) — hệ thống phía sau từ chối các yêu cầu này, do đã biết độ sâu bit của từng camera: Lỗi 400 HTTP với `error_code` `FORMAT_CANNOT_HOLD_LEVEL`, được nâng lên thành `ChlorosConnectError` (`CameraSession` / `ArraySession`) hoặc `RuntimeError` (`CameraHandle` / `ArrayHandle`). Việc chụp ảnh một cấp độ `debayered` từ một camera sẽ thu được khung hình 8-bit cho định dạng PNG/JPEG và không bị từ chối.

Lưu dữ liệu phóng xạ dưới dạng `format="tiff"` (`ext=".tiff"`); sử dụng định dạng PNG/JPEG cho `display` và các bản render chỉ mục.

#### Cách thức hoạt động của tính năng Phơi sáng Tự động

- **Phơi sáng trước, độ khuếch đại sau.** Khi độ khuếch đại ở chế độ Tự động, độ khuếch đại sẽ duy trì ở mức 0 dB cho đến khi phơi sáng đạt đến giá trị tối đa được phép.
- **Khi điều chỉnh phơi sáng thủ công, độ lợi vẫn ở chế độ Tự động** cho đến khi bạn thay đổi độ lợi hoặc chuyển sang chế độ thủ công. Khi quay lại chế độ Tự động, giới hạn độ lợi sẽ trở về mức ban đầu.
- **Các vùng sáng nhỏ không làm tối cảnh.** Vùng phản xạ lên đến 1% khung hình sẽ bị bỏ qua, và trên các tấm của mục tiêu hiệu chuẩn đã biết, lên đến 0,5% diện tích tấm.
- **Giới hạn phơi sáng DAQ chỉ áp dụng khi mục tiêu hiệu chuẩn nằm trong khung hình.** Nó ngăn không cho tấm nền trắng bị cắt sáng; khi không có tấm nền nào trong khung hình, máy ảnh sẽ phơi sáng theo cảnh.
- **Ánh sáng ổn định sẽ giữ nguyên, và ánh sáng thay đổi sẽ ổn định lại trong vài giây.** Khi ánh sáng ổn định, mức phơi sáng và giới hạn của nó sẽ không thay đổi, và sau khi có sự thay đổi lớn về ánh sáng, khung hình sẽ ổn định lại trong vài giây. Hiện tượng cắt sáng không còn giới hạn phơi sáng vĩnh viễn, và khi vùng quan tâm (ROI) thay đổi, máy ảnh sẽ tự động điều chỉnh lại mức phơi sáng.
- **Mục tiêu độ sáng luôn nằm trong phạm vi kiểm soát.** Mức độ sáng không tăng lên khi máy ảnh đã đạt giới hạn phơi sáng và độ khuếch đại không thể tăng thêm, do đó ánh sáng quay trở lại sẽ không gây lóa khung hình.
- **Một camera có chế độ phơi sáng tự động ngừng phản hồi sẽ được giữ ở mức phù hợp với điều kiện ánh sáng hiện tại.** Dưới ánh nắng chói chang, camera có thể giảm xuống mức phơi sáng ngắn nhất và giữ nguyên ở đó với khung hình tối, trong khi các thông số vẫn hiển thị bình thường. Chloros theo dõi khung hình, giữ camera ở mức đã thiết lập cho điều kiện ánh sáng đó, và thả ra khi ánh sáng thay đổi. Cảm biến ánh sáng bị che khuất hoặc tối đi sẽ không ảnh hưởng đến quá trình này.

#### Phơi sáng trên các thành viên mảng

Một máy ảnh LATTICE giới hạn `ExposureTime` ở một chu kỳ của tốc độ khung hình. Trong khi mảng truyền dữ liệu, Chloros chạy mọi thành viên với `AcquisitionFrameRateEnable` bật, với tốc độ chỉ cao hơn một chút so với tốc độ kích hoạt, do đó giới hạn phơi sáng của một thành viên sẽ nằm hơi dưới chu kỳ kích hoạt, **tại bất kỳ vùng quan tâm (ROI) nào**:

- Chu kỳ khung hình của một thành viên là chu kỳ kích hoạt trừ đi độ trễ 15% của chu kỳ (tối thiểu 3 ms, tối đa bằng một nửa chu kỳ), và giới hạn phơi sáng của nó là chu kỳ khung hình đó trừ đi khoảng 0,1 ms. Ví dụ: một ngưỡng kích hoạt 10 fps (100 ms) cho tốc độ thành viên khoảng 11,8 fps và giới hạn phơi sáng khoảng 85 ms.
- **Nếu thời gian phơi sáng thủ công `ExposureTime` dài hơn giới hạn đó, tần số kích hoạt sẽ được điều chỉnh chậm lại để phù hợp.** Chloros trước tiên mở giới hạn phơi sáng của thành viên đó cho giá trị đó và cho phép tốc độ chậm hơn chạy trong vài chu kỳ kích hoạt (việc ghi có thể mất thêm vài giây), sau đó ghi giá trị đó, và tốc độ kích hoạt sẽ không nhanh hơn mức mà thời gian phơi sáng cho phép. Trên mảng tốc độ tự động, tốc độ hoạt động trước đó sẽ được khôi phục khi mọi thành viên có thời gian phơi sáng thủ công làm chậm hệ thống đều quay trở lại chế độ phơi sáng tự động. Một giá trị mà chính máy ảnh không thể ghi vẫn bị từ chối, với phạm vi chấp nhận hiện tại nằm trong vùng lỗi, và chế độ phơi sáng tự động của thành viên sẽ được khôi phục nếu nó chỉ bị tắt tạm thời cho lần ghi đó. Trước phiên bản 1.3.0, một vùng ROI nhỏ giới hạn mỗi thành viên ở chu kỳ chạy tự do của cảm biến, bất kể tốc độ kích hoạt là bao nhiêu: 4,9 ms ở độ phân giải 768 × 304, do đó không thành viên nào có thể được làm sáng hơn.
- Chloros không bao giờ rút ngắn thời gian phơi sáng thủ công để phù hợp với tốc độ kích hoạt nhanh hơn. Tốc độ tự động luôn nằm trong giới hạn cho phép của các lần phơi sáng thủ công; nếu tốc độ cố định nhanh hơn giới hạn phơi sáng, thành viên đó sẽ được áp dụng giới hạn thời gian đủ dài cho nó, và nó chỉ nhận các tín hiệu kích hoạt không nhanh hơn giới hạn phơi sáng cho phép. Hãy giảm thời gian phơi sáng hoặc tốc độ cố định khi mọi thành viên phải xử lý mọi tín hiệu kích hoạt.
- **Đọc lại:** việc đọc các nút của thành viên cho thấy `AcquisitionFrameRateEnable` True và `AcquisitionFrameRate` ở tốc độ của thành viên đó; tín hiệu kích hoạt sẽ tắt lại khi mảng dừng lại.
- **Mảng sở hữu hai nút đó. Không được ghi `AcquisitionFrameRateEnable` hoặc `AcquisitionFrameRate` lên một thành viên của mảng** — thay vào đó, hãy đặt tốc độ của mảngbằng `target_fps` / `set_target_fps()` thay thế.

Ghi mức phơi sáng của một thành viên bằng `arr.set_member_settings(serial, exposure_time=20000)` (các từ khóa của `CameraSession.set_settings()`; `chloros-cli lattice exposure --serial` cũng thực hiện tương tự). Thành viên vẫn ở trong mảng. Không mở một thành viên mảng bằng `connect_camera()`: connect sẽ từ chối camera đã có trong mảng, và lệnh `CameraSession` sẽ loại bỏ camera khỏi mảng khi ngắt kết nối. `ArraySession.status()["member_ae"]` báo cáo thông số phơi sáng của từng thành viênthông số phơi sáng và độ khuếch đại từ khung hình mới nhất của nó.

### Một bản ghi dự án, mọi chế độ

Một thư mục dự án chứa các cài đặt của mọi thiết bị: `cameras.json` — của từng camera (`cameras[].settings`, dành cho một camera đơn lẻ và cho từng thành viên trong mảng) và các thiết lập chung cho toàn bộ mảng (`arrays[].capture`: định dạng, vùng quan tâm (ROI), gộp pixel, tần suất kích hoạt, ngân sách dây) — cùng với `sensors.json`, các thiết lập của từng cảm biến ánh sáng (thời gian tích hợp, trung bình khung hình, giới hạn). Ứng dụng, giao diện dòng lệnh (CLI), bộ công cụ phát triển phần mềm (SDK) và máy chủ MCP đều đọc và ghi cùng các tệp này theo cùng một cách:

- **Một thiết bị được kết nối trong một dự án sẽ được lưu vào dự án đó và ghi lại mọi thay đổi vào đó.** "Trong một dự án" có nghĩa là khi dự án đang mở ở phía máy chủ (của ứng dụng), hoặc — thông qua một tệp `ChlorosProject` (`open_project()`), tệp này chỉ định thư mục riêng của nó cho phía máy chủ (`project_path`). Một camera, mảng cảm biến hoặc cảm biến ánh sáng mà dự án chưa có sẽ được thêm vào các tệp của dự án khi thiết bị đó kết nối (`connect_camera()`, `connect_array()`, kết nối cảm biến ánh sáng, CLI, ứng dụng, `chloros-mcp`), kèm theo các cài đặt của nó. Mọi thay đổi từ bất kỳ chế độ nào (`set_settings()`, `ArraySession.set_member_settings()`, `set_target_fps()`, cài đặt mảng, `set_cap()`, CLI, các điều khiển của ứng dụng) sẽ được lưu vào các tệp của dự án đó ngay sau đó. Cài đặt phơi sáng, độ khuếch đại hoặc mục tiêu AE trên máy ảnh sẽ được lưu dưới dạng đã giữ, do đó khi mở lại, các cài đặt này sẽ được giữ nguyên.
- **Việc mở một dự án sẽ thiết lập các thiết bị từ dự án đó**, trong mọi chế độ: một máy ảnh, mảng cảm biến hoặc cảm biến ánh sáng được kết nối mà không có cài đặt riêng sẽ nhận các cài đặtđã lưu của dự án (`lattice cam-connect` / `array-connect` khi dự án đang mở, `chloros-cli project connect`, `open_project().connect_all()`, khi mở lại ứng dụng). Một mảng được kết nối dưới một ID mới sẽ cập nhật mảng mà dự án đã lưu với cùng các camera; một mảng có các camera khác sẽ được thêm vào bên cạnh nó.
- **Việc lưu của chính ứng dụng không bao giờ ghi đè lên các cài đặt trực tiếp của thiết bị** bằng bản sao cũ hơn của chúng, và giữ nguyên các tùy chỉnh riêng cho từng thành viên mà người gọi CLI hoặc SDK đã lưu.
- **Định dạng pixel mà bạn thiết lập chính là định dạng mà dự án lưu giữ.** Định dạng được thiết lập trong Cài đặt Mảng, bằng `--pixel-format` hoặc `connect_array(pixel_format=)` (bao gồm cả 8-bit), sẽ được lưu lại và kết nối lại chính xác. Nếu không được thiết lập, mỗi camera sẽ kết nối ở định dạng mặc định 12-bit (`BayerRG12`, `Mono12` trên cảm biến đơn sắc). Tên định dạng chỉ định độ sâu bit của nó; gia đình là của cảm biến, do đó máy ảnh màu không bao giờ được đặt thành định dạng đơn sắc hoặc máy ảnh đơn sắc thành định dạng Bayer, và dòng `[PixelFormat <serial>] ... written` sẽ chỉ ra điều này khi định dạng được ánh xạ. Các tệp xuất từ bản chụp 8-bit là 8-bit, bao gồm cả phản xạ: được điều chỉnh tỷ lệ và đóng dấu bằng `PixelScale`.
- Một thiết bị được kết nối mà không thuộc dự án nào sẽ không ghi lại ghi lại gì cả.
- **Một bản ghi sẽ thuộc về dự án được tạo sau nó; khi mở lại, nó sẽ giữ nguyên dự án ban đầu.**Khi một dự án được tạo — `create_project()`, `chloros-mcp`'s `create_project`, hoặc tính năng**Dự án mới**của ứng dụng — hoặc được sao chép (tính năng**Sao chép**của ứng dụng, mở bản sao) trong khi bản ghi vẫn đang ghi vào dự án đã mở trước đó, thì bản ghi sẽ tiếp tục, và những gì nó ghi sẽ được lưu vào dự án mới, tại cùng vị trí bên trong dự án đó (bên dưới). Mở một dự án hiện có — `ChlorosProject.process()` (điều này sẽ mở dự án đó ở phía máy chủ), `chloros-cli process`, tính năng**Mở Dự án** của ứng dụng — sẽ để quá trình ghi hình vẫn tiếp tục với dự án mà nó đang ghi vào: quá trình ghi hình sẽ kết thúc tại đó, và các tệp của nó sẽ được thêm vào danh sách tệp của dự án đó (ứng dụng sẽ dừng mọi quá trình ghi hình trước khi mở một dự án).
  - `ArraySession.record()` và `ArraySession.burst()`: video, hoặc thư mục chứa từng khung hình, sẽ được di chuyển nguyên vẹn đến đó khi quá trình ghi hình dừng lại. Kết quả khi dừng ghi sẽ báo cáo đường dẫn mới (`out_dir`, `video_path`), và quá trình tạo bản dựng từ loạt ảnh sẽ được thực hiện từ đó. Nếu ổ đĩa từ chối di chuyển, bản ghi sẽ vẫn ở vị trí ban đầu, và kết quả sẽ ghi nhận điều này (`relocation_error`). Các khung hình thô của loạt ảnh đã dừng và tệp `.daq` sẽ được thêm vào danh sách tệp của dự án mà chúng thuộc về; video sẽ không được thêm vào danh sách tệp của dự án.
  - `ArraySession.continuous()`: mọi bản ghi sau khi thay đổi sẽ được lưu vào dự án mới; những bản ghi được lưu trước đó vẫn giữ nguyên vị trí.
  - `DAQSensorSession.record_start()` (và bản ghi từ cảm biến ánh sáng của chính ứng dụng): tệp `.daq` sẽ được di chuyển vào thư mục `light_sensor` của dự án mới khi quá trình ghi hình kết thúc; `record_stop()` trả về đường dẫn mới, và tệp đó sẽ được thêm vào danh sách tệp của dự án đó.

  Việc ghi vào một thư mục bên ngoài dự án đang mở sẽ giữ nguyên vị trí hiện tại, và các tác vụ `start_camera_continuous()` / đa luồng sẽ giữ nguyên thư mục đã được chỉ định.
- **Trong ứng dụng, các bản ghi sau khi tạo dự án mới sẽ được chuyển vào dự án đó.** Việc tạo hoặc sao chép một dự án sẽ giữ nguyên tất cả các phiên ghi đang chạy và quá trình ghi tiếp tục diễn ra; mỗi bản ghi được thực hiện sau đó (Capture All, Start Capturing) sẽ được lưu vào dự án mới và được thêm vào danh sách tệp của dự án đó. Việc mở một dự án hiện có trong ứng dụng sẽ tạm dừng tất cả các tác vụ đó trước tiên.
- **Dự án sẽ không bị di chuyển trong khi đang ghi.**Việc di chuyển dự án đang mở sang một thư mục làm việc khác (trong phần**Cài đặt → Thư mục làm việc** của ứng dụng; không có lệnh SDK nào di chuyển dự án, mà chỉ có client HTTP gửi yêu cầu đến `/api/move-project` mới thực hiện được) sẽ bị từ chối với mã trạng thái HTTP 409 `PROJECT_RECORDING` khi có hoạt động ghi mảng, chụp liên tục mảng, chụp liên tục/đa luồng từ camera hoặc ghi dữ liệu cảm biến ánh sáng đang ghi vào dự án đó: `Project '<name>' is recording (<what>), so moving the project now is refused. Stop the recording, then try again.`

Điều này khác với trạng thái còn lại của các camera khi ngắt kết nối (xem [Bộ nhớ camera](#camera-memory--keep_settings--clear_settings)): một dự án tự thiết lập các camera của mình khi được mở.

### Bộ nhớ camera — `keep_settings` / `clear_settings`

Khi một mảng được kết nối, các camera trong mảng đó sẽ áp dụng cài đặt của mảng, và sau đó bạn có thể thay đổi các cài đặt này. Khi Chloros ngắt kết nối các camera — `ArraySession.disconnect()` / `force_disconnect()`, khi kết thúc một hàm tạm thời, hoặc khi dừng `start_camera_continuous()` — hệ thống sẽ khôi phục từng camera về trạng thái ban đầu trước khi kết nối: kết nối tín hiệu kích hoạt và đường đồng bộ, cài đặt PTP và cài đặt camera — `ExposureAuto` / `GainAuto` (và chế độ thủ công `ExposureTime` / `Gain`), `TargetBrightness` cùng các giới hạn và độ giảm chấn AE, cân bằng trắng, mức đen, `PixelFormat`, tính năng gộp pixel và vùng quan tâm (ROI), quy trình xử lý màu (`GammaEnable` / `Gamma`, `LUTEnable`, `ColorTransformationEnable`), `ReverseX` / `ReverseY`, `UserSetSelector`, các thành phần PTP là `PtpSlaveOnly` / `PtpDomainNumber`, kích thước gói tin (`GevSCPSPacketSize` / `DeviceStreamChannelPacketSize`), tốc độ (`DeviceLinkThroughputLimitMode` / `DeviceLinkThroughputLimit`, `GevSCPD`, `GevSCFTD`), dữ liệu khối (`ChunkModeActive`, mỗi `ChunkEnable` dưới `ChunkSelector`), loại bỏ nhiễu đầu vào kích hoạt và một mảng cung cấp đường đồng bộ cho thiết bị phụ (`LineFilterSelector` / `LineFilterWidth`) và tốc độ khung hình thu nhận (chỉ áp dụng cho `AcquisitionFrameRate` khi tín hiệu kích hoạt của nó đang bật: khi tắt, tốc độ khung hình sẽ đọc giá trị tối đa mà chế độ điều chỉnh cho phép, không phải là một thiết lập cố định). Chỉ những thông tin khác biệt mới được ghi lại: máy ảnh dừng hoạt động trước tiên, chế độ điều chỉnh và tốc độ khung hình trước khi phơi sáng — những thông số này cần có chu kỳ khung hình tương ứng.

Để sử dụng camera ngoài dự án, hai tham số ghi vào **bộ nhớ riêng** của camera — dữ liệu khởi động của nó, trong bất kỳ phần mềm nào (trên `disconnect()` / `force_disconnect()`, mọi hàm tạm thời, `start_camera_continuous()` và `ChlorosProject.disconnect_all()`; MCP: các tham số tương tự như của `disconnect_device` và các công cụ tạm thời; CLI: `--keep-settings` / `--clear-settings`):

- **`keep_settings=True`** lưu các cài đặt của từng camera vào bộ nhớ của nó theo trạng thái khi kết thúc phiên làm việc (UserSet2, được thiết lập khi khởi động). Các camera vẫn được để chạy tự do (`TriggerMode` `Off`), dây đồng bộ được giữ nguyên như ban đầu. Một camera không có UserSet2 sẽ lưu vào UserSet1; máy tính này ghi nhớ điều này, do đó cơ chế tự phục hồi của mảng Chloros — vốn sẽ đặt lại UserSet1 khi nó lệch khỏi giá trị cơ sở của Chloros — sẽ không can thiệp vào camera đó.
- **`clear_settings=True`** đặt lại bộ nhớ của từng camera về cài đặt gốc: mức cơ sở của Chloros, mức mà tính năng tự phục hồi mảng của nó tích hợp — cài đặt mặc định nhà máy với `TriggerMode`, `Off`, `TriggerSource`, `Line2`, phơi sáng tự động, cân bằng trắng đã hiệu chuẩn của camera RGB được giữ nguyên — trong UserSet1, tạo thành bộ khởi động.
- Không phải cả hai (`ValueError`; hệ thống phía sau trả về mã trạng thái 400). Phản hồi chứa `camera_memory`, tùy theo camera là `{"saved_to": "UserSet2" | "UserSet1" | None, "errors": [...]}` hoặc `{"cleared": bool, "errors": [...]}` — `force_disconnect()` trả về giá trị đó; đối với một tác vụ, nó đi kèm với `stop()`.

Ứng dụng không cung cấp các giá trị này.

**Mọi lối thoát đều khôi phục lại các camera.**Quá trình khôi phục theo trạng thái ban đầu được thực hiện trên mỗi đường thoát: `CameraSession.disconnect()` (camera**X**của ứng dụng),**X** trên một mảng hoặc bất kỳ phần tử nào của mảng (toàn bộ mảng sẽ được giải phóng), đóng ứng dụng (phần backend có tối đa 20 giây để khôi phục mọi camera, mảng và quá trình ghi hình liên tục đang chạy; một hàm tạm thời hoặc lệnh gọi một lần vẫn đang chạy sẽ dừng lại tại điểm an toàn tiếp theo, khôi phục các camera của nó trước tiên, trong tối đa 8 giây, và trả về mã trạng thái HTTP 503 `SHUTTING_DOWN`; một `connect_array()` vẫn đang chạy sẽ dừng lại trước camera tiếp theo và khôi phục các camera đã mở, trong tối đa 10 giây, và gây ra lỗi `ChlorosConnectError` (HTTP 503 `SHUTTING_DOWN`); và một lệnh đến trong quá trình tắt máy sẽ không mở bất kỳ camera nào: HTTP 503 `SHUTTING_DOWN`), và lệnh `chloros-cli` bị ngắt hoặc cửa sổ terminal của nó đóng lại (công việc của nó bị dừng và các camera được trả về trạng thái ban đầu trước tiên). Một backend bị treo hoặc bị ngắt cưỡng bức không thể trả lại bất kỳ thứ gì; việc khởi động lại nguồn sẽ đưa mỗi camera trở về bộ nhớ của nó.**Một dự án tự thiết lập các camera của mình.** Khi một mảng kết nối thông qua một dự án — dự án đang mở của ứng dụng, hoặc một `ChlorosProject`, dự án này tự đặt tên cho mình với backend (`project_path`) — mỗi thành viên sẽ nhận được các cài đặt mà dự án đã lưu cho nó: mức phơi sáng, độ khuếch đại hoặc mục tiêu AE được giữ cố định (được coi là của bạn, do đó chế độ phơi sáng tự động sẽ không can thiệp) và chế độ cân bằng trắng (sửa lỗi Chloros; chế độ cân bằng trắng của chính máy ảnh sẽ bị tắt), bất kể bộ nhớ của máy ảnh đang lưu trữ gì.

### Chế độ chụp & Thiết bị ghi

Bề mặt mảng phản ánh bảng điều khiển chụp trên giao diện người dùng (GUI): các chế độ màn trập Đơn / Liên tục / Khoảng thời gian / Nhanh nhất, cùng với hai thiết bị ghi (video tổng hợp trực tiếp và chụp liên tiếp định dạng thô → xử lý lại ngoại tuyến).

```python
import time, chloros_sdk

with chloros_sdk.connect_array(serials) as arr:
    # Single (default) — one synced group
    arr.capture("out/", processing="reflectance")

    # Fastest — raw + .daq + combined index now, calibrate later
    arr.capture_fastest("flightline/")

    # Interval — one reflectance pass every 2 s, 5 passes (bounded so it ends)
    arr.capture_repeated("timelapse/", count=5, interval_s=2.0,
                         processing="reflectance",
                         on_capture=lambda i, r: print(f"pass {i}: {len(r)} frames"))

    # Continuous — every synced group for 60 s, saved by the backend itself
    with arr.continuous("run1/", processing="raw", duration_s=60) as run:
        run.wait()
    print(run.result["captures"], run.result["captures_per_s"],
          run.result["stop_reason"])

    # Combined-index video/GIF recorder: the GUI's combined view, or with no
    # GUI open the array's NDVI (pass expr="..." for another index)
    with arr.record("monitoring/", fps=10, gif=True) as rec:
        time.sleep(30)
    if rec.result and rec.result.get("error"):   # leaving the block does not raise
        print("recording failed:", rec.result["error"])
    else:
        print(rec.result["video_path"])

    # Raw-Bayer burst → offline reprocess into calibrated video(s)
    with arr.burst("capture/", duration_s=5) as b:
        pass
    out = arr.build_video(b.result["out_dir"], products=[
        {"kind": "per_cam", "level": "reflectance"},
        {"kind": "combined", "level": "index"}])
    print(out["outputs"])
```

- **`capture_repeated`**là vòng lặp Liên tục/Khoảng thời gian của SDK. Vì không có `Ctrl+C` để ngắt vòng lặp này từ kịch bản, bạn**phải** truyền `count` và/hoặc `duration_s` (chu trình sẽ dừng khi đạt đến một trong hai giá trị này). `interval_s` được tính từ đầu mỗi lần lặp (tương ứng với giao diện người dùng). Các lần lặp liên tục (`interval_s=0`) nối tiếp các giá trị `group_seq` → `after_seq`, do đó chúng là các nhóm đồng bộ liên tiếp theo tốc độ của mảng. Các tham số kwargs còn lại được truyền thẳng đến `capture()`.
- **`continuous`**là chế độ ghi liên tục riêng của backend — tương ứng với chế độ Liên tục trên GUI.**Hãy sử dụng nó thay vì `capture_repeated()` khi bạn muốn mọi tín hiệu kích hoạt được lưu trên đĩa với tốc độ khung hình cao:** `capture_repeated()` thực hiện một yêu cầu HTTP cho mỗi nhóm từ skript của bạn, trong khi `continuous()` duy trì hai luồng ghi hình đang xử lý bên trong backend mà không cần trao đổi dữ liệu qua lại giữa các nhóm. Đo được tại ROI 512 trên mảng 9 camera: 73–77 lần ghi/giây, so với mảng xuất khoảng 78. Giữ lại `capture_repeated()` cho việc ghi theo khoảng thời gian (`interval_s`) và cho các lần quét mà bạn kiểm tra từng lần một. Trên Windows, hãy loại trừ hệ thống backend khỏi Microsoft Defender để đạt tốc độ cao (xem [Bước 1](#step-1--install-the-chloros-platform-package)).
- **`record`** là *cấp độ giám sát*: nó ghi lại bản tổng hợp chỉ số kết hợp trực tiếp đã được hiển thị. Khi cửa sổ xem kết hợp trên giao diện người dùng (GUI) đang mở, nó ghi lại giao diện đó như được hiển thị. Nếu không có cửa sổ nào mở, phần nền sẽ tự hiển thị giao diện: `expr` nếu bạn truyền vào một dải, ngược lại là giá trị NDVI của mảng (các dải đỏ và NIR gần nhất với 660 nm và 850 nm, theo cách mà chế độ xem trực tiếp chọn chúng), ngay sau khi mảng được căn chỉnh. Đặt tên các dải trong `expr` theo cách mà mảng đặt tên cho chúng: `<band>_<filter>` cho camera M3C (`NIR_850_RGN`, `Red_660_RGN`, `Orange_615_OCN`), `Mono_<filter>` cho máy ảnh M3M (`Mono_850`); tên màu ngắn (`NIR`, `Red`) chỉ hoạt động khi một dải trong mảng có tên đó. Một cửa sổ GUI được mở trong quá trình ghi sẽ được ghi lại miễn là nó vẫn mở. Một bản ghi không ghi được khung hình nào sẽ báo lỗi với `error` và `"no-frames: …"` (mảng có thể vẫn đang căn chỉnh hoặc ổn định thời gian phơi sáng), và `stop()` sẽ phát sinh lỗi (xem [`RecorderHandle`](#recorderhandle)). Mỗi mảng chỉ cho phép một trình ghi tổng hợp (sẽ phát sinh lỗi nếu đã có một trình ghi đang chạy).
- **`burst` → `build_video`** là *cấp độ phân tích*: `burst` ghi các khung hình thô + tệp manifest cho từng khung hình + một `.daq` cho mỗi lần đọc DLS riêng biệt dưới `<output>/bursts/<base>/` ở tốc độ tối đa của vòng lặp chụp (không có chuỗi, không dùng exiftool, không xem trực tiếp). `build_video` đồng bộ thời gian từng khung hình với `.daq` gần nhất và chạy lại chuỗi xử lý nhập dữ liệu về độ sáng/độ phản xạ/chỉ số. `products` là danh sách các giá trị `{"kind": "per_cam"|"combined", "level": "radiance"|"reflectance"|"index"}` (mặc định: chỉ số kết hợp). `burst().stop()` cũng tự động khởi tạo một chỉ số kết hợp, được trả về dưới dạng `build_job` trong kết quả dừng.

##### Kết quả `build_video`

| Khóa | Ý nghĩa |
| --- | --- |
| `success` | `True` chỉ khi mọi sản phẩm được yêu cầu đều được xây dựng đúng theo yêu cầu: có ít nhất một đầu ra, không có gì trong `skipped`, và không có trong `degraded`. |
| `outputs` | Đường dẫn của các video (và GIF) đã được ghi. |
| `errors` | Các vấn đề, dưới dạng văn bản. |
| `skipped` | Một `{"serial", "level", "reason"}` cho mỗi sản phẩm được yêu cầu mà **không** tạo ra video — do camera (không có gói hiệu chuẩn cho camera đó, không có dữ liệu DLS về độ phản xạ, chuỗi xử lý gặp lỗi) hoặc chỉ số kết hợp, chứa `"kind": "combined"` và `serial` `None` (không có cặp dải đỏ/NIR nào trong số các camera, không có nhóm nào có dữ liệu radiance cho cả hai dải). Bất kỳ mục nào cũng sẽ tạo ra `success` và `False`; các video còn lại vẫn được ghi và liệt kê. |
| `substitutions` | Các camera được hiển thị ở mức độ khác so với yêu cầu, và lý do — một camera RGB được yêu cầu cung cấp dữ liệu độ sáng/độ phản xạ sẽ nhận được video hiển thị, đã được cân bằng trắng khi gói hiệu chuẩn của camera có sẵn. Đây không phải là lỗi. |
| `notes` | Văn bản thông tin, không phải lỗi — ví dụ: một camera không có biểu thức chỉ số riêng cho từng camera sẽ không có video chỉ số, hoặc video hiển thị của camera RGB không được cân bằng trắng (không có gói hiệu chuẩn, hoặc gói hiệu chuẩn không có cân bằng trắng mặc định), kèm theo tên camera. Không phải là lỗi. |
| `degraded` | `True` khi một sản phẩm đã được tạo nhưng không phải là sản phẩm được yêu cầu: chỉ số kết hợp ngoại tuyến là sự khác biệt chuẩn hóa đỏ/NIR tích hợp sẵn, do đó biểu thức chỉ số đã cấu hình không được đánh giá. Gây ra `success` `False`. |
| `combined` | Tóm tắt về chỉ số kết hợp: các dải được sử dụng, phạm vi LUT, các khung hình được tạo. |
| `clamp_masks` | Đường dẫn của các [mặt nạ kẹp tách kênh](#unmix-clamp-mask--which-pixels-a-band-was-clamped-on) của các tệp TIFFTIFF được hiệu chỉnh từng khung hình (`save_tiffs=True`), trong thư mục `clamp_masks/<level>/` bên cạnh thư mục `<level>/` của các tệp TIFF, mỗi tệp TIFF có một tệp tương ứng cho các pixel bị giới hạn. Một video không chứa bất kỳ tệp nào. |
| `clamp_fractions` | Thông tin mà video cung cấp thay cho mặt nạ: một tệp `{"kind", "serial", "level", "frames"}` cho mỗi sản phẩm video (`kind` `"combined"` dành cho các camera thành viên của chỉ mục kết hợp), `frames` là danh sách các `[frame, R, G, B]` — tỷ lệ phần trăm pixel của khung hình đó mà quá trình tách màu đã giới hạn trong dải màu mà kênh R / G / B của nó chứa (`Chloros:Band{R,G,B}UnmixClampFraction`). |

#### `RecorderHandle`

Được trả về bởi `ArraySession.record()` và `ArraySession.burst()`. Sử dụng nó như một trình quản lý bối cảnh để tự động dừng khi thoát khỏi phạm vi, hoặc điều khiển thủ công.

| Thành phần | Mô tả |
| --- | --- |
| `job_id` | ID tác vụ backend (chuỗi). |
| `kind` | `"composite"` (từ `record`) hoặc `"raw"` (từ `burst`). |
| `start_stats` | Từ điển được trả về bởi lệnh gọi `start`. |
| `result` | `None` trong khi đang chạy; đối tượng từ điển kết quả dừng cuối cùng sau khi dừng. |
| `stats(timeout=10.0)` | Thống kê công việc thời gian thực (số khung hình đã ghi, fps thực tế, thời gian đã trôi qua). |
| `stop(timeout=60.0)` | Dừng trình ghi; trả về và lưu vào bộ nhớ đệm kết quả cuối cùng (`.result` vẫn giữ nguyên giá trị `None` nếu chính yêu cầu dừng bị lỗi). Đối với một bản ghi `record()` bị lỗi — tức là bản ghi không ghi được khung hình nào (`error` `"no-frames: …"`) — hệ thống sẽ lưu kết quả vào bộ nhớ đệm tại `.result`, sau đó kích hoạt ngoại lệ tại `ChlorosConnectError`. Tính idempotent (lần gọi thứ hai sẽ trả về kết quả đã lưu trong bộ nhớ đệm, mà không kích hoạt ngoại lệ). Việc để lại khối `with` sẽ dừng trình ghi mà không gây ra lỗi, vì vậy hãy kiểm tra `rec.result.get("error")` sau đó. |

```python
rec = arr.burst("capture/")
# ... drive manually ...
print(rec.stats()["frames"])
result = rec.stop()
print(result["out_dir"], result.get("build_job"))
```

#### `ContinuousHandle`

Được trả về bởi `ArraySession.continuous()`. Sử dụng nó như một trình quản lý bối cảnh — việc thoát khỏi khối `with` sẽ dừng quá trình ghi nếu nó vẫn đang chạy — hoặc điều khiển nó bằng `status()` / `wait()` / `stop()`. Mỗi mảng chỉ chạy một quá trình thu thập liên tục tại một thời điểm.

| Thành phần | Mô tả |
| --- | --- |
| `start_status` | Từ điển trạng thái được trả về bởi lệnh bắt đầu. |
| `result` | `None` trong quá trình chạy; từ điển trạng thái cuối cùng sau khi quá trình thu thập kết thúc (được thiết lập bởi `stop()`, hoặc bởi một lệnh `status()` / `wait()` đã phát hiện việc kết thúc). |
| `status(timeout=10.0)` | Đối tượng trạng thái đang hoạt động (các trường bên dưới). |
| `wait(timeout=None, poll_s=0.5)` | Chờ cho đến khi quá trình ghi dữ liệu tự động kết thúc — do đạt đến giá trị `max_captures` hoặc `duration_s`, mảng dữ liệu hết, ổ đĩa đầy, hoặc xảy ra lỗi — và trả về trạng thái cuối cùng. Với `timeout`, trả về trạng thái mới nhất sau số giây đó ngay cả khi quá trình vẫn đang chạy: kiểm tra `running`. |
| `stop(timeout=30.0)` | Dừng quá trình ghi lại và trả về trạng thái cuối cùng của nó. Tính idempotent (lần gọi thứ hai sẽ trả về kết quả đã được lưu trong bộ nhớ đệm). |

Các trường trạng thái:

| Trường | Ý nghĩa |
| --- | --- |
| `running` | True trong khi quá trình thu thập đang chạy. |
| `captures` | Các nhóm đã đồng bộ hóa được lưu. |
| `files` | Các tệp đã được ghi. |
| `failed_files` | Các tệp không thể ghi (như trong [`CaptureResult.failed`](#files-that-could-not-be-written--captureresultfailed)). |
| `failed_captures` | Các lần thử ghi lại thất bại hoàn toàn, bao gồm cả những lần đã thử lại (lỗi HTTP 503 khi mảng không có nhóm mới kịp thời). |
| `skipped_groups` | Các nhóm mà mảng đã xuất bản nhưng chưa được lưu (quá trình thu thập dữ liệu bị chậm trễ). |
| `captures_per_s` | Tỷ lệ lưu trung bình kể từ khi bắt đầu. |
| `elapsed_s` / `started_at` | Số giây đã chạy cho đến nay; thời gian bắt đầu (giây theo hệ thống Unix epoch). |
| `last_group_seq` | Nhóm được đồng bộ hóa mới nhất đã được lưu. |
| `last_error` | Lỗi mới nhất, hoặc `None`. |
| `stop_reason` | Khi kết thúc: `count` (đã đạt đến `max_captures`), `duration` (đã đạt đến `duration_s`), `stopped` (đã gọi đến `stop()`), `array_gone` (mảng đã bị ngắt kết nối), `error` (20 lần thất bại liên tiếp, hoặc yêu cầu bị hệ thống phía sau từ chối), `disk_full` (tệp mà ổ đĩa không còn dung lượng để chứa). `None` trong khi đang chạy. |
| `max_captures` / `duration_s` | Giới hạn ban đầu khi khởi chạy (`0` = không có). |
| `defender_cores` | Số lõi CPU mà Microsoft Defender đã sử dụng trong khoảng thời gian 10 giây gần nhất; `None` cho đến khi có giá trị được đo lường, và luôn là `None` trên máy chủ không cài đặt Defender. |
| `defender_warned` | Đúng khi bản ghi đã ghi nhận rằng Defender đang làm chậm quá trình này (tại 1 lõi trở lên). Loại trừ phần backend khỏi quá trình quét của Defender — xem [Bước 1](#step-1--install-the-chloros-platform-package). |

```python
run = arr.continuous("captures/run1", processing="raw", max_captures=5000)
try:
    while True:
        st = run.wait(timeout=5)              # returns every 5 s while running
        print(f"{st['captures']} captures, {st['captures_per_s']:.1f}/s, "
              f"{st['skipped_groups']} skipped")
        if not st["running"]:
            break
finally:
    final = run.stop()                        # no-op once it has ended
if final["failed_files"] or final["stop_reason"] in ("disk_full", "error"):
    raise SystemExit(f"continuous capture: {final['stop_reason']}: "
                     f"{final['last_error']}")
```

### Khung hình trực tiếp trong quy trình xử lý của riêng bạn

Để đưa các khung hình đồng bộ vào mã của riêng bạn — học máy, trình xem tùy chỉnh, trình ghi hình của riêng bạn — hãy sử dụng `ArraySession.frame_stream()`. Mỗi lần tick sẽ tạo ra một `GrabResult`: `{serial: numpy array}` cho mỗi camera, **tất cả từ một tín hiệu kích hoạt**, với ID của tín hiệu kích hoạt đó được ghi trên `.group_id`. Đây là cách triển khai tương tự như `ArrayHandle.frame_stream()` trong API của dự án, vì vậy mọi nội dung trong [Một tín hiệu kích hoạt cho mỗi lần đọc](#one-trigger-per-read--group_id), [`band`](#exporting-a-single-band--band), [`align`](#aligning-the-streams--align) và [`bit_depth`](#bit_depth--trading-precision-for-bandwidth) đều áp dụng cho cả hai.

```python
import chloros_sdk
from chloros_sdk import FrameFetchError

serials = ["213800234", "214000533", "214701288", "214701292"]   # master first

with chloros_sdk.connect_array(serials) as arr:
    try:
        for frames in arr.frame_stream(processing="raw", fps=10, duration=300,
                                       with_metadata=True):
            if not frames:              # no camera answered this tick
                continue
            if frames.errors:           # cameras missing from this trigger
                print("left out:", {s: str(e) for s, e in frames.errors.items()})
            for serial, (img, hdr) in frames.items():
                # img: numpy array (raw: one Bayer or mono plane)
                # hdr: group_id, frame_id, timestamp_ns, exposure_us, gain_db, ...
                my_pipeline(frames.group_id, serial, img, hdr["timestamp_ns"])
    except FrameFetchError as exc:      # the stream cannot continue
        print("stream ended:", exc.status, exc)
```

- **Mỗi tập hợp được trả về đều là một tín hiệu kích hoạt mới.** `frames.group_id` không bao giờ lặp lại trong cùng một luồng; khi bạn yêu cầu nhiều `fps` hơn tốc độ chạy của mảng, luồng sẽ chờ tín hiệu kích hoạt tiếp theo, do đó nó sẽ chạy ở tốc độ chậm hơn trong hai giá trị đó.
- **Các camera bị thiếu sẽ được liệt kê tên, không được điền bổ sung.** Một camera không thể được phục vụ tại tín hiệu kích hoạt đó sẽ vắng mặt trong từ điển và được liệt kê trong `frames.errors`.
- **Nó kết thúc một cách ồn ào.** `FrameFetchError` khi bị từ chối, lần kích hoạt tiếp theo sẽ lặp lại, sau 60 giây không có khung hình hoặc sau 60 giây không có tín hiệu kích hoạt mới.**Kết quả mang lại.**`frame_stream()` truyền các khung hình numpy nguyên vẹn qua kết nối HTTP cục bộ và đạt tốc độ tối đa gần**170 MB/s**. Tốc độ này đủ để xử lý trực tiếp các khung hình thô, hoặc mảng đã được gộp (binned) hoặc vùng quan tâm (ROI), nhưng không đủ để xử lý mọi tín hiệu kích hoạt ở độ phân giải đầy đủ — để lưu lại**mọi** tín hiệu kích hoạt, hãy lưu vào đĩa bằng [`continuous()`](#continuoushandle). Đo trên mảng bốn camera (FOCN, FRGN, FRGB, F450) với truyền tải đóng gói; mỗi tick là một bộ kích hoạt hoàn chỉnh:

| | Cảm biến đầy đủ (mảng 14,2–14,4 khung hình/giây) | Gộp 2 × 2 (mảng ~33 khung hình/giây) |
| --- | --- | --- |
| `frame_stream(processing="raw")` | 6,7 bộ/giây | 22,2 bộ/giây |
| `frame_stream(processing="debayered")` | 2,3 bộ/giây | 9,3 bộ/giây |
| `frame_stream(processing="display")` | 0,8 bộ/giây | 3,0 bộ/giây |
| MJPEG cho từng camera, 4 luồng ở độ phân giải 720p | 13,5 khung hình/giây mỗi luồng; mỗi khung hình được khớp với tín hiệu kích hoạt | ~25 khung hình/giây mỗi luồng; ~45% tín hiệu kích hoạt có mặt trên cả 4 luồng |

Sử dụng `processing="raw"` để tối ưu thông lượng và xử lý demosaic tại phía bạn; mức `debayered` cũng loại bỏ độ lệch tối của gói hiệu chuẩn, trong khi quá trình khử mô-đun của `raw` thì không.

#### Luồng MJPEG cho từng camera (không hỗ trợ RTSP)

**RTSP không được hỗ trợ.** Giao diện truyền phát là MJPEG qua HTTP (`multipart/x-mixed-replace`, một tệp JPEG cho mỗi phần, mỗi phần có một `Content-Length`), một URL cho mỗi camera:

| URL | Nội dung được phục vụ |
| --- | --- |
| `GET /api/camera/stream/<serial>` | Xem trước 8-bit, tối đa ~30 khung hình/giây. Đối với thành viên mảng, `max_h` (mặc định 720, 64–2160) và `jpeg_q` (mặc định 80) sẽ thiết lập kích thước và chất lượng. |
| `GET /api/camera/<serial>/stream-annotated` | Cùng bản xem trước nhưng có các lớp phủ giao diện người dùng (zebra, lưới, thước ngắm, biểu đồ histogram, peaking, máy đo điểm). `ChlorosLocal.camera_stream()` sẽ đọc thông tin này cho bạn. |
| `GET /api/camera/<serial>/stream-reflectance` | Độ phản xạ 8-bit trên mỗi dải — xem [Dòng dữ liệu độ phản xạ trực tiếp](#live-reflectance-stream-mjpeg). |

- **Tiêu đề kích hoạt.** Đối với một phần tử mảng, mọi phần của cả ba luồng đều chứa `X-Chloros-Group-Id`, `X-Chloros-Frame-Id` và `X-Chloros-Timestamp-Ns`. Các phần từ các camera khác nhau có cùng `X-Chloros-Group-Id` là cùng một tín hiệu kích hoạt: thu thập các phần và nhóm chúng theo tiêu đề đó để ghép lại thành một bộ. Một camera độc lập không có nhóm: các phần xem trước của nó không chứa bất kỳ tiêu đề nào trong số này (các bộ phận phản xạ của nó mang tiêu đề `X-Chloros-Group-Id` trống).
- **Dòng dữ liệu xem trước sẽ bỏ qua các khung hình khi tải nặng.** Nó sẽ truyền khung hình mới nhất và bỏ qua bất kỳ khung hình nào không thể gửi kịp thời, do đó ở tốc độ mảng cao, không phải mọi tín hiệu kích hoạt đều đến được dòng dữ liệu của mọi camera (xem bảng trên). Đối với mỗi tín hiệu kích hoạt, hãy sử dụng `frame_stream()` hoặc `continuous()`.
- **`/api/camera/stream/<serial>` tạm dừng khi luồng trực tiếp kết hợp của mảng đang mở** (`/api/camera/array/stream/<array_id>` — chế độ xem mảng trên GUI sẽ mở luồng này). Sau đó, luồng của mỗi thành viên sẽ gửi một khung hình giữ chỗ “Tạm dừng (luồng mảng đang hoạt động)”, không có tiêu đề `X-Chloros-*`, và không gửi thêm gì nữa cho đến khi luồng kết hợp đóng lại; sau đó nó sẽ tự động tiếp tục. `stream-annotated` và `stream-reflectance` không tạm dừng. Đóng cửa sổ xem mảng trên GUI khi một skript đọc các luồng xem trước của từng camera.
- **Đối với số liệu (không phải hình ảnh), hãy sử dụng `frame_stream()`.**Định dạng JPEG là định dạng nén có mất mát và 8-bit; `frame_stream()` trả về các sản phẩm tương tự mà không có codec trong đường dẫn.

### Chụp theo khoảng thời gian cho khảo sát bằng máy bay không người lái

Một chuyến bay khảo sát cần một nhóm hình ảnh được đồng bộ hóa sau mỗi vài mét di chuyển, chứ không phải mỗi khung hình mà mảng có thể tạo ra. Có hai cài đặt điều khiển việc này:**`target_fps`**thiết lập tần suất máy ảnh chụp một khung hình, và**`capture_repeated(interval_s=…)`** thiết lập tần suất lưu một khung hình.**Máy ảnh tiếp tục truyền dữ liệu liên tục giữa các lần lưu.** Mỗi khoảng thời gian sẽ lưu nhóm hình ảnh đồng bộ mới nhất; các khung hình ở giữa sẽ không được ghi lại. Chế độ phơi sáng tự động điều chỉnh trên mỗi khung hình, do đó phơi sáng đã ổn định khi quá trình lưu diễn ra. Không có chế độ nào khiến các camera dừng lại giữa các lần chụp và được kích hoạt lại cho từng lần: nếu có, mỗi lần chụp sẽ phải chờ luồng dữ liệu bắt đầu và phơi sáng ổn định.**Đặt tốc độ khung hình phù hợp với yêu cầu của khoảng thời gian lưu.**Nếu để chế độ tự động, hệ thống sẽ chạy ở tốc độ cao nhất mà máy chủ và mạng có thể hỗ trợ. Đặt tốc độ thấp hơn sẽ giảm nhiệt độ camera, tải mạng và tải CPU của máy chủ mà không ảnh hưởng đến dữ liệu được lưu. Chọn tốc độ sao cho đạt**2–4 khung hình mỗi khoảng thời gian lưu**:

- Mỗi lần lưu sẽ chờ khung hình tiếp theo sau khi được yêu cầu, do đó một nhóm ảnh lưu lại sẽ được ghi lại muộn nhất là một chu kỳ khung hình so với thời điểm đã lên lịch. Tốc độ cao hơn sẽ giúp thu hẹp khoảng cách này.
- Chế độ phơi sáng tự động điều chỉnh một lần cho mỗi khung hình, do đó tốc độ quá thấp sẽ làm chậm tốc độ phản ứng của phơi sáng khi có đám mây đi qua.

**Tính toán khoảng thời gian dựa trên kế hoạch bay:**

```
spacing    = along-track footprint × (1 − forward overlap)
interval_s = spacing / ground speed
```

Ví dụ: vùng quét 60 m với độ chồng lấn phía trước 80% tương ứng với khoảng cách 12 m; ở tốc độ 10 m/s, giá trị là `interval_s=1.2`, và giá trị cố định `target_fps=3` tương ứng với khoảng 3 đến 4 khung hình mỗi khoảng thời gian. Quá trình chụp dựa trên thời gian: SDK không có chế độ kích hoạt theo khoảng cách hoặc GPS, do đó hãy duy trì tốc độ mặt đất ổn định trên các tuyến khảo sát.

**Lưu dữ liệu thô trên máy bay và hiệu chuẩn sau.** `interval_s` được tính từ thời điểm bắt đầu mỗi lần lưu, do đó nếu một lần lưu kéo dài hơn khoảng thời gian quy định sẽ khiến lần lưu tiếp theo bắt đầu trễ và khoảng cách giữa các khung hình bị lệch. Lưu dữ liệu thô cùng với giá trị cảm biến ánh sáng (`processing="raw"`; giá trị này được lưu theo mặc định) sẽ giúp mỗi lần lưu diễn ra nhanh chóng và đảm bảo mọi khung hình đều có thể chuyển đổi thành giá trị phản xạ sau này.

```python
import chloros_sdk

INTERVAL_S = 1.2      # from the flight plan
FLIGHT_S = 20 * 60    # bounds the loop; it also stops at count= if given

with chloros_sdk.connect_array(serials, target_fps=3.0) as arr:
    groups = arr.capture_repeated(
        "survey/", interval_s=INTERVAL_S, duration_s=FLIGHT_S,
        processing="raw", render_index=False,
        on_capture=lambda i, r: print(f"group {i}: {len(r)} frames"))
```

- **`connect_array(target_fps=…)`**cố định tốc độ từ đầu;**`arr.set_target_fps(fps)`**thay đổi tốc độ giữa chuyến bay mà không cần kết nối lại, và**`arr.set_target_fps(None)`** đưa tốc độ về chế độ tự động. Tốc độ vượt quá khả năng truyền tải của máy chủ và mạng sẽ được hạ xuống mức trần đó — kiểm tra `clamped` trong kết quả — và tốc độ thấp nhất là 0,1 fps.
- **Không nên sử dụng `burst()` để loại bỏ khung hình.** Nó ghi lại mọi khung hình mà mảng tạo ra; nó hoạt động theo một giá trị `target_fps` đã được cố định, do đó chỉ là công cụ phù hợp khi cần giữ lại mọi khung hình.
- **`target_fps` không sửa các khung hình bị hỏng.** Việc giảm giá trị này sẽ làm giảm số lượng khung hình được tạo ra, chứ không phải tốc độ gửi từng khung hình; đối với các khung hình bị hỏng do GVSP, hãy giảm giá trị `wire_ceiling_mbps` thay vào đó (xem [Tình trạng mảng](#array-health--which-subsystem-is-losing-frames)).

```python
r = arr.set_target_fps(5.0)
if r["clamped"]:
    print(f"asked for {r['requested']} fps, running at {r['target_fps']}")
```

### Kết nối với một mảng đã được kết nối — `attach_array`

Nếu mảng đã hoạt động (GUI đã mở nó, hoặc một phiên SDK trước đó đã gọi `connect_array`), hãy sử dụng `attach_array` để lấy handle của mảng thay vì kết nối lại. `connect_array` luôn báo lỗi "Camera  <sn>đã có trong mảng<id>" trong tình huống đó, vì việc gửi yêu cầu POST với `/array/connect` cho một thành viên trong nhóm không phải là idempotent; `attach_array` đọc `/api/camera/array/list` và so khớp dựa trên array_id hoặc số sê-ri.

```python
import chloros_sdk

# By serials (matches if every serial is a member of one existing array)
arr = chloros_sdk.attach_array(
    ["213800234", "214000533", "214701288", "214701292"])

# By array_id (when you've already noted it down)
arr = chloros_sdk.attach_array("array-1779862544497")

# attach_array returns the same ArraySession as connect_array
arr.capture("output/", processing="reflectance")
```

Mẫu: Các tập lệnh SDK chia sẻ với GUI máy tính để bàn nên thử `attach_array` trước và chuyển sang `connect_array` nếu chưa có mảng nào trong nhóm.

```python
import chloros_sdk

try:
    arr = chloros_sdk.attach_array(serials)
except chloros_sdk.ChlorosConnectError:
    arr = chloros_sdk.connect_array(serials)
```

> **Mảng được gắn vào là mảng mượn, không phải mảng sở hữu.** Một handle từ `attach_array` không bao giờ giải phóng mảng: cả khi khối `with` kết thúc lẫn khi gọi hàm giải phóng rõ ràng `disconnect()` đều không giải phóng nó (lệnh gọi rõ ràng này chỉ in một dòng lên stderr thông báo rằng để mảng tiếp tục chạy). Quy tắc này cũng áp dụng cho `CameraSession` / `DAQSensorSession`. Do đó, việc chia sẻ tài nguyên với GUI là an toàn dù có hay không có `with`:
>
> ```python
> with chloros_sdk.attach_array(serials) as arr:
>     arr.capture("output/", processing="reflectance")
> # … the array stays up for the GUI
> ```
>
> Khi bạn thực sự muốn kết thúc kết nối — ví dụ như để kết nối lại với một `wire_ceiling_mbps` khác — hãy gọi hàm `arr.force_disconnect()`. Việc kết nối lại sau khi gọi hàm `disconnect()` thông thường trên một handle đã được gắn sẽ thất bại với thông báo "Camera <sn>đã </sn>có <sn>trong mảng<id>", vì không có đối tượng nào được giải phóng.

### Trợ lý Phân tích Mạng

Hữu ích trước khi mở mảng — dự đoán xem các thiết lập đề xuất của bạn có phù hợp hay không:

```python
result = chloros_sdk.analyze_array_network(
    master_serial="214701288",
    slave_serials=["213800234", "214000533", "214701162"],
    width=2048, height=1536,
    pixel_format="BayerRG12",
    binning=1,
)

if result["status"] == "ok":
    print("Use the requested settings.")
elif result["status"] == "auto_capped_fps":
    r = result["recommended"]
    print(f"Keep the resolution; cap the trigger rate at {r['recommended_target_fps']} fps")
elif result["status"] == "auto_shrunk":
    r = result["recommended"]
    # width / height are sensor pixels BEFORE binning (what connect_array
    # takes with binning=); out_width / out_height is what is streamed.
    print(f"Connect with width={r['width']} height={r['height']} "
          f"binning={r['binning']} ({r['out_width']}x{r['out_height']} streamed)")
elif result["status"] == "needs_force_slip":
    print("Sim-sync impossible on this wire; force_tier='slip-emit-and-capture' required")
```

```python
analyze_array_network(
    master_serial,
    slave_serials=None,
    width=2048, height=1536,              # sensor pixels before binning
    pixel_format=None,                    # None = what connect applies (Mono12 / BayerRG12)
    binning=1,                            # 1 | 2 | 4
    force_tier=None,
    packed="auto",                        # "auto" | "on" | "off"
    backend_url="http://127.0.0.1:5000",
    timeout=30.0,
) -> dict
```

Nó không bao giờ khởi động backend: nếu backend không phản hồi sẽ gây ra lỗi `ChlorosConnectError`, trong khi yêu cầu bị từ chối sẽ trả về kết quả. Một lỗi `packed` sẽ gây ra lỗi `ValueError`.

`status` là một trong các mã lỗi `ok` / `auto_capped_fps` / `auto_shrunk` / `needs_force_slip` (nếu không thì là `error` — bao gồm yêu cầu bị từ chối: chưa đăng nhập, `error_code: AUTH_REQUIRED`, hoặc gói dịch vụ dưới mức sàn SDK, `PLAN_UPGRADE_REQUIRED`). `auto_capped_fps` có nghĩa là độ phân giải được yêu cầu chỉ phù hợp với vòng RX ở tốc độ kích hoạt bị giới hạn — giữ nguyên độ phân giải và chuyển từ `target_fps=result["recommended"]["recommended_target_fps"]` sang `connect_array` (xem [Ví dụ 6](#6-capability-probe-before-connecting-a-4-cam-array)). `auto_shrunk` chỉ xuất hiện khi khung hình được yêu cầu không thể được truyền ở bất kỳ tốc độ khung hình nào; connect không bao giờ tự động thu nhỏ khung hình, do đó hãy tự áp dụng `recommended`.

**Cách đọc thông tin chiếu** (cùng mô hình với bảng Cài đặt Mảng trên GUI):

- **Burst (`frame_bytes_total`) được cộng dồn theo từng camera ở định dạng pixel thực của mỗi camera.**Máy ảnh**M3M**đơn kênh truyền tín hiệu Mono12 (2 bit/pixel), hoặc với `pixel_format`, định dạng đơn kênh của độ sâu (`"BayerRG8"` là `Mono8`, 1 bit/pixel), do đó, nếu không có camera nào, một khung hình độ phân giải đầy đủ từ 4 camera sẽ có dung lượng**~25 MB** khi sử dụng ba camera Mono, chứ không phải ~12,6 MB như giả định tất cả đều là 8-bit. Hệ thống phía sau sẽ xác định định dạng của từng camera dựa trên mô hình của nó.
- **Admittance (`burst_fits_nic_ring`) có khả năng nhận biết lưu lượng thoát**, chứ không phải so sánh giữa chế độ toàn bộ đợt dữ liệu và vòng lặp: chế độ phát-nhận (sim-emit) hoạt động khi máy chủ xả dữ liệu khỏi vòng lặp RX nhanh hơn tốc độ các camera nạp vào. Một máy chủ 10G kết hợp với các camera 1 GbE**vẫn cho phép** truyền dữ liệu độ phân giải đầy đủ ngay cả khi đợt dữ liệu vượt quá dung lượng vòng lặp; một host 1 GbE sẽ chặn (`needs_force_slip` / `auto_shrunk`).
- **`achievable_fps_max` là giới hạn an toàn cho chế độ lấy dữ liệu nối tiếp** — `max(readout+emit, N×emit)` với tốc độ phát của từng camera bị giới hạn ở mức liên kết camera 1 GbE, không phụ thuộc vào độ phơi sáng. Ví dụ: ~2,8 khung hình/giây cho mảng 4 camera độ phân giải đầy đủ 12-bit (phù hợp với giá trị đo được trong thời gian chạy là ~2,7–3,0). Mô hình đầy đủ: [Tham khảo CLI → Mô hình fps & burst của mảng](cli-reference.md#array-fps--burst-model).
- **Sau khi ghi hình liên tục, kết quả dự đoán sẽ phản ánh tốc độ thực tế đã đạt được.** Việc ghi hình trong 30 giây trở lên ở tốc độ tự động sẽ ghi lại tốc độ ổn định tại kích thước mảng đó trên máy tính này; các thử nghiệm ngắn của bộ điều khiển tốc độ ở tốc độ cao hơn sẽ bị loại bỏ. Từ đó trở đi, dòng của `array-connect`', phân tích của `connect_array()` và bảng Cài đặt Mảng sẽ trích dẫn tốc độ đó khi nó thấp hơn tốc độ của mô hình, kèm theo lý do giới hạn trong `cycle_limiter`: `"clock"` (mảng chạy ở tốc độ đã ổn định, vòng lặp ghi dữ liệu chờ tín hiệu đồng hồ kích hoạt), `"loop"` (vòng lặp ghi dữ liệu không còn thời gian), `"capture"` (quá trình ghi hình bị chậm lại và bỏ qua các nhóm) hoặc `"measured"` (không có thông tin nào chỉ ra cụ thể là trường hợp nào). Con số của mô hình được nêu trong `wire_fps_max`. Mảng vẫn tăng lên đến mức mà đường truyền có thể xử lý.
- **Quá tải (`oversubscribed: true`) có nghĩa là đường truyền không thể xử lý mảng ở bất kỳ tốc độ nào mà các camera đạt được** — các trường fps (`achievable_fps_max` / `fps_bright` / `fps_dark`) hiển thị giá trị 0, và việc gộp pixel (binning) không thể khắc phục được vấn đề này (vì nó chỉ giảm số byte trên mỗi khung hình, chứ không giảm số byte được đồng bộ hóa mỗi giây). Chỉ riêng đường truyền chậm không gây ra hiện tượng này: thay vào đó, tốc độ khung hình sẽ giảm, và nếu các camera không thể được điều chỉnh tốc độ đủ chậm, cấp độ sẽ chuyển sang `slip-emit-and-capture` (chế độ nối tiếp). Các giải pháp bao gồm giảm số lượng camera, sử dụng khung jumbo hoặc sử dụng NIC nhanh hơn; `max_cams_collision_safe` báo cáo giới hạn tối đa của đường truyền và camera của bạn. Phản hồi cũng chứa các mã lỗi `aggregate_demand_bps`, `collision_safe_ceiling_bps` và `per_cam_floor_bps`. Xem [Quá tải](#over-subscription-the-per-cam-floor).

### Khám phá & Liệt kê

```python
chloros_sdk.discover_lattice_cameras()   # list all cams visible to the backend
chloros_sdk.list_cameras()               # cams currently in the pool
chloros_sdk.list_arrays()                # active arrays in the pool
```

Mỗi hàm này chỉ chấp nhận các từ khóa `backend_url=` và `timeout=30.0`, và trả về một danh sách các từ điển. `discover_lattice_cameras()` (`serial`, `ip`, `mac`, `model`, `lattice_model`, `vendor`, `connected`, `array_id`, …) cũng bao gồm `auto_start_backend=True`; các lệnh `list_*` không bao giờ khởi động backend.

---

## Smart-AE / Smart-Capture

Các mảng LATTICE chạy chế độ AE liên tục ở chế độ nền ngay khi được kết nối, nhưng một cảnh vừa được định hướng sẽ mất một chút thời gian để hội tụ. **Smart-capture** là tính năng tiện lợi được tích hợp sẵn: nó kiểm tra độ phơi sáng của từng camera, chờ cho đến khi mảng ổn định trong một khoảng thời gian nhất định, sau đó kích hoạt quá trình chụp. Tính năng này tương đương với giao diện người dùng (GUI): nút “smart” capture trên ứng dụng máy tính để bàn gọi cùng một điểm cuối backend.

```python
import chloros_sdk

with chloros_sdk.connect_array([
        "213800234", "214000533", "214701288", "214701292"]) as arr:
    # Initial pose
    arr.capture("pose_a/", processing="reflectance", smart=True)
    input("Move the rig, then press Enter...")
    # New pose — smart-capture waits for AE to re-settle automatically
    arr.capture("pose_b/", processing="reflectance", smart=True)
```

Khi điều khiển qua `ChlorosProject` (phần tiếp theo), bạn sẽ có thêm các tùy chọn:

```python
proj.arrays["main_rig"].capture_smart(
    output_dir="out/",
    processing="reflectance",
    settle_timeout_s=5.0,           # max wait
    stability_window_s=1.5,         # exposure must hold steady this long
    exposure_tolerance_pct=5.0,     # %-spread allowed within the window
)
```

Chính sách AE thông minh mặc định là bảo thủ. Hãy thu hẹp `exposure_tolerance_pct` cho các công việc đo lường bức xạ đòi hỏi độ chính xác cao; mở rộng cho các cảnh thay đổi nhanh, nơi bạn chỉ cần "gần đúng".

---

## Phiên cảm biến DAQ

Nhóm backend liên tục cho các cảm biến quang phổ (DAQ-U qua USB, DAQ-M qua BLE, DAQ-E qua Ethernet). Phản ánh bề mặt camera: phát hiện thông minh, tái sử dụng nhóm, kết nối idempotent.

### Phát hiện thông minh (Không cần cấu hình)

```python
import chloros_sdk

with chloros_sdk.connect_daq_sensor() as daq:
    print(daq.model, daq.transport, daq.address)
    for frame in daq.latest(n=10):
        spectrum = frame["spectrum"]   # list[float] (W/m²/nm if calibrated)
        is_sat = frame["is_saturated"]
        x, y, z = frame["x"], frame["y"], frame["z"]
        print(len(spectrum), is_sat)
```

Thứ tự ưu tiên: Ethernet → BLE → USB. Truyền bất kỳ gợi ý rõ ràng nào để cố định giao thức truyền tải.

### Giao thức truyền tải cố định

```python
# DAQ-U on a specific serial port
daq = chloros_sdk.connect_daq_sensor(transport="usb", port="COM3")

# DAQ-M over BLE by MAC (implies transport="ble")
daq = chloros_sdk.connect_daq_sensor(mac="AA:BB:CC:DD:EE:FF")

# DAQ-E over Ethernet by hostname (implies transport="eth")
daq = chloros_sdk.connect_daq_sensor(eth_host="daq-e-xxx.local")

# A specific sensor by id (as list_daq_sensors() shows it; ':' or '-' alike)
daq = chloros_sdk.connect_daq_sensor(sensor_id="CB-7C-A7-5A-73")

# Tuning knobs
daq = chloros_sdk.connect_daq_sensor(
    port="COM3",
    integration_time=64,      # ms
    frame_avg=20,
    enable_ae=True,
    start_streaming=True,
)
```

`sensor_id=` chỉ định một cảm biến cụ thể, và chỉ cảm biến đó mới được mở. Hệ thống sẽ so khớp với danh sách các cảm biến đã mở; sau đó, tên máy chủ hoặc địa chỉ IP của DAQ-E (nếu DAQ-E chỉ được phát hiện qua IP trong quá trình quét mạng, hệ thống sẽ yêu cầu (chỉ đọc) tên, địa chỉ MAC và số sê-ri của nó, nên bất kỳ thiết bị nào trong số đó cũng có thể phát hiện được nó), địa chỉ MAC BLE của DAQ-M, và ID mà DAQ-U hoặc DAQ-M báo cáo sau khi được mở (bất kỳ thiết bị nào khác được mở để kiểm tra sẽ được đóng lại). Nếu không tìm thấy, `connect_daq_sensor` sẽ kích hoạt `ChlorosConnectError` liệt kê những gì đã tìm thấy; kèm theo địa chỉ (`port=` / `mac=` / `eth_host=`) chứa một cảm biến khác, nó cũng sẽ kích hoạt. Chương trình không bao giờ tự động chuyển sang cảm biến nào đang hiển thị — đó chính là mục đích của lệnh kiểm tra cờ zero.</id></sn></id></sn>

### `connect_daq_sensor()` Chữ ký

```python
connect_daq_sensor(
    *,                                    # every argument is keyword-only
    sensor_id=None,                       # one named sensor (see above)
    transport=None,                       # "usb" | "ble" | "eth"; None = smart-detect
    port=None,                            # DAQ-U serial port ("COM3", "/dev/ttyUSB0")
    mac=None,                             # DAQ-M BLE MAC (implies transport="ble")
    eth_host=None,                        # DAQ-E hostname or IP (implies transport="eth")
    integration_time=None,                # ms; None = the sensor's own
    frame_avg=None,
    enable_ae=None,
    cap_id=None,                          # cap correction: "sunshine_cosine", "fov_30", ..., "none" (bare)
    start_streaming=True,
    backend_url="http://127.0.0.1:5000",
    timeout=60.0,
    auto_start_backend=True,              # spawn a local backend if none is running
) -> DAQSensorSession
```

Một cảm biến đã có sẵn trong nhóm sẽ được trả về nguyên trạng (mượn): `integration_time`, `frame_avg` và `enable_ae` không được áp dụng cho cảm biến đó. Hãy ngắt kết nối cảm biến trước để mở nó với các cài đặt của bạn. `cap_id` là trường hợp ngoại lệ: trên cảm biến mượn, nó được thiết lập bằng `set_cap()`. Nếu bỏ qua, giá trị mặc định của hệ thống nền sẽ được áp dụng (xem ghi chú dưới bảng phương thức).

### Các phương pháp `DAQSensorSession`

| Phương pháp | Mô tả |
| --- | --- |
| `status(timeout=10.0)` | Tóm tắt mục trong nhóm (trạng thái truyền trực tiếp/ghi hình, dải bước sóng, giá trị SHA hiệu chuẩn, thời gian tích hợp, frame_avg, trạng thái AE). Đối với DAQ-E có luồng dữ liệu ngừng hoạt động: `reconnect` (trong trường hợp hệ thống backend đang thực hiện kết nối lại tự động: `state`, `attempts`, `next_attempt_in_s`, `last_error`; `None` khi đang truyền dữ liệu), `stream_lost_s`, `reconnects` và `last_reconnect`, cùng với `record_gaps` cho quá trình ghi đang diễn ra; `rx_stream_fault` cho biết trạng thái kết nối lại cho đến khi khung phổ đầu tiên đến. |
| `latest(*, n=1, timeout=10.0)` | Trả về tối đa N khung phổ gần đây nhất (mỗi khung là `ts_ns`, `spectrum`, `is_saturated`, `integration_time`, `x`, `y`, `z`). |
| `stream_start()` / `stream_stop()` | Tiếp tục / tạm dừng truyền dữ liệu (cổng vẫn mở). |
| `record_start(*, output_dir=None, device_name=None, timeout=15.0)` | Bắt đầu ghi tệp .daq. Trả về đường dẫn tệp. Bị từ chối đối với DAQ-U/M không có gói hiệu chuẩn AWS (DAQ-E được miễn trừ). |
| `record_stop()` | Dừng ghi. Trả về `{path, rows, warning, gaps}`; `warning` tương ứng với `None` đối với bản ghi sạch, còn lại là thông báo chẩn đoán từ hệ thống nền (ví dụ: tệp trống hoặc khoảng trống trong luồng dữ liệu), thông báo này cũng được in ra stderr. `gaps` liệt kê từng đoạn mà luồng dữ liệu không truyền tải gì: `start_utc`, `end_utc` (`None`: vẫn bị mất khi dừng), `duration_s`, `reconnected`, `marked_in_file`. Dòng này trống trong trường hợp ghi liên tục. |
| `set_cap(cap_id, timeout=15.0)` | Đặt cấu hình hiệu chỉnh giới hạn được áp dụng cho các giá trị đo từ lần tiếp theo trở đi: `"sunshine_cosine"`, `"fov_15"` ... `"fov_90"`, hoặc `"none"` (hoặc `None`) đối với cảm biến trần. Trả về, mà `cap_id` sẽ đọc lại. Thiết bị DAQ-E-S luôn giữ giá trị `as_recorded` bất kể yêu cầu gì (bộ khuếch tán của nó được tích hợp sẵn). Kích hoạt `ChlorosConnectError` cho hồ sơ không không tải được. CLI: `chloros-cli daq pool-set-cap`. |
| `enable_ptp(timeout=15.0)` | Chỉ dành cho DAQ-E: bật lại PTP cho thiết bị có PTP đang tắt (`status()["rx_ptp_enabled"]` là `False`; cài đặt được lưu trên thiết bị, do đó chế độ này sẽ duy trì trạng thái tắt cho đến khi được bật lại). Trả về trạng thái đã được cập nhật. Gây ra lỗi `ChlorosConnectError` đối với DAQ-U / DAQ-M, hoặc khi thiết bị không xác nhận. Chloros không bao giờ tự động bật lại thiết bị, vì trạng thái tắt có thể là do chủ ý. CLI: `chloros-cli daq pool-enable-ptp`. |
| `device_info(timeout=15.0)` | Chỉ dành cho DAQ-E: các cài đặt riêng của thiết bị. `firmware` (`running`, `bundled`, `update_available`), `ip` và `ip_mode` (`dhcp` / `linklocal` / `static`: cái đang chạy hiện tại), `network` (cài đặt đã lưu: `mode`, cộng với `ip` / `mask` / `gateway` khi ở chế độ tĩnh), `network_change_pending` (đúng cho đến khi thiết bị chạy cài đặt đã lưu, một cài đặt đã lưu bao gồm `set_network(..., reboot=False)`), `auto_stream`, `ptp`. `firmware["update_available"]` là `None` khi phiên bản của thiết bịkhông thể đọc được phiên bản của thiết bị. Trong khi luồng dữ liệu từ thiết bị không đến được máy tính này, nó được đọc trên một kết nối mới và từ điển bổ sung `stream_silent` (`True`), `stream_fault` và `reconnect`; thiết bị không phản hồi sẽ gây ra lỗi. CLI: `pool-info`. |
| `set_network(mode="auto", *, ip=None, mask=None, gateway=None, reboot=True, force=False, comeback_timeout=45.0)` | DAQ-E chỉ: DHCP (`"auto"`) hoặc địa chỉ tĩnh. Khởi động lại thiết bị để áp dụng cài đặt và kết nối lại; phiên làm việc này vẫn tiếp tục hoạt động và `address` sẽ theo dõi thiết bị. `reboot=False` chỉ lưu lại cài đặt. Xem [Cài đặt Thiết bị DAQ-E](#daq-e-device-settings). CLI: `pool-set-network`. |
| `set_auto_stream(enabled, timeout=15.0)` | Chỉ dành cho DAQ-E: truyền dữ liệu ngay khi bật nguồn mà không cần kết nối phần mềm (được lưu trên thiết bị). Trả về cài đặt đã được xác nhận. CLI: `pool-set-auto-stream`. |
| `reboot(*, comeback_timeout=45.0)` | Chỉ dành cho DAQ-E: Khởi động lại thiết bị và kết nối lại với các cài đặt ban đầu. CLI: `pool-reboot`. |
| `update_firmware(*, comeback_timeout=45.0)` | Chỉ dành cho DAQ-E: Cập nhật firmware đi kèm với bản phát hành Chloros này lên thiết bị đang chạy phiên bản cũ hơn, sau đó kết nối lại (khoảng một phút; không tắt nguồn). Trả về các mã lỗi `updated`, `fw_before`, `fw_after`, `bundled` và `warning` khi thiết bị khởi động lại chậm hoặc khởi động lại với phiên bản khác; `updated` tương ứng với `False` đối với thiết bị đã cập nhật lên phiên bản mới nhất. Gây ra lỗi `ChlorosConnectError` đối với thiết bị không thể đọc được phiên bản. CLI: `pool-update-firmware`. |
| `disconnect()` | Giải phóng khỏi nhóm. Không-op cho các handle được gắn nhưng không thuộc sở hữu. Trên một cảm biến mà phiên này đã mở, lệnh này là `force_disconnect()`, và gây ra lỗi `ChlorosConnectError` khi việc giải phóng không được xác nhận (việc thoát khỏi khối `with` không bao giờ gây ra lỗi). |
| `force_disconnect(timeout=30.0)` | Giải phóng cảm biến bất kể ai đã mở nó — ví dụ như một cảm biến bị kịch bản gặp sự cố bỏ lại trong nhóm, mà kịch bản mới chỉ truy cập được dưới dạng một handle được gắn vào, trong đó `disconnect()` không thực hiện hành động nào. Việc kết thúc cảm biến của một khách hàng khác sẽ dừng mọi quá trình ghi dữ liệu mà cảm biến đó đang thực hiện. Hàm trả về kết quả ngay khi phần nền xác nhận việc giải phóng, hoặc khi không có gì để giải phóng (cảm biến không có trong nhóm, hoặc không có backend nào đang chạy); việc giải phóng hết thời gian chờ hoặc bị backend từ chối sẽ gây ra lỗi `ChlorosConnectError`. CLI: `chloros-cli daq pool-disconnect --sensor-id ID`. |

Thuộc tính: `sensor_id`, `model`, `transport`, `address` (theo sau một DAQ-E mà `set_network()` đã định địa chỉ lại), `info` và `backend_url`.

> **Đặt tên cho nắp.** DAQ-U, DAQ-M và DAQ-E được giao hàng mà không có tụ điện, và Chloros không thể xác định liệu có tụ điện được lắp đặt hay không: một cảm biến được kết nối mà không có `cap_id` sẽ nhận được giá trị `sunshine_cosine`, một giả định mà các bản ghi của nó cho thấy đã được thực hiện (`cap_id_source=auto_default`). Một DAQ-U không có nắp dưới điều chỉnh đó sẽ hiển thị giá trị cao gấp khoảng 30 lần, vì vậy hãy truyền giá trị `cap_id="none"` cho cảm biến không có nắp (hoặc `set_cap("none")`), còn nếu không, hãy sử dụng cấu hình của nắp mà bạn đã lắp.

### DAQ-E-S — bộ khuếch tán cố định và cảm biến nghiêng

**DAQ-E-S** là một DAQ-E với hai điểm khác biệt quan trọng đối với phần mềm. Tất cả các tính năng khác — phát hiện, kết nối, PTP, ghi dữ liệu, các cài đặt thiết bị dưới đây — đều hoạt động chính xác như đối với DAQ-E.**Bộ khuếch tán của nó là cố định, do đó không cần hiệu chỉnh nắp.** Bộ khuếch tán kiểu Sunshine được tích hợp sẵn và đã có trên thiết bị khi thực hiện hiệu chuẩn tại nhà máy, do đó hiệu chuẩn này đã phản ánh chính xác bức xạ xuống. Chloros đặt giới hạn (cap) của nó thành `as_recorded` (không có cấu hình) bất kể yêu cầu là gì: một giới hạn được đặt tên cho nó (`sunshine_cosine`, `fov_*`, `none`) sẽ bị bỏ qua kèm theo thông báo, và các bản ghi của thiết bị lưu trữ giá trị `cap_id_source=model`, do đó cảnh báo về "giả định nắp" sẽ không bao giờ kích hoạt đối với thiết bị này. Cũng không nên áp dụng giới hạn cho dữ liệu của nó bằng tay: `sunshine_cosine` ở trên cùng sẽ hiển thị giá trị cao gấp khoảng 11 lần.

**Nó được trang bị cảm biến gia tốc 3 trục.** Đáp ứng cosin của bộ khuếch tán ánh sáng mặt trời chỉ có giá trị khi thiết bị ở trạng thái cân bằng, do đó, cảm biến bị nghiêng sẽ báo cáo cường độ bức xạ sai — và độ phản xạ cũng kế thừa lỗi này — mà không có triệu chứng nào khác. Do đó, mỗi giá trị đọc từ DAQ-E-S đều mang theo thông tin về tư thế của thiết bị:

| Trường | Ý nghĩa |
| --- | --- |
| `tilt_deg`, `roll_deg`, `pitch_deg` | Góc tính bằng độ. `None` khi phần mềm nhúng đánh dấu góc là không xác định (rơi tự do, trục hỏng). |
| `x_mg`, `y_mg`, `z_mg`, `mag_mg` | Gia tốc tính bằng milli-g. `mag_mg` khác xa ~1000 có nghĩa là thiết bị đang di chuyển và các góc không có ý nghĩa đối với mẫu đó. |
| `cal_applied` | Đã thiết lập: các trục và góc nằm trong hệ quy chiếu của bộ khuếch tán (đã áp dụng hiệu chuẩn lắp đặt của thiết bị). Chế độ rõ ràng: hệ quy chiếu gốc của bo mạch, có thể hiển thị độ nghiêng lên đến ~2,3° nhưng không phản ánh thực tế. |
| `sample_age_ms` | Thời gian trôi qua kể từ khi mẫu gia tốc kế được ghi lại cho đến khi phổ dữ liệu được gửi đi. |
| `temp_raw` | **Xu hướng** nhiệt độ chip chưa hiệu chuẩn — chỉ thể hiện sự chênh lệch, không phải giá trị đo trực tiếp từ nhiệt kế. |

`DAQSensorSession.latest()` trả về các giá trị này dưới khóa `imu` của từng khung (`None` trên DAQ-U, DAQ-M hoặc DAQ-E tiêu chuẩn). Một `.daq` được ghi lại từ DAQ-E-S lưu trữ các giá trị tương tự trong các cột `imu_*` của nó (`imu_tilt_deg`, `imu_roll_deg`, `imu_pitch_deg`, `imu_mag_mg`, `imu_cal_applied`, …) bên cạnh mỗi phổ, nhờ đó người đọc có thể phân biệt được hiện tượng mây với trường hợp chân máy bị va đập. Trên các mẫu khác, các cột này đều trống.

Dòng dữ liệu tư thế báo cáo trên các trường riêng của nó trong `status()` (`rx_imu_stream_fault`, `rx_imu_stream_faults`, `rx_last_imu_stream_fault`), được tách biệt với các trường phổ để sự cố cảm biến gia tốc không bao giờ bị hiểu nhầm là khoảng trống trong bản ghi. `discover_daq_sensors()` và `model` của phiên làm việc hiển thị `DAQ-E-S`.

### Cài đặt thiết bị DAQ-E

```python
with chloros_sdk.connect_daq_sensor(eth_host="daq-e-def330.local") as daq:
    info = daq.device_info()
    print(info["ip"], info["ip_mode"], info["firmware"])

    # A fixed address on the network this computer is on
    daq.set_network("static", ip="192.168.1.50", gateway="192.168.1.1")
    print(daq.address)              # 192.168.1.50: the session follows it

    if info["firmware"]["update_available"]:
        daq.update_firmware()
```

- **Máy tính này phải có địa chỉ tĩnh trên mạng.** `set_network` sẽ gây ra lỗi `ChlorosConnectError` đối với những máy tính có mạng con không bao gồm địa chỉ của máy tính nàytrên mạng của cảm biến: sau khi khởi động lại, không có thiết bị nào có thể kết nối với cảm biến để khôi phục cài đặt ban đầu. Chỉ áp dụng `force=True` khi máy tính này được gán lại địa chỉ hoặc định tuyến sang mạng con mới.
- **`set_network`, `reboot` và `update_firmware` khởi động lại cảm biến**, và kích hoạt `ChlorosConnectError` trong khi cảm biến đang ghi dữ liệu, và khi nó không quay lại trong vòng `comeback_timeout` giây. ID cảm biến và tên máy chủ `daq-e-<id>.local` không bao giờ thay đổi.
- **Quá trình ghi dữ liệu bắt đầu ngay trước khi khởi động lại sẽ được chính quá trình này đóng lại và đặt tên.** Khi cảm biến hoạt động trở lại, đối tượng dict trả về sẽ chứa `recording_closed` (đường dẫn tệp) và một giá trị `warning` xác nhận điều này. Khi cảm biến không kết nối lại hoặc quá trình chuyển firmware thất bại, giá trị `ChlorosConnectError` sẽ thông báo điều này thay thế: thông báo của nó kết thúc bằng cảnh báo, và các giá trị `e.recording_closed` / `e.warning` được thiết lập (cả hai `None` khi không có bản ghi nào được đóng). Bắt đầu ghi lại ngay khi cảm biến đã kết nối lại.
- **Chỉ cảm biến đó được kết nối lại.** Sau khi khởi động lại, thiết bị phải trả lời vị trí của nó -- và sau khi `set_network("auto")` cũng dưới tên `.local` của nó -- bằng tên, tên máy chủ và mẫu máy, từ bộ điều hợp mạng (MAC) mà nó từng có; `address` trở thành địa chỉ IP đã phản hồi. Một phản hồi dưới tên đó chỉ được tính khi sử dụng chính địa chỉ MAC đó, do đó từ một máy tính kết nối với cảm biến qua bộ định tuyến, hoặc từ mạng NAT mặc định của WSL2, một cảm biến đã được DHCP di chuyển sẽ không được kết nối lại (`ChlorosConnectError`; hãy kết nối lại tại địa chỉ mới của nó).
- **Việc cập nhật firmware yêu cầu cảm biến phải kết nối lại.** `update_firmware()` gửi yêu cầu kết nối đến thiết bị, và thiết bị sẽ mở một kết nối TCP trở lại máy tính này để tải hình ảnh, trên cổng do hệ điều hành chọn. Phần backend lắng nghe trên bộ điều hợp kết nối với thiết bị (trên mọi bộ điều hợp chỉ khi không xác định được cụ thể là bộ nào) và chỉ phục vụ thiết bị đó: mọi kết nối khác đều bị từ chối (`[DAQ-FW] … refused a firmware connection from <ip>` trong đầu ra của máy chủ) trong khi máy chủ tiếp tục chờ đợi. Tường lửa chặn kết nối này sẽ khiến quá trình cập nhật thất bại với thông báo `device never connected back on TCP <port>`. Trình cài đặt Windows sẽ tự động thêm quy tắc cho việc này (xem [Bước 1](#windows-exe)); trên tường lửa khác, hãy cho phép lưu lượng TCP đến từ mạng con của cảm biến vào máy chủ phía sau.
- **Trong môi trường WSL2, hãy sử dụng mạng phản chiếu** (`networkingMode=mirrored` trong `.wslconfig`): ở chế độ NAT mặc định, mọi địa chỉ tĩnh đều bị từ chối (địa chỉ riêng của WSL không nằm trong mạng của cảm biến), luồng dữ liệu không đến được WSL và quá trình cập nhật firmware không thể hoàn tất.
- Giao diện người dùng (GUI) duy trì kết nối riêng với cảm biến: việc khởi động lại từ tập lệnh sẽ kết thúc phiên kết nối của GUI với cảm biến đó. GUI có cùng các thiết lập (tab Cảm biến Ánh sáng, thiết lập của cảm biến, Mạng).

### Khám phá — tìm địa chỉ để kết nối

`discover_daq_sensors()` quét các cổng USB / BLE / ETH để tìm các cảm biến mà bạn *có thể* mở. Đây là phiên bản DAQ tương ứng với `discover_lattice_cameras()`, và là cách duy nhất để lấy **địa chỉ MAC BLE của DAQ-M** — trong khi DAQ-E có tên máy chủ và DAQ-U có cổng COM, nhưng địa chỉ MAC không được in trên thiết bị cũng như không được hệ điều hành liệt kê.

```python
for s in chloros_sdk.discover_daq_sensors():
    print(s["transport"], s["address"], s["model"], s["extra"])
# ble  C3:D8:85:E0:0A:19  DAQ-M  {'name': 'NSP32_SPECTRUM'}
# usb  COM3               DAQ-U  {'vid': '0403', 'pid': '6001', 'manufacturer': 'FTDI'}

# `address` is exactly what connect_daq_sensor wants:
for s in chloros_sdk.discover_daq_sensors(transports=["ble"]):
    if s["model"] == "DAQ-M":
        daq = chloros_sdk.connect_daq_sensor(mac=s["address"])
```

| Trường | Mô tả |
| --- | --- |
| `transport` | `usb` \| `ble` \| `eth`. |
| `address` | Cổng COM / MAC BLE / tên máy chủ — chuyển sang `connect_daq_sensor` dưới dạng `port=` / `mac=` / `eth_host=`. |
| `display` | Nhãn dễ đọc. |
| `model` | `DAQ-U` \| `DAQ-M` \| `DAQ-E` \| `DAQ-E-S`, hoặc `None`. Cảm biến Ethernet là `DAQ-E-S` hoặc `DAQ-E` vì nó tự báo cáo khi được yêu cầu, chỉ đọc, trong quá trình quét (`DAQ-E` khi nó không trả lời kịp thời). Cổng nối tiếp USB hiển thị giá trị `DAQ-U` khi chip cầu nối của nó là một trong những loại mà DAQ-U sử dụng (FTDI, CH340, CP210x, Prolific) — đây là gợi ý từ mô tả của cổng, không phải là thiết bị dò — và `None` cho bất kỳ thiết bị USB nào khác, thiết bị này vẫn được liệt kê nên bạn có thể truyền nó dưới dạng `port=`. Một cổng nối tiếp không có ID nhà sản xuất USB và không có chip cầu nối như vậy (một UART trên bo mạch chủ như `/dev/ttyS*` của Linux, một liên kết nối tiếp Bluetooth, Intel AMT) không thể là DAQ-U và không được liệt kê. |
| `extra` | Chi tiết theo từng giao thức (Tên quảng cáo BLE; USB `vid`, `pid`, `manufacturer`, `serial_number`; DAQ-E ip/fw/…). Các giá trị trống sẽ bị bỏ qua. |

| Thông số | Mặc định | Mô tả |
| --- | --- | --- |
| `transports` | cả ba | Dãy (hoặc chuỗi CSV) giới hạn quá trình quét. Nên truyền giá trị này khi bạn biết chính xác điều mình muốn — BLE là phần chậm nhất. |
| `scan_timeout` | 5 | Khoảng thời gian quét cho mỗi giao thức tính bằng giây; hệ thống nền sẽ giới hạn trong khoảng 1–20. |
| `timeout` | 60,0 | Giới hạn HTTP cho toàn bộ cuộc gọi (như ở các phần khác trong SDK). |
| `auto_start_backend` | `True` | Khởi tạo một backend cục bộ nếu chưa có backend nào đang chạy. Không bao giờ khởi tạo cho một `backend_url` từ xa. |

> **Các cảm biến đã được mở trong nhóm sẽ không xuất hiện.** Một thiết bị ngoại vi BLE đã kết nối sẽ ngừng phát tín hiệu quảng cáo và một cổng COM đang mở không thể được dò tìm, do đó danh sách phát hiện chỉ liệt kê những gì *có sẵn để kết nối*. Kết quả trống ngay sau khi bạn kết nối thiết bị là điều bình thường — hãy sử dụng `list_daq_sensors()` cho những thiết bị bạn đã kết nối. Các giao thức truyền tải không thể quét (không cài đặt bleak / zeroconf) sẽ bị bỏ qua thay vì báo lỗi, do đó máy tính không có Bluetooth vẫn nhận được kết quả từ USB và ETH.

### Danh sách

```python
for s in chloros_sdk.list_daq_sensors():
    print(s["sensor_id"], s["model"], s["transport"], s["wavelength_range"])
```

### Thiết bị DAQ nào cấp dữ liệu cho camera nào — `daq_bindings()`

Độ phản xạ của một lần chụp và tệp `.daq` mà nó lưu lại sẽ sử dụng DAQ được gán cho camera hoặc mảng đó; nếu không, hệ thống sẽ sử dụng DAQ mới nhất mà phần nền có sẵn. Việc chọn cảm biến ánh sáng trên giao diện người dùng và tệp `chloros-cli daq pool-bind` sẽ thiết lập điều này; cũng như:

```python
chloros_sdk.set_daq_bindings(cameras={"214701281": "COM7"},          # a sensor id or an address
                             arrays={"array-1779862544497": "daq-e-def330"})
chloros_sdk.set_daq_bindings(cameras={"214701281": None})            # None clears it
for serial, b in chloros_sdk.daq_bindings()["cameras"].items():
    print(serial, b["binding"], b["source"], b["resolved"]["in_pool"])
```

`daq_bindings()` trả về `{"cameras": {serial: b}, "arrays": {array_id: b}}`: mỗi `b` chứa `binding` (giá trị đã được thiết lập), `source` (`project`, từ `sensors.json` của dự án, hoặc `runtime`), `known` / `known_as` (liệu tên khóa camera hay mảng có phải là tên mà một Chloros nào đó biết hay không, và như thế nào: `connected`, `project` — `cameras.json` của dự án đang mở — hoặc, đối với camera, `seen`: đã từng được mở trên máy tính này trước đây; `known: False` cho một khóa không chỉ định bất kỳ đối tượng nào) và `resolved` (cảm biến nhóm mà nó hiện đang ánh xạ đến: `in_pool`, `sensor_id`, `address`, `model`, `streaming`). Liên kết thời gian chạy sẽ ưu tiên hơn so với tệp dự ántrong phiên làm việc; tệp dự án không bị thay đổi. Việc ngắt kết nối một camera, một mảng hoặc một thiết bị DAQ sẽ hủy bỏ mọi liên kết với phần cứng đó, bất kể ai đã thiết lập (các thành viên của mảng sẽ được liên kết cùng với mảng).

`set_daq_bindings()` chỉ liên kết với camera đang được kết nối, có trong dự án đang mở hoặc đã từng được mở trên máy tính này trước đó, và mảng đang được kết nối hoặc có trong dự án đang mở (hoặc phím đã được liên kết). Bất kỳ phím nào khác sẽ tạo ra `ChlorosConnectError` với tên (`400`, `code`, `UNKNOWN_BINDING_TARGET`, cùng với các phím `unknown`), và **không có gì** trong lệnh gọi đó được gán. `None` xóa bất kỳ khóa nào.

### Phản xạ môi trường xung quanh + đối tượng — `light_sensor_reflectance()`

Hai thiết bị DAQ được kết hợp, một hướng lên bầu trời (ánh sáng xung quanh) và một hướng xuống mục tiêu (đối tượng), cung cấp chỉ số phản xạ và chỉ số thực vật của mục tiêu từ các giá trị đo mới nhất: cặp "Ánh sáng xung quanh + Đối tượng" trên giao diện người dùng (GUI) và `chloros-cli daq reflectance` mà không cần mở lại các cảm biến lần thứ hai. Cả hai thiết bị phải được kết nối và đang truyền dữ liệu.

```python
r = chloros_sdk.light_sensor_reflectance("daq-e-def330", "daq-e-54b774")
print(r["reflectance_type"])                       # "Apparent Reflectance" when both are calibrated
for name, band in r["bands"].items():
    print(name, band["rho"] if band["valid"] else band["reason"])
for idx in r["indices"]:
    print(idx["name"], idx["value"])
```

```python
light_sensor_reflectance(
    ambient_sensor_id, object_sensor_id, *,
    bands=None,          # [{"name", "center_nm", "fwhm_nm", "role"}]; role blue / green / red / nir
    preset=None,         # a preset id from daq_band_presets(); default: the suggested one, else "GENERIC"
    expressions=None,    # custom indices: [{"name": "MyNDVI", "expr": "(NIR_850 - Red_660) / (NIR_850 + Red_660)"}]
    builtins=True,       # include the built-in indices
    backend_url="http://127.0.0.1:5000", timeout=30.0,
) -> dict
```

- **Dải phổ.** Độ phản xạ của mỗi dải phổ là độ phản xạ tích phân của dải phổ đối tượng- tích phân trên dải của môi trường xung quanh. `daq_band_presets(filter_code=None)` liệt kê các bộ dải: mỗi bộ tương ứng với một bộ lọc LATTICE M3C (`RGN`, `OCN`, `NGB`, mô hình dải của chính máy ảnh, do đó NDVI của DAQ và NDVI của máy ảnh là cùng một đại lượng) và `GENERIC` dải rộng. `suggested` là cài đặt sẵn tương ứng với `filter_code`; nếu không, đó là bộ lọc của máy ảnh được kết nối đầu tiên (`suggested_from` chỉ định cụ thể).
- **Trả về** `bands` (theo từng dải tần: `rho`, `valid`, `reason`, `e_ambient`, `e_object`), `indices` (các), `custom` (mỗi cái đi kèm với `value`, hoặc `valid: False` và `error`), `invalid_bands`, `reflectance_type`, `preset` và hai giá trị đo là `timestamps` (`ts_ns`). Dải giá trị mà cảm biến môi trường không thể đo được (nằm ngoài phạm vi đo hoặc ở mức nhiễu nền) sẽ bị coi là không hợp lệ, kèm theo lý do, và mọi chỉ số cần dựa vào dải đó sẽ bị loại bỏ, không bao giờ được tính toán dựa trên giá trị 0.
- **Tương đối hoặc biểu kiến.** Các chỉ số phụ thuộc vào tỷ lệ (SAVI, OSAVI, EVI, MSAVI2, RDVI, TDVI, LAI, NLI, MNLI, FCI, GEMI) cần tỷ lệ tuyệt đối, do đó chúng chỉ được tính khi cả hai cảm biến đều đã được hiệu chuẩn công suất (mỗi cảm biến có mã `calibration_bundle_sha`): `"Apparent Reflectance"`. Ngược lại, kết quả sẽ là `"Relative Reflectance"`, chỉ bao gồm các chỉ số không phụ thuộc vào tỷ lệ.
- **Hai lưới bước sóng khác nhau** được so sánh dựa trên vùng chồng chéo: các điểm của cảm biến môi trường nằm trong phạm vi của đối tượng, phổ của đối tượng được nội suy lên các điểm đó.
- **Hai cảm biến, không bao giờ là một.** Đặt tên một cảm biến là cả hai (cùng một id, bất kể cách viết nào mà pool chấp nhận) sẽ gây ra lỗi `ChlorosConnectError`: giá trị đọc của cảm biến trên chính nó là 1,0 trên mọi dải tần, điều này không đo được gì cả. Hệ thống backend cũng từ chối điều này (`POST /api/daq/indices` trả lời `400`, `code` `SAME_SENSOR`, trong khi `ambient_sensor_id` và `object_sensor_id` mà SDK gửi là một cảm biến được gộp chung, ví dụ: ID và cổng của cảm biến đó).

### Chia sẻ với CLI và các tập lệnh khác

Nhóm cảm biến DAQ của backend được chia sẻ bởi mọi khách hàng CLI và SDK trên máy. Nếu một cảm biến đã có trong nhóm — được mở bởi `chloros-cli daq pool-connect` hoặc bởi một skript khác — việc gọi `connect_daq_sensor(port="COM3")` từ Python sẽ trả về một handle mượn: lệnh `disconnect()` của nó không thực hiện thao tác nào, do đó skript của bạn sẽ không làm mất cảm biến khỏi client đã mở nó khi thoát khỏi phạm vi. Điều tương tự cũng áp dụng cho `sensor_id=` (được gõ cùng với `:` hoặc `-`, cả hai đều phù hợp), đối với một DAQ-E được chỉ định bằng địa chỉ IP khi nó được mở bằng tên máy chủ (hoặc ngược lại), và đối với tính năng phát hiện thông minh có cờ zero khi phần backend trả lời bằng một cảm biến mà nó đã nắm giữ sẵn — khối lệnh `with` chỉ giải phóng cảm biến mà chính nó đã mở.

Giao diện người dùng (GUI) của Chloros **không** thuộc nhóm này: bảng điều khiển DAQ của nó thiết lập kết nối riêng với cảm biến, do đó `connect_daq_sensor` sẽ không bao giờ mượn cảm biến mà GUI đã mở. Không sử dụng cùng một cảm biến từ GUI và từ tập lệnh cùng một lúc. Một thiết bị DAQ-U hoặc DAQ-M mà GUI đang giữ thường sẽ không mở được, nhưng một DAQ-E có thể chấp nhận phiên thứ hai: lệnh kết nối từ skript sẽ thay đổi cài đặt thu thập dữ liệu của thiết bị dưới sự kiểm soát của GUI, và lệnh `disconnect()` sẽ dừng luồng dữ liệu của thiết bị, điều này sẽ kết thúc chế độ xem trực tiếp của GUI và bất kỳ quá trình ghi hình nào đang diễn ra. Hãy đóng cảm biến trong GUI trước.

### PTP Grandmaster — `time_sync_status()`

Hệ thống nền chạy PTP Grandmaster mà các camera LATTICE và thiết bị DAQ-E lấy dấu thời gian từ đó. Nó khởi động theo yêu cầu: `connect_camera()`, `connect_array()`, `connect_daq_sensor()` và `connect_all()` sẽ khởi động nó nếu nó chưa chạy — bao gồm cả trường hợp kết nối thất bại, chẳng hạn như `connect_all()` cố gắng kết nối với một thiết bị DAQ-E không tồn tại — và các lệnh `test_array_sync()` cùng mọi lệnh mở camera cho một lệnh (`capture_cameras_transient()`, `start_multi_camera_stream()`, các lệnh căn chỉnh, `capture_live_index()`).

- **Nơi nó thông báo.** Trên các bộ điều hợp mạng có dây kết nối trực tiếp với camera (bao gồm địa chỉ link-local `169.254.x.x`), nếu không thì trên bộ điều hợp của tuyến đường mặc định nếu bộ điều hợp đó là có dây. Không bao giờ trên Wi-Fi hoặc bộ điều hợp ảo (Tailscale, WSL): không có camera hay DAQ-E nào nằm phía sau chúng. Nếu không có bộ điều hợp nào như vậy đang hoạt động, nó sẽ chạy chế độ thông báo trên không (`health` `NO_SOCKETS`) và sẽ gán bộ điều hợp khi camera hoặc DAQ tiếp theo kết nối và kích hoạt bộ điều hợp đó.
- **Sau khi mất kết nối.** Khi bộ điều hợp camera mất kết nối và lấy lại được (do rút cáp, thay thế bộ chuyển mạch, bộ điều hợp USB được liệt kê lại), grandmaster sẽ tự động xây dựng lại các socket của mình mà không cần khởi động lại, và kiểm tra lại mọi thiết bị DAQ-E đã kết nối, kích hoạt lại thiết bị bị kẹt do khoảng trống. Nó cũng tự động xây dựng lại khi mọi camera và DAQ-E mà nó đang phục vụ im lặng trong 15 giây.
- **Liệu tín hiệu có đến được dây cáp không?** Xem `time_sync_status()` (`GET /api/time-sync/status`). `state` chỉ cho biết luồng dữ liệu của grandmaster đang chạy; `health` cho biết liệu các gói tin của nó có được gửi đi hay không: `OK` (không phát hiện lỗi), `STOPPED`, `NO_SOCKETS`, `SENDS_FAILING`, `DEGRADED` (một số bộ điều hợp gặp sự cố) hoặc `PEERS_SILENT` (các camera và thiết bị DAQ-E mà nó đang phục vụ đã ngừng phản hồi), kèm theo một thông báo trong `health_detail`. `announce_sent` / `sync_sent` / `delay_resps_sent` chỉ đếm các tin nhắn đã được gửi đi; `announce_send_errors`, `sync_send_errors`, `follow_up_send_errors`, `delay_resp_send_errors`, `last_send_error`, `failing_interfaces` và `last_delay_req_age_s` mô tả các tin nhắn chưa được gửi đi, còn `rebinds` / `last_rebind` là các lần gửi lại. `sync_sends_slow` đếm các Syncs có thời gian gửi vượt quá 200 us và `sync_send_max_us` là trường hợp lâu nhất; thời gian gốc của mỗi Follow_Up chỉ tính tối đa 200 us cho quá trình gửi của nó. Grandmaster chạy trong một tiến trình riêng (`own_process`, `process_pid`), với mức ưu tiên cao trên Windows (`process_priority`), do đó, một backend bận rộn không thể trì hoãn thời gian đóng dấu và đồng bộ hóa đồng hồ của tất cả các camera cùng lúc; nếu tiến trình đó không thể khởi chạy, nó sẽ chạy bên trong backend như trước đây (`own_process` false).

```python
st = chloros_sdk.time_sync_status()
if st.get("health") not in (None, "OK", "STOPPED"):
    print("PTP:", st["health"], "-", st.get("health_detail"))
    chloros_sdk.restart_time_sync()
```

| Chức năng | Tác dụng |
| --- | --- |
| `time_sync_status()` | Trạng thái, tình trạng hoạt động, định danh đồng hồ, mức ưu tiên và bộ đếm tin nhắn của grandmaster (như trên). `chloros-cli time-sync status`. |
| `time_sync_peers()` | Các thiết bị nô lệ PTP mà nó nhận được tín hiệu (máy ảnh, DAQ-E): `peers`, từng `port_identity`, `last_seen_ip`, `last_delay_req_age_ms`, `delay_req_count`. `time-sync peers`. |
| `time_sync_cameras()` | Mỗi camera mảng được kết nối có `PtpStatus`, `PtpOffsetFromMaster` và `PtpMeanPathDelay` (một camera có firmware không có nút PTP chỉ có số sê-ri). `time-sync cameras`. |
| `restart_time_sync()` | Dừng và khởi động lại grandmaster; PTP của mọi DAQ-E trong nhóm sẽ được kiểm tra lại sau đó (`daq_ptp_rechecked`). Gây ra sự kiện `ChlorosConnectError` khi không khôi phục lại. `time-sync restart`. |
| `set_time_sync_priority(priority1=None, priority2=None)` | Các mức ưu tiên PTP của nó (0-255, mức thấp hơn sẽ thắng trong cuộc bầu chọn best-master), dành cho mạng có một máy chủ PTP khác. Yêu cầu máy chủ đó đang chạy. `time-sync set-priority`. |

Mỗi thành phần sẽ lấy `backend_url=` và `timeout=` để khởi động một backend cục bộ chưa đang chạy.

---

## Tiện ích máy ảnh

`chloros-cli lattice` thực hiện những tác vụ gì ngoài việc kết nối và ghi hình, dưới dạng các hàm đơn giản trong `chloros_sdk` (module `chloros_sdk.camera_utils`). Mỗi hàm gọi đường dẫn backend mà lệnh CLI sử dụng, tuân theo các quy tắc của CLI, do đó cả hai hoạt động tương tự nhau. Chúng nhận thông số **số sê-ri** của camera, không phải phiên làm việc, và các từ khóa `backend_url=` / `timeout=` giống như các lệnh smart-connect; một backend không đang chạy sẽ được khởi động, giống như cách `connect_camera()` thực hiện. Nếu bị từ chối, sự kiện `ChlorosConnectError` sẽ được kích hoạt kèm theo lý do từ backend.

```python
import chloros_sdk as c

c.set_camera_color_profile("214701281", profile="natural")
c.set_camera_highlights("214701281", -40)
print(c.camera_firmware_status()["cameras"])
st = c.network_adapters(estimate=True, cameras=4, pixel_format="BayerRG12")
```

| Chức năng | Tác dụng |
| --- | --- |
| `camera_color_profile(serial)` / `set_camera_color_profile(serial, profile=, custom_cct_k=)` | Hồ sơ màu hiển thị: `raw`, `linear`, `natural`, `enhanced` hoặc `custom_temp`, và nhiệt độ màu của nó (được giới hạn trong khoảng 2000–10000 K; được đọc khi cấu hình là `custom_temp`). |
| `camera_color_adjust(serial)` / `set_camera_color_adjust(serial, saturation=, contrast=, reset=False)` | Độ bão hòa và độ tương phản của màn hình, mỗi thông số từ 0 đến 200% (100 = không thay đổi); `reset=True` đặt cả hai thông số này trở lại 100. |
| `camera_highlights(serial)` / `set_camera_highlights(serial, value)` | Giảm độ sáng của các pixel sáng nhất, trắng nhất trên màn hình: -100..0 (được cắt bớt về phía 0, sau đó được giới hạn; 0 = tắt). |
| `camera_white_balance_once(serial, settle_s=None)` | Cân bằng trắng một lần: camera truyền dữ liệu trong thời gian ngắn, hội tụ và trả về `red_ratio` / `blue_ratio` (`ratios_read` False khi không thể báo cáo các giá trị này). Nó giữ nguyên cân bằng trắng đã tính toán; mọi cài đặt khác mà hàm open đã ghi sẽ được trả về nguyên trạng. |
| `set_camera_white_balance(serial, mode=None, red=None, blue=None)` | `mode="auto"` (liên tục) hoặc `"off"`; hoặc các tỷ lệ cân bằng cố định `red` / `blue`, những tỷ lệ này sẽ tắt chế độ tự động trước tiên. Một tỷ lệ mà máy ảnh từ chối được đặt tên là `errors`. |
| `camera_presets()` / `apply_camera_preset(serial, preset)` | Các cài đặt sẵn của máy ảnh, và khi áp dụng một trong số đó: máy ảnh sẽ giữ nguyên các thiết lập theo tên cài đặt sẵn (`settings` trong phản hồi, vì máy ảnh đọc các thông số này sau đó, ví dụ như tốc độ khung hình mà máy ảnh đang chạy; `None` cho trường mà cài đặt sẵn không đặt tên), và mọi cài đặt khác được ghi trong tệp mở sẽ được khôi phục về trạng thái ban đầu. `"default"` đặt lại máy ảnh về cài đặt mặc định ban đầu (cài đặt ban đầu với chế độ phơi sáng tự động chạy tự do, giới hạn thông lượng liên kết và tính năng sửa lỗi bị tắt, cân bằng trắng đã hiệu chuẩn của máy ảnh RGB được giữ nguyên), không được lưu vào bộ nhớ của máy ảnh: phản hồi chứa `factory_defaults: True` và mọi trường của `settings`. Trong `camera_presets()`, `default` là `{factory_defaults: True, description}`. |
| `camera_filter_models()` / `camera_filter_model_info(name)` | Các mô hình bộ lọc và chi tiết của một mô hình (dải sóng, số điều kiện đáp ứng quang phổ). Mô hình bộ lọc của camera được đọc từ chuỗi mô hình của nó; nó không được thiết lập thủ công. |
| `camera_firmware_status(serial=None)` | Phần mềm của từng máy ảnh: `current_version`, `available_version`, `update_offered`, `needs_minimum_update` và `minimum_firmware`. |
| `update_camera_firmware(serial, force=False)` | Nạp phần mềm firmware của bản dựng Chloros này vào một camera: khôi phục cài đặt gốc, nạp firmware, khởi động lại, các thiết lập cơ bản của Chloros được ghi lại; cài đặt hiệu chuẩn của camera được giữ nguyên. Quá trình này mất vài phút; camera phải được cấp nguồn liên tục. `force=True` cài đặt lại firmware cho camera hiện tại. |
| `camera_power_probe(serial)` | Các điểm đo điện áp và nhiệt độ của camera cùng với nhiệt độ của nó. |
| `sleep_camera(serial)` / `wake_camera(serial)` | Chế độ chờ tiết kiệm năng lượng của máy ảnh (máy ảnh sẽ ở trạng thái ngủ cho đến khi được đánh thức: giới hạn thông lượng liên kết được bật (`DeviceLinkThroughputLimitMode` `On`) ở mức tối thiểu, chế độ tiết kiệm năng lượng được bật (nếu camera có), kèm theo nhiệt độ trước và sau; khi được đánh thức, kết nối sẽ được khôi phục về trạng thái như khi camera chuyển sang chế độ ngủ (giới hạn, chế độ và độ trễ gói tin `GevSCPD`, mà camera duy trì ở mức 0 khi giới hạn được bật; dữ liệu này được lưu trữ trên máy tính này, do đó một yêu cầu đánh thức từ hệ thống phía sau sau này cũng sẽ nhận được thông tin này), hoặc, đối với một camera mà máy tính này không đưa vào chế độ ngủ nhưng trông có vẻ đang ngủ (giới hạn của nó được bật ở mức sàn), với giới hạn bị tắt và giá trị `GevSCPD` được để ở 0 tại 80 (cài đặt mặc định); giới hạn được đặt ở mức trên mức tối thiểu sẽ được giữ nguyên. Chỉ trạng thái nguồn được lưu lại; mọi cài đặt khác mà lệnh open ghi vào sẽ được khôi phục về trạng thái ban đầu. |
| `calibrate_camera_with_target(serial, filter=None, attempts=30, settings=None)` | Ghi lại các khung hình cho đến khi mục tiêu hiệu chuẩn MAPIR xuất hiện trong tầm nhìn, và điều chỉnh các hệ số theo từng kênh từ các bảng điều khiển của nó: `target_id`, `panel_dns`, `coefficients`. |
| `network_adapters(estimate=False, cameras=1, pixel_format="BayerRG8")` | Các bộ điều hợp Ethernet của máy tính này dành cho GigE Vision (MTU, tốc độ kết nối, khung jumbo) và bất kỳ `issues`; `estimate=True` bổ sung tốc độ khung hình và băng thông mà các camera `cameras` sẽ nhận được. |
| `fix_network_settings()` | Áp dụng cài đặt bộ điều hợp GigE Vision cho bộ điều hợp camera của máy tính này. Tính năng này thay đổi cài đặt bộ điều hợp và chỉ hoạt động khi Chloros được chạy với quyền Quản trị viên; nếu không, nó sẽ hiển thị nhật ký về những gì đã phát hiện. |
| `apply_vignette_correction(input_dir, output_dir, lens_model=None)` | Chỉnh sửa hiện tượng tối góc (vignette) cho các hình ảnh trong một thư mục, mỗi hình ảnh sử dụng bản đồ trường phẳng riêng của ống kính: `processed`, `lens_models` (ống kính được sử dụng để hiệu chỉnh từng tệp), `skipped` (kèm lý do). Nếu không có `lens_model`, ống kính của từng tệp sẽ được lấy từ trường Model trong EXIF; tệp `lens_model` áp dụng cho các tệp không có tên Model trong thông tin máy ảnh, và tệp nào có thông tin Model mâu thuẫn sẽ bị bỏ qua. Chỉ áp dụng cho tệp. |
| `read_dls(mac, filter=None, camera_serial=None)` | Một giá trị đo từ cảm biến ánh sáng chiếu xuống DLS (Bluetooth): phổ, cường độ bức xạ theo từng dải tần và độ lớn của dải tần thông qua phản ứng riêng của máy ảnh khi được chỉ định `camera_serial`. `discover_daq_sensors(transports=["ble"])` xác định địa chỉ MAC. |
| `probe_system(pixel_format="BayerRG12", discover=True)` / `system_info()` | Phần cứng của máy chủ dành cho camera và cấu hình được đề xuất (`report_text`), cùng với CPU / GPU / môi trường chạy PyTorch của hệ thống. |

- **Một số cần sử dụng camera riêng.** `camera_white_balance_once()`, `apply_camera_preset()`, `sleep_camera()` / `wake_camera()`, `calibrate_camera_with_target()` và `update_camera_firmware()` mở mở camera. Trong khi Chloros đang giữ nó (đã kết nối trong ứng dụng, với `connect_camera()`, là thành viên của mảng, hoặc đang được sử dụng bởi một lệnh khác ngay lúc này, chẳng hạn như kiểm tra mảng hoặc lệnh chụp ảnh đang mở camera của nó), hệ thống phía sau sẽ từ chối với mã trạng thái HTTP 409 `CAMERA_HELD` (cả trong `code` và `error_code`), chỉ rõ cách giải phóng tài nguyên đó. Các thiết lập màu sắc, vùng sáng, `camera_power_probe()` và các lệnh đọc dữ liệu hoạt động bình thường trên camera đã kết nối.
- **Màu sắc, vùng sáng và cân bằng trắng dành cho camera RGB.** Trên camera đơn dải đơn sắc (M3M), các hàm thiết lập không thay đổi gì và trả về `{"skipped": True, "reason": ...}`, giống như CLI. Đây là các cài đặt hiển thị: chế độ xem trực tiếp và ảnh chụp ở mức hiển thị, không bao giờ là độ sáng hay độ phản xạ.

---

## Mảng tạm thời, hồ sơ căn chỉnh & chỉ số

Chức năng của `chloros-cli lattice` khi không có kết nối liên tục, cũng như các hàm trong `chloros_sdk` (module `chloros_sdk.array_utils`): `continuous`, `multi-capture`, `multi-stream`, `multi-test`, `multi-detect`, `analyze-array`, `align-calibrate`, `align-apply`, `align-stream`, `align-info`, `align-reorder` và `index`. Mỗi lệnh sẽ gửi yêu cầu mà lệnh CLI gửi đi, với các giá trị mặc định của CLI.

```python
import chloros_sdk as c

prof = c.calibrate_alignment_profile(["214000535", "214000551", "214000556"],
                                     "D:/rig/alignment_profile.json", method="orb", model="rigid")
print({sn: t["reprojection_error"] for sn, t in prof["transforms"].items()})

c.capture_aligned_multiband("D:/rig/alignment_profile.json", "D:/rig/out")
idx = c.capture_live_index("D:/rig/alignment_profile.json", preset="NDVI",
                           output_dir="D:/rig/ndvi", capture_level="reflectance")

with c.start_multi_camera_stream(["214000535", "214000551"], "D:/rig/stream", fps=5, count=50) as job:
    print(job.wait()["final_stats"])          # waits for the 50 groups, then closes the cameras
```

- **Chúng tự mở các camera.** Mỗi lệnh sẽ mở các camera của mình, thực hiện công việc và đóng chúng lại, do đó cần các camera mà Chloros không đang giữ (đã kết nối trong ứng dụng, với `connect_camera()`, hoặc là thành viên của mảng); hệ thống backend sẽ từ chối với mã trạng thái HTTP 409 `CAMERA_HELD` và hướng dẫn cách giải phóng một camera. Việc mở tạm thời mất một khoảng thời gian (khoảng 40 giây cho vài camera). Đối với việc chụp lặp lại, `connect_array()` thực hiện một lần là phương án nhanh nhất, và nó lưu được mọi định dạng xuất trong khi việc chụp tạm thời chỉ lưu được hình ảnh đã loại bỏ hiệu ứng Bayer. `calibrate_alignment_profile()` là trường hợp ngoại lệ: trên một mảng camera được kết nối, nó xử lý từ chính các khung hình của mảng đó (`used_live_array`).
- **`serials`** là danh sách các camera cần mở, trong đó camera đầu tiên sẽ kích hoạt tín hiệu; `None` mở tất cả các camera trên mạng.
- **`settings`** là một từ điển các thiết lập camera được ghi đè, các trường được thiết lập bởi các cờ của CLI: `exposure_time` (µs), `gain` (dB), `pixel_format`, `width`, `height`, `target_brightness`, `ae_damping`, `ae_upper_limit`, `frame_rate`, `gev_packet_size` và `preset` (một cài đặt sẵn của máy ảnh theo tên). Việc chỉ định giá trị phơi sáng hoặc độ khuếch đại sẽ tắt chế độ điều khiển tự động này, giống như các cờ (flags) làm vậy.
- **`gpu`**: `True` buộc sử dụng GPU warp, `False` buộc sử dụng CPU (`--gpu` / `--no-gpu`); `None` cho phép backend tự chọn.
- **`keep_settings`**/**`clear_settings`** (mọi hàm mở camera, mặc định là `False`): khi kết thúc, mỗi camera sẽ được khôi phục về trạng thái ban đầu; `keep_settings=True` lưu cài đặt của lần chạy vào bộ nhớ riêng của camera (chế độ chạy tự do), `clear_settings=True` đặt lại bộ nhớ đó về cài đặt gốc — xem [Bộ nhớ camera](#camera-memory--keep_settings--clear_settings). Phản hồi (cho một tác vụ, của `stop()`) chứa `camera_memory`.
- **Các đường dẫn là của backend**: các đường dẫn tương đối sẽ được chuyển thành đường dẫn tuyệt đối so với thư mục làm việc của bạn khi backend nằm trên máy này. Các hàm xử lý tệp cấu hình căn chỉnh (`alignment_profile_info()`, `reorder_alignment_profile()`) đọc tệp trên máy tính này, giống như cách CLI thực hiện.

| Chức năng | Tác dụng |
| --- | --- |
| `start_camera_continuous(serial, output_dir="output", *, format="tiff", jpeg_quality=95, queue_depth=64, settings=None)` | Ghi hình liên tục từ một camera cho đến khi dừng lại. Trả về một `CliJob`. Các khung hình trong một lần chạy `tiff` là tệp `raw` chứa mảng dữ liệu ghi lại từ camera đó (một mặt phẳng ghép hình tại cảm biến, không nén); `png` và `jpg` chứa 8 bit, do đó các chuỗi khung hình này là các khung hình đã được khử mô-saic để xem. Một DAQ được gán hoặc gộp sẽ ghi một tệp `.daq` cho mỗi khung hình trong chuỗithư mục `daq/` của đợt ghi (`daq_files` trong thống kê của nó). Mỗi khung hình được ghi một lần kèm theo siêu dữ liệu đầy đủ (Mô hình, thời gian chụp, ống kính, dải phổ, hiệu chuẩn trên khung hình thô); `stop()` sẽ trả về sau khi tất cả các khung hình đã thu được được ghi xong (tối đa 25 giây), và chỉ sau đó máy ảnh mới được tắt. Một máy ảnh được để ở chế độ sẵn sàng cho tín hiệu kích hoạt phần cứng (`TriggerMode` `On`) sẽ được đặt ở chế độ chạy tự do cho lần chạy này (phản hồi khởi động `trigger` sẽ cho biết kết quả phát hiện). Khi dừng, camera sẽ được khôi phục về trạng thái ban đầu — bao gồm cài đặt kích hoạt và mọi cài đặt đã thay đổi trong quá trình chạy — trừ khi có lệnh `keep_settings=True`, lệnh này lưu các cài đặt của lần chạy vào bộ nhớ riêng của camera (chế độ chạy tự do), hoặc `clear_settings=True`, vốn sẽ đặt lại bộ nhớ đó. Một phiên chạy không ghi lại khung hình nào sẽ tự động kết thúc sau 10 giây với các mã `stop_reason` và `no_frames`, cùng lý do được ghi trong `last_error`, và lệnh `stop()` sẽ trả về lý do đó dưới dạng `error`. Các khung hình bị bỏ qua do bộ ghi không kịp xử lý sẽ được thống kê, với `drop_reason` giải thích lý do. Đối với một camera do Chloros quản lý, hãy sử dụng `CameraSession.capture_repeated()`. `lattice continuous`. |
| `start_multi_camera_stream(serials=None, output_dir="output", *, fps=3.0, count=None, format="tiff", jpeg_quality=95, settings=None)` | Truyền các nhóm khung hình đồng bộ tại `fps`, cho các nhóm tại `count` hoặc cho đến khi dừng lại. Trả về một `CliJob` (tham số `start_reply` chứa `n_cams` và đường truyền `pacing`); thống kê cuối cùng của nó bao gồm số nhóm đã lấy, đã ghi và bị bỏ qua, các tệp mà trình ghi không thể ghi và các nhóm mà camera không gửi dữ liệu nào. `lattice multi-stream`. |
| `capture_cameras_transient(serials=None, output_dir="output", *, format="tiff", jpeg_quality=95, settings=None)` | Một lần chụp đồng bộ, một hình ảnh đã khử hiệu ứng Bayer cho mỗi camera: `results` (`filepath`, `serial`, `frame_id`) và `pacing`. Một nhóm thiếu camera sẽ hiển thị thông báo, nêu rõ số lượng tệp đã được lưu trước đó. `lattice multi-capture` khi không có mảng camera nào được kết nối. |
| `test_array_sync(serials=None, count=10, ptp_settle_s=None, timeout=None)` | `count` chụp đồng bộ ở mức phơi sáng cố định: `sync_groups`, `master`, mỗi hàng `group_timestamps` tương ứng với một lần kích hoạt (một camera bỏ lỡ một lần sẽ vắng mặt trong hàng của nó) và `sync`: độ lệch của mỗi máy ảnh so với máy chủ, theo từng lần kích hoạt (`offsets_us`: cho mỗi máy ảnh `mean_us`, `min_us`, `max_us`, `jitter_us`, `groups`), trên MỘT xung đồng hồ (`clock_domain`: `ptp`, hoặc `device` — PTP tắt, mỗi camera sử dụng đồng hồ thời gian hoạt động riêng, do đó độ lệch của nó được xác định khi khởi động và chỉ độ dao động mới đo lường sự đồng bộ). Một tín hiệu kích hoạt có dấu thời gian kết hợp giữa đồng hồ thiết bị và PTP (PTP tắt hoặc vẫn đang khóa) sẽ bị loại trừ và được tính (`groups_mixed_domain`), cũng như trường hợp đồng hồ PTP vẫn chênh lệch hơn `agree_within_us` so với đồng hồ chủ (`groups_clocks_disagree`); `notes` xác nhận điều này. `clock_offset_mean_us` / `clock_offset_jitter_us` là giá trị trung bình và độ dao động lớn nhất trên mỗi camera. `success` là false, cùng với `error`, khi một tín hiệu kích hoạt bị bỏ lỡ, khi có ít hơn hai tín hiệu kích hoạt khiến mọi camera đồng bộ trên cùng một đồng hồ, hoặc khi các đồng hồ không bao giờ khớp (`sync.problems`); các hàng vẫn được trả về trong cả hai trường hợp. Nó khởi động máy chủ chính PTP nếu máy chủ này chưa chạy (`time_sync` cho biết `grandmaster_running` và `grandmaster_started_by_this_test`), bật PTP cho mọi camera (như cách `connect_array()` thực hiện), và trước tín hiệu kích hoạt đầu tiên, hệ thống sẽ chờ cho đến khi đồng hồ PTP của mọi camera đồng bộ với grandmaster và ổn định: `PtpStatus` ở chế độ Slave trên một grandmaster, chênh lệch không quá 100 us so với nó và ổn định trong 3 giây, và mỗi camera đều khóa đồng bộ như vậy, không gián đoạn, trong 120 s — khoảng 2 phút kể từ khi bật nguồn. Một đồng hồ vừa khóa có thể chênh lệch lên đến ~200 us trong khoảng 2 phút sau khi nó đọc trạng thái Slave, và chính `PtpOffsetFromMaster` của camera không hiển thị điều này; 120 giây sẽ đưa các camera vào khoảng cách ~10 us so với đồng hồ đã ổn định. Một camera vẫn đang khóa từ một lần thử nghiệm trước đó mà PTP vẫn được bật (theo cách mà backend này nhận diện), sẽ tính thời gian từ lần khóa đó, do đó, một thử nghiệm ngay sau thử nghiệm khác sẽ không phải chờ đợi thêm; một camera được bật trong thử nghiệm, hoặc một camera được phát hiện đang bật PTP mà hệ thống backend này chưa từng thấy đã khóa (được bật ở nơi khác, hoặc trước khi hệ thống backend khởi động lại), sẽ chờ 120 giây, và trong một mảng bao gồm cả các camera vừa khóa, chúng vẫn tiếp tục chờ thời gian của mình. Một camera rời khỏi chế độ Slave, vượt quá 100 us hoặc theo dõi một grandmaster khác trong 3 lần đọc liên tiếp (1,5 giây) trong thời gian chờ sẽ bắt đầu đếm lại; một hoặc hai lần đọc như vậy sẽ được ghi lại (`[array-sync] PTP SN:<serial> off lock at <s> s (<n> of 3): <status> <±x> us gm <id> -- locked <s> s, counting on`) và quá trình đếm tiếp tục, còn ID grandmaster không thể đọc được sẽ được coi là không xác định, chứ không phải là sự thay đổi. Việc số đếm của mọi camera bắt đầu lại cùng lúc (trong vòng một giây) là do thời gian của Grandmaster thay đổi, không phải của các camera: nhật ký của hệ thống backendghi lại là `[array-sync] PTP grandmaster step: all <n> cameras restarted at <s> s …`, `ptp_lock` liệt kê nó trong `grandmaster_steps`, và thời gian chờ hết hạn sau đó bắt đầu báo lỗi với `every camera's count restarted together at <s> s -- the PTP grandmaster's time moved under them …`. Giới hạn thời gian không được gia hạn cho trường hợp này. `ptp_settle_s` (0-600; `0` nếu không có thời gian chờ ổn định) thiết lập thời gian ổn định cho lần chạy; giá trị khác sẽ bị từ chối (lỗi HTTP400). Thời gian chờ được giới hạn ở thời gian ổn định cộng thêm 90 giây (mặc định là 210 giây, tối thiểu 90 giây), và cuộc gọi sẽ chờ hệ thống nền trong `timeout` giây: mặc định là 240 cộng với thời gian ổn định (360 giây), giống như `chloros-mcp` và `test_array_sync`. `ptp_lock` có `converged`, `waited_s`, `timeout_s`, `settle_s`, `locked_s` (thời gian mỗi camera đã được khóa và ổn định cùng nhau), `found_settled` (các camera đã được khóa trong thời gian ổn định khi được phát hiện) và, cho từng camera, `status`, `offsets_ns`, `slave_s` (thời gian đã khóa; None: chưa khóa) và `remaining_s` (None khi PTP của camera nào cũng không bật). Nhật ký của hệ thống backend ghi lại một dòng mỗi 5 giây, bao gồm trạng thái của từng camera, thời gian đã khóa và thời gian còn lại. Mỗi camera được bật nguồn với chế độ PTP tắt; từ đó, đồng hồ sẽ đồng bộ trong khoảng 10 giây. Sau đó, mỗi camera được đặt lại theo trạng thái mà bài kiểm tra đã phát hiện — chế độ PTP, phơi sáng tự động và độ khuếch đại mà bài kiểm tra giữ ở trạng thái Tắt trong suốt quá trình chạy (`ExposureAuto` / `GainAuto`, và chế độ thủ công `ExposureTime` / `Gain`), và các trường hợp còn lại, trừ khi là `keep_settings=True` — tức là một camera vẫn để chế độ PTP bật (`PtpEnable=True`, hoặc một lần chạy với `keep_settings=True`) sẽ duy trì trạng thái khóa, và một khi backend này đã xác nhận nó ở trạng thái khóa trong thời gian ổn định, các lần kiểm tra sau đó sẽ không chờ đợi nó nữa. Nếu đồng hồ chưa ổn định, `success` sẽ có giá trị false trước bất kỳ tín hiệu kích hoạt nào, và `error` chỉ định cách xử lý: một camera chưa bao giờ khóa (`not_slave`) có nghĩa là grandmaster không thể kết nối với nó (`time_sync_status()`); một camera liên tục bị ngắt kết nối sẽ được đặt tên theo thời gian nó đã ở trạng thái khóa (`remaining_s`); đồng hồ vẫn đang hội tụ (`far`): kiểm tra `time_sync_status()`, và để đảm bảo chúng đã được khóa trước khi kiểm tra, hãy bật PTP và giữ nguyên trạng thái đó (`PtpEnable=True` trên mỗi camera, hoặc `keep_settings=True`; quá trình kiểm tra sẽ khôi phục lại trạng thái ban đầu, do đó chế độ này sẽ vẫn được bật), sau đó chạy kiểm tra. `lattice multi-test`. |
| `detect_array_master(line=None)` | Các camera trên đường truyền đồng bộ và một máy chủ mặc định: `master_serial`, `slave_serials`, các camera được kiểm tra là `line`. `lattice multi-detect`. |
| `analyze_array_models(camera_models=None, *, width=2048, height=1536, n_active=None, force_tier=None, binning=1, packed="auto", pixel_format=None)` | Thật là một loạt các mẫu camera này (tên sản phẩm, `"LATT-M3C-L87-FOCN"`) sẽ xuất hiện trên mạng của máy tính này: `tier`, `achievable_fps_max`, ngân sách cáp (`max_cams_collision_safe`, `oversubscribed`), `cam_caps` và `fix_suggestions`. Theo mặc định, các mẫu camera được phát hiện; không có camera nào được mở. Đối với các camera bạn có, cùng với các cài đặt mà kết nối sẽ sử dụng, hãy xem [`analyze_array_network()`](#network-analysis-helper). `lattice analyze-array`. |
| `calibrate_alignment_profile(serials=None, save_path="alignment_profile.json", *, frames=3, method="orb", model="rigid", reference_serial=None, checkerboard=None, control_points=None, preview=False, vignette=False, roi=None, mask_path=None, rms_threshold_px=None, per_cam_overrides=None, settings=None, ...)` | Xác định cấu hình căn chỉnh từ các khung hình trực tiếp và lưu lại. `method`: `orb`, `akaze`, `phase`, `checkerboard` (cùng với `checkerboard="9x6"`, các góc trong) hoặc `manual` (cùng với `control_points={serial: [[x, y], ...]}`, cả camera tham chiếu nữa). `model`: `translation`, `rigid`, `affine` hoặc `homography`. Các tùy chọn chi tiết hơn là các cờ của CLI: `max_features`, `ratio_threshold`, `matcher` (`bf` / `flann`), `knn_k`, `prefilter` (`none` / `gradient` / `clahe` / `blur` / `hist_match`), `ransac_threshold_px`, `ransac_max_iters`, `ransac_confidence`, `averaging`, `lock_rotation`, `lock_scale`, `lock_axis`; `per_cam_overrides` là `{serial: {option: value}}`. `rms_threshold_px` từ chối lưu giải pháp kém hơn. Trả về `save_path`, `output_width` / `output_height`, `crop_roi`, `reference_serial`, `transforms` (cho mỗi camera là `reprojection_error`, `num_matches`) và `preview_path`. `lattice align-calibrate`. |
| `capture_aligned_multiband(profile_path, output_dir="output", *, serials=None, format="tiff", bit_depth=None, gpu=None, no_crop=False, bands=None, order=None, vignette=False, per_band=False, per_camera=False, interpolation="linear", border_mode="constant", border_value=0, settings=None)` | Một hình ảnh đa dải được căn chỉnh theo cấu hình: `files` (tệp TIFF đa dải kèm theo `bands` và kích thước, cộng thêm một tệp cho mỗi dải với `per_band` và một tệp cho mỗi camera với `per_camera`) và `vignette_corrected`. `bands` chọn và `order` sắp xếp các dải theo tên (dưới dạng danh sách hoặc chuỗi có dấu phẩy). `lattice align-apply`. |
| `start_aligned_stream(profile_path, output_dir="output", *, serials=None, fps=3.0, count=None, ...)` | Hình ảnh đa dải được căn chỉnh theo luồng (các tùy chọn căn chỉnh giống như `capture_aligned_multiband()`), cho đến khi đạt `count` hoặc cho đến khi dừng lại. Trả về một `CliJob`; số liệu thống kê cuối cùng của nó là `frames_written` và bất kỳ `frame_groups_incomplete` nào. `lattice align-stream`. |
| `alignment_profile_info(path)` | Một tệp cấu hình được tóm tắt dưới dạng `align-info` sẽ in ra: `name`, `created`, `method`, `reference_serial`, `sensor_resolution`, `output_width` / `output_height`, `crop_roi`, `camera_serials`, `transforms` (cho mỗi camera `model`, `reprojection_error`, `num_matches`, `reference`), `camera_models`, `layer_order` (cho mỗi camera: `band_names`, `wavelengths_nm`, `enabled`); `profile` là toàn bộ tệp. |
| `reorder_alignment_profile(path, order=None, enable=None, disable=None)` | Thay đổi tệp cấu hình tại chỗ: `order` là tên các dải (mỗi dải di chuyển lớp của camera tương ứng lên trên, theo thứ tự đó), `enable` / `disable` là các số sê-ri. Trả về tệp mới `layer_order`. `lattice align-reorder`. |
| `index_catalog()` | Các cài đặt sẵn chỉ mục (`presets`: lần lượt là `name`, `channels`, `formula`) và màu `gradients`. `lattice index --list-presets` / `--list-gradients`. |
| `compute_index(input_path, *, preset=None, formula=None, channel_map=None, output=None, capture_level="unknown", output_format="all", colorize=False, gradient="RdYlGn", vmin=None, vmax=None, percentile=None, bg_mode="clip")` | Chỉ mục trên tệp TIFF đa dải được căn chỉnh: một `preset` (`NDVI`, `NDRE`, ...) hoặc một `formula` trên các ký hiệu `channel_map` được ánh xạ sang các dải (`{"NIR": "NIR_850", "Red": 0}`, theo tên hoặc chỉ mục). Một cài đặt sẵn'biểu tượng `channel_map` sẽ loại bỏ dải duy nhất trong tệp có tên bắt đầu bằng nó (`red` -> `Red_660`, `nir` → `NIR_850_RGN`), được đặt tên bởi chính tệp đó (danh sách dải của nó, hoặc `Chloros:Band{R,G,B}Name` của bản chụp/sản phẩm Chloros), vì vậy NDVI trên một bản chụp phản xạ Chloros không cần `channel_map`; hai dải phổ tương ứng với một ký hiệu sẽ bị từ chối, không được đoán. `capture_level` mô tả nội dung của các dải phổ: `reflectance` hoặc `radiance` chuyển các giá trị thành định lượng; một bộ dữ liệu chưa được hiệu chuẩn (của `align-apply`) cung cấp các giá trị định tính; `"unknown"` đọc dữ liệu từ tệp, bao gồm `Chloros:ProcessingLevel` trong tệp Chloros. Trả về `stats` (hình dạng, phạm vi phần trăm từ 1 đến 99, phạm vi LUT, `capture_level` được sử dụng) và `files` được ghi bên cạnh đầu vào (`<input stem>_<preset>.tif`, `..._LUT.png`) hoặc tại `output`, một đường dẫn: với phần mở rộng hình ảnh, tên tệp được đặt dựa trên phần gốc (`output="D:/out/ndvi_FRGN.png"` sẽ tạo ra `ndvi_FRGN.tif` và `ndvi_FRGN_LUT.png`), nếu không có phần mở rộng thì đó là tiền tố (`D:/out/ndvi` → `ndvi.tif`, `ndvi_LUT.png`); đọc các đường dẫn từ `files`. Chúng chứa thông tin ghi nhận từ máy ảnh của dữ liệu đầu vào (Hãng sản xuất / Mẫu máy / Số sê-ri, GPS, ống kính, thời gian chụp theo UTC, mã định danh hiệu chuẩn nhưng không bao giờ bao gồm hiệu chuẩn raw-DN), `Chloros:ProcessingLevel=index`, và chỉ số làm dải duy nhất của chúng (`XMP-Camera:BandName`, cả tệp TIFF và tệp LUT PNG đều như vậy). Chỉ tệp; không có thông tin máy ảnh. `lattice index --input`. |
| `capture_live_index(profile_path, *, preset=None, formula=None, channel_map=None, serials=None, output_dir="output", output_prefix="index", capture_level="reflectance", save_multiband=False, ...)` | Chụp, hiệu chuẩn (`reflectance` hoặc `radiance`; `raw` bỏ qua bước này), căn chỉnh và tính toán chỉ số trong một bước; `save_multiband=True` giữ lại chồng ảnh đã hiệu chỉnh, mà `compute_index()` có thể sử dụng sau này. Trả về `stats` (kèm theo `serials` chứa các dải tần đã sử dụng) và `files`. Nó mở `serials` (thiết bị đầu tiên kích hoạt tín hiệu), nếu không thì các camera trong cấu hình có mục `layer_order` được kích hoạt, camera tham chiếu được ưu tiên trước (được mở ngay cả khi các dải tần của nó bị vô hiệu hóa); một camera có các dải tần bị vô hiệu hóa sẽ không bao giờ được hiệu chuẩn. `channel_map` chỉ cần thiết cho một biểu tượng mà ngăn xếp không thể trả lời, như trong `compute_index()`: biểu tượng của một cài đặt sẵn sẽ lấy dải tần duy nhất được kích hoạt có tên bắt đầu bằng biểu tượng đó, được kiểm tra trước khi bất kỳ camera nào được mở (trường hợp không có hoặc có nhiều camera đáp ứng sẽ bị từ chối, đồng thời liệt kê tên của chúng); một `formula` tùy chỉnh cần có cho mỗi biểu tượng. `index.tif` và LUT chứa thông tin về máy ảnh trong ngăn xếp, thời gian chụp (thời gian phơi sáng của máy ảnh tham chiếu, theo UTC) và ống kính, và cả hai đều đặt tên chỉ mục làm dải duy nhất của chúng (`XMP-Camera:BandName`, ví dụ `NDVI`: tệp PNG màu chỉ định chỉ số, không phải các kênh màu của nó). `lattice index --live`. |

### `CliJob`

Tham chiếu mà ba luồng trả về. Công việc chạy trên nền, nơi quản lý các camera của nó cho đến khi bị dừng lại.

| Thành phần | Mô tả |
| --- | --- |
| `stats(timeout=10.0)` | `running`, `state`, `stats` (khung hình đã lưu / đã ghi, bị bỏ qua, `fps`, `elapsed`, lỗi ghi, `stop_reason`; đối với tác vụ liên tục còn có `frames_grabbed` và `frames_queued` -- mỗi khung hình được thu thập đều được ghi, xếp hàng, bị loại bỏ hoặc thất bại -- và `drop_reason`, `writers` và `write_ms`, thời gian trung bình để ghi một khung hình) và `output_dir`. `state` là `running`, `ended` (nó tự dừng và chưa bị dừng), `stopping` (một quá trình dừng đang ghi dữ liệu từ hàng đợi: thống kê thời gian thực, và `stopped_by`) hoặc `finished` (thống kê cuối cùng của nó, các giá trị `error` và `stopped_by` của quá trình dừng). Bản ghi của một tác vụ đã dừng được lưu giữ trong 10 phút (tối đa 32 bản ghi gần nhất), đối với mỗi khách hàng: một skript khác hoặc `chloros-mcp` sẽ đọc cùng trạng thái và thống kê cuối cùng. Một tác vụ `chloros-cli` đã được khởi chạy (`lattice continuous`, `multi-stream`, `align-stream`) sẽ bị hệ thống backend dừng lại khi CLI ngừng gửi báo cáo trong 30 giây — bị ngắt, máy của nó đang ở chế độ ngủ — với các tiến trình `stop_reason`, `client_gone` và `stopped_by` trên hệ thống backend; việc đọc số liệu thống kê từ đây không giúp duy trì trạng thái hoạt động của nó. Một tác vụ được khởi chạy từ SDK sẽ chạy cho đến khi bị dừng. Mỗi lần khởi động sẽ gửi `timeout` làm thời hạn của hệ thống nền (`client_deadline_s`): một camera mở sau khi nó (vẫn đang khởi động) được đưa trở lại và đóng lại, và không có tác vụ nào được khởi chạy, và cuộc gọi này sẽ phát sinh giá trị `ChlorosConnectError` (`CLIENT_DEADLINE_PASSED`). Một camera đang được một tác vụ giữ sẽ bị từ chối khi có bất kỳ lần mở nào khác (`connect_camera()`, một lần khởi động khác, một lần chụp tạm thời) với mã lỗi HTTP 409 `CAMERA_HELD`, nêu rõ tên tác vụ và người đã khởi chạy nó. |
| `stop(timeout=30.0)` | Dừng tác vụ và đóng các camera của nó: `final_stats`, `output_dir` và `error` khi quá trình dừng không diễn ra suôn sẻ. Lệnh gọi thứ hai trả về cùng kết quả, và việc dừng từ một client khác cũng vậy: lệnh được gửi trong khi client đầu tiên vẫn đang ghi dữ liệu vào hàng đợi sẽ chờ nó (tối đa 25 giây; sau thời gian đó, lỗi sẽ được báo cáo, và thống kê cuối cùng sẽ xuất hiện trên `stats()`). |
| `wait(timeout=None, poll_s=0.5)` | Chờ cho đến khi nó tự kết thúc (đạt đến `count`, ổ đĩa đầy, hoặc xảy ra lỗi), sau đó `stop()` để thu thập số liệu cuối cùng. `timeout` gây ra sự kiện `TimeoutError` trong khi quá trình vẫn đang chạy; một tác vụ không có `count` chỉ kết thúc khi bị dừng. |
| `job_id`, `kind`, `output_dir`, `start_reply`, `stopped` | ID và loại của nó (`continuous`, `multi-stream`, `align-stream`), vị trí ghi dữ liệu, phản hồi từ tuyến bắt đầu và trạng thái đã chạy hay chưa của `stop()`. |

Sử dụng nó như một trình quản lý bối cảnh để dừng nó khi thoát khỏi phạm vi.

---

## Thư mục dự án & Công thức

**Thư mục dự án** là thư mục dùng để tạo các dự án mới, được chia sẻ giữa ứng dụng Chloros, giao diện dòng lệnh (CLI) và bộ công cụ phát triển phần mềm (SDK) (module `chloros_sdk.system_utils`; `chloros-cli project-folder`):

```python
print(chloros_sdk.project_folder())                # the configured folder, else the default
chloros_sdk.set_project_folder("D:/Flights")       # created if missing
chloros_sdk.reset_project_folder()                 # -> {"path": <default>, "was_set": True}
```

Nếu thư mục đã được cấu hình không còn tồn tại, hệ thống sẽ tự động chuyển sang thư mục mặc định (`Documents/MAPIR/Chloros Projects`), giống như ứng dụng. Đường dẫn tương đối được chỉ định cho `set_project_folder()` sẽ được lấy từ thư mục làm việc của bạn khi máy chủ phía sau (backend) nằm trên máy này.

**Một công thức** là danh sách hành động YAML / JSON mà `chloros-cli project run PATH RECIPE` thực thi trên một dự án đã lưu. `run_project_recipe()` thực thi nó bằng trình chạy riêng của CLI(được cài đặt cùng với SDK), do đó một công thức sẽ hoạt động giống nhau khi được thực thi từ:

```python
r = chloros_sdk.run_project_recipe("D:/campaign/morning.yaml", "D:/Projects/Field_A")
print(r["ok"], r["actions"])
print(r["log"])                                    # what the CLI would have printed

chloros_sdk.run_project_recipe({"actions": [{"apply": {"name": "FrontLeft", "settings": {"exposure_us": 5000}}},
                                            {"wait": "2s"},
                                            {"capture": {"name": "FrontLeft", "output": "pose_a/", "format": "tiff"}}]},
                               "D:/Projects/Field_A")
```

```python
run_project_recipe(recipe, project_path, *, dry_run=False, echo=False,
                   backend_url="http://127.0.0.1:5000") -> dict
```

- **`recipe`** là một tệp `.yaml` / `.yml` / `.json` hoặc chính công thức đó: một từ điển chứa `actions` (`apply`, `capture`, `burst`, `stream`, `sensor`, `wait`), và tùy chọn `overrides` và `stop_on_error` (mặc định là true: quá trình thực thi sẽ dừng lại tại hành động đầu tiên gặp lỗi). Các hành động được định nghĩa theo [Recipe DSL](cli-reference.md#recipe-dsl) trong tài liệu tham khảo CLI; đường dẫn tương đối `output` sẽ được giải quyết dựa trên thư mục làm việc của skript.
- **Trả về** `ok` (tất cả các hành động đều thành công), `actions` (số lượng), `log` (đầu ra của trình chạy; `echo=True` cũng in ra nội dung này trong quá trình chạy) và `validation`. Tệp công thức bị thiếu hoặc không thể phân tích cú pháp sẽ gây ra lỗi `ChlorosConnectError`.
- **Được kiểm tra trước khi chạy.**`validation` là `{"ok", "errors", "warnings", "steps"}`: mỗi hành động phải có chính xác một động từ đã biết làm khóa (`{"capture": {"name": ...}}`, không phải `{"action": "capture"}` hay `{"type": "capture"}`) chỉ định một camera, mảng hoặc cảm biến mà dự án có, thuộc loại phù hợp. Một công thức có lỗi sẽ**không** thực thi bất kỳ hành động nào (`ok` false, các lỗi trong `log`), bất kể `stop_on_error` nói gì. Các khóa không được sử dụng, và mọi mảng hoặc cảm biến ánh sáng mà công thức điều khiển nhưng hiện không được kết nối, đều thuộc trường hợp `warnings`. Một công thức không kết nối thiết bị: hãy kết nối thiết bị đầu tiên của dự án (`ChlorosProject.connect_all()`, `chloros-cli project connect`, hoặc công cụ MCP `connect_project_devices`). `dry_run=True` không chạy bất kỳ thứ gì và trả về `{"recipe": ..., "validation": ...}`.
- Mỗi lần chỉ có một công thức được thực thi trong một quy trình. Một công thức YAML cần PyYAML.

---

## Tự động hóa dự án — `ChlorosProject`

Một dự án Chloros đã lưu là một thư mục chứa `cameras.json` + `sensors.json` + `project.json`. `open_project(path, relink=None)` tải tệp manifest (`relink`: thư mục chứa các tệp nguồn bị thiếu của dự án đã di chuyển, được sử dụng khi phần nền tải nó -- `process()` -- xem **Dự án đã di chuyển** trong phần `ChlorosLocal`), và `connect_all` đưa mọi camera, mảng cảm biến và cảm biến ánh sáng đã lưu vào trạng thái hoạt động cùng với các cài đặt đã lưu — trạng thái phần cứng giống như giao diện người dùng (GUI) sẽ tạo ra. Các cảm biến ánh sáng kết nối thông qua [bộ cảm biến DAQ](#daq-sensor-sessions), do đó chúng cần backend đang chạy (xem [`SensorHandle`](#sensorhandle)).

### Ví dụ tối thiểu

```python
import chloros_sdk

proj = chloros_sdk.open_project("/home/user/Chloros Projects/Field_A")
report = proj.connect_all(verbose=True)
print(report)  # {'cameras': {...}, 'arrays': {...}, 'sensors': {...}}

# Cameras and arrays are addressable by name OR serial / array_id
cam = proj.cameras["FrontLeft"]
cam.capture("./out", format="tiff", processing="reflectance")

arr = proj.arrays["main_rig"]
arr.capture("./out", format="tiff", processing="reflectance")

# Light sensors are addressable by name OR slot_id
reading = proj.sensors["Sky"].read()   # (spectrum, is_saturated, ...) or None

# One frame from every camera and array, one spectrum from every sensor
proj.capture_all("./out")

proj.disconnect_all()
```

Hoặc dưới dạng trình quản lý bối cảnh:

```python
with chloros_sdk.open_project("/path/to/proj") as proj:
    proj.connect_all()
    proj.arrays["main_rig"].capture("./out", processing="reflectance")
```

### Các phương thức `ChlorosProject`

| Phương thức | Mô tả |
| --- | --- |
| `connect_all(*, cameras=True, arrays=True, sensors=True, verbose=False, align=None, packed=None)` | Phát hiện và kết nối mọi camera, mảng và cảm biến ánh sáng đã lưu. Trả về báo cáo kết nối theo từng lớp (`{"cameras": {...}, "arrays": {...}, "sensors": {...}}`, mỗi báo cáo đi kèm với `connected`, `missing` và `errors`; `arrays` cũng hỗ trợ `alignment`). Với một backend đang lắng nghe trên `127.0.0.1:5000`, các camera và mảng đã lưu sẽ được tìm thấy và kết nối thông qua nó. Nếu không có, quá trình này sẽ tự tìm và điều khiển chúng thông qua `lattice_sdk`, yêu cầu môi trường chạy Arena SDK và OpenCV (`cv2`) — gói cài đặt SDK không cài đặt cả hai; khi không thể cài đặt, mọi camera và mảng đã lưu sẽ được liệt kê dưới `errors` kèm lý do, và `missing` sẽ trống. Một backend đang chạy nhưng từ chối SDK (đã đăng xuất: HTTP 401; một kế hoạch dưới Copper: 403) không được coi là "không có backend": mọi camera, mảng và cảm biến ánh sáng đã lưu đều được liệt kê dưới `errors` kèm theo lý do từ backend, và không có gì được mở trong quá trình này. Mỗi lần gọi đều kiểm tra lại máy chủ phía sau, do đó `connect_all()` sau khi đăng nhập hoặc sau khi khởi động Chloros sẽ kết nối với cùng một đối tượng dự án. Một mảng đã được lưu và đang hoạt động trên backend với cùng các camera (của GUI hoặc của `chloros-cli project connect`) sẽ được gắn vào nguyên trạng, mà không áp dụng lại các cài đặt đã lưu, và `disconnect_all()` để nó tiếp tục chạy. Các cảm biến ánh sáng luôn đi qua backend (xem [`SensorHandle`](#sensorhandle)). Nó không bao giờ khởi tạo một backend mới. |
| `disconnect_all()` | Ngừng hoạt động mọi thứ. Mỗi thiết bị sẽ được giải phóng trước; sau đó, nếu phần nền không xác nhận việc giải phóng camera, mảng hoặc cảm biến ánh sáng (quá trình ngừng hoạt động hết thời gian chờ hoặc bị từ chối), một `ChlorosConnectError` sẽ chỉ định tên cho từng thiết bị đó. Một thiết bị đã bị ngắt kết nối, hoặc không có backend đang chạy, sẽ không có phản hồi. Mảng hoặc cảm biến ánh sáng mà một client khác đã mở sẽ tiếp tục chạy. Lệnh thoát `with` của dự án sẽ không bao giờ được kích hoạt. |
| `capture_all(output_dir=".")` | Một khung hình từ mỗi camera và mảng đã kết nối, và một phổ (`SensorHandle.read()`) từ mỗi cảm biến ánh sáng đã kết nối, được thu thập lần lượt (trước tiên là camera, sau đó là mảng, rồi đến cảm biến), không phải cùng một lúc. |
| `stream(camera, overlays=False, fps=10.0)` | Bộ tạo khung hình BGR `numpy` từ một camera (hoặc mảng) được chỉ định. `overlays=False` đọc các khung hình bằng handle'`grab()` ở cấp độ `debayered`: qua backend khi dự án kết nối qua một backend, nếu không thì là vòng lặp lấy dữ liệu trực tiếp `lattice_sdk` (mảng tạo ra các từ điển `{serial: frame}`). `overlays=True` định tuyến qua `ChlorosLocal.camera_stream()` → luồng MJPEG `/api/camera/<serial>/stream-annotated` của backend, với khối `ui.overlay` đã lưu của camera được truyền qua dưới dạng tham số truy vấn. Yêu cầu chế độ backend, OpenCV (`cv2`) và một **camera độc lập**: một camera ở chế độ trực tiếp sẽ gây ra lỗi `RuntimeError` (backend không thể lấy camera mà quá trình này sở hữu) và một mảng sẽ gây ra lỗi `NotImplementedError` (tạo lớp phủ tổng hợp cho từng camera — truyền luồng thành viên theo tên). Tương đương với lệnh một lần: `CameraHandle.capture(annotated=True)`. |
| `align_arrays(align=True, verbose=False)` | Chạy căn chỉnh trên mọi mảng hiện đang được kết nối. |
| `process(mode="parallel", wait=True, progress_callback=None, poll_interval=2.0)` | Chạy quy trình hiệu chỉnh / lập chỉ mục trên các hình ảnh của dự án (bao gồm `ChlorosLocal.process`; bốn tham số này là **duy nhất** được chấp nhận — `indices=` v.v. gây ra lỗi `TypeError`; thiết lập chỉ số thông qua `ChlorosLocal.configure()`). Tạo một đối tượng `ChlorosLocal()` theo cách lười biếng, đối tượng này sẽ tự động khởi động một backend. |

Thuộc tính:
- `proj.cameras` — `Dict[str, CameraHandle]` được lập chỉ mục theo tên VÀ số sê-ri.
- `proj.arrays` — `Dict[str, ArrayHandle]` được lập chỉ mục theo tên VÀ array_id.
- `proj.sensors` — `Dict[str, SensorHandle]` được lập chỉ mục theo tên VÀ slot_id.
- `proj.config` — `project.json["config"]` (danh sách).

### `CameraHandle`

```python
cam = proj.cameras["FrontLeft"]

# Save a frame to disk (processing-aware)
filepath = cam.capture(
    output_dir="./out",
    format="tiff",
    processing="radiance",           # see the level table below
    apply_calibration=True,          # DSNU + flat + 3x3 unmix + NIST
    apply_white_balance=True,        # DLS-aware WB
    apply_index=False,
    index_expression=None,
)

# In-memory grab (numpy array)
frame = cam.grab(processing="debayered")
frame, header = cam.grab(processing="radiance", with_metadata=True)

# Frame iterator (generator)
for arr in cam.frame_stream(processing="debayered", fps=5, count=100):
    my_analysis(arr)
```

**Các cấp độ xử lý.** `capture()`, `grab()` và `frame_stream()` đều sử dụng cùng một mã thông báo `processing`, và chuỗi này là tích lũy — mỗi cấp độ sẽ chạy tất cả các cấp độ ở trên nó:

| Cấp độ | Đầu ra | Ghi chú |
| --- | --- | --- |
| `raw` | Bayer 1 kênh, định dạng gốc của cảm biến | Không có quá trình khử mô-saic. Các lớp phủ không khả dụng ở cấp độ này. |
| `debayered` | Màu 3 kênh (**mặc định** cho `CameraHandle` và mọi `grab()` / `frame_stream()`; `ArrayHandle.capture()`, `capture_smart()` và `stream()` mặc định là `raw`): BGR trong bộ nhớ (`grab()` / `frame_stream()`), được lưu dưới dạng RGB bởi `capture()` | Trong chế độ backend, khi chụp ảnh: độ lệch tối (DSNU) của gói hiệu chuẩn được trừ khỏi dữ liệu thô, sau đó là thuật toán khử mô-saic nhận diện cạnh của OpenCV — không sử dụng bảng tra tuyến tính (LUT), hiệu chỉnh trường phẳng hoặc tách kênh. Cấp độ duy nhất hoạt động mà không cần chế độ backend là thuật toán khử mô-saic tuyến tính đơn thuần của thư viện máy ảnh (không có độ lệch tối). Tệp TIFF được lưu là RGB quang độ, do đó hãy đảo ngược các kênh của khung hình trong bộ nhớ (`frame[..., ::-1]`) để so sánh với tệp. |
| `radiance` | float32, W/m²/sr/nm, **một mặt phẳng cho mỗi dải** | Chuỗi đo quang học đầy đủ: khử mô-saic + tách màu 3×3 (đa phổ) + DSNU + hiệu chỉnh trường phẳng + quy mô NIST, với phơi sáng × độ khuếch đại được loại bỏ để các giá trị là tuyệt đối. |
| `reflectance` | uint16, 32768 = 1.0 (hoặc uint8, 255 = 1.0 — xem `bit_depth`), **một mặt phẳng cho mỗi dải** | Độ rạng rỡ chia cho bức xạ xuống (ρ = π·L/E). Cần có chỉ số DLS/DAQ — xem ghi chú bên dưới. |
| `display` | 8-bit tương tự sRGB | Hiển thị tương đương giao diện người dùng: CCM + cân bằng trắng + gamma thông qua cấu hình màu đang hoạt động của máy ảnh. |

Chỉ trong chế độ trực tiếp (không qua backend), chỉ có `debayered` khả dụng: `grab()` ở mức khác sẽ kích hoạt
`NotImplementedError`, `capture()` bỏ qua `processing`, và `frame_stream()` nâng
`RuntimeError` ở mọi cấp độ. `reflectance` cần một giá trị đo hướng xuống có thể sử dụng — điểm cuối khung hình tự động kéo
dữ liệu DAQ đã gộp vào khe DLS của camera, nhưng nếu không có DAQ nào được liên kết, chuỗi sẽ từ chối
lối ra phản xạ và ghi rõ việc hạ cấp trong metadata trả về thay vì im lặng
trả về một sản phẩm có chất lượng thấp hơn.

> **Thang đo DN phản xạ — không nên mã hóa cứng.** Độ phản xạ LATTICE sử dụng `32768` = ρ 1.0 và ghi
> XMP `Chloros:PixelScale=32768`; độ phản xạ Survey3 sử dụng `65535` = ρ 1.0 và không chứa
> thẻ `Chloros:*`. Đọc giá trị thẻ và chia cho nó. Giá trị này được định nghĩa trong miền uint16, do đó nó vẫn giữ nguyên
> giá trị `32768` cho mọi định dạng thực hiện quy đổi tỷ lệ (TIFF 16-bit, TIFF 8-bit, PNG/JPG 8-bit, phần trăm 32-bit)
> — trước tiên hãy chuẩn hóa kiểu dữ liệu được lưu trữ trở lại uint16 (nhân với 257 từ 8-bit, nhân với 65535 từ float). Trường hợp
> ngoại lệ duy nhất là tệp từ phiên bản trước 1.3.0: lúc đó tệp TIFF 8-bit bị *cắt bớt* chứ không phải điều chỉnh tỷ lệ, do đó
> không có thông số tỷ lệ nào mô tả nó, và nó không chứa bộ tuple `PixelScale` hoặc MicaSense. Xử lý trường hợp thiếu thẻ trên
> tệp phản xạ LATTICE như "không có tỷ lệ hợp lệ", chứ không phải là giá trị mặc định.

**Thẻ hiệu chuẩn: chỉ trên `raw`.** Tệp `raw` của một lần chụp chứa gói hiệu chuẩn của máy ảnh: dữ liệu tải (payload) `CalibrationJSON` và các thẻ tương thích với MicaSense (`BlackLevel`, `RadiometricCalibration`, `Band{R,G,B}RadiometricCalibration`, `MicaSenseVignetting*`), mô tả các giá trị thô của cảm biến. Các tệp ở các cấp độ khác (`debayered`, `display`, `radiance`, `reflectance`, bản hiển thị `index/`) và mỗi khung hình ghi liên tục / luồng đa camera đều chứa thông tin nhận dạng của camera (`XMP-mapir:CameraSerial`, `CameraModel`, `FilterType`, `LensModel`, `CalBundleSha256`, ...) và thông tin ghi hình riêng của khung hình, `XMP-mapir:FrameJSON` (thời gian phơi sáng, độ khuếch đại, nhiệt độ, định dạng pixel, thời gian chụp, khớp DAQ), vì mỗi sản phẩm `process()` đều mang mã định danh này. Quá trình xử lý sẽ tìm kiếm thông tin hiệu chuẩn của tệp đó dựa trên số sê-ri và `CalBundleSha256` trong bộ nhớ đệm hiệu chuẩn củChloros, đồng thời tải xuống gói dữ liệu một lần nếu máy tính này chưa có.

#### Tệp được tạo từ nhiều máy ảnh

Một tệp được tạo từ các dải phổ của nhiều máy ảnh — một chồng dải phổ đa kênh (`capture_aligned_multiband()`, `start_aligned_stream()`, `capture_live_index(save_multiband=True)`, `Registration.save_multiband()`), một chỉ mục trực tiếp được tạo từ nhiều máy ảnh (`index.tif`, `index_LUT.png`), hình ảnh `composite/` từ việc chụp mảng — liệt kê tên từng máy ảnh đã được sử dụng:

- **Theo-danh sách theo từng camera, theo thứ tự dải tần** (hình ảnh tổng hợp: lớp nền, sau đó là lớp tiền cảnh; một lớp chỉ mục, các camera cung cấp dải màu tương ứng): `XMP-mapir:SourceCameraSerials`, `SourceCameraModels`, `SourceSensorProfiles`, `SourceProductLines`, `SourceFilterTypes`, `SourceLensModels`, `SourceCalDateUTCs`, `SourceCalBundleSha256s` (gói hiệu chuẩn mà các dải của từng camera đã trải qua, nếu không — một bộ `capture_aligned_multiband()` / `start_aligned_stream()`, các dải của bộ này không trải qua gói nào — gói mà Chloros đã lưu vào bộ nhớ đệm cho camera, gói có tên trùng với tên tệp ghi hình của camera), `SourceLightboxCertIds`, `SourceHalogenLampCertIds`, `SourcePackTiers` — các danh sách song song, mục *n* của mỗi danh sách mô tả cùng một máy ảnh; `-` là trường hợp máy ảnh không có giá trị (không có gói hiệu chuẩn, ống kính không xác định). Mỗi máy ảnh là **một** mục: khi gói dữ liệu của nó có nhiều giá trị cho một trường — các chứng chỉ đèn halogen của gói dữ liệu — chúng được nối bằng `;` (`F-1479-1;F-1480-1;F-1481-1`), không bao giờ dùng dấu phẩy, do đó danh sách hiển thị một mục cho mỗi máy ảnh dù được in ra như thế nào.
- **EXIF `SerialNumber`** chứa các số sê-ri giống nhau, được nối bằng dấu phẩy (`214000535,214000551`).
- Nó **không** chứa thông tin nhận dạng riêng lẻ của từng camera (`XMP-mapir:CameraSerial`, `CalBundleSha256`, …): những thông tin này chỉ định một camera và gói dữ liệu của nó, và quá trình xử lý sẽ xác định thông số hiệu chuẩn của tệp từ các thông tin đó. Một chỉ số được tính toán từ các dải dữ liệu của nhiều camera (`index.tif`, `index_LUT.png`) có EXIF `Model` `LATTICE` (bộ thiết bị), chứ không phải mô hình `LATT-…` của một máy ảnh duy nhất.

Một tệp chứa các dải màu của **một** máy ảnh — tệp TIFF cho từng dải màu, khung hình `capture_aligned_multiband(per_camera=True)`, tập hợp hoặc chỉ mục trực tiếp của một máy ảnh duy nhất — mang các thẻ nhận dạng riêng của máy ảnh đó, giống như mọi tệp ghi hình khác. Bộ xếp chồng của `capture_aligned_multiband()`, các tệp theo dải và theo camera, cùng mỗi bộ xếp chồng `start_aligned_stream()` và các tệp theo dải của nó, đều chứa thời gian ghi hình của nhóm (thời gian phơi sáng của camera tham chiếu, theo UTC), và khung hình `per_camera` của camera đơn sắc là một mặt phẳng, tại `bit_depth` giống như bộ xếp chồng.**Hướng của camera (lật / xoay).** Các khung hình từ `grab()` / `frame_stream()` luôn ở
hướng gốc của máy ảnh, ở mọi cấp độ. Các bản chụp được lưu sẽ tuân theo cài đặt hướng
của máy ảnh (các tùy chọn Gương ngang / Gương dọc / xoay trên giao diện người dùng):

- **Các tệp `raw` và `debayered` chính xác là những gì cảm biến đọc được**, bất kể cài đặt nào — một mô-đun Bayer
  bị xoay hoặc lật sẽ tạo thành một mẫu bộ lọc màu khác, và các bản đồ hiệu chuẩn được
  trên lưới riêng của cảm biến.
- **Các tệp `display`, `radiance`, `reflectance` và các tệp chỉ mục đã được định hướng** — xoay một lần, sau khi hoàn tất
  các phép tính hiệu chuẩn. Các mặt nạ giới hạn của chúng nằm trên cùng một lưới.
- **Mỗi tệp chụp đều ghi lại cài đặt** trong XMP: `XMP-Camera:ChlorosFlipHorizontal` /
  `ChlorosFlipVertical` (`true`/`false`), `ChlorosRotation` (0 / 90 / 180 / 270, theo chiều kim đồng hồ, sau
  các thao tác lật) và `ChlorosOrientationApplied` — `false` trên dữ liệu thô và đã tách kênh màu, `true` trên các
  sản phẩm đã định hướng. `ChlorosProcessing` liệt kê các thao tác được áp dụng cho các pixel của tệp đó (`none` khi
  ở định dạng gốc). Điều này bao gồm mọi khung hình trong một bản ghi liên tục (cài đặt gốc của máy ảnh,
  chưa được áp dụng) và trong luồng đa camera (dạng danh tính: các khung hình thành phần của nó ở định dạng gốc, giống như
  trong trường hợp chụp mảng).
- **Quá trình xử lý dự án sẽ áp dụng hướng đã ghi** cho các sản phẩm được tạo ra từ dữ liệu thô
  (đầu tiên là hiệu chỉnh trong tọa độ cảm biến, tiếp theo là hiệu chỉnh biến dạng mảng, cuối cùng là hướng),
  để chúng khớp với các sản phẩm của bản ghi trực tiếp — bao gồm cả các sản phẩm dự phòng chưa được hiệu chỉnh phản ứng cảm biến / đã được hiệu chỉnh hiện tượng tối góc;
  sản phẩm đã tách màu của nó vẫn giữ nguyên định dạng gốc.
- **Trình xem ảnh Chloros hiển thị tệp thô hoặc tệp đã loại bỏ hiệu ứng bayesian được xoay theo thông tin ghi lại** — chỉ hiển thị,
  tệp không bị thay đổi; tệp không có thông tin ghi lại sẽ được hiển thị đúng như khi lưu trữ.
- **Các bản chụp không có thông tin ghi lại sẽ được xử lý như định dạng gốc.** Trước phiên bản 1.3.0, một lần chụp bằng một máy ảnh
  tự chuyển đổi các tệp thô và tệp đã loại bỏ hiệu ứng bayered của chính nó; Chloros không thể phân biệt tệp thô đó với tệp gốc và
  không đưa ra phỏng đoán.
- **Các lần chụp mảng giữ nguyên định dạng gốc cho mọi tệp thành viên theo mặc định** và ghi lại thông tin nhận dạng;
  `ArraySession.capture(orient=True)` xử lý các sản phẩm của chúng (xem các điều khiển chụp mảng).

#### Đầu ra theo dải và mảng hỗn hợp

Ở các cấp độ `radiance` và `reflectance`, một camera trả về **một mặt phẳng cho mỗi dải của
camera đó** — ba mặt phẳng cho M3C (ba dải tần chưa trộn, thứ tự kênh BGR), một cho M3M (
bộ lọc dải hẹp duy nhất). Các mặt phẳng được nhân đôi của máy ảnh đơn sắc được gộp lại, do đó **mảng hỗn hợp
M3C/M3M sẽ cho ra các hình dạng mảng khác nhau cho mỗi chuỗi**. Đọc khóa `bands` từ
siêu dữ liệu khung hình để biết từng mặt phẳng là gì; không suy luận dựa trên số lượng kênh:

```python
for serial, (arr, hdr) in arr_handle.grab(processing="reflectance",
                                          with_metadata=True).items():
    print(serial, arr.shape, hdr["bands"])   # ('…', (1536, 2048, 3), ['nir','green','red'])
                                             # ('…', (1536, 2048, 1), ['F685'])
    rho = arr.astype("float32") / hdr["pixel_scale"]
```

Luôn kiểm tra `hdr["processing_level"] == "reflectance"` trước khi sử dụng các giá trị. Một trường hợp hạ cấp
(không có dữ liệu DAQ, không có gói hiệu chuẩn, bố cục dải tần không xác định) được báo cáo trong `processing_level` và
`radiometric_refused` thay vì được nâng cấp — mảng bạn nhận được vẫn là một khung dữ liệu hợp lệ, chỉ là
không phải sản phẩm mà bạn yêu cầu. Một bản ghi đã lưu bị hạ cấp sẽ mang cùng mã trong tệp XMP
như `Chloros:RadiometricRefused` (ví dụ: `no-band-layout`, `no-dls-snapshot`).

#### Một tín hiệu kích hoạt cho mỗi lần đọc — `group_id`

**Các thành viên trong mảng được phục vụ từ nhóm khung hình đồng bộ của mảng, không phải từ bộ đệm chụp riêng
của từng camera.** Các camera trong một mảng đồng bộ phần cứng được phơi sáng cùng lúc, nhưng N lần đọc
từ mỗi camera không *lấy mẫu* cùng lúc — việc đọc khung hình được đệm mới nhất của từng camera sẽ vượt qua, và chỉ cần một khung hình bị bỏ sót là đủ để làm mất đồng bộ toàn bộ mảng mà không ai nhận ra.

Việc lấy dữ liệu từ nhóm là cần thiết nhưng chưa đủ: mỗi yêu cầu vẫn
chụp nhanh *nhóm mới nhất* tại thời điểm riêng của nó, và vòng lặp chụp có thể xuất bản trong khoảng thời gian đó.
Khi đo trên một mảng 4 camera trực tiếp, **12 trong số 20 lần đọc song song nằm giữa hai nhóm** — luôn
chính xác liền kề nhau. Vì vậy, lần đọc được **ghim**:

1. Mỗi khung hình đều mang `group_id`, bộ đếm xuất bản đơn điệu của mảng.
2. `grab()` đọc dữ liệu từ máy chủ chính (hoặc, nếu máy chủ chính không thể phục vụ, thì từ camera tiếp theo có thể phục vụ),
   sau đó cố định các camera còn lại vào ID nhóm mà nó đã truy cập (`?group_id=N` trên điểm cuối HTTP).
3. Hệ thống backend lưu giữ các nhóm gần đây trong **ít nhất nửa giây**, do đó một camera cùng nhóm đã
   bị thay thế bởi các bản phát hành sau đó vẫn được phục vụ khi kích hoạt — đủ lâu để quá trình truyền dữ liệu phản xạ
   dữ liệu phản xạ của từng camera, ngay cả trên mảng cảm biến đầy đủ gồm 9 camera. Một ghim mà máy chủ phía sau không
   còn giữ nữa sẽ bị từ chối với mã `409` và `group_aged_out: true` thay vì được trả lời im lặng
   bằng một tín hiệu kích hoạt khác — `grab()` sẽ đọc lại tập hợp đó.

Đo lại với tính năng ghim: **20 trong số 20 lần đọc trả về một tín hiệu kích hoạt.** `grab()` thực hiện điều này cho bạn;
bạn chỉ cần `group_id=` khi truy cập trực tiếp vào điểm cuối HTTP.

- Một camera không thể được phục vụ tại nhóm đã ghim — không có trong nhóm đó, hoặc từ chối yêu cầu
  (một `band` mà nó không có, một mức mà nó không thể tạo ra) — sẽ bị **bỏ qua thay vì được điền vào**
  từ một trình kích hoạt khác, và các camera còn lại vẫn được trả về.
- Đối tượng dict mà `grab()` trả về là một `GrabResult` (một lớp con `dict` thuần túy;
  `from chloros_sdk import GrabResult`): `.errors` ánh xạ từng
  số sê-ri bị bỏ qua đến ngoại lệ mà thao tác đọc của nó gây ra, và `.group_id` là nhóm chung. Một
  `.errors` trống có nghĩa là mọi camera đều thuộc nhóm này.
- Một camera độc lập không thuộc nhóm nào, báo cáo `group_id: None`, và **từ chối** một `group_id=`
  thay vì bỏ qua nó.

```python
frames = arr_handle.grab(processing="reflectance")
for serial, err in frames.errors.items():
    print(f"SN:{serial} omitted: {err}")       # e.g. status=400 ... band 'F685' not in [...]

frames = arr_handle.grab(processing="reflectance", with_metadata=True)
ids = {hdr["group_id"] for _a, hdr in frames.values()}
assert len(ids) == 1, f"straddled triggers: {ids}"     # belt and braces
```

Cùng một bộ đếm được công bố trên **mỗi luồng MJPEG riêng cho từng camera** của một thành viên mảng —
bản xem trước (`/api/camera/stream/<serial>`), bản xem trước có chú thích (`/stream-annotated`) và
luồng phản xạ (`/stream-reflectance`) — dưới dạng `X-Chloros-Group-Id` trên mỗi phần, bên cạnh
`X-Chloros-Frame-Id` và `X-Chloros-Timestamp-Ns`. Các luồng là liên tục chứ không phải
tại một thời điểm cụ thể, do đó chúng được ghép nối bằng cách thu thập các phần và nhóm lại dựa trên tiêu đề đó — không có
cài đặt nào cần thiết. Một camera độc lập không thuộc nhóm nào. Xem
[Luồng MJPEG theo từng camera](#per-camera-mjpeg-streams-no-rtsp).

**`frame_stream()` không bao giờ im lặng.** Một tick thất bại vì lý do có thể tự khắc phục
sẽ bị bỏ qua — luồng camera không trả về gì cho tick đó, trong khi luồng mảng trả về các camera đã
trả lời (có thể là không có camera nào). Những lý do đó là: chưa có khung hình; luồng lấy dữ liệu của camera bị tạm dừng trong
một hoặc hai giây (`404 not streaming`, mà phần nền trả lời mỗi khi có camera khác kết nối
hoặc ngắt kết nối, hoặc thay đổi cài đặt làm cân bằng lại nhóm); hệ thống backend khởi động lại hoặc `5xx`; quá trình căn chỉnh
vẫn đang ổn định (`409 … retry shortly`); và từ chối phản xạ vì DAQ chưa có giá trị đo
(`no-dls-snapshot`, giá trị đo đã lỗi thời hoặc trống). Việc từ chối ở lần tiếp theo sẽ chỉ lặp lại — một
lỗi không xác định `band`, `bit_depth=8` về độ rạng rỡ, một gói hiệu chuẩn không có thang đo độ rạng rỡ, việc từ chối phản xạ
với **hoàn toàn không có DAQ** (không có thiết bị nào được liên kết với máy ảnh hoặc mảng cảm biến của nó, không có thiết bị nào được kết nối với
hệ thống xử lý phía sau: `dls_source` của lần từ chối là `"none"`), bất kỳ `4xx` nào khác —
sẽ gây ra `chloros_sdk.FrameFetchError` (một `RuntimeError`, chứa `.status` và `.body`) cùng
lúc. Tương tự, **60 giây** bỏ qua các tick mà không có khung hình nào xen kẽ, dù đó là `fps` — đủ
dài để vượt qua quá trình ổn định căn chỉnh và khởi động lại hệ thống phía sau, nhưng cũng đủ ngắn để một camera đã
thực sự không còn hoạt động sẽ ngừng phát luồng. Lỗi từ bỏ này cũng là một `FrameFetchError`, áp dụng cho cả camera lẫn
mảng, với ngoại lệ từ lần đọc cuối cùng được ghi nhận là `.errors` (và, đối với camera, là `__cause__`). Đối với
mảng, "bị từ chối" có nghĩa là mọi camera đều bị từ chối; ngoại lệ `.errors` chứa lý do
của từng camera.

**Một mảng `frame_stream()` chỉ trả về mỗi tín hiệu kích hoạt một lần** (cả `ArrayHandle` và `ArraySession` đều như vậy). Một
tick có tập mới nhất chính là tín hiệu kích hoạt mà nó đã trả về sẽ chờ tín hiệu kích hoạt tiếp theo thay vì
trả về cùng một tập đó lần nữa, do đó luồng dữ liệu chạy với tốc độ chậm hơn giữa `fps` và tốc độ khung hình của chính mảng, và các bộ dữ liệu liên tiếp không bao giờ chia sẻ chung một `group_id`. Một mảng ngừng phát hành sẽ tạo ra
`FrameFetchError` (“không có tín hiệu kích hoạt mới trong 60 giây”) thay vì lặp lại tập dữ liệu cuối cùng. Nếu không có cơ chế này, một
luồng yêu cầu nhiều hơn số tập dữ liệu mà mảng đã xuất bản — hoặc bắt đầu ngay sau khi kết nối — sẽ nhận được
các bản lặp lại: trong một lần đo lường, 20 bộ dữ liệu chứa 11 ID nhóm khác nhau.

```python
from chloros_sdk import FrameFetchError

try:
    for arr in cam.frame_stream(processing="radiance", bit_depth=8):
        my_analysis(arr)
except FrameFetchError as exc:
    print(exc.status, exc.body)        # the HTTP status and the backend's reason
```

#### Xuất một dải tần duy nhất — `band`

`grab()` / `frame_stream()` sử dụng `band="<name>"` để xuất chỉ một dải của camera dưới dạng
mảng `(H, W, 1)` một kênh — có hình dạng tương tự như camera đơn sắc tạo ra, do đó đầu ra một dải
được xử lý giống hệt nhau dù đến từ M3M hay từ việc tách M3C. Dải màu đỏ của FRGN:

```python
red = cam.grab(processing="reflectance", band="red")        # (1536, 2048, 1)
```

Các cách viết được chấp nhận là tên bố cục đầy đủ (`Red_660`, `NIR_850`, `Green_550`) hoặc tên viết tắt
của nó (`red`, `nir`, `green`), không phân biệt chữ hoa chữ thường. Tên không xác định sẽ bị từ chối cùng với danh sách các
tên có sẵn.

> **Việc lựa chọn được thực hiện theo tên, không phải theo số kênh — đây là thiết kế có chủ đích.** "Kênh 1" là `Green_550` theo
> thứ tự kênh BGR của mảng nhưng là *đỏ* theo thứ tự chữ cái trong mã bộ lọc(`RGN`), và hai
> thứ này không khớp nhau. Chọn nhầm sẽ tạo ra một khung hình hoàn toàn hợp lý nhưng thuộc dải tần sai, mà
> không có thiết bị nào ở phía sau có thể phát hiện được. Chính vì lý do đó mà không có chỉ số số.

#### Căn chỉnh các luồng — `align`

`align=True` biến dạng khung hình thành khung tham chiếu chung của mảng. Mỗi camera được biến dạng
thông qua độ phân giải cấu hình và vùng cắt ROI **giống nhau**, do đó các luồng dữ liệu của từng camera được mở độc lập
sẽ được **đồng bộ hóa với nhau** — bạn không cần phải yêu cầu chúng cùng lúc:

```python
frames = arr_handle.grab(processing="reflectance", align=True)   # {serial: array}
# or one MJPEG stream per camera, each co-registered on its own:
#   GET /api/camera/<serial>/stream-reflectance?align=1
```

Yêu cầu và hành vi:

- Camera phải nằm trong một mảng **đồng bộ phần cứng** (dây đồng bộ M8) có hồ sơ căn chỉnh
  — việc căn chỉnh chung giả định các thành viên được phơi sáng đồng thời, điều mà mảng chạy tự do phần mềm hoặc
  mảng chạy tự do không cung cấp. Nếu không, yêu cầu sẽ bị từ chối với mã lỗi `409`.
- **Bị từ chối, không bao giờ tự động chấp nhận trạng thái không căn chỉnh.** Đường dẫn ghi hình có thể lưu dữ liệu không căn chỉnh kèm cảnh báo
  vì tệp vẫn giữ các thẻ nguồn gốc và có thể được căn chỉnh lại sau này; trong khi luồng trực tiếp không có
  cơ hội thứ hai như vậy, do đó việc thiếu hồ sơ căn chỉnh là lỗi chứ không phải giải pháp dự phòng.
- **Yêu cầu căn chỉnh sẽ khởi động quá trình này.** Yêu cầu `align` trên một mảng không có cấu hình sẽ kích hoạt
  quá trình hiệu chuẩn và trả về `409` kèm theo thông tin về điều nó đang chờ (ví dụ: *"đang chờ độ phơi sáng
  ổn định ở 214000551 (1/3)"*) — thử lại và nó sẽ thành công, thường trong vòng vài chục giây.
  Điều này rất quan trọng khi chạy ở chế độ không giao diện người dùng: tính năng tự động căn chỉnh khi kết nối sẽ từ bỏ sau 30 giây, và mọi nỗ lực
  trong khoảng thời gian đó sẽ bị bỏ qua khi chế độ AE thông minh vẫn đang ổn định, do đó một mảng có độ phơi sáng
  hội tụ chậm thường sẽ không có hồ sơ cho toàn bộ phiên và không có cách nào để lấy
  hồ sơ đó mà không mở chế độ xem kết hợp trên máy tính để bàn. `alignment_status` cũng báo cáo
  `ae_align_holdouts`, nêu tên các camera vẫn đang ổn định và tiến trình của chúng.
- Gán cho mọi luồng của một mảng cùng một **`max_h`**, nếu không các vùng cắt của chúng sẽ nằm trên các lưới pixel
  khác nhau.
- Việc căn chỉnh được áp dụng *trước* khi mã hóa độ sâu bit, do đó quá trình biến dạng sẽ nội suy các giá trị có độ chính xác đầy đủ.

#### `bit_depth` — đánh đổi độ chính xác để lấy băng thông

`grab()` và `frame_stream()` chấp nhận `bit_depth=16` (mặc định) hoặc `bit_depth=8`:

| `bit_depth` | Mã hóa độ phản xạ | Chi phí |
| --- | --- | --- |
| `16` | uint16, 32768 = ρ 1.0, dải dư đến ρ 2.0 | Độ chính xác đầy đủ. |
| `8` | uint8 bao trùm `0..refl_max` (mặc định ρ 1.0) | Nửa số byte; lượng tử hóa ~0.004 ρ tại ngưỡng trần mặc định. Các giá trị vượt quá ngưỡng trần sẽ bị cắt — xem bên dưới. |

##### Giới hạn 8-bit — `refl_max`

`refl_max` xác định phạm vi 8-bit mà ρ bao phủ, vì vậy với `pixel_scale = 255 / refl_max`, ρ vẫn là
`pixel / pixel_scale`. `grab()` và `frame_stream()` sử dụng nó như một từ khóa trên cả camera và
các handle mảng (`?refl_max=` trên điểm cuối HTTP). Tăng giá trị này để tạo khoảng trống cho các điểm sáng phản chiếu, với các bước thô hơn tương ứng:

```python
frame = cam.grab(processing="reflectance", bit_depth=8, refl_max=1.5)   # 0..1.5 in 0..255
```

| `refl_max` | `pixel_scale` | bước |
| --- | --- | --- |
| `1.0` (mặc định) | 255 | 0,0039 ρ |
| `1.5` | 170 | 0,0059 ρ |
| `2.0` | 127,5 | 0,0078 ρ |

> **Chế độ phơi sáng tự động không thể khắc phục hiện tượng cắt sáng 8-bit, và việc sử dụng nó để cố gắng khắc phục sẽ làm dữ liệu trở nên tồi tệ hơn.**
> ρ = π·L/E (sau khi loại bỏ các yếu tố phơi sáng và độ khuếch đại), do đó ρ là thuộc tính của cảnh, không phải của
> cài đặt máy ảnh — khi đo trên mảng cảm biến trực tiếp, việc giảm một nửa thời gian phơi sáng làm thay đổi giá trị trung bình của ρ là 1,3%. Ngược lại, việc tăng
> độ phơi sáng để “phù hợp” với dải động lại khiến các pixel rơi vào trạng thái bão hòa **cảm biến**, điều này
> dẫn đến việc đánh giá thấp L và làm sai lệch ρ theo hướng **thấp** mà không báo lỗi (kết quả đo: ρ tối đa 1,203 → 0,985 ở
> độ phơi sáng gấp 2 lần, trung bình −23%). Chọn dải động bằng `refl_max`; việc giữ cho cảm biến không bị
> bão hòa chính là mục đích của chế độ phơi sáng tự động (AE), và điều đó bảo vệ *tính hợp lệ* của ρ thay vì dải động của nó.

**Đọc `reflectance_clipped_fraction`, chứ không phải cờ.** `reflectance_clipped_at_1_0` kích hoạt chỉ vì
một pixel duy nhất — trong một cảnh thực tế, nó báo cáo `True` trong khi tỷ lệ là `0.000001`, một pixel
trong một triệu. Tỷ lệ này chính là yếu tố quyết định liệu bạn có nên nâng giới hạn trên hay không.

**`bit_depth=8` chỉ áp dụng cho độ phản xạ.** Yêu cầu giá trị này cho `radiance` sẽ bị từ chối thay vì
được đáp ứng: độ phản xạ có phạm vi cố định từ 0 đến 1, phù hợp với mã hóa 8-bit, trong khi độ rạng rỡ không có
phạm vi cố định, do đó để phù hợp với 8 bit, cần phải thực hiện việc kéo giãn theo phần trăm cho từng khung hình, trong đó bản đồ
thay đổi theo từng khung hình — đó là một hình ảnh, không phải là một phép đo.

#### Dòng dữ liệu phản xạ trực tiếp (MJPEG)

`GET /api/camera/<serial>/stream-reflectance` cung cấp độ phản xạ 8-bit cho từng dải tần dưới dạng
`multipart/x-mixed-replace` — định dạng M3C dưới dạng tệp JPEG 3 kênh, định dạng M3M dưới dạng ảnh xám. Thông số truy vấn:
`fps` (mặc định 10), `jpeg_q` (mặc định 95), `max_h` (0 = định dạng gốc), `band` (một dải có tên, như
trên), `align` (`1` để đăng ký chung — các quy tắc tương tự như `align` trong SDK), `refl_max` (giới hạn mã hóa 8-bit,
như trên). Mỗi phần chứa
`X-Chloros-Group-Id`, `X-Chloros-Frame-Id`, `X-Chloros-Timestamp-Ns`, `X-Chloros-Bands`,
`X-Chloros-Pixel-Scale`, `X-Chloros-Aligned` và `X-Chloros-Reflectance-Clipped`.
**Các phần từ các camera khác nhau mang cùng một `X-Chloros-Group-Id` là cùng một tín hiệu kích hoạt** —
đó là cách các luồng dữ liệu riêng lẻ từ mỗi máy ảnh trong cùng một mảng được ghép lại, và điều này được đảm bảo
ở phía máy chủ vì các thành viên trong mảng được cung cấp từ nhóm khung hình đã đồng bộ (xem
[Một tín hiệu kích hoạt cho mỗi lần đọc](#one-trigger-per-read--group_id)). Nó trả về `409` khi giá trị phản xạ
không có sẵn tại thời điểm kết nối và kết thúc luồng nếu giá trị này không còn khả dụng trong quá trình chạy, do đó một luồng bị đình trệ
có nghĩa là “không còn giá trị phản xạ” chứ không phải “vẫn ổn, chỉ là tĩnh”.

> **Định dạng JPEG là nén có mất mát — các pixel này là kết quả đo lường.** Lỗi nén ảnh hưởng trực tiếp đến
> các giá trị phản xạ và nghiêm trọng nhất ở các cạnh, nơi có ranh giới vật liệu. Chất lượng
> mặc định là 95 vì lý do đó. Nếu bạn cần giữ nguyên các giá trị số, hãy sử dụng
> `grab(processing="reflectance")` / `frame_stream(...)`, các lệnh này sẽ trả về kết quả tương tự mà không có
> codec trong đường dẫn (và dùng `bit_depth=8` nếu chi phí đường truyền, chứ không phải codec, là yếu tố hạn chế).

> **Dữ liệu EXIF được giữ nguyên khi xuất.** `process()` sao chép khối GPS của bản ghi nguồn
> **và ExifIFD của nó** vào mỗi sản phẩm, do đó các tệp xuất sẽ chứa `FocalLength`, `FNumber`,
> `ExposureTime`, `ISO`, `DateTimeOriginal` và `CameraSerialNumber` cũng như
> tham chiếu địa lý. `FocalLength` là dữ liệu mà Pix4D sử dụng để tính khoảng cách mẫu mặt đất — nếu thiếu nó
> quá trình tái tạo sẽ chuyển sang tỷ lệ sai lệch nghiêm trọng (một trường hợp đo đạc đã biến khu vực 411 m
> thành 47,8 km). Bản sao này cố ý không phải là `-all:all`: các thẻ cấu trúc của IFD0 làm hỏng
> đầu ra của LATTICE, và `ExifImageWidth`/`Height` bị loại trừ vì chúng mô tả quá trình
> chụp nguồn thay vì raster đã xuất.

Các cờ phụ ở giai đoạn thu thập (áp dụng cho các mức phóng xạ — `radiance`, `reflectance`, `display`):

| Cờ | Mặc định | Ý nghĩa |
| --- | --- | --- |
| `apply_calibration` | `True` | DSNU + hiệu chỉnh trường phẳng + tách thành phần 3x3 + thang đo bức xạ NIST. |
| `apply_white_balance` | `True` | Bảng tra cứu cân bằng trắng (WB LUT). Hỗ trợ DLS khi thiết bị thu thập dữ liệu (DAQ) được kết nối với camera. |
| `apply_index` | `False` | Đánh giá chỉ số thực vật. |
| `index_expression` | `None` | Ghi đè công thức. Khôngtrống → tự động kích hoạt chỉ số. |
| `annotated` | `False` | Hiển thị các hiệu ứng giao diện người dùng (zebra/grid/peaking). Không khả dụng cho `raw`. |

### `ArrayHandle`

```python
arr = proj.arrays["main_rig"]

# Single synced capture group
files = arr.capture("./out", format="tiff", processing="reflectance")
# → {"213800234": "/path/to/x.tif", "214000533": "/path/to/y.tif", ...}

# Multi-level: each serial's value becomes an ordered LIST, not a str
files = arr.capture("./out", processing="all")
# → {"213800234": ["/raw.tif", "/debayered.tif", ...], "combined": "/idx.tif"}

# Smart capture (wait for AE to settle)
result = arr.capture_smart(
    "./out", processing="reflectance",
    settle_timeout_s=5.0,
    stability_window_s=1.5,
    exposure_tolerance_pct=5.0,
)
print(result["frames"], result["settle"])

# In-memory grab: {serial: numpy array}
frames = arr.grab(processing="debayered")
frames = arr.grab(processing="radiance", with_metadata=True)

# Stream-to-disk loop
arr.stream(count=60, output_dir="./stream", fps=5, processing="raw")

# Frame-iterator (tolerates per-cam drops; great for downstream analysis pipelines)
for frames in arr.frame_stream(processing="radiance", fps=5, count=100):
    if "213800234" in frames:
        my_analysis_pipeline(frames["213800234"])

# Preview iterator (live MJPEG-equivalent; tolerates partial cycles)
counts = arr.preview_stream("./preview", fps=3.0, duration=30.0)
print(counts)  # frames written per serial
```

> **Giá trị trả về là một `chloros_sdk.CapturePaths`, có kiểu là `CapturePathMap`, không phải `Dict[str, str]`.**
> `chloros_sdk.CapturePathMap` là `Dict[str, Union[str, List[str]]]`: một cấp
> `processing` cung cấp cho mỗi máy ảnh một đường dẫn, trong khi `processing="all"` cung cấp
> **danh sách có thứ tự** của mọi sản phẩm đã lưu cho máy ảnh đó. `ArrayHandle.capture()` không chấp nhận
> không nhận `levels=`, và nó bỏ qua các đối số từ khóa mà nó không nhận biết (`levels`, `daq`,
> `render_index`, `force_daq`), vì vậy chỉ truyền các tham số được liệt kê ở đây (`aligned=False`
> lưu các tệp gốc, giống như trên `ArraySession.capture()`); hãy sử dụng
> `ArraySession.capture()` cho các điều khiển đó. Một bản tổng hợp trực tiếp, nếu đang phát trực tuyến, sẽ được gửi dưới khóa bổ sung
> khóa `"combined"` thay vì dưới một khóa nối tiếp. Mã giả định `str` sẽ gặp lỗi ở
> dạng danh sách mà không có trình kiểm tra kiểu nào phản đối — chú thích cho biết `Dict[str, str]`
> trong một thời gian sau khi dạng danh sách được phát hành, đó là lý do tại sao biệt danh này tồn tại. Chuẩn hóa
> khi bạn muốn dạng phẳng:
>
> ```python
> paths = arr.capture(processing="all")
> flat = [p for v in paths.values()
>         for p in (v if isinstance(v, list) else [v])]
> ```

> **Điều không thể viết: `.failed`.** Bản đồ mà `capture()` trả về là một
> `chloros_sdk.CapturePaths` — một lớp con đơn thuần của `dict`, do đó mọi thứ ở trên vẫn hoạt động
> bình thường — đồng thời chứa cả `.failed`: các tệp mà hệ thống nền không thể ghi,
> các mục tương tự như [`CaptureResult.failed`](#files-that-could-not-be-written--captureresultfailed)
> (`serial`, `level`, `error` và `disk_full: True` trên ổ đĩa đầy). Việc
> ghi lại một phần vẫn trả về mọi đường dẫn đã được ghi; trên ổ đĩa đầy, bản đồ sẽ
> trống và `.failed` giải thích lý do. `.skipped` liệt kê các đoạn bỏ qua theo từng camera và
> `.group_seq` là nhóm đồng bộ, và `.alignment` / `.oriented` là các
> thông tin từ hệ thống nền, tương tự như trên `CaptureResult`. `capture_smart()` trả về chúng dưới dạng
> `result["frames"].failed` và `result["failed"]`.
>
> ```python
> paths = arr.capture("./out", processing="all")
> if any(f.get("disk_full") for f in paths.failed):
>     raise SystemExit("drive full -- free space and capture again")
> ```

### Căn chỉnh mảng

`ArrayHandle` hiển thị toàn bộ bề mặt căn chỉnh. Theo mặc định, các hồ sơ chỉ tồn tại trong phiên — hãy gọi `export_alignment()` một cách rõ ràng để lưu trữ.

```python
from chloros_sdk import AlignmentSpec

arr = proj.arrays["main_rig"]

# Defaults: ORB / affine / one synced snapshot — same as the GUI's auto-cal
result = arr.calibrate_alignment()
print(result["profile"]["rms_residual_px"])

# Custom spec for tough scenes (low-contrast canopy)
spec = AlignmentSpec(
    method="feature_orb",         # feature_orb / feature_akaze / phase_correlation / checkerboard / manual
    model="rigid",                # translation / rigid / affine / homography
    num_frames=5,
    max_features=8000,
    ratio_threshold=0.7,
    ransac_threshold_px=2.0,
    min_matches=30,
    max_reproj_err_px=2.0,
)
arr.calibrate_alignment(spec)

# Or tweak one knob at a time
arr.calibrate_alignment(num_frames=3, model="affine")

# Manual control points: each camera's pixel coordinates of the same
# features, the reference camera's included (>= 2 per camera). Every one
# of num_frames is solved from these points.
arr.calibrate_alignment(method="manual", model="rigid", control_points={
    "214701173": [[412, 300], [1630, 310], [1020, 1180]],   # reference (master)
    "214701292": [[418, 297], [1637, 306], [1027, 1177]],
})

# Inspect / manipulate
status = arr.alignment_status()
arr.tweak_alignment("214701292", dx=2.5, dy=-1.0, rotation_deg=0.0, scale=1.0)
arr.export_alignment("/tmp/main_rig_alignment.json")
arr.import_alignment("/tmp/main_rig_alignment.json", validate=True)
arr.clear_alignment()
```

#### Căn chỉnh khi kết nối

`connect_all(align=...)` có thể tự động căn chỉnh mọi mảng khi kết nối:

```python
# Align every array with defaults
proj.connect_all(align=True)

# Per-array control
proj.connect_all(align={
    "main_rig": AlignmentSpec(num_frames=5, model="affine"),
    "side_rig": True,             # use defaults
    "verify_rig": False,          # skip
})
```

Sẽ chuyển sang sử dụng `project.json["config"]["auto_align_on_connect"]` nếu không được chỉ định.

### `SensorHandle`

Một cảm biến ánh sáng đã lưu (DAQ-U, DAQ-M hoặc DAQ-E), được mở bởi `connect_all()` trong [bể cảm biến DAQ](#daq-sensor-sessions) — cùng bể mà `connect_daq_sensor()` và `chloros-cli daq pool-connect` sử dụng.

```python
reading = proj.sensors["Sky"].read()
if reading is not None:
    spectrum, is_saturated, integration_time, x, y, z = reading
```

- `read()` chờ phổ **tiếp theo** của cảm biến — phổ đã được lưu trong bộ đệm khi bạn gọi hàm sẽ không được tính — và trả về `(spectrum, is_saturated, integration_time, x, y, z)`, hoặc `None` nếu không có dữ liệu nào đến trong vòng 5 giây. Dòng dữ liệu bị tạm dừng được tiếp tục. `spectrum` được đo bằng W/m²/nm khi cảm biến đã được hiệu chuẩn.
- `.sensor` là [`DAQSensorSession`](#phiên-cảm-biến-daq), đối với các trường hợp khác: `latest(n=...)`, `status()`, `record_start()` / `record_stop()`.
- `.slot_id`, `.model`, `.serial`, `.name` và `.settings` là các mục đã lưu từ `sensors.json`.

**Những gì `connect_all` khôi phục.** Mỗi cảm biến được mở tại địa chỉ đã lưu của nó — cổng COM, địa chỉ Bluetooth hoặc máy chủ DAQ-E — và, khi dự án đã ghi lại ID của cảm biến, thì đó phải chính là cảm biến đó: một thiết bị khác tại địa chỉ đó sẽ bị từ chối, không được kết nối. Thời gian tích hợp, trung bình khung hình và phơi sáng tự động đã lưu của nó được áp dụng, cùng với hiệu chỉnh nắp:

| Hồ sơ giới hạn đã lưu | Kết nối với |
| --- | --- |
| Một hồ sơ giới hạn (ví dụ: `fov_30`) | Hồ sơ đó. |
| Chưa chọn | `sunshine_cosine`, mặc định khi kết nối, được ghi lại như giả định (`cap_id_source=auto_default`), do đó quá trình xử lý sẽ hiển thị cảnh báo. Các cảm biến được xuất xưởng ở trạng thái trần: hãy chọn nắp phù hợp. |
| `none` — được chọn là **Không có (cảm biến trần)** trong giao diện người dùng | Không có hiệu chỉnh nắp. |

Cảm biến DAQ-E-S không sử dụng cấu hình nắp, bất kể cấu hình nào đã được lưu: bộ khuếch tán của nó là một phần của quá trình hiệu chuẩn (xem [DAQ-E-S](#daq-e-s--fixed-diffuser-and-tilt-sensor)). Một dự án được lưu bởi phiên bản Chloros trước đó đã ghi lại cảm biến trần là tùy chọn mặc định, do đó nó sẽ kết nối lại với `sunshine_cosine`; hãy chọn **None (cảm biến trần)** trong giao diện người dùng (GUI) một lần nữa và lưu dự án.

Một cảm biến mà một khách hàng khác đã mở sẵn trong nhóm (CLI, một tập lệnh khác) sẽ được kết nối nguyên trạng, mà không cần áp dụng lại cài đặt, và `disconnect_all()` để nó tiếp tục chạy; cảm biến do `connect_all()` mở ra sẽ được giải phóng. Giống như với `connect_daq_sensor()`, giao diện người dùng Chloros **không** thuộc về nhóm: hãy đóng cảm biến trong GUI trước khi kết nối dự án từ tập lệnh.**Yêu cầu và báo cáo.** Các cảm biến ánh sáng cần có một backend Chloros đang chạy và đã đăng nhập vào gói Chloros+ trả phí (Copper trở lên), và `connect_all()` không bao giờ khởi động một backend như vậy. Khi không có backend nào đang chạy, mọi cảm biến đã lưu sẽ được liệt kê dưới `report["sensors"]["errors"]`, kèm theo thông báo này. Một cảm biến mà backend không thể mở cũng được liệt kê ở đó, kèm theo lý do từ backend; `report["sensors"]["missing"]` luôn trống đối với các cảm biến ánh sáng.

---

## Phần cứng trực tiếp (Không cần backend)

Khi bạn muốn không phụ thuộc vào backend cho các camera LATTICE (CI, robot không đầu, nhúng), hãy nhập trực tiếp `lattice_sdk` — nó được xuất lại bởi `chloros_sdk`. Lưu ý về `CAMERA_AVAILABLE`: `lattice_sdk` có trong gói SDK, nhưng yêu cầu phải có môi trường chạy Arena SDK và OpenCV (`cv2`), trong khi gói này không cài đặt cả hai. Cảm biến ánh sáng phải đi qua backend — [`connect_daq_sensor()`](#daq-sensor-sessions) hoặc `proj.sensors` của dự án — với một ngoại lệ là trình trợ giúp độ lớn dải DAQ-M [`DLS`](#daq-m-band-magnitudes--dlsband_magnitudes), cũng cần có `bleak`.

```python
from chloros_sdk import (
    # cameras
    LatticeCamera, CameraSettings, PRESETS, CameraPool,
    Calibration, CalibrationCoefficients, FilterModel, list_filters,
    DLS, NetworkDiagnostics, gpu_info, gpu_available,
    # discovery
    discover_cameras, discover_cameras_via_backend,
    # exceptions
    LatticeError, CameraNotFoundError, StreamError, CaptureError,
    CalibrationError, NetworkError, DLSError,
)

# Find a camera and capture in one go
cams = discover_cameras(timeout_ms=3000)
print(cams)

settings = PRESETS["high_quality"]
with LatticeCamera(serial="213800234", settings=settings) as cam:
    result = cam.capture(output_dir="./out", format="tiff")
    print(result.filepath, result.width, result.height)
```

##### Các cài đặt sẵn và tín hiệu kích hoạt

Ba trong số bốn cài đặt sẵn là chế độ **chạy tự do**: máy ảnh phơi sáng liên tục và
`capture()` trả về khung hình tiếp theo. `triggered` là trường hợp ngoại lệ — nó kích hoạt
máy ảnh khi phát hiện tín hiệu cạnh phần cứng trên Dòng 2, do đó sẽ không ghi lại gì cho đến khi tín hiệu đó xuất hiện.

| Cài đặt sẵn | Cơ chế kích hoạt | Sử dụng khi |
| --- | --- | --- |
| `default` | free-run | sử dụng chung |
| `high_speed` | free-run | 8-bit, giới hạn 60 fps, phơi sáng ngắn |
| `high_quality` | free-run | 12-bit, không giới hạn fps — lựa chọn thông thường cho ảnh tĩnh |
| `triggered` | **sẵn sàng, Dòng 2** | máy ảnh được kết nối qua cáp đồng bộ M8 và một thiết bị khác kích hoạt nó |

Nếu bạn chọn `triggered` (hoặc tự thiết lập `trigger_mode="On"`) mà không có gì
điều khiển Dòng 2, mọi `capture()` sẽ hết thời gian chờ — đúng như mong đợi, vì bạn đã yêu cầu
camera chờ. SDK sẽ giải thích điều này khi sự cố xảy ra; xem
[SC_ERR_TIMEOUT trong quá trình ghi hình](#direct-hardware-backend-free).

> **Lưu ý — Các thông báo "GVSP probe" / `SC_ERR_TIMEOUT -1011` khi kết nối không phải là lỗi.**
> Khi kết nối, SDK sẽ cố gắng đàm phán**khung jumbo** (gói tin GVSP 9000 byte) để đạt thông lượng cao hơn. Trên kết nối NIC điểm-điểm trực tiếp (ví dụ: địa chỉ `169.254.x.x` cục bộ trên liên kết), mạng thường không hỗ trợ khung jumbo, do đó quá trình kiểm tra này sẽ hết thời gian chờ và ghi lại các dòng như:
>
> ```
> [Network] GVSP probe: unexpected error (TimeoutError: ... SC_ERR_TIMEOUT -1011)
> [Network] GVSP probe at 9000 did not deliver a complete buffer; reverting to ICMP-chosen size
> [Network] GVSP packet size: 1500 bytes (standard)
> ```
>
> Đây là **cơ chế dự phòng theo thiết kế**: SDK tự động quay trở lại sử dụng các gói tin tiêu chuẩn 1500 byte và camera tiếp tục kết nối bình thường (các dòng `[chunk-enable …]` tiếp theo là một phần của chuỗi kết nối thông thường). Chức năng ghi hình vẫn hoạt động bình thường.
>
> Bạn có thể bỏ qua lệnh kiểm tra này, nhưng **nó không chỉ đơn thuần là công cụ ẩn nhật ký — nó còn tắt tính năng khung jumbo.** Camera chỉ trả lời các gói ping "Don't-Fragment" có kích thước tối đa 1500 byte, bất kể mạng của bạn tốt đến đâu, do đó chỉ riêng thử nghiệm ping không bao giờ phát hiện được khung jumbo; lệnh kiểm tra này là cách duy nhất có thể làm được điều đó. Nếu tắt nó đi, camera sẽ chạy các gói tin tiêu chuẩn 1500 byte mãi mãi, trên bất kỳ mạng nào:
>
> ```bash
> CHLOROS_GVSP_PROBE_FALLBACK=0   # gives up jumbo — see the warning it prints
> ```
>
> Chỉ đáng để sử dụng trên mạng mà bạn *biết chắc* không hỗ trợ khung jumbo, nơi nó giúp tiết kiệm khoảng một giây thời gian kết nối cho mỗi camera. Vì đây là sự đánh đổi thực sự chứ không phải chỉ mang tính hình thức, nên SDK hiện sẽ thông báo điều này khi bạn sử dụng nó:
>
> ```
> [Network] ⚠️ GVSP probe disabled (CHLOROS_GVSP_PROBE_FALLBACK=0) — staying at
> 1500 bytes, jumbo NOT tested. … if this network does carry it, you are giving
> up ~1.45x wire ceiling. Unset the variable to test for jumbo.
> ```
>
> **Đừng thay đổi cài đặt này trừ khi bạn có lý do chính đáng.**Nếu để tính năng này bật, mỗi camera sẽ được đo lường một lần cho mỗi phiên làm việc của backend, và đo lại khi kết nối của nó thay đổi (chuyển sang adapter khác, tốc độ kết nối khác, hoặc kết nối bị ngắt, như khi chuyển đổi switch); các lần kết nối lại sẽ sử dụng lại kết quả đã có.**Sau khi có thay đổi phía sau một switch vẫn hoạt động bình thường, hãy khởi động lại backend để đo lường lại.** Lần kết nối đầu tiên đôi khi có thể mất vài phút để hoàn tất kiểm tra này, và nhật ký sẽ ghi lại điều đó khi xảy ra.
>
> Nếu bạn *muốn* đo thông lượng gói jumbo, hãy kích hoạt chế độ jumbo end-to-end (MTU NIC 9000 + bộ chuyển mạch hỗ trợ gói jumbo), hoặc cố định kích thước bằng `CHLOROS_GVSP_PACKET_SIZE_FORCE=9000` khi bạn chắc chắn liên kết hỗ trợ — tuy nhiên nên ưu tiên sử dụng lệnh `CHLOROS_GVSP_PACKET_SIZE_FORCE=9000 python …` theo từng lệnh thay vì thiết lập vĩnh viễn, vì kích thước được cố định sẽ bỏ qua quá trình kiểm tra và ngừng thích ứng với mạng phía trước. **Mọi** thiết bị trên đường truyền đều phải hỗ trợ gói jumbo — bao gồm cả bộ chia hoặc bộ cấp nguồn PoE, đây thường là lý do khiến một hệ thống vốn có khả năng hỗ trợ gói jumbo lại không thể truyền chúng.

> **Lỗi `SC_ERR_TIMEOUT -1011` trong quá trình `capture()` / `grab*()` là một vấn đề khác — đó là một lỗi thực sự.**
> Lưu ý trên chỉ liên quan đến mã lỗi `-1011` được ghi lại bởi**cảm biến thời gian kết nối**. Nếu cùng một lỗi xuất hiện từ một**quá trình ghi lại**, điều đó có nghĩa là camera đã kết nối thành công nhưng không gửi bất kỳ hình ảnh nào:
>
> ```
> File ".../lattice_sdk/camera.py", line ..., in grab_frame_with_metadata
>   buffer = self._get_buffer(timeout)
> lattice_sdk.exceptions.CaptureError: Capture failed: ... SC_ERR_TIMEOUT -1011
> ```
>
> Dấu hiệu nhận biết là camera có kênh *điều khiển* hoạt động bình thường — quá trình phát hiện thành công, cài đặt và các lệnh ghi `[chunk-enable …]` đều thành công — trong khi *mọi* khung hình đều bị hết thời gian chờ.
>
> **Nguyên nhân thường gặp là camera đang được kích hoạt bằng tín hiệu kích hoạt phần cứng.** Với mã lỗi `trigger_mode="On"` và `trigger_source="Line2"`, camera sẽ không phát ra bất kỳ tín hiệu nào cho đến khi có tín hiệu điện xuất hiện trên cáp đồng bộ M8. Nếu không có cáp nào điều khiển đường tín hiệu đó, mọi lệnh lấy khung hình sẽ bị treo vô thời hạn. Máy ảnh không bị hỏng và mạng hoạt động bình thường — nó đang hoạt động chính xác theo lệnh đã được thiết lập.
>
> `CameraSettings()` và các mã `default` / `high_speed` / `high_quality` được cài đặt sẵn ở chế độ chạy tự do, và một lệnh chụp bị hết thời gian chờ trong khi đang ở trạng thái sẵn sàng sẽ hiển thị thông báo giải thích thay vì chỉ hiển thị mã lỗi `-1011`. `PRESETS["triggered"]` kích hoạt Line2, theo thiết kế.
>
> Để buộc bất kỳ camera nào chạy tự do:
>
> ```python
> import copy
> settings = copy.deepcopy(PRESETS["high_quality"])   # PRESETS entries are shared
> settings.trigger_mode = "Off"        # free-run; don't wait for an M8 edge
> ```
>
> Nếu vẫn hết thời gian chờ với lệnh `trigger_mode="Off"`, có nghĩa là camera thực sự không truyền dữ liệu — hãy gửi cho chúng tôi tệp nhật ký và lệnh `ip link show`.

#### Cấu hình màu (xem trước trực tiếp RGB) — `set_color_profile`

`LatticeCamera.set_color_profile(profile, custom_cct_k=None)` chọn cấu hình màu hiển thị cho **xem trước trực tiếp** trên các camera RGB (các camera đa phổ sẽ bỏ qua cài đặt này):

| Cấu hình | Ý nghĩa |
| --- | --- |
| `raw` | Bỏ qua hoàn toàn chuỗi đo quang. |
| `linear` | DSNU + flat + WB, không có CCM, không có gamma. |
| `natural` | Linear + CCM đo được + gamma sRGB, chỉ với hiệu ứng hoàn thiện cơ bản (làm mịn màu sắc + giảm bão hòa vùng sáng) — cài đặt mặc định thực tế. |
| `enhanced` | `natural` cộng với hiệu ứng hoàn thiện đầy đủ (loại bỏ viền màu, độ sống động, độ tương phản cục bộ CLAHE). Hình ảnh phong phú hơn với chi phí hoàn thiện mỗi khung hình gấp khoảng **hai lần**, do đó tốc độ khung hình LIVE thấp hơn. |
| `custom_temp` | `natural` nhưng WB được cố định ở `custom_cct_k` Kelvin (DLS bị bỏ qua; giới hạn trong khoảng 2000–10000 K ở phía backend). |

Cấu hình này là một công cụ điều chỉnh tốc độ/hình ảnh **chỉ dành cho xem trước trực tiếp**: các bản ghi được lưu luôn nhận được hiệu ứng hoàn thiện đầy đủ và phong phú bất kể cấu hình nào được chọn, do đó việc chọn `natural` để tiết kiệm thời gian xử lý khung hình sẽ không làm giảm chất lượng của dữ liệu được lưu vào đĩa. Một cấu hình không xác định sẽ kích hoạt `ValueError`; khi backend chloros có thể truy cập được, thay đổi này cũng sẽ được gửi (POST) đến backend đó để khung hình xem trước tiếp theo phản ánh thay đổi (người dùng SDK trực tiếp không có backend vẫn nhận được sự thay đổi cài đặt).

```python
with LatticeCamera(serial="214701292") as cam:   # RGB cam
    cam.set_color_profile("enhanced")            # richer look, lower LIVE fps
    cam.set_color_profile("custom_temp", custom_cct_k=5600)
```

#### Điểm nổi bật (màn hình RGB) — `set_highlights`

`LatticeCamera.set_highlights(value)` lấy các pixel sáng nhất, trắng nhất từ **màn hình** trên các camera RGB — đây là bản sửa lỗi cho bề mặt trắng sáng bị hiển thị thành một đốm trắng phẳng. `value` là `-100`..`0` (được giới hạn); `0` bị tắt, và hình ảnh hiển thị lúc này sẽ chính xác như khi không có cài đặt này. Các cài đặt mạnh hơn sẽ làm giảm độ trắng của màn hình (xuống khoảng 60% ở mức `-100`) và mở rộng đường cong tông màu, do đó, bề mặt mà độ sáng tự động của bản xem trước đẩy lên màu trắng tinh khiết sẽ lấy lại được chi tiết.

Đây là một tùy chọn điều khiển màn hình, nhưng khác với độ bão hòa và độ tương phản (chỉ thay đổi bản xem trước trực tiếp), nó cũng thay đổi những gì bạn lưu lại: bản xem trước trực tiếp và các tệp chụp ở **mức độ hiển thị** đều tuân theo cài đặt này, do đó các tệp đó cũng sẽ tối hơn (chúng mang giá trị `Chloros:DisplayHighlights`); các tệp xuất radiance và reflectance thì không bao giờ bị ảnh hưởng, và nó không thể khôi phục dữ liệu cảm biến đã bị cắt — hãy giảm phơi sáng để khắc phục điều đó. Các máy ảnh đa phổ (Multispec) và đơn sắc (mono) sẽ bỏ qua cài đặt này. Giá trị được lưu trữ trong `settings.highlights` và được POST đến một backend chloros có thể truy cập, hoàn toàn giống như `set_color_profile`; lệnh tương đương trong giao diện dòng lệnh (CLI) là `lattice highlights --set N`.

```python
with LatticeCamera(serial="214701292") as cam:   # RGB cam
    cam.set_highlights(-60)                      # 0 = off, -100 = strongest
```

#### Máy ảnh đơn sắc (M3M) và `Calibration`

Máy ảnh đơn sắc **M3M** (`M3M-<lens>-F<wavelength>`) là loại đơn dải: một mặt phẳng xám, không có mô-đun Bayer, không có ma trận nhiễu chéo quang phổ 3×3. `Calibration` nhận diện được điều này và hiển thị cờ `is_mono`. Độ phản xạ vẫn được áp dụng như một bản đồ phóng xạ theo từng dải (ma trận tách thành phần là ma trận đơn vị), nhưng các phép toán đa dải trên một camera duy nhất sẽ cho kết quả hợp lý thay vì trả về giá trị vô nghĩa:

```python
from chloros_sdk import Calibration, CalibrationError

calib = Calibration("M3M-L87-F685")
print(calib.is_mono)        # True  (False for any M3C / RGN Bayer cam)
print(calib.filter_type)    # 'mono'  (sentinel; not a real crosstalk key)

# NDVI needs two bands (Red + NIR); one mono band can't supply both.
try:
    calib.compute_ndvi(reflectance_frame)
except CalibrationError as e:
    print(e)   # "...single-band mono (M3M) camera. Combine multiple..."
```

Để xây dựng chỉ số thực vật từ phần cứng đơn sắc, hãy kết hợp nhiều camera M3M ở các bước sóng khác nhau thành một chồng hình đa dải đã được căn chỉnh (xem [Căn chỉnh mảng](#array-alignment)) và tính toán chỉ số trên toàn bộ chồng hình đó thay vì trên một camera duy nhất.

#### Độ sáng dải sóng DAQ-M — `DLS.band_magnitudes`

`DLS(mac_address, filter_model="RGN-IMX265")` kết nối với DAQ-M qua Bluetooth bằng địa chỉ MAC BLE riêng của nó. Đây là lớp cảm biến ánh sáng duy nhất giao tiếp trực tiếp với cảm biến thay vì thông qua phần mềm nền, do đó nó cần các thành phần mà các lớp camera trực tiếp yêu cầu (`CAMERA_AVAILABLE`: môi trường chạy Arena SDK và OpenCV) cộng với `bleak` cho Bluetooth (`pip install "chloros-sdk[camera]"` hoặc `pip install bleak`; nếu thiếu thành phần này, `DLS` sẽ gây ra lỗi `DLSError`). Một thiết bị DAQ-M được kết nối ở nơi khác (nhóm của backend, một dự án, GUI) sẽ ngừng phát tín hiệu, vì vậy hãy giải phóng nó ở đó trước. **`mac_address` là bắt buộc**: không có giá trị mặc định, vì mỗi thiết bị đều có giá trị riêng, và `DLS()` nếu thiếu giá trị này sẽ kích hoạt `DLSError` trước khi thử kết nối nào. Tìm địa chỉ MAC bằng `discover_daq_sensors(transports=["ble"])` (xem [Discovery](#discovery--finding-an-address-to-connect-with)) hoặc `chloros-cli daq pool-discover --only ble`.

`DLS.band_magnitudes(spectrum=None, *, spectral_response=None)` tích hợp phổ DAQ-M (giá trị đọc mới nhất khi `spectrum` là `None`; `DLSError` nếu chưa có) thông qua phản hồi của ba kênh camera và trả về `{"red": …, "green": …, "nir": …}` — các khóa đặt tên cho các kênh theo thứ tự đó; với các đường cong của camera RGB, kênh thứ ba (`nir`) là kênh màu xanh dương.

- Nếu không có `spectral_response`, nó sẽ sử dụng đường cong tiêu chuẩn của mô hình bộ lọc (đường cong mà `filter_model` và `DLS` được xây dựng dựa trên), đường cong này được đo trên một thiết bị.
- `spectral_response` thay vào đó sử dụng các đường cong đo được **riêng** của camera: một dict `{"wavelengths": [...], "R": [...], "G": [...], "B": [...]}`, được lưu trữ trong gói hiệu chuẩn của camera đó (`color_correction.spectral_response_smoothed` — camera RGB; các gói đa quang phổ không chứa đường cong như vậy). Nếu không thể sử dụng dict này, hệ thống sẽ chuyển sang sử dụng đường cong của dòng sản phẩm.

```python
from chloros_sdk import DLS

# camera_curves: {"wavelengths": [...], "R": [...], "G": [...], "B": [...]}
with DLS(mac_address="AA:BB:CC:DD:EE:FF") as dls:
    reading = dls.acquire()
    fleet = dls.band_magnitudes(reading)                         # fleet curve
    own = dls.band_magnitudes(reading,
                              spectral_response=camera_curves)   # the camera's own
```

Lệnh `chloros-cli lattice dls --camera SERIAL` trong giao diện dòng lệnh (CLI) thực hiện tương tự thông qua phần nền, tìm đường cong của camera trong gói hiệu chuẩn đã tải hoặc được lưu trong bộ nhớ đệm, và thông báo đường cong của camera nào đã được sử dụng. Trên giao thức HTTP, đó là `POST /api/dls/cli-read` với `mac` (bắt buộc: nếu thiếu, phản hồi sẽ là lỗi HTTP400, `code` `MAC_REQUIRED`) và `camera_serial` trong phần nội dung; giá trị `band_magnitudes_source` trong phản hồi sẽ là `camera <serial>` khi sử dụng đường cong riêng của camera đó, hoặc `model <filter>` cho đường cong chung của đội (camera không có đường cong: không phải RGB, hoặc chưa tải gói hoặc chưa được lưu vào bộ nhớ đệm).

---

## Các trường hợp ngoại lệ

Bắt lớp cơ sở để xử lý "bất kỳ sự cố nào xảy ra với Chloros":

```python
import chloros_sdk

try:
    chloros_sdk.process_folder("/path/to/folder")
except chloros_sdk.ChlorosAuthenticationError:
    print("Run `chloros-cli login` first.")
except chloros_sdk.ChlorosLicenseError:
    print("Chloros+ subscription required.")
except chloros_sdk.ChlorosError as e:
    print(f"Chloros error: {e}")
```

> `ChlorosAuthenticationError` và `ChlorosConfigurationError` là các thuộc tính của `chloros_sdk` (và của `chloros_sdk.exceptions`) nhưng không có trong `__all__`, do đó `from chloros_sdk import *` không đưa chúng vào: hãy nhập chúng theo tên. Gia đình ngoại lệ `ChlorosError` được kế thừa từ `ChlorosLocal` và các hàm được xây dựng dựa trên nó (`process_folder()`, `process_lattice_capture()`); bề mặt kết nối thông minh kế thừa từ `ChlorosConnectError`.

Cấu trúc phân cấp:

```

ChlorosError
├── ChlorosBackendError           (backend binary not found / failed to start)
├── ChlorosConnectionError        (HTTP transport failure; no backend running
│                                  with auto_start_backend=False)
├── ChlorosLicenseError           (403 PLAN_UPGRADE_REQUIRED: plan below Copper)
├── ChlorosAuthenticationError    (401 AUTH_REQUIRED: not logged in)
├── ChlorosConfigurationError     (bad configure() inputs)
└── ChlorosProcessingError        (pipeline failed; create/import refused)
    └── ChlorosProjectFilesMissingError  (open_project(): none of the
                                  project's source files found, even after
                                  the relink; .missing_roots, .audit)

ChlorosConnectError                (the smart-connect surface: connect_*, attach_array,
                                    list_*, discover_*, every CameraSession /
                                    ArraySession / DAQSensorSession method,
                                    RecorderHandle, ContinuousHandle,
                                    disconnect_all(), and analyze_array_network()
                                    when no backend answers; 401 and 403 included.
                                    Derives from plain Exception, NOT from
                                    ChlorosError, so `except ChlorosError` will
                                    not catch it. .warning: a warning the
                                    backend sent beside its error (also at the
                                    end of the message); .recording_closed: the
                                    recording a failed DAQ-E restart closed;
                                    both None otherwise)

FrameFetchError (RuntimeError)     (grab() / frame_stream() refusals; .status,
                                    .body, .errors)
ValueError                         (capture() arguments: PNG/JPEG with a level they
                                    cannot hold; a bad packed=)
FileNotFoundError / ImportError    (open_project(): no such folder / no project
                                    loaders)
RuntimeError                       (CameraHandle / ArrayHandle: a backend refusal)

lattice_sdk exceptions:
LatticeError
├── CameraNotFoundError
├── CameraConnectionError
├── StreamError
├── CaptureError
├── CalibrationError
├── NetworkError
└── DLSError
```

---

## Ví dụ từ đầu đến cuối

### 1. Xử lý một thư mục với thanh tiến trình tùy chỉnh

```python
from chloros_sdk import ChlorosLocal

def progress(percent, message):
    bar = "#" * (percent // 5)
    print(f"\r[{bar:<20s}] {percent:3d}% {message}", end="", flush=True)

with ChlorosLocal() as cl:
    cl.create_project("FieldA_2026-05-26")
    cl.import_images("C:/DroneImages/Flight001", recursive=True)
    cl.configure(
        debayer="High Quality (Faster)",
        vignette_correction=True,
        reflectance_calibration=True,
        indices=["NDVI", "GNDVI", "OSAVI", "SAVI"],
        export_format="TIFF (16-bit)",
    )
    cl.process(progress_callback=progress)
print()
```

### 2. Mảng LATTICE thời gian thực → Độ phản xạ + Tham chiếu DAQ

```python
import chloros_sdk

# Open a paired sensor first so the array's reflectance step has an
# absolute reference. Smart-detect picks USB / BLE / ETH automatically.
with chloros_sdk.connect_daq_sensor() as daq:
    with chloros_sdk.connect_array([
            "213800234", "214000533", "214701288", "214701292"
    ]) as arr:
        # Smart capture: wait for AE to settle, then snap
        arr.capture("./out", processing="reflectance", smart=True)

        # Record the corresponding DAQ frames as ground truth
        daq.record_start(output_dir="./out", device_name="sky-reference")
        # ... do whatever capture campaign ...
        info = daq.record_stop()
        print(info["path"], info["rows"])
```

### 3. Chiến dịch thu thập dữ liệu theo dự án

```python
import time, chloros_sdk

with chloros_sdk.open_project("/home/user/Chloros Projects/Field_A") as proj:
    report = proj.connect_all(verbose=True, align=True)
    if report["arrays"]["errors"]:
        raise SystemExit(f"Array(s) failed to connect: {report['arrays']['errors']}")

    rig = proj.arrays["main_rig"]

    # Re-align right before the campaign
    rig.calibrate_alignment(num_frames=5)
    rig.export_alignment("./alignments/main_rig.json")

    # 50 sequential single-frame captures at 2 fps
    for i in range(50):
        frames = rig.capture(
            output_dir=f"./out/frame_{i:04d}",
            processing="reflectance",
            apply_calibration=True,
            apply_white_balance=True,
        )
        time.sleep(0.5)

    # End-of-day: process the captured folder. process() accepts only
    # mode/wait/progress_callback/poll_interval — indices come from the
    # project's saved config (or set them via ChlorosLocal.configure()).
    proj.process()
```

### 4. Dòng khung hình từ nhiều camera → Quy trình xử lý NumPy

```python
import chloros_sdk
import numpy as np

with chloros_sdk.open_project("/path/to/proj") as proj:
    proj.connect_all()
    rig = proj.arrays["main_rig"]

    for frames in rig.frame_stream(
            processing="radiance",
            fps=5.0, count=300,
            apply_calibration=True,
            apply_white_balance=True):
        # frames is {serial: numpy_array}; cams not delivering this tick are omitted
        for serial, frame in frames.items():
            print(serial, frame.shape, frame.dtype, frame.mean())
```

### 5. Tập lệnh thu thập dữ liệu trực tiếp trên phần cứng (không cần backend)

```python
from chloros_sdk import LatticeCamera, PRESETS, discover_cameras

cams = discover_cameras(timeout_ms=3000)
print(f"Found {len(cams)} cams")

settings = PRESETS["high_quality"]
for c in cams:
    with LatticeCamera(serial=c.serial, settings=settings) as cam:
        result = cam.capture(output_dir="./out", format="tiff")
        print(c.serial, result.filepath)
```

### 6. Kiểm tra khả năng trước khi kết nối mảng 4 camera

```python
import chloros_sdk

serials = ["214701288", "213800234", "214000533", "214701162"]

probe = chloros_sdk.analyze_array_network(
    master_serial=serials[0],
    slave_serials=serials[1:],
    width=2048, height=1536,
    pixel_format="BayerRG12",
)

if probe["status"] == "ok":
    arr = chloros_sdk.connect_array(
        serials, width=2048, height=1536, pixel_format="BayerRG12")
elif probe["status"] == "auto_capped_fps":
    r = probe["recommended"]
    print(f"Keeping resolution; capping trigger rate at "
          f"{r['recommended_target_fps']} fps")
    arr = chloros_sdk.connect_array(
        serials, width=2048, height=1536, pixel_format="BayerRG12",
        target_fps=r["recommended_target_fps"])
elif probe["status"] == "auto_shrunk":
    r = probe["recommended"]
    print(f"Auto-shrinking to {r['out_width']}x{r['out_height']} "
          f"binning={r['binning']} for sim-sync")
    arr = chloros_sdk.connect_array(
        serials,
        width=r["width"], height=r["height"],     # before binning
        pixel_format=r["pixel_format"], binning=r["binning"])
elif probe["status"] == "needs_force_slip":
    print("Wire can't sustain sim-sync; falling back to slip mode")
    arr = chloros_sdk.connect_array(
        serials, force_tier="slip-emit-and-capture")
else:
    raise RuntimeError(f"Probe error: {probe.get('error')}")
```

### 7. Công thức ghi hình tương đương (Python thuần túy)

`run_project_recipe()` chạy công thức CLI nguyên bản (xem [Thư mục dự án & Công thức](#project-folder--recipes)). Khi được viết bằng Python, các bước tương tự như sau:

```python
import time, chloros_sdk

with chloros_sdk.open_project("/path/to/proj") as proj:
    proj.connect_all()
    cam = proj.cameras["FrontLeft"]
    rig = proj.arrays["main_rig"]
    sky = proj.sensors["Sky"]

    # apply: connect_all() applied each camera's saved settings. CameraHandle
    # has no settings method; a CameraSession on the same pooled camera does
    # (it is borrowed, so it never releases the camera)
    chloros_sdk.connect_camera(cam.serial).set_settings(
        exposure_time=5000, gain=6.0)

    # wait
    time.sleep(2)

    # capture
    cam.capture("pose_a/", format="tiff", processing="radiance")

    # stream
    rig.stream(count=60, fps=5, output_dir="stream/", processing="raw")

    # sensor read
    print(sky.read())
```

---

## Máy chủ MCP (Trợ lý AI)

`chloros-mcp` cho phép một trợ lý AI hỗ trợ [Giao thức Bối cảnh Mô hình](https://modelcontextprotocol.io) (MCP), chẳng hạn như Claude Desktop, Claude Code hoặc Cursor, điều khiển Chloros: xử lý hình ảnh (tạo dự án, nhập thư mục chứa các bản chụp, chọn cài đặt, xử lý, báo cáo kết quả xuất ra) và điều khiển camera LATTICE, mảng cảm biến và cảm biến ánh sáng DAQ (kết nối, xem trước, thiết lập độ phơi sáng và màu sắc, chụp ảnh, ghi lại, cập nhật firmware). Các công cụ của nó bao quát các chức năng mà SDK này thực hiện. Bạn chỉ cần yêu cầu bằng ngôn ngữ thông thường và trợ lý sẽ gọi các công cụ của máy chủ:

```text
You:  Process the flight in D:\Flights\2026-10-17 to reflectance and NDVI.

The assistant calls:
  create_project          {"project_name": "Flight_2026-10-17"}
  import_images           {"folder_path": "D:\\Flights\\2026-10-17"}
  configure_processing    {"indices": ["NDVI"]}
  start_processing        {}
  get_processing_status   {}      (every 30-60 s until its state is "finished")

You:  Show me what the array sees, then capture reflectance every 10 seconds
      for 5 minutes.

The assistant calls:
  list_devices            {}
  get_camera_preview      {"array_id": "array-1779862544497"}
  start_capture_run       {"array_id": "array-1779862544497", "mode": "interval",
                           "interval_s": 10, "duration_s": 300,
                           "processing": "reflectance"}
  get_job_status          {"job_id": "..."}   (until its state is "finished")
```

Máy chủ được xây dựng dựa trên SDK này: các công cụ xử lý sẽ gọi [`ChlorosLocal`](#image-processing--chloroslocal) và các công cụ phần cứng được [smart-connect gọi](#smart-connect-for-lattice-cameras), [các phiên DAQ](#daq-sensor-sessions), `ChlorosProject` và các [tiện ích camera](#camera-utilities), tất cả đều giao tiếp với cùng một hệ thống nền mà ứng dụng Chloros sử dụng. Các yêu cầu của nó mang mã `X-Chloros-Client: sdk`, do đó hệ thống nền sẽ áp dụng các kiểm tra đăng nhập và gói dịch vụ của SDK, cùng với các quy tắc của SDK (ví dụ: tính năng Texture Aware bị từ chối đối với các bản ghi hình từ LATTICE).

### Cài đặt

Máy chủ được cung cấp trong gói `chloros-sdk`; không cần cài đặt thêm bất kỳ thành phần nào khác. SDK sẽ cài đặt lệnh `chloros-mcp`, tương đương với `python -m chloros_sdk.mcp_server`:

```bash
pip install chloros-sdk        # not needed where the Chloros installer set up the SDK (Step 2)
chloros-mcp --help
```

- **Windows:** Trình cài đặt Chloros sẽ cài đặt SDK thông qua pip vào môi trường Python của bạn, từ đó đặt tệp `chloros-mcp.exe` vào thư mục `Scripts` của Python đó.
- **Hệ điều hành Linux (.deb):** gói phần mềm này cài đặt SDK cho hệ thống `python3` và đặt lệnh `chloros-mcp` vào thư mục `/usr/bin`, do đó lệnh này có sẵn trong thư mục `PATH`. Trong môi trường ảo (virtualenv), `pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl` sẽ thêm `chloros-mcp` vào thư mục `bin` của môi trường ảo.

Máy chủ chỉ sử dụng thư viện tiêu chuẩn dựa trên SDK (Python 3.7+). Một client MCP khởi động máy chủ và giao tiếp với nó qua stdin/stdout; bạn không cần tự chạy nó, trừ khi sử dụng `--help`.

### Cài đặt máy khách

**Claude Desktop:**thêm máy chủ vào `claude_desktop_config.json` (trên Windows là `%APPDATA%\Claude\claude_desktop_config.json`; vào**Cài đặt > Nhà phát triển > Chỉnh sửa cấu hình** để mở), sau đó khởi động lại Claude Desktop:

```json
{
  "mcpServers": {
    "chloros": {"command": "chloros-mcp"}
  }
}
```

**Claude Code:**

```bash
claude mcp add chloros -- chloros-mcp
```

**Con trỏ:** mục `mcpServers` giống như trong Claude Desktop, tại `~/.cursor/mcp.json` (cho mọi dự án; `%USERPROFILE%\.cursor\mcp.json` trên Windows) hoặc `.cursor/mcp.json` trong một dự án.

Các tùy chọn được đặt trong `args`, hoặc sau lệnh cho Claude Code (`claude mcp add chloros -- chloros-mcp --no-auto-start`). Để sử dụng SDK cụ thể của Python thay vì phiên bản mặc định trên hệ thống, hãy chạy mô-đun với phiên bản Python đó (đường dẫn đầy đủ của nó dưới dạng `command` nếu nó không phải là phiên bản đầu tiên `python3` trên `PATH`):

```json
{
  "mcpServers": {
    "chloros": {"command": "python3", "args": ["-m", "chloros_sdk.mcp_server"]}
  }
}
```

Một máy khách khởi động máy chủ mà không cần vỏ lệnh (shell) của bạn, vì vậy nếu máy khách báo cáo rằng máy chủ không khởi động được, có thể `chloros-mcp` không nằm trên `PATH` mà máy khách nhìn thấy. Thay vào đó, hãy cung cấp đường dẫn đầy đủ (`where chloros-mcp` trên Windows, `which chloros-mcp` trên Linux, trong đó tệp .deb nằm tại `/usr/bin/chloros-mcp`), hoặc chạy mô-đun với phiên bản Python đã cài đặt SDK.

### Tùy chọn

| Tùy chọn | Mặc định | Chức năng |
| --- | --- | --- |
| `--backend-url URL` | `http://127.0.0.1:5000` | Backend Chloros cần sử dụng. |
| `--no-auto-start` | tắt | Không bao giờ khởi động backend: sử dụng backend đang chạy sẵn, chẳng hạn như của ứng dụng Chloros, dịch vụ backend Linux, hoặc một đường hầm đến thư mục trên máy khác (xem [Chế độ Backend Từ Xa](#remote-backend-mode-pip-only-host-via-tunnel); trong trường hợp này, đường dẫn thư mục phải trỏ đến cùng một thư mục trên cả hai máy). |
| `--backend-exe PATH` | nơi Chloros cài đặt nó | Dịch vụ nền cần khởi động khi không có dịch vụ nào đang chạy, dành cho trường hợp Chloros được cài đặt ở vị trí mà SDK không tìm thấy. |
| `--log-level LEVEL` | `WARNING` | `DEBUG`, `INFO`, `WARNING` hoặc `ERROR`. Nhật ký được ghi vào stderr, mà hầu hết các client hiển thị trong nhật ký MCP của chúng; stdout chỉ chứa các thông điệp giao thức. |

### Công cụ

Các tham số của mỗi công cụ chính là các tham số của lệnh SDK mà công cụ đó bao bọc; trình khách liệt kê từng tham số kèm theo lược đồ đầy đủ của nó. Đường dẫn là đường dẫn tuyệt đối.

**Xử lý**

| Công cụ | Lệnh SDK | Chức năng |
| --- | --- | --- |
| `create_project` | `ChlorosLocal.create_project` | Tạo một dự án và đặt nó làm dự án đang mở, mà các công cụ xử lý sẽ tác động lên. Một tên đơn giản sẽ tạo thư mục trong thư mục dự án mặc định; một đường dẫn tuyệt đối sẽ tạo thư mục đó tại vị trí chỉ định. |
| `open_project` | `ChlorosLocal.open_project` | Mở một dự án đã lưu và đặt nó làm dự án đang mở. Các đường dẫn của dự án đã di chuyển sẽ được điều chỉnh lại trỏ đến thư mục hiện tại của nó; `relink_folder` (tên tuyệt đối) chỉ định thư mục hiện tại chứa các tệp còn thiếu khác. Trả về `source_files` và `image_count`. Không tìm thấy: lỗi liệt kê vị trí của chúng và đặt tên là `relink_folder`; không có dự án nào được mở. |
| `import_images` | `import_images` | Nhập các cặp tệp `.RAW`+`.JPG` từ Survey3, LATTICE `.tif` ghi lại và các bản ghi DAQ `.daq`; trả về `count` (hình ảnh) và `scan_count` (bản ghi). |
| `configure_processing` | `configure` | Thay đổi các thiết lập được truyền vào và không thay đổi các thiết lập khác (giá trị: [Giá trị được hỗ trợ](#supported-values)); `custom_settings` gửi toàn bộ cấu hình như đã chỉ định. |
| `get_project_settings` | `get_config` | Đọc các cài đặt xử lý, xuất và chỉ mục đã lưu của dự án đang mở. |
| `start_processing` | `process(wait=False)` | Bắt đầu xử lý dự án đang mở và trả về ngay lập tức. |
| `get_processing_status` | `get_status` | Báo cáo `state` (`idle`, `starting`, `processing` hoặc `finished`) và tiến trình; khi hoàn tất, phiên chạy sẽ ghi lại các mã lỗi `summary`, `export_check_failure` khi không thể xác minh được dữ liệu xuất ra, và `image_failure` khi hình ảnh bị mất: các sản phẩm còn lại vẫn được ghi lại; báo cáo phiên chạy là thất bại. |
| `process_folder` | `process_folder` / `process_lattice_capture` | Tạo dự án cho một thư mục, nhập dự án, cấu hình và khởi chạy trong một lệnh duy nhất (`lattice: true` dành cho việc thu thập dữ liệu LATTICE). Trong khi quá trình chạy đang diễn ra, hệ thống sẽ từ chối thực hiện bất kỳ thao tác nào trước khi tạo ra bất kỳ dữ liệu nào, và hệ thống nền cũng từ chối việc thay đổi dự án từ bất kỳ công cụ nào trong trường hợp đó. |
| `export_light_sensor` | `export_light_sensor` | Ghi các tệp `.daq` và `.csv` đã hiệu chuẩn cho các bản ghi cảm biến ánh sáng mà không xử lý hình ảnh, lấy dữ liệu hiệu chuẩn nhà máy từ MAPIR Cloud khi dữ liệu này không được lưu trong bộ nhớ đệm (xem [Ghi dữ liệu cảm biến ánh sáng](#light-sensor-recordings--calibrated-daq--csv)). |
| `read_image_audit_tags` | `read_image_audit_tags` | Đọc siêu dữ liệu kiểm tra Chloros của hình ảnh đã xử lý (nguồn, hiệu chuẩn, các giai đoạn đã áp dụng). |

**Máy ảnh và mảng cảm biến**

| Công cụ | Lệnh SDK | Chức năng |
| --- | --- | --- |
| `list_devices` | `list_cameras`, `list_arrays`, `list_daq_sensors` | Các thiết bị hiện đang kết nối, trên bất kỳ máy khách nào, mỗi thiết bị đều có `connected_here`; cùng với các tác vụ đang chạy trên máy chủ. |
| `discover_devices` | `discover_lattice_cameras`, `discover_daq_sensors` | Các camera và cảm biến ánh sáng có thể được kết nối. |
| `get_device_status` | `CameraSession.status`, `ArraySession.status` / `capability`, `DAQSensorSession.status` | Trạng thái trực tiếp của một camera, mảng camera (kèm theo khả năng và tình trạng hoạt động) hoặc cảm biến ánh sáng. |
| `analyze_array_network` | `analyze_array_network` | Khả năng hỗ trợ của máy chủ và mạng này đối với một mảng được đề xuất. |
| `connect_camera` / `connect_array` | `connect_camera` / `connect_array` | Kết nối một camera hoặc một mảng đồng bộ (máy chủ trước). |
| `connect_project_devices` | `open_project(...).connect_all` | Kết nối mọi thiết bị mà dự án đã lưu ghi nhớ, cùng với các cài đặt của chúng. |
| `disconnect_device` | `disconnect` / `force_disconnect` | Ngắt kết nối camera, mảng hoặc cảm biến ánh sáng (xem Hành vi). `keep_settings: true` trên một mảng lưu cài đặt của từng camera vào bộ nhớ riêng của nó, `clear_settings: true` đặt lại bộ nhớ đó (kết quả của nó mang giá trị `camera_memory`). |
| `disconnect_project_devices`, `align_project_arrays`, `capture_project_devices` | `disconnect_all`, `align_arrays`, `capture_all` | Các thiết bị của dự án, cùng nhau. |
| `read_camera_settings` / `set_camera_settings` | `CameraSession.read_nodes` / `set_settings` | Đọc hoặc ghi cài đặt máy ảnh, theo tên thân thiện hoặc tên nút GenIC. |
| `set_array_frame_rate` | `ArraySession.set_target_fps` | Cố định tốc độ của mảng hoặc khôi phục về tốc độ tự động. |
| `capture_camera` / `capture_array` | `CameraSession.capture` / `ArraySession.capture` | Một lần chụp (`fastest` và `wait_for_exposure` là `capture_fastest` và lần chụp với thời gian phơi sáng đã được thiết lập). Nếu không có `output_dir`, nó sẽ lưu vào thư mục mở của dự ánthư mục `captures` của dự án đang mở, và các tệp sẽ được thêm vào dự án (được giữ lại cho đến khi quá trình xử lý kết thúc); phản hồi `project_add` xác nhận điều này. |
| `start_capture_run` | `ArraySession.continuous`, `capture_repeated` | Chụp liên tục hoặc theo khoảng thời gian, dưới dạng một tác vụ. |
| `start_array_recording` | `ArraySession.record` / `burst` | Video chỉ mục kết hợp hoặc chuỗi ảnh thô, dưới dạng tác vụ. |
| `build_burst_video` | `ArraySession.build_video` | Xử lý lại một loạt ảnh thành video đã hiệu chỉnh, dưới dạng tác vụ. |
| `get_job_status` / `stop_job` | | Theo dõi hoặc dừng một tác vụ. |
| `get_camera_preview` | | Xem trước trực tiếp từ mỗi camera, được trả về dưới dạng hình ảnh. |
| `grab_frames` | `ArraySession.grab`, `CameraHandle.grab` | Một khung hình (hoặc một bộ đồng bộ) ở mức xử lý, không lưu: thống kê theo từng mặt phẳng và tệp PNG của từng khung hình. |
| `calibrate_array_alignment`, `get_array_alignment`, `clear_array_alignment`, `nudge_array_alignment`, `export_array_alignment`, `import_array_alignment` | `ArrayHandle.calibrate_alignment`, `alignment_status`, `clear_alignment`, `tweak_alignment`, `export_alignment`, `import_alignment` | Căn chỉnh mảng. |

**Cảm biến ánh sáng**

| Công cụ | Lệnh SDK | Chức năng |
| --- | --- | --- |
| `connect_light_sensor` | `connect_daq_sensor` | Kết nối DAQ-U, DAQ-M hoặc DAQ-E (`cap_id` thiết lập hiệu chỉnh giới hạn). |
| `get_light_sensor_reading` | `DAQSensorSession.latest` | Các giá trị đo mới nhất: dấu thời gian, mức bão hòa, dải phổ và đỉnh phổ, cùng phổ được trung bình theo các khoảng bước sóng (`full_spectrum` cho từng điểm). |
| `set_light_sensor_streaming` | `stream_start` / `stream_stop` | Bắt đầu hoặc dừng luồng dữ liệu. |
| `start_light_sensor_recording` / `stop_light_sensor_recording` | `record_start` / `record_stop` | Ghi tệp `.daq` dưới dạng tác vụ; `duration_s` sẽ tự động dừng nó. |
| `set_light_sensor_cap` | `DAQSensorSession.set_cap` | Hiệu chỉnh giới hạn (`none` dành cho cảm biến trần). |
| `get_light_sensor_bindings` / `set_light_sensor_bindings` | `daq_bindings` / `set_daq_bindings` | Cảm biến nào cung cấp dữ liệu phản xạ cho từng camera và mảng. |
| `get_light_sensor_band_presets` / `get_light_sensor_reflectance` | `daq_band_presets` / `light_sensor_reflectance` | Các dải phổ, cùng với giá trị phản xạ và chỉ số của môi trường xung quanh và đối tượng từ hai cảm biến truyền dữ liệu liên tục. |
| `get_light_sensor_device_info`, `set_light_sensor_network`, `set_light_sensor_auto_stream`, `enable_light_sensor_ptp`, `reboot_light_sensor`, `update_light_sensor_firmware` | `device_info`, `set_network`, `set_auto_stream`, `enable_ptp`, `reboot`, `update_firmware` | Cài đặt riêng của DAQ-E, được lưu trong thiết bị. |

**Các tiện ích máy ảnh** ([Các tiện ích máy ảnh](#camera-utilities))

| Công cụ | Lệnh SDK | Chức năng |
| --- | --- | --- |
| `get_camera_color_settings` / `set_camera_color_settings` | `camera_color_profile`, `camera_color_adjust`, `camera_highlights` và các hàm thiết lập tương ứng | Màu hiển thị của camera RGB: hồ sơ và nhiệt độ màu, độ bão hòa / độ tương phản, vùng sáng. |
| `run_camera_white_balance` / `set_camera_white_balance` | `camera_white_balance_once` / `set_camera_white_balance` | Cân bằng trắng một lần, hoặc tự động / tắt / tỷ lệ cố định. |
| `list_camera_presets` / `apply_camera_preset` | `camera_presets` / `apply_camera_preset` | Cài đặt sẵn của máy ảnh. |
| `list_filter_models` | `camera_filter_models`, `camera_filter_model_info` | Các mẫu bộ lọc, hoặc chọn theo tên. |
| `get_camera_firmware_status` / `update_camera_firmware` | `camera_firmware_status` / `update_camera_firmware` | Phiên bản phần mềm và đèn flash (một tác vụ theo mặc định). |
| `get_camera_power_info` / `set_camera_power` | `camera_power_probe`, `sleep_camera`, `wake_camera` | Trạng thái nguồn và nhiệt; chế độ ngủ hoặc thức. |
| `calibrate_camera_with_target` | `calibrate_camera_with_target` | Hệ số từ mục tiêu hiệu chuẩn đang hiển thị. |
| `get_network_adapters` / `fix_network_settings` | `network_adapters` / `fix_network_settings` | Các bộ điều hợp mạng của máy ảnh, ước tính và bản sửa lỗi (Chỉ dành cho quản trị viên). |
| `apply_vignette_correction` | `apply_vignette_correction` | Chỉnh sửa hiệu ứng tối góc cho một thư mục hình ảnh. |
| `read_dls` | `read_dls` | Một lần đọc DLS. |
| `probe_system` / `get_system_info` | `probe_system` / `system_info` | Phần cứng và môi trường chạy của máy chủ. |

**Camera tạm thời, cấu hình căn chỉnh và chỉ số** ([Mảng tạm thời, cấu hình căn chỉnh & chỉ số](#transient-arrays-alignment-profiles--indices)). Mọi công cụ ở đây khi mở camera đều sử dụng `keep_settings` / `clear_settings` (mặc định là false): xem [Bộ nhớ camera](#camera-memory--keep_settings--clear_settings).

| Công cụ | Lệnh SDK | Chức năng |
| --- | --- | --- |
| `start_camera_continuous` / `start_multi_camera_stream` | `start_camera_continuous` / `start_multi_camera_stream` | Ghi hình từ một camera hoặc phát trực tiếp đồng bộ nhiều camera dưới dạng một tác vụ (`duration_s` sẽ tự động dừng tác vụ này). |
| `capture_cameras_transient` | `capture_cameras_transient` | Ghi hình đồng bộ một lần mà không cần kết nối mảng camera. |
| `test_array_sync` / `detect_array_master` | `test_array_sync` / `detect_array_master` | Kiểm tra đồng bộ tín hiệu kích hoạt và xác định các camera đang nằm trên đường truyền đồng bộ. |
| `analyze_array_models` | `analyze_array_models` | Một mảng gồm các mẫu này sẽ hoạt động như thế nào trên mạng này. |
| `calibrate_alignment_profile` | `calibrate_alignment_profile` | Giải quyết và lưu tệp cấu hình căn chỉnh (`options` chứa các tùy chọn giải quyết chi tiết hơn). |
| `capture_aligned_multiband` / `start_aligned_stream` | `capture_aligned_multiband` / `start_aligned_stream` | Một hình ảnh đa dải đã được căn chỉnh, hoặc một chuỗi các hình ảnh như vậy dưới dạng một tác vụ. |
| `read_alignment_profile` / `reorder_alignment_profile` | `alignment_profile_info` / `reorder_alignment_profile` | Nội dung của tệp cấu hình; thứ tự dải và các camera được kích hoạt. |
| `list_index_presets`, `compute_index`, `capture_live_index` | `index_catalog`, `compute_index`, `capture_live_index` | Các cài đặt sẵn và độ dốc màu; chỉ mục cho tệp TIFF đa dải; chụp và tạo chỉ mục trong một bước. |

**Đồng bộ hóa thời gian, thư mục dự án và công thức** ([PTP Grandmaster](#ptp-grandmaster--time_sync_status), [Thư mục dự án & Công thức](#project-folder--recipes))

| Công cụ | Lệnh SDK | Chức năng |
| --- | --- | --- |
| `get_time_sync_status` | `time_sync_status`, `time_sync_peers`, `time_sync_cameras` | Tình trạng hoạt động của máy chủ chính PTP, các máy chủ ngang hàng và trạng thái PTP của từng camera mảng. |
| `restart_time_sync` / `set_time_sync_priority` | `restart_time_sync` / `set_time_sync_priority` | Khởi động lại nó; thiết lập mức độ ưu tiên của nó. |
| `get_project_folder` / `set_project_folder` / `reset_project_folder` | `project_folder` / `set_project_folder` / `reset_project_folder` | Thư mục để tạo các dự án mới. |
| `run_project_recipe` | `run_project_recipe` | Chạy công thức của một dự án đã lưu dưới dạng tác vụ kèm theo nhật ký của nó. Kiểm tra trước tiên: công thức có lỗi sẽ bị từ chối và không thực thi gì cả; các mảng / cảm biến chưa kết nối sẽ được cảnh báo (chương trình không kết nối chúng: thực hiện `connect_project_devices` trước). `dry_run` kết thúc quá trình kiểm tra và không thực thi gì cả. |

Không có công cụ nào để mở dự án đã lưu để xử lý: mỗi tác vụ xử lý bắt đầu bằng `create_project` (hoặc `process_folder`).

### Yêu cầu

- **Chloros phải được cài đặt trên cùng một máy tính.** Phần backend của Chloros thực hiện việc xử lý. Máy chủ cũng có thể sử dụng một backend được kết nối qua đường hầm (`--backend-url` kết hợp với `--no-auto-start`, xem [Tùy chọn](#options)).
- **Đã đăng nhập trên máy tính đó**, trong ứng dụng Chloros hoặc bằng `chloros-cli login`. Máy chủ không thể đăng nhập thay cho bạn: nếu công cụ được gọi khi bạn chưa đăng nhập, nó sẽ trả về thông báo "Chưa đăng nhập vào Chloros" kèm lý do từ backend, và trợ lý sẽ thông báo cho bạn.
- **Bất kỳ gói Chloros+ trả phí nào (Copper trở lên).** Máy chủ có quyền truy cập SDK, trong khi gói miễn phí Iron không có: khi gọi công cụ trên gói miễn phí, hệ thống sẽ trả lời "Không khả dụng trên gói Chloros này" kèm lý do từ hệ thống nền.
- Python 3.7 trở lên với SDK và một máy khách MCP khởi động các máy chủ cục bộ (stdio).

### Cách hoạt động

- **Hệ thống nền (backend).** Việc khởi động máy chủ hoặc liệt kê các công cụ của nó sẽ không khởi chạy bất kỳ tác vụ nào; chỉ khi có yêu cầu công cụ đầu tiên thì kết nối mới được thiết lập. Nếu ứng dụng Chloros (hoặc dịch vụ nền trên Linux) đang chạy, máy chủ sẽ sử dụng hệ thống nền của ứng dụng đó và để nó tiếp tục chạy. Nếu không có backend nào đang chạy, máy chủ sẽ khởi động backend đã cài đặt, ở chế độ không cửa sổ. Khi máy chủ thoát (bạn tắt client hoặc máy chủ được khởi động lại), nó sẽ dừng các tác vụ thu thập dữ liệu và ghi dữ liệu cảm biến ánh sáng mà nó đã khởi tạo, đồng thời ngắt kết nối các thiết bị đã kết nối; backend vẫn tiếp tục chạy, do đó quá trình xử lý chưa hoàn tất sẽ tiếp tục cho đến khi kết thúc, và nó sẽ tự động dừng sau 30 phút nếu không có tác vụ nào sử dụng nó. Với phiên bản `--no-auto-start`, máy chủ sẽ không bao giờ khởi động dịch vụ nền: khi gọi công cụ trong khi không có dịch vụ nền nào đang chạy, hệ thống sẽ yêu cầu bạn mở ứng dụng Chloros, và lần gọi tiếp theo sau khi bạn thực hiện sẽ thiết lập kết nối.
- **Một dự án đang mở.** Phần nền có một dự án đang mở, được chia sẻ với ứng dụng Chloros khi ứng dụng đang mở, vì vậy đừng làm việc trên một dự án khác trong ứng dụng khi trợ lý đang hoạt động.
- **Chỉ các thiết lập được truyền vào mới thay đổi.** `configure_processing` giữ nguyên giá trị đã lưu của mọi cài đặt không được truyền vào (SDK `configure()` sẽ đặt lại các cài đặt như debayer, chỉnh sửa hiệu ứng viền tối, hiệu chuẩn phản xạ, PPK và định dạng xuất về giá trị mặc định khi chúng không được truyền vào; máy chủ sẽ điền các giá trị này từ dự án). Nếu không thể đọc được các thiết lập đã lưu của dự án, nó sẽ không thay đổi gì và thông báo điều đó, trừ khi cả năm thiết lập đều được chỉ định. `indices` thay thế danh sách chỉ mục của dự án. Kết quả liệt kê những gì đã được gửi dưới dạng `settings_sent`, và đặt tên dưới `warnings` cho bất kỳ chỉ mục nào mà các dải quang phổ của máy ảnh không thể tạo ra.
- **Quá trình xử lý theo kiểu “bắt đầu rồi kiểm tra tiến độ”.** `start_processing` trả về ngay khi quá trình chạy bắt đầu, sau đó mất từ vài phút đến vài giờ, và trợ lý sẽ tiếp tục với lệnh `get_processing_status`. `state` là `idle` (không có dữ liệu đang xử lý), `starting` (hệ thống nền chưa báo cáo kết quả chạy, tối đa 20 giây), `processing` hoặc `finished`, kèm theo [tóm tắt](#post-run-summary--hints): những gì đã được xuất ra, và các gợi ý khi kết quả xuất ra ít hơn dự kiến (không có hình ảnh, không tìm thấy mục tiêu hiệu chuẩn). Các sản phẩm được lưu trong thư mục dự án, giống như trong ứng dụng. `start_processing` sẽ bị từ chối khi một lần chạy đang diễn ra, và một hệ thống nền ngừng phản hồi sẽ được báo cáo là lỗi, không phải là lần chạy đã hoàn thành.
- **Những gì trợ lý quan sát được.** Các cảnh báo từ SDK sẽ xuất hiện trong kết quả công cụ dưới dạng `warnings`, còn các lỗi (đã đăng xuất, kế hoạch, đường dẫn không hợp lệ) sẽ được hiển thị dưới dạng văn bản lỗi mà trợ lý có thể đọc và chuyển tiếp cho bạn. Danh sách có hơn 20 mục sẽ bị cắt xuống còn 20 mục đầu tiên, với độ dài đầy đủ được ghi là `<key>_total`. Đường dẫn phải là đường dẫn tuyệt đối.
- **Thiết bị được chia sẻ.** Camera, mảng và cảm biến ánh sáng đều nằm ở phía máy chủ, do đó trợ lý có thể điều khiển một thiết bị đã kết nối qua ứng dụng Chloros, và mọi thay đổi sẽ hiển thị trong ứng dụng. `disconnect_device` hiển thị thông tin về máy chủ đã kết nối; một thiết bị được kết nối bởi một khách hàng khác (ứng dụng, `chloros-cli`, một tập lệnh, một `chloros-mcp` khác) chỉ được giải phóng bằng `force: true`, tức là `force_disconnect` của SDK, và tên của người đã kết nối thiết bị đó sẽ được ghi lại khi hệ thống backend biết; một thiết bị mà hệ thống backend hoàn toàn không lưu trữ sẽ được báo cáo là chưa kết nối (`released: false`, `connected: false`, không có gì để hiển thị). Một bản ghi hoặc luồng mà hệ thống backend đang chạy sẽ dẫn đến chế độ xem `get_job_status` kèm theo các giá trị `counts` — `frames_written`, `frames_dropped`, `frames_failed`, `drop_reason` (các nhóm khung hình cho luồng đa camera) — và một dòng `summary` ("220 khung hình đã ghi, 55 khung hình bị bỏ qua, 0 khung hình thất bại: ..."), trong khi nó đang chạy và, khi hoàn tất, các con số cuối cùng của nó; danh sách của một khách hàng khác về nó (`other_clients_jobs`) cũng hiển thị thông tin tương tự (`list_cameras()`, `list_arrays()` và `list_daq_sensors()` hiển thị thông tin của từng thiết bị`connected_by`: `client`, `label`, `name`, `at`). Mỗi `chloros-mcp` tự đặt tên cho mình với hệ thống backend là `chloros-mcp (pid N)` (`chloros_sdk.connect.CLIENT_LABEL`, được gửi dưới dạng `X-Chloros-Client-Label`; bất kỳ tập lệnh SDK nào cũng có thể thiết lập giá trị này). Việc dừng ghi dữ liệu cảm biến ánh sáng của một client khác cũng cần đến `force`.
- **Công việc kéo dài là một tác vụ.** `start_capture_run`, `start_array_recording`, `build_burst_video`, `start_light_sensor_recording`, `start_camera_continuous`, `start_multi_camera_stream`, `start_aligned_stream`, `run_project_recipe`, và bất kỳ lệnh capture, solve hoặc index nào yêu cầu `background`, sẽ trả về `job_id` ngay lập tức; `get_job_status` tiếp theo và `stop_job` kết thúc nó. Quá trình chạy sẽ tiếp tục cho đến khi bị dừng trừ khi bạn cung cấp `max_captures`, `duration_s` (hoặc, đối với chế độ bùng nổ, `max_frames`); `min_free_gb` sẽ từ chối khởi động khi ổ đĩa còn ít dung lượng trống. Không có giới hạn nào được áp dụng trừ khi bạn thiết lập. Một bản ghi hoặc luồng mà backend đang chạy (`start_camera_continuous`, `start_multi_camera_stream`, `start_aligned_stream`) sẽ giữ lại ID tác vụ của backend, do đó một tác vụ `chloros-mcp` khác — được khởi chạy sau đó — sẽ tìm thấy nó thông qua ID đó: `get_job_status` sẽ tiếp nối nó (và liệt kê các tác vụ như `other_clients_jobs`, từ `GET /api/camera/cli-jobs`, mỗi tác vụ có `started_by`), và `stop_job` chỉ dừng nó bằng `force: true`. Bất kỳ tác vụ nào khác (một lần chạy thu thập, ghi mảng hoặc ghi liên tục, thu thập nền) sẽ chạy trong `chloros-mcp` đã khởi chạy nó, và chỉ máy chủ đó mới biết điều này: một máy chủ khác yêu cầu nó sẽ xác nhận điều đó. Một tác vụ đã dừng sẽ đọc `stopping` trong khi ghi ra hàng đợi của nó, sau đó là `finished` với số liệu cuối cùng, trong 10 phút và từ bất kỳ máy chủ `chloros-mcp` nào: máy chủ khởi tạo cũng nhận thấy việc dừng bắt buộc của máy chủ khác theo cách tương tự. `get_job_status` không bao giờ chờ đợi một lệnh dừng: trong quá trình dừng (của chính nó `stop_job`, của một client khác, hoặc lệnh dừng giải phóng các camera của một công việc đã tự kết thúc), nó trả lời ngay lập tức bằng `stopping` cùng số đếm trực tiếp, và `stop_job` của một công việc đang dừng sẽ thông báo điều đó. Một công việc đã kết thúc sẽ nêu lý do trong `stop_reason`: lý do riêng của nó (`no_frames`, `disk_full`, `member_missing`, hoặc đối với chuỗi chạy liên tục của mảng: `count`, `duration`, `array_gone`), `limit reached` chỉ khi số đếm của nó hiển thị giá trị `count` (hoặc `max_frames` của một đợt, `duration_s` của một bản ghi`duration_s`) đạt đến, `error`, `stopped` (với `stopped_by`) khi khách hàng dừng quá trình ghi, nếu không thì là `finished`.
- **Nơi lưu trữ các bản ghi.** Vào thư mục `captures` của dự án đang mở, nơi ứng dụng lưu trữ chúng, trừ khi được chỉ định `output_dir`.
- **Hình ảnh.** `get_camera_preview` và `grab_frames` trả về các hình ảnh mà trợ lý có thể xem, bên cạnh dữ liệu JSON của chúng. Hình ảnh `grab_frames` được điều chỉnh độ tương phản để hiển thị; các số liệu thống kê của chúng là các giá trị thực.
- **Khi máy chủ thoát**, các tác vụ mà nó đã khởi chạy sẽ dừng lại và bản ghi cảm biến ánh sáng mà nó đã khởi tạo sẽ được đóng lại. Nếu máy chủ đã khởi chạy phần nền, nó cũng sẽ ngắt kết nối các thiết bị đã kết nối trước khi dừng phần nền; trong phần nền của ứng dụng, các thiết bị này vẫn duy trì kết nối.
- **Giao thức.** Chỉ hỗ trợ stdio (không có điểm cuối HTTP), chỉ dành cho công cụ (không có tài nguyên hoặc lời nhắc), kết quả dưới dạng văn bản và hình ảnh, các phiên bản giao thức MCP từ 2024-11-05 đến 2025-11-25.**Những gì bị loại trừ.** Đăng nhập hoặc đăng xuất (tài khoản thuộc về bạn; máy chủ không thể đăng nhập), và dừng một backend mà máy chủ không khởi động. Mọi tác vụ khác mà SDK này thực hiện liên quan đến xử lý và phần cứng đều có công cụ tương ứng. (Việc cấu hình DAQ-E — tên, token xác thực, mật khẩu OTA, yêu cầu chữ ký, khôi phục cài đặt gốc — và các máy chủ DAQ trực tiếp chỉ dành riêng cho `chloros-cli`, cũng có trong SDK: tên hoặc mã thông báo sai sẽ khiến mọi bản cài đặt Chloros bị khóa khỏi thiết bị.)

---

## Tự động khởi động dịch vụ nền

Các điểm vào smart-connect — `connect_camera`, `connect_array`, `connect_daq_sensor`, `discover_lattice_cameras` và `discover_daq_sensors` — là các máy khách HTTP nhẹ, giả định rằng một backend đang lắng nghe tại `127.0.0.1:5000` (URL mặc định của giao diện smart-connect). Khi GUI hoặc CLI đã đang chạy, thì đã có một máy chủ như vậy hoạt động. Từ một tập lệnh trần, có thể không có — vì vậy các hàm này **tự động khởi động tệp nhị phân máy chủ đi kèm** (chế độ không cửa sổ, giống như cách `ChlorosLocal` thực hiện) trước lần gọi đầu tiên, sau đó chờ tối đa 60 giây để máy chủ khởi động. `list_cameras()`, `list_arrays()`, `list_daq_sensors()`, `attach_array()` và `analyze_array_network()` không bao giờ khởi động tệp nhị phân này.

Quy tắc:

- **Chỉ có URL cục bộ mới được khởi tạo.** Một `backend_url` trỏ đến `localhost` / `127.0.0.1` / `[::1]` mới được chấp nhận; bất kỳ máy chủ nào khác đều được coi là máy của người khác và sẽ không bao giờ được khởi tạo.
- **Nếu một backend thoát trước khi trả lời, quá trình chờ sẽ kết thúc.** Lệnh gọi sẽ trả về ngay lập tức với mã thoát, lệnh đã thực thi và các dòng cuối cùng mà backend in ra, thay vì chờ hết 60 giây. Mã thoát 98 là trường hợp ngoại lệ: một backend khác đã chiếm giữ cổng hoặc tệp khóa (của ứng dụng Chloros, dịch vụ backend hoặc một skript khác vẫn đang khởi động), do đó cuộc gọi tiếp tục chờ backend đó trả lời. `ChlorosLocal` hoạt động tương tự.
- **Backend được để chạy để tái sử dụng** (giống như trên giao diện dòng lệnh CLI) — không có quá trình tắt máy ngầm định khi skript của bạn kết thúc. Việc chạy lại skript sẽ tái sử dụng backend đang hoạt động.
- **Tắt tính năng này bằng `auto_start_backend=False`** trên bất kỳ lệnh gọi nào trong số đó (ví dụ: khi bạn trỏ đến một backend từ xa, hoặc bạn tự quản lý vòng đời của backend).

```python
import chloros_sdk

# Fresh shell, no backend running, no GUI open — this still works:
with chloros_sdk.connect_camera("213800234") as cam:   # spawns the backend
    cam.capture("output/")

# Remote backend (via tunnel — see Remote-Backend Mode): don't spawn one locally
arr = chloros_sdk.connect_array(serials,
                                backend_url="http://127.0.0.1:5000",
                                auto_start_backend=False)
```

Nếu không thể tìm thấy hoặc khởi động tệp nhị phân đi kèm, cuộc gọi HTTP tiếp theo sẽ gây ra một lỗi `ChlorosConnectError` có thể xử lý được và **phù hợp với nền tảng** thay vì chỉ là một bản ghi lỗi từ chối kết nối đơn thuần — trên Windows, nó sẽ hướng dẫn bạn đến ứng dụng trên desktop hoặc lệnh `chloros-cli`; trên Linux (không có giao diện đồ họa), nó sẽ hướng dẫn bạn đến lệnh `chloros-cli` hoặc `.deb`.

---

## Môi trường & Tiêu đề

SDK gắn nhãn mọi yêu cầu HTTP phía máy chủ bằng `X-Chloros-Client: sdk`. Phía máy chủ áp dụng các quy tắc cấp phép của SDK/CLI (yêu cầu đăng nhập **và** gói Chloros+ trả phí) thay vì lộ trình miễn phí của giao diện đồ họa. Thiết lập này được thực hiện tự động tại thời điểm nhập — bạn không cần thực hiện bất kỳ thao tác nào.

Tiêu đề này được thêm vào mọi yêu cầu `requests` / `urllib` có URL chứa `localhost` hoặc `127.0.0.1`, trên bất kỳ cổng nào — bao gồm cả các dịch vụ cục bộ của bạn — và thay thế giá trị mà bạn đã tự thiết lập. Các cuộc gọi đến các máy chủ khác sẽ không bị thay đổi.

Ghi đè URL của backend bằng cách truyền `backend_url=` (`ChlorosLocal` chấp nhận `backend_url=` hoặc `api_url=`; `process_folder()` và `process_lattice_capture()` chỉ chấp nhận `api_url=`):

```python
chloros_sdk.connect_camera("213800234", backend_url="http://127.0.0.1:5000")
chloros_sdk.connect_array(serials, backend_url="http://127.0.0.1:5000")
chloros_sdk.connect_daq_sensor(eth_host="daq-e-1.local",
                                backend_url="http://127.0.0.1:5000")
chloros_sdk.ChlorosLocal(backend_url="http://127.0.0.1:5000")
```

(Một backend đã cài đặt chỉ lắng nghe trên loopback, do đó một `backend_url` trên mạng LAN không thể kết nối với nó; xem [Chế độ Backend Từ xa](#remote-backend-mode-pip-only-host-via-tunnel) để biết mẫu đường hầm.)

---

## Phiên bản & Tương thích

- Phiên bản SDK được hiển thị dưới dạng `chloros_sdk.__version__`.
- SDK cố định hành vi theo phiên bản backend đi kèm. Việc kết hợp SDK cũ với backend mới hơn thường hoạt động bình thường (các điểm cuối tương thích ngược), nhưng việc kết hợp SDK mới hơn với backend cũ hơn có thể gây ra lỗi `404` trên các điểm cuối mới — hãy nâng cấp ứng dụng máy tính để bàn cho phù hợp.
- Giao diện smart-connect (`connect_camera` / `connect_array` / `connect_daq_sensor`) và điểm cuối phân tích mạng trả về các lược đồ JSON ổn định; các trường mới được bổ sung.

---

## Hướng dẫn khắc phục sự cố

- **`ChlorosAuthenticationError: Login required`** (từ `ChlorosLocal`; các lệnh gọi Smart-Connect cũng gây ra lỗi `ChlorosConnectError: Login required. …` do cùng nguyên nhân) → Chạy lệnh `chloros-cli login EMAIL PASSWORD` một lần trên máy này, hoặc đăng nhập qua ứng dụng máy tính để bàn Chloros.
- **`ChlorosProcessingError: Failed to start processing: … The project folder is too deep for this computer …`** (hoặc `Failed to create project: …` từ `create_project()`) → Tính năng đường dẫn dài của Windows đã bị tắt trên máy này (cài đặt mặc định của Windows), do đó không có đường dẫn nào được vượt quá 260 ký tự, và các sản phẩm nằm cách thư mục dự án khoảng 130 ký tự. Không có dữ liệu nào được xử lý. Thông báo này hiển thị thư mục dự án dài nhất vẫn phù hợp: hãy sử dụng một thư mục ngắn hơn, hoặc bật đường dẫn dài củWindows và khởi động lại Chloros. Trình cài đặt Windows cung cấp tùy chọn này dưới tên "Bật đường dẫn dài của Windows", được chọn mặc định; nếu không, hãy sử dụng Chính sách Nhóm: *Cấu hình Máy tính > Mẫu Quản trị > Hệ thống > Hệ thống Tệp > Bật đường dẫn dài Win32*.
- **`ChlorosConnectError: No Chloros backend is running …`** → Chức năng kết nối thông minh (smart-connect) tự động khởi động một backend cục bộ, do đó lỗi này chỉ xuất hiện khi không thể tìm thấy hoặc khởi động được tệp nhị phân đi kèm (ví dụ: máy chủ chỉ chạy pip- không có gói máy tính để bàn). Thông báo này tùy thuộc vào nền tảng: trên Windows, hãy mở ứng dụng máy tính để bàn hoặc chạy bất kỳ lệnh `chloros-cli` nào; trên Linux, hãy chạy lệnh `chloros-cli` (không có giao diện đồ họa) hoặc cài đặt `.deb`. Đối với backend từ xa, hãy truyền `backend_url=` (và `auto_start_backend=False`).
- **`CAMERA_AVAILABLE == False`** khi nhập → `lattice_sdk` không thể tải: thời gian chạy Arena SDK, hoặc OpenCV (`cv2`), không thể nhập vào phiên bản Python này. Bề mặt do backend điều khiển (`connect_*`, hiển thị qua backend, `ChlorosLocal`) vẫn hoạt động bình thường.
- **Mảng kết nối ở chế độ cảm biến đầy đủ nhưng tốc độ khung hình thấp**→ Theo thiết kế: chế độ tự động giữ nguyên cảm biến đầy đủ và để tốc độ khung hình giảm xuống mức mà đường truyền có thể hỗ trợ. Chuyển qua `binning=2` / `4` hoặc các phiên bản nhỏ hơn như `width` / `height` để tăng số khung hình mỗi giây, hoặc kích hoạt khung jumbo / nâng cấp kết nối. Binning và ROI**không** giải quyết được tình trạng quá tải tổng hợp (`oversubscribed: true`, các trường fps bằng 0): số lượng camera quá nhiều so với băng thông không thể khắc phục bằng binning/ROI — hãy giảm số lượng camera, kích hoạt khung jumbo hoặc chuyển sang NIC nhanh hơn (xem [Quá tải](#over-subscription-the-per-cam-floor)).
- **`analyze_array_network()` báo cáo vòng nhận (RX ring) của NIC rất nhỏ (~0,26 MB) / các cổng kết nối hiển thị cảnh báo "FRAMES WILL DROP"** → Vòng nhận của NIC máy chủ đang ở mức mặc định (thường được đặt lại thành 32 sau khi cập nhật trình điều khiển NIC). Trên bộ điều hợp Realtek USB 10GbE, hãy thiết lập `ReceiveBufferLen=256` và `PendingReceives=64` (với quyền cao hơn), sau đó khởi động lại hệ thống backend để nó đọc lại vòng nhận. Quy trình đầy đủ: [Tham khảo CLI → Cài đặt & Tối ưu hóa NIC máy chủ](cli-reference.md#host-nic-setup--tuning-lattice-arrays).
- **Máy chủ bị treo khi khởi động lại/tắt máy, sau đó xuất hiện lỗi WMI `Invalid class` / NIC không thể kích hoạt** → Trình điều khiển USB 10GbE đã lỗi thời gây ra lỗi `DRIVER_POWER_STATE_FAILURE` (màn hình xanh BSOD `0x9F`). Cập nhật trình điều khiển bộ điều hợp lên phiên bản mới nhất (≥ 2026) và áp dụng lại các thiết lập vòng nhận. Xem [Tham khảo CLI → Cài đặt & Tối ưu hóa NIC máy chủ](cli-reference.md#host-nic-setup--tuning-lattice-arrays).
- **Việc thu thập mảng thất bại với lỗi HTTP503 "Không có nhóm khung mới được đồng bộ hóa"** → Mảng không công bố nhóm khung mới nào trong thời gian chờ. Kiểm tra `arr.status()`: `stalled: True` (hoặc `streaming: False`) với giá trị `actual_fps` bằng 0 có nghĩa là vòng lặp thu thập đã ngừng phát hành — hãy ngắt kết nối và kết nối lại mảng. Nếu `stalled` là False và `actual_fps` lớn hơn 0, hãy thử lại. Ngay sau khi `connect_array()` xuất hiện, một lần ghi đã chờ tối đa 20 giây cho nhóm đầu tiên.
- **`status()` hiển thị `link_speed_mbps: 4294`** → Hệ điều hành Windows không thể đọc được tốc độ của bộ điều hợp; đây là hiện tượng thường gặp ở các bộ điều hợp Ethernet USB và không phải là lỗi. Xem [Tốc độ khung dự kiến](#expected-frame-rates).
- **`update_firmware()` thất bại với `device never connected back on TCP <port>`** → Tường lửa đã chặn kết nối của DAQ-E trở lại máy tính này. Trình cài đặt Windows sẽ thêm quy tắc này (xem [Cài đặt Thiết bị DAQ-E](#daq-e-device-settings)); nếu sử dụng tường lửa khác, hãy cho phép lưu lượng TCP đến từ mạng con của cảm biến vào hệ thống phía sau.
- **Từ chối phản xạ** → Để đo độ phản xạ theo thang đo tuyệt đối, thiết bị DAQ đang hoạt động phải được liên kết với camera (hoặc mảng camera). Bạn có thể thực hiện liên kết qua giao diện người dùng (GUI) hoặc sử dụng `processing="radiance"` (W/m²/sr/nm), phương pháp này không yêu cầu cảm biến ghép đôi.
- **Quá trình thu thập dữ liệu bằng `smart=True` mất nhiều thời gian hơn dự kiến** → Sự hội tụ AE phụ thuộc vào động lực của cảnh. Với `ArrayHandle.capture_smart()`, hãy mở rộng `exposure_tolerance_pct`, giảm thời gian `stability_window_s` hoặc giảm giá trị `settle_timeout_s` để có tín hiệu kích hoạt nhanh hơn và ít ổn định hơn.

---

## Xem thêm

- [Tài liệu tham khảo CLI](cli-reference.md) — mỗi lệnh con CLI đều tương ứng với một lệnh gọi SDK.
- [Cảm biến ánh sáng DAQ](https://mapir.gitbook.io/chloros/daq-light-sensors) trong tài liệu hướng dẫn Chloros — các cảm biến, giới hạn và phạm vi hiệu chuẩn của chúng, ghi dữ liệu, kết nối mạng DAQ-E.
- [Camera LATTICE](https://mapir.gitbook.io/chloros/lattice-cameras/lattice) trong tài liệu hướng dẫn Chloros — kết nối, cài đặt camera, chế độ chụp, mảng camera.
- Tài liệu trực tuyến: `https://mapir.gitbook.io/chloros/api-python-sdk`
