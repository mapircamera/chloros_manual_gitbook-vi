# Tài liệu tham khảo Chloros CLI

**Phiên bản:**

1.3.0**Ngày tạo:**29/07/2026 19:19 ·**Ngày cập nhật:** 06/11/2026**Đối tượng:** Tối ưu hóa cho việc sử dụng bởi mô hình ngôn ngữ lớn (LLM); dễ đọc cho con người.**Phạm vi:** Mọi lệnh con dành cho người dùng của `chloros-cli`, kèm theo các tùy chọn và ví dụ có thể sao chép và dán.

Tài liệu này là tài liệu tham khảo đầy đủ cho công cụ dòng lệnh `chloros-cli` đi kèm với MAPIR Chloros. Tài liệu này được thiết kế để bao quát toàn diện nhằm giúp LLM (hoặc con người) có thể tạo ra bất kỳ quy trình làm việc nào được hỗ trợ từ các danh sách dưới đây mà không cần kiểm tra mã nguồn.

Nếu bạn chỉ cần những nội dung chính, hãy chuyển đến:
- [Hướng dẫn bắt đầu nhanh trong 5 phút](#five-minute-quickstart)
- [Quy trình kết nối lần đầu với camera LATTICE](#lattice-camera-first-connect-workflow)
- [Quy trình kết nối lần đầu với cảm biến DAQ](#daq-sensor-first-connect-workflow)
- [Smart-AE / Smart-Capture](#smart-ae--smart-capture)
- [Chế độ ghi hình, thiết bị ghi và xử lý lại ngoại tuyến](#capture-modes-recorders--offline-reprocess)

> **Điều khiển Chloros từ trợ lý AI?** Claude Desktop, Claude Code, Cursor và các ứng dụng khách MCP khác có thể chạy các tác vụ xử lý, camera và mảng LATTICE, cũng như cảm biến ánh sáng DAQ thông qua `chloros-mcp`, máy chủ MCP được cài đặt cùng với Python SDK (và với gói .deb cho Linux, trong `/usr/bin`): `claude mcp add chloros -- chloros-mcp`. Cài đặt, tùy chọn và công cụ được mô tả trong phần [Máy chủ MCP (Trợ lý AI)](sdk-reference.md#mcp-server-ai-assistants). Trợ lý có giao diện dòng lệnh cũng có thể thực thi trực tiếp mọi lệnh trên trang này.

> **Thay đổi trong phiên bản 1.3.0** (những gì mà một tập lệnh hoặc quy trình làm việc 1.2.0 hiện có cần lưu ý).
>
> **Những thay đổi mà một tập lệnh 1.2.0 có thể cần xử lý:**
>
> - **`--preset` áp dụng.** `lattice capture`, `continuous`, `calibrate`, `multi-capture`, `multi-stream`, `align-calibrate`, `align-apply` và `align-stream` đã bị loại bỏ trong phiên bản 1.2.0 và chạy theo cài đặt mặc định của máy ảnh. Hiện tại, nó thiết lập định dạng, độ phơi sáng, độ khuếch đại và tốc độ khung hình của preset: `--preset high_speed` ghi hình 8-bit ở 5 ms, 6 dB, 60 khung hình/giây. `lattice cam-connect --preset` cũng giữ nguyên định dạng của cài đặt sẵn (phiên bản 1.2.0 đã thiết lập máy ảnh ở chế độ 12-bit sau khi áp dụng cài đặt sẵn). Xem [Các tùy chọn chung](#common-options-shared-by-most-subcommands).
> - **Định dạng pixel Bayer trên máy ảnh M3M (đơn sắc) chính là độ sâu của nó.** Các cài đặt sẵn đặt tên cho các định dạng Bayer, mà cảm biến đơn sắc không thể ghi được: trong phiên bản 1.2.0, máy ảnh đã từ chối định dạng đó và giữ nguyên định dạng hiện có, do đó `--preset high_speed` không phải là 8-bit trong trường hợp đó. Hiện tại, đây là định dạng Mono có cùng độ sâu: `high_speed` và `--pixel-format BayerRG8` cho ra `Mono8`, `high_quality`, `triggered` và `BayerRG12` tạo thành `Mono12`. Trong một mảng hỗn hợp M3C + M3M, mỗi camera sẽ sử dụng định dạng chung trong chính dòng sản phẩm của mình, và dự án `network-analysis` / `analyze-array --pixel-format` sẽ hiển thị từng camera ở độ sâu đó. Ngược lại cũng tương tự: một camera M3C được yêu cầu ở định dạng Mono sẽ sử dụng định dạng Bayer của độ sâu đó, do đó một mảng có camera chính là M3M sẽ không còn mở lại các camera M3C của mình ở chế độ Mono. Xem [Camera đơn dải Mono (M3M)](#mono-m3m-single-band-cameras).
> - **Các tệp xuất từ `lattice array-capture` được căn chỉnh theo mặc định, giống như các tệp của ứng dụng.** Mỗi tệp `debayered`, `display`, `radiance`, `reflectance` và các tệp chỉ mục đều được biến dạng sao cho khớp với khung hình của camera chính và được cắt xén theo vùng chồng lấn của các camera thành viên (nhỏ hơn cảm biến do độ lệch của các camera, `XMP-mapir:AlignmentJSON` `"applied": true`), do đó, bản chụp từ CLI sẽ là các tệp giống hệt bản chụp cùng cảnh từ ứng dụng Chloros; `raw` vẫn giữ định dạng gốc. Trước đây, khi chụp mà không có `--aligned`, mọi tệp đều được lưu ở kích thước gốc, chưa được điều chỉnh khung hình. `--no-aligned` vẫn giữ nguyên điều này. Cài đặt mặc định tương tự cũng áp dụng cho `--smart`, `--continuous` và `--interval`, cũng như đối với SDK và `chloros-mcp`. Xem [các chế độ `array-capture`](#array-capture-modes).
> - **`lattice array-capture --compression` mặc định là `none`.** Các tệp được ghi ở dạng không nén (khoảng 6,3 MB cho mỗi khung hình độ phân giải đầy đủ) để đạt tốc độ ghi liên tục nhanh nhất; phiên bản 1.2.0 ghi ở chế độ `deflate` (khoảng 4,1 MB). Chọn `--compression deflate` để có các tệp nhỏ hơn. Cả hai đều là định dạng không mất dữ liệu.
> - **Định dạng PNG và JPEG chỉ hỗ trợ các sản phẩm `display` / `preview` và tệp chỉ mục.** Việc ghi lại bằng `--format png` hoặc `jpg` với mức độ mà tệp 8-bit không thể chứa sẽ bị từ chối trước khi bắt đầu ghi lại (lỗi HTTP400, thoát khỏi `1`), và trường hợp tương tự cũng xảy ra với thiết lập mặc định `--processing all` khi kết hợp với `--format png`/`jpg`. Xem [Các mức xuất dữ liệu ghi lại](#capture-export-levels-the-all-default).
> - **`lattice dls` cần có `--mac`.**Không còn địa chỉ MAC mặc định nữa (trước đây là địa chỉ của một thiết bị); `daq pool-discover --only ble` tìm địa chỉ của một cảm biến.**`lattice dls --camera SERIAL`** là mới.
> - **Các lỗi khác sẽ thoát khỏi `1`.** `process` khi không thể xác minh các tệp xuất của một lần chạy hoặc nó bị mất hình ảnh, mặc dù các sản phẩm khác đã được ghi; lệnh chụp với bất kỳ tệp nào mà nó không thể ghi (các tệp khác vẫn được lưu); `lattice continuous` và `multi-stream` khi ổ đĩa đầy; `array-burst` / `array-record` khi thiết bị ghi báo lỗi; `array-build-video` khi một sản phẩm được yêu cầu bị bỏ qua hoặc không được tạo theo yêu cầu; `project burst` / `stream` khi quá trình ghi hình bị lỗi giữa chừng; `project sensor log` khi bản ghi không chứa dữ liệu đọc; `lattice exposure --set`, `gain --set` và `configure --set` với giá trị mà camera từ chối. `lattice network-analysis --json` kết thúc cùng lúc với bản tóm tắt (`1` cho `needs_force_slip`). Ngược lại: `daq pool-disconnect` của một cảm biến không còn trong danh sách sẽ thoát khỏi `0`. Xem [Mã thoát](#exit-codes).
> - **Một `lattice array-connect` chạy chậm sẽ không bao giờ thất bại khi mảng vẫn được kết nối.** Nó sẽ chờ tối đa 600 giây (180 giây trong phiên bản 1.2.0; `project connect` 90 giây), đồng thời hiển thị từng bước thực hiện của hệ thống nền, và nếu việc kết nối chưa hoàn tất vào thời điểm đó, hệ thống nền sẽ hủy bỏ kết nối — mỗi camera sẽ được đưa về trạng thái ban đầu — do đó, mã lỗi `Error:` (thoát với mã lỗi `1`) có nghĩa là không có mảng nào được kết nối. Trong phiên bản 1.2.0, việc kết nối ngay sau khi camera được bật nguồn có thể kéo dài hơn thời gian chờ và kết nối mảng sau khi lệnh đã thất bại. Ctrl+C hiện sẽ hủy kết nối. Xem [Kết nối liên tục](#persistent-connections-smart-prep-gui-equivalent-flow).
> - **`lattice array-connect` giữ nguyên cảm biến đầy đủ.** Nếu không có `--width` / `--height` / `--binning`, mọi camera đều được thiết lập ở chế độ cảm biến đầy đủ và tốc độ khung hình sẽ giảm xuống mức mà máy tính và mạng này có thể cung cấp; kết quả kết nối liệt kê các tùy chọn nhanh hơn (gộp pixel hoặc vùng quan tâm nhỏ hơn) cùng với tốc độ dự kiến của chúng. Phiên bản 1.2.0 đã thu nhỏ hoặc gộp các khung hình để duy trì tốc độ. Xem [Tùy chọn `array-connect`](#array-connect-options).
> - **Mạng chậm không còn khiến mảng camera ngừng hoạt động.** Mỗi camera được điều chỉnh tốc độ theo băng thông được phân bổ và tốc độ khung hình sẽ giảm theo. Trong trường hợp các camera không thể hoạt động chậm đến mức đó, mảng camera sẽ chạy ở chế độ phát tín hiệu nối tiếp (`slip-emit-and-capture`): mọi khung hình đều được truyền đến, nhưng các camera không được phơi sáng cùng lúc. `--force-tier sim-capture-sim-emit` yêu cầu phơi sáng đồng thời. Xem [Tốc độ khung hình mảng & mô hình burst](#array-fps--burst-model).
> - **`lattice network-analysis` / `analyze-array` thể hiện cách áp dụng kết nối**: nếu không có `--pixel-format`, các định dạng được thiết lập bởi connect (Mono12 / BayerRG12, trước đây là BayerRG8), và quyết định về [định dạng 12-bit nén](#packed-12-bit-transport---packed). Do đó, cùng một lệnh có thể hiển thị tốc độ khung hình khác so với phiên bản 1.2.0.
> - **Đường dẫn tương đối được tính từ vị trí bạn chạy lệnh.** `-o` và các đường dẫn khác mà phần nền mở ra (`--input`, `--burst-dir`, `--profile`, `--save`, `--mask`), đường dẫn `output` của một bước công thức, và giá trị mặc định `output/` của `lattice index --live` được giải quyết dựa trên thư mục mà bạn chạy `chloros-cli` từ đó. Trong phiên bản 1.2.0, backend giải quyết các giá trị này trong thư mục cài đặt riêng của nó, điều này dẫn đến lỗi "Access is denied" khi chạy trên một bản dựng đã được cài đặt. Xem [Quy ước](#conventions).
> - **`lattice index` chạy trên backend dưới mọi hình thức**, giống như các lệnh `lattice` khác, vì vậy hãy khởi động backend trước. Chỉ mục ngoại tuyến (`--input`) được tính toán bởi backend, vốn đọc tệp TIFF và ghi các sản phẩm bên cạnh nó hoặc tại `-o`, và `--list-presets` / `--list-gradients` in ra các bảng của phần nền. Trên Windows, các lệnh này hiện đã hoạt động: trước đây, giao diện dòng lệnh Windows (CLI) trả lời mọi dạng của `lattice index`, bao gồm cả `--live`, bằng `Error: lattice_sdk not available`. Trên Linux, chỉ mục ngoại tuyến trước đây chạy bên trong CLI mà không cần backend và không cần đăng nhập; hiện nay nó cần cả hai, giống như mọi lệnh CLI khác. Xem [Chỉ mục / Toán học Thực vật](#index--vegetation-maths).
> - **Các tệp mới bên cạnh các sản phẩm.** Mỗi sản phẩm radiance, reflectance và chỉ số mà quá trình tách dải (band unmix) đã giới hạn (clamp) một pixel sẽ có một tệp mặt nạ (mask) trong thư mục `clamp_masks/` bên cạnh thư mục của sản phẩm, và bản in sẽ hiển thị một dòng `clamp mask:`. Xem [Mặt nạ giới hạn tách dải](#unmix-clamp-mask-which-pixels-a-band-was-clamped-on).
> - **Hướng máy ảnh.** Quá trình chụp bằng một máy ảnh duy nhất sẽ lưu các tệp `raw` và `debayered` theo cách cảm biến đọc chúng và ghi lại thông tin lật/xoay của máy ảnh trong tệp XMP; tệp `process` sẽ áp dụng thông tin này cho các sản phẩm được tạo ra. Các tệp thành viên của mảng vẫn giữ định dạng gốc trừ khi bạn truyền tệp mới `--orient` (`array-capture`, `array-burst`, `project capture` / `burst` / `stream`). Xem [Hướng máy ảnh](#what-a-captures-folder-looks-like).
> - **Tên cấp độ xuất không phải là số sẽ bị từ chối.** Danh sách `lattice capture --processing` và mọi danh sách `--levels` đều chấp nhận `raw`, `debayered`, `radiance`, `reflectance` hoặc `display` (hoặc một tên gọi khác như `preview`; `--processing` cũng như `all`). Bất kỳ tên nào khác đều bị từ chối trước khi chạm vào camera (HTTP 400 `UNKNOWN_LEVEL`, thoát khỏi `1`); 1.2.0 đã lưu `raw` cho nó và thoát khỏi `0`.
> - **`daq pool-set-cap` cần `--cap-id`.** Nếu thiếu nó, lệnh sẽ thoát khỏi `2`; 1.2.0 đã loại bỏ hiệu chỉnh nắp, để lại cảm biến trần. `--cap-id none` yêu cầu cảm biến trần.
> - **`lattice resolution --binning-mode` đã bị loại bỏ.** Chế độ gộp (binning) luôn là Trung bình; cờ này vẫn được chấp nhận, hiển thị cảnh báo và không thực hiện hành động nào. Xem [Tối ưu hóa cảm biến](#sensor-tuning).
> - **`process --debayer texture-aware` bị từ chối đối với các bản ghi LATTICE.** Đối với thư mục chứa các bản chụp LATTICE, `process` sẽ thoát khỏi `1` sau khi nhập, liệt kê số lượng bản chụp, trước khi xử lý bất kỳ dữ liệu nào. Các tệp ảnh LATTICE luôn được xử lý demosaic theo tiêu chuẩn, do đó phiên bản 1.2.0 đã chấp nhận tùy chọn này nhưng bỏ qua nó; ứng dụng Chloros cũng không cung cấp tùy chọn này cho các tệp ảnh LATTICE. Xem [`chloros-cli process`](#chloros-cli-process).
> - **Các camera sẽ quay trở lại trạng thái ban đầu — trừ khi sử dụng `--keep-settings`.** `lattice array-disconnect`, mọi lệnh tạm thời (`multi-capture` không có mảng, `multi-stream`, `multi-test`, `align-calibrate`, `align-apply`, `align-stream`, `index --live`) và `lattice continuous` sẽ khôi phục lại trạng thái ban đầu của từng máy ảnh: dây kích hoạt và dây đồng bộ, kết nối PTP, cùng mọi cài đặt máy ảnh đã được thay đổi trong phiên làm việc (độ phơi sáng và độ khuếch đại, mục tiêu AE, giới hạn và độ giảm chấn, cân bằng trắng, mức đen, định dạng pixel, gộp pixel và vùng quan tâm (ROI), chế độ lật, tốc độ khung hình). `--keep-settings` thay vào đó lưu các cài đặt của từng máy ảnhvào bộ nhớ riêng của máy ảnh, để máy ảnh khởi động với các thiết lập đó; `--clear-settings` đặt lại bộ nhớ đó về cài đặt gốc. Dù bằng cách nào, các máy ảnh vẫn được để chạy tự do. Một dự án sẽ thiết lập các máy ảnh của mình từ các thiết lập đã lưu của chính dự án khi kết nối chúng. Xem [Bộ nhớ camera](#camera-memory----keep-settings----clear-settings).
> - **Mọi chế độ đều lưu cài đặt cho dự án theo cùng một cách.** Bất kỳ thay đổi nào đối với camera, mảng cảm biến hoặc cảm biến ánh sáng được kết nối trong một dự án — từ ứng dụng, giao diện dòng lệnh (CLI), bộ công cụ phát triển phần mềm (SDK) hoặc máy chủ MCP — đều được lưu vào tệp `cameras.json` / `sensors.json`, và việc mở dự án trong bất kỳ chế độ nào sẽ thiết lập các thiết bị dựa trên dự án đó. Trước đây, chỉ ứng dụng mới lưu dự án, và lần lưu tiếp theo sẽ ghi đè lên mọi dữ liệu khác. Xem [Một bản ghi dự án, mọi chế độ](#one-project-record-every-mode).
> - **Các tệp được tạo từ nhiều camera sẽ ghi tên từng camera.** Một chồng hình đa dải (`align-apply`, `align-stream`, `index --live --save-multiband`), một chỉ mục trực tiếp được tạo từ các dải phổ của nhiều máy ảnh, và hình ảnh `composite/` từ một lần chụp mảng đều chứa số sê-ri, mẫu máy và gói hiệu chuẩn của từng máy ảnh nguồn dưới dạng danh sách (`XMP-mapir:SourceCameraSerials`, `SourceCameraModels`, `SourceCalBundleSha256s`, …), và danh sách EXIF `SerialNumber` liệt kê các số sê-ri, được phân tách bằng dấu phẩy. Trước đây, các tệp này không chứa thông tin về bất kỳ camera nào, hoặc chỉ chứa số sê-ri của camera đầu tiên. Xem [Các tệp được tạo từ nhiều camera](#files-made-from-several-cameras).
> - **Chỉ tệp `raw` mới chứa các thẻ hiệu chuẩn của camera.** Bộ tương thích với MicaSense (`BlackLevel`, `RadiometricCalibration`, tệp theo kênh `Band{R,G,B}RadiometricCalibration`, các bộ `MicaSenseVignetting*`) và dữ liệu tải `CalibrationJSON` mô tả các giá trị thô của cảm biến, do đó chúng chỉ được chứa trong tệp ghi `raw`. Tất cả các tệp còn lại — các tệp ghi lại `debayered`, `display`, `radiance`, `reflectance` và các tệp chỉ mục, các khung `lattice continuous` và `multi-stream`, và mọi tệp sản phẩm `process` — đều chứa thông tin nhận dạng của camera (`XMP-mapir:CameraSerial`, `CameraModel`, `FilterType`, `LensModel`, `CalBundleSha256`, ...) và, khi chụp ảnh, bản ghi riêng của khung hình đó (`XMP-mapir:FrameJSON`: độ phơi sáng, độ khuếch đại, nhiệt độ, định dạng pixel, thời gian chụp). Trong phiên bản 1.2.0, chúng cũng chứa bộ dữ liệu thô, mà một công cụ áp dụng (Pix4D, Metashape) sẽ áp dụng cho các pixel không phải là giá trị thô. `process` xác định thông số hiệu chuẩn của tệp đó dựa trên số seri và `CalBundleSha256`.
>
> **Các thay đổi khác:**
>
> - **Tốc độ khung hình tự động của mảng sẽ điều chỉnh theo dữ liệu mà mảng cung cấp.** Tốc độ nào mất hơn 1% số lần kích hoạt sẽ giảm xuống, được đánh giá qua nhiều lần quan sát kéo dài thay vì chỉ một lần, và tốc độ đã giảm xuống trước đó sẽ được thử lại thỉnh thoảng. Tốc độ do người dùng thiết lập sẽ được giữ nguyên.
> - **Chế độ phơi sáng tự động ổn định hơn.** Chế độ này duy trì mức phơi sáng khi ánh sáng không đổi, ổn định trong vòng vài giây sau khi có thay đổi, bỏ qua các vùng sáng nhỏ và khôi phục hoạt động của máy ảnh khi máy ngừng phản hồi dưới ánh nắng chói chang. Xem [Cách hoạt động của phơi sáng tự động](#how-auto-exposure-behaves).
> - **`process` chỉ đếm các hình ảnh và bản ghi của bạn.** Khi chạy lại trên một thư mục chứa tệp `camera_cal/` của dự án, các tệp hiệu chuẩn đó sẽ không còn được tính nữa ("Tìm thấy 96 hình ảnh" trên bộ 90 hình ảnh), và dự án đã nhập các tệp này trước đó sẽ loại bỏ chúng.
> - **`lattice multi-test` bắt đầu từ trạng thái bật nguồn của các camera.** Nó bật PTP của các camera cho lần chạy, chờ đồng hồ của chúng khóa và ổn định trước lần kích hoạt đầu tiên — mỗi camera được khóa trong 120 giây, khoảng 2 phút kể từ khi bật nguồn (`--ptp-settle SECONDS` thiết lập điều này), vì một đồng hồ vừa khóa có thể chênh lệch lên đến ~200 us trong khoảng 2 phút — và đưa PTP của từng camera trở lại trạng thái ban đầu. Một camera vẫn đang khóa từ lần thử nghiệm trước sẽ không bị chờ đợi lại. Trước đây, nó thường tắt PTP trên mạng chỉ có một bộ chuyển mạch khi không có DAQ-E nào phản hồi, và đọc đồng hồ chạy tự do của từng camera như một đồng hồ PTP sẽ không đồng bộ (`PTP is not locked`, mỗi lần chạy). Xem [Nhiều camera (Phiên làm việc tạm thời)](#multi-camera-transient-sessions).
> - **Một bản ghi `.daq` cho biết nắp bảo vệ đã được chọn hay được giả định.** DAQ-U, DAQ-M và DAQ-E được xuất xưởng mà không có nắp, và Chloros không thể xác định liệu có nắp được lắp hay không, do đó một cảm biến được kết nối mà không có tên nắp sẽ được gán mã `sunshine_cosine` theo mặc định, đây là một *giả định*. Mọi bản ghi Chloros (schema 1.30) hiện lưu trữ `cap_id_source` bên cạnh `cap_id`: `operator` khi bạn đặt tên cho nắp (bao gồm cả `none`), và `auto_default` khi đó là tên mặc định. Khi xử lý bản ghi mà nắp được giả định, với cài đặt nắp DAQ của dự án để ở chế độ Tự động, sẽ hiển thị thông báo `[daq-dls] WARNING` với tên tệp: một DAQ-U không có bộ lọc dưới điều kiện hiệu chỉnh ánh nắng mặt trời sẽ cho giá trị cao gấp khoảng 30 lần. **Hãy đặt tên cho bộ lọc bạn đã cài đặt** khi kết nối, hoặc thiết lập bộ lọc DAQ của dự án, và cảnh báo sẽ biến mất.
> - **Không còn cửa sổ console dạng pip trên Windows.** Trên máy tính có cài đặt Python, phần backend và mỗi công cụ xử lý hình ảnh của nó đều chạy lệnh `pip` một lần khi khởi động, và mỗi tiến trình đều mở một cửa sổ console. Hiện tại, điều này không còn xảy ra nữa.
> - **Các bản ghi trực tiếp `.daq` vẫn tồn tại sau khi mất điện.**Bản ghi được ghi vào nhật ký và đồng bộ hóa toàn bộ đĩa sau mỗi lần commit, phía sau cảm biến, do đó, đĩa chậm chỉ làm chậm tệp mà không bao giờ ảnh hưởng đến các giá trị đo. Một bản ghi bị dừng giữa chừng khi commit (mất điện, bị ngắt) có thể để lại tệp `<name>.daq-journal` bên cạnh:**sao chép cả hai tệp lại với nhau.** Chloros sẽ khôi phục nhật ký khi đọc tệp lần đầu tiên, do đó tệp ghi sẽ mở ra với toàn bộ dữ liệu đã được cam kết trước khi dừng (khoảng 3 giây cuối cùng bị mất); trong phiên bản 1.2.0, việc mất điện vào thời điểm không thích hợp có thể khiến toàn bộ tệp không thể đọc được ("hình ảnh đĩa cơ sở dữ liệu bị lỗi").
> - **Việc ghi dữ liệu trên đĩa đang bận không còn làm chậm cảm biến nữa.** `daq pool-record` xếp hàng các lần đọc và ghi chúng trên một luồng riêng; một lần đọc không thể ghi được sẽ được tính (`daq pool-list` hiển thị `N write error(s)` với lỗi cuối cùng) thay vì làm đóng băng cảm biến (một lần ghi mất 8,4 giây khi có bốn camera đang lưu tệp TIFF).
> - **`lattice multi-capture` không sử dụng mảng sẽ không còn mất các khung hình đã lưu khi tín hiệu kích hoạt hết thời gian chờ.** Khi tín hiệu kích hoạt của camera hết thời gian chờ (`SC_ERR_TIMEOUT -1011`, được quan sát với bốn camera trên một bộ chuyển mạch 1 GbE), quá trình ghi sẽ kết thúc tại nhóm khung hình đó: các nhóm đã được lưu sẽ được liệt kê với `Saved:`, sau đó là thông báo lỗi, thoát `1`. Trong phiên bản 1.2.0, đó là lỗi HTTP500 mà không có danh sách, mặc dù các tệp vẫn tồn tại trên đĩa.
> - **Các camera kích hoạt bằng phần mềm không còn bỏ qua mỗi lần kích hoạt thứ hai.** Một camera ở chế độ kích hoạt bằng phần mềm giờ đây sẽ xếp hàng một tín hiệu kích hoạt đến trong quá trình đọc dữ liệu (hoặc Chloros sẽ đợi cho đến khi nó sẵn sàng), giống như các máy ảnh đồng bộ qua cáp đã làm từ trước.
> - **`daq pool-discover` đặt tên cho DAQ-E-S `DAQ-E-S`** trong cột `MODEL` của nó (và `--json` của `model`). Nó hiển thị `DAQ-E` cho mọi cảm biến Ethernet; một cảm biến không trả lời kiểm tra danh tính trong khoảng 2 giây vẫn được liệt kê, với tên `DAQ-E`.
> - **Các sản phẩm chỉ số LATTICE được cải thiện** (NDVI và các chỉ số khác) từ `process`, trong mọi chế độ xử lý.
> - **Camera OCN: các sản phẩm về độ rạng rỡ, độ phản xạ và chỉ số dưới ánh sáng mặt trời được cải thiện.** Sửa lỗi cho toàn bộ đội camera được áp dụng sau khi tách dải phổ; camera RGN không thay đổi, và các giá trị OCN khác với phiên bản 1.2.0. Điều này cũng áp dụng cho dữ liệu OCN được thu thập bằng phiên bản trước đó, khi dữ liệu đó được xử lý trên máy tính lưu trữ thông số hiệu chuẩn đã được sử dụng khi thu thập dữ liệu (Chloros lưu giữ thông số hiệu chuẩn của máy ảnh sau khi máy ảnh được kết nối).
> - **`lattice continuous` ghi lại khung hình thô.** Một phiên chạy `tiff` đã ghi ra 3, mà `process` không thể xử lý trực tiếp. Các khung hình của nó hiện là tệp `raw` mà quá trình chụp mảng tạo ra: một mặt phẳng ghép mosaic với giá trị gốc của cảm biến, không nén, kèm theo gói hiệu chuẩn. Một tập lệnh đọc ba mặt phẳng từ các tệp này chỉ đọc được một mặt phẳng.
>
> - **Mỗi lần ghi lại đều lưu lại giá trị DAQ đã sử dụng, giống như giao diện người dùng (GUI).** `lattice capture`, `lattice continuous` (một tệp `.daq` cho mỗi khung hình), `array-capture` và `multi-capture` khi kết nối với mảng (và SDK) sẽ ghi tệp `.daq` và `.match.json` mỗi khi một DAQ được liên kết hoặc gộp vào nhóm, bao gồm cả việc chụp dữ liệu thô; trong phiên bản 1.2.0, chỉ có một bản ghi thô hoặc đã loại bỏ hiệu ứng Bayer được lưu với `--force-daq`. `--no-daq` tắt tính năng này, `--daq-tolerance-ms MS` thiết lập độ dung sai khớp cho bản ghi đó, và `daq pool-bind` mới sẽ liên kết máy ảnh hoặc mảng với DAQ từ CLI. Với `.daq`, bản chụp được lưu chỉ được khớp trong `process` với các máy ảnh được chỉ định. Lúc này, lần ghi hình sẽ chờ lần đọc tiếp theo của DAQ để có thể kết hợp các giá trị đọc ở hai bên khoảng thời gian phơi sáng. Xem [Lần đọc DAQ mà quá trình ghi hình lưu lại](#the-daq-reading-a-capture-saves) và [Lần đọc DAQ cách xa thời gian phơi sáng](#a-daq-reading-far-from-the-exposure).
> - **`process --indices NAME`** thêm chỉ mục vào dự án cho các dải phổ của từng camera, bao gồm một dự án mới. Trong phiên bản 1.2.0, tên dự án chưa được liệt kê sẽ bị bỏ qua khi sử dụng `Unknown index name(s) ignored`.
> - **`process` khi chạy bên cạnh một mảng trực tiếp:**nó không còn khởi động lại backend chứa các camera hoặc DAQ, và chờ tối đa 15 giây cho một phiên chạy đang kết thúc.**Ctrl+C** dừng phiên chạy trong backend, không chỉ trên giao diện dòng lệnh (CLI). Xem [Ghi chú về `process`](#notes).
> - **Thư mục dự án nằm quá sâu so với giới hạn đường dẫn 260 ký tự của Windows** sẽ bị từ chối ngay từ đầu bằng cách sử dụng thư mục dài nhất có thể, thay vì thực thi mà không ghi dữ liệu nào. Trình cài đặt Windows hiện cung cấp tùy chọn "Bật đường dẫn dài của Windows" và loại trừ Microsoft Defender cho phần nền, cả hai đều được chọn mặc định. Xem [Windows (.exe)](#windows-exe).
> - **`lattice array-capture --continuous`** chạy tính năng ghi liên tục của phần nền: hiển thị thanh tiến trình mỗi 2 giây, hiển thị tóm tắt thay vì danh sách từng tệp, và trả về mã thoát không bằng 0 nếu có tệp nào chưa được ghi hoặc ổ đĩa đầy. Xem [các chế độ `array-capture`](#array-capture-modes).
> - **Lệnh `array-capture` đầu tiên sau `array-connect`** sẽ chờ tối đa 20 giây để nhận nhóm khung hình đầu tiên của mảng thay vì báo lỗi HTTP 503 "Mảng vẫn đang khởi động".
> - **Thời gian phơi sáng của mảng trong vùng ROI:** một vùng ROI nhỏ không còn giới hạn thời gian phơi sáng của thành viên mảng ở mức chu kỳ hoạt động tự do của cảm biến (4,9 ms ở độ phân giải 768 × 304). Xem [Thời gian phơi sáng trên thành viên mảng](#exposure-on-an-array-member).
> - **Chế độ phơi sáng tự động không còn làm giảm tốc độ khung hình tự động của mảng.** Một mảng đồng bộ qua cáp hoạt động ở tốc độ mà mạng và tốc độ đọc dữ liệu của các camera cho phép; chế độ phơi sáng tự động lấp đầy khoảng thời gian mà tốc độ đó cung cấp, sau đó tăng độ khuếch đại. Thời gian phơi sáng thủ công dài hơn khoảng thời gian (`lattice exposure --set`, `configure --set`) sẽ làm chậm tín hiệu kích hoạt để phù hợp với thời gian đó thay vì bị từ chối, và tốc độ sẽ trở lại bình thường khi thành viên chuyển về chế độ tự động. Xem [Phơi sáng trên một thành viên mảng](#exposure-on-an-array-member).
> - **Mảng cung cấp ít nhóm khung hình không hoàn chỉnh hơn**, và ít lỗi 503 ("Không có nhóm khung hình đồng bộ mới") hơn: đầu vào kích hoạt của mỗi camera phụ hiện đã được loại bỏ nhiễu.
> - **Truyền tải 12-bit nén** (`--packed`, mặc định là `auto`) giúp tăng tốc độ khung hình của mảng cảm biến đầy đủ lên khoảng một phần ba. `array-connect --packet-size` hiện đã được áp dụng (phiên bản 1.2.0 đã chấp nhận nhưng bỏ qua nó). Xem [Giao thức truyền tải 12-bit nén](#packed-12-bit-transport---packed).
> - **Mảng camera tự phục hồi sau khi khởi động lại nguồn.** Sau khi khởi động lại nguồn bộ chuyển mạch, mảng của `array-connect` sẽ-mở lại các camera khi chúng phản hồi và tiếp tục truyền dữ liệu với PTP được kích hoạt trở lại; trong phiên bản 1.2.0, quá trình này dừng hẳn và cần một phiên bản `array-connect` mới. `array-status` báo cáo `ptp_enabled: False` (và `ptp_off:`) trong khi camera đang đọc PTP ở chế độ tắt, trong khi trước đây nó báo cáo cài đặt mà mảng đã kết nối. Xem [Kết nối liên tục](#persistent-connections-smart-prep-gui-equivalent-flow).
> - **`lattice array-record` ghi hình khi không có GUI nào đang mở.** Khi chế độ xem kết hợp của GUI bị đóng, hệ thống nền sẽ tự hiển thị chế độ xem: mảngNDVI của mảng, hoặc chỉ số mà bạn truyền vào với `--expr` mới. Một bản ghi không ghi khung hình nào giờ đây sẽ thất bại (`no-frames: …`, thoát khỏi `1`). Trước đây, nó chỉ ghi khi chế độ xem kết hợp của GUI đang mở, và một bản ghi không ghi được gì vẫn thoát khỏi `0`. Xem [`array-record`](#array-record--combined-index-videogif-monitoring-grade).
> - **Một camera mà Chloros đang giữ sẽ không được mở lần thứ hai.** Một lệnh cố gắng mở lại camera đó sẽ nhận được phản hồi HTTP 409 `CAMERA_HELD`, trong đó nêu rõ tên của đối tượng đang giữ camera và lệnh giải phóng nó. Điều này bao gồm cả camera đang được lệnh khác sử dụng tại thời điểm hiện tại (một lệnh `multi-test` hoặc `multi-capture` đang mở camera của nó, hoặc một lệnh `lattice capture`): lệnh thứ hai sẽ bị từ chối cho đến khi lệnh đầu tiên hoàn tất. Việc mở lại có thể làm dừng luồng video của thiết bị đang giữ hoặc gây treo hệ thống backend. Xem [Kết nối liên tục](#persistent-connections-smart-prep-gui-equivalent-flow).
> - **Các lệnh đa camera tạm thời sẽ gửi dữ liệu từ mọi camera hoặc thất bại.** `lattice multi-capture` không kết nối với mảng nào, `multi-stream`, `multi-test`, `align-calibrate`, `align-apply`, `align-stream` và `index --live` thoát khỏi `1` khi chỉ định một camera không gửi khung hình nào. Các luồng bỏ qua một nhóm khung hình thiếu camera và dừng lại sau ba lần liên tiếp; `multi-test` in một hàng cho mỗi lần kích hoạt, `(no frame)` nơi camera đã bỏ lỡ; `align-stream` cũng thoát khỏi `1` do lỗi công việc. Sau đó, mỗi lần chạy sẽ đưa mọi camera trở lại trạng thái ban đầu: kết nối kích hoạt và đường dây đồng bộ, PTP (`PtpEnable`) và mọi cài đặt máy ảnh mà lần chạy đã thay đổi — độ phơi sáng và độ khuếch đại, mục tiêu AE, giới hạn và độ giảm chấn, cân bằng trắng, mức đen, định dạng pixel, gộp pixel và vùng quan tâm (ROI), lật hình, tốc độ khung hình — trừ khi `--keep-settings` / `--clear-settings` (xem [Bộ nhớ máy ảnh](#camera-memory----keep-settings----clear-settings)). Trước đây, các lệnh này lưu lại bất kỳ khung hình nào nhận được và thoát khỏi `0` (`align-stream` hiển thị lỗi công việc khi thực thi `Note:`), và để lại mọi camera trong `TriggerMode` `On`. Chúng cũng thiết lập độ phơi sáng của từng camera dựa trên cảnh trước khi lưu khung hình (tối đa vài giây), trên mục tiêu mà mảng kết nối khởi động mỗi camera ở: 40 đa phổ, 60 khung hình RGB (`--brightness N` thiết lập giá trị này), và duy trì cài đặt này trong suốt quá trình chụp; một camera `lattice capture` duy nhất mà Chloros không kiểm soát cũng làm tương tự. Trước đây, khung hình đầu tiên của mỗi camera được chụp ở giới hạn phơi sáng riêng của camera đó, khoảng 100 ms, bất kể điều kiện ánh sáng, và thường bị cắt bớt. Xem [Đa camera (Phiên làm việc tạm thời)](#multi-camera-transient-sessions).
> - **Các lệnh đa camera tạm thời chia sẻ mạng theo cách mà `array-connect` thực hiện, và chỉ mở `--serials`.** Mỗi camera được điều chỉnh tốc độ theo phần băng thông được phân bổ trên liên kết chủ, do đó bốn camera hoạt động trên liên kết 1 Gbps; trước đây, bốn camera này yêu cầu tổng cộng 125 MB/s, chiếm hết băng thông 1 GbE, và mỗi khung hình đều bị mất gói tin. Một mảng mà mạng không thể truyền tải sẽ bị từ chối trước khi bất kỳ camera nào bắt đầu truyền dữ liệu. `--serials` hiện chọn các camera, trong đó camera đầu tiên kích hoạt tín hiệu; trước đây, mọi lệnh đều mở tất cả camera trên mạng bất kể `--serials` chỉ định gì. Một sự cố mất gói tin GVSP giờ đây sẽ hiển thị thông báo cụ thể thay vì chỉ ra cáp đồng bộ M8. Xem [Nhiều camera (Phiên làm việc tạm thời)](#multi-camera-transient-sessions).
> - **`lattice array-disconnect` để các camera hoạt động tự do, như đã phát hiện.** `TriggerMode` `Off`, và mọi thứ khác như kết nối đã phát hiện — các cài đặt kích hoạt và đường dây đồng bộ (`TriggerSource`, `TriggerActivation`, `TriggerOverlap`, `TriggerDelay`, `LineMode` / `LineSource`), PTP, và các cài đặt máy ảnh đã được thay đổi trong phiên làm việc — do đó, chỉ cần mộtlệnh camera trên một thành viên trước đó sẽ lấy được các khung hình ngay lập tức, và máy chủ sẽ không còn bị kẹt ở `TriggerSource` `Software`. `--keep-settings` lưu các cài đặt của phiên làm việc vào bộ nhớ của từng camera, `--clear-settings` sẽ đặt lại bộ nhớ đó (xem [Bộ nhớ camera](#camera-memory----keep-settings----clear-settings)). `lattice continuous` cũng đặt camera mà nó phát hiện đang ở chế độ kích hoạt sang chế độ chạy tự do cho lần chạy đó (và đưa nó trở lại trạng thái ban đầu sau đó), và một lần chạy không có khung hình nào sẽ thoát khỏi `1`.
> - **`lattice configure --get` / `lattice exposure`** in ra một `note:` vớithông số phơi sáng hoặc độ khuếch đại thực tế của camera trong khi chế độ tự động đang hoạt động. Một lệnh bị từ chối `lattice exposure --set` / `configure --set` sẽ thông báo phạm vi mà camera chấp nhận và khôi phục chế độ tự động thay vì tắt chế độ phơi sáng tự động. Xem [Điều chỉnh cảm biến](#sensor-tuning).
> - **Các lệnh điều khiển camera mới:**`lattice highlights` (làm tối các bề mặt trắng sáng trong bản hiển thị RGB) và `lattice exposure --brightness-auto`.**`lattice multi-detect`** tìm các camera có dây trong một vòng quét và mặc định thiết lập camera chính là camera RGB.
> - **Các lệnh DAQ mới:** `daq pool-bind`; `pool-enable-ptp`; và các lệnh dành cho cài đặt riêng của DAQ-E: `pool-info`, `pool-set-network`, `pool-set-auto-stream`, `pool-reboot` và `pool-update-firmware`. Xem [Cài đặt thiết bị DAQ-E](#daq-e-device-settings).
> - **Một thiết bị DAQ-E ngừng truyền dữ liệu sẽ được hệ thống backend kết nối lại.** Khi luồng dữ liệu của một thiết bị DAQ-E trong nhóm ngừng hoạt động (do khởi động lại nguồn của bộ chuyển mạch cấp nguồn cho nó, mất kết nối camera), hệ thống backend sẽ kết nối lại thiết bị đó với các cài đặt ban đầu, đồng thời thử lại với khoảng thời gian ngày càng dài trong suốt thời gian thiết bị ngừng hoạt động. Quá trình ghi hình trên thiết bị đó sẽ tiếp tục trong cùng tệp `.daq` với khoảng trống được đánh dấu trong tệp, và tệp `pool-record --stop` liệt kê từng khoảng trống. Trong phiên bản 1.2.0, cảm biến vẫn im lặng cho đến khi tệp `pool-disconnect` + `pool-connect` được tạo ra, điều này cũng kết thúc quá trình ghi hình. Xem [Một DAQ-E ngừng truyền dữ liệu](#a-daq-e-that-stops-delivering).
> - **`time-sync status`** báo cáo liệu các gói tin từ máy chủ PTP chính có đến được mạng hay không (`health`), và sẽ hiển thị thông báo lỗi `WARNING:` trên stderr khi chúng không đến được. Xem [`time-sync`](#chloros-cli-time-sync).
> - **Gói .deb cho Linux:** gói này tăng kích thước bộ đệm nhận socket của kernel mà các luồng dữ liệu từ camera cần (với cài đặt mặc định của kernel, các mảng sẽ bị mất khung hình và làm giảm tốc độ khung hình), thêm một dịch vụ backend chạy dưới quyền người dùng của bạn, và `lattice network` kiểm tra các bộ đệm này. CLI không bao giờ dừng dịch vụ backend đang chạy, và quá trình nâng cấp sẽ khởi động lại các dịch vụ backend đã được kích hoạt. Xem [Chạy dịch vụ backend dưới dạng dịch vụ](#running-the-backend-as-a-service-headless) và [Máy chủ Linux — bộ đệm nhận socket](#linux-hosts--socket-receive-buffers).
> - **`lattice array-capture` thông báo khi không thể căn chỉnh:**mỗi lý do khiến dữ liệu ghi lại bị lưu không căn chỉnh (không có cấu hình căn chỉnh, các thành phần có độ phân giải khác nhau, mảng không được đồng bộ qua cáp, khung hình mà cấu hình không thể ánh xạ vào) sẽ được in ra dưới dạng dòng `Alignment:`.**`chloros-cli daq help`** là `daq --help` (nó đã thoát khỏi `2`), và trên Linux, phần trợ giúp liệt kê các lệnh phần cứng trực tiếp `daq` mà bản dựng này bao gồm.
> - **Jetson:** gói phần mềm là `chloros_1.3.0_arm64_jp6.deb`, dành cho JetPack 6, và quá trình xử lý GPU của nó chạy trên Orin với CUDA PyTorch của JetPack. Xem [Linux arm64 — Jetson](#linux-arm64--jetson-jetpack-6).

---

## Quy ước

- Tất cả các lệnh đều có tiền tố `chloros-cli`. Trên Windows, tệp nhị phân là `chloros-cli.exe`; trên Linux/Jetson thì là `chloros-cli`.
- Các đối số tùy chọn được hiển thị dưới dạng `--flag`. Các đối số vị trí bắt buộc được hiển thị mà không có dấu ngoặc vuông.
- Khi có giá trị mặc định, việc bỏ qua tham số sẽ sử dụng giá trị đó.
- Giao diện dòng lệnh (CLI) là một client HTTP nhẹ chạy trên nền tảng Chloros (máy chủ trên `127.0.0.1:5000` mà ứng dụng desktop cũng sử dụng). **Các lệnh nào khởi chạy nó:**
  - Các lệnh chính — `process`, `login`, `logout`, `status`, `export-status`, `time-sync` và `selftest` — sử dụng backend đang chạy, hoặc khởi động một backend mới khi không có backend nào đang chạy và dừng nó lại khi hoàn tất.
  - **Các lệnh `lattice`, `project` và `daq` không bao giờ khởi động dịch vụ nền.** Chúng yêu cầu một backend đã đang chạy: ứng dụng desktop Windows, dịch vụ backend Linux (#running-the-backend-as-a-service-headless), hoặc một backend mà bạn tự khởi động (xem [Khởi động Backend](#start-the-backend)). Nếu không có dịch vụ nền, các lệnh này sẽ hiển thị thông báo lỗi `Error: Chloros backend not reachable at http://127.0.0.1:5000` kèm theo tên lệnh dùng để khởi động dịch vụ, sau đó thoát khỏi chương trình với thông báo `1`.
- `CHLOROS_BACKEND_URL=<url>` định hướng các nhóm lệnh **`lattice`**,**`project`**và**`daq pool-*`** sang một backend khác (`lattice network-analysis` lại trỏ đến `--backend-url`). Các lệnh cốt lõi cố ý khóa `http://127.0.0.1:<port>` và bỏ qua nó (việc sử dụng địa chỉ IPv4 cố định giúp tránh được mức phạt khoảng 2 giây cho mỗi yêu cầu khi Windows chuyển đổi từ `localhost` sang `::1`). Xem [Biến môi trường](#environment-variables).
- **Đường dẫn tương đối được giải quyết dựa trên thư mục mà bạn chạy `chloros-cli` từ đó.** Phần backend là quá trình mở các thư mục đầu ra và đầu vào, vì vậy trước khi gửi đường dẫn, giao diện dòng lệnh (CLI) sẽ chuyển nó thành đường dẫn tuyệt đối: `-o` / `--output` (bao gồm cả giá trị mặc định của nó, `output`), `--input`, `--burst-dir`, `--profile`, `--save` và `--mask`, một bước [công thức](#recipe-dsl) `output` (mặc định là `output`), và thư mục mặc định `output` của `lattice index --live`. Do đó, `-o captures/` ghi vào `<current folder>/captures/`, và các đường dẫn được in ra là đường dẫn tuyệt đối. `~` được mở rộng. Khi kết nối với một máy chủ từ xa (`CHLOROS_BACKEND_URL` chỉ định một máy khác), các đường dẫn được gửi nguyên văn và được giải quyết trên máy đó.
- Cần có tài khoản Chloros+ để đăng nhập cho tất cả các lệnh SDK/CLI (chạy `chloros-cli login` một lần trên mỗi máy; được lưu trong bộ nhớ đệm tại `~/.chloros/`).
- Các ví dụ sử dụng đường dẫn Linux; trên Windows, hãy thay thế `/home/user/...` bằng `C:/Users/.../...`. Cổng nối tiếp của DAQ-U'có dạng `COM3` trên Windows và `/dev/ttyUSB0` / `/dev/ttyACM0` trên Linux.
- Các tóm tắt ghi các cờ loại trừ lẫn nhau dưới dạng `{--a | --b}` và các cờ tùy chọn dưới dạng `[...]`.

---

## Tóm tắt cấp cao nhất

```
chloros-cli [global options] COMMAND [command options]
```

### Tùy chọn toàn cục

| Cờ | Mô tả |
| --- | --- |
| `--backend-exe PATH` | Ghi đè tệp thực thi backend được phát hiện tự động. |
| `--port N` | Cổng HTTP của backend (mặc định: `5000`). Chỉ được sử dụng bởi các lệnh cốt lõi (`process`, `login`, `logout`, `status`, `export-status`, `time-sync`, `selftest`); `lattice`, `project` và `daq` sử dụng `CHLOROS_BACKEND_URL` (`http://127.0.0.1:5000` mặc định). |
| `-v, --verbose` | Bật chế độ hiển thị chi tiết. |
| `--restart` | Buộc khởi động lại backend (kết thúc mọi tiến trình `backend_server.py` đang chạy). Một backend được chạy bởi dịch vụ systemd (gói .deb trên Linux) sẽ không bao giờ bị dừng: lệnh sẽ hiển thị thông báo `sudo systemctl restart <unit>` thay thế và thoát khỏi `1`. |
| `--version` | Hiển thị phiên bản (`Chloros CLI 1.3.0`). |
| `--help` | Hiển thị hướng dẫn sử dụng cấp cao nhất. |

### Chỉ mục lệnh

| Lệnh | Mục đích |
| --- | --- |
| [`process`](#chloros-cli-process) | Xử lý toàn bộ thư mục chứa các bản ghi Survey3 hoặc LATTICE từ đầu đến cuối. |
| [`login`](#chloros-cli-login) | Xác thực máy này bằng tài khoản Chloros+. |
| [`logout`](#chloros-cli-logout) | Xóa thông tin đăng nhập đã lưu trong bộ nhớ đệm. |
| [`status`](#chloros-cli-status) | Hiển thị trạng thái giấy phép / xác thực hiện tại. |
| [`export-status`](#chloros-cli-export-status) | Hiển thị tiến trình xuất Thread-4 trực tiếp trong khi chạy `process`. |
| [`language`](#chloros-cli-language) | Đặt hoặc liệt kê ngôn ngữ hiển thị CLI (hỗ trợ 38 ngôn ngữ). |
| [`set-project-folder`](#project-folder-commands) / [`get-project-folder`](#project-folder-commands) / [`reset-project-folder`](#project-folder-commands) | Thư mục dự án mặc định (chia sẻ với GUI). |
| [`update`](#chloros-cli-update) | Kiểm tra và cài đặt các bản cập nhật Chloros (Linux/Jetson). |
| [`selftest`](#chloros-cli-selftest) | Chẩn đoán hệ thống + kiểm tra chức năng cơ bản. |
| [`time-sync`](#chloros-cli-time-sync) | Trạng thái / điều khiển máy chủ chính PTP. |
| [`lattice`](#chloros-cli-lattice) | Điều khiển và ghi hình camera LATTICE (hơn 45 lệnh con). |
| [`daq`](#chloros-cli-daq) | Điều khiển cảm biến quang phổ DAQ (DAQ-U / DAQ-M / DAQ-E). |
| [`project`](#chloros-cli-project) | Mở và điều khiển một dự án Chloros đã lưu (camera + DAQ). |

---

## Cài đặt

`chloros-cli` được tích hợp sẵn trong trình cài đặt Chloros dành cho máy tính để bàn trên mọi nền tảng được hỗ trợ — không có bản tải xuống CLI riêng biệt. Việc cài đặt gói nền tảng sẽ thêm `chloros-cli` vào thư mục `PATH` của bạn cùng với ứng dụng máy tính để bàn và tệp nhị phân backend mà nó điều khiển.

Các bản tải xuống mới nhất: [`https://mapir.gitbook.io/chloros/download`](https://mapir.gitbook.io/chloros/download)

> Trình cài đặt cũng đi kèm với các tập lệnh khởi chạy tiện lợi (`Chloros_CLI.bat` / `Chloros_CLI.ps1`, `Launch_CLI.*`, `chloros-cli.sh`) để mở một cửa sổ dòng lệnh (CLI) sẵn sàng sử dụng.

### Windows (.exe)

1. Tải xuống trình cài đặt Windows từ trang tải xuống.
2. Chạy `Chloros Installer 1.3.0.exe` và làm theo hướng dẫn của trình hướng dẫn. Đường dẫn cài đặt mặc định là `C:\Program Files\Chloros\` (CLI sẽ được đặt trong thư mục `C:\Program Files\Chloros\cli\`, mà trình cài đặt sẽ thêm vào biến môi trường PATH).
3. Mở một cửa sổ terminal mới (`cmd.exe`, PowerShell hoặc Windows Terminal) để hệ thống nhận diện được phiên bản `PATH` đã được cập nhật.

```powershell
chloros-cli --version
```

Trình cài đặt sẽ tự động thêm `chloros-cli.exe` vào thư mục hệ thống `PATH` của bạn và gói kèm các trình điều khiển camera LATTICE. Phần nền là `C:\Program Files\Chloros\backend\chloros-backend.exe` (thư mục `backend` trong thư mục cài đặt của bạn), thành phần này không có trên `PATH`.

**Cài đặt hệ thống.** Trang *Chọn tác vụ bổ sung* của trình cài đặt có hai tùy chọn trong phần **Cài đặt hệ thống**, cả hai đều**được chọn mặc định**:

| Tùy chọn (theo nội dung trong trình cài đặt) | Chức năng | Tắt cài đặt im lặng |
| --- | --- | --- |
| **Bật đường dẫn dài của Windows** (cho phép thư mục dự án nằm sâu hơn 260 ký tự; được khuyến nghị) | Đặt giá trị `LongPathsEnabled` trên toàn hệ thống Windows thành 1. Windows được cài đặt sẵn với tùy chọn này tắt, và do đó không có đường dẫn nào được phép vượt quá 260 ký tự; các sản phẩm của Chloros nằm sâu hơn khoảng 130 ký tự so với thư mục dự án, nên thư mục dự án nằm sâu hơn sẽ bị từ chối (xem [Khắc phục sự cố](#troubleshooting-pointers)). Cài đặt này có hiệu lực đối với các chương trình được khởi chạy sau khi cài đặt. Giữ nguyên khi gỡ cài đặt, vì các chương trình khác có thể phụ thuộc vào cài đặt này. | `/MERGETASKS="!longpaths"` |
| **Loại trừ backend Chloros khỏi quá trình quét của Microsoft Defender**(quá trình chụp ảnh từ camera nhanh hơn nhiều; được khuyến nghị) | Thêm một ngoại lệ**quá trình** cho `chloros-backend.exe` trong thư mục cài đặt, do đó tính năng bảo vệ thời gian thực sẽ không quét từng tệp mà backend ghi vào, bất kể tệp đó được ghi ở đâu. Không có chương trình nào khác bị loại trừ; việc loại trừ này sẽ bị xóa khi gỡ cài đặt. Trong trường hợp Defender không có hoặc bị khóa bởi chính sách, bước này sẽ thất bại mà không có thông báo và quá trình cài đặt sẽ tiếp tục. | `/MERGETASKS="!defenderexclusion"` |**Tại sao tùy chọn Defender lại quan trọng:** ở tốc độ ghi hình cao, việc quét thời gian thực các tệp ghi hình tiêu tốn khoảng 30% tốc độ ghi hình (đo được trên hệ thống ghi hình liên tục với 9 camera: 77,7 lần ghi/giây (không quét), 52,7 lần ghi/giây (đã quét)). `lattice array-capture --continuous` sẽ hiển thị thông báo khi phát hiện Defender sử dụng một nhân CPU trở lên trong quá trình ghi hình; hãy chạy lại trình cài đặt với tùy chọn này được chọn.

Quá trình cài đặt im lặng (`/SILENT` hoặc `/VERYSILENT`) sẽ áp dụng cả hai tùy chọn trừ khi người dùng từ chối, ví dụ: `"Chloros Installer 1.3.0.exe" /VERYSILENT /MERGETASKS="!defenderexclusion"`.

**Tường lửa Windows.** Trình cài đặt luôn thêm hai quy tắc vào cho `chloros-backend.exe` trên mọi cấu hình mạng, và trình gỡ cài đặt sẽ xóa chúng: `Chloros Backend (UDP-In)` dành cho luồng video camera, PTP, mDNS và luồng dữ liệu DAQ-E, và `Chloros Backend (TCP-In)`, giới hạn trong mạng con cục bộ, dành cho việc cập nhật firmware DAQ-E, trong đó cảm biến kết nối trở lại máy tính này để thu thập hình ảnh (xem [Cài đặt Thiết bị DAQ-E](#daq-e-device-settings)). HTTP API của phần nền vẫn được liên kết với `127.0.0.1`.

### Linux amd64 (.deb)

Dành cho Ubuntu 22.04 LTS trở lên hoặc các máy trạm x86_64 dựa trên Debian.

> **Ubuntu 20.04 không được hỗ trợ.** Danh sách phụ thuộc của gói phần mềm được xác định dựa trên
> những thành phần mà phần lõi thực sự liên kết, và điều này bao gồm `libc6 (>= 2.35)`;
> phiên bản Focal đi kèm với glibc 2.31. `apt` từ chối cài đặt thay vì để quá trình cài đặt thất bại tại
> thời điểm chạy. Danh sách này cũng đề cập đến `libgl1`, thành phần mà OpenCV đi kèm sẽ tải và một
> hình ảnh máy chủ tối thiểu có thể thiếu; `apt install` cung cấp thành phần này.

```bash
sudo apt install ./chloros_1.3.0_amd64.deb
chloros-cli --version
```

Tệp .deb cài đặt:
- Từ `chloros-cli` đến `/usr/bin/chloros-cli`, và `chloros-backend` (khởi động phần backend) đến `/usr/bin/chloros-backend`
- Phần backend và giao diện dòng lệnh (CLI) đã được biên dịch, giải nén, nằm trong `/usr/lib/chloros/backend/` và `/usr/lib/chloros/cli/`
- Các trình điều khiển camera LATTICE
- Các mô hình khử nhiễu, gói hiệu chuẩn và cấu hình kênh cập nhật
- Bộ công cụ phát triển phần mềm (PythonSDK) cho hệ thống `python3` (`import chloros_sdk`). `apt install` cũng đi kèm với các yêu cầu của nó, bao gồm `python3-numpy` và `python3-requests` (được khuyến nghị nhưng không bắt buộc: các chương trình trên là độc lập; `dpkg -i` bỏ qua chúng, và kết quả cài đặt sẽ liệt kê những thành phần còn thiếu). Trong môi trường virtualenv: `pip install /usr/lib/chloros/sdk/chloros_sdk-*.whl`
- `/etc/sysctl.d/60-chloros-gige.conf`, giúp tăng kích thước bộ đệm nhận socket của kernel cho các luồng camera lên 32 MB và áp dụng ngay lập tức (nếu giá trị đã được đặt cao hơn thì sẽ giữ nguyên). Gỡ bỏ gói phần mềm sẽ xóa tệp này. Xem [Máy chủ Linux — bộ đệm nhận socket](#linux-hosts--socket-receive-buffers).
- Hai dịch vụ systemd tùy chọn cho backend, cả hai đều chưa được kích hoạt (phần tiếp theo)

#### Chạy backend dưới dạng dịch vụ (chế độ không giao diện đồ họa)

Nếu không có dịch vụ, backend (và cùng với đó là PTP grandmaster) chỉ chạy khi có thứ gì đó khởi động nó: `chloros-backend` trong terminal, một tập lệnh SDK, hoặc lệnh chính `chloros-cli` trong suốt thời gian chạy của nó. Để có một backend luôn hoạt động trên máy không giao diện đồ họa, hãy kích hoạt dịch vụ **dưới quyền người dùng của bạn**:

```bash
sudo systemctl enable --now chloros-backend@$USER.service    # $USER expands to your user name
systemctl status chloros-backend@$USER.service
```

`chloros-backend@<user>.service` sẽ chạy backend dưới danh nghĩa người dùng đó. Các bản ghi mà nó tạo ra thuộc về người dùng đó, nhật ký chi tiết của nó nằm trong tệp `~/.cache/chloros/logs` của người dùng đó, và nó sử dụng tệp `chloros-cli login` (`~/.chloros`) của người dùng đó, vì vậy hãy đăng nhập một lần với tư cách người dùng đó trước. PTP không cần quyền root. Giống như mọi backend đã cài đặt, nó chỉ lắng nghe trên cổng `127.0.0.1:5000`.

`chloros-backend.service` chạy backend với tư cách **root**. Nó vẫn được cài đặt để máy tính đã kích hoạt dịch vụ này tiếp tục hoạt động, nhưng mọi dữ liệu mà backend root ghi lại đều thuộc về root, do đó người dùng không thể xóa các bản ghi của chính mình. Để chuyển sang tài khoản người dùng của bạn:

```bash
sudo systemctl disable --now chloros-backend.service
sudo systemctl enable --now chloros-backend@$USER.service
```

Nhật ký chi tiết của dịch vụ root nằm trong tệp `/var/log/chloros`, có thể được đọc bởi các thành viên của nhóm `adm` (dịch vụ này thiết lập biến môi trường `CHLOROS_LOG_DIR`; xem [Biến môi trường](#environment-variables)). Khi cài đặt gói phần mềm, lệnh `enable` sẽ được hiển thị với tên người dùng của bạn đã được điền sẵn, cùng với tùy chọn chuyển đổi khi dịch vụ root được kích hoạt.

**Kích hoạt một đơn vị backend.** Chỉ có một đơn vị backend chạy tại một thời điểm. Một đơn vị thứ hai được kích hoạt (`chloros-backend.service` bên cạnh `chloros-backend@<user>.service`, hoặc hai đơn vị `@` của hai người dùng) sẽ không khởi động được và vẫn ở trạng thái `failed` với trạng thái thoát 98; systemd sẽ không khởi động lại nó. Tệp `systemctl status` giải thích lý do: `Another instance is already running. Exiting.`, sau đó là yếu tố đang giữ backend (cổng 5000, hoặc tệp khóa và người dùng sở hữu tệp đó), sau đó là `systemd will not restart <unit> (exit status 98)`. Sau khi khởi động lại, một trong hai đơn vị có thể là đơn vị chạy. Để chuyển đổi, hãy vô hiệu hóa đơn vị mà bạn không sử dụng, như đã nêu ở trên.**CLI** không bao giờ dừng phần backend của dịch vụ. `chloros-cli` sử dụng nó nguyên trạng, giống như GUI và SDK: `process` chạy trên đó, tải dự án của chính nó thay thế cho dự án cuối cùng. Trong trường hợp CLIs sẽ khởi động lại phần nền (`--restart`, hoặc một `process` mà quá trình nhập không tìm thấy gì), nó sẽ đặt tên cho đơn vị, in ra `sudo systemctl restart <unit>` và thoát khỏi `1`, vì một backend bị dừng theo cách đó sẽ vẫn ở trạng thái tắt. `GET /api/status` đặt tên đơn vị là `service` (`null` đối với backend không có dịch vụ nào đang chạy).**Nâng cấp và gỡ bỏ.** Việc nâng cấp gói phần mềm sẽ dừng mọi dịch vụ backend, sau đó khởi động lại từng dịch vụ đã được kích hoạt, bắt đầu từ các đơn vị `@<user>` (một đơn vị được khởi động thủ công nhưng chưa bao giờ được kích hoạt sẽ vẫn ở trạng thái dừng, giống như sau khi khởi động lại hệ thống). Khi có nhiều hơn một dịch vụ được kích hoạt, chỉ có một dịch vụ chạy và các dịch vụ còn lại vẫn ở trạng thái lỗi như trên; quá trình nâng cấp sẽ hiển thị mã lỗi `Chloros: N backend units are enabled: …` và `Keep one enabled: sudo systemctl disable --now <unit>`. Việc gỡ bỏ gói này sẽ dừng và vô hiệu hóa các dịch vụ này. Việc xóa hoàn toàn cũng sẽ xóa `/var/log/chloros`.

### Linux arm64 — Jetson (JetPack 6)

```bash
sudo apt install ./chloros_1.3.0_arm64_jp6.deb
chloros-cli --version
```

Cấu trúc tương tự như gói .deb cho amd64, bao gồm các bộ đệm nhận và [dịch vụ backend](#running-the-backend-as-a-service-headless), dành cho Jetson Orin / Orin NX / Orin Nano trên **JetPack 6** (L4T R36). PyTorch trong gói này là phiên bản CUDA của JetPack 6 dành cho Orin, do đó việc xử lý GPU — bao gồm cả thuật toán debayer nhận diện kết cấu — sẽ chạy trên GPU của Jetson. Gói này không chứa CUDA: nó sử dụng các thư viện CUDA và cuDNN mà JetPack 6 cài đặt (`sudo apt install nvidia-jetpack` trên hình ảnh không có các thư viện này). Trong trường hợp không có GPU CUDA nào có thể sử dụng được, quá trình xử lý sẽ chạy trên CPU và tính năng nhận diện kết cấu sẽ sử dụng thuật toán debayer tiêu chuẩn (xem [`process`](#chloros-cli-process)).

### Xác thực một lần cho mỗi máy

Mỗi nền tảng yêu cầu đăng nhập Chloros+ một lần trước khi các lệnh SDK/CLI hoạt động:

```bash
chloros-cli login user@example.com 'YourPassword'
```

Thông tin đăng nhập được lưu trong bộ nhớ đệm tại `~/.chloros/user_session.json`.

### Khởi động backend

Các lệnh `lattice`, `project` và `daq` giao tiếp với một backend đang chạy; chúng không bao giờ khởi động backend (xem [Quy ước](#conventions)). Hãy giữ một backend đang chạy trong khi bạn sử dụng các lệnh này:

| Nền tảng | Cách thực hiện |
| --- | --- |
| Windows | Mở ứng dụng máy tính để bàn Chloros (backend của ứng dụng này cũng phục vụ cho CLI), hoặc chạy lệnh `"C:\Program Files\Chloros\backend\chloros-backend.exe"` trong một cửa sổ terminal riêng (thư mục `backend` trong thư mục cài đặt của bạn) và để nó mở. |
| Linux/Jetson | Chạy `chloros-backend` trong một cửa sổ terminal riêng, hoặc luôn giữ một bản chạy với [dịch vụ backend](#running-the-backend-as-a-service-headless). |

Chỉ có một backend chạy tại một thời điểm; mọi client (GUI, CLI, các tập lệnh SDK) đều chia sẻ nó. Các lệnh cốt lõi (`process`, `login`, `status`, …) cũng sử dụng nó và sẽ khởi động một dịch vụ khi không có dịch vụ nào đang chạy.

**Một backend tiếp tục chạy giữa các lệnh**, do đó lệnh tiếp theo sẽ không phải khởi động lại backend. Nó sẽ dừng khi bạn đăng xuất (`chloros-cli logout`, trừ khi ứng dụng Chloros đang mở hoặc đang có quá trình chạy hoặc xây dựng ngoại tuyến), khi bạn chạy lệnh `chloros-cli backend stop`, hoặc tự động dừng sau 30 phút nếu không có gì sử dụng nó: không có lệnh, tập lệnh hay ứng dụng nào, không có phiên chạy hay bản dựng ngoại tuyến nào, không có camera hay cảm biến ánh sáng nào được kết nối. `backend stop` từ chối khi một phiên chạy hoặc bản dựng ngoại tuyến đang diễn ra, đồng thời nêu rõ tên của nó; `--force` sẽ dừng nó ngay lập tức và phiên chạy hoặc bản dựng đó sẽ bị mất (một loạt dữ liệu được giữ lại, và `array-build-video` sẽ xây dựng lại nó). Đặt `CHLOROS_BACKEND_IDLE_MIN` trước khi dịch vụ backend bắt đầu để thay đổi khoảng thời gian 30 phút (`0`: không bao giờ); dịch vụ backend (#running-the-backend-as-a-service-headless) sẽ không bao giờ tự động dừng lại. `chloros-cli backend status` hiển thị xem có dịch vụ nào đang chạy không, ứng dụng nào đang sử dụng nó và bất kỳ bản dựng ngoại tuyến nào đang chạy. `process` mở dự án của nó trong backend đang chạy, trừ khi ứng dụng Chloros đang mở với một dự án mà nó đã mở: trong trường hợp đó, nó sẽ từ chối, và bạn phải xử lý từ ứng dụng hoặc đóng dự án đó trước. Một dự án mà phiên bản `process` trước đó hoặc một tập lệnh SDK để mở sẽ đơn giản bị thay thế.

### Xác minh cài đặt

```bash
chloros-cli --version           # prints "Chloros CLI 1.3.0"
chloros-cli selftest            # 7-step diagnostic (backend, GPU, models, CUDA)
chloros-cli status              # shows license tier + logged-in user
```

`selftest` chỉ thoát khỏi `0` khi tất cả bảy bước kiểm tra đều thành công. Bước cuối cùng yêu cầu GPU CUDA (GPU NVIDIA, hoặc Jetson Orin) và các mô hình khử nhiễu; do đó, trên máy tính không có GPU, hệ thống sẽ hiển thị `Failed: CUDA + Denoiser` và thoát khỏi `1`; mọi chức năng vẫn hoạt động bình thường ngoại trừ thuật toán debayer nhận diện kết cấu chỉ chạy trên GPU.

> **Yêu cầu đăng ký Chloros+.**Giao diện dòng lệnh (CLI) yêu cầu gói Chloros+ đang hoạt động.**Copper**là gói Chloros+ cơ bản — mọi gói Chloros+ trả phí đều có quyền truy cập CLI/SDK; chỉ có gói miễn phí**Iron** là không có. (Bảng đối chiếu ID gói: `0`=Iron/miễn phí, `1`=Copper, `2`=Bronze, `3`=Silver, `4`=Gold.) Nâng cấp tại [`https://cloud.mapir.camera/pricing`](https://cloud.mapir.camera/pricing).
>
> Giới hạn này được áp dụng bởi hệ thống phía máy chủ (backend), không chỉ bởi giao diện dòng lệnh (CLI): yêu cầu được gửi qua SDK hoặc CLI mà không có gói dịch vụ trả phí sẽ bị từ chối với mã lỗi `403 PLAN_UPGRADE_REQUIRED`, dù yêu cầu đó đến từ `chloros-cli`, Python SDK hay một client HTTP tự phát triển. Người gọi đã đăng xuất sẽ nhận được mã lỗi `401 AUTH_REQUIRED` thay thế. Quyền truy cập vẫn hoạt động ngoại tuyến trong thời gian ân hạn của gói dịch vụ (30 ngày đối với gói hàng tháng, đến khi hết hạn đối với gói hàng năm) và ngừng khi thời gian này hết hạn; mã lỗi `chloros-cli status` vẫn tiếp tục hoạt động để hiển thị lý do (đây là đường dẫn duy nhất của SDK/CLI được miễn trừ khỏi cơ chế kiểm soát cấp độ — `GET /api/license-status`).

---

## Hướng dẫn bắt đầu nhanh trong 5 phút

```bash
# 1. Sign in once on this machine
chloros-cli login user@example.com 'YourPassword'

# 2. Survey3 / LATTICE folder → finished radiance + NDVI in one call
chloros-cli process "/home/user/captures/flight_001" \
  --vignette --reflectance --indices NDVI GNDVI OSAVI

# 3. The lattice / daq commands need a running backend: on Windows open the
#    Chloros desktop app; on Linux start one (or see "Start the Backend")
chloros-backend &

# 4. Take a single LATTICE photo with the first camera found
chloros-cli lattice capture -o output/

# 5. Connect a 4-cam LATTICE array with the GUI's smart-prep flow
chloros-cli lattice array-connect \
  --serials 213800234,214000533,214701288,214701292

# 6. Read a spectrum from a connected DAQ-U (Windows: --port COM3)
chloros-cli daq pool-connect --port /dev/ttyUSB0
chloros-cli daq pool-latest --sensor-id CB-7C-A8-2E-5F   # id from 'daq pool-list'
```

`process` (bước 2) sẽ khởi động một backend cho chính quá trình chạy của nó khi chưa có backend nào đang chạy và dừng nó khi hoàn tất, vì vậy hãy khởi động một backend cho các bước từ bước 4 trở đi. Xem [Khởi động Backend](#start-the-backend).

---

## `chloros-cli process`

Xử lý một thư mục hình ảnh qua toàn bộ quy trình Chloros (phát hiện mục tiêu → hiệu chỉnh → hiệu ứng viền → phản xạ → xuất chỉ số).

### Tóm tắt

```
chloros-cli process INPUT [OPTIONS]
```

### Các tham số vị trí

| Tham số | Mô tả |
| --- | --- |
| `INPUT` | Đường dẫn đến thư mục đầu vào chứa các tệp `.raw + .jpg` (Survey3), `.tif/.tiff` (LATTICE) hoặc `.dng`. |

### Tùy chọn chung

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `-o, --output PATH` | một thư mục mới được đặt tên theo `-n` trong thư mục dự án mặc định | Thư mục dự án. Dự án được đặt tên theo thư mục này, và nếu đã có một thư mục chứa tệp `project.json` thì tệp này sẽ được đặt tên là `_1` / `_2` thay vì bị ghi đè. Thư mục dự án mặc định là thư mục mà `set-project-folder` (hoặc giao diện người dùng) đã chọn, nếu không thì là `Documents/MAPIR/Chloros Projects`. |
| `-n, --project-name NAME` | tự động (dấu thời gian) | Tên dự án, chỉ được sử dụng **khi không có** `-o` (khi có `-o`, dự án sẽ lấy tên từ thư mục). |
| `--debayer {standard,texture-aware}` | `standard` | `texture-aware` sử dụng thuật toán khử hiệu ứng Bayer thần kinh Chloros+ cho hình ảnh Survey3; chậm hơn nhưng chất lượng cao hơn. Tính năng này chỉ dành riêng cho Survey3: Các ảnh chụp LATTICE luôn sử dụng thuật toán demosaic tiêu chuẩn, do đó đối với thư mục chứa ảnh chụp LATTICE, `process` sẽ từ chối `texture-aware` sau khi nhập, kèm theo thông báo lỗi nêu rõ số lượng ảnh, và thoát khỏi `1` trước khi xử lý bất kỳ dữ liệu nào (ứng dụng Chloros cũng không hỗ trợ tính năng Texture Aware cho các tệp này). Chương trình yêu cầu GPU CUDA (GPU NVIDIA trên Windows / Linux x64, hoặc GPU của Orin trên Jetson): nếu không có GPU, quá trình chạy sẽ sử dụng `standard`, và phần backend ghi lại một dòng `[DEBAYER] … needs a CUDA GPU …` giải thích lý do (được hiển thị cùng với `--verbose`). |
| `--vignette / --no-vignette` | `--vignette` | Chỉnh sửa hiệu ứng viền tối. |
| `--reflectance / --no-reflectance` | `--reflectance` | Hiệu chuẩn độ phản xạ (sử dụng mục tiêu bảng nếu tìm thấy, hiệu chuẩn theo số sê-ri của NIST cho LATTICE). Đối với dữ liệu đa phổ LATTICE, bước này cũng đóng vai trò là công cụ bật/tắt **sản phẩm** độ phản xạ — xem [Các tùy chọn xuất theo từng sản phẩm](#per-product-export-toggles-lattice-multispectral). |
| `--ppk` | tắt | Áp dụng các hiệu chỉnh PPK GNSS từ các tệp sidecar. |
| `--exposure-pin-1 CAMERA_MODEL` | không | Mô hình máy ảnh được kết nối với chân phơi sáng số 1, để đồng bộ hóa thời gian PPK. |
| `--exposure-pin-2 CAMERA_MODEL` | không | Mô hình máy ảnh được kết nối với chân phơi sáng số 2, để đồng bộ hóa thời gian PPK. |
| `--recal-interval SECONDS` | `0` | Khoảng thời gian hiệu chỉnh lại tối thiểu, tính bằng giây (tham số *Khoảng thời gian hiệu chỉnh lại tối thiểu* của dự án). |
| `--timezone-offset HOURS` | `0` | Chênh lệch múi giờ của cảm biến ánh sáng so với UTC, tính bằng giờ (tham số *Chênh lệch múi giờ cảm biến ánh sáng* của dự án). |
| `--format FORMAT` | `TIFF (16-bit)` | Một trong các giá trị sau: `TIFF (16-bit)`, `TIFF (32-bit, Percent)`, `TIFF (8-bit)`, `PNG (8-bit)`, `JPG (8-bit)`. Chloros không bao giờ tăng độ sâu bit của hình ảnh: trên tất cả các hình ảnh 8-bit, định dạng sâu hơn được ghi dưới dạng `TIFF (8-bit)` và quy trình này chỉ đề cập đến điều đó một lần. Trong một dự án kết hợp các ảnh chụp 8-bit và 12/16-bit, mỗi ảnh chụp sẽ được xuất ở độ sâu bit của chính nó hoặc thấp hơn. Độ sáng (Radiance) luôn là 32-bit số thực. |
| `--indices NAME [NAME ...]` | không | Các chỉ số thực vật theo tên cài đặt sẵn (`NDVI`, `GNDVI`, `OSAVI`, `SAVI`, `EVI`, `GCI`, …), không phân biệt chữ hoa chữ thường. Mỗi chỉ số được thêm vào dự án dựa trên các dải phổ của máy ảnh tương ứng, kèm theo một dự án mới. Tên không phải là cài đặt sẵn, hoặc tên mà các dải phổ của máy ảnh không thể tạo ra (ví dụ: `NDRE` cần dải RE và NIR, mà không có camera nào cung cấp đầy đủ), sẽ được liệt kê trong cảnh báo và không nhận được hình ảnh chỉ mục. |
| `--input-level {auto,raw,debayered,processed}` | `auto` | Buộc điểm vào của quy trình xử lý cho các tệp TIFF LATTICE (tệp .raw của Survey3 không bị ảnh hưởng). Ngoài ra, đây là lối thoát cho phép xử lý hoàn toàn một lần chụp **không có tệp raw** — xem [Thư mục captures trông như thế nào](#what-a-captures-folder-looks-like). |
| `--debayered / --no-debayered` | bật | Xuất sản phẩm đã khử hiệu ứng Bayer tuyến tính (`Debayered_Images`). Xem [Các công tắc xuất theo từng sản phẩm](#per-product-export-toggles-lattice-multispectral). |
| `--preview / --no-preview` | bật | Xuất bản xem trước trên màn hình (`Preview_Images`): RGB = cân bằng trắng (nguồn sáng DAQ nếu có, nếu không thì chế độ gray-world) + gamma; đa phổ = kéo dãn màu giả. |
| `--radiance / --no-radiance` | bật | Xuất dữ liệu radiance dạng float32 (`Radiance_Images`, W/m²/sr/nm). |
| `--reflectance-source {daq,target,auto}` | `auto` | Tham chiếu cho sản phẩm phản xạ LATTICE: `auto` = Mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) là tham chiếu tuyệt đối (một mục tiêu có cờ QA sẽ cạnh tranh với DAQ về điểm số đo được), phương án dự phòng là DAQ-downwelling (ρ = π·L/E); `target` = nghiêm ngặt (không thay thế bằng DAQ); `daq` = DAQ là nguồn tham chiếu chính thức. Xem [Công tắc xuất theo từng sản phẩm](#per-product-export-toggles-lattice-multispectral). |
| `--target-reflectance-dir DIR` | không | Thư mục chứa các bản quét độ phản xạ mục tiêu **đo được** theo từng đơn vị (`<serial>.csv`); sẽ chuyển sang sử dụng phổ T3/T4P danh nghĩa nếu không tìm thấy. |
| `--array-alignment / --no-array-alignment` | bật | Mảng LATTICE: áp dụng sự căn chỉnh giữa các mô-đun được ghi trong tệp XMP của mỗi lần chụp (`Chloros:Alignment*`) cho mọi sản phẩm đã xử lý (loại bỏ hiệu ứng Bayer / xem trước / độ sáng / độ phản xạ / chỉ số). Không thực hiện thao tác nào đối với các hình ảnh không có thẻ này. |
| `--array-alignment-crop / --no-array-alignment-crop` | cắt | Cắt các tệp xuất đã căn chỉnh sao cho nằm trong vùng chồng chéo chung của mảng, để tất cả các mô-đun chia sẻ cùng một vùng phủ; `--no-…` giữ nguyên khung cảm biến đầy đủ (điền màu đen bên ngoài vùng nguồn). |
| `--array-alignment-interp {bilinear,nearest,cubic}` | `bilinear` | Lấy mẫu lại cho biến dạng căn chỉnh. `nearest` giữ nguyên các giá trị DN chính xác của nguồn (không trộn lẫn các giá trị đo quang giữa các điểm ảnh). |

### Tùy chọn phát hiện mục tiêu

| Cờ | Mô tả |
| --- | --- |
| `--min-target-size PIXELS` | Kích thước tối thiểu của mục tiêu hiệu chuẩn (tính bằng pixel) cho bộ phát hiện. |
| `--target-clustering 0-100` | Tỷ lệ phần trăm nhóm mục tiêu (mặc định là `60`). |
| `--target / --targets` | Chỉ tìm kiếm mục tiêu hiệu chuẩn trong thư mục con `target` hoặc `targets` của tệp đầu vào (nhanh hơn). |

### Ví dụ

```bash
# Simplest: defaults are good for most surveys
chloros-cli process "/home/user/images/survey_001"

# Multi-index with explicit format
chloros-cli process "/home/user/images/survey_001" \
  --vignette \
  --reflectance \
  --format "TIFF (32-bit, Percent)" \
  --indices NDVI GNDVI OSAVI

# Texture-aware debayer for highest quality (Chloros+ only)
chloros-cli process "/home/user/images/survey_001" \
  --debayer texture-aware \
  --indices NDVI

# Process LATTICE captures explicitly (auto-detects from EXIF normally)
chloros-cli process "/home/user/captures/lattice_flight" \
  --input-level processed

# LATTICE multispectral → float32 radiance only (no DAQ downwelling needed)
chloros-cli process "/home/user/captures/lattice_flight" \
  --no-debayered --no-preview --no-reflectance

# LATTICE reflectance anchored to an in-frame target (strict, no DAQ fallback),
# with per-unit measured target scans looked up by serial
chloros-cli process "/home/user/captures/lattice_flight" \
  --reflectance-source target --target-reflectance-dir "/home/user/target_scans"

# LATTICE array capture: keep native geometry (ignore stamped alignment)
chloros-cli process "/home/user/captures/array_flight" \
  --no-array-alignment

# Aligned, uncropped, value-preserving resampling
chloros-cli process "/home/user/captures/array_flight" \
  --no-array-alignment-crop --array-alignment-interp nearest

# Save to a project folder of your choice (the project is named after it)
chloros-cli process "C:/input" -o "C:/projects/Field_A_2026-05-26"

# Name the project and let it go to the default project folder
chloros-cli process "C:/input" -n "Field_A_2026-05-26"
```

### Công tắc xuất theo từng sản phẩm (LATTICE đa phổ)

Quá trình xử lý LATTICE được phân chia thành **mọi sản phẩm áp dụng trong một lần chạy**. Bốn công tắc theo từng loại — `--debayered`, `--preview`, `--radiance`, `--reflectance` — đều**được bật theo mặc định**; sử dụng biểu mẫu `--no-<type>` để tắt một tùy chọn. Các camera chính RGB chỉ phát ra dữ liệu đã loại bỏ hiệu ứng Bayer + bản xem trước (không có độ sáng/độ phản xạ theo từng dải), do đó `--radiance`/`--reflectance` không có tác dụng đối với chúng. Các tùy chọn bật/tắt này bị bỏ qua đối với `.raw` của Survey3 (theo đường dẫn phản xạ/mục tiêu tiêu chuẩn). *(Cờ `--radiometric-output {reflectance,radiance,sensor-response}` đã bị **loại bỏ** và thay thế bằng các tùy chọn này; không còn mức `sensor-response` nữa.)*

| Sản phẩm | Đầu ra | Cần thu thập dữ liệu hướng xuống (DAQ) không? |
| --- | --- | --- |
| `--debayered` | Demosaic tuyến tính (`Debayered_Images`): cùng một tệp mà quá trình chụp lưu lại ở mức `debayered` — bù tối (DSNU) từ gói hiệu chuẩn trên dữ liệu thô, sau đó là khử mô-saic nhận diện cạnh của OpenCV; không có bảng tra tuyến tính (LUT), hiệu chỉnh trường phẳng hoặc tách kênh. | Không. |
| `--preview` | Xem trước trên màn hình (`Preview_Images`): RGB = cân bằng trắng (WB) + gamma; đa phổ = kéo dãn màu giả. | Không. |
| `--radiance` | giá trị float32 W/m²/sr/nm từ chuỗi đo quang học đầy đủ (`Radiance_Images`). | Số. |
| `--reflectance` | Độ phản xạ ρ uint16 (`32768` = 1,0), Sẵn sàng cho Pix4D. | **Có**, trừ khi có mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) làm điểm neo (xem bên dưới). |

`--reflectance-source` chọn tham chiếu độ phản xạ:**`auto`**(mặc định) lấy mục tiêu trong khung hình đã qua kiểm tra chất lượng (QA) làm**tham chiếu tuyệt đối**— các chuỗi đường thẳng thực nghiệm được neo bởi mục tiêu sẽ được so sánh chéo trên các tấm hình được giữ lại và kết quả đo được áp dụng theo từng dải: một dải mà DAQ không thể hiệu chuẩn, hoặc dải mà việc phù hợp độ dốc DAQ thất bại, sẽ lấy đường thẳng của chính mục tiêu đó (`ReflectanceElBandMethods` chỉ định cụ thể) — một mục tiêu vượt qua kiểm tra chất lượng (QA) nhưng có cờ cảnh báo (một bảng bị loại bỏ, một dải tần bị lỗi) cũng sẽ cạnh tranh với giá trị chia của DAQ dựa trên điểm số đo được đó, và khung hình mà nó thua sẽ được đánh dấu là `ReflectanceChain=daq` — chuyển sang ngưỡng phân chia xuống của DAQ (ρ = π·L/E) khi không có mục tiêu hoặc kiểm tra chất lượng (QA) thất bại;**`target`**áp dụng quy tắc nghiêm ngặt (không thay thế bằng DAQ);**`daq`**chọn chế độ hành vi do DAQ quyết định. Hình học mục tiêu (ArUco / vùng quan tâm cố định / dải) được lấy từ cấu hình mục tiêu của dự án; `--target-reflectance-dir DIR` lưu trữ các lần quét**đo lường** quét (`<serial>.csv`) được tra cứu theo số sê-ri/mã QR của thiết bị đích, với phổ T3/T4P danh định làm phương án dự phòng.

Đường dẫn phản xạ DAQ tự động giải quyết **dữ liệu hướng xuống khớp theo dấu thời gian**từ một tệp**`.daq`**(DAQ-U/M/E)**hoặc tệp `.csv` gốc của DAQ-M**được tìm thấy cùng với hình ảnh. Nếu gói hiệu chuẩn cho từng camera hoặc DAQ không được lưu trữ cục bộ, quy trình xử lý sẽ**tự động tải xuống từ AWS** khi sử dụng lần đầu (cần kết nối internet một lần; được lưu trữ dưới dạng `~/.chloros/`).**Đây là dữ liệu đo của ai.**Dữ liệu `.daq` mà một lần ghi lại lưu trữ là dữ liệu đo của (các) camera mà nó mô tả — được đặt tên trong tệp sidecar `<file>.daq.cameras.json` bên cạnh nó (`lattice capture`, `multi-capture`, `array-capture` và giao diện người dùng (GUI); một loạt ảnh thô (raw burst) liệt kê giá trị đo của từng thành viên trong bản kê khai của nó) — và `process` chỉ khớp với**các hình ảnh từ những camera đó**. Do đó, một camera độc lập không có dữ liệu đo lường riêng sẽ không còn mượn DAQ của camera khác nữa; hãy liên kết một DAQ với nó hoặc ghi lại một đoạn. Bản ghi riêng của DAQ (`daq pool-record`, hoặc bản ghi được bắt đầu trong GUI), hoặc bất kỳ tệp `.daq` nào không có tệp sidecar đó (bao gồm cả các bản ghi cũ), vẫn phục vụ cho mọi camera.

#### Đọc các pixel phản xạ (Pix4D / Metashape / các tập lệnh của riêng bạn)

Phản xạ được lưu trữ dưới dạng số nguyên DN, và **giá trị DN tương ứng với ρ = 1.0 phụ thuộc vào camera nguồn**:

| Nguồn | ρ = 1.0 là | Cách xác định |
| --- | --- | --- |
| LATTICE (M3C / M3M) | `32768` (dải giá trị lên đến ρ 2.0) | Tệp có đóng dấu XMP `Chloros:PixelScale=32768`. |
| Survey3 | `65535` (bị cắt tại ρ 1.0) | Không có thẻ XMP `Chloros:*` — chính sự vắng mặt đó *chính là* dấu hiệu. |

**Đọc giá trị `Chloros:PixelScale` và chia cho nó** thay vì giả định một hằng số. Thẻ này được định nghĩa trong miền uint16, do đó nó vẫn giữ nguyên giá trị `32768` trên các định dạng đầu ra thực hiện quy đổi tỷ lệ — `TIFF (16-bit)`, `TIFF (8-bit)`, `PNG (8-bit)`, `JPG (8-bit)` và `TIFF (32-bit, Percent)` đều tự mô tả (trước tiên hãy chuẩn hóa kiểu dữ liệu được lưu trữ trở lại uint16: ×257 từ 8-bit, ×65535 từ float).

> **Các tệp từ các bản dựng cũ hơn có thể không có tỷ lệ.** Một tệp TIFF phản xạ 8-bit được điều chỉnh tỷ lệ và đánh dấu là `Chloros:PixelScale` giống như các tệp khác. Các bản dựng trước phiên bản 1.3.0 thay vào đó đã *cắt* nó xuống phạm vi 0..255, do đó mọi giá trị lớn hơn ρ≈0.008 đều bị làm phẳng thành 255, và họ đã bỏ qua cả cặp `Chloros:PixelScale` lẫn `MicaSense:RadiometricCalibration`. **Nếu thẻ này vắng mặt trên tệp phản xạ LATTICE, đừng giả định có tỷ lệ — hãy xử lý lại tệp** thay vì chia các pixel vốn không thể chia được.

#### Đọc các pixel chỉ số (chỉ số NDVI và các chỉ số thực vật khác)

**Một raster chỉ số nguyên là quá trình điều chỉnh độ tương phản cho từng hình ảnh, không phải là bản đồ −1…1.** Các sản phẩm chỉ số `TIFF (16-bit)`, `TIFF (8-bit)`, `PNG (8-bit)` và `JPG (8-bit)` — bao gồm cả tệp TIFF 8-bit được tạo từ dữ liệu chụp 8-bit — lưu trữ

```

DN = (value − IndexMin) / (IndexMax − IndexMin) × full_scale
```

trong đó `IndexMin` / `IndexMax` là **các giá trị chỉ mục hữu hạn thấp nhất và cao nhất của chính khung hình đó**. DN 0 không phải là −1, và mức đầy đủ không phải là +1.

Hệ thống Chloros đánh dấu cả hai điểm cuối để phép ánh xạ vẫn có thể đảo ngược:

| Thẻ | Ý nghĩa |
| --- | --- |
| `Chloros:IndexScaling` | `linear_minmax` — quy ước dưới đây |
| `Chloros:IndexMin` | giá trị chỉ số được lưu trữ dưới dạng DN 0 |
| `Chloros:IndexMax` | giá trị chỉ số được lưu trữ dưới dạng giá trị toàn thang |

Đọc các giá trị này và đảo ngược, chuẩn hóa giá trị DN đã lưu thành khoảng 0…1 bằng cách lấy giá trị tối đa của kiểu dữ liệu (dtype) đã lưu trước tiên, để một biểu thức duy nhất có thể áp dụng cho mọi định dạng số nguyên:

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

> **`IndexMin == IndexMax`** có nghĩa là mọi pixel của khung hình đó đều có cùng một giá trị chỉ số. Hình raster được lấp đầy ở mức giữa và không có gì để chia — hãy lấy điểm cuối.

> **Không có thẻ `Chloros:Index*` có nghĩa là không có bản đồ được chỉ định.** Xuất LUT là một bản render có màu, trong đó các pixel là các màu chuyển sắc được liên kết với cửa sổ ngưỡng LUT của bạn, và một khung hình không có pixel chỉ số hữu hạn sẽ không có phạm vi để chỉ định. Đừng giả định có ánh xạ trong cả hai trường hợp — quy tắc này cũng áp dụng khi thiếu `PixelScale`.**Vì độ giãn được áp dụng cho từng hình ảnh, hai khung hình trong cùng một chuyến bay không thể so sánh với nhau về giá trị DN**— mỗi khung hình có các điểm cuối riêng, do đó cùng một giá trị NDVI sẽ tương ứng với các giá trị DN khác nhau trong các hình ảnh khác nhau. Đối với công việc định lượng trên toàn chuyến bay (thống kê theo vùng, ngưỡng cố định, chuỗi thời gian), hãy xuất**`TIFF (32-bit, Percent)`**: các raster chỉ số được ghi ở đó dưới dạng float32**chính các giá trị chỉ số**, không cần quy đổi tỷ lệ và không cần thẻ. Mặc dù tên định dạng là vậy, các sản phẩm chỉ số không phải là ×100 — tỷ lệ đó chỉ áp dụng cho phản xạ có kiểu dữ liệu số nguyên.

#### Mặt nạ giới hạn tách phổ (các pixel mà dải phổ bị giới hạn)

Quá trình tách phổ 3×3 có thể khiến một dải phổ trở nên âm — khi ánh sáng hầu như không chứa dải phổ đó, hoặc khi cảnh nằm ngoài phạm vi mà ma trận tách phổ của máy ảnh mô tả. Đó không phải là giá trị thực, do đó quá trình xuất dữ liệu sẽ cố định giá trị đó thành 0 — và chế độ xem trực tiếp cũng hiển thị giá trị 0 tương tự. Một dải phổ có thể bị giới hạn trên một vài pixel hoặc gần như tất cả các pixel; mặt nạ sẽ ghi lại cả hai trường hợp. `Chloros:Band{R,G,B}UnmixClampFraction` cho biết tỷ lệ khung hình bị ảnh hưởng; mặt nạ giới hạn cho biết **các pixel cụ thể nào**:

- **`clamp_masks/<product folder>/<product name>_clampmask.tif`**— trong thư mục `clamp_masks/`**bên cạnh** thư mục của sản phẩm, không bao giờ nằm bên trong nó (`radiance/x_radiance.tif` → `clamp_masks/radiance/x_radiance_clampmask.tif`; tệp xuất từ `process` sẽ tạo ra `tiff16/Reflectance_Calibrated_Images/x.tif` → `tiff16/clamp_masks/Reflectance_Calibrated_Images/x_clampmask.tif`) — đối với mỗi tệp dữ liệu về độ sáng, độ phản xạ và chỉ số trong đó có ít nhất một pixel bị giới hạn: các tệp ghi lại và xuất ra `process` đều như nhau (cùng với các tệp TIFF theo từng khung hình của `array-build-video --save-tiffs`). Dải đơn `uint8`, trên lưới pixel riêng của sản phẩm (bao gồm hướng, vùng quan tâm (ROI), nhóm pixel và căn chỉnh mảng). Mỗi mặt nạ ghi tên máy ảnh của sản phẩm (`Make` / `Model` / số seri), thời gian ghi hình và thẻ ống kính, đồng thời liệt kê các dải mà các bit của nó đại diện (`XMP-Camera:BandName`, bit 0 ở đầu); các bit `ImageDescription` và `Software` đánh dấu nó là mặt nạ kẹp, và nó không chứa thông tin hiệu chuẩn.
- **Bit**: bit 0 = dải trong kênh R của sản phẩm (`Chloros:BandRName`), bit 1 = G (`BandGName`), bit 2 = B (`BandBName`). Một bit được đặt có nghĩa là giá trị của pixel trong dải đó đã bị giới hạn ở 0 — hãy coi nó, cũng như bất kỳ chỉ số nào được xây dựng từ nó, là có độ tin cậy thấp.
- **`Chloros:UnmixClampMask`** được đóng dấu trên mọi sản phẩm đã chạy quá trình tách màu: đường dẫn của mặt nạ so với thư mục của chính sản phẩm (`../clamp_masks/<product folder>/<product name>_clampmask.tif`, luôn được phân tách bằng `/` — ghép nó với thư mục hình ảnh để mở mặt nạ), hoặc `none` khi không có pixel nào bị giới hạn. Không có mã này có nghĩa là sản phẩm chưa bao giờ chạy quá trình tách màu (máy ảnh RGB hoặc đơn sắc, mức độ thô/đã loại bỏ hiệu ứng debayering).
- `lattice capture` / `array-capture` in đường dẫn của mặt nạ dưới tệp đã lưu (`clamp mask: …`), và danh sách kết quả chụp sẽ liệt kê nó là `clamp_mask` của khung hình.
- `lattice index --live --save-multiband` tạo một mặt nạ cho mỗi camera nguồn trong quá trình ghép chồng đa dải — xem [`lattice index` — mặt nạ ghép chồng đa dải](#lattice-index--multiband-stack-masks). Video không có mặt nạ: `array-build-video` thay vào đó báo cáo các tỷ lệ giới hạn (clamp fractions) của từng khung hình (xem [`array-build-video`](#array-build-video--offline-reprocess-a-saved-burst)).

Vì không có thư mục sản phẩm nào chứa mặt nạ ở bất kỳ cấp độ nào, nên một thư mục sản phẩm (`radiance/`, `reflectance/`, thư mục `*_Images/` của tệp xuất `process`) có thể được chuyển trực tiếp cho Pix4D, Metashape hoặc phần mềm photogrammetry khác vì nó — bao gồm cả các thư mục con — và các phần mềm này chỉ chấp nhận hình ảnh. Hãy chuyển thư mục sản phẩm đó, chứ không phải thư mục ở trên nó (thư mục đó còn chứa `clamp_masks/` và mọi loại tệp xuất khác). Tính năng nhập liệu của Chloros sẽ bỏ qua các mặt nạ. Đọc thông tin về mặt nạ: `(mask & 1) != 0` là "dải kênh R đã bị giới hạn tại đây", `& 2` là G, `& 4` là B.

#### Dữ liệu EXIF được chuyển sang tệp xuất

`process` sao chép **khối GPS và ExifIFD** của ảnh gốc vào mỗi sản phẩm, do đó khi
xuất ra sẽ bao gồm `FocalLength`, `FNumber`, `ExposureTime`, `ISO`, `DateTimeOriginal` và
`CameraSerialNumber` cùng với thông tin địa lý.

**`FocalLength` là trường bắt buộc trong photogrammetry.** Pix4D tính toán khoảng cách mẫu mặt đất (GSD) từ
tiêu cự cộng với độ cao; nếu thiếu thẻ này, phần mềm sẽ tự động áp dụng một tỷ lệ hoàn toàn sai. Trong một
chuyến bay chụp 49 khung hình tại vườn cam, việc thiếu thẻ này đã biến khu vực 411 m × 160 m thành một bản tái tạo
47,8 km × 13 km — một ảnh chính quy 455 MP chủ yếu là vùng không có dữ liệu (nodata), sau đó được chẩn đoán là vấn đề ghép ảnh và
vấn đề BigTIFF trước khi ai đó kiểm tra GSD. Nếu ảnh chính quy của bạn hiển thị ở tỷ lệ không hợp lý, hãy chạy lệnh `exiftool -FocalLength` trên sản phẩm đã xuất trước tiên.

Bản sao này cố ý **không** phải là `-all:all`: các thẻ cấu trúc của IFD0'vì các thẻ cấu trúc của nó làm hỏng đầu ra LATTICE khi
được sao chép, và `ExifImageWidth` / `ExifImageHeight` bị loại trừ vì chúng mô tả
quá trình chụp *nguồn* — một tệp xuất đã từng được thay đổi kích thước sẽ mang theo các kích thước
mâu thuẫn với chính raster của nó. XMP được ghi trực tiếp thay vì sao chép, vì ExifTool
loại bỏ các thẻ XMP cùng phiên bản khi khối XMP được sao chép (điều này sẽ làm mất các thẻ hiệu chuẩn MAPIR).

### Vị trí lưu trữ kết quả

Các sản phẩm được ghi **vào thư mục dự án, được nhóm theo máy ảnh và sau đó theo định dạng tệp**:

```
<project>/
└── LATT-M3M-L41-F550/                  # one folder per camera model+lens+filter
    ├── tiff16/
    │   ├── Reflectance_Calibrated_Images/
    │   ├── Debayered_Images/
    │   ├── Preview_Images/
    │   └── <INDEX>_Index_Images/        # e.g. NDVI_Index_Images
    └── tiff32/
        └── Radiance_Images/             # float32 radiance always lands here
```

Thư mục máy ảnh là `LATT-<sensor>-<lens>-F<filter>` cho LATTICE (phù hợp với EXIF của bản chụp
`Model`) và `<model>_<filter>` dành cho Survey3 — hai máy ảnh chia sẻ cùng cảm biến và bộ lọc nhưng khác nhau
về ống kính sẽ được lưu trong các cây thư mục riêng biệt, vì hiện tượng tối góc, trường nhìn và độ méo khác nhau. Thư mục định dạng
thư mục tuân theo `--format`: `tiff16`, `tiff8`, `png8`, `jpg8`, hoặc `tiff32` cho
`TIFF (32-bit, Percent)`.

> **Mọi sản phẩm được xuất ra đều giữ nguyên tên tệp NGUỒN.** Một tệp xuất dữ liệu radiance của
> `capture_…_raw.tif` vẫn được gọi là `capture_…_raw.tif` — nó chỉ nằm trong
> `tiff32/Radiance_Images/`. **Thư mục xác định sản phẩm, không phải tên tệp**, vì vậy việc tìm kiếm theo mẫu
> cho `*radiance*.tif` sẽ không tìm thấy gì; thay vào đó, hãy so khớp với thư mục.

### Bản ghi cảm biến ánh sáng — đã hiệu chuẩn `.daq` + `.csv`

`process` cũng xử lý các bản ghi `.daq` trong thư mục đầu vào của bạn, và nó **không**
cần bất kỳ hình ảnh nào để thực hiện việc này: một DAQ-U / DAQ-M / DAQ-E được bay riêng lẻ đã là một
bản ghi hoàn chỉnh, và một thư mục chỉ chứa các tệp `.daq` là đầu vào hợp lệ.

Một thiết bị DAQ có thể được ghi lại **mà không cần** hiệu chuẩn — đó chính là cách mà các bộ ghi công khai
[`chloros_scripts`](https://github.com/mapircamera/chloros_scripts)
(`record_daq.py`) thực hiện theo mặc định: chúng ghi lại số đếm thô từ cảm biến và đóng dấu thời gian lên tệp để
Chloros lấy dữ liệu hiệu chuẩn nhà máy của cảm biến đó **qua cổng nối tiếp** (trước tiên từ bộ nhớ đệm cục bộ,
sau đó từ đám mây MAPIR) và áp dụng nó. `process` ghi kết quả trở lại:

```
<project>/
└── Light Sensor/
    ├── <name>_calibrated.daq        # reprocessable archive, declares its bundle
    └── <name>_calibrated.csv        # W/m^2/nm per reading + photometric columns
```

`.csv` chứa một hàng cho mỗi lần đọc: dấu thời gian UTC, thời gian tích hợp, công suất tổng,
lux quang giác/tối giác, PPFD (và phân tách màu xanh dương/xanh lục/đỏ), bước sóng đỉnh, sau đó là
quang phổ đầy đủ trên lưới bước sóng riêng của cảm biến. `.daq` được nhập lại mà không cần
hiệu chuẩn lần thứ hai.

`<name>` là tên tệp của bản ghi. Hai bản ghi trong các thư mục khác nhau có thể chia sẻ
một — các giá trị đo cho từng lần chụp (`<capture>/daq/daq_data_<sensor id>_<ts>.daq`) được lưu trong
cùng một mili giây bởi hai camera — và mỗi bản ghi vẫn nhận được các sản phẩm riêng của mình: bản ghi đầu tiên giữ
`<name>`, bản tiếp theo thêm tên thư mục để phân biệt **sau** tên tệp
(`<name>__<capture>_daq_calibrated.daq`), do đó ngày và giờ của bản ghi vẫn được đặt ở đầu.

Khi thành công, quá trình chạy sẽ báo cáo `Light-sensor products written: N (calibrated .daq + .csv)`.
Phần trong ngoặc đơn mô tả những gì thực sự đã được ghi, do đó nội dung sẽ là
`(RAW COUNTS — this sensor has no calibration bundle)` đối với cảm biến không có gói và
`(N calibrated, M raw counts)` đối với thư mục chứa cả hai. Các tiêu đề riêng của hệ thống nền
`[DAQ-EXPORT]` và `[RUN-SUMMARY]` cũng được đặt tên theo cách tương tự — không có
bản ghi nào trong ba bản ghi này có thể được coi là đã được hiệu chuẩn khi xuất dữ liệu thô.

Một bản ghi DAQ-U / DAQ-M / DAQ-E mà gói hiệu chuẩn không thể truy xuất — do bạn đang
ngoại tuyến, hoặc cảm biến đó không có tệp hiệu chuẩn — sẽ được **bị bỏ qua kèm lý do** trên dòng
`[DAQ-EXPORT]`, tuyệt đối không được ghi ra dưới dạng tệp “đã hiệu chuẩn” chứa số đếm thô.
Kết nối với internet và chạy lại. Lý do là lý do mà trình đọc thực sự
xác định cho tệp đó (cấu trúc không đọc được, không có gói, lỗi ghi), và bản tóm tắt
chạy liệt kê các lý do **riêng biệt** — hai mươi tệp bị bỏ qua vì một nguyên nhân sẽ được hiển thị là một
nguyên nhân, không phải hai mươi lần lặp lại của nó. CLI sẽ liệt kê từng bản ghi bị bỏ qua ở cuối
quá trình chạy, kèm theo lý do:

```
⚠ Light-sensor recording not exported: <file>.daq: no calibration bundle available for …
```

Một thư mục chứa các bản ghi mà **không** xuất được bản nào sẽ bị lỗi: xem
[Một lần chạy không ghi được hình ảnh nào sẽ bị lỗi](#a-run-that-writes-no-images-fails).

Một bản ghi có tên kết thúc bằng `_calibrated` hoặc `_raw` (`field_raw.daq`) **không
được nhập**: đó là những tên mà `process` đặt cho các sản phẩm của chính nó, và một sản phẩm được đọc lại
dưới dạng bản ghi sẽ bị tính hai lần. CLI sẽ thông báo điều này và nêu tên tệp —
`Not imported as a light-sensor recording: field_raw.daq … Rename the file to import it.`
— thay vì báo cáo một thư mục trống. Hãy đổi tên bản ghi đó trước khi xử lý.

#### Các bản ghi DAQ-A được xuất dưới dạng số đếm thô

Dòng sản phẩm **DAQ-A** ra đời trước hệ thống gói theo số sê-ri và không có gói hiệu chuẩn
để tải về — thay vào đó, nó được hiệu chuẩn tại hiện trường so với mục tiêu phản xạ, đó là
lý do tại sao nó không bao giờ cần gói hiệu chuẩn. Việc từ chối các bản ghi này khiến chúng không có cách nào để xuất
số liệu ra ngoài, do đó chúng được xuất dưới một **tên khác**:

```
<project>/
└── Light Sensor/
    ├── <name>_raw.daq        # NOT _calibrated
    └── <name>_raw.csv        # raw spectral sensor counts, NOT irradiance
```

Một tên tệp khác thay vì một cờ bên trong tệp, vì thông tin này phải được giữ nguyên
khi được gửi qua email dưới dạng tên tệp thuần túy. Tiêu đề `.csv` ghi rõ
`raw spectral sensor counts (NOT irradiance)` và cảnh báo rằng các giá trị chỉ có thể so sánh
**trong phạm vi** tệp — đây chính xác là mục đích mà hiệu chuẩn dựa trên mục tiêu sử dụng chúng — chứ
không phải giữa các cảm biến. Các cột quang độ phụ thuộc vào công suất (công suất tổng, lux quang giác và
lux tối giác, PPFD) được ghi là **NULL** thay vì được tích hợp từ số đếm, và bản tóm tắt
chạy ghi là `RAW COUNTS` nên "dữ liệu đã xuất" trong nhật ký không thể được hiểu là bức xạ.

Các bản ghi cũ **v1.01 / v1.02** (được ghi bởi DAQ-A-SD) không chứa thời điểm cho từng lần đọc,
chỉ có thời gian ghi của tệp. Công cụ khớp hình ảnh↔tia xuống vẫn từ chối các bản ghi này — việc khớp một
khung hình với thời gian ghi sẽ dẫn đến sai sót không thể phát hiện — nhưng trình xuất dữ liệu vẫn đọc được chúng, và
tệp CSV in ra `clock=daq_created_on`, do đó sản phẩm này cho biết nó đang chạy trên đồng hồ nào.

### Ghi chú

- `process` tự động phát hiện xem thư mục của bạn thuộc loại Survey3, LATTICE hay hỗn hợp.
- Tiến trình được truyền qua Server-Sent Events; giao diện dòng lệnh (CLI) hiển thị tiến trình theo từng luồng thời gian thực (Phát hiện, Phân tích, Xử lý, Xuất).
- Đối với Linux/Jetson, CLI sẽ kiểm tra bộ nhớ ảo (swap) và có thể hiển thị cảnh báo trước khi xử lý các thư mục lớn. Tính năng debayer nhận diện kết cấu cũng tự động áp dụng giới hạn tần số GPU trên các thiết bị Jetson tiết kiệm năng lượng (Nano, Orin Nano).
- Khi thành công, quá trình chạy sẽ báo cáo số lượng sản phẩm hình ảnh đã được ghi (`Image products written: N`, và `(plus M clamp masks)` khi quá trình tách kênh làm giới hạn bất kỳ pixel nào).
- **`process` chia sẻ backend đang chạy một cách an toàn.** Nó chỉ tự động khởi động lại backend đang chạy khi backend đó đang ở trạng thái nhàn rỗi với dự án trước đó đã được tải và không giữ bất kỳ camera, mảng hoặc DAQ nào. Một backend đang giữ bất kỳ thành phần nào trong số đó (GUI đang truyền dữ liệu từ mảng, kịch bản SDK, bản ghi DAQ) sẽ được sử dụng nguyên trạng, không bao giờ được khởi động lại, do đó `process` có thể chạy song song với một mảng đang hoạt động. Nếu backend báo cáo có phiên chạy đang diễn ra, `process` sẽ chờ tối đa 15 giây cho đến khi quá trình đó kết thúc (một phiên vừa hoàn tất mất khoảng một giây để xóa), sau đó từ chối với mã lỗi `Backend is mid-run (processing). Wait for it to finish, or pass --restart to force a fresh backend (this kills the in-flight run).`. Cờ toàn cục `--restart` vẫn buộc phải khởi động lại backend mới và kết thúc mọi phiên camera, mảng hoặc DAQ trên đó. Một backend được chạy dưới dạng dịch vụ systemd trên Linux sẽ không bao giờ được khởi động lại: `process` sử dụng nó, và `--restart` hiển thị thông báo `sudo systemctl restart <unit>` thay thế (xem [Chạy backend dưới dạng dịch vụ](#running-the-backend-as-a-service-headless)). Backend riêng của ứng dụng Chloros cũng vậy: khi ứng dụng đang mở, `process` sẽ sử dụng nó, tải dự án của nó thay cho dự án của ứng dụng (một backend được khởi động thay thế sẽ khóa ứng dụng đang mở khỏi nó).
- **Một phiên xử lý đang diễn ra sẽ giữ nguyên dự án của nó.** Trong khi một phiên xử lý đang diễn ra — được khởi chạy bởi ứng dụng, `process`, một tập lệnh SDK hoặc `chloros-mcp` — backend sẽ từ chối tạo, mở, nhập vào, thêm hoặc xóa tệp, di chuyển, xóa hoặc thay đổi cài đặt xử lý của dự án, với mã lỗi HTTP 409 `PROCESSING_IN_PROGRESS` và `A processing run is in progress on project '<name>', so ... now is refused`; một phiên bản `process` nhận được thông báo này sẽ thoát khỏi `1`. Cài đặt hiển thị (kích thước hình thu nhỏ) vẫn được lưu. Chế độ xử lý và cài đặt độ phản xạ mục tiêu của dự án cũng bị từ chối theo cách tương tự (một lần chạy sẽ giữ nguyên chế độ ban đầu). Một bản ghi được lưu vào dự án trong khi đang chạy (từ ứng dụng, CLI, SDK hoặc `chloros-mcp`) sẽ được lưu như bình thường, nhưng chỉ được thêm vào danh sách tệp của dự án khi phiên chạy kết thúc; thông báo phản hồi cũng xác nhận điều này (`project_add`), và điều tương tự áp dụng cho bản ghi cảm biến ánh sáng bị dừng trong dự án. Trước đây, một máy khách thứ hai có thể chuyển sang dự án đang mở giữa quá trình chạy, và phiên chạy đang diễn ra sẽ được xuất sang dự án khác.
- **Ctrl+C dừng quá trình chạy ở phía máy chủ (backend), không chỉ dừng ở giao diện dòng lệnh (CLI).** CLI yêu cầu máy chủ ngừng xử lý, hiển thị thông báo `Waiting for the backend to stop (Ctrl+C again to stop waiting)...` và chờ tối đa 120 giây cho đến khi nhận được `Processing stopped.`, do đó `chloros-cli process … && next` sẽ không khởi chạy `next` trên backend vẫn đang bận với quá trình chạy. Lần nhấn Ctrl+C thứ hai sẽ kết thúc thời gian chờ. `process` sau đó sẽ thoát khỏi `130`, giống như bất kỳ lệnh nào bị gián đoạn.

#### Một lần chạy không ghi hình ảnh nào sẽ thất bại

Nếu bạn yêu cầu các sản phẩm và quá trình chạy không ghi **bất kỳ** — chỉ có `project.json` và
`calibration_data.json` — thì `process` coi đó là một lỗi: nó sẽ in ra
`Processing finished but wrote no image products.` và **thoát với giá trị không bằng 0**, do đó tập lệnh có thể
phát hiện được. Thông báo này nêu tên thư mục dự án và các nguyên nhân thường gặp:

- thư mục đầu vào không được nhận diện là dữ liệu thu thập (kiểm tra bố cục và `--input-level`), hoặc
- mọi sản phẩm được yêu cầu đều bị bỏ qua vì không áp dụng được cho các camera đó (ví dụ: yêu cầu
  độ rạng rỡ/độ phản xạ từ các camera chỉ hỗ trợ RGB).

Chạy lại với `--verbose` và kiểm tra nhật ký backend để tìm các dòng `[LATTICE-EXPORT]` / `[EXPORT-CHECK]`,
giải thích các trường hợp bỏ qua theo từng camera mà không hiển thị trong kết quả đầu ra của CLI.

Một lần chạy chỉ tập trung vào metadata — tắt tất cả các tùy chọn sản phẩm và không sử dụng `--indices` — vẫn được coi là
**thành công**, vì đầu ra hình ảnh trống chính là kết quả đúng trong trường hợp này.

#### Một lần chạy mà các tệp xuất không thể được xác minh sẽ thất bại

Sau khi chạy, `process` sẽ kiểm tra các thẻ của các tệp đã xuất bằng ExifTool. Nếu ExifTool không thể chạy, nó
sẽ hiển thị `Xác minh xuất dữ liệu THẤT BẠI: ExifTool không thể chạy, do đó các thẻ của N trong số M tệp đã xuất
chưa được kiểm tra (xem các dòng [EXIFTOOL] trong nhật ký backend)` and `Các sản phẩm đã được ghi và vẫn
nằm nguyên tại chỗ; hãy chạy lại để xác minh chúng.`, does not print `Quá trình xử lý hoàn tất!`, và **thoát với giá trị không bằng 0**. Các
sản phẩm vẫn được giữ lại.

#### Các ảnh chụp được tạo trong quá trình chạy

Các ảnh chụp hoàn thành trước khi `process` được chạy sẽ được bao gồm. Các ảnh chụp được lưu trong quá trình chạy sẽ không được bao gồm; `process`
sẽ cảnh báo số lượng ảnh bị bỏ qua, và việc chạy lại công cụ sẽ bao gồm chúng.

#### Một lần chạy bị mất hình ảnh sẽ thất bại

Nếu một hình ảnh bị lỗi trong quá trình chạy, `process` sẽ hiển thị thông báo `N image(s) failed and were not exported` kèm theo
nguyên nhân phổ biến nhất, không hiển thị thông báo `Processing complete!`, và **thoát với giá trị không bằng 0**. Các sản phẩm khác vẫn
được ghi và giữ nguyên tại chỗ, và nhật ký phía sau ghi lại từng lỗi.

Điều này cũng áp dụng cho **lần chạy chỉ sử dụng cảm biến ánh sáng**: một thư mục chứa các bản ghi `.daq` sẽ không có hình ảnh để xuất
theo định nghĩa, và lần chạy sẽ được đánh giá dựa trên các tệp `.daq` / `.csv` đã hiệu chuẩn mà nó ghi ra thay thế.
Nếu nó không ghi **bất kỳ tệp nào** — mọi bản ghi đều bị bỏ qua — nó sẽ hiển thị
`Processing finished but wrote no light-sensor products.`, đặt tên cho từng bản ghi kèm theo
lý do bị bỏ qua, và **thoát với giá trị không bằng 0**.

---

## `chloros-cli login`

Xác thực máy này bằng tài khoản đám mây Chloros+. Thông tin đăng nhập được lưu trữ an toàn trong `~/.chloros/user_session.json`.

```
chloros-cli login EMAIL PASSWORD
```

### Ví dụ

```bash
chloros-cli login user@example.com 'YourPassword'

# Passwords containing $ should use SINGLE quotes
chloros-cli login user@example.com 'my$ecret$pass'
```

> **PowerShell `$$` mangling is auto-corrected.** In double quotes PowerShell expands `$$` (loại bỏ hoặc sao chép một phần mật khẩu). Khi gặp lỗi 401, giao diện dòng lệnh (CLI) sẽ tự động thử lại với `$$` được- sau đó với nửa mật khẩu đã loại bỏ trùng lặp; nếu lần thử lại thành công, hệ thống sẽ đăng nhập cho bạn và hiển thị cú pháp dấu nháy đơn chính xác để sử dụng lần sau.

> **Sử dụng ở chế độ không giao diện người dùng/tự động hóa: không có phiên làm việc được lưu trong bộ nhớ đệm có nghĩa là sẽ xuất hiện lời nhắc đăng nhập.** Các lệnh cốt lõi (`process`, `status`, `export-status`, `time-sync`, …) được chạy mà không có phiên làm việc được lưu trong bộ nhớ đệm sẽ yêu cầu nhập `Email:` / `Password:` qua stdin trước khi tiếp tục. Khi stdin bị đóng (cron, một đơn vị systemd), chương trình sẽ thất bại ngay lập tức với lỗi `login cancelled`; khi stdin là một thiết bị đầu cuối mở hoặc ống dẫn, chương trình sẽ chờ nhận đầu vào. Hãy chạy `chloros-cli login EMAIL PASSWORD` một lần trên mỗi máy trước khi lên lịch công việc không cần giao diện người dùng.

---

## `chloros-cli logout`

Xóa phiên làm việc được lưu trong bộ nhớ đệm và buộc phải đăng nhập lại trong lần gọi tiếp theo.

Việc đăng xuất trong khi quá trình xử lý đang diễn ra sẽ không dừng quá trình đó: quá trình sẽ kết thúc ở chế độ xử lý mà nó đã bắt đầu, và lần chạy tiếp theo sẽ sử dụng chế độ xử lý tiêu chuẩn. `logout` đã nêu rõ điều này (`The processing run in progress finishes in <mode> mode; the next run uses standard`), và ứng dụng cũng thông báo tương tự khi bạn đăng tại đó.

```bash
chloros-cli logout
```

---

## `chloros-cli status`

Hiển thị cấp độ giấy phép hiện tại (Iron/Copper/Bronze/Silver/Gold), người dùng đã xác thực và số lần liên kết thiết bị.

```bash
chloros-cli status
```

---

## `chloros-cli export-status`

Kiểm tra tiến trình xuất Thread-4 đang diễn ra. Có thể gọi lệnh này một cách an toàn **trong khi** lệnh `process` đang chạy từ một cửa sổ lệnh khác.

```bash
chloros-cli export-status
```

---

## `chloros-cli language`

Đặt ngôn ngữ hiển thị của giao diện dòng lệnh (CLI) (hỗ trợ 38 ngôn ngữ, bao gồm CJK, RTL và các ngôn ngữ Ấn Độ). Tự động chuyển sang tiếng Anh một cách mượt mà trên các giao diện dòng lệnh cũ không thể hiển thị kịch bản.

```
chloros-cli language [LANG_CODE] [--list]
```

### Ví dụ

```bash
# List all available languages
chloros-cli language --list

# Switch to Spanish
chloros-cli language es

# Show the currently-active language
chloros-cli language
```

---

## Lệnh liên quan đến thư mục dự án

Các lệnh này quản lý vị trí thư mục dự án mặc định (được chia sẻ với giao diện đồ họa).

```bash
chloros-cli set-project-folder "/home/user/Chloros Projects"
chloros-cli get-project-folder
chloros-cli reset-project-folder
```

`get-project-folder` hiển thị thư mục mà các dự án mới sẽ được lưu vào. Nếu thư mục đã cấu hình không còn tồn tại (đã bị xóa, đổi tên hoặc nằm trên ổ đĩa không được kết nối), CLI sẽ chuyển sang sử dụng thư mục mặc định, giống như giao diện đồ họa (GUI): thư mục mặc định sẽ được hiển thị trên stdout, một thông báo cảnh báo nêu tên thư mục bị thiếu sẽ được ghi vào stderr, và mã thoát là `0`.

---

## `chloros-cli update`

Chỉ dành cho Linux/Jetson. Kiểm tra phiên bản `version_url` so với `/etc/chloros/update.conf` và, khi có phiên bản mới hơn, hệ thống sẽ tải xuống phiên bản tương ứng `.deb` và cài đặt ngay lập tức bằng lệnh `sudo dpkg -i` (không có lời nhắc xác nhận; lệnh `sudo` có thể yêu cầu nhập mật khẩu). Trên Jetson, chỉ hệ thống JetPack 6 mới được cung cấp bản cập nhật. Trên Windows, lệnh này không thực hiện gì cả và báo cáo `up to date`: cập nhật bằng trình cài đặt mới.

```bash
chloros-cli update            # check + install
chloros-cli update --check    # check only
```

Trên Linux/Jetson, CLI cũng **tự động kiểm tra cập nhật mỗi khi khởi động** (không chặn, không bao giờ làm chậm lệnh): nó đọc `/etc/chloros/update.conf`, lưu kết quả vào bộ nhớ đệm trong 1 giờ tại `~/.chloros/update_cache.json`, và hiển thị `Update available: vX.Y.Z / Run: chloros-cli update` khi có phiên bản mới hơn. Tự động bỏ qua mà không báo lỗi trong trường hợp có lỗi hoặc trên hệ điều hành Windows; `CHLOROS_DISABLE_UPDATE_CHECK=1` sẽ tắt tính năng này. Do bộ nhớ đệm 1 giờ, một bản phát hành được công bố trong vòng 1 giờ đó vẫn có thể được đọc là đã cập nhật.

---

## `chloros-cli selftest`

Thực hiện kiểm tra nhanh gồm 7 bước: phiên bản, tính khả dụng của cổng, khởi động backend, `/api/test`, `/api/system-info` (GPU/CUDA/PyTorch), sự hiện diện của mô hình khử nhiễu, sự sẵn sàng của CUDA + bộ lọc nhiễu. Công cụ này sử dụng một backend đang chạy, hoặc khởi động một backend để kiểm tra và dừng nó sau đó. Nó hiển thị `N/7 checks passed` và các kiểm tra không thành công, và chỉ thoát khỏi `0` khi cả bảy bước đều thành công; kiểm tra CUDA+bộ lọc nhiễu sẽ thất bại trên máy tính không có GPU CUDA (xem [Xác minh cài đặt](#verify-the-install)).

```bash
chloros-cli selftest
```

---

## `chloros-cli time-sync`

Trạng thái và kiểm soát grandmaster PTP. Máy chủ Chloros chạy grandmaster PTP; các camera LATTICE và các đơn vị DAQ-E hoạt động như thiết bị phụ thuộc vào nó để đồng bộ dấu thời gian giữa các thiết bị (khoảng 1 ms giữa các thiết bị). PTP đồng bộ hóa **dấu thời gian**: các camera trong mảng chụp ảnh đồng bộ vì tín hiệu kích hoạt phần cứng M8 kích hoạt chúng (khoảng cách dưới 100 µs giữa các mô-đun), chứ không phải do PTP. `chloros-cli time-sync` khi không có lệnh con sẽ chạy lệnh `status`.

| Lệnh con | Mô tả |
| --- | --- |
| `status` | Hiển thị trạng thái grandmaster, mức ưu tiên BMCA, danh tính đồng hồ và liệu các gói tin của nó có đến được mạng hay không: `health` là `OK` (`STOPPED` khi grandmaster không hoạt động), hoặc `SENDS_FAILING` / `DEGRADED` / `NO_SOCKETS` / `PEERS_SILENT` kèm theo một dòng `health_detail`, và sau đó một dòng `WARNING:` được in ra stderr (stdout vẫn là JSON). `sync_sent` / `announce_sent` chỉ đếm các tin nhắn đã được gửi đi; `*_send_errors` và `last_send_error` đếm những tin nhắn chưa được gửi đi. Sau khi kết nối camera bị ngắt và được khôi phục, grandmaster sẽ tự động xây dựng lại các socket của mình (`rebinds`, `last_rebind`). |
| `peers` | Danh sách các thiết bị nô lệ được phát hiện qua Delay_Req (camera + cảm biến DAQ-E). |
| `cameras` | Trạng thái PTP của từng camera (`PtpStatus`, `PtpOffsetFromMaster`, `PtpMeanPathDelay`). |
| `restart` | Khởi động lại quá trình grandmaster. |
| `set-priority --priority1 N --priority2 N` | Ghi đè các mức ưu tiên BMCA. |

Grandmaster thông báo trên các NIC của camera (bất kỳ bộ điều hợp nào có địa chỉ `169.254.x.x` được bao gồm). Nếu không có bộ điều hợp nào hoạt động, nó sẽ chuyển sang sử dụng bộ điều hợp của tuyến mặc định chỉ khi đó là kết nối có dây, tuyệt đối không phải Wi-Fi hoặc bộ điều hợp ảo (Tailscale, WSL), vì không có camera hoặc DAQ-E nào nằm phía sau bộ điều hợp đó. Khi không có gì để thông báo, hệ thống sẽ chạy mà không sử dụng socket (`health` `NO_SOCKETS`), và khi camera hoặc DAQ tiếp theo kết nối, hệ thống sẽ tái thiết lập trên NIC đang hoạt động vào thời điểm đó; `time-sync restart` thực hiện điều này ngay lập tức.

Grandmaster chạy trong một tiến trình riêng, bên cạnh backend (`own_process` / `process_pid` trong `time-sync status`), với mức ưu tiên cao trên Windows (`process_priority`), do đó một backend đang bận (một chu trình xử lý, một lần xuất dữ liệu, nhiều lệnh cùng lúc) cũng không thể làm chậm thời gian mà nó ghi lại; trước đây, một sự chậm trễ như vậy có thể làm thay đổi đồng hồ của tất cả các camera cùng lúc. Quy trình này kết thúc cùng với backend; nếu không thể khởi động, grandmaster sẽ chạy bên trong backend như trước đây (`own_process` false, và nhật ký backend ghi lại điều này).

### Ví dụ

```bash
chloros-cli time-sync status
chloros-cli time-sync peers
chloros-cli time-sync cameras
chloros-cli time-sync restart
chloros-cli time-sync set-priority --priority1 1 --priority2 1
```

---

## `chloros-cli lattice`

Điều khiển camera LATTICE. Mỗi lệnh con đều được định tuyến qua backend Chloros; backend quản lý nhóm camera (cam pool) nên các lệnh CLI tiếp theo sẽ tái sử dụng cùng một handle đã mở.

### Các tùy chọn chung (được chia sẻ bởi hầu hết các lệnh con)

| Cờ | Mô tả |
| --- | --- |
| `-d, --device N` | Chỉ số camera (mặc định: 0). |
| `-s, --serial SN` | Số sê-ri cụ thể; ghi đè lên `--device`. |
| `--serials SN1,SN2,…` | Các số sê-ri được phân tách bằng dấu phẩy cho hoạt động đa camera. Các [lệnh tạm thời cho nhiều camera](#multi-camera-transient-sessions) chỉ mở các camera này, trong đó camera đầu tiên sẽ kích hoạt cơ chế kích hoạt. |
| `--all` | Hoạt động trên mọi camera được phát hiện. |
| `--exposure US` | Thời gian phơi sáng tính bằng microgiây. |
| `--gain DB` | Độ khuếch đại (Gain) tính bằng dB. |
| `--pixel-format FMT` | Ví dụ: `BayerRG8`, `BayerRG12`; `Mono8`, `Mono12` trên M3M. Tên này xác định độ sâu bit trong dòng sản phẩm của chính máy ảnh: trên M3M, tên Bayer xác định định dạng Mono của độ sâu đó (`BayerRG8` xác định `Mono8`), trên M3C, tên Mono xác định tên Bayer (`Mono12` xác định `BayerRG12`). |
| `--width N` / `--height N` | Kích thước hình ảnh. |
| `--preset {default,high_quality,high_speed,triggered}` | Áp dụng cài đặt sẵn: định dạng pixel, độ phơi sáng, độ khuếch đại và tốc độ khung hình (`high_speed`: `BayerRG8`, 5 ms, 6 dB, 60 fps; `high_quality`: `BayerRG12`, 20 ms, 0 dB; `lattice presets` liệt kê các giá trị này). Trên M3M, định dạng là Mono có cùng độ sâu: `high_speed` ghi lại `Mono8`, `high_quality` và `triggered` `Mono12`. Một cờ được cung cấp kèm theo (`--exposure`, `--gain`, `--pixel-format`) sẽ được ưu tiên hơn cài đặt sẵn, và các thiết lập không được đề cập trong cài đặt sẵn sẽ giữ nguyên giá trị mặc định của máy ảnh. Tất cả các chế độ chạy tự dotrừ `triggered`, tùy chọn này kích hoạt camera khi phát hiện tín hiệu biên phần cứng trên Dòng 2 — nếu không có tín hiệu nào tác động lên dòng đó, camera sẽ chờ mãi mãi thay vì ghi hình. |
| `-o, --output DIR` | Thư mục đầu ra (mặc định: `output`). |
| `--packet-size {auto,jumbo,standard,N}` | Kích thước gói tin GVSP. `auto` thực thi các lệnh kiểm tra ICMP+GVSP; `jumbo` = 9000; `standard` = 1500. |

### Quy trình kết nối lần đầu của camera LATTICE

```bash
# 0. A backend must be running: the Chloros desktop app (Windows),
#    `chloros-backend`, or the Linux service (see "Start the Backend")

# 1. Discover cameras on the network
chloros-cli lattice info

# 2. Single-cam smoke test: capture one frame.
#    By default this saves EVERY export type applicable to the cam
#    (raw, debayered, radiance, reflectance, preview). Pass e.g.
#    `--processing debayered` to save just one.
chloros-cli lattice capture -o output/

# 3. Connect a synchronized array (RECOMMENDED ENTRY POINT for arrays).
#    This is the same "smart-prep" flow the Chloros GUI uses:
#      - Network capability probe (ICMP DF ping + GVSP probe)
#      - Tier auto-pick (sim-emit / ftd-stagger / slip)
#      - Full sensor (no binning, no ROI); the frame rate falls to
#        what the host and wire deliver, and the faster options are
#        printed
#      - PTP enabled by default
#      - Per-cam pixel format auto-pick
#      - AE seeding from the cam's saved state
#      - GPIO trigger config on Line2
chloros-cli lattice array-connect \
  --serials 213800234,214000533,214701288,214701292

# 4. Capture one synced frame group from the live array.
#    Defaults to --processing all (one file per export type per cam);
#    pass a single level to narrow it, e.g. --processing reflectance.
chloros-cli lattice array-capture --processing reflectance -o output/

# 5. Live-preview one cam in your browser
chloros-cli lattice viewer --serial 213800234

# 6. Tear down when done
chloros-cli lattice array-disconnect
```

### Tham chiếu các lệnh con

#### Khám phá & Thông tin

| Lệnh con | Mục đích |
| --- | --- |
| `lattice info` | Liệt kê các camera đã kết nối (nhà sản xuất, mẫu, số sê-ri, IP, MAC). |
| `lattice probe [--pixel-format FMT] [--json] [--no-discover]` | Phân tích hệ thống máy chủ để tối ưu cấu hình camera. `--no-discover` bỏ qua quá trình phát hiện camera (nhanh hơn, chỉ phân tích NIC). |
| `lattice network [--fix] [--estimate] [--cameras N] [--pixel-format FMT]` | Kiểm tra/sửa cài đặt NIC (trên Linux, bao gồm cả bộ đệm nhận socket của kernel — xem [máy chủ Linux](#linux-hosts--socket-receive-buffers)); ước tính băng thông/FPS. |
| `lattice network-analysis --master SN [--slaves SN1,SN2,…] [--width N] [--height N] [--pixel-format FMT] [--binning {1,2,4}] [--force-tier TIER] [--packed {auto,on,off}] [--backend-url URL] [--json]` | Khả năng mạng backend theo lược đồ ổn định+ đề xuất mảng (trả về `status` ∈ `ok` / `auto_shrunk` / `auto_capped_fps` / `needs_force_slip` / `error`). `--width` / `--height` mặc định là cảm biến đầy đủ, `2048` × `1536`. Chương trình thoát khỏi `0` cho `ok`, `auto_capped_fps` và `auto_shrunk`, và chuyển từ `1` sang `needs_force_slip` và `error`, có hoặc không có `--json`. Nó sẽ đến backend tại `--backend-url` (mặc định là `http://127.0.0.1:5000`), chứ không phải `CHLOROS_BACKEND_URL`. `auto_capped_fps` giữ nguyên độ phân giải được yêu cầu nhưng giới hạn tốc độ khung hình mục tiêu — đọc giá trị tại `recommended.recommended_target_fps` và truyền nó làm mục tiêu kết nối; coi đó là thành công, chứ không phải là lỗi. `auto_shrunk` chỉ xuất hiện khi khung hình được yêu cầu không thể được truyền ở bất kỳ tốc độ khung hình nào; connect không bao giờ tự động thu nhỏ khung hình. |
| `lattice analyze-array [--models M1,M2,…] [--binning {1,2,4}] [--n-active N] [--width N] [--height N] [--pixel-format FMT] [--force-tier TIER] [--packed {auto,on,off}] [--json]` | Phân tích giả định mà không mở camera (`--width` / `--height` mặc định là `2048` × `1536`). Cả hai tùy chọn này và `network-analysis` đều dự đoán nội dung mà connect sẽ phát trực tuyến: nếu không có `--pixel-format`, các định dạng cho từng camera mà kết nối áp dụng (Mono12 / BayerRG12), và với `--packed auto` (mặc định), quyết định mà kết nối sẽ đưa ra trên máy tính này là [chuyển tải 12-bit đóng gói](#packed-12-bit-transport---packed) mà Connect sẽ áp dụng trên máy tính này — do đó, fps được nêu là tốc độ mà mảng camera hoạt động. Cả hai đều hiển thị quyết định đóng gói. **`--n-active` là tổng số camera trên đường truyền, không chỉ riêng mảng camera này**— hãy tăng giá trị này khi các camera độc lập truyền dữ liệu đồng thời, hoặc khi ngân sách đường truyền được tính toán dựa trên nhu cầu không tính đủ số lượng camera (mặc định: `len(--models)`). Luôn hiển thị các dòng tổng hợp `Wire budget:` (dung lượng yêu cầu (MB/s) so với giới hạn an toàn va chạm) và `Max cameras:`, đồng thời đánh dấu cảnh báo `** OVER-SUBSCRIBED**` khi mảng sử dụng quá mức băng thông — xem [Mô hình fps & burst của mảng](#array-fps--burst-model). |
| `lattice gpu` | Hiển thị trạng thái GPU. |
| `lattice firmware [--update] [--force] [-y\|--yes]` | Kiểm tra hoặc cập nhật phần mềm camera. Lựa chọn cục bộ `.fwa` bị khóa: tệp trong `firmware/<MODEL_PREFIX>/` khớp với `MIN_FIRMWARE_VERSION` của bản dựng sẽ được nạp khi có sẵn (chỉ phiên bản cao nhất được dùng làm phương án dự phòng), do đó hình ảnh nhà cung cấp mới hơn được lưu trữ trên đĩa sẽ không hoạt động cho đến khi ghim đó được gỡ bỏ — các bản phát hành mới hơn sẽ được phân phối đến các thiết bị thông qua bản kê khai AWS đã ký, phương thức này được ưu tiên khi có phiên bản mới hơn. |
| `lattice presets [--apply NAME]` | Hiển thị danh sách hoặc áp dụng các cài đặt sẵn của camera. `--apply` xác định các thông số của cài đặt sẵn (định dạng pixel, độ phơi sáng, độ khuếch đại, tốc độ khung hình, tín hiệu kích hoạt) và hiển thị các thiết lập này theo cách camera đọc chúng sau khi: trên M3M, định dạng Mono của độ sâu cài đặt sẵn; tốc độ khung hình mà nó chạy, có thể thấp hơn mức yêu cầu của cài đặt sẵn (`high_speed` yêu cầu 60 fps; một camera toàn khung hình trên kết nối 1 Gbps chạy khoảng 35). Mọi cài đặt khác mà tệp mở đã ghi sẽ được khôi phục về trạng thái ban đầu. `--apply default` đặt lại máy ảnh về cài đặt mặc định nhà máy: các cài đặt mặc định với chế độ phơi sáng tự động chạy tự do trên Line2, giới hạn băng thông kết nối và tính năng chỉnh sửa lỗi bị tắt, cân bằng trắng đã hiệu chuẩn của máy ảnh RGB được giữ nguyên (giá trị mặc định trên các loại khác). Không có dữ liệu nào được lưu vào bộ nhớ của máy ảnh, do đó khi khởi động lại lần tiếp theo, máy ảnh sẽ khởi động từ bộ nhớ của chính nó như trước đây. Khi thực hiện lệnh ghi hình hoặc kết nối với `--preset`, `default` không phải là cài đặt sẵn. |
| `lattice status` | Trạng thái camera trực tiếp. |

#### Ghi hình

| Lệnh con | Mục đích |
| --- | --- |
| `lattice capture [--format tiff\|tif\|png\|jpg\|jpeg] [--jpeg-quality N] [--processing LEVEL] [--levels L1,L2,…] [--no-daq] [--daq-tolerance-ms MS]` | Khung hình đơn. **Lưu mọi định dạng xuất theo mặc định**(`--processing all`); xem [Các mức độ xuất khi ghi hình](#capture-export-levels-the-all-default). `--processing` chấp nhận các lệnh con `all`, `raw`, `debayered`, `radiance`, `reflectance` hoặc `display` (`preview` được chấp nhận cho `display`); bất kỳ tên nào khác sẽ bị từ chối trước khi chạm vào máy ảnh (thoát khỏi `1`). `--levels` lưu một tập con rõ ràng (ghi đè lên `--processing`). Một camera mà Chloros không đang giữ sẽ được mở để ghi hình, và thời gian phơi sáng của nó được thiết lập dựa trên cảnh ban đầu (tối đa vài giây), giống như các [lệnh tạm thời](#multi-camera-transient-sessions); khi quá trình chụp kết thúc, bất kể kết thúc như thế nào, camera đó sẽ được khôi phục về trạng thái ban đầu (định dạng, thời gian phơi sáng, quy trình xử lý màu, mọi cài đặt đã thay đổi trong quá trình chụp — danh sách được liệt kê trong [Multi-Camera (Transient Sessions)](#multi-camera-transient-sessions)). Dữ liệu DAQ được sử dụng để ghi lại cảnh quay sẽ được lưu dưới dạng tệp sidecar `.daq`, bao gồm một bản ghi thô; `--no-daq` / `--daq-tolerance-ms` điều chỉnh nó ([Bộ DAQ đọc dữ liệu ghi lại sẽ lưu lại](#the-daq-reading-a-capture-saves)). `--force-daq` vẫn được chấp nhận. `--jpeg-quality` = chất lượng JPEG từ 1–100 (mặc định 95).**Các tệp được lưu vào dự án đang mở sẽ được thêm vào dự án đó.** Giống như trong ứng dụng, một bản ghi mà các tệp của nó được lưu vào thư mục của dự án đang mở trong Chloros sẽ được thêm vào danh sách tệp của dự án đó; các tệp được lưu ở nơi khác sẽ không bị ảnh hưởng. Lệnh này sẽ hiển thị một dòng thông báo như vậy (`Project: ...`), hoặc, trong khi quá trình xử lý đang diễn ra, thông báo rằng các tệp đó sẽ được giữ lại và được thêm vào khi quá trình kết thúc. Các mã lệnh `multi-capture`, `array-capture` (cũng như `--smart`) và `project capture`. |
| `lattice continuous [--format tiff\|tif\|png\|jpg\|jpeg] [--jpeg-quality N] [--queue-depth N] [--no-daq] [--daq-tolerance-ms MS]` | Truyền dữ liệu ra đĩa cho đến khi nhấn Ctrl+C. Các khung hình của một lần chạy `tiff`là các khung hình của tệp `raw` — một mảng ghi lại dữ liệu từ camera đó: bản ghép cảm biến, mỗi mặt phẳng có giá trị riêng, không nén, kèm gói hiệu chuẩn (một khung hình 2048 x 1536 12-bit có kích thước 6,0 MiB cộng với gói dữ liệu, bất kể cảnh nào), do đó `process` bắt đầu từ các khung hình này. `png` và `jpg` chứa 8 bit, do đó các chuỗi này là các khung hình đã được khử mô-saic để hiển thị, mà `process` không bắt đầu từ đó. Với chế độ DAQ bị giới hạn (`daq pool-bind`) hoặc được gộp chung, mỗi khung hình sẽ có các tệp `.daq` và `.match.json` riêng trong thư mục `daq/` của chuỗi dữ liệu, vì `lattice capture` lưu một khung hình; `--no-daq` tắt tính năng đó, và lệnh dừng sẽ hiển thị số lượng khung hình. Mỗi khung hình được ghi một lần, kèm theo metadata đầy đủ; khi nhấn Ctrl+C, các khung hình đã được chụp sẽ được ghi lại trước khi máy ảnh được thả ra. Tự động dừng khi ổ đĩa đầy, in ra `Failed:` / `Disk full:` và thoát khỏi `1`; bất kỳ khung hình nào không thể ghi cũng sẽ thoát khỏi `1`. Máy ảnh được để ở chế độ sẵn sàng cho tín hiệu kích hoạt phần cứng (`TriggerMode` `On`) sẽ được đặt ở chế độ chạy tự do (free-run) trong suốt quá trình chạy. Khi quá trình chạy kết thúc, máy ảnh sẽ được khôi phục về trạng thái ban đầu — bao gồm các cài đặt kích hoạt và mọi cài đặt đã được thay đổi trong quá trình chạy (thời gian phơi sáng, độ khuếch đại, định dạng, …) — trừ khi gặp lỗi `--keep-settings`, trong trường hợp này các cài đặt của đợt chạy sẽ được lưu vào bộ nhớ riêng của máy ảnh (chế độ chạy tự do), hoặc lỗi `--clear-settings`, khi đó bộ nhớ đó sẽ được khôi phục về cài đặt gốc. Một lần chạy không ghi được khung hình nào được coi là thất bại: nếu không có khung hình nào trong 10 giây, nó sẽ tự động dừng lại, in ra mã lỗi `No frames:` kèm lý do và thoát khỏi chương trình với mã `1`. Các khung hình mà bộ ghi không kịp xử lý sẽ được tính là bị bỏ qua, và khi dừng, hệ thống sẽ hiển thị mã lỗi `Dropped:` kèm lý do (tốc độ khung hình của camera, của bộ ghi và của hàng đợi); nếu tốc độ khung hình thấp hơn hoặc định dạng pixel 8-bit, tất cả các khung hình đó sẽ được giữ lại. Lệnh này kiểm tra trạng thái với hệ thống nền cứ sau 2 giây; nếu nó ngừng hoạt động mà không dừng ghi hình (`kill -9`, hết bộ nhớ), hệ thống nền sẽ dừng quá trình ghi hình 30 giây sau lần kiểm tra cuối cùng và đưa camera trở lại (`stop_reason` `client_gone`, và một dòng trong nhật ký máy chủ). `multi-stream` và `align-stream` cũng thực hiện tương tự. Một lần khởi động mà camera mất thời gian mở lâu hơn thời gian chờ của lệnh (60 giây; 180 giây đối với `multi-stream` / `align-stream`) — camera vẫn đang khởi động — sẽ bị hệ thống backend hủy bỏ: camera sẽ được đưa trở lại trạng thái ban đầu và đóng lại, đồng thời không còn bản ghi nào đang chạy (mã trạng thái HTTP 504 `CLIENT_DEADLINE_PASSED`). |
| `lattice viewer [--brightness N] [--ae-damping F] [--frame-rate FPS]` | Xem trước MJPEG trực tiếp trên trình duyệt: kết nối với camera, hiển thị URL luồng video và mở nó trong trình duyệt của bạn. `--ae-damping` thiết lập độ giảm phơi sáng tự động (0,4–100). |

##### Dòng video trực tiếp (MJPEG; không hỗ trợ RTSP)

Chloros cung cấp video trực tiếp dưới dạng **MJPEG qua HTTP**(`multipart/x-mixed-replace`, giới hạn `frame`) từ máy chủ phía sau.**RTSP không được hỗ trợ.** Mở URL trong trình duyệt hoặc đọc bằng bất kỳ phần mềm khách MJPEG nào (`cv2.VideoCapture(url)` của OpenCV, ffmpeg, VLC). Camera phải được kết nối với máy chủ trước tiên (`lattice cam-connect`, `lattice array-connect`, `lattice viewer` hoặc giao diện người dùng).

| URL (`http://127.0.0.1:5000` + …) | Dòng dữ liệu |
| --- | --- |
| `/api/camera/stream/<serial>` | Hình xem trước, tối đa khoảng 30 khung hình/giây. Đối với một phần tử trong mảng, `?max_h=` (mặc định là `720`) và `?jpeg_q=` (mặc định là `80`) dùng để thiết lập chiều cao và chất lượng JPEG. Đây là URL mà `lattice viewer` mở ra. |
| `/api/camera/<serial>/stream-annotated` | Cùng bản xem trước với các lớp phủ của giao diện người dùng (GUI) được vẽ vào: `zebra=1`, `crosshair=1`, `grid=3x3` (hoặc `9x9`), `histogram=1`, `peaking=1`, `spot=1`; nếu không có yêu cầu, chương trình sẽ sử dụng các thiết lập lớp phủ đã lưu của dự án cho camera đó. |
| `/api/camera/<serial>/stream-reflectance` | Độ phản xạ 8-bit trên mỗi dải: ρ = pixel ÷ phần`X-Chloros-Pixel-Scale` (255 trừ khi `refl_max` được kích hoạt). `fps` (mặc định `10`), `jpeg_q` (mặc định là `95`), `max_h` (mặc định là `0` = định dạng gốc), `band=<name>`, `align=1`. Định dạng JPEG, do đó có mất dữ liệu. Bị từ chối với mã trạng thái HTTP 409 khi camera không có giá trị phản xạ (camera RGB, không có gói hiệu chuẩn, không có dữ liệu đọc từ DAQ). |

- **Khớp các khung hình giữa các camera.**Mỗi phần mà một**thành viên mảng** gửi đi, trên cả ba luồng, đều mang theo `X-Chloros-Group-Id` (bộ đếm nhóm đồng bộ của mảng), `X-Chloros-Frame-Id` và `X-Chloros-Timestamp-Ns`. Các phần từ các camera khác nhau có cùng `X-Chloros-Group-Id` đều xuất phát từ cùng một tín hiệu kích hoạt. Một camera độc lập không có nhóm và không gửi ID nhóm. Trình duyệt bỏ qua các tiêu đề của từng phần; một máy khách tự phân tích luồng đa phần sẽ đọc chúng.
- **Tạm dừng khi chế độ xem mảng đang mở.** `/api/camera/stream/<serial>` của một thành viên trong mảng sẽ gửi khung hình tĩnh "paused" khi luồng trực tiếp kết hợp của mảng đang mở (ví dụ: chế độ xem mảng trên GUI), và tiếp tục khi chế độ xem này đóng lại.

#### Điều chỉnh cảm biến

| Lệnh con | Mục đích |
| --- | --- |
| `lattice configure [{--get N1 N2… \| --set N=V N=V… \| --dump}] [--json]` | Đọc/ghi bất kỳ nút GenICam nào. `--set ExposureTime=…` (`Gain=…`) mà không có các công tắc `ExposureAuto` (`GainAuto`) sẽ tắt chế độ tự động trước, như `lattice exposure --set` thực hiện; nếu camera từ chối giá trị nào đó, chương trình sẽ in `Failed to set <node>: …` lên stderr và thoát khỏi `1`. `--get` và chế độ xem nhóm mặc định sẽ hiển thị thông báo `note:` dưới `ExposureTime` / `Gain` trong khi chế độ tự động đang chạy — xem [Đọc thông số phơi sáng và độ khuếch đại trong chế độ tự động](#reading-exposure-and-gain-under-auto-mode). |
| `lattice exposure [{--auto \| --auto-once \| --off \| --set US}] [--brightness 0-255] [--brightness-auto] [--damping 0.4-100] [--upper-limit US]` | Độ phơi sáng & AE. Một giá trị độ phơi sáng `--set` hoặc mục tiêu `--brightness` sẽ được giữ làm của bạn (hệ thống điều khiển phơi sáng tự động của Chloros sẽ không can thiệp); độ khuếch đại vẫn giữ nguyên, do đó ở chế độ tự động, hệ thống sẽ tiếp tục điều chỉnh hướng tới mục tiêu cho đến khi bạn thiết lập `lattice gain`. `--auto` trả lại quyền điều khiển phơi sáng, `--brightness-auto` trả lại mục tiêu về chế độ tự động của Chloros. Nếu không có cờ nào được đặt, hệ thống sẽ đọc các điểm phơi sáng, với cùng giá trị `note:` như `configure --get`. Nếu máy ảnh từ chối giá trị `--set`, nó sẽ in `Failed to set ExposureTime: ExposureTime=<value> refused (<camera error>); the camera accepts <min>..<max> now (ExposureAuto put back to <mode>)` lên stderr — chế độ tự động mà lệnh ghi đã tắt sẽ được khôi phục thay vì để ở trạng thái Tắt — không in dòng `Exposure: … us (auto OFF)` nào, và thoát khỏi `1`. |
| `lattice gain [{--auto \| --off \| --set DB}]` | Độ khuếch đại & tự động điều chỉnh độ khuếch đại. Giá trị `--set` khiến camera từ chối in `Failed to set Gain: …` trên stderr, không có dòng `Gain: … dB (auto OFF)`, và thoát khỏi `1`. |
| `lattice resolution [--set WxH] [--offset X,Y] [--binning N]` | Vùng quan tâm (ROI) của cảm biến và gộp pixel. Chế độ gộp pixel luôn là **Trung bình** — xem bên dưới. |
| `lattice format [--set FMT] [--list]` | Định dạng pixel. |
| `lattice trigger [--mode On\|Off] [--source SRC] [--delay-us US] [--activation EDGE] [--list-sources] [--software]` | Kích hoạt phần cứng/phần mềm. |
| `lattice white-balance [--auto] [--off] [--red R] [--blue B]` (không có cờ = cân bằng trắng một lần) | Các thao tác cân bằng trắng. Chỉ áp dụng cho camera RGB/Bayer; không thực hiện (bỏ qua) trên camera đơn sắc M3M. Chế độ chụp một lần giữ nguyên cân bằng trắng đã tính toán (`BalanceWhiteAuto` và các hệ số khuếch đại); mọi thiết lập khác được ghi vào bộ nhớ mở sẽ được khôi phục về giá trị ban đầu, và toàn bộ dữ liệu sẽ được khôi phục khi chế độ chụp một lần không thể thực hiện. |
| `lattice color-profile [--set raw\|linear\|natural\|enhanced\|custom_temp] [--cct 2000-10000] [--get]` | Quy trình xử lý màu hiển thị RGB. `natural` (mặc định) là chế độ hoàn thiện trực tiếp tiết kiệm tài nguyên; `enhanced` bổ sung tính năng khử viền + độ sống động + độ tương phản cục bộ CLAHE để tạo ra hiệu ứng cân bằng trung tâm hoàn chỉnh với chi phí xử lý mỗi khung hình gấp khoảng 2 lần, do đó tốc độ khung hình **trực tiếp** sẽ thấp hơn — dù sao thì các bản chụp được lưu lại luôn được xử lý hoàn chỉnh. Chỉ dành cho máy ảnh RGB/Bayer; bị bỏ qua trên M3M đơn sắc. |
| `lattice color [--saturation 0-200] [--contrast 0-200] [--reset] [--get]` | Độ bão hòa/độ tương phản trong chế độ xem trước trực tiếp (máy ảnh bộ lọc RGB); `100` là trung tính, `--reset` đặt cả hai thông số này thành 100. Các ảnh chụp đã lưu không bị ảnh hưởng. Bỏ qua trên M3M đơn sắc. |
| `lattice highlights [{--set -100..0 \| --reset \| --get}]` (không có cờ = đọc) | Hiển thị **Điểm sáng** (máy ảnh có bộ lọc RGB): làm mờ các pixel sáng nhất, trắng nhất — ví dụ: một bề mặt trắng sáng được hiển thị như một đốm trắng phẳng. `0` = tắt, `-100` = mức mạnh nhất. Bản xem trước trực tiếp và các ảnh chụp ở mức hiển thị tuân theo cài đặt này; độ sáng/độ phản xạ thì không bao giờ tuân theo, và tính năng này không thể khôi phục dữ liệu cảm biến đã bị cắt (hãy giảm độ phơi sáng để khắc phục điều đó). Bỏ qua trên M3M đơn sắc. |
| `lattice filter [--list]` | Liệt kê các mô hình bộ lọc mà Chloros biết (`RGN-IMX265`, `OCN`, `NGB`, …). Bộ lọc của máy ảnh được xác định từ chuỗi mô hình của nó và không thể thiết lập: `--set NAME` chỉ tra cứu mô hình, sau đó thoát khỏi `1` với `setting the filter model is not supported`. |
| `lattice power [--sleep]` | Kiểm tra các nút nguồn/nhiệt; bật/tắt chế độ chờ tiết kiệm năng lượng. `--sleep` để máy ảnh ở chế độ ngủ (giới hạn thông lượng liên kết được bật (`DeviceLinkThroughputLimitMode` `On`) ở mức tối thiểu, chế độ tiết kiệm năng lượng được bật nếu máy ảnh có) với mọi cài đặt khác, các giá trị ban đầu được khôi phục như ban đầu; trường hợp chế độ ngủ không thành công sẽ được khôi phục hoàn toàn. Khi thức dậy, kết nối sẽ được khôi phục về trạng thái như khi vào chế độ ngủ (giới hạn, chế độ và độ trễ gói tin `GevSCPD`, mà camera duy trì ở mức 0 khi giới hạn được bật; bản ghi được lưu trữ trên máy tính này, do đó khi thức dậy sau khi máy chủ phía sau khởi động lại cũng sẽ tìm thấy nó), hoặc, đối với camera mà máy tính này không đưa vào chế độ ngủ nhưng trông có vẻ đang ngủ (giới hạn của nó được bật ở mức sàn), với giới hạn tắt và giá trị `GevSCPD` được giữ ở 0 tại 80 (cài đặt mặc định); giới hạn được đặt trên mức sàn sẽ được giữ nguyên. Cảm biến và lệnh đánh thức sẽ khôi phục lại giá trị mà lệnh mở đã ghi. |

> **Chế độ gộp pixel luôn là Trung bình.** `lattice resolution --binning N` thiết lập chế độ gộp pixel Trung bình và
> không hỗ trợ chuyển đổi chế độ. Chế độ gộp Sum nhân mỗi giá trị DN với N², tiêu tốn khoảng hai stop
> dải sáng cao nhất cho mỗi bước gộp; Chloros chia lại hệ số đó trước khi vào
> chuỗi đo bức xạ, nhưng các pixel bị chế độ Sum đẩy lên mức trần của cảm biến sẽ bị cắt và không thể
> khôi phục. Chế độ Sum không mang lại lợi ích gì mà chế độ Trung bình không có — chỉ số là bất biến theo tỷ lệ và các
> đại lượng bức xạ được hiệu chuẩn theo tỷ lệ Trung bình. Nếu máy ảnh được cài đặt sẵn ở chế độ Sum (do
> một công cụ khác để lại, hoặc do khôi phục cài đặt gốc), Chloros sẽ phát hiện điều này thông qua dữ liệu đọc lại từ nút và
> bù đắp; chạy lệnh `lattice resolution --binning N` sẽ đưa nó trở lại chế độ Average. Phiên bản 1.2.0
> `--binning-mode` vẫn được chấp nhận nên các tập lệnh cũ vẫn chạy, nhưng nó chỉ hiển thị một cảnh báo:
> chế độ gộp pixel vẫn ở chế độ Trung bình.

> **Thay đổi chế độ nhóm pixel, định dạng pixel và tín hiệu kích hoạt trên camera sẽ không có tác dụng.** Trên camera,
> phần nền vẫn mở và truyền dữ liệu (`cam-connect`, giao diện người dùng), lệnh `lattice resolution --binning`
> không có hiệu lực (nó hiển thị chế độ gộp pixel vẫn đang được áp dụng, `Binning: 1x1`), và
> `lattice format --set` và `lattice trigger --mode` bị camera từ chối
> (`SC_ERR_ERROR -1001`). Hãy giải phóng nó trước (`lattice cam-disconnect -s <SN>`), hoặc thiết lập
> chúng khi kết nối (`cam-connect --pixel-format …`, `array-connect --binning …`).

##### Đọc thông số phơi sáng và độ khuếch đại trong chế độ tự động

Trong khi `ExposureAuto` (`GainAuto`) của camera đang chạy, nút `ExposureTime` (`Gain`) sẽ lưu giữ **giá trị cuối cùng được ghi**, chứ không phải giá trị hiện tại của camera. Đối với camera mà backend đang giữ mở (`cam-connect`, `array-connect`, GUI), `lattice configure --get`, chế độ xem mặc định `lattice configure` và `lattice exposure` (không có cờ) do đó sẽ hiển thị giá trị của nút và, bên dưới đó, một ghi chú kèm theo khung hình mới nhất của camera:

```

ExposureTime = 2000.448
  note: ExposureAuto is Continuous: ExposureTime holds the last value written (2000.448); the camera's newest frame was at 4900 us.
```

Đọc giá trị khung hình từ ghi chú; giá trị nút chỉ là lần ghi cuối cùng. Qua HTTP, cùng một phản hồi `POST /api/camera/<serial>/read-nodes` giữ nguyên `nodes` và thêm `frame_values` (`{"ExposureTime": <us>, "Gain": <dB>}`, của khung hình mới nhất) và `notes` (câu trên). Cả hai chỉ tồn tại khi chế độ tự động không phải là `Off` và có khung hình được xác định; một camera được mở chỉ để đọc không có khung hình và không nhận được cả hai. `lattice gain` (không có cờ) chỉ in nút — sử dụng `lattice configure --get Gain` cho ghi chú.

##### Phơi sáng trên một thành viên mảng

Trong khi mảng đang truyền dữ liệu, mỗi thành viên chạy với **`AcquisitionFrameRateEnable`**, với tốc độ khung hình cao hơn một chút so với tốc độ kích hoạt của mảng: chu kỳ của nó là chu kỳ kích hoạt trừ đi 15% (tối thiểu 3 ms), hoặc tốc độ tối đa của chính camera khi tốc độ kích hoạt đã gần đạt đến mức đó. Do đó, chế độ**`configure --get`**trên một thành phần sẽ hiển thị giá trị**`AcquisitionFrameRateEnable`**dưới dạng**`True`**và**`AcquisitionFrameRate`**ở cài đặt đó; chế độ kích hoạt sẽ tắt lại khi mảng dừng lại.**Không ghi hai nút này lên thành viên mảng** — mảng sẽ đồng bộ hóa chúng với tín hiệu kích hoạt của mình và ghi đè lên chúng.

Cài đặt này cũng là giới hạn thời gian phơi sáng tối đa của thành viên: khoảng 85 ms ở tần số kích hoạt 10 fps (chu kỳ 100 ms), ở **bất kỳ vùng quan tâm (ROI) hoặc chế độ gộp pixel (binning) nào**. Trước phiên bản 1.3.0, một vùng quan tâm (ROI) nhỏ sẽ giới hạn thời gian phơi sáng của thành viên ở chu kỳ hoạt động tự do của cảm biến (4,9 ms ở độ phân giải 768 × 304), bất kể mảng kích hoạt chậm đến đâu.**Một lệnh `ExposureTime` thủ công có thời gian phơi sáng dài hơn giới hạn sẽ làm chậm tốc độ kích hoạt để phù hợp với nó** khi được ghi bằng `lattice exposure --set` hoặc `configure --set` (hoặc từ giao diện người dùng). Mảng trước tiên sẽ mở giới hạn trên của thành viên đó cho giá trị đó và để tốc độ chậm hơn chạy trong vài chu kỳ kích hoạt, do đó việc ghi có thể mất thêm vài giây; sau đó, tốc độ kích hoạt sẽ không nhanh hơn mức mà thời gian phơi sáng cho phép. Trên mảng có tốc độ tự động, tốc độ chạy trước đó sẽ được khôi phục lại khi mỗi thành viên có thời gian phơi sáng thủ công làm chậm mảng đã quay trở lại chế độ phơi sáng tự động. Một giá trị mà chính máy ảnh không thể chụp được vẫn sẽ bị từ chối, kèm theo phạm vi giá trị được chấp nhận trong thông báo lỗi, và chế độ phơi sáng tự động của thành viên đó sẽ được khôi phục nếu nó chỉ bị tắt tạm thời cho lần ghi đó. Một công thức phơi sáng `apply` không làm chậm bộ kích hoạt: nó được giữ ở mức trần. Chloros không bao giờ rút ngắn thời gian phơi sáng thủ công để phù hợp với tốc độ kích hoạt nhanh hơn; tốc độ cố định (`array-connect --target-fps F`) nhanh hơn mức cho phép của thời gian phơi sáng sẽ giữ cho thành viên đó ở mức tối đa đủ lâu, và thành viên đó sẽ nhận các tín hiệu kích hoạt không nhanh hơn mức cho phép của thời gian phơi sáng.

#### Hiệu chuẩn & Cảm biến

| Lệnh con | Mục đích |
| --- | --- |
| `lattice calibrate [--filter NAME] [--attempts N] [--save PATH]` | Hiệu chuẩn từ mục tiêu phản xạ. Tối đa `--attempts` khung hình (mặc định `30`) sẽ được tìm kiếm; một khung dữ liệu không đầy đủ (mất gói tin GVSP) sẽ làm mất một lần thử, không phải toàn bộ quá trình chạy. Lệnh sẽ thất bại với giá trị `no target detected in N attempts`, hoặc `no complete frame in N attempts: <last error>` khi không có khung dữ liệu nào đến đầy đủ. |
| `lattice dls {--connect \| --spectrum \| --irradiance} --mac MAC [--filter NAME] [--camera SERIAL] [--json]` | Phổ và cường độ dải tần của DAQ-M, được đọc qua Bluetooth (BLE) thông qua hệ thống nền. `--mac` là địa chỉ MAC BLE riêng của cảm biến và là thông số bắt buộc cho bất kỳ hành động nào trong ba hành động sau: không có giá trị mặc định, vì mỗi thiết bị đều có địa chỉ riêng (`daq pool-discover --only ble` liệt kê chúng); nếu thiếu thông tin này, lệnh sẽ thoát với mã lỗi `1`. Nếu không chọn hành động nào, lệnh sẽ hiển thị hướng dẫn sử dụng. `--camera SERIAL` tích hợp các giá trị cường độ dải tần đã hiển thị thông qua đáp ứng phổ đo được của chính camera đó thay vì đường cong chuẩn của mô hình bộ lọc — máy ảnh RGB, với gói hiệu chuẩn của máy ảnh đã được tải (kết nối) hoặc lưu trong bộ nhớ đệm trên máy tính này. Tiêu đề cho biết đường cong của ai đã được sử dụng: `Band magnitudes (camera <SERIAL>):`, hoặc `Band magnitudes (model <filter>):` khi máy ảnh không có (không phải máy ảnh RGB, hoặc không có gói nào được tải hoặc lưu trong bộ nhớ đệm) và thay vào đó, đường cong tiêu chuẩn đã được sử dụng. Qua HTTP, `POST /api/dls/cli-read` nhận `camera_serial` và trả về `band_magnitudes_source`. |
| `lattice vignette --input DIR --output DIR [--lens-model KEY]` | Áp dụng hiệu chỉnh hiệu ứng viền tối cho các hình ảnh hiện có, mỗi hình ảnh có trường phẳng riêng của ống kính (từ trường Model trong EXIF). `--lens-model` áp dụng cho các hình ảnh mà trường Model không chỉ định tên máy ảnh; hình ảnh có trường Model mâu thuẫn sẽ bị bỏ qua. |

#### Nhiều máy ảnh (Phiên làm việc tạm thời)

Một lệnh tạm thời sẽ mở các máy ảnh cho một lần chạy và đóng chúng khi kết thúc: `multi-capture` không kết nối với mảng dữ liệu lưu trữ, `multi-stream` và `multi-test` ở đây, và `align-calibrate`, `align-apply`, `align-stream` và `index --live` bên dưới. Các quy tắc này áp dụng cho tất cả các lệnh trên:

- **`--serials SN1,SN2,…` chọn các camera.** Chỉ những camera đó mới được mở, và camera đầu tiên sẽ kích hoạt tín hiệu (đối với `align-calibrate`, `--reference` khi được chỉ định). Một thiết bị nối tiếp không nằm trên mạng sẽ khiến quá trình chạy thất bại trước khi bất kỳ camera nào được mở. Nếu không có `--serials`, tất cả camera trên mạng sẽ được mở. Một camera mà Chloros đã nắm giữ (GUI, `cam-connect`, `array-connect`) sẽ từ chối thực thi dù `--serials` có chỉ định tên nó hay không: nó chia sẻ mạng mà quá trình chạy được thiết lập cho. (`index --live` mà không có `--serials` sẽ mở các camera được kích hoạt trong hồ sơ của nó thay vào đó: xem [Index / Vegetation Maths](#index--vegetation-maths).)
- **Các camera chia sẻ mạng theo cách mà `array-connect` chia sẻ.** Mỗi camera được điều chỉnh tốc độ theo phần chia sẻ của mình trong tổng dung lượng mà liên kết chủ truyền tải (đây là độ trễ giữa các gói tin, không phải giới hạn thông lượng của camera, mức sàn 31,25 MB/s của giới hạn này đã bị bốn camera trên mạng 1 GbE vượt qua), do đó bốn camera hoạt động trên một liên kết 1 Gbps. Khi các camera không thể gửi dữ liệu cùng lúc ở tốc độ chậm nhất mà chúng đạt được, chúng sẽ được kích hoạt lần lượt, mỗi camera có liên kết riêng, và lệnh sẽ hiển thị `Serial emit: the wire cannot carry these cameras at once, so each is triggered <ms> ms after the one before; their frames are not exposed together`. Khi chỉ cần một camera nào đó không phù hợp, quá trình chạy sẽ bị từ chối trước khi bất kỳ camera nào bắt đầu truyền dữ liệu: `array over-subscribes the wire: <n> cameras demand <x> MB/s but the collision-safe ceiling is <y> MB/s. …`, mã lỗi HTTP 409, thoát khỏi `1`.
- **Mọi camera đều phải truyền dữ liệu, nếu không quá trình sẽ thất bại.** Một camera không truyền được khung hình nào là lỗi: lệnh sẽ hiển thị `Error: … no frame from <n> of <total> cameras: SN:<serial> (<why>), …`, trong đó `<why>` là lỗi lấy dữ liệu của camera đó, sau đó là nguyên nhân gây ra lỗi, và thoát khỏi quá trình với mã `1`. Một khung hình đến với tình trạng mất gói tin GVSP (`IncompleteBufferError: GVSP packet loss …`) có nghĩa là mạng không đáp ứng được băng thông tổng hợp của các camera (`Every missing frame arrived with GVSP packet loss: …`): kiểm tra tốc độ kết nối của máy chủ và đường uplink của bộ chuyển mạch (`lattice network`), sử dụng khung jumbo, hoặc giảm số lượng camera đang hoạt động với lệnh `--serials`. Một camera không truyền dữ liệu thường là do không nhận được tín hiệu kích hoạt từ camera chính: kiểm tra cáp đồng bộ M8 của nó, sau đó chạy lệnh `lattice multi-detect`. Tác vụ mà mỗi lệnh thực hiện khi một nhóm khung dữ liệu thiếu một camera được mô tả trong hàng tương ứng.
- **Mỗi camera sẽ được khôi phục về trạng thái ban đầu.** Khi quá trình chạy kết thúc, mỗi camera sẽ quay trở lại trạng thái ban đầu khi quá trình chạy bắt đầu: `TriggerMode`, `TriggerSource`, `TriggerActivation`, `TriggerOverlap`, `TriggerDelay`, và `LineMode` / `LineSource` trên `Line2` và `Line3` (và tín hiệu khử nhiễu đầu vào kích hoạt từ mảng cung cấp cho đường tín hiệu của thiết bị phụ, `LineFilterSelector` / `LineFilterWidth`); đó là `PtpEnable` (và bộ `PtpSlaveOnly` / `PtpDomainNumber` được thiết lập cùng với nó); `ExposureAuto` / `GainAuto` (và `ExposureTime` / `Gain` khi nó được đặt ở giá trị thủ công); `TargetBrightness`, `ExposureAutoLimitAuto` / `ExposureAutoUpperLimit` / `ExposureAutoLowerLimit`, `GainAutoUpperLimit`, `ExposureAutoDamping`; `BalanceWhiteAuto` (và mỗi `BalanceRatio` trong đó cân bằng trắng được thiết lập thủ công); `BlackLevelAuto` / `BlackLevel`; `PixelFormat`, ghép pixel, `Width` / `Height` / `OffsetX` / `OffsetY`; `GammaEnable` (và `Gamma` trong trường hợp gamma được bật), `LUTEnable`, `ColorTransformationEnable`; `ReverseX` / `ReverseY`; `UserSetSelector`; kích thước gói tin (`GevSCPSPacketSize` / `DeviceStreamChannelPacketSize`); tốc độ truyền, `DeviceLinkThroughputLimitMode` (và `DeviceLinkThroughputLimit` khi chế độ là `On`), độ trễgiữa các gói tin `GevSCPD` và độ lệch thời gian bắt đầu `GevSCFTD` mà một mảng cung cấp cho mỗi camera; dữ liệu khối mà mỗi luồng kích hoạt (`ChunkModeActive`, mỗi `ChunkEnable` dưới `ChunkSelector` của nó); `AcquisitionFrameRateEnable` (và `AcquisitionFrameRate` khi tính năng kích hoạt được bật — khi tắt, tốc độ chỉ ở chế độ đọc và đọc tối đa theo nhịp độ cho phép, do đó đây không phải là một cài đặt). Chỉ những gì khác biệt mới được ghi lại; máy ảnh dừng lại trước tiên, tốc độ nhịp và tốc độ khung hình trước khi phơi sáng cần chu kỳ khung hình của chúng (máy ảnh giới hạn `ExposureTime` ở một chu kỳ khung hình). Với `--keep-settings`, dây kích hoạt và dây đồng bộ được ngắt kết nối, máy ảnh hoạt động tự do, và phần còn lại được lưu vào bộ nhớ riêng của máy ảnh theo cài đặt của chế độ chạy; `--clear-settings` khôi phục các cài đặt mặc định tại đó.
- **Thời gian phơi sáng được thiết lập dựa trên cảnh quay trước khi khung hình đầu tiên được lưu lại.** Chế độ chạy thu thập các nhóm khung hình đồng bộ và loại bỏ chúng trong khi thiết lập độ phơi sáng và độ khuếch đại cho từng máy ảnh dựa trên các khung hình đó. Mục tiêu là đạt được độ sáng trung bình trên mục tiêu của máy ảnh với không quá 1% khung hình bị cắt sáng; một cảnh có vùng sáng mạnh sẽ dừng lại trước khi đạt đến mục tiêu. Quá trình này thường mất 2–6 nhóm khung hình và tối đa 5 giây.
  - Mỗi camera nhắm vào mục tiêu mà mảng kết nối đặt làm điểm khởi đầu, thông qua bộ lọc của nó: Độ sáng mục tiêu **40**trên camera đa phổ (RGN, OCN, NGB) và**60** trên camera RGB. Con số này bằng một nửa mục tiêu phơi sáng 80 (ba phần tư trên RGB), do đó tấm phản xạ sáng ít có khả năng bị cắt hơn. `--brightness N` (0-255) thay vào đó sẽ thiết lập mục tiêu cho từng camera (`multi-test` không có `--brightness`: chế độ phơi sáng tự động của nó bị tắt). Tên nhật ký của hệ thống nền ghi lại mục tiêu phơi sáng của từng máy ảnh: `[transient-ae] AE target per camera (filter default): SN:<serial> <target>, …`.
  - Phơi sáng được ưu tiên trước, tối đa đến giới hạn phơi sáng tự động của chính máy ảnh. Độ khuếch đại chỉ điều chỉnh phần còn lại.
  - Giá trị phơi sáng và độ khuếch đại đã xác định được giữ nguyên trong suốt quá trình ghi hình, do đó mọi khung hình của `multi-stream` đều được chụp với cùng mức phơi sáng. Việc ghi hình cần theo dõi sự thay đổi của ánh sáng nên được thực hiện trên một mảng kết nối (`array-connect`), với tính năng phơi sáng tự động liên tục điều chỉnh.
  - `--exposure` và `--gain` được giữ nguyên như đã chỉ định. Khi cả hai đều được chỉ định, không có khung hình nào bị loại bỏ.
  - Khi quá trình chụp kết thúc, tính năng phơi sáng tự động của mỗi camera sẽ được bật lại.
  - Nhật ký của hệ thống nền sẽ báo cáo kết quả: `[array-ae] exposure solved in <s> s (<n> warm-up groups): SN:<serial> <ms> ms <dB> dB mean <m>/<target>, …`, hoặc `exposure NOT solved after …` kèm lý do, và `(not settled)` trên mỗi camera không được xử lý. Trong trường hợp này, các khung hình vẫn được ghi lại.
  - `CHLOROS_TRANSIENT_AE_SETTLE_S` thay đổi giới hạn 5 giây; `0` tắt tính năng này (xem [Biến môi trường](#environment-variables)).

**Trong khi Chloros đang điều khiển bất kỳ camera nào** (giao diện người dùng, `cam-connect` hoặc một mảng camera được kết nối), `multi-stream`, `multi-test`, `multi-detect`, `align-apply`, `align-stream` và `index --live` sẽ từ chối chạy, vì chúng mở tất cả các camera trên mạng và việc mở lần thứ hai của một camera đang bị giữ có thể làm sập hệ thống backend. (`index --live` từ chối khi camera mà nó định mở đang bị giữ: đó là `--serials` hoặc các camera trong cấu hình của nó, và bất kỳ camera nào thuộc cấu hình không chỉ định camera cụ thể.) Hệ thống backend trả về mã trạng thái HTTP 409 cho `CAMERA_HELD`, liệt kê từng camera đang bị giữ và các lệnh giải phóng (`chloros-cli lattice cam-disconnect --all` / `chloros-cli lattice array-disconnect --all`), và lệnh sẽ thoát khỏi `1`. Khi có một mảng được kết nối, `array-capture --continuous` ghi lại mảng đó thay thế cho `multi-stream`, và `array-capture` (được căn chỉnh theo mặc định) lưu một tập hợp đã được căn chỉnh thay thế cho `align-apply`; `multi-capture` và `align-calibrate` sử dụng một mảng được kết nối chứa các camera của chúng thay vì mở chúng.

| Lệnh con | Mục đích |
| --- | --- |
| `lattice multi-info [--serials SN1,SN2,…]` | Liệt kê tất cả các camera có vai trò đồng bộ; với `--serials`, chỉ liệt kê những camera đó, và với `MASTER`, chỉ liệt kê camera đầu tiên. |
| `lattice multi-capture [--serials SN1,SN2,…] [--format FMT] [--jpeg-quality N] [--processing LEVEL] [--brightness N] [--no-daq] [--daq-tolerance-ms MS]` | Một khung hình đồng bộ từ mỗi camera. Lưu **tất cả các loại xuất theo mặc định**khi mảng lưu trữ được kết nối, và giá trị đọc từ DAQ được sử dụng ([Giá trị đọc từ DAQ khi ghi lại khung hình](#the-daq-reading-a-capture-saves)); chế độ dự phòng tạm thời không có mảng chỉ được**loại bỏ hiệu ứng debayering** (chạy `array-connect` trước cho phần còn lại). `--brightness` chỉ áp dụng cho chế độ dự phòng: một mảng được kết nối sẽ giữ nguyên chế độ phơi sáng tự động của riêng nó. Chế độ dự phòng không lưu `.daq` (`--no-daq` / `--daq-tolerance-ms` không có tác dụng ở đó), và `multi-capture` không áp dụng `--force-daq`. Trong chế độ dự phòng, một camera không cung cấp khung hình nào sẽ khiến quá trình ghi hình thất bại (`Error: multi-capture failed: no frame from …`, thoát khỏi `1`) và không có tệp nào của nhóm khung hình đó được lưu lại. |
| `lattice multi-stream [--serials SN1,SN2,…] [--fps F] [--count N] [--format FMT] [--jpeg-quality N] [--brightness N]` | Khung hình đồng bộ hóa luồng (tạm thời), `--fps` mặc định là `3`, cho đến khi nhấn Ctrl+C hoặc `--count` theo nhóm. Nhóm khung hình mà camera không gửi gì sẽ không được ghi, và ba nhóm liên tiếp sẽ kết thúc luồng. Nếu có bất kỳ nhóm nào bị bỏ qua, quá trình sẽ kết thúc bằng cách in ra `Incomplete: <n> frame group(s) not written, a camera delivered nothing (SN:<serial> x<count>, …)`, cùng với `Error: the stream stopped: …` ghi tên camera khi nó kết thúc luồng, và thoát khỏi `1`X. Tự động dừng khi ổ đĩa đầy, in ra `Failed:` / `Disk full:` và thoát khỏi `1`; bất kỳ tệp nào không thể ghi được cũng sẽ thoát khỏi `1`. |
| `lattice multi-test [--serials SN1,SN2,…] [--count N] [--ptp-settle SECONDS]` | Kiểm tra thời gian đồng bộ GPIO (tạm thời), `--count` các tín hiệu kích hoạt mặc định `10`. Hiển thị cách mạng lưới được chia sẻ (`Wire: each camera paced to <x> MB/s (<n> on a <y> MB/s ceiling, <link> Mbps link)`, và dòng `Serial emit:` khi các camera được kích hoạt lần lượt, khoảng cách thời gian giữa các lần kích hoạt này sẽ được phản ánh trong dấu thời gian), một dòng `Sync group: master SN:<serial> -> SN:<serial>, …` cho mỗi nhóm đồng bộ (`(no slaves: not synced)` dành cho camera không có cáp đồng bộ, được kích hoạt độc lập), sau đó một hàng cho mỗi lần kích hoạt: `[  0] SN:<serial>=<timestamp> \| …`, với `SN:<serial>=(no frame)` dành cho camera không gửi dữ liệu nào trong lần kích hoạt đó. Sau đó là dòng `Offsets from master SN:<serial> (<PTP / device (PTP off)> clock, <n> trigger(s)):` và dòng `SN:<serial>: mean <±x> us, jitter <y> us` cho mỗi camera, mỗi camera so với máy chủ trong một chu kỳ đồng hồ: một tín hiệu kích hoạt có dấu thời gian kết hợp giữa đồng hồ thiết bị của các camera và PTP (PTP tắt hoặc vẫn đang khóa) sẽ bị loại bỏ, với dòng `Note:` dùng để đếm chúng. Bất kỳ khung hình nào bị bỏ lỡ đều khiến bài kiểm tra thất bại (`Error: multi-test failed: <k> of <n> frame groups incomplete (SN:<serial> missed <k>); …`, thoát khỏi `1`); ba tín hiệu kích hoạt không hoàn chỉnh liên tiếp sẽ kết thúc bài kiểm tra sớm. Bài kiểm tra cũng thất bại (thoát `1`) khi có ít hơn hai tín hiệu kích hoạt khiến tất cả camera đồng bộ theo cùng một đồng hồ (`… the clocks cannot be compared`), hoặc khi các đồng hồ không bao giờ khớp: Đồng hồ PTP chênh lệch hơn 1 ms (cộng với độ lệch tầng nối tiếp) so với đồng hồ chủ (`… PTP is not locked`), hoặc đồng hồ thiết bị có độ lệch thay đổi nhiều hơn mức đó giữa các lần kích hoạt. Nó sẽ khởi động grandmaster PTP nếu grandmaster chưa chạy, bật PTP cho mọi camera (như cách `array-connect` thực hiện, bất kể cấu trúc mạng như thế nào), và trước lần kích hoạt đầu tiên, chờ cho đến khi đồng hồ PTP của mọi camera đồng bộ với grandmaster và ổn định: `PtpStatus` ở chế độ Slave trên một máy chủ chính, chênh lệch không quá 100 us so với máy chủ chính và ổn định trong 3 giây, và mỗi camera đều khóa đồng hồ như vậy, liên tục không gián đoạn, trong 120 giây — khoảng 2 phút kể từ khi bật nguồn. Một đồng hồ vừa khóa có thể lệch tới ~200 us trong khoảng 2 phút sau khi hiển thị trạng thái Slave, và chỉ số `PtpOffsetFromMaster` của chính camera không phản ánh điều này; sau 120 giây, các camera sẽ nằm trong khoảng ~10 us so với đồng hồ đã ổn định. Một camera vẫn đang đồng bộ kể từ lần kiểm tra trước đó khi PTP vẫn được bật (theo cách mà backend này nhận diện), sẽ tiếp tục đếm từ thời điểm khóa đó, do đó một thử nghiệm ngay sau thử nghiệm khác sẽ không phải chờ lại; một camera được bật trong thử nghiệm, hoặc một camera được phát hiện đang bật PTP mà backend này chưa từng thấy trạng thái khóa (được bật ở nơi khác, hoặc trước khi khởi động lại backend), sẽ chờ 120 giây, và trong danh sách các camera vừa khóa, chúng vẫn tiếp tục chờ thời gian của mình. Một camera rời khỏi chế độ Slave, vượt quá 100 us hoặc theo dõi một grandmaster khác trong 3 lần đọc liên tiếp (1,5 s) trong thời gian chờ sẽ bắt đầu đếm lại; một hoặc hai lần đọc như vậy được ghi lại (`[array-sync] PTP SN:<serial> off lock at <s> s (<n> of 3): <status> <±x> us gm <id> -- locked <s> s, counting on`) và quá trình đếm tiếp tục; ID Grandmaster không thể đọc được sẽ được coi là không xác định, không phải là sự thay đổi. Việc số đếm của mọi camera bắt đầu lại cùng lúc (trong vòng một giây) là do thời gian của Grandmaster thay đổi, không phải của các camera: nhật ký của hệ thống backend ghi là `[array-sync] PTP grandmaster step: all <n> cameras restarted at <s> s …`, `ptp_lock` liệt kê nó trong `grandmaster_steps`, và một khoảng thời gian chờ sau đó hết hạn bắt đầu báo lỗi với `every camera's count restarted together at <s> s -- the PTP grandmaster's time moved under them …`. Giới hạn thời gian không được mở rộng cho trường hợp này. `--ptp-settle SECONDS` thiết lập thời gian ổn định cho lần chạy (0-600; `0` cho trường hợp không chờ ổn định). Thời gian chờ được giới hạn ở thời gian ổn định cộng thêm 90 giây (mặc định là 210 giây, tối thiểu 90 giây), và lệnh này chờ backend trong 240 giây cộng với thời gian ổn định (mặc định là 360 giây). Lệnh này in ra `PTP: locked after <s> s (each camera locked <settle> s, already locked: SN:<serial>, …; from the grandmaster: SN:<serial> <±x> us, …)`, và nhật ký của backend sẽ có một dòng mỗi 5 giây ghi lại trạng thái và độ lệch của từng camera, thời gian đã khóa và thời gian còn lại: `[array-sync] PTP at <s> s: SN:<serial> Slave <±x> us for <s> s (<s> s left), … -- <s> s left, the wait gives up at <s> s`. Mỗi camera được bật nguồn với PTP tắt; từ đó các đồng hồ sẽ khóa với nhau trong khoảng 10 giây. Sau đó, mỗi camera được khôi phục về trạng thái mà bài kiểm tra đã phát hiện — chế độ PTP, phơi sáng tự động và độ khuếch đại được bài kiểm tra giữ ở trạng thái Tắt trong suốt quá trình chạy, và các thiết lập còn lại (trừ khi có `--keep-settings`) — do đó, một camera vẫn bật PTP (`lattice configure -s <serial> --set PtpEnable=True`, hoặc một lần chạy với `--keep-settings`) sẽ duy trì trạng thái đồng bộ, và một khi hệ thống nền đã xác nhận nó đã đồng bộ trong thời gian ổn định, các lần kiểm tra sau đó sẽ không chờ đợi nó nữa. Nếu các đồng hồ không ổn định trong thời gian chờ, nó sẽ thất bại trước khi kích hoạt (`PTP: NOT locked …`, thoát `1`) và chỉ ra cách xử lý: một camera chưa bao giờ khóa (`PtpStatus Listening`) có nghĩa là máy chủ chính không thể kết nối với nó (`time-sync status`); một camera liên tục bị ngắt kết nối sẽ được ghi tên kèm theo thời gian đã khóa (`… had not been locked for the 120 s settle time … (SN:<serial> locked <s> s, <s> s to go)`); đồng hồ vẫn đang hội tụ: kiểm tra `time-sync status`, và để đảm bảo chúng được khóa trước khi kiểm tra, hãy bật PTP và để chế độ này hoạt động (`lattice configure -s <serial> --set PtpEnable=True` cho từng camera, hoặc chạy kiểm tra với `--keep-settings`; quá trình kiểm tra sẽ khôi phục lại trạng thái ban đầu, do đó chế độ này vẫn được duy trì), sau đó chạy kiểm tra. |
| `lattice multi-detect [--line LINE] [--json]` | Tìm các camera trên đường dây đồng bộ M8 và một camera chủ mặc định: camera RGB (mã model kết thúc bằng `-FRGB`) nếu nó được kết nối bằng cáp, nếu không thì là camera được kết nối bằng cáp đầu tiên theo thứ tự số sê-ri. Các dây cáp M8 gặp nhau tại một điểm nối, do đó **bất kỳ camera nào được kết nối bằng cáp đều có thể làm máy chủ** — mã in `MASTER` chỉ là giá trị mặc định, không phải thông tin về cách kết nối. Mỗi camera lần lượt điều khiển đường truyền và các camera khác sẽ được kiểm tra tín hiệu xung của nó; một camera đã được phát hiện trong nhóm có dây sẽ không bị quét lại, do đó một mảng được kết nối đầy đủ chỉ mất một vòng quét. Nếu không có `--line`, hệ thống sẽ quét `Line2`, sau đó là `Line3`. Hiển thị `MASTER: SN:…`, một giá trị `SLAVE: SN:…` cho mỗi camera khác đã được kết nối và `(probed on <line>)`; `--json` thêm kết quả đầy đủ (`master`, `slaves`, `no_cable` — các camera mà xung không đến được — `line`, và `results` cho từng camera). Khi hoàn tất, mỗi camera sẽ lấy lại các thiết lập kích hoạt và đường đồng bộ mà nó đã có (các nút tương tự như trên). |

#### Căn chỉnh

| Lệnh con | Mục đích |
| --- | --- |
| `lattice align-calibrate [--serials SN1,SN2,…] [--method orb\|akaze\|phase\|checkerboard\|manual] [--model translation\|rigid\|affine\|homography] [--frames N] [--checkerboard RxC] [--points PATH] [--reference SN] [--save PATH] [--preview] [--vignette] [--prefilter none\|gradient\|clahe\|blur\|hist_match] [--rms-threshold-px N] [--brightness N]` — các núm điều chỉnh bộ phát hiện/bộ khớp `[--max-features N] [--ratio-threshold F] [--matcher bf\|flann] [--knn-k N]`, các thông số RANSAC `[--ransac-threshold-px F] [--ransac-iters N] [--ransac-confidence F]`, kết hợp nhiều khung hình `[--averaging mean\|median\|inlier_weighted]`, ràng buộc hình học `[--lock-rotation] [--lock-scale] [--lock-axis x\|y]`, hạn chế không gian `[--roi X0,Y0,X1,Y1] [--mask PATH]`, và-ghi đè nô lệ `[--per-cam-override SN:KEY=VALUE]` (có thể lặp lại) | Tính toán hồ sơ căn chỉnh từ camera trực tiếp. Giá trị mặc định: `--method orb`, `--model rigid`, `--frames 3`, `--save alignment_profile.json` (trong thư mục bạn chạy lệnh này), `--max-features 2000`, `--ratio-threshold 0.75`, `--matcher bf`, `--knn-k 2`, `--ransac-threshold-px 3.0`, `--ransac-iters 2000`, `--ransac-confidence 0.99`, `--averaging mean`. (`project align calibrate` có các giá trị mặc định riêng; xem [bảng của nó](#project-align-calibrate-options).) `--prefilter` mặc định là `gradient` (bản đồ cạnh; tương thích với công cụ căn chỉnh GUI/mảng — các cạnh vẫn tồn tại qua các dải quang phổ). `--matcher flann` mang lại hiệu quả khi có trên ~5000 đặc trưng; `--averaging median` ổn định ngay cả khi có một lần chụp hình không tốt, `inlier_weighted` cân bằng theo số lần khớp; `--lock-scale` chiếu lên góc xoay gần nhất (không thay đổi tỷ lệ), `--lock-axis` đặt thành 0 một thành phần dịch chuyển; `--mask` áp dụng cho mọi camera (sử dụng `--per-cam-override` cho cài đặt riêng từng camera, ví dụ: `--per-cam-override 214701292:method=phase`). `--rms-threshold-px` từ chối lưu bản hiệu chỉnh có giá trị RMS tái chiếu vượt quá ngưỡng. `--method manual` lấy các điểm từ `--points PATH`: một đối tượng JSON `{"<serial>": [[x, y], ...], ...}` cung cấp tọa độ pixel của các đặc trưng tương ứng trên từng camera, bao gồm cả camera tham chiếu (ít nhất 2 điểm cho mỗi camera); mỗi bản hiệu chỉnh `--frames` được xác định từ các điểm đó. Khi một mảng kết nối chứa các camera, quá trình hiệu chỉnh sẽ sử dụng các khung hình trực tiếp từ mảng đó, và cấu hình cũng trở thành thông số căn chỉnh cho các khung hình của mảng đó. Trong trường hợp khác, hệ thống sẽ mở một [mảng tạm thời](#multi-camera-transient-sessions): một camera không cung cấp khung hình nào trong bất kỳ `--frames` nào sẽ khiến quá trình hiệu chuẩn thất bại (thoát khỏi `1`, đặt tên cho nó), và không có cấu hình nào được lưu lại. |
| `lattice align-apply --profile PATH [--serials SN1,SN2,…] [--format tiff\|png] [--bit-depth 8\|12\|16] [--bands NAMES] [--order NAMES] [--gpu\|--no-gpu] [--no-crop] [--per-camera] [--per-band] [--vignette] [--interpolation nearest\|linear\|cubic\|lanczos] [--border-mode constant\|replicate\|reflect\|wrap] [--border-value N] [--brightness N]` | Ghi lại một khung hình đa dải đã được căn chỉnh. `--bit-depth` mặc định sẽ khớp với máy ảnh; `--no-crop` giữ nguyên khung hình đầy đủ (điền bằng màu đen); `--interpolation` (mặc định là `linear`) và `--border-mode`/`--border-value` (mặc định là `constant`/0) điều khiển độ biến dạng CPU — đường dẫn GPU luôn là tuyến tính hai chiều. Mở một [mảng tạm thời](#multi-camera-transient-sessions): nếu một camera không gửi khung hình nào thì quá trình ghi hình sẽ thất bại (thoát khỏi `1`, đặt tên cho nó), và không có dữ liệu nào được ghi. |
| `lattice align-stream --profile PATH [--serials SN1,SN2,…] [--fps F] [--count N] [--bit-depth 8\|12\|16] [--bands NAMES] [--order NAMES] [--gpu\|--no-gpu] [--no-crop] [--per-band] [--vignette] [--interpolation nearest\|linear\|cubic\|lanczos] [--border-mode MODE] [--border-value N] [--brightness N]` | Các khung hình đa dải được căn chỉnh theo luồng (các thông số warp giống như `align-apply`), `--fps` mặc định `3`. Mở một [mảng tạm thời](#multi-camera-transient-sessions): nhóm khung hình mà camera không gửi gì sẽ không được lưu, và ba lần liên tiếp sẽ kết thúc luồng. Nếu bất kỳ nhóm nào bị bỏ qua, quá trình chạy sẽ kết thúc bằng cách in ra `Incomplete: <n> frame group(s) not saved, a camera delivered nothing (SN:<serial> x<count>, …)` và `Error: …`, và thoát khỏi chương trình với thông báo `1`. Một luồng kết thúc do bất kỳ lỗi nào khác cũng sẽ hiển thị thông báo `Error: …` và thoát khỏi chương trình với thông báo `1`. |
| `lattice align-info --profile PATH [--json]` | Hiển thị chi tiết cấu hình. |
| `lattice align-reorder --profile PATH [--order NAMES] [--enable SERIALS] [--disable SERIALS]` | Thay đổi thứ tự lớp. |

#### Chỉ mục / Toán học thảm thực vật

```bash
# Offline: compute NDVI from an aligned multi-band TIFF (an RGN camera's bands)
chloros-cli lattice index --input aligned.tif --preset NDVI \
  --channel red=Red_660_RGN --channel nir=NIR_850_RGN \
  --output ndvi.tif --colorize --gradient RdYlGn

# Live: open the cameras, capture one aligned set with a profile made by
# `lattice align-calibrate`, and compute the index from it
chloros-cli lattice index --live --profile align.json --preset NDVI \
  --channel red=Red_660_RGN --channel nir=NIR_850_RGN \
  --save-multiband -o output/
```

**`--channel` ánh xạ các ký hiệu của cài đặt sẵn sang các dải** (NDVI: `red`, `nir`), mỗi dải một `--channel SYMBOL=BAND`.

- **`--live`: chỉ dành cho biểu tượng mà bộ xếp chồng không thể trả lời.** Giống như `--input`, biểu tượng cài đặt sẵn mà bạn bỏ qua sẽ lấy một dải của ngăn xếp (các dải của camera được kích hoạt trong cấu hình) có tên bắt đầu bằng biểu tượng đó. Việc này được kiểm tra trước khi bất kỳ camera nào được mở: một biểu tượng mà không có dải tần nào, hoặc nhiều dải tần, đáp ứng sẽ bị từ chối ngay lúc đó, liệt kê tên chúng, và thoát khỏi `1`. Một `--formula` tùy chỉnh cần có một `--channel` cho mỗi ký hiệu.
- **Các sản phẩm `--live` cho biết chúng thuộc về ai.** `index.tif` và `index_LUT.png` chứa thông tin về camera trong bộ (Hãng, Model, số sê-ri), thời gian chụp (thời gian phơi sáng của camera tham chiếu, UTC) và ống kính; cả hai đều đặt tên chỉ mục làm dải duy nhất của chúng (`XMP-Camera:BandName` `NDVI`; tệp PNG màu đặt tên chỉ mục, không phải các kênh màu của nó), giống như sản phẩm `--input`. Một chỉ mục từ các dải của nhiều máy ảnh sẽ đặt tên cho từng dải đó, và `Model` của nó là `LATTICE` — xem [Tệp được tạo từ nhiều máy ảnh](#files-made-from-several-cameras).
- **`--input`: chỉ dành cho ký hiệu mà tệp không thể xác định.** Ký hiệu bạn bỏ qua sẽ được gán cho dải màu duy nhất trong tệp có tên bắt đầu bằng ký hiệu đó: `red` sẽ lấy `Red_660` hoặc `Red_660_RGN`, `nir` sẽ lấy `NIR_850`. Các tên này được lấy từ chính tệp: danh sách dải của chính stack, hoặc, trên bản ghi độ sáng/độ phản xạ Chloros hoặc sản phẩm `process`, các thẻ `Chloros:Band{R,G,B}Name` / `XMP-Camera:BandName` của nó. Vì vậy, `lattice index --input <reflectance capture> --preset NDVI` không cần `--channel`.
- **Hai dải phổ tương ứng với một ký hiệu sẽ không được tự động gán tên** (một bộ ghép 9 camera có hai dải phổ NIR): đặt tên cho dải đó. Một ký hiệu không tương ứng với dải nào sẽ bị từ chối với `channel_map missing entries`, thoát khỏi `1`.**Tên dải phổ trong một bộ camera** mang tên bộ lọc của camera: các dải phổ của camera M3C là `<band>_<filter>` — `NIR_850_RGN`, `Green_550_RGN`, `Red_660_RGN` trên máy ảnh RGN; `NIR_808_OCN`, `Cyan_490_OCN`, `Orange_615_OCN` trên cảm biến OCN; `Blue_475_RGB`, `Green_550_RGB`, `Red_625_RGB` trên cảm biến RGB; `Blue_475_NGB`, `Green_550_NGB`, `NIR_850_NGB` trên NGB — và dải đơn của camera M3Mdải đơn của máy ảnh là `Mono_<filter>` (`Mono_660`, `Mono_850`). `lattice align-info --profile PATH` liệt kê các dải của một cấu hình.

Bộ cờ đầy đủ: `--input PATH | --live --profile PATH`, `--preset NAME` (NDVI/NDRE/EVI/SAVI/GNDVI/…), `--formula EXPR`, `--channel SYM=BAND` (có thể lặp lại), `--capture-level raw|debayered|radiance|reflectance|unknown` (ghi đè mức độ ghi lại được ghi trong tệp TIFF nguồn; mặc định: đọc từ siêu dữ liệu TIFF, nếu không có thì sẽ dùng giá trị `Chloros:ProcessingLevel` từ tệp Chloros, do đó dữ liệu phản xạ sẽ báo cáo giá trị `reflectance`), `--output PATH` / `-o` (ngoại tuyến: một đường dẫn; khi có phần mở rộng hình ảnh, tên tệp sẽ được đặt dựa trên phần gốc — `-o ndvi_FRGN.png` sẽ ghi thành `ndvi_FRGN.tif` và `ndvi_FRGN_LUT.png` — và nếu không có phần mở rộng, nó sẽ là tiền tố, `-o ndvi` -> `ndvi.tif`, `ndvi_LUT.png`; mặc định bên cạnh đầu vào, như `<input name>_<preset>`; với `--live` và không có `-o`, một thư mục `output/` nằm trong thư mục mà bạn chạy lệnh từ đó), `--output-format all|raw|tif|colorized|lut|png`, `--gradient NAME|JSON`, `--vmin/--vmax/--percentile LO,HI`, `--bg-mode clip|transparent|indexColor|backgroundColor`, `--colorize`, `--list-presets`, `--list-gradients`. Với `--live`, các núm điều chỉnh độ méo căn chỉnh cũng được áp dụng: `--save-multiband`, `--gpu/--no-gpu`, `--no-crop`, `--bit-depth 8|12|16`, `--vignette`, `--interpolation nearest|linear|cubic|lanczos`, `--border-mode constant|replicate|reflect|wrap`, `--border-value N`, `--brightness N` (mục tiêu phơi sáng tự động của camera, xem các quy tắc tạm thời), `--serials SN1,SN2,…`. `--live` mở một [mảng tạm thời](#phiên-tạm-thời-nhiều-camera): một camera không gửi khung hình nào sẽ khiến quá trình chạy thất bại (thoát khỏi `1`, đặt tên cho nó), và không có ngăn xếp hay chỉ mục nào được ghi lại. Nó mở `--serials` (cái đầu tiên kích hoạt tín hiệu), nếu không thì các camera trong cấu hình có mục `layer_order` được bật, với camera tham chiếu được ưu tiên trước — camera tham chiếu ngay cả khi các dải màu của nó bị vô hiệu hóa, vì các camera khác được căn chỉnh theo nó và nó kích hoạt bộ kích hoạt. Một camera có dải tần bị vô hiệu hóa sẽ không bao giờ được hiệu chuẩn, do đó một camera RGB bị vô hiệu hóa trong cấu hình sẽ không còn thất bại trong chỉ số phản xạ (`no-absolute-radiance-scale`). Một cấu hình không chỉ định camera nào sẽ mở tất cả camera trên mạng, như trước đây.

Mọi biểu mẫu đều chạy trên phần nền, do đó phần nền phải đang chạy: khởi động nó từ giao diện người dùng Chloros, hoặc bằng lệnh mà CLI hiển thị khi không thể kết nối được với nó. Khi ngoại tuyến, phần nền đọc `--input` và tự ghi tệp TIFF dạng số thực cùng tệp PNG đã được tô màu, do đó một `--input` hoặc `-o` sẽ được giải quyết dựa trên thư mục mà bạn chạy lệnh từ đó (xem [Quy ước](#conventions)). Cả hai sản phẩm ngoại tuyến đều chứa thông tin ghi lại từ máy ảnh của tệp đầu vào, giống như khi xuất chỉ mục `process`: Hãng / Mẫu / Số sê-ri, GPS, thẻ ống kính, thời gian chụp (thời gian từ chính tệp đầu vào; khi EXIF không ghi nhận, thì lấy thời điểm phơi sáng được ghi lại hoặc dấu thời gian trong tên tệp, theo UTC) và mã định danh hiệu chuẩn MAPIR — không bao giờ là hiệu chuẩn raw-DN, vì hiệu chuẩn này không áp dụng cho chỉ mục — cùng với `Chloros:ProcessingLevel=index` và biểu thức chỉ mục; tên tệp TIFF dạng số thực chỉ định một dải, tức là cài đặt sẵn. `--list-presets` và `--list-gradients` in ra bảng cài đặt sẵn và bảng độ dốc của backend. Một công thức bị backend từ chối, hoặc một dải `--channel` mà tệp TIFF không có, sẽ thoát khỏi `1` kèm theo lý do từ backend.

> **Các ký hiệu `--channel` phân biệt chữ hoa chữ thường.** Phần ký hiệu phải trùng khớp chính xác với tên kênh của cài đặt sẵn (các cài đặt sẵn sử dụng chữ thường, ví dụ: NDVI = `red`, `nir` — kiểm tra `--list-presets`), và phần dải phải trùng khớp với tên dải trong chồng hình ảnh đã căn chỉnh (hoặc là chỉ số dải bắt đầu từ 0 ở chế độ ngoại tuyến). `--channel red=Red_660_RGN --channel nir=NIR_850_RGN` hoạt động; `--channel RED=660` không thành công và báo lỗi `channel_map missing entries` .

#### `lattice index` — Mặt nạ chồng ảnh đa dải

Tại `--capture-level radiance` hoặc `reflectance`, quá trình tách dải 3×3 của mỗi camera có thể giới hạn giá trị của một dải xuống 0 (xem [Mặt nạ giới hạn tách kênh](#unmix-clamp-mask-which-pixels-a-band-was-clamped-on)). Một chồng ảnh đa dải chứa các dải từ nhiều camera, và mỗi camera giới hạn dải của riêng mình, do đó `--live --save-multiband` ghi **một mặt nạ cho mỗi camera nguồn** bên cạnh chồng ảnh:

```
output/
├── index_multiband.tif
└── clamp_masks/
    ├── index_multiband_SN<serial A>-<filter A>_clampmask.tif
    └── index_multiband_SN<serial B>-<filter B>_clampmask.tif
```

- Mỗi mặt nạ là một dải đơn `uint8` trên lưới pixel **của bộ xếp chồng** (qua cùng một quá trình căn chỉnh, biến dạng và cắt xén), với các bit riêng của camera đó: bit 0 = dải của kênh R, bit 1 = G, bit 2 = B. Một dải dữ liệu trong chồng ảnh lấy từ kênh *c* của một camera sẽ được giới hạn giá trị tại vị trí mà bit *c* của camera đó (R = 0, G = 1, B = 2) được đặt.
- Thông tin nguồn gốc nhúng trong mỗi dải dữ liệu sẽ chỉ định tên mặt nạ của camera tương ứng (`unmix_clamp_mask`, tương đối so với thư mục chứa dải, hoặc `none`) và bit cần đọc (`unmix_clamp_mask_bit`).
- Máy ảnh mà quá trình tách kênh (unmix) không giới hạn giá trị nào sẽ không có tệp; máy ảnh đơn sắc (mono) hoặc RGB không thực hiện tách kênh và cũng không có tệp nào. Giao diện dòng lệnh (CLI) hiển thị từng mặt nạ dưới tập ảnh (`clamp mask SN…: …`); phản hồi liệt kê chúng dưới dạng `clamp_masks`.
- Tệp chỉ mục TIFF/PNG của cùng một lần chạy được tính toán từ cùng một ngăn xếp, trên cùng một lưới, do đó các mặt nạ cũng mô tả tệp này. `--capture-level raw` không thực hiện tách màu và không ghi mặt nạ nào.
- Dãy hình ảnh và tệp chỉ mục nằm trực tiếp trong `--output`, do đó các mặt nạ được lưu trong `clamp_masks/` thuộc chính `--output` — không bao giờ nằm trên thư mục mà bạn đã đặt tên. Thư mục đó chứa nhiều loại tệp khác nhau (stack, index, bản render đã tô màu): hãy chọn các tệp cụ thể trong đó, đừng chuyển toàn bộ thư mục cho phần mềm photogrammetry.

#### Kết nối liên tục (Smart-Prep, Quy trình tương đương GUI)

Các lệnh này giữ các camera mở trong nhóm máy chủ phía sau (backend pool) qua các lần gọi lệnh dòng lệnh (CLI).

| Lệnh con | Mục đích |
| --- | --- |
| `lattice cam-connect [--serial SN]` | Thêm một camera vào nhóm (một camera duy nhất, không phải mảng). Các [tùy chọn chung](#common-options-shared-by-most-subcommands) `--exposure`, `--gain`, `--preset`, `--pixel-format`, `--width` / `--height` và `--packet-size` (`standard` = 1500, `jumbo` = 9000, hoặc byte; các cảm biến `auto`) được áp dụng khi kết nối (các công tắc `--exposure` hoặc `--gain` sẽ tự động tắt chế độ này). Nó sẽ trả về kết quả ngay khi luồng dữ liệu của camera bắt đầu (chờ khoảng 15 giây cho việc đó, sau đó 10 giây cho bộ đệm hoàn chỉnh đầu tiên) và hiển thị `stream: streaming`, hoặc `stream: no_frames -- connected, no frames yet: …` khi luồng dữ liệu không truyền được gì — đối với camera sử dụng gói tin jumbo, kèm lời khuyên kết nối lại với mã lỗi `--packet-size standard`. Trong khi một camera đang kết nối (lần kết nối đầu tiên tải xuống dữ liệu hiệu chuẩn, khoảng 70 giây), lệnh khác cần camera sẽ chờ tối đa 5 giây, sau đó báo lỗi với mã trạng thái HTTP 503 là `CAMERA_POOL_BUSY` kèm tên camera đang kết nối; hãy chạy lại lệnh sau một lúc. `lattice status` và danh sách camera không bao giờ chờ đợi: một camera đang trong quá trình kết nối sẽ hiển thị trạng thái “đang kết nối” kèm theo giai đoạn hiện tại. |
| `lattice cam-disconnect [--serial SN] [--all] [--keep-settings \| --clear-settings]` | Giải phóng. Camera được khôi phục về trạng thái ban đầu khi kết nối (cấu hình kích hoạt, PTP, cài đặt); `--keep-settings` lưu cài đặt vào bộ nhớ riêng của nó, `--clear-settings` đặt lại bộ nhớ đó về cài đặt gốc (xem [Bộ nhớ camera](#camera-memory----keep-settings----clear-settings)). Một camera là thành viên của mảng sẽ giải phóng toàn bộ mảng. |
| `lattice cam-list` | Liệt kê các camera trong nhóm. |
| **`lattice array-connect`**|**Kết nối một mảng đồng bộ hóa liên tục (ĐIỂM VÀO ĐƯỢC KHUYẾN NGHỊ).** Chạy toàn bộ quy trình chuẩn bị thông minh (smart-prep) trên giao diện người dùng (GUI). Mất khoảng một phút cho 9 camera; ngay sau khi camera bật nguồn, quá trình này có thể mất vài phút. Trong khi chờ đợi, hệ thống sẽ hiển thị từng bước mà phần backend thực hiện, dưới dạng `    <s> s: <step>` (ví dụ: `    41 s: open SN:214000535 (4 of 9)`), và nhật ký của hệ thống nền có một dòng thời gian cho mỗi bước (`[array-connect <id>] <step>: <s> s …`) cùng một dòng cuối cùng liệt kê các bước chậm nhất. Nó chờ tối đa 600 giây. Nếu việc kết nối chưa hoàn tất vào thời điểm đó, hệ thống backend sẽ hủy bỏ: mọi camera đã được mở sẽ được đưa về trạng thái ban đầu và đóng lại, lệnh sẽ hiển thị `Error:` kèm bước đã đạt được và thoát với `1`, đồng thời không có mảng nào được kết nối (mã trạng thái HTTP504 `CLIENT_DEADLINE_PASSED`). Ctrl+C hủy kết nối theo cách tương tự (`Interrupted: …`); nếu mảng đã kết nối, dòng lệnh sẽ thông báo điều này và nêu tên lệnh `array-disconnect` giải phóng kết nối. Một lệnh bị ngắt trong khi đang chờ (`kill -9`) sẽ khiến kết nối bị hủy bỏ khi máy chủ phía sau không nhận được tín hiệu từ lệnh đó trong 30 giây. Một mảng đang hoạt động nhưng chưa có nhóm khung đồng bộ vẫn được báo cáo là đã kết nối, với luồng của nó là `no_frames`. |
| `lattice array-disconnect [--array-id ID] [--all] [--keep-settings \| --clear-settings]` | Giải phóng một mảng. Mỗi camera được để ở chế độ chạy tự do (`TriggerMode` `Off`), với tất cả các thiết lập khác giữ nguyên như khi kết nối được thiết lập — bao gồm cài đặt kích hoạt và đường đồng bộ (cài đặt của máy chủ`TriggerSource` trở về trạng thái ban đầu, đường đồng bộ trở về chế độ đầu vào), PTP và các cài đặt máy ảnh mà phiên làm việc đã thay đổi — do đó, lệnh dành cho một máy ảnh trên một thành viên sẽ có hiệu lực ngay lập tức. `--keep-settings` lưu từng cài đặt máy ảnhcủa từng máy ảnh theo trạng thái khi phiên làm việc kết thúc vào bộ nhớ riêng của máy ảnh; `--clear-settings` đặt lại bộ nhớ đó về cài đặt gốc. Xem [Bộ nhớ máy ảnh](#camera-memory----keep-settings----clear-settings). |
| `lattice array-list` | Liệt kê các mảng được kết nối. |
| `lattice array-status [--array-id ID]` | Tốc độ khung hình trực tiếp (fps), PTP, lỗi gần nhất. `ptp_enabled` chỉ bằng `True` khi mọi thành viên đều bật chế độ đọc PTP; `ptp_off:` chỉ ra tên của thành viên không bật chế độ này. Dòng lệnh `state:` đọc `restarting (applying settings)` trong quá trình khởi động lại cài đặt, `recovering (…)` khi một thành viên bị mất nguồn được mở lại (xem ghi chú bên dưới), `stalled (…)` khi vòng lặp grab đang chạy nhưng không xuất bản nhóm khung hình, và `stopped (…)` khi vòng lặp grab đã từ bỏ (kết nối lại mảng). |
| `lattice array-capture [--processing LEVEL\|all] [--levels L1,L2,…] [--aligned\|--no-aligned] [--orient] [--index\|--no-index] [--no-daq] [--daq-tolerance-ms MS] [--smart] [--fastest] [--compression deflate\|none] [--continuous\|--interval S] [--count N] [--duration S]` | Một lần chụp đồng bộ từ mảng trực tiếp — Đơn lẻ / Liên tục / Theo khoảng thời gian / Nhanh nhất. **Mặc định là `all`**(một tệp cho mỗi loại xuất áp dụng trên mỗi camera),**được căn chỉnh** theo cách chụp của ứng dụng (`--no-aligned` cho tệp gốc). Các camera bị bỏ qua (ví dụ: RGB) sẽ được báo cáo bằng `Skipped: SN:<serial> (<reason>)`; dữ liệu DAQ đọc từ lần chụp được sử dụng sẽ được lưu kèm theo, bao gồm cả bản chụp chỉ dữ liệu thô, và được báo cáo bằng `DAQ: <path>` ([Dữ liệu DAQ đọc từ lần chụp được lưu](#the-daq-reading-a-capture-saves)). Xem [Chế độ chụp, Thiết bị ghi và Xử lý lại ngoại tuyến](#capture-modes-recorders--offline-reprocess). |
| `lattice array-record [--fps F] [--duration S] [--gif] [--gif-only] [--expr EXPR]` | Ghi lại chế độ xem chỉ số kết hợp trực tiếp thành video/GIF (chất lượng giám sát; khi không có cửa sổ GUI nào mở, hệ thống nền sẽ hiển thị giá trị NDVI của mảng, hoặc `--expr`). |
| `lattice array-burst [--duration S] [--max-frames N] [--orient] [--build] [--products …]` | Chụp liên tiếp Bayer thô với tốc độ khung hình cao (chất lượng phân tích; xử lý lại ngoại tuyến). Giống như `array-record` và `array-capture --continuous`, nó kiểm tra trạng thái với hệ thống nền tại mỗi lần thăm dò: bị ngắt mà không dừng lại (`kill -9`), hệ thống backend sẽ dừng nó 30 giây sau lần báo cáo trạng thái cuối cùng (`stop_reason` `client_gone`; bản kê khai của chuỗi ảnh được hoàn tất và quá trình xây dựng chỉ mục kết hợp bắt đầu, giống như khi dừng) và mảng vẫn duy trì kết nối. |
| `lattice array-build-video --burst-dir DIR [--products …] [--fps F] [--save-tiffs] [--gif]` | Xử lý lại một loạt ảnh thô đã lưu thành video đã hiệu chỉnh. |

> **Một camera bị mất nguồn hoặc mất kết nối sẽ tự động kết nối lại.** Khi một thành viên ngừng phản hồi (việc khởi động lại chu kỳ nguồn của bộ chuyển mạch sẽ khởi động lại mọi camera mà nó cấp nguồn), mảng sẽ chờ đợi, mở lại kết nối khi thiết bị đó phản hồi trở lại, và khôi phục lại các thiết lập mà `array-connect` đã đặt: PTP, tín hiệu kích hoạt và đồng bộ, tốc độ mạng, định dạng pixel và kích thước khung hình, cùng chế độ phơi sáng tự động. Sau đó, nó tiếp tục truyền dữ liệu mà không cần thiết lập mới nào từ `array-connect`. Trong khi đó, `array-status` hiển thị `state: recovering (...)` và `last_error` cho biết camera nào đang được chờ; sau đó nó in ra `re-opened:`. Một máy ảnh đã khởi động lại sẽ trở lại với chế độ PTP tắt, do đó cho đến khi nó được mở lại, `ptp_enabled` sẽ là `False` và `ptp_off:` sẽ đặt tên cho nó.

> **Một camera đang được giữ sẽ không bao giờ được mở lần thứ hai.** Việc mở lần thứ hai sẽ dừng luồng dữ liệu của người giữ và ghi đè cài đặt của nó, đồng thời có thể gây treo hệ thống backend. Vì vậy, đối với một camera mà pool đang giữ (GUI hoặc `cam-connect`), một phần tử trong mảng, hoặc một camera mà tác vụ nền đang giữ (một `lattice continuous` đang chạy, `multi-stream` hoặc `align-stream`, của chính nó hoặc của khách hàng khác), `lattice continuous`, `lattice capture`, `cam-connect`, `power --sleep`, `calibrate`, mộtlần `white-balance` và `presets --apply` từ chối với mã trạng thái HTTP 409 `CAMERA_HELD`, nêu rõ chủ sở hữu và lệnh giải phóng (`chloros-cli lattice cam-disconnect -s <SN>`, `chloros-cli lattice array-disconnect --array-id <ID>`, hoặc đối với một tác vụ là ID của nó và người đã khởi tạo), và thoát khỏi `1`. Cũng như vậy với một camera đang được lệnh khác sử dụng ngay lúc này (một lệnh `multi-test` hoặc `multi-capture` đang mở camera của nó, một lệnh khác chỉ thực hiện một lần), đối với mọi lệnh ở đây: thông báo lỗi yêu cầu chờ lệnh đó hoàn tất. `lattice power` (không kèm `--sleep`) sẽ đọc qua handle riêng của đối tượng chứa (nếu có — như bể chứa, mảng, hoặc camera đang mở của một tác vụ), và sẽ bị từ chối nếu camera đó đang được lệnh khác sử dụng. Mỗi phản hồi `CAMERA_HELD` đều chứa mã trong cả `code` và `error_code`. Đối với các lệnh mở tất cả camera, xem [Multi-Camera (Transient Sessions)](#multi-camera-transient-sessions).

##### Các tùy chọn `array-connect`

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--serials SN1,SN2,…` | tự động phát hiện tất cả camera LATTICE (cần ≥2) | Thiết bị nối tiếp đầu tiên là MASTER. Khi bỏ qua, quá trình phát hiện sẽ lọc các mẫu LATTICE (M3C / M3M) và kết nối tất cả chúng. |
| `--line {Line0,Line2,Line3}` | `Line2` | Dòng đồng bộ GPIO. |
| `--target-fps F` | tự động | Tốc độ kích hoạt của thiết bị chủ. |
| `--force-tier {sim-capture-sim-emit, sim-capture-ftd-stagger, slip-emit-and-capture}` | tự động | Ghi đè bộ chọn cấp độ. |
| `--wire-ceiling-mbps MB_PER_S` | tự động phát hiện | **Ngân sách băng thông liên tục của máy chủ, tính bằng MB/s — con số mà toàn bộ việc phân bổ mảng phụ thuộc vào.** Hãy giảm giá trị này khi mảng báo cáo các khung dữ liệu bị hỏng GVSP: giá trị tự động được tính từ tốc độ kết nối được NIC công bố, vốn thường đánh giá quá cao đối với các bộ điều hợp USB, các làn PCIe mỏng và các cấu trúc chia sẻ bận rộn. Giá trị này được lưu trong khối ghi lại mảng của dự án, do đó việc mở lại / kết nối lại qua CLI/SDK sẽ khôi phục nó. Xem [Tình trạng mảng](#array-health--which-subsystem-is-losing-frames). |
| `--binning {1,2,4}` | không (cảm biến đầy đủ) | Ghép pixel phần cứng (luôn là Trung bình). |
| `--width N` / `--height N` / `--pixel-format FMT` | cảm biến đầy đủ; định dạng theo từng camera | Kích thước khung hình hoặc định dạng pixel cụ thể cho từng camera, thay vì cảm biến đầy đủ và lựa chọn định dạng theo từng camera (Mono12 / BayerRG12). |
| `--no-recommend` | tắt | Bỏ qua bước phân tích mạng (mỗi camera sẽ giữ nguyên kích thước hiện tại thay vì được đặt thành cảm biến đầy đủ). |
| `--no-ptp` | tắt | Vô hiệu hóa PTP (dấu thời gian giữa các camera sau đó **không** thể so sánh được). |
| `--packed {auto,on,off}` | `auto` | Truyền các khung hình 12-bit đã nén và giải nén trên máy tính này — nhiều khung hình hơn mỗi giây ở chế độ cảm biến đầy đủ, dữ liệu giống hệt nhau. `auto` chỉ nén khi khi cần thiết và máy tính này đủ khả năng xử lý; `off` không bao giờ đóng gói. Xem [Truyền tải 12-bit đóng gói](#packed-12-bit-transport---packed). |
| `--packet-size {auto,jumbo,standard,N}` | `auto` | Kích thước gói tin GVSP cho mỗi camera trong mảng. `auto` cho phép mỗi camera dò tìm mạng; `jumbo` = 9000, `standard` = 1500, hoặc một kích thước tính bằng byte sẽ khóa nó lại. Trước phiên bản 1.3.0, `array-connect` chấp nhận cờ này nhưng bỏ qua nó. |

> **Cảm biến đầy đủ theo mặc định.** Nếu không có `--width` / `--height` / `--binning` /
> `--pixel-format`, `array-connect` sẽ thiết lập mọi camera ở chế độ cảm biến đầy đủ — không
> gộp pixel, không cắt vùng ROI — và cho phép tốc độ khung hình giảm xuống mức mà máy tính và
> mạng có thể cung cấp, thay vì thu nhỏ kích thước khung hình để duy trì tốc độ. Dòng
> phân tích mạng hiển thị tốc độ dự kiến, và dòng tiếp theo liệt kê
> các tùy chọn nhanh hơn (gộp pixel hoặc vùng ROI nhỏ hơn) cùng với tốc độ dự kiến của chúng; bỏ qua
> `--binning` hoặc `--width`/`--height` để chọn một tùy chọn. Chế độ "Connect" không bao giờ tự động gộp pixel hoặc cắt vùng
> một cách độc lập.

##### Truyền tải 12-bit đóng gói (`--packed`)

Khi hoạt động ở công suất tối đa của cảm biến, tốc độ khung hình của mảng được xác định bởi tốc độ mà mỗi camera có thể gửi
các khung hình của mình. Các định dạng 12-bit (`BayerRG12`, `Mono12`) thường được truyền dưới dạng các
từ 16-bit; truyền tải **đóng gói** gửi cùng 12 bit trong 1,5 byte cho mỗi pixel và
giải nén chúng trên máy tính này, do đó mỗi khung hình mất ít thời gian hơn 25% để truyền —
khoảng **tăng thêm 33% khung hình mỗi giây ở chế độ cảm biến đầy đủ** (đo được: 10,7 → 14,0 fps
trên mảng 9 camera). **Dữ liệu được lưu trữ là giống hệt nhau**: cùng giá trị, cùng
tệp 16-bit, cùng siêu dữ liệu.

- `auto` (mặc định) chỉ nén khi điều đó có lợi **và** máy tính này có đủ
  khả năng giải nén. Khi kết nối, hệ thống phía sau sẽ đo tốc độ giải nén của chính nó, số lượng CPU,
  tải CPU hiện tại và băng thông mạng, do đó một máy tính để bàn có lõi dư thừa sẽ nén
  ở chế độ cảm biến đầy đủ, trong khi máy tính bận rộn hoặc có hiệu suất thấp sẽ truyền dữ liệu ở dạng đã giải nén. Các vùng quan tâm (ROI) nhỏ và khung hình được gộp lại sẽ
  ít.
- `off` không bao giờ nén — sử dụng tùy chọn này để loại trừ việc nén trong quá trình chẩn đoán, hoặc trên một
  máy tính phải dành toàn bộ CPU cho các tác vụ khác.
- `on` luôn nén ở định dạng 12-bit.

Kết quả đầu ra của connect hiển thị quyết định và lý do (`packed 12-bit: on (tự động:
dự kiến 11,0 -> 14,4 fps, …)`); `/api/camera/array/status` báo cáo
`packed_transport` và `packed_decision`. Để thiết lập giá trị mặc định cho toàn hệ thống
thay vì phải truyền tham số này mỗi lần, hãy đặt `CHLOROS_ARRAY_PACKED12=off` (hoặc
`on`) trong môi trường của backend — một giá trị `--packed on|off` được chỉ định rõ ràng vẫn có ưu tiên cao hơn.
Các định dạng 8-bit và 16-bit không bao giờ được nén.

### Một bản ghi dự án, mọi chế độ

Một thư mục dự án chứa các thiết lập của mọi thiết bị: `cameras.json` — của từng camera (`cameras[].settings`, cho một camera riêng lẻ và cho từng thành viên trong mảng) cũng như các thiết lập chung cho toàn bộ mảng (`arrays[].capture`: định dạng, vùng quan tâm (ROI), gộp pixel, tần suất kích hoạt, ngân sách băng thông) — và `sensors.json`, của từng cảm biến ánh sáng (thời gian tích hợp, trung bình khung hình, giới hạn). Ứng dụng, giao diện dòng lệnh (CLI), bộ công cụ phát triển phần mềm (SDK) và máy chủ MCP đều đọc và ghi vào cùng các tệp, theo cùng một cách:

- **Một thiết bị được kết nối trong một dự án sẽ được lưu vào dự án đó và ghi lại mọi thay đổi vào đó.** "Trong một dự án" có nghĩa là khi dự án đang mở ở phía máy chủ (của ứng dụng), hoặc — từ SDK — thông qua một tệp `ChlorosProject`, tệp này tự đặt tên cho thư mục của mình. Một camera, mảng cảm biến hoặc cảm biến ánh sáng mà dự án chưa lưu trữ sẽ được thêm vào các tệp của dự án khi thiết bị đó kết nối (`lattice cam-connect`, `array-connect`, `daq pool-connect`, ứng dụng, SDK, `chloros-mcp`), kèm theo các thiết lập của nó. Bất kỳ thay đổi nào từ bất kỳ chế độ nào (`lattice exposure`, `gain`, `configure --set`, `resolution`, cài đặt của mảngcài đặt và tốc độ kích hoạt, giới hạn của cảm biến ánh sáng, các điều khiển của ứng dụng) sẽ được lưu vào các tệp của dự án đó ngay sau đó. Các thiết lập về độ phơi sáng, độ khuếch đại hoặc mục tiêu AE trên máy ảnh sẽ được lưu dưới dạng đã khóa, do đó khi mở lại, các thiết lập này sẽ được giữ nguyên.
- **Khi mở một dự án, các thiết bị sẽ được thiết lập theo dự án đó**, trong mọi chế độ: máy ảnh, mảng cảm biến hoặc cảm biến ánh sáng được kết nối mà không có cài đặt riêng sẽ nhận các thiết lập đã lưu của dự án (`lattice cam-connect` / `array-connect` khi dự án đang mở, `chloros-cli project connect`, `open_project().connect_all()`, khi mở lại ứng dụng). Một mảng được kết nối dưới một ID mới sẽ cập nhật mảng mà dự án đã lưu với cùng các camera; một mảng có các camera khác sẽ được thêm vào bên cạnh nó.
- **Việc lưu của chính ứng dụng không bao giờ ghi đè lên các cài đặt trực tiếp của thiết bị** bằng bản sao cũ hơn của chúng, và giữ nguyên các tùy chỉnh riêng cho từng thành viên mà người gọi qua CLI hoặc SDK đã lưu.
- **Định dạng pixel mà bạn thiết lập chính là định dạng mà dự án lưu giữ.** Định dạng được thiết lập trong Cài đặt Mảng, bằng `--pixel-format` hoặc `connect_array(pixel_format=)` (bao gồm cả định dạng 8-bit), sẽ được lưu lại và kết nối lại chính xác. Nếu không được thiết lập, mỗi camera sẽ kết nối với định dạng mặc định 12-bit (`BayerRG12`, `Mono12` trên cảm biến đơn sắc). Tên định dạng thể hiện độ sâu bit của nó; dòng cảm biến thuộc về cảm biến đó, do đó máy ảnh màu không bao giờ được đặt thành định dạng đơn sắc (Mono) hay máy ảnh đơn sắc được đặt thành định dạng Bayer, và dòng `[PixelFormat <serial>] ... written` sẽ ghi chú điều này khi định dạng được ánh xạ. Các tệp xuất từ chế độ chụp 8dữ liệu chụp 8-bit sẽ được xuất ra dưới dạng 8-bit, bao gồm cả dữ liệu phản xạ: được điều chỉnh tỷ lệ và gắn nhãn với mã `PixelScale`.
- Thiết bị được kết nối mà không thuộc dự án nào sẽ không ghi lại dữ liệu.
- **Quá trình ghi dữ liệu sẽ tuân theo dự án được tạo sau đó; khi dự án được mở lại, thiết bị sẽ giữ nguyên dự án đó.**Khi một dự án được tạo — một dự án SDK `create_project()`, `chloros-mcp` của `create_project`, hoặc tùy chọn**New Project**— hoặc được sao chép (tính năng**Sao chép**của ứng dụng, mở bản sao) trong khi bản ghi vẫn đang ghi vào dự án đã mở trước đó, thì bản ghi sẽ tiếp tục và những gì nó ghi sẽ được lưu vào dự án mới, tại cùng vị trí bên trong đó (bên dưới). Mở một dự án hiện có — `chloros-cli process`, `project devices --reconnect`, `ChlorosProject.process()`, tính năng**Mở Dự án** của ứng dụng — sẽ để lại bản ghi vẫn đang diễn ra cùng với dự án mà nó đang ghi vào: bản ghi sẽ hoàn tất tại đó, và các tệp của nó sẽ được thêm vào danh sách tệp của dự án đó (ứng dụng sẽ dừng mọi bản ghi trước khi mở một dự án).
  - `array-record` và `array-burst`: video, hoặc thư mục chứa từng khung hình, sẽ được chuyển nguyên vẹn sang đó khi quá trình ghi hình dừng lại (tệp `--duration`, Ctrl+C). Tệp `Burst dir` được tạo ra là tệp mới, và `--build` cùng `array-build-video` hoạt động dựa trên tệp này. Nếu ổ đĩa từ chối di chuyển, bản ghi sẽ vẫn ở vị trí ban đầu; nhật kýsẽ giải thích lý do, cũng như phản hồi HTTP khi dừng (`relocation_error`). Các khung hình thô của đợt ghi bị dừng và `.daq` sẽ được thêm vào danh sách tệp của dự án mà nó thuộc về; một video (một `array-record`, video được tạo từ đợt ghi) sẽ không được thêm vào danh sách tệp của dự án.
  - `array-capture --continuous`: mọi dữ liệu ghi lại sau khi thay đổi sẽ được lưu vào dự án mới; những dữ liệu được lưu trước đó vẫn giữ nguyên vị trí cũ.
  - `daq pool-record` và `project sensor log` (cùng với tính năng ghi hình bằng cảm biến ánh sáng của ứng dụng): tệp `.daq` sẽ được chuyển vào thư mụcthư mục `light_sensor` của dự án đó khi quá trình ghi dừng lại, và được thêm vào danh sách tệp của dự án đó.

  Một bản ghi vào thư mục bên ngoài dự án đang mở (ví dụ: `-o` ở vị trí khác) sẽ vẫn ở nguyên vị trí cũ, còn `lattice continuous` và `multi-stream` sẽ giữ nguyên thư mục đã được chỉ định.
- **Trong ứng dụng, các bản ghi sau khi tạo dự án mới sẽ được lưu vào dự án đó.** Việc tạo hoặc nhân bản một dự án sẽ giữ nguyên mọi phiên ghi và quá trình ghi đang diễn ra, và mỗi bản ghi được thực hiện sau đó (Chụp Tất cả, Bắt đầu Ghi) sẽ được lưu vào dự án mới và được thêm vào danh sách tệp của dự án đó. Việc mở một dự án hiện có trong ứng dụng sẽ dừng tất cả các quá trình này trước tiên.
- **Một dự án sẽ không bị di chuyển trong khi đang ghi.**Việc di chuyển dự án đang mở sang một thư mục làm việc khác (trong phần**Cài đặt → Thư mục làm việc** của ứng dụng; không có lệnh CLI nào di chuyển dự án, chỉ có client HTTP gửi yêu cầu đến `/api/move-project` mới thực hiện được) sẽ bị từ chối với mã trạng thái HTTP 409 `PROJECT_RECORDING` khi đang diễn ra ghi mảng hoặc chế độ chụp liên tục, một tác vụ `lattice continuous` / `multi-stream` hoặc quá trình ghi dữ liệu từ cảm biến ánh sáng đang ghi vào thư mục đó: `Project '<name>' is recording (<what>), so moving the project now is refused. Stop the recording, then try again.` Ứng dụng cũng từ chối theo cách tương tự, trước khi yêu cầu chọn thư mục, trong khi các tác vụ "Capture All", "Start Capturing" hoặc ghi hình từ cảm biến ánh sáng của chính ứng dụng đang chạy.

Điều này khác với cài đặt còn lại trên camera khi ngắt kết nối (xem [Bộ nhớ camera](#camera-memory----keep-settings----clear-settings)): một dự án tự thiết lập các camera của mình khi được mở.

### Bộ nhớ camera — `--keep-settings` / `--clear-settings`

Khi một mảng được kết nối, các camera trong mảng đó sẽ áp dụng cài đặt của mảng, và bạn có thể thay đổi chúng sau đó. Khi Chloros ngắt kết nối các camera — `array-disconnect`, kết thúc của một lệnh tạm thời (`multi-capture` không có mảng, `multi-stream`, `multi-test`, `align-calibrate`, `align-apply`, `align-stream`, `index --live`), điểm dừng của `lattice continuous`, điểm kết thúc của `lattice capture` thuộc về một camera mà nó không nắm giữ — nó đưa từng thiết bị trở lại trạng thái ban đầu trước khi kết nối: dây kích hoạt và dây đồng bộ, cài đặt PTP và máy ảnh (danh sách đầy đủ nằm trong [Nhiều máy ảnh (Phiên tạm thời)](#multi-camera-transient-sessions)).

Để sử dụng các máy ảnh bên ngoài dự án, hai cờ trên bất kỳ lệnh nào trong số đó sẽ ghi vào **bộ nhớ riêng** của máy ảnh — những cài đặt mà máy ảnh khởi động với, trong bất kỳ phần mềm nào:

- **`--keep-settings`** lưu các cài đặt của từng máy ảnh vào bộ nhớ của nó theo trạng thái khi phiên làm việc kết thúc (UserSet2, được thiết lập làm bộ cài đặt khởi động). Các camera vẫn được để chạy tự do (`TriggerMode` `Off`), kết nối dây đồng bộ được giữ nguyên như hiện trạng. Một camera không có UserSet2 sẽ lưu vào UserSet1; máy tính này ghi nhớ điều này, do đó mảng Chloros tự phục hồi — vốn sẽ đặt lại UserSet1 đã lệch khỏi mức cơ sở của Chloros — sẽ không can thiệp vào cài đặt đó.
- **`--clear-settings`** đặt lại bộ nhớ của từng camera về cài đặt gốc: mức cơ sở của Chloros, tức là các cài đặt mặc định của nhà máy được tích hợp sẵn trong cơ chế tự phục hồi của mảng — các cài đặt mặc định nhà máy với `TriggerMode` `Off`, `TriggerSource` `Line2`, phơi sáng tự động, cân bằng trắng đã hiệu chỉnh của camera RGB được giữ lại — trong UserSet1, được thiết lập làm bộ khởi động.
- Hai chế độ này không thể kết hợp với nhau. Mỗi camera có một dòng: `SN:…: settings saved to the camera's memory (UserSet2); it powers up with them.` hoặc `SN:…: camera memory reset to the factory settings.`; phản hồi `camera_memory` cũng tương tự cho từng camera.

Ứng dụng không cung cấp các cờ này.

**Mọi cách thoát đều khôi phục lại các camera.**Quy trình khôi phục nguyên trạng như ban đầu được thực hiện trên mỗi đường thoát, không chỉ các lệnh trên: `cam-disconnect` (và camera**X**của ứng dụng);**X** của ứng dụng trên một mảng hoặc trên hàng của bất kỳ thành viên nào trong mảng (một mảng không thể tiếp tục chạy nếu thiếu thành viên, do đó toàn bộ mảng sẽ được giải phóng); đóng ứng dụng (nó cho phép hệ thống nền có tối đa 20 giây để khôi phục mọi camera, mảng và lệnh `lattice continuous` đang chạy trở lại trước khi dừng ứng dụng; nếu lúc đó vẫn còn lệnh đang chạy — một lệnh tạm thời đang mở camera hoặc chờ đồng hồ PTP của chúng, một lệnh `lattice capture` chỉ chạy một lần, `power`, `white-balance` hoặc `presets --apply` — sẽ dừng tại điểm an toàn tiếp theo, đưa các camera trở lại trạng thái ban đầu trước tiên, trong tối đa 8 giây, và trả về mã trạng thái HTTP 503 `SHUTTING_DOWN`; một kết nối dạng mảng vẫn đang chạy — `lattice array-connect`, kết nối của ứng dụng — sẽ dừng lại trước camera tiếp theo và đóng các camera đã mở, trong tối đa 10 giây, và trả về mã trạng thái HTTP 503 `SHUTTING_DOWN`; và một yêu cầu đến trong quá trình tắt máy sẽ không mở bất kỳ camera nào: HTTP 503 `SHUTTING_DOWN`); và một lệnh CLI bị ngắt hoặc khi cửa sổ terminal đóng lại (SIGTERM / SIGHUP dừng công việc — và khôi phục lại các camera — trước khi thoát khỏi `143` / `129`, giống như Ctrl+C). Một tiến trình `lattice continuous`, `multi-stream` hoặc `align-stream` bị giết ngay lập tức (SIGKILL), vốn không thể dừng bất kỳ tác vụ nào, sẽ có tác vụ của nó bị dừng và các camera được đưa về trạng thái ban đầu bởi hệ thống backend sau 30 giây sau lần kiểm tra cuối cùng. Một backend bị treo hoặc bị buộc dừng không thể khôi phục lại bất kỳ dữ liệu nào; việc khởi động lại nguồn sẽ đưa mỗi camera về trạng thái ban đầu trong bộ nhớ, và lần kết nối tiếp theo của Chloros sẽ thiết lập lại camera mà nó phát hiện đang ở chế độ hoạt động.**Một dự án tự thiết lập các camera của mình.** Khi một mảng kết nối với một dự án đang mở, mỗi thành viên sẽ nhận được các cài đặt mà dự án đã lưu cho nó — mức phơi sáng, độ khuếch đại hoặc mục tiêu AE được giữ nguyên (được coi là của bạn, do đó chế độ phơi sáng tự động sẽ không can thiệp) và chế độ cân bằng trắng (sửa chữa của Chloros; chế độ cân bằng trắng gốc của camera sẽ bị tắt) — bất kể dữ liệu nào được lưu trong bộ nhớ của camera.

### Smart-AE / Smart-Capture

Các mảng LATTICE chạy chế độ phơi sáng tự động liên tục (AE) ở chế độ nền ngay khi được kết nối, nhưng một cảnh mới được định hướng sẽ mất một chút thời gian để ổn định. `array-capture --smart` là **tính năng tiện lợi tích hợp sẵn**: nó chờ cho đến khi AE ổn định trên mọi máy ảnh trong mảng, sau đó mới kích hoạt chụp ảnh. Sử dụng tính năng này khi bạn thay đổi cảnh giữa phiên chụp.

```bash
# Connect once, then take settled captures whenever you re-point the rig
chloros-cli lattice array-connect --serials SN1,SN2,SN3,SN4
chloros-cli lattice array-capture --smart --processing reflectance -o pose_a/
# (move the rig)
chloros-cli lattice array-capture --smart --processing reflectance -o pose_b/
```

Chính sách ổn định mặc định khá thận trọng: thời gian chờ 5 giây, khoảng thời gian ổn định 1,5 giây, dung sai độ phơi sáng ±5%. Điều chỉnh thông qua SDK (`ArrayHandle.capture_smart(settle_timeout_s=…, stability_window_s=…, exposure_tolerance_pct=…)`) nếu bạn cần hành vi khác so với chế độ tự động.

#### Cách hoạt động của chế độ phơi sáng tự động

- **Độ phơi sáng trước, độ lợi sau.** Khi độ lợi ở chế độ Tự động, độ lợi sẽ duy trì ở mức 0 dB cho đến khi độ phơi sáng đạt đến giá trị tối đa được phép.
- **Khi phơi sáng thủ công, độ lợi vẫn ở chế độ Tự động** cho đến khi bạn thay đổi độ lợi hoặc chuyển sang chế độ thủ công. Khi quay lại chế độ Tự động, giới hạn độ lợi sẽ trở về mức ban đầu.
- **Các vùng sáng nhỏ không làm tối cảnh.** Các vùng phản xạ chiếm tối đa 1% khung hình sẽ bị bỏ qua, và trên các tấm của mục tiêu hiệu chuẩn đã biết, tối đa 0,5% diện tích tấm.
- **Giới hạn phơi sáng DAQ chỉ áp dụng khi mục tiêu hiệu chuẩn nằm trong khung hình.** Tính năng này ngăn không cho tấm màu trắng bị cắt sáng; khi không có tấm nào trong khung hình, máy ảnh sẽ phơi sáng theo cảnh.
- **Ánh sáng ổn định sẽ giữ nguyên, và ánh sáng thay đổi sẽ ổn định lại trong vài giây.** Khi ánh sáng ổn định, mức phơi sáng và giới hạn của nó sẽ không thay đổi; sau một sự thay đổi lớn về ánh sáng, khung hình sẽ ổn định lại trong vài giây. Hiện tượng cắt sáng (clipping) sẽ không còn giới hạn mức phơi sáng vĩnh viễn, và khi vùng quan tâm (ROI) thay đổi, mức phơi sáng sẽ được điều chỉnh lại.
- **Mục tiêu độ sáng vẫn trong tầm với.** Mục tiêu này không tăng lên khi máy ảnh đang ở giới hạn phơi sáng và độ khuếch đại của máy ảnh không thể tăng lên, do đó, ánh sáng quay trở lại sẽ không làm lóa khung hình.
- **Một máy ảnh có tính năng phơi sáng tự động ngừng phản hồi sẽ được giữ ở mức phù hợp với ánh sáng hiện tại.** Dưới ánh nắng chói chang, máy ảnh có thể giảm xuống mức phơi sáng ngắn nhất và giữ nguyên ở đó với khung hình tối, trong khi tất cả các thông số vẫn hiển thị bình thường. Chloros theo dõi khung hình, giữ máy ảnh ở mức phơi sáng đã thiết lập cho điều kiện ánh sáng đó, và thả ra khi ánh sáng thay đổi. Cảm biến ánh sáng bị che khuất hoặc tối đi sẽ không ảnh hưởng đến quá trình này.

### Mức độ xuất ảnh (mặc định của `all`)

Kể từ phiên bản này, `lattice capture`, `lattice multi-capture` và `lattice array-capture` **được đặt mặc định thành `--processing all`** — mỗi loại xuất sẽ có một tệp lưu riêng áp dụng cho từng camera, tương ứng với hành vi "Capture All" trên giao diện người dùng (GUI). Các mức này là:

| Mức | Đầu ra | Áp dụng cho |
| --- | --- | --- |
| `raw` | Bayer kênh đơn (camera đơn sắc: dải đơn) trực tiếp từ cảm biến. | Tất cả camera. |
| `debayered` | Màu 3 kênh, được lưu theo thứ tự kênh **RGB**(tệp TIFF với diễn giải quang học RGB; PNG/JPEG cũng mở dưới dạng RGB): giá trị bù tối (DSNU) từ gói hiệu chuẩn được trừ khỏi dữ liệu thô, sau đó áp dụng thuật toán khử mô-saic nhận diện cạnh của OpenCV— không có LUT tuyến tính, hiệu chỉnh trường phẳng hay tách màu (máy ảnh đơn sắc: 1 kênh xám). Các khung hình đã khử hiệu ứng bayesian trong bộ nhớ mà Python SDK trả về (`grab()`, `frame_stream()`) có thứ tự**BGR** theo tiêu chuẩn OpenCV; chỉ tệp lưu trữ mới ở định dạng RGB. | Tất cả các camera. |
| `radiance` | float32 W/m²/sr/nm thông qua chuỗi đo quang học đầy đủ. | Chỉ dành cho camera đa phổ (M3C/M3M) — **bỏ qua đối với các camera có bộ lọc RGB**. |
| `reflectance` | uint16 ρ (`32768` = 1.0), sẵn sàng cho Pix4D. | Chỉ đa quang phổ, và **chỉ khi DAQ được liên kết + máy ảnh được hiệu chuẩn**; nếu không thì sẽ bị bỏ qua. |
| `display` | Chuỗi xem trước GUI đầy đủ (CCM + WB + gamma theo cấu hình của camera). Được lưu trong thư mục `preview/`. `lattice capture` cũng chấp nhận tên `preview` cho nó; `array-capture` / `multi-capture` chỉ chấp nhận `display`. | Tất cả các camera. |

Chỉ định một cấp độ để chỉ lưu cấp độ đó (`--processing debayered`). Khi yêu cầu `all`, các cấp độ không áp dụng cho cam cụ thể sẽ bị bỏ qua (và được báo cáo), chứ không gây lỗi — một camera không được kết nối hoặc chưa được hiệu chuẩn vẫn nhận được `raw` / `debayered` / `preview`.

#### Dữ liệu DAQ được ghi lại khi lưu bản chụp

Mỗi khi một DAQ được liên kết với camera hoặc mảng camera, hoặc được gộp chung, dữ liệu hướng xuống được sử dụng trong bản chụp sẽ được ghi vào tệp **`.daq`** trong thư mục con `daq/` của thư mục captures, kèm theo giá trị `.match.json` (khoảng cách giữa giá trị đo và thời điểm phơi sáng) và `.cameras.json` (thuộc về giá trị đo nào), và được báo cáo trên dòng `DAQ:`. Điều này áp dụng cho mọi cấp độ, bao gồm cả ảnh thô hoặc ảnh đã tách màu, do đó bất kỳ ảnh nào cũng có thể được xử lý lại thành độ sáng, độ phản xạ hoặc chỉ số sau này. Đó là cùng một tệp, với cùng tên và tệp phụ, bất kể ảnh chụp đến từ GUI, CLI hay SDK. Nếu hoàn toàn không có giá trị DAQ thì sẽ không có `.daq`, và một `multi-capture` không có mảng kết nối (cơ chế dự phòng tạm thời) sẽ không lưu lại dữ liệu nào.

| Cờ | Tác dụng |
| --- | --- |
| `--no-daq` | Không lưu giá trị đọc trên một lần ghi dữ liệu mà các mức của nó không sử dụng cờ này (`raw`, `debayered`). Các mức `radiance`, `reflectance` hoặc `display` vẫn ghi giá trị này, vì quá trình nhập lại cần giá trị đo được dùng để tạo ra sản phẩm đó. |
| `--daq-tolerance-ms MS` | Khoảng cách tối đa mà giá trị đo có thể lệch so với giá trị phơi sáng mà vẫn được coi là nằm trong giới hạn cho phép (mặc định là 500). Chỉ kết quả đánh giá là thay đổi (đối với `.match.json`, `in_tolerance` và `tolerance_ms`, cũng như đối với độ phản xạ `DlsInTolerance`), chứ không bao giờ thay đổi giá trị đo được sử dụng. Xem [Giá trị đo DAQ cách xa giá trị phơi sáng](#a-daq-reading-far-from-the-exposure). |
| `--force-daq` | Cách cũ để yêu cầu giá trị `.daq` trên bản ghi chỉ định dạng thô. Hiện nay đây là thiết lập mặc định, và `lattice capture` cùng `array-capture` vẫn chấp nhận cờ này (`multi-capture` thì không). `--no-daq` sẽ được ưu tiên hơn. |

DAQ nào: DAQ được liên kết với máy ảnh hoặc mảng cảm biến (lựa chọn cảm biến ánh sáng trên giao diện người dùng, liên kết đã lưu của dự án, hoặc `chloros-cli daq pool-bind`), nếu không thì sẽ là DAQ mới nhất mà hệ thống nền có. Một loạt ảnh thô (`array-burst`) vẫn giữ tệp `.daq` riêng cho mỗi lần đọc riêng biệt trong thư mục loạt ảnh như trước đây.

**Các định dạng PNG và JPEG chứa 8 bit trên mỗi kênh, do đó chúng chỉ sử dụng các tệp `display` / `preview` và các sản phẩm chỉ mục.** Các tệp `--format png` hoặc `jpg` có mức độ mà chúng không thể lưu trữ sẽ bị từ chối trước khi bất kỳ dữ liệu nào được ghi lại, đồng thời ghi rõ mức độ đó và định dạng TIFF dùng để lưu trữ nó:

- `radiance` (float32) và `reflectance` (uint16, `32768` = 1.0) — luôn luôn như vậy, do đó `--processing all` (mặc định) cùng với `--format png`/`jpg` cũng bị từ chối: sử dụng `--format tiff` hoặc `--levels display`.
- `raw` và `debayered` từ camera truyền dữ liệu với độ sâu bit lớn hơn 8 bit (các khung hình 12-bit M3C / Mono12 M3M) — CLI không thể xác định độ sâu bit của camera, do đó hệ thống phía sau từ chối các yêu cầu này (HTTP 400). Khung hình đơn`lattice capture --processing debayered` chỉ lấy một khung hình 8-bit cho định dạng PNG/JPEG và không bị từ chối, cũng như `multi-capture` không có mảng lưu trữ (đã tách pixel 8-bit).

Mọi đường dẫn ghi hình đều trả lời giống nhau (`error_code` `FORMAT_CANNOT_HOLD_LEVEL`): `lattice capture`, `multi-capture`, `array-capture`, `project capture` / `burst`, cùng với các tuyến HTTP phía sau chúng.

### Cấu trúc của thư mục ghi hình

Mỗi loại xuất dữ liệu sẽ được lưu vào **thư mục con riêng**dưới thư mục `-o`, do đó, trong quá trình ghi hình nhiều cấp độ, các loại dữ liệu sẽ không bao giờ bị trộn lẫn. Bản phác thảo minh họa**quá trình ghi hình mặc định `all` của một camera bởi `lattice capture`** — mỗi camera có nhiều cấp độ, do đó mỗi tệp đều mang hậu tố `_<level>`:

```
output/
├── raw/           capture_<ts>_SN<serial>-<filter>_raw.tiff
├── debayered/     capture_<ts>_SN<serial>-<filter>_debayered.tiff
├── radiance/      capture_<ts>_SN<serial>-<filter>_radiance.tiff
├── reflectance/   capture_<ts>_SN<serial>-<filter>_reflectance.tiff
├── preview/       capture_<ts>_SN<serial>-<filter>_display.tiff
├── index/         per-camera vegetation-index (LUT) render, when --index is on
├── composite/     array foreground/background live-view composite, when produced
├── clamp_masks/   radiance/…_radiance_clampmask.tif, reflectance/…, index/… — the unmix clamp
│                  masks, one subfolder per product folder, only when a band clamped
└── daq/           daq_data_<sensor id>_<ts>.daq: the downwelling reading matched to the capture
                   (+ .daq.cameras.json: whose)
```

Cách tên tệp thay đổi:

- **Trường camera — số sê-ri, sau đó là bộ lọc:** mỗi tệp ghi hình đều đặt tên camera đã chụp là `SN<serial>-<filter>`, trong đó `<filter>` là phần bộ lọc của mẫu camera: `SN261902142-F450` đối với `M3M-L87-F450`, `SN213800234-FRGN` cho `M3C-L87-FRGN`, `SN214701292-FRGB` cho máy ảnh RGB. Phần mã lọc được ghép với số sê-ri thành `-`, chứ không phải `_`, do đó tên tệp vẫn có các trường được phân tách bằng `_` như trước đây (tiền tố, ngày, giờ, mili giây, camera, mức độ). Một camera không thể đọc được mẫu (DeviceUserID đã bị xóa) sẽ được đặt tên đơn thuần là `SN<serial>`. Các bản ghi từ các phiên bản trước không có bộ lọc trong tên; Chloros đọc cả hai loại.
- **Tiền tố:** `lattice capture` đặt tên cho các tệp là `capture_…`; `array-capture`, `multi-capture` khi có mảng liên kết, và `project capture` / `burst` / `stream` của một mảng sẽ được đặt tên là `sync_<ts>_SN<serial>-<filter>…`.
- **Chỉ có hậu tố cấp độ khi có nhiều cấp độ.**Máy ảnh lưu**nhiều**cấp độ trong một lần chụp (`all` hoặc `--levels a,b`) sẽ thêm `_<level>`. Máy ảnh lưu**một** mức sẽ giữ nguyên tên cơ bản, không có hậu tố — `--processing reflectance` sẽ ghi thành `reflectance/sync_<ts>_SN<serial>-<filter>.tiff` — do đó các tập lệnh được viết cho các lần chụp một mức vẫn hoạt động bình thường. Mức độ vẫn được xác định bởi thư mục.
- **Mặt nạ giới hạn đi kèm với tên sản phẩm:** `<product name>_clampmask.tif`. Một bản chụp phản xạ một cấp sẽ ghi thành `clamp_masks/reflectance/sync_<ts>_SN<serial>-<filter>_clampmask.tif`; một bản chụp `all` sẽ ghi thành `clamp_masks/reflectance/sync_<ts>_SN<serial>-<filter>_reflectance_clampmask.tif`.
- **Phần mở rộng** đi sau `--format`: giá trị mặc định `tiff` sẽ tạo ra `.tiff`, `--format tif` tạo ra `.tif` (mặt nạ giới hạn luôn là `.tif`).
- **Một trường hợp ngoại lệ:** `lattice capture --processing debayered` (hoặc `--levels debayered`) nhận một đường dẫn khung đơn nhanhvà ghi trực tiếp `capture_<ts>_SN<serial>-<filter>.tiff` vào `-o` — không qua thư mục con.
- **Dòng dữ liệu và chuỗi dữ liệu** sử dụng cùng một trường: `lattice continuous` ghi `frame_<seq>_SN<serial>-<filter>.tiff`, `lattice multi-stream`, `stream_<seq>_SN<serial>-<filter>_<frame id>.tiff` và một loạt ảnh thô `<burst>_f<frame>_SN<serial>-<filter>_raw.tiff`.

`<ts>` là dấu thời gian ghi hình và `<serial>` là số sê-ri máy ảnh, do đó một nhóm đồng bộ sẽ chia sẻ cùng một
dấu thời gian trên các máy ảnh. **Lưu ý điểm bất đối xứng duy nhất:** cấp độ `display` được lưu trữ trong một thư mục
có tên `preview/`, trong khi các tệp đa cấp giữ `_display` trong tên — tên thư mục và hậu tố chỉ khác nhau
đối với cấp độ đó. Các cấp độ không xác định sẽ được lưu vào một thư mục mang tên của chính chúng, và nếu thư mục con
không thể được tạo, tệp sẽ được ghi vào thư mục gốc đầu ra thay vì bị mất.

**Xử lý lại một thư mục ghi hình:**chỉ định `chloros-cli process` vào**thư mục gốc captures**
(`output/`). `process` thông thường chỉ nhập thư mục mà bạn chỉ định, nhưng khi thư mục đó không chứa
hình ảnh nào nhưng có các thư mục con, nó sẽ tự động đi xuống các thư mục con — do đó, các thư mục con cùng cấp và
thư mục con `daq/` đều được nhập cùng một lúc. Mỗi cấp độ của một bản chụp sẽ được nhập dưới dạng một hình ảnh duy nhất, với
các cấp độ khác có sẵn dưới dạng chế độ, thay vì một hình ảnh cho mỗi cấp độ.

Việc đặt tên trực tiếp cho **thư mục con cấp độ** (ví dụ: `output/raw/`) cũng hoạt động. Làm như vậy sẽ bỏ lại `daq/`, vì vậy hãy sao chép hoặc chỉ định dữ liệu DAQ đi kèm khi bạntạo sản phẩm đo xạ
từ `raw/` — nếu không, việc khớp dấu thời gian sẽ không có dữ liệu nào để đối chiếu.

**Quá trình xử lý luôn bắt đầu từ `raw`.** Trong mỗi lần chụp, khung hình thô là nguồn đầu vào của quy trình xử lý;
`debayered`, `radiance`, `reflectance` và `preview` xuất hiện dưới dạng các chế độ hiển thị nhưng không bao giờ được đưa
trở lại qua đường ống xử lý. Việc xử lý lại một sản phẩm phái sinh sẽ áp dụng lại hiệu ứng vignette, CCM và
các phép tính radiance đã được tích hợp sẵn vào các pixel của nó, do đó Chloross từ chối thay vì
xử lý hai lần. Hai hậu quả đáng lưu ý:

- Các bản render `index/` và `composite/` **không bao giờ** được xử lý. Chúng là đầu ra, không phải dữ liệu chụp —
  một bản hiển thị LUT NDVI không có thông tin radiance có ý nghĩa.
- Một thư mục dữ liệu chụp được xuất **không có** `raw` (ví dụ: `array-capture --processing reflectance`) không có
  không có nguồn hợp lệ trong quy trình xử lý. Các bản ghi này vẫn nhập và hiển thị bình thường, nhưng `process` sẽ bỏ qua
  chúng và thông báo như sau:

  ```
  [IMPORT-LEVEL] Skipping 4 already-processed file(s) with no raw source: capture_…_reflectance.tif
  [IMPORT-LEVEL] Processing starts from raw. Re-capture with --processing raw, or force an entry
                 point with --input-level.
  ```

  Nếu bạn thực sự cần đưa một sản phẩm phái sinh qua — một phiên hub được ghi lại với
  `demosaic` được bật, hoặc một thư mục cũ — `--input-level {raw,debayered,processed}` sẽ ép buộc điểm vào
  và bỏ qua thiết lập bỏ qua. Cờ này là lối thoát được thiết kế sẵn; `auto` (mặc định)
  không bao giờ xử lý bản ghi không có dữ liệu thô.

**Thẻ hiệu chuẩn: chỉ trên `raw`.** Tệp `raw` của một lần ghi dữ liệu chứa gói hiệu chuẩn của camera: phần dữ liệu hữu ích (payload) `CalibrationJSON` và các thẻ tương thích với MicaSense (`BlackLevel`, `RadiometricCalibration`, `Band{R,G,B}RadiometricCalibration`, `MicaSenseVignetting*`), mô tả các giá trị thô của cảm biến. Tất cả các tệp ở các cấp độ khác (`debayered`, `display`, `radiance`, `reflectance`, bản render `index/`) và mỗi khung hình `lattice continuous` / `multi-stream` đều chứa thông tin nhận dạng của camera (`XMP-mapir:CameraSerial`, `CameraModel`, `FilterType`, `LensModel`, `CalBundleSha256`, ...) cùng với bản ghi riêng của khung hình, `XMP-mapir:FrameJSON` (thời gian phơi sáng, độ khuếch đại, nhiệt độ, định dạng pixel, thời gian chụp, sự khớp DAQ), vì mỗi sản phẩm `process` đều mang mã định danh này. `process` tìm bản hiệu chuẩn của tệp đó dựa trên số sê-ri và `CalBundleSha256` trong bộ nhớ đệm hiệu chuẩn củChloros, tải xuống gói này một lần nếu máy tính chưa có.

#### Tệp được tạo từ nhiều máy ảnh

Một tệp được tạo từ các dải phổ của nhiều máy ảnh — một chồng dải phổ đa kênh (`align-apply`, `align-stream`, `index --live --save-multiband`), một chỉ mục trực tiếp được tạo từ nhiều camera (`index.tif`, `index_LUT.png`), hình ảnh `composite/` từ việc chụp mảng — liệt kê tên từng camera đã được sử dụng:

- **Danh sách theo từng camera, theo thứ tự dải màu** (hình ảnh tổng hợp: lớp nền, sau đó là lớp tiền cảnh; một lớp chỉ mục, các camera mà các dải màu của nó xuất phát từ): `XMP-mapir:SourceCameraSerials`, `SourceCameraModels`, `SourceSensorProfiles`, `SourceProductLines`, `SourceFilterTypes`, `SourceLensModels`, `SourceCalDateUTCs`, `SourceCalBundleSha256s` (gói hiệu chuẩn mà các dải phổ của từng máy ảnh đã đi qua, nếu không — một bộ `align-apply` / `align-stream`, các dải của bộ này không trải qua bất kỳ bước hiệu chuẩn nào — gói mà Chloros đã lưu vào bộ nhớ đệm cho camera, chính là tên của tệp ghi hình), `SourceLightboxCertIds`, `SourceHalogenLampCertIds`, `SourcePackTiers` — các danh sách song song, mục *n* của mỗi danh sách mô tả cùng một máy ảnh; `-` khi máy ảnh không có giá trị (không có gói hiệu chuẩn, ống kính không xác định). Mỗi máy ảnh là **một** mục: nếu gói dữ liệu của nó có nhiều giá trị cho một trường — chẳng hạn như các chứng chỉ đèn halogen của gói — chúng được nối với nhau bằng `;` (`F-1479-1;F-1480-1;F-1481-1`), không bao giờ dùng dấu phẩy, do đó danh sách sẽ hiển thị một mục cho mỗi máy ảnh dù được in ra như thế nào.
- **EXIF `SerialNumber`** chứa cùng các số sê-ri, được nối bằng dấu phẩy (`214000535,214000551`).
- Nó **không** chứa bất kỳ thông tin nhận dạng máy ảnh đơn lẻ nào (`XMP-mapir:CameraSerial`, `CalBundleSha256`, …): các mã này chỉ định một máy ảnh và gói dữ liệu của nó, và `process` sẽ xác định thông số hiệu chỉnh của tệp dựa trên các thông tin đó. Một chỉ số được tính toán từ các dải dữ liệu của nhiều máy ảnh (`index.tif`, `index_LUT.png`) có thông tin EXIF `Model` `LATTICE` (bộ thiết bị), chứ không phải mô hình `LATT-…` của một máy ảnh duy nhất.

Một tệp chứa các dải màu của **một** máy ảnh — một tệp TIFF (`--per-band`), một khung hình `align-apply --per-camera`, một chồng ảnh hoặc chỉ mục trực tiếp của một máy ảnh duy nhất — chứa các thẻ nhận dạng riêng của máy ảnh đó, giống như mọi tệp chụp khác. Các tệp chồng ảnh, tệp theo dải và tệp theo máy ảnh của `align-apply`, cùng với mỗi chồng ảnh `align-stream` và các tệp theo dải của nó, đều chứa thời gian chụp của nhóm đó (thời gian phơi sáng của máy ảnh tham chiếu, theo UTC), và khung hình `--per-camera` của camera đơn sắc là một mặt phẳng, tại `--bit-depth` giống như bộ xếp chồng.**Hướng camera (lật / xoay).** Cài đặt hướng của camera (các tùy chọn Mirror H /
Mirror V / xoay) quyết định nội dung được lưu khi chụp:

- **`raw` và `debayered` được lưu chính xác như cách cảm biến đọc chúng**, bất kể cài đặt nào. Một
  Mô hình khảm Bayer bị xoay hoặc lật sẽ tạo thành một mẫu bộ lọc màu khác, và các bản đồ hiệu chuẩn
  (dark offset, flat field) nằm trên lưới riêng của cảm biến.
- **`preview`/`display`, `radiance`, `reflectance` và bản render `index/` được định hướng** — xoay
  một lần, sau khi hoàn tất tất cả các phép tính hiệu chuẩn, để chúng mở ra theo cách máy ảnh đã được lấy khung. Các tệp
  `clamp_masks/…_clampmask.tif` nằm trên cùng một lưới.
- **Mỗi tệp ảnh đều ghi lại cài đặt** trong XMP: `XMP-Camera:ChlorosFlipHorizontal` /
  `ChlorosFlipVertical` (`true`/`false`), `ChlorosRotation` (0 / 90 / 180 / 270, theo chiều kim đồng hồ, sau
  các thao tác lật) và `ChlorosOrientationApplied` — `false` trên dữ liệu thô và dữ liệu đã tách kênh (pixel gốc),
  `true` trên các sản phẩm đã định hướng. `ChlorosProcessing` liệt kê các thao tác được áp dụng cho các
  pixel của tệp đó (`none` khi ở dạng gốc). Điều này bao gồm mọi khung hình mà `lattice continuous` truyền (cài đặt
  của chính máy ảnh, chưa được áp dụng) và mọi khung hình của `lattice multi-stream` (khung hình gốc: các
  khung hình thành phần của nó là gốc, giống như trong chụp mảng). Các tập hợp `lattice align-stream` / `align-apply`
  ghi lại cài đặt của máy ảnh chính, chưa được áp dụng: các dải của chúng được biến dạng vào khung hình của máy ảnh chính
  nhưng không bị xoay.
- **`process` áp dụng hướng đã ghi lại** khi xử lý dữ liệu thô: bù tối, hiệu chỉnh
  trường phẳng, khử mô-saic và tách kênh trong tọa độ cảm biến trước tiên, tiếp theo là biến dạng căn chỉnh mảng, và
  hướng — do đó các sản phẩm xem trước, độ rạng rỡ, độ phản xạ và chỉ số sẽ chính xác như
  khi chụp trực tiếp, và ghi lại `ChlorosOrientationApplied=true`. Các sản phẩm dự phòng chưa được hiệu chuẩn
  (`Sensor_Response_Images`, `Vignette_Corrected_Images`) được định hướng theo cùng một cách, sau
  hình nền phẳng. Sản phẩm đã loại bỏ hiệu ứng Bayer giữ nguyên định dạng gốc.
- **Trình xem hình ảnh Chloros hiển thị tệp thô hoặc tệp đã loại bỏ hiệu ứng Bayer được xoay theo hướng ghi** (chỉ hiển thị —
  tệp không bị thay đổi), do đó trông giống như các sản phẩm của nó; hình thu nhỏ, chế độ xem kích thước đầy đủ,
  đọc dữ liệu con trỏ và hộp cát Index/LUT đều sử dụng cùng một hướng xoay. Một tệp không có bản ghi và
  một sản phẩm (đã được định hướng) được hiển thị như khi được lưu trữ. Việc xuất sang vùng thử nghiệm ghi lại hướng mà các
  pixel mang theo (`ChlorosOrientationApplied=true`; mã định danh cho một lớp không có thông tin định hướng).
- **Các bản chụp không có bản ghi sẽ được xử lý dưới dạng gốc** (hành vi không thay đổi). Trước phiên bản 1.3.0, một
  bản chụp từ một máy ảnh duy nhất có thiết lập lật hoặc xoay sẽ tự động xoay các tệp thô và tệp đã loại bỏ hiệu ứng Bayer;
  Chloros không thể phân biệt được tệp thô đã được- so với tệp gốc và không đoán, do đó nó
  xử lý theo đúng định dạng lưu trữ. Các bản đồ hiệu chuẩn nằm trên lưới riêng của cảm biến, vì vậy đối với trường hợp
  chụp như vậy, hãy sử dụng bản chụp bằng phiên bản 1.3.0 khi hiệu chuẩn là yếu tố quan trọng.
- **Theo mặc định, các mảng không xoay bất kỳ tệp thành viên nào** (sự biến dạng căn chỉnh và tệp XMP `Alignment*` nằm trong
  khung hình gốc của chính máy ảnh, chưa được xoay) và ghi lại thông tin nhận dạng. `array-capture --orient` xoay các
  kết quả của từng thành viên, sau khi biến dạng căn chỉnh: theo hướng của **khung chính** khi chúng được
  biến dạng vào khung của nó (để các thành viên vẫn được đăng ký chung), nếu không thì theo hướng của chính máy ảnh của chúng. Mỗi
  tệp ghi lại hướng của khung hình mà các pixel của chính nó nằm trong — một tệp thô, theo hướng của chính camera đó —
  và một tệp thô chứa phép biến đổi căn chỉnh cũng ghi lại `ChlorosAlignedFlipHorizontal` /
  `ChlorosAlignedFlipVertical` / `ChlorosAlignedRotation`: của khung chính, đối với sản phẩm được biến dạng
  vào khung hình của máy ảnh chính. `process` xoay từng sản phẩm theo giá trị thuộc về các pixel của nó:
  giá trị đã căn chỉnh khi nó biến dạng tệp thô (`--array-alignment`, mặc định — các sản phẩm của quá trình chụp được căn chỉnh trực tiếp), sản phẩm của chính máy ảnh khi không căn chỉnh (`--no-array-alignment` — các
  kết quả chụp trực tiếp chưa được căn chỉnh). Tùy chọn tương tự cũng có sẵn cho `array-burst`, `project capture` / `burst` /
  `stream`, các công thức (recipes) và SDK.

### Các lần chụp bị bỏ qua trong mảng camera hỗn hợp

Khi bạn kết hợp các camera RGB và đa quang phổ trong một mảng, `array-capture --processing radiance` (hoặc `reflectance`) sẽ lưu các khung hình đa quang phổ và **bỏ qua** các camera RGB — độ rạng rỡ theo từng điểm Bayer không có ý nghĩa đối với cảm biến băng thông rộng. CLI sẽ hiển thị rõ ràng từng tệp được lưu (kèm mức độ xuất), từng tệp `.daq` được ghi và từng trường hợp bỏ qua, do đó số lượng tệp không gây ngạc nhiên:

```
  Saved: …/output/reflectance/sync_…_SN213800234-FRGN.tiff [reflectance] (SN:213800234, fid:1)
  Saved: …/output/reflectance/sync_…_SN214000533-FOCN.tiff [reflectance] (SN:214000533, fid:1)
  Saved: …/output/reflectance/sync_…_SN214701288-FNGB.tiff [reflectance] (SN:214701288, fid:1)
  DAQ:   …/output/daq/daq_data_daq-e-54b5e0_….daq
  Skipped: SN:214701292 (reflectance-not-applicable-to-rgb-cam filter=RGB)

  3 synchronized frames captured. (1 skipped)
```

Các mã lý do bỏ qua tuân theo mẫu `<level>-not-applicable-to-rgb-cam`. Một camera không được tải gói hiệu chuẩn sẽ bỏ qua `radiance` / `reflectance` với `<level>-needs-calibration (connect the camera so its cal pack loads)`, và khi không có giá trị phản xạ DAQ có thể sử dụng, nó sẽ bỏ qua với mã `reflectance-skipped-no-dls`. Giá trị phản xạ cũng có thể bị bỏ qua với các mã `reflectance-skipped-no-fresh-dls` / `reflectance-skipped-bound-daq-unavailable (…)`, và với `dls-uncalibrated-band-<nm>` khi dải sóng chủ yếu nằm ngoài phạm vi hiệu chuẩn quang phổ của cảm biến ánh sáng DAQ (~374–974 nm) — trong số các SKU đang được phân phối, chỉ có F988, với quy trình làm việc được hỗ trợ là quy trình bảng phản xạ.

Sử dụng `--processing debayered` (hoặc `display`) để bao gồm mọi camera bất kể loại bộ lọc, hoặc giá trị mặc định `all` để thu được mọi mức áp dụng cho từng camera trong một lần chụp.

### Giá trị DAQ cách xa giá trị phơi sáng

Mỗi giá trị `.daq` được lưu khi chụp là giá trị DAQ tương ứng với giá trị phơi sáng đó, và giá trị `.match.json` bên cạnh ghi lại khoảng cách giữa hai giá trị này (`gap_ms`) và liệu khoảng cách đó có nằm trong giới hạn cho phép (`in_tolerance`, 500 ms trừ khi bộ chụp ảnh đã thiết lập `--daq-tolerance-ms`). Giá trị nằm ngoài giới hạn này vẫn được sử dụng, do đó không có khung hình nào bị mất, và nó được gắn nhãn: tệp phản xạ (reflectance file) chứa các thông tin XMP là `DlsInTolerance=False` và `DlsMatchGapS`. `lattice capture`, `multi-capture` và `array-capture` cũng ghi giá trị này dưới mục `.daq` liên quan, kèm theo lỗi luồng dữ liệu DAQ'khi giải thích như sau:

**Cách một giá trị đo được khớp với thời gian phơi sáng.** Quá trình ghi dữ liệu sẽ chờ giá trị đo đầu tiên từ DAQ sau khi kết thúc phơi sáng, để có thể kết hợp các giá trị đo ở hai bên của nó (`method` `interpolated`). Một DAQ-E gửi giá trị đo mỗi 0,4 giây vào ban ngày và mỗi 1–2 giây trong nhà, do đó thời gian chờ được cộng thêm khoảng một rưỡi khoảng thời gian đó vào lệnh, chứ không phải vào thời gian phơi sáng. Một DAQ đã ngừng gửi dữ liệu sẽ không được chờ đợi. Giá trị đo-E là giá trị ánh sáng trung bình trong khoảng thời gian kể từ lần đọc trước đó, do đó nó được đặt ở giữa khoảng thời gian đó: `gap_ms` được tính từ thời điểm phơi sáng đến điểm giữa đó, và `bracket_stamps_ns` cung cấp dấu thời gian riêng của các giá trị đọc. `array-capture --continuous` chờ mà không làm chậm quá trình chụp: các tệp thô của nó được ghi ngay lập tức và các giá trị `.daq` được cập nhật ngay khi giá trị đọc đến. Với `--interval`, mỗi lần quét sẽ chờ như một lần chụp riêng lẻ.

```
  DAQ:   …/output/daq/daq_data_daq-e-54b774_….daq
         DAQ reading 114.9 s from the exposure (tolerance 500 ms): reflectance uses it, labelled DlsInTolerance=False
         DAQ stream fault: no frame accepted from daq-e-54b774.local in 25 s, after 1932 good frame(s). …
```

Khi không có tệp phản xạ nào được ghi từ lần đọc đó, dòng đầu tiên sẽ kết thúc bằng `saved anyway; its .match.json says in_tolerance: false` thay thế.

Quá trình thu thập vẫn kết thúc bằng `0`: mọi tệp đã được ghi và ghi rõ nội dung của chúng. Một thiết bị DAQ có luồng dữ liệu bị đình trệ vẫn tiếp tục cung cấp lần đọc cuối cùng, do đó hãy kiểm tra bằng `chloros-cli daq pool-list` trước khi dựa vào giá trị phản xạ của lần thu thập đó. Hệ thống backend sẽ kết nối lại với thiết bị DAQ-E đang hoạt động theo chế độ pool đã ngừng cung cấp dữ liệu (xem [Một DAQ-E Ngừng Cung Cấp Dữ Liệu](#a-daq-e-that-stops-delivering)). Một giá trị đo nằm ngoài ngưỡng cho phép chỉ hiển thị dòng `DAQ:`, trừ khi đó là giá trị được giữ lại (xem phần dưới).

**Một giá trị được giữ lại sau khi DAQ của nó ngừng hoạt động.** Khi không có DAQ nào có thể cung cấp giá trị cho một lần phơi sáng (DAQ được liên kết không khả dụng, hoặc không có DAQ trong nhóm nào có giá trị, ví dụ: sau `daq pool-disconnect`), quá trình thu thập sẽ tái sử dụng giá trị đo cuối cùng mà Chloros có, và dòng `.match.json` mô tả giá trị đo đó đối với lần phơi sáng này: `method` `retained`, `gap_ms` từ lần phơi sáng này đến các mẫu mà giá trị đo được lấy từ đó, `in_tolerance` từ khoảng trống đó, và `retained_from_epoch_ns`, lần phơi sáng mà giá trị đo được khớp lần đầu tiên. Do đó, một lần ghi dữ liệu được thực hiện rất lâu sau khi hệ thống thu thập dữ liệu (DAQ) ngừng hoạt động sẽ in ra các dòng trên, và dưới chúng một dòng nữa, dòng này được in ngay cả khi giá trị đo vẫn nằm trong giới hạn cho phép:

```
         This is the DAQ's last reading: the DAQ is no longer reporting, so each capture reuses it until a new one arrives
```

Mã `.daq` vẫn chỉ định thiết bị DAQ đã thực hiện phép đo, cùng với thông số hiệu chuẩn và nắp bảo vệ được sử dụng khi thực hiện phép đo đó, do đó khi nhập lại dữ liệu, hệ thống sẽ biết chính xác thiết bị DAQ, thông số hiệu chuẩn và nắp bảo vệ nào đã tạo ra dữ liệu đó.

### Các tệp không thể ghi

Một camera không thể ghi tệp — do ổ đĩa đầy, lỗi quyền truy cập — sẽ không ngăn cản việc lưu tệp của các camera khác. Mỗi lệnh chụp (`lattice capture`, `multi-capture`, `array-capture`; `project capture`, `burst`, `stream`; và các lệnh `capture`, `burst` và `stream` trong công thức) sẽ in tên từng tệp không lưu được ra stderr, sau đó **thoát khỏi `1`** mặc dù phần còn lại đã được lưu:

```
  Failed: SN:214701292 [reflectance] OSError: [Errno 28] No space left on device
  Disk full: the drive the backend captures to has no space left. Free space on it (or capture to another drive) before capturing again.
```

Tất cả các tệp mà một lần ghi hình cần đều được xử lý, bao gồm cảkhung hình đã được loại bỏ hiệu ứng Bayer (`lattice capture --levels debayered`) cũng được bao gồm. Một tệp đọc `.daq` mà tệp sidecar nguồn gốc `.match.json` không thể được ghi sẽ được liệt kê dưới dạng `[match]` (bản thân tệp `.daq` đã được lưu).

Trên ổ đĩa đầy, các bản chụp lặp lại (`array-capture --continuous` / `--interval`, `project burst` / `stream`, một công thức `burst` / `stream`) **sẽ dừng lại tại lượt quét đó** thay vì không ghi gì vào cùng ổ đĩa trong phần còn lại của quá trình chạy. Bất kỳ lỗi nào khác đều được báo cáo ở mỗi lượt quét mà lỗi xảy ra, và quá trình chạy tiếp tục đến hết, sau đó thoát khỏi `1`. `array-capture --continuous` (quá trình ghi liên tục của backend) không liệt kê bất kỳ tệp nào, kể cả các tệp bị lỗi: nó đếm chúng trong các dòng tiến trình và tóm tắt (`N failed`, `N file(s) failed to write.`) và thoát khỏi `1` — xem [Chế độ liên tục](#continuous-mode---continuous).

Các trình ghi — `array-burst` và `array-record` — báo cáo các lỗi riêng của chúng. `array-burst` tự động dừng khi ổ đĩa đầy hoặc ngay trước khi đầy. Cả hai đều tiếp tục hoạt động bất chấp bất kỳ lỗi ghi nào khác. Khi một trong hai báo cáo lỗi, thông báo lỗi sẽ được hiển thị sau các bộ đếm, và lệnh **thoát khỏi `1`**:

```
  Recorder error: disk-full: OSError: [Errno 28] No space left on device
  Disk full: the drive the backend captures to has no space left. Free space on it (or capture to another drive) before capturing again.
```

`Disk low:` thay vì `Disk full:` có nghĩa là quá trình ghi liên tục dừng lại trước khi lấp đầy ổ đĩa. Các khung hình được ghi trước khi dừng sẽ được giữ lại. `array-burst --build` sẽ bỏ qua quá trình xây dựng khi hết dung lượng, vì các video sẽ được ghi vào cùng một ổ đĩa. Hãy giải phóng dung lượng trước, sau đó chạy `array-build-video --burst-dir …`. Sau bất kỳ lỗi nào khác, quá trình xây dựng vẫn tiếp tục chạy.

Một trường hợp `array-record` không ghi được khung hình nào cũng được coi là ghi hình thất bại (`Recorder error: no-frames: the combined view published no frame to record …`, thoát khỏi `1`) — xem [`array-record`](#array-record--combined-index-videogif-monitoring-grade).

---

## Chế độ ghi hình, Trình ghi và Xử lý lại ngoại tuyến

Tất cả các tính năng này đều hoạt động trên một **mảng lưu trữ** (chạy lệnh `array-connect` trước tiên). Chúng phản ánh bảng điều khiển ghi hình trên giao diện người dùng (GUI).

### Các chế độ của `array-capture`

`array-capture` là một lệnh duy nhất với bốn chế độ chụp cùng một bộ tùy chọn xuất:

| Chế độ | Cờ | Hành vi |
| --- | --- | --- |
| **Đơn lẻ** *(mặc định)* | (không có) | Một nhóm chụp đồng bộ, sau đó thoát. |
| **Liên tục** | `--continuous` | Ghi liên tiếp cho đến khi `Ctrl+C`, `--count N` ghi, `--duration S` hoặc khi ổ đĩa đầy. Chạy chế độ chụp liên tục riêng của backend (chế độ Liên tục của GUI) — xem [Chế độ Liên tục](#continuous-mode---continuous) bên dưới. |
| **Khoảng thời gian** | `--interval S` | Một lần quét mỗi `S` giây (được tính từ khi bắt đầu mỗi lần quét), cùng phạm vi. |
| **Nhanh nhất** | `--fastest` | Chỉ dữ liệu thô+ giá trị đọc DAQ được gán + chỉ số tổng hợp; bỏ qua các phép tính về độ rạng rỡ/độ phản xạ/hiển thị để khung hình được xử lý nhanh. Ngụ ý chế độ `--processing raw --force-daq`. Xử lý lại dữ liệu đã lưu `.daq` thành các sản phẩm đã hiệu chuẩn sau này. |

Các tùy chọn xuất (có thể kết hợp với bất kỳ chế độ nào; tất cả đều sử dụng chung giao diện người dùng (GUI) và điểm cuối SDK):

| Cờ | Tác dụng |
| --- | --- |
| `--array-id ID` | Mảng cần ghi lại (bỏ qua nếu chỉ có một thiết bị được kết nối). |
| `-o, --output DIR` | Thư mục đầu ra (mặc định là `output`, nằm trong thư mục mà bạn chạy lệnh từ đó). |
| `--format {tiff,tif,png,jpg,jpeg}` | Định dạng tệp (mặc định là `tiff`). PNG / JPEG chỉ hỗ trợ `display` và chỉ số render; xem [Các mức xuất dữ liệu ghi lại](#capture-export-levels-the-all-default). |
| `--processing LEVEL` | Mức xuất duy nhất (`raw`, `debayered`, `radiance`, `reflectance`, `display`), hoặc `all` (mặc định). |
| `--levels L1,L2,…` | Tập con cụ thể của các loại xuất (ví dụ: `raw,radiance,reflectance`); **ghi đè lên `--processing`**. |
| `--aligned` / `--no-aligned` | **Mặc định: căn chỉnh, giống như cách ứng dụng Chloros ghi lại cùng một mảng**(cài đặt ghi lại**Căn chỉnh** cho từng mảng của ứng dụng, được bật trừ khi bạn tắt nó đi). Mọi tệp xuất không phải dạng thô của từng thành viên (`debayered`, `display`, `radiance`, `reflectance`, bản render chỉ mục) đều được biến dạng vào khung hình của camera chính và được cắt xén theo vùng chồng chéo chung của các thành viên, do đó các tệp được căn chỉnh theo pixel và có kích thước, vùng cắt xén cùng các thẻ `Alignment*` giống như của ứng dụng — nhỏ hơn cảm biến (hoặc vùng ROI) do độ lệch của các thành viên; trong trường hợp vùng ROI, phép biến đổi được căn chỉnh lại theo khung cửa sổ. `XMP-mapir:AlignmentJSON` chỉ định `"applied": true` và cung cấp `crop_roi`. Dữ liệu thô vẫn giữ nguyên kích thước gốc mà không bị biến dạng nhưng mang theo thông tin biến đổi trong siêu dữ liệu (`"applied": false`). `--no-aligned` lưu mọi tệp ở kích thước gốc, không biến dạng. `--aligned` là giá trị mặc định được chỉ định rõ. Dù biến dạng có được áp dụng hay không, mỗi tệp thành viên đều ghi lại phép biến đổi `Alignment*` của máy ảnh tương ứng ngay khi mảng có hồ sơ căn chỉnh — ngoại trừ máy ảnh mà hồ sơ không thể căn chỉnh (bị loại trừ hoặc bị ép về vị trí gốc do giải quyết thất bại), và máy ảnh tham chiếu khi không có máy ảnh nào khác được căn chỉnh: những máy ảnh này không mang thẻ `Alignment*`. Nếu mảng không có hồ sơ, dữ liệu chụp sẽ được lưu ở trạng thái chưa căn chỉnh, và CLI sẽ hiển thị lý do trên dòng `Alignment:`, giống như trong mọi trường hợp khác mà quá trình biến dạng không thể được áp dụng (các thành viên có độ phân giải khác nhau, mảng không được đồng bộ hóa cáp, khung hình mà hồ sơ không thể được ánh xạ vào). Tính năng tự động căn chỉnh của mảng sẽ tạo hồ sơ ngay khi độ phơi sáng ổn định; `align-calibrate` tạo một cấu hình theo yêu cầu. |
| `--orient` | Chuyển đổi các **sản phẩm**của từng thành viên — `display`, `radiance`, `reflectance`, bản render `index/` và `composite/` — theo hướng của máy ảnh, sau khi biến dạng căn chỉnh: một sản phẩm được biến dạng vào khung hình chính (mặc định) theo hướng**hướng của khung hình chính**, do đó các thành viên vẫn được đăng ký chính xác; một tệp chưa được căn chỉnh (`--no-aligned`, hoặc một bản chụp mà quá trình biến dạng không thể áp dụng được) sẽ dựa trên hướng của chính máy ảnh đó.**Mặc định: tắt — mọi tệp thành viên vẫn giữ định dạng gốc**, giống như các bản chụp mảng luôn có. `raw` và `debayered` vẫn giữ định dạng gốc trong cả hai trường hợp. Mỗi tệp đều ghi lại hướng mà các sản phẩm của nó mang theo (xem [Hướng máy ảnh](#what-a-captures-folder-looks-like)); tệp XMP của `Alignment*` vẫn giữ biến đổi khung hình gốc, được áp dụng trước hướng, do đó `process` (biến dạng, sau đó là hướng) sẽ tái tạo chính xác sản phẩm gốc. |
| `--index` / `--no-index` | Lưu / bỏ qua lớp phủ chỉ số thảm thực vật cho từng camera khi có cấu hình. Mặc định: hiển thị lớp phủ này. |
| `--no-daq` | Bỏ qua tệp sidecar `.daq` khi không có cấp độ nào được chọn sử dụng DAQ (chỉ lấy dữ liệu thô). Theo mặc định, dữ liệu đo được lưu lại, do đó các khung hình có thể được xử lý lại thành phản xạ/chỉ số ngoại tuyến. `--force-daq` vẫn được chấp nhận (mặc định). |
| `--daq-tolerance-ms MS` | Độ dung sai khớp DAQ cho các lần chụp này (mặc định 500 ms). |
| `--smart` | Chờ AE ổn định trên tất cả các camera trước khi kích hoạt (xem [Smart-AE / Smart-Capture](#smart-ae--smart-capture)). |
| `--compression {deflate,none}` | Nén pixel TIFF. `none` (mặc định từ phiên bản 1.3.0; phiên bản 1.2.0 ghi là `deflate`) = không nén — các pixel của cảm biến được ghi lại chính xác như khi đọc, ~6,3 MB cho mỗi khung hình độ phân giải đầy đủ, và tốc độ ghi liên tục nhanh nhất; `deflate` = nén zlib L1 không mất dữ liệu + bộ dự đoán theo chiều ngang, ~4,1 MB cho mỗi khung hình, chậm hơn. Cả hai đều là định dạng không mất dữ liệu và được đọc giống hệt nhau khi nhập vào. |

> **TIFF ghi một lần + mô hình tốc độ ổn định.**Mỗi tệp được ghi**một lần**: các điểm ảnh, gói XMP, IFD0 (Thương hiệu/Mẫu/Số sê-ri máy ảnh) và các thẻ EXIF `ExposureTime` / `SerialNumber` trong một lần duy nhất, không có gì ghi đè lên sau đó. `--continuous`X ghi lại các nhóm đồng bộ liên tiếp — mỗi lần ghi sẽ lấy nhóm tiếp theo sau nhóm trước đó, bao gồm cả nhóm đã được xuất bản trong khi lần ghi đó vẫn đang lưu — do đó nó chạy với tốc độ của mảng. DEFLATE giữ GIL của Python, do đó các thao tác ghi nén**không**được thực hiện song song trên các luồng ghi riêng cho từng máy ảnh; việc ghi hình độ phân giải đầy đủ liên tục ở tốc độ cảm biến cần các thao tác ghi không nén (mặc định)**và** ổ đĩa loại NVMe (~600 MB/s liên tục cho mảng 9 camera ở ~10 fps). Cùng một tùy chọn này được hiển thị dưới tên `compression` trên `POST /api/camera/array/capture`.

```bash
# Interval timelapse: one reflectance pass every 10 s for 5 minutes
chloros-cli lattice array-capture --interval 10 --duration 300 \
  --processing reflectance -o timelapse/

# Fastest grab for a moving rig — raw + .daq now, calibrate later
chloros-cli lattice array-capture --fastest -o flightline/

# Co-registered multi-band export (aligned is the default; drop the index overlay)
chloros-cli lattice array-capture --processing reflectance --no-index -o out/

# Each member at its native size, unwarped
chloros-cli lattice array-capture --processing reflectance --no-aligned -o native/

# Continuous raw capture for 60 s, as fast as the array publishes
chloros-cli lattice array-capture --continuous --duration 60 --fastest -o flightline/
```

#### Chế độ liên tục (`--continuous`)

`array-capture --continuous` (không kèm `--interval` hoặc `--smart`) chuyển quyền điều khiển cho **chức năng ghi liên tục tích hợp sẵn của backend** — phiên làm việc mà chế độ Liên tục trên giao diện người dùng (GUI) đang chạy. Backend tự lưu từng nhóm đồng bộ hóa, với hai lần ghi hình đang diễn ra và không có yêu cầu từ CLI giữa các nhóm, do đó nó theo kịp một mảng nhanh, nơi một yêu cầu cho mỗi lần ghi hình sẽ bị tụt lại (được đo là 73–77 lần ghi hình/giây trên mảng 9 camera với vùng quan tâm (ROI) 512 pixel). Mỗi tùy chọn xuất ở trên đều áp dụng cho từng lần ghi hình.

- **Giới hạn:** `--count N` dừng sau N lần ghi hình (một lần ghi hình = một nhóm đồng bộ: các tệp của mọi thành viên); `--duration S` sau S giây. Nếu không có cả hai điều kiện trên, chương trình sẽ chạy cho đến khi đạt đến `Ctrl+C`, ổ đĩa đầy, xảy ra lỗi hoặc mảng camera ngừng hoạt động. `Ctrl+C` dừng phiên làm việc của phần nền, sau đó in bản tóm tắt.
- **Tiến trình** cứ sau 2 giây, với `, N failed` và `, N groups skipped` được thêm vào khi giá trị không bằng 0 (một nhóm bị bỏ qua là nhóm mà mảng đã công bố nhưng không có lần ghi nào thực hiện):

  ```
     12.0s  880 captures (73.3/s), 7920 files
  ```

- **Không liệt kê theo từng tệp.** Quá trình chạy kết thúc với một dòng tóm tắt — số lần ghi lại, tốc độ, tệp và thư mục đầu ra — tiếp theo là `N file(s) failed to write.` / `N group(s) skipped.` khi giá trị khác 0:

  ```
  Done — 880 captures (73.3/s), 7920 files in /home/user/flightline.
  ```

- **Dừng lại** khi ổ đĩa đầy với mã lỗi `Continuous capture stopped: the drive is full.`, và khi xảy ra lỗi hoặc mảng bị ngắt kết nối với mã lỗi `Continuous capture stopped: <reason>.` (cả hai đều hiển thị trên stderr).
- **Trạng thái thoát:** `0` khi đã lưu các bản chụp và mọi tệp đều được ghi thành công, bao gồm cả `Ctrl+C`; `1` khi có bất kỳ tệp nào không ghi được, ổ đĩa đầy, lỗi, mảng bị ngắt kết nối hoặc không có bản ghi nào cả.
- **Microsoft Defender:** khi Defender sử dụng một lõi CPU trở lên để quét các tệp mà bản ghi ghi ra, quá trình chạy sẽ hiển thị một lần trên stderr: `Note: Microsoft Defender is using N.N CPU cores scanning the capture files, CPU the capture needs. Re-run the Chloros installer with "Exclude the Chloros backend from Microsoft Defender scanning" checked.` Xem phần [Cài đặt hệ thống](#windows-exe).
- Mỗi mảng chỉ được ghi liên tục một lần; lần ghi thứ hai sẽ bị từ chối với thông báo `a continuous capture is already running for this array`. Qua HTTP, phiên làm việc có dạng `POST /api/camera/array/continuous/start` (nội dung `/api/camera/array/capture` kèm theo `max_captures` / `duration_s`), `GET /api/camera/array/continuous/status?array_id=ID` và `POST /api/camera/array/continuous/stop`.

**Interval**(`--interval S`, có hoặc không có `--continuous`) và**`--continuous --smart`**giữ vòng lặp theo từng yêu cầu: một yêu cầu thu thập dữ liệu cho mỗi lần quét — tại mỗi thời điểm đã lên lịch cho**Interval**, sau khi AE ổn định đối với `--smart` — với các tệp của mỗi lần quét được liệt kê như trong chế độ Đơn, một dòng `Done — N capture pass(es).` ở cuối và `--count N` đếm số lần quét.

### `array-record` — video/GIF chỉ mục kết hợp (cấp độ giám sát)

Ghi lại **chế độ xem chỉ số kết hợp trực tiếp** của mảng vào tệp `.avi` (và tùy chọn tệp `.gif`). Khi chế độ xem kết hợp trên giao diện người dùng đang mở, nó sẽ ghi lại chế độ xem đó như đang hiển thị. Nếu không có cửa sổ nào mở, hệ thống sẽ tự động hiển thị chế độ xem: chỉ số bạn truyền qua `--expr`, hoặc mặc định là chỉ số NDVI của mảng, sau khi mảng đã được căn chỉnh. NDVI mặc định sử dụng các dải màu đỏ và NIR gần nhất với 660 nm và 850 nm, giống như chế độ xem trực tiếp. Chế độ xem GUI được mở trong quá trình ghi lại sẽ được ghi lại miễn là nó vẫn mở. Chương trình sẽ kiểm tra tiến trình cứ sau 2 giây và dừng lại khi gặp `--duration`, `Ctrl+C`, hoặc khi trình ghi tự động kết thúc.

Việc ghi hình không ghi được khung hình nào sẽ thất bại: trình ghi báo cáo `no-frames: …` (mảng có thể vẫn đang căn chỉnh hoặc ổn định độ phơi sáng) và lệnh **thoát khỏi `1`**.

```bash
# 30-second combined-index clip at 10 fps, plus a GIF (no GUI needed)
chloros-cli lattice array-record --duration 30 --fps 10 --gif -o monitoring/

# Record a chosen index instead of the default NDVI
chloros-cli lattice array-record --duration 30 \
  --expr "(NIR_850_RGN-Red_660_RGN)/(NIR_850_RGN+Red_660_RGN)" -o monitoring/
```

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--array-id ID` | chỉ mảng | Mảng đích (bỏ qua nếu chỉ có một mảng được kết nối). |
| `-o, --output DIR` | `output` | Thư mục đầu ra. Đường dẫn tương đối nằm trong thư mục mà bạn chạy lệnh. |
| `--fps F` | `10` | Tốc độ khung hình ghi. |
| `--duration S` | cho đến khi nhấn Ctrl+C | Tự động dừng sau `S` giây. |
| `--gif` | tắt | Cũng ghi tệp GIF động. |
| `--gif-only` | tắt | Chỉ ghi tệp GIF (không ghi `.avi`). |
| `--expr EXPR` | NDVI của mảng | Biểu thức chỉ số để ghi lại khi không có cửa sổ GUI nào mở, trong tên các dải của mảng (ví dụ: `'(NIR_850_RGN-Red_660_RGN)/(NIR_850_RGN+Red_660_RGN)'`; xem [tên dải](#index--vegetation-maths)). Tên màu ngắn (`NIR`, `Red`) chỉ hoạt động khi có ít nhất một dải trong mảng có tên đó. Nếu có cửa sổ GUI đang mở, giá trị hiển thị trên cửa sổ đó sẽ được ghi lại thay thế. |

### `array-burst` — chế độ chụp liên tục tốc độ khung hình cao (raw-Bayer) (dành cho phân tích)

Đọc trực tiếp từ bộ đệm nhóm đồng bộ của vòng lặp chụp — **không cần chuỗi hiệu chuẩn, không cần exiftool, không cần chế độ xem trực tiếp** — do đó nó chạy ở tốc độ chụp tối đa của máy ảnh. Ghi các khung hình thô + bản kê khai cho từng khung hình + một tệp `.daq` cho mỗi lần đọc DLS riêng biệt dưới `<output>/bursts/<base>/`. Xử lý lại ngoại tuyến (lệnh tiếp theo), hoặc truyền `--build` để thực hiện ngay lập tức khi dừng.

```bash
# 5-second raw burst, then build the combined index video in one shot
chloros-cli lattice array-burst --duration 5 --build \
  --products combined:index --fps 10 -o capture/
```

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--array-id ID` | chỉ mảng | Mảng đích. |
| `-o, --output DIR` | `output` | Thư mục đầu ra (dữ liệu được ghi vào `<DIR>/bursts/<base>/`). |
| `--duration S` | cho đến khi nhấn Ctrl+C | Tự động dừng sau `S` giây. |
| `--max-frames N` | không giới hạn | Tự độngdừng sau `N` khung hình thô. |
| `--orient` | tắt | Các sản phẩm mà quá trình xây dựng tạo ra từ đợt chụp này được xoay theo hướng của máy ảnh — các sản phẩm của từng máy ảnh được xoay theo hướng của chính máy ảnh đó, chỉ số kết hợp theo hướng của máy ảnh chính — như cách `array-capture --orient` thực hiện. Các khung hình thô giữ nguyên định dạng gốc; `manifest.json` của loạt ảnh ghi lại hướng của từng thành viên và tùy chọn. |
| `--build` | tắt | Sau khi dừng, ngay lập tức xử lý lại loạt ảnh (giống như `array-build-video`). |
| `--products …` | `combined:index` | Với `--build`: chọn video nào để tạo (xem bên dưới). |
| `--fps F` | `10` | Khi sử dụng `--build`: tốc độ khung hình (fps) của video đầu ra. |
| `--save-tiffs` | tắt | Khi sử dụng `--build`: cũng lưu các tệp TIFF đã hiệu chỉnh theo từng khung hình (mỗi tệp có [mặt nạ kẹp unmix](#unmix-clamp-mask-which-pixels-a-band-was-clamped-on) khi một pixel bị giới hạn; kết quả biên dịch liệt kê chúng dưới tên `clamp_masks`). |
| `--gif` | tắt | Với `--build`: cũng ghi tệp GIF động. |

### `array-build-video` — xử lý lại ngoại tuyến một chuỗi ảnh đã lưu

Đồng bộ hóa thời gian từng khung hình thô với giá trị `.daq` đã lưu gần nhất và đưa nó qua **chuỗi radiance / reflectance / index giống như trong quy trình nhập**, từ đó tạo ra một hoặc nhiều video.

`--products` là danh sách các mục `kind:level` được phân tách bằng dấu phẩy, trong đó `kind` ∈ `per_cam` | `combined` và `level` ∈ `radiance` | `reflectance` | `index`. Một `level` đơn thuần (không có `kind:`) sẽ mặc định là `per_cam`. Giá trị mặc định là `combined:index`.

```bash
# Per-cam reflectance video for every member + one combined NDVI video
chloros-cli lattice array-build-video \
  --burst-dir "capture/bursts/2026-06-24_141500" \
  --products per_cam:reflectance,combined:index \
  --fps 10 --save-tiffs
```

| Cờ | Giá trị mặc định | Mô tả |
| --- | --- | --- |
| `--burst-dir DIR` | (bắt buộc) | Đường dẫn đến thư mục burst (`…/bursts/<base>/`). |
| `--products …` | `combined:index` | Danh sách `kind:level`, như trên. |
| `--fps F` | `10` | Tốc độ khung hình (fps) của video đầu ra. |
| `--save-tiffs` | tắt | Cũng lưu các tệp TIFF đã hiệu chỉnh theo từng khung hình cùng với video(s). |
| `--gif` | tắt | Cũng ghi các tệp GIF động. |

**Video không chứa mặt nạ giới hạn** (một luồng hình ảnh, 8-bit). Thay vào đó, kết quả xây dựng chứa `clamp_fractions`: cho mỗi sản phẩm video, phần pixel tương ứng của mỗi khung hình mà quá trình tách màu đã giới hạn trong dải màu của kênh R / G / B — giá trị `Chloros:Band{R,G,B}UnmixClampFraction` là tích của độ sáng/độ phản xạ của khung hình đó sẽ được ghi vào (cho chỉ số tổng hợp, của từng camera thành viên). Quá trình chạy sẽ in khung hình kém nhất cho mỗi sản phẩm trên một dòng `Clamp:`. `--save-tiffs` thêm các mặt nạ theo từng pixel cho các tệp TIFF, trong tệp `clamp_masks/<level>/` bên cạnh thư mục `<level>/` chứa các tệp TIFF.**Quá trình xây dựng bị gián đoạn sẽ không để lại video bị hỏng.** Mỗi video được ghi dưới dạng `<name>.partial.avi` và chỉ nhận tên cuối cùng khi hoàn tất, do đó video cũ có tên đó sẽ vẫn tồn tại cho đến khi bản thay thế hoàn chỉnh. Chạy lại quá trình xây dựng để thay thế tệp `.partial.avi` còn sót lại.

Quá trình chạy sẽ hiển thị mọi video đã được ghi, sau đó là bất kỳ sản phẩm `Skipped:` nào (một camera/mức độ được yêu cầu — hoặc chỉ số kết hợp — không tạo ra video, kèm theo lý do — ví dụ: không có gói hiệu chuẩn, không có dữ liệu DLS cho phản xạ, hoặc không có cặp dải đỏ/NIR cho chỉ số kết hợp) và bất kỳ tệp `Note:` nào. Một camera RGB được yêu cầu cung cấp độ sáng hoặc phản xạ sẽ nhận được video hiển thị thay thế, được cân bằng trắng khi gói hiệu chuẩn của camera có sẵn; một camera được hiển thị mà không có gói hiệu chuẩn sẽ được liệt kê trong `Note:`. Chức năng này **trả về giá trị khác 0** khi sản phẩm được yêu cầu bị bỏ qua hoặc không được xây dựng theo yêu cầu (biểu thức chỉ số đã cấu hình không được đánh giá ngoại tuyến); các video đã được xây dựng vẫn được ghi và liệt kê.

> **Chọn trình ghi phù hợp.** `array-record` là phiên bản *dành cho giám sát* — nó ghi lại hình ảnh tổng hợp trực tiếp đã được xử lý: chế độ xem kết hợp trên giao diện người dùng (GUI), hoặc khi không mở GUI thì là chế độ xem NDVI (hoặc `--expr`) của hệ thống nền. `array-burst` → `array-build-video` là *cấp độ phân tích* — nó lưu dữ liệu cảm biến thô ở tốc độ tối đa và tái tạo các video về độ rạng rỡ/độ phản xạ/chỉ số đã được hiệu chuẩn sau đó, mà không cần chế độ xem trực tiếp.

### Máy ảnh đơn dải (M3M)

Dòng **M3M**là phiên bản đơn dải của dòng Bayer**M3C**: mỗi máy ảnh có một bộ lọc nhiễu dải hẹp (ví dụ: `M3M-<lens>-F<wavelength>`, `M3M-L87-F685`), do đó cảm biến cung cấp**một dải xám duy nhất** mà không có mô-đun Bayer. Không cần xử lý demosaic, không có nhiễu chéo giữa các kênh cần tách biệt, và không cần thiết lập cân bằng trắng — toàn bộ quy trình xử lý màu hiển thị RGB đơn giản là không áp dụng.

Điều này có nghĩa là trên giao diện dòng lệnh (CLI):

- **`lattice white-balance`, `lattice color-profile`, `lattice color`**phát hiện camera đơn sắc và**bỏ qua với một thông báo một dòng** thay vì đẩy các cài đặt vô nghĩa. Chúng vẫn hoạt động bình thường khi gặp camera RGB/Bayer M3C trong cùng một phiên làm việc.
- **Định dạng pixel Bayer đồng nghĩa với độ sâu của nó.** M3M chỉ hỗ trợ định dạng đơn sắc (Mono), do đó khi gặp định dạng Bayer từ `--pixel-format` hoặc `--preset`, hệ thống sẽ thiết lập định dạng đơn sắc (Mono) với cùng độ sâu: `BayerRG8` và `--preset high_speed` sẽ tạo ra `Mono8`; `BayerRG12`, `high_quality` và `triggered` sẽ tạo ra `Mono12`; một tên đã được đóng gói vẫn giữ nguyên dạng đóng gói (`BayerRG12p` tạo ra `Mono12p`). Các tệp và tên `lattice format` chỉ định định dạng Mono. Một mảng kết hợp các máy ảnh M3C và M3M sẽ sử dụng một định dạng theo cùng cách: Bayer trên mỗi M3C, độ sâu Mono trên mỗi M3M. Tên là độ sâu bit của nó: điều này cũng đúng theo chiều ngược lại, do đó máy ảnh M3C không bao giờ được đặt ở định dạng Mono (`--pixel-format Mono12` tương ứng với `BayerRG12` trên máy ảnh M3C, bất kể máy ảnh nào là máy chủ của mảng), và `lattice format --set` được ánh xạ theo cách tương tự và xuất định dạng đã thiết lập.
- **`lattice calibrate` / `process --reflectance` / `array-capture --processing radiance`** vẫn hoạt động — radiance và reflectance là các bản đồ phóng xạ *theo từng dải* và được định nghĩa hoàn toàn chính xác cho một dải. Các khung hình đơn sắc mang ma trận phản hồi cảm biến **nhận dạng** (không có phép tách 3×3), do đó mặt phẳng này đi qua các phép toán hiệu chuẩn mà không bị thay đổi.
- **Một camera đơn sắc duy nhất không thể tạo ra chỉ số thực vật.**NDVI/NDRE/v.v. cần ít nhất hai dải (ví dụ: Red + NIR). Để tính chỉ số từ phần cứng đơn sắc, hãy hướng**nhiều** camera M3M vào các bước sóng khác nhau, ghép chúng thành một chồng ảnh đa dải, và tính chỉ số *từ đó*:

```bash
# Red (660) + NIR (850) mono pair -> aligned 2-band stack -> NDVI
chloros-cli lattice index --live --profile align.json \
  --preset NDVI --channel red=Mono_660 --channel nir=Mono_850 \
  --save-multiband -o output/
```

`index --live` tự động khởi động các camera (một [phiên làm việc tạm thời](#multi-camera-transient-sessions)), do đó trước tiên không cần kết nối bất kỳ thiết bị nào: `--serials`, nếu không các camera được kích hoạt trong cấu hình (xem [Chỉ mục / Toán học Thực vật](#index--vegetation-maths)). Trong khi Chloros đang giữ bất kỳ camera nào (GUI, `cam-connect`, `array-connect`) thì yêu cầu sẽ bị từ chối với mã trạng thái HTTP 409 `CAMERA_HELD` và thoát khỏi `1`; hãy giải phóng chúng bằng `chloros-cli lattice cam-disconnect --all` / `chloros-cli lattice array-disconnect --all`.

Các ký hiệu `--channel` phải khớp **chính xác** với tên kênh của cài đặt sẵn (phân biệt chữ hoa chữ thường; các kênh NDVI viết thường `red`, `nir` — xem `--list-presets`), và tên bên dải phải chỉ định một dải trong chồng dữ liệu đã căn chỉnh (chế độ ngoại tuyến cũng chấp nhận chỉ số dải bắt đầu từ 0, ví dụ: `--channel red=0 --channel nir=1`).

Yếu tố phân biệt trong toàn bộ chồng dữ liệu là token `M3M` trong chuỗi mô hình (nó không bao giờ xuất hiện trong chuỗi `M3C`), được hiển thị trên GUI/SDK dưới dạng `is_mono`.

---

## Cài đặt và Tối ưu hóa NIC Máy chủ (Mảng LATTICE)

Các camera LATTICE truyền dữ liệu GVSP qua bộ điều hợp Ethernet của máy chủ, do đó đối với các mảng nhiều camera, **trình điều khiển**và**kích thước vòng nhận** của bộ điều hợp này cũng quan trọng không kém tốc độ kết nối. Các cài đặt sai sẽ hiển thị dưới dạng cổng `FRAMES WILL DROP` / `Reduce ROI to enable` trong bảng Cài đặt Mảng (và trong `lattice network-analysis` / phần `analyze_array_network()` của SDK), ngay cả khi chính các camera vẫn hoạt động bình thường.

### Bộ điều hợp USB 10GbE — Realtek RTL8157 ("Bộ điều khiển gia đình Realtek USB 10GbE")

Các thiết lập dưới đây là thuộc tính trình điều khiển trên Windows. Trên Linux, bộ điều hợp USB cũng có giới hạn băng thông (400 MB/s trên x86_64, 200 MB/s trên NVIDIA Jetson) (xem [Mô hình fps & burst của mảng](#array-fps--burst-model)), và bộ đệm nhận socket của kernel rất quan trọng đối với mọi bộ điều hợp — xem [Máy chủ Linux](#linux-hosts--socket-receive-buffers).

| Mục | Giá trị yêu cầu | Tại sao lại quan trọng |
| --- | --- | --- |
| **Phiên bản trình điều khiển**|**≥ v10.67 (tháng 1 năm 2026)**, INF `rtump64x64sta.inf` | Trình điều khiển cũ**2016**(v10.65, `rtump64x64.inf`) xử lý không đúng cách việc tắt nguồn và các lỗi hệ thống (bugchecks) với**`DRIVER_POWER_STATE_FAILURE` (BSOD `0x9F`)**khi tắt máy/khởi động lại/chế độ ngủ. Quá trình chuyển đổi bị treo (~5 phút hết thời gian chờ), người dùng phải tắt nguồn cưỡng bức, và việc tắt máy không an toàn lặp đi lặp lại**làm hỏng kho lưu trữ WMI**(PowerShell/các công cụ bắt đầu gặp lỗi với `Invalid class`) và**làm kẹt ngăn xếp USB** trong lần khởi động tiếp theo (NIC không kích hoạt được; các thiết bị USB ngừng liệt kê). Cập nhật từ realtek.com (hoặc nhà cung cấp dongle) trước khi thực hiện khởi động lại an toàn. |
| **Bộ đệm nhận**— từ khóa `ReceiveBufferLen` |**256**(giá trị tối đa của trình điều khiển) | Vòng đệm nhận (RX ring) của NIC. Giá trị mặc định**32**của trình điều khiển chỉ để lại khoảng ~0,26 MB dung lượng vòng đệm khả dụng — quá nhỏ để xử lý các đợt truyền dữ liệu liên tục từ nhiều camera — do đó bảng điều khiển mảng báo lỗi `Sim-emit burst … exceeds NIC RX ring usable capacity 0.26 MB` và chặn kết nối. Ở mức**256**, vòng đệm này rất lớn (**~13,5 MB được đo trên máy chủ 10GbE trong phòng thí nghiệm**), cung cấp cho đường ống nhận (RX pipeline) dung lượng dự trữ thực sự cho các đợt truyền dữ liệu GVSP đa camera. (Việc một cấu hình cụ thể có thực sự *kết nối* hay không được quyết định bởi hai kiểm tra — kiểm tra **nhận biết mức xả**và kiểm tra**quá tải tổng hợp** — chứ không phải so sánh trực tiếp giữa luồng dữ liệu thô và vòng đệm; xem [Mô hình fps & luồng dữ liệu của mảng](#array-fps--burst-model).) |
| **URB nhận**— từ khóa `PendingReceives` |**64** (tối đa) | Các khối yêu cầu USB đang truyền; tăng cùng với Bộ đệm nhận để hấp thụ các đợt truyền dữ liệu đột biến. |
| **Khung Jumbo** — từ khóa `*JumboPacket` | **9014** | Cần thiết cho các gói tin GVSP 9000 byte (số gói tin/khung ít hơn 6 lần so với 1500). |

> ⚠️ **Việc cập nhật trình điều khiển NIC sẽ ĐẶT LẠI các thuộc tính nâng cao này về giá trị mặc định.**Sau khi cập nhật hoặc thay thế trình điều khiển bộ điều hợp, hãy**áp dụng lại** `ReceiveBufferLen=256` và `PendingReceives=64`, nếu không bảng điều khiển mảng sẽ lại bị khóa dù “không có gì thay đổi về phần cứng”. Đây là nguyên nhân hàng đầu khiến một hệ thống từng hoạt động bình thường đột ngột từ chối kết nối.

Áp dụng từ PowerShell **với quyền quản trị** (thay thế bằng tên bộ điều hợp của bạn, ví dụ: `"Ethernet 5"`):

```powershell
Set-NetAdapterAdvancedProperty -Name "Ethernet 5" -RegistryKeyword ReceiveBufferLen -RegistryValue 256
Set-NetAdapterAdvancedProperty -Name "Ethernet 5" -RegistryKeyword PendingReceives  -RegistryValue 64
Get-NetAdapterAdvancedProperty  -Name "Ethernet 5" -RegistryKeyword ReceiveBufferLen,PendingReceives   # verify
```

> **`lattice network --fix` áp dụng cho các bộ điều hợp USB 10GbE.** Lệnh này hiện sẽ phát hiện loại bộ điều hợp và điều chỉnh từ khóa receive-ring phù hợp: `*ReceiveBuffers`→2048 cho các NIC PCIe (Intel I219, v.v.), hoặc `ReceiveBufferLen`→256 + `PendingReceives`→64 cho bộ điều khiển **USB** 10GbE của Realtek (không cung cấp giá trị `*ReceiveBuffers`). Các giá trị mục tiêu được giới hạn ở mức tối đa do từng trình điều khiển báo cáo (`NumericParameterMaxValue`), do đó nó sẽ không bao giờ ghi giá trị nằm ngoài phạm vi. Chạy lệnh này từ một cửa sổ terminal **có quyền quản trị**; giống như bất kỳ tùy chỉnh nào dựa trên registry, thay đổi sẽ có hiệu lực sau khi khởi động lại bộ điều hợp hoặc khởi động lại hệ thống. Các lệnh `Set-NetAdapterAdvancedProperty` thủ công ở trên vẫn là một lựa chọn thay thế tốt — chúng áp dụng ngay lập tức (gán lại bộ điều hợp) mà không cần khởi động lại.

### Máy chủ Linux — bộ đệm nhận của socket

Trên Linux, bao gồm cả NVIDIA Jetson, bộ đệm nhận của socket trong nhân hệ điều hành có vai trò quan trọng không kém vòng nhận của NIC. Ở giá trị mặc định của nhân hệ điều hành (`net.core.rmem_max` / `net.core.rmem_default` = 212.992 byte), một loạt gói tin GVSP từ camera sẽ làm tràn bộ đệm socket luồng trước khi Chloros đọc được chúng. Các gói tin bị loại bỏ trong nhân hệ điều hành trong khi chính NIC không báo cáo mất mát nào, và mảng camera mất các nhóm khung hình và giảm tốc độ khung hình xuống. Khi đo trên một mảng 4 camera kết nối qua bộ điều hợp USB 10 GbE, việc tăng cả hai thông số lên 32 MB đã nâng tốc độ khung hình của mảng từ 4,96 lên 7,52 fps sau khi kết nối, và tốc độ ghi thô liên tục từ 4,7 lên 7,6 lần ghi/giây, mà không có gói tin nào bị loại bỏ trong nhân hệ điều hành.

- **Tệp .deb sẽ thiết lập các giá trị này.** Việc cài đặt gói phần mềm sẽ ghi giá trị `/etc/sysctl.d/60-chloros-gige.conf` (cả hai đều ở mức `33554432`) và áp dụng ngay lập tức. Giá trị nào đã được thiết lập cao hơn sẽ được giữ nguyên.
- **`lattice network` kiểm tra các giá trị này.** Khi một trong hai giá trị này thấp hơn, nó sẽ liệt kê `socket receive buffers too small for GigE Vision (…)` trong phần *Vấn đề phát hiện*, và nhật ký của backend sẽ hiển thị cảnh báo tương tự khi camera hoặc mảng camera đầu tiên kết nối.
- **Để tăng các giá trị này thủ công** (cho đến lần khởi động lại tiếp theo; tệp của gói phần mềm sẽ áp dụng chúng mỗi khi khởi động):

  ```bash
  sudo sysctl -w net.core.rmem_max=33554432 net.core.rmem_default=33554432
  ```

  `lattice network --fix` cũng tăng và duy trì các giá trị này, nhưng chỉ khi backend chạy với quyền root. Một backend chạy dưới tài khoản người dùng của bạn (được khởi động với `chloros-backend`, hoặc [`chloros-backend@<user>`](#running-the-backend-as-a-service-headless)) không thể thay đổi cài đặt kernel và thay vào đó sẽ hiển thị dòng `sudo sysctl`.

**Việc điều chỉnh bộ điều hợp tự động chỉ áp dụng cho các bộ điều hợp đã kết nối.** Khi camera kết nối lần đầu tiên, một backend chạy với quyền root sẽ điều chỉnh các bộ điều hợp có dây có vòng nhận (receive ring) nhỏ. Hiện tại, quá trình này chỉ tối ưu hóa các bộ điều hợp có kết nối, do đó cổng tích hợp bị ngắt kết nối sẽ giữ nguyên MTU của mình và các bộ đệm nhận dữ liệu sẽ không bị thay đổi. `lattice network` không còn liệt kê MTU của bộ điều hợp bị ngắt kết nốilà một vấn đề. `lattice network`, `--fix`, `lattice probe` và quá trình điều chỉnh bộ điều hợp chỉ xem xét các cổng Ethernet có dây, chứ không phải bus CAN của Jetson (`can0`) hay các kết nối ở chế độ thiết bị USB (`usb0`, `usb1`) mà nó cung cấp cho máy tính chủ; bộ chuyển đổi Ethernet qua USB dành cho camera vẫn được tính.

### Kiến thức cơ bản về mạng (tất cả các liên kết LATTICE)

- **Địa chỉ:** địa chỉ liên kết cục bộ `169.254.0.0/16` (GigE Vision LLA). Máy chủ sử dụng địa chỉ tĩnh `169.254.x.x/16`; các camera và DAQ-E tự động gán địa chỉ trong cùng dải. Không cần DHCP/cổng mạng.
- **Kích thước gói tin:**ưu tiên jumbo (9000), nhưng để tính năng tự động dò tìm xác định — tính năng này sẽ bỏ qua giới hạn 1500 byte của ICMP trên camera thông qua gói tin dò tìm GVSP, do đó sẽ sử dụng kích thước jumbo ở bất kỳ đâu mà đường truyền thực sự hỗ trợ.**Mỗi camera được đo một lần trong mỗi phiên backend, và đo lại khi kết nối của nó thay đổi (sử dụng adapter khác, tốc độ kết nối khác, hoặc kết nối bị ngắt — như khi thay đổi switch); các lần kết nối lại sẽ sử dụng lại kết quả đã có. Sau khi có thay đổi phía sau một switch vẫn hoạt động bình thường, hãy khởi động lại backend để đo lại.**Lần kết nối đầu tiên đôi khi có thể mất vài phút trong quá trình kiểm tra này, và nhật ký sẽ ghi lại điều đó khi xảy ra. Chỉ nên sử dụng tùy chọn `CHLOROS_GVSP_PACKET_SIZE_FORCE=9000` khi bạn chắc chắn hơn công cụ kiểm tra, và ưu tiên thiết lập theo từng lệnh thay vì thiết lập vĩnh viễn: tùy chọn này sẽ bỏ qua công cụ kiểm tra, nên nếu đường dẫn thực tế không thể truyền 9000,**mọi** lần ghi dữ liệu sẽ hết thời gian chờ với `SC_ERR_TIMEOUT -1011` (xem [Biến môi trường](#environment-variables)).
- **Vòng đệm RX thay đổi theo giá trị `ReceiveBufferLen`:**ở mức mặc định `32`, dung lượng vòng lặp có thể sử dụng là ~0,26 MB (quá nhỏ cho bất kỳ đợt truyền dữ liệu liên tục từ nhiều camera nào); ở mức tối đa `256`, dung lượng này lớn (~13,5 MB được đo trên máy chủ 10GbE trong phòng thí nghiệm), mang lại dung lượng dự phòng thực sự. Việc một cấu hình có kết nối được hay không sau đó được quyết định bởi kiểm tra khả năng chấp nhận dựa trên lưu lượng thoát**và** kiểm tra tổng mức đăng ký vượt quá dưới đây — chứ không phải so sánh trực tiếp giữa tốc độ bùng nổ và dung lượng vòng.

### Mô hình fps & tốc độ bùng nổ của mảng

Cách đọc bảng Cài đặt Mảng (và `lattice analyze-array` / `analyze_array_network` trong SDK):

- **Dữ liệu đột biến được tính tổng theo từng camera dựa trên định dạng pixel thực tế của camera đó.**Các camera Mono**M3M**truyền dữ liệu ở định dạng**Mono12 (2 bit/pixel)**, hoặc với `--pixel-format` là định dạng Mono của kênh độ sâu (`BayerRG8` tương đương với `Mono8`, 1 bit/pixel); các camera**M3C**Bayer truyền tín hiệu 8- hoặc 12-bit (phần mềm M3C tự động phát ra BayerRG12 ngay cả khi được yêu cầu BayerRG8). Vì vậy, một khung hình độ phân giải đầy đủ từ 4 camera sẽ có dung lượng**~12,6 MB nếu tất cả đều là 8-bit nhưng ~25 MB với ba camera mono 12-bit**. Hệ thống xử lý xác định định dạng của từng camera dựa trên mẫu của nó (bộ nhớ đệm nhận dạng), do đó dữ liệu truyền tải khớp với những gì thực sự được truyền qua đường truyền — không phải giả định chung là BayerRG8.
- **Bộ chuyển đổi Ethernet USB bị giới hạn ở 200 MB/s bất kể thông số kỹ thuật ghi trên nhãn.** Bảng hiệu suất chuyển đổi tốc độ kết nối thành giá trị duy trì được tính toán dựa trên PCIe; một NIC USB công bố tốc độ kết nối *Ethernet* của nó nhưng bị giới hạn bởi bus USB và trình điều khiển của nó. Một bộ chuyển đổi USB 10GbE từng đạt ~1063 MB/s "ổn định" — một con số chưa bao giờ được kiểm chứng — và cơ chế điều tiết tốc độ này đã làm hỏng 6–18% các khung dữ liệu trong khi vẫn báo cáo tốc độ khung hình mục tiêu (fps) ở mức ổn định. Các bộ điều hợp mạng kết nối qua USB hiện bị giới hạn tuyệt đối ở mức: **200 MB/s** trên Windows và trên NVIDIA Jetson, **400 MB/s** trên Linux x86_64 (trên Linux, một bộ điều hợp cắm vào cổng USB 2.0 được phân bổ **30 MB/s**, và ghi chú về khả năng cho biết nên cắm nó vào cổng USB 3) (giới hạn này phụ thuộc vào bus USB và trình điều khiển của máy chủ, do đó không phụ thuộc vào thông số kỹ thuật trên nhãn; một bộ điều hợp USB 1 GbE đạt khoảng 80 MB/s và không bị ảnh hưởng). Trên Linux, bộ điều hợp được nhận diện qua sysfs (sử dụng tên dự phòng là `ethtool -i` hoặc `bus-info`). Mỗi con số đều được đo lường: trên Windows, cùng một mảng 4 camera làm hỏng 6–10 % khung hình khi tốc độ vượt quá 200 MB/s; trên Linux x86_64, cùng bộ chuyển đổi này hoạt động ổn định ở mọi mức giới hạn lên đến 480 MB/s (đạt 273 MB/s, giới hạn của chính các camera). Trên NVIDIA Jetson, cùng bộ điều hợp này chỉ hoạt động ổn định ở 200 MB/s và bị mất khung hình ngắt quãng khi vượt quá mức đó. `wire_ceiling_source` trong bản ghi khả năng đã nêu rõ điều này bằng văn bản, và `nic_is_usb` đánh dấu cảnh báo. Có thể ghi đè bằng cách sử dụng `--wire-ceiling-mbps`.
- **Khả năng chấp nhận dữ liệu phụ thuộc vào dòng tải (drain-aware), không phải so sánh giữa toàn bộ đợt truyền (whole-burst) và vòng lặp (ring).** Một đợt truyền đồng thời chỉ cần phù hợp với *dữ liệu tồn đọng tạm thời* = `max(0, Σ per-cam arrival − host drain) × emit_window`, chứ không phải toàn bộ đợt truyền. Trên kiến trúc máy chủ nhanh / camera chậm (một **PCIe**máy chủ 10G + 4× cam 1 GbE: tốc độ đến ≈ 320 MB/s, tốc độ xả ≈ 1063 MB/s), máy chủ xả dữ liệu nhanh hơn tốc độ lấp đầy của các camera, lượng tồn đọng ≈ 0, do đó chế độ phát mô phỏng độ phân giải đầy đủ**vẫn được chấp nhận**mặc dù đợt truyền 25 MB vượt quá dung lượng vòng 13,5 MB. Nếu đặt cùng bốn camera này phía sau một bộ chuyển đổi**USB** 10GbE trên hệ điều hành Windows, tốc độ xả dữ liệu sẽ là 200 MB/s, không phải 1063 — tốc độ đến vượt quá tốc độ xả, và sự mất mát này thể hiện dưới dạng các khung dữ liệu bị hỏng thay vì tốc độ khung hình thấp hơn. Trên máy chủ 1 GbE, ngưỡng DLThr 31,25 MB/s của các camera khiến tốc độ đến vượt quá tốc độ xả → hệ thống **chặn** đúng cách (đối với *loại* chặn này, hãy giảm ROI hoặc sử dụng binning ≥ 2). Độ dẫn là một trong **hai** cổng kết nối — cổng còn lại là kiểm tra quá tải tổng hợp bên dưới.
- **Tốc độ khung hình dự kiến (fps) là chu kỳ của camera chậm nhất.**Trong chế độ phát đồng thời, mỗi camera gửi khung hình của mình cùng lúc, do đó mảng hoạt động theo chu kỳ dài nhất giữa thời gian đọc cảm biến và thời gian phát của thành viên chậm nhất (số byte khung hình ÷ tốc độ phát của camera:**liên kết truy cập**, 1 GbE ≈ 80 MB/s, hoặc phần chia sẻ của nó trong giới hạn băng thông tối đa của máy chủ khi giá trị đó nhỏ hơn). Mỗi lần phơi sáng chồng lấn lên quá trình đọc dữ liệu của khung hình trước đó, do đó thời gian phơi sáng không được tính vào chu kỳ: chế độ phơi sáng tự động lấp đầy khoảng thời gian do tốc độ phát định, sau đó tăng độ nhạy, và cảnh tối không còn làm giảm tốc độ phát (trong phiên bản 1.2.0 thì có). Chỉ có phơi sáng thủ công kéo dài hơn khoảng thời gian đó mới làm chậm tín hiệu kích hoạt (xem [Phơi sáng trên một thành viên mảng](#exposure-on-an-array-member)). Việc thêm camera sẽ không làm chậm mảng cho đến khi dung lượng chia sẻ trên đường truyền của chúng giảm xuống. Trong chế độ phát tín hiệu nối tiếp (`slip-emit-and-capture`), thời gian phát tín hiệu của các camera sẽ được cộng dồn, và thời gian phơi sáng của từng thành viên được tính vào độ lệch pha. Đo được ở chế độ cảm biến đầy đủ, 12-bit, trên mảng 9 camera với máy chủ PCIe 10 GbE: 10,7 fps, hoặc 14,0 fps với [vận chuyển đóng gói](#packed-12-bit-transport---packed).
- **Sau một lần ghi hình liên tục, phần "Dự báo" sẽ hiển thị tốc độ đã đạt được.** Việc ghi hình trong 30 giây trở lên ở tốc độ tự động sẽ ghi lại tốc độ mà hệ thống đã ổn định ở kích thước mảng đó trên máy tính này; các thử nghiệm ngắn của bộ điều khiển tốc độ ở tốc độ cao hơn sẽ bị loại bỏ. Từ đó trở đi, dòng `array-connect` và bảng Cài đặt Mảng sẽ hiển thị tốc độ đó khi nó thấp hơn tốc độ của mô hình, kèm theo nguyên nhân giới hạn: `clock-bound` (mảng chạy ở tốc độ đã ổn định, vòng lặp thu thập đang chờ tín hiệu đồng hồ kích hoạt), `loop-bound` (vòng lặp thu thập đã hết thời gian), `capture-bound` (quá trình thu thập bị chậm lại và bỏ qua các nhóm) hoặc `measured` (không có thông tin nào chỉ rõ là trường hợp nào). Con số của mô hình nằm ở `wire_fps_max`. Mảng vẫn tăng lên đến mức mà đường truyền có thể chịu được.
- **Mảng hoạt động trên bất kỳ mạng nào — tốc độ khung hình là yếu tố quyết định.**Mỗi camera nhận được phần chia đều của**giới hạn tối đa an toàn va chạm trên đường truyền**(`sustained × sim_emit_factor`), được điều chỉnh theo nhịp độ của GevSCPD, và tất cả đều kích hoạt cùng một tín hiệu. Trên đường truyền chậm, phần chia sẻ sẽ nhỏ và tốc độ khung hình cũng giảm theo: 8 camera độ phân giải đầy đủ 12-bit trên mạng 1 GbE không sử dụng khung jumbo sẽ nhận được 52 / 8 = 6,5 MB/s mỗi camera, tương đương khoảng**1 khung hình/giây**(khoảng 1,3 khung hình/giây khi nén), trong khi một máy chủ 10G có thể chạy chúng ở tốc độ 11–14 khung hình/giây. Nếu các camera không thể được điều chỉnh tốc độ chậm như vậy — hãy kết nối và đọc phạm vi GevSCPD của từng camera để xác định — mảng sẽ chuyển sang chế độ phát**nối tiếp**(`slip-emit-and-capture`): các camera sẽ lần lượt truyền dữ liệu trên đường truyền ở tốc độ liên kết tối đa, được kích hoạt lần lượt, do đó mọi khung dữ liệu vẫn đến được nhưng các thành viên**không được truyền đồng thời**. `analyze-array` và tên đầu ra của lệnh connect chỉ định chế độ; đặt `--force-tier sim-capture-sim-emit` thành 0 để từ chối chế độ dự phòng. Kích thước khung hình không ảnh hưởng đến việc chia sẻ này, do đó**việc gộp khung hình (binning) và vùng quan tâm (ROI) nhỏ hơn sẽ tăng tốc độ khung hình nhưng không thay đổi khả năng mảng camera có vừa với đường truyền hay không**. Chỉ đường truyền không thể truyền mảng dữ liệu ngay cả theo chế độ nối tiếp mới bị**quá tải**— ví dụ: số lượng camera vượt xa khả năng xử lý của một đường truyền, hoặc có nhiều camera khác đang truyền dữ liệu trên cùng đường truyền đó — và trong trường hợp này, triệu chứng là mất gói tin GVSP, chứ không phải tốc độ giảm: `analyze-array` đặt các giá trị fps có thể đạt được về 0 và hiển thị `** OVER-SUBSCRIBED**`, kết nối vẫn tiếp tục với một cảnh báo rõ ràng, và kết nối HTTP với `pin_resolution: true` (chế độ nghiêm ngặt) **bị từ chối**. Các giải pháp bao gồm giảm số lượng camera, sử dụng khung jumbo từ đầu đến cuối, hoặc sử dụng NIC nhanh hơn. `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED=1` hạ mức cảnh báo từ chối xuống thành cảnh báo mạnh cho các tác vụ thử nghiệm — xem [Biến môi trường](#environment-variables).

### Tình trạng mảng — hệ thống con nào đang mất khung

`GET /api/camera/array/<array_id>/capability` của một mảng được kết nối chứa một khối
`health` theo thời gian thực, được đánh giá lại trong một cửa sổ **10 giây** lăn. Nó phân chia tình trạng mất khung
thành hai nguyên nhân cần các biện pháp khắc phục trái ngược nhau, thay vì báo cáo một tỷ lệ "không hoàn chỉnh"
mà không nêu rõ nguyên nhân nào:

| Trường | Ý nghĩa | Hệ thống con nào |
| --- | --- | --- |
| `gvsp_corrupt_rate_pct` (theo cổng nối tiếp) | Khung dữ liệu **đã đến nhưng có cấu trúc sai**— Mất gói tin GVSP. |**Mạng**: băng thông đường truyền, tốc độ truyền, vòng nhận NIC, MTU |
| `never_arrived_rate_pct` (theo cổng nối tiếp) | Khung dữ liệu **chưa bao giờ đến**— camera không kích hoạt, hoặc không có dữ liệu nào được gửi đi. |**Kích hoạt / đồng bộ**: Cáp M8, `--line`, `TriggerMode` |
| `worst_gvsp_corrupt_pct` / `worst_never_arrived_pct` | Tỷ lệ camera tệ nhất cho từng trường hợp. | — |
| `per_cam_rate_pct` | Tỷ lệ không hoàn chỉnh tổng hợp trên mỗi camera (cả hai nguyên nhân cộng lại). | — |
| `stable_for_seconds` | Thời gian mỗi camera duy trì tỷ lệ dưới 0,01 %. | — |

Khi vượt quá 5%, hệ thống backend sẽ ghi vào nhật ký một dòng `[array-health <id>] WARN` nêu rõ nguyên nhân — khi
xảy ra vi phạm lần đầu, khi thay đổi mức độ nghiêm trọng, mỗi phút một lần trong suốt thời gian tình trạng này còn tồn tại, và một lần khi
tình trạng này được khắc phục. Nửa bị hỏng ghi dòng `[gvsp-corrupt <SN>]` vào tệp nhật ký backend khi
xảy ra lần đầu tiên cho mỗi camera và lý do, sau đó tổng hợp mỗi 60 giây; bảng nhật ký của ứng dụng hiển thị dòng
dòng `[array-health <id>]` thay thế. Mọi lần đánh giá vẫn được ghi vào tệp nhật ký phía sau;
các bộ đếm vẫn tăng lên với mỗi vùng đệm bất kể nội dung được in ra là gì.

Cùng một bản ghi báo cáo con số mà toàn bộ quá trình phân bổ phụ thuộc vào:

| Trường | Ý nghĩa |
| --- | --- |
| `wire_ceiling_mbps` | Ngân sách băng thông liên tục hiện tại của máy chủ, MB/s. |
| `wire_ceiling_source` | Nguồn gốc của con số đó, bằng văn bản — ví dụ: `USB-capped 200 MB/s (was theoretical 1062; PnPDeviceID=USB\VID_0BDA&PID_815A)` (trên Linux x86_64, `USB-capped 400 MB/s (…; sysfs: bus=usb (usb2/2-1/2-1:1.0))`) hoặc `user override 120 MB/s (auto said 200)`. |
| `wire_ceiling_is_user_set` | `true` khi `--wire-ceiling-mbps` (hoặc trường **Ngân sách băng thông** trên giao diện đồ họa) thiết lập giá trị này. |
| `nic_is_usb` | `true` dành cho bộ điều hợp Ethernet USB — xem giới hạn USB ở trên. |

**Giải thích:** Giá trị `gvsp_corrupt_rate_pct` khác 0 trong khi `never_arrived_rate_pct` bằng 0
có nghĩa là kích hoạt và đồng bộ cáp hoàn hảo và 100% tổn thất nằm trên đường truyền mạng
— hãy giảm giá trị `--wire-ceiling-mbps` và kết nối lại. Mô hình ngược lại chỉ ra vấn đề
nằm ở cáp đồng bộ hoặc đường dây kích hoạt.

> **`--target-fps` không phải là yếu tố quyết định đối với các khung dữ liệu bị hỏng.** Tốc độ điều chỉnh GevSCPD được thiết lập
> một lần khi kết nối, do đó việc giảm tần số kích hoạt sẽ thay đổi chu kỳ làm việc chứ không phải
> tốc độ phát đồng thời theo đợt. Việc giảm 5 lần nhu cầu đã không mang lại cải thiện;
> việc giảm giới hạn băng thông từ 240 xuống 200 MB/s đã đưa cùng một hệ thống từ 10,4%
> khung bị hỏng xuống 0,00%.

> **Một mảng đang hoạt động không thể tự lập kế hoạch lại trên firmware LATTICE (M3C / M3M).** Nó
> không thể tự khắc phục vấn đề này; hãy ngắt kết nối và kết nối lại để bộ chọn thời gian kết nối có thể
> lập kế hoạch lại với giới hạn mới.

### Triệu chứng → cách khắc phục

| Triệu chứng (Cài đặt mảng / kết nối / `analyze_array_network`) | Nguyên nhân | Cách khắc phục |
| --- | --- | --- |
| `FRAMES WILL DROP … exceeds NIC RX ring usable capacity 0.26 MB`, `Reduce ROI to enable` | `ReceiveBufferLen` được đặt lại về 32 (thường xảy ra sau khi cập nhật trình điều khiển) | Đặt `ReceiveBufferLen`→256, `PendingReceives`→64; mở lại bảng điều khiển (khởi động lại phần nền nếu nó đã lưu kích thước vòng cũ vào bộ nhớ đệm) |
| Quá trình khởi động lại/tắt máy bị treo; sau đó xuất hiện lỗi WMI liên quan đến `Invalid class`, NIC không kích hoạt được, ổ USB bị mất | Trình điều khiển Realtek USB 10GbE cũ năm 2016 → BSOD `0x9F` → tắt nguồn cưỡng bức | Cập nhật trình điều khiển bộ điều hợp lên ≥ v10.67 (2026), sau đó áp dụng lại các thiết lập vòng nhận ở trên |
| Mảng kết nối ở mức cảm biến tối đa nhưng tốc độ khung hình thấp | Chế độ Tự động duy trì mức cảm biến tối đa và để tốc độ giảm xuống mức mà đường truyền hỗ trợ | Chọn một trong các tùy chọn nhanh hơn trong danh sách đầu ra kết nối (`--binning 2` / `4`, hoặc một phiên bản `--width`/`--height`), bật khung jumbo hoặc nâng cấp kết nối |
| Mảng báo cáo fps mục tiêu bình thường nhưng chỉ truyền được một phần nhỏ; `health.gvsp_corrupt_rate_pct` khác 0, `never_arrived_rate_pct` 0 | Dung lượng băng thông dự đoán của máy chủ cao hơn mức thực tế mà nó có thể duy trì (thường gặp trên bộ điều hợp Ethernet USB, làn PCIe hẹp hoặc mạng chia sẻ) | Kết nối lại với giá trị `--wire-ceiling-mbps` thấp hơn và kiểm tra lại khối trạng thái. **Không phải** `--target-fps` — Tốc độ điều chỉnh GevSCPD được cố định khi kết nối |
| Linux: một mảng mất các nhóm khung và giảm tốc độ khung trong khi NIC báo cáo không có gói tin bị mất; `lattice network` liệt kê `socket receive buffers too small` | Bộ đệm nhận socket của kernel đang ở mức mặc định, do đó các gói tin từ camera bị bỏ qua trong kernel | Tăng `net.core.rmem_max` / `rmem_default` lên 32 MB — xem [máy chủ Linux](#linux-hosts--socket-receive-buffers). Gói .deb thực hiện điều này khi cài đặt |
| Các camera không xuất hiện trong các nhóm đã công bố; `health.never_arrived_rate_pct` khác 0, `gvsp_corrupt_rate_pct` bằng 0 | Kích hoạt / đường dẫn đồng bộ — các camera không hoạt động, không phải là vấn đề mạng | Kiểm tra cáp đồng bộ M8 và `--line`; xác nhận mọi thành viên đều đã được kích hoạt (`TriggerMode=On`) |
| `**OVER-SUBSCRIBED**` / `Wire budget` vượt quá giới hạn trong `analyze-array`, hoặc từ chối kết nối ở chế độ nghiêm ngặt (`pin_resolution`; `array over-subscribes the wire`) | Dây cáp không thể truyền tải dữ liệu từ mảng camera ở bất kỳ tốc độ nào mà các camera đạt được, dù đồng thời hay lần lượt — số lượng camera vượt quá khả năng chia sẻ của dây cáp, hoặc có các camera khác đang truyền dữ liệu trên đó | Giảm số lượng camera, sử dụng khung jumbo từ đầu đến cuối, hoặc sử dụng NIC nhanh hơn. **ROI/binning SẼ KHÔNG GIÚP ĐƯỢC** (việc chia sẻ không phụ thuộc vào kích thước khung). `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED=1` ghi đè trên môi trường thử nghiệm (chấp nhận mất gói tin) |
| Mảng được kết nối qua `slip-emit-and-capture` (kết nối nối tiếp) trên mạng chậm | Các camera không thể được điều chỉnh tốc độ đủ chậm để chia sẻ đường truyền đồng thời, do đó chúng phải luân phiên; các khung hình không được ghi lại cùng một lúc | Khung hình jumbo hoặc NIC nhanh hơn sẽ khôi phục khả năng ghi hình đồng thời; `--force-tier sim-capture-sim-emit` vẫn khăng khăng thực hiện (và thay vào đó bị mất gói tin) |

---

## `chloros-cli daq`

Các lệnh cảm biến quang phổ. Các lệnh `pool-*` là các client HTTP nhẹ điều khiển cảm biến thông qua nhóm kết nối liên tục của backend, do đó chúng cần backend đang chạy (xem [Khởi động Backend](#start-the-backend)). Hệ thống backend quản lý kênh truyền tải, do đó các skript CLI và SDK chia sẻ chung một kết nối hoạt động thay vì tranh giành cổng nối tiếp. `pool-discover` là cách duy nhất để xác định địa chỉ MAC BLE của DAQ-M.

> **`chloros-cli daq --help`** (hoặc `-h`, hoặc `daq help`) liệt kê các lệnh con của `pool-*` (trên Linux, cùng với các lệnh điều khiển phần cứng trực tiếp mà bản dựng bao gồm), và `chloros-cli daq <subcommand> --help` (hoặc `daq help <subcommand>`) cung cấp các tùy chọn cho từng lệnh. Chúng bao gồm kết nối, stream, ghi tệp `.daq` đã hiệu chuẩn, hoán đổi cấu hình cap và kết nối DAQ với camera; đối với DAQ-E, các tùy chọn này bao gồm cài đặt mạng, PTP, khởi động lại và cập nhật firmware. Thiết bị DAQ cũng có thể được điều khiển từ Python bằng lệnh `chloros_sdk.connect_daq_sensor()`, sử dụng cùng đường dẫn chung.
>
> **Sử dụng các lệnh `pool-*` trên mọi nền tảng.** Chúng hoạt động giống nhau trên Windows, Linux và Jetson. Trên Windows, `daq discover` và `daq list` là các tên gọi khác của `pool-discover`, và bất kỳ lệnh `daq` nào khác không phải là `pool-*` sẽ thoát khỏi `1` bằng lệnh `DAQ direct-hardware commands require the full daq package`. Trên Linux và Jetson, `daq discover`, `daq list` và các lệnh khác không phải là `pool-*` `daq` là các lệnh phần cứng trực tiếp-phần cứng trực tiếp của công cụ DAQ: chúng mở cảm biến trong tiến trình riêng của CLI, ngoài nhóm cảm biến của backend (do đó không phải là cảm biến do backend, GUI hoặc tập lệnh nào đó đang giữ), và tại đó, các lệnh `daq discover`, `--only` và `--timeout` được chấp nhận nhưng không chấp nhận lệnh `--json`. Tài liệu tham khảo này chỉ ghi lại các lệnh `pool-*`.

### Quy trình kết nối lần đầu với cảm biến DAQ

```bash
# 1. Scan every transport, showing the address to connect with.
#    This is how you find a DAQ-M's BLE MAC — unlike a DAQ-E hostname or a
#    DAQ-U serial port, a MAC isn't printed on the device or listed by the OS.
chloros-cli daq pool-discover
chloros-cli daq pool-discover --only ble           # BLE only
chloros-cli daq pool-discover --json               # machine-readable

# 2. Open a persistent pool session (handle stays alive across CLI calls)
chloros-cli daq pool-connect                                   # smart-detect
chloros-cli daq pool-connect --port /dev/ttyUSB0               # DAQ-U on a serial port (Windows: COM3)
chloros-cli daq pool-connect --mac AA:BB:CC:DD:EE:FF           # DAQ-M by BLE MAC
chloros-cli daq pool-connect --eth-host daq-e-xxx.local        # DAQ-E by hostname

# 3. List what's in the pool, including the sensor_id you'll use next
#    (DAQ-U ids look like 'CB-7C-A8-2E-5F'; DAQ-E ids like 'daq-e-def330')
chloros-cli daq pool-list

# 4. Read the latest spectrum frame
chloros-cli daq pool-latest --sensor-id CB-7C-A8-2E-5F

#    Bind it to a camera, so that camera's captures use and save its readings
chloros-cli daq pool-bind --camera 213800234 CB-7C-A8-2E-5F

# 5. Record a calibrated .daq file for 60s
chloros-cli daq pool-record --sensor-id CB-7C-A8-2E-5F --duration 60 \
  -o ~/Documents/spectra --device-name "field-A"

# 6. Release
chloros-cli daq pool-disconnect --sensor-id CB-7C-A8-2E-5F
```

### Tài liệu tham khảo về `pool-*`

| Lệnh con | Mục đích |
| --- | --- |
| `daq pool-connect` (smart-detect) | Mở một cảm biến trong nhóm cảm biến phía sau. |
| `daq pool-connect --port PORT` (`-p`) | DAQ-U trên cổng nối tiếp cụ thể. |
| `daq pool-connect --ble` | DAQ-M qua BLE, tự động quét địa chỉ MAC. |
| `daq pool-connect --mac MAC` (`-m`) | DAQ-M qua BLE tại địa chỉ MAC đã biết (ngụ ý `--ble`). |
| `daq pool-connect --eth-host HOST` | DAQ-E qua Ethernet tại tên máy chủ hoặc địa chỉ IP đã biết (ngụ ý `--eth`). |
| `daq pool-connect --eth` | DAQ-E qua Ethernet, máy chủ được tự động phát hiện (mDNS + dự phòng ARP; hoạt động ngay cả khi bộ nhớ đệm ARP trống trên Windows và Linux). |
| `daq pool-connect --integration-time MS (-t) --frame-avg N (-f) --no-ae` | Điều chỉnh cửa sổ tích hợp / trạng thái AE. |
| `daq pool-connect --no-stream` | Kết nối nhưng chưa bắt đầu truyền dữ liệu (tiếp tục với `pool-stream --start`). |
| `daq pool-connect --cap-id {none, fov_15, fov_30, fov_45, fov_60, fov_90, sunshine_cosine}` | Hồ sơ hiệu chỉnh giới hạn. Giá trị mặc định ở phía máy chủ là `sunshine_cosine`; `none` là cảm biến trần. Một [DAQ-E-S](#daq-e-s--fixed-diffuser-and-tilt-sensor) sẽ bỏ qua nó (bộ khuếch tán cố định). **Các cảm biến được giao hàng ở dạng trần, vì vậy hãy đặt tên cho nắp mà bạn đã lắp:** giá trị mặc định được ghi lại là (`cap_id_source=auto_default`) và quá trình xử lý sẽ cảnh báo về điều này, vì một DAQ-U trần khi áp dụng hiệu chỉnh ánh nắng mặt trời sẽ cho giá trị cao gấp ~30 lần. |
| `daq pool-discover [--only usb,ble,eth] [--timeout SEC] [--json]` | Quét từng phương tiện truyền tải để tìm các cảm biến có thể kết nối, mà không thực sự kết nối. **Đây là cách bạn tìm địa chỉ MAC BLE của DAQ-M.** `--timeout` áp dụng cho từng giao thức (mặc định 5 giây, 1-20). Các cảm biến đã được mở trong danh sách không được liệt kê — một DAQ-M đã kết nối sẽ ngừng phát tín hiệu quảng cáo — vì vậy hãy sử dụng `pool-list` cho những trường hợp đó. `usb` chỉ liệt kê các thiết bị nối tiếp USB: các cổng nối tiếp của chính máy tính (`COM1`, `/dev/ttyS*` trên Linux) không thể là cảm biến và sẽ bị loại trừ. |
| `daq pool-list` | Hiển thị mọi cảm biến trong nhóm backend. |
| `daq pool-bind (--camera SERIAL \| --array ARRAY_ID) {SENSOR \| --clear}` | Gán một camera hoặc mảng vào một DAQ, tương tự như cách chọn cảm biến ánh sáng trong giao diện người dùng: các lần chụp của nó sẽ sử dụng, khớp và lưu các giá trị đo của DAQ đó. `SENSOR` là ID cảm biến từ `pool-list` hoặc địa chỉ mà DAQ được kết nối. `--clear` (không có `SENSOR`) sẽ hủy liên kết, và các lần ghi hình sẽ chuyển sang DAQ mới nhất. Kết nối duy trì cho đến khi phần nền (backend) khởi động lại, hoặc cho đến khi camera, mảng hoặc DAQ đó bị ngắt kết nối: việc ngắt kết nối (ở đây, từ SDK hoặc trong ứng dụng) sẽ hủy bỏ mọi kết nối với phần cứng bị ngắt, bất kể ai đã thiết lập nó, bao gồm cả các thành viên của mảng cùng với chính mảng đó. Camera phải được kết nối, nằm trong dự án đang mở, hoặc đã được mở trên máy tính này trước đó, và mảng phải được kết nối hoặc nằm trong dự án đang mở; bất kỳ số sê-ri hoặc ID nào khác sẽ bị từ chối theo tên và không có kết nối nào được thiết lập. |
| `daq pool-disconnect {--sensor-id ID \| --all}` | Giải phóng — bất kể ai đã mở cảm biến, do đó nó cũng giải phóng cảm biến mà một skript bị treo đã để lại trong bể (mọi quá trình ghi trên cảm biến đó phải hoàn tất trước). Một cảm biến không nằm trong bể không có gì để giải phóng: nó in ra `ID: already released (not in the pool)` và thoát khỏi `0`; một lệnh giải phóng bị phần nền từ chối hoặc không xác nhận sẽ thoát khỏi `1`. SDK: `DAQSensorSession.force_disconnect()`. |
| `daq pool-latest --sensor-id ID [--recent N] [--json]` | N khung phổ gần đây nhất (mặc định là 1). |
| `daq pool-stream --sensor-id ID [--start \| --stop]` | Tiếp tục / tạm dừng truyền phát. |
| `daq pool-record --sensor-id ID [--duration SEC] [-o DIR] [--device-name NAME] [--confirm-after S] [--heartbeat S]` / `--stop` | Bắt đầu / dừng ghi tệp .daq. `-o` mặc định là `~/Documents/DAQ Live View` trên máy tính của máy chủ (đường dẫn tương đối nằm trong thư mục mà bạn chạy lệnh); `--duration` (`-d`, mặc định là `0` = cho đến khi đạt đến `--stop`). `--confirm-after S` sẽ cảnh báo khi không có giá trị đo nào được nhận sau S giây kể từ khi bắt đầu (mặc định là 20; 0 để tắt tính năng này); với `--duration`, `--heartbeat S` sẽ hiển thị số lượng giá trị đo mỗi S giây và cảnh báo khi số lượng này ngừng tăng (mặc định là 30; đặt thành 0 để tắt tính năng này). Quá trình ghi dữ liệu trên một DAQ-E bị ngắt kết nối sẽ tiếp tục trong cùng một tệp sau khi hệ thống nền kết nối lại, và `--stop` liệt kê các khoảng trống ([Một DAQ-E ngừng truyền dữ liệu](#a-daq-e-that-stops-delivering)). Phím tắt Ctrl+C sẽ kết thúc sớm quá trình ghi dữ liệu của `--duration` và báo cáo sự việc; nếu không có `--duration`, quá trình ghi sẽ tiếp tục chạy sau khi nhấn Ctrl+C (thoát khỏi `130`) cho đến khi `--stop`. |
| `daq pool-set-cap --sensor-id ID --cap-id CAP` | Đổi cấu hình hiệu chỉnh điện dung trong quá trình chạy. `--cap-id` là bắt buộc; `--cap-id none` loại bỏ hiệu chỉnh điện dung (cảm biến trần). Thiết bị DAQ-E-S sẽ giữ nguyên giá trị đã cài đặt cho `as_recorded`. |
| `daq pool-enable-ptp --sensor-id ID` | Bật lại PTP cho thiết bị DAQ-E đang tắt PTP. Cài đặt này được lưu trên thiết bị, do đó thiết bị đã tắt sẽ vẫn ở trạng thái tắt — dấu thời gian của nó không được đồng bộ hóa theo PTP — cho đến khi lệnh này được thực thi. `pool-list` đánh dấu thiết bị đó (`!! PTP is switched OFF`) và in lệnh này cho thiết bị đó. Chloros không bao giờ tự động bật lại thiết bị, vì việc tắt có thể là cố ý. |
| `daq pool-info --sensor-id ID [--json]` | Các cài đặt riêng của DAQ-E: phần mềm nhúng (và liệu bản phát hành Chloros này có chứa phiên bản mới hơn hay không), địa chỉ của thiết bị và cách thiết bị nhận được địa chỉ đó (DHCP / link-local / tĩnh), cài đặt mạng đã lưu cho đến khi cảm biến chạy nó (một cài đặt được lưu kèm theo `--no-reboot`), luồng dữ liệu khi bật nguồn và PTP. Trên một cảm biến mà luồng dữ liệu không đến được máy tính này, nó sẽ đọc cảm biến trên một kết nối mới và thông báo điều đó. |
| `daq pool-set-network --sensor-id ID {--auto \| --static IP [--mask M] [--gateway G]} [--no-reboot] [--force] [--timeout S]` | Cài đặt DAQ-E thành DHCP (`--auto`) hoặc địa chỉ tĩnh (`--mask` mặc định là `255.255.255.0`). Cảm biến sẽ khởi động lại để áp dụng cài đặt và kết nối lại với các thiết lập ban đầu, chờ tối đa `--timeout` giây (mặc định là 45) cho quá trình này; `--no-reboot` chỉ lưu lại cài đặt này. Xem [Cài đặt Thiết bị DAQ-E](#daq-e-device-settings). |
| `daq pool-set-auto-stream --sensor-id ID {--on \| --off}` | Xác định liệu DAQ-E có bắt đầu truyền dữ liệu ngay khi bật nguồn mà không có phần mềm nào được kết nối hay không (cài đặt này được lưu trên cảm biến). Dù trong trường hợp nào, Chloros cũng tự động bắt đầu truyền dữ liệu. |
| `daq pool-reboot --sensor-id ID [--timeout S]` | Khởi động lại DAQ-E và kết nối lại với các cài đặt trước đó (chờ tối đa `--timeout`, mặc định 45 giây). |
| `daq pool-update-firmware --sensor-id ID [--timeout S]` | Cập nhật firmware DAQ-E đi kèm với bản phát hành Chloros này lên cảm biến đang chạy phiên bản cũ hơn, sau đó kết nối lại (khoảng một phút; không tắt nguồn; `--timeout` mặc định 45 giây để thiết bị hoạt động trở lại). Cảm biến đã chạy phiên bản mới nhất sẽ không bị can thiệp. |

### Cảm biến DAQ-E ngừng truyền dữ liệu

Một cảm biến DAQ-E trong bể nếu không truyền phổ trong 20 giây sẽ được hệ thống backend kết nối lại. Việc tắt/bật nguồn bộ chuyển mạch cấp nguồn cho cảm biến sẽ thực hiện điều này: cảm biến khởi động lại cùng với bộ chuyển mạch và sẽ không truyền dữ liệu trở lại cho đến khi được yêu cầu (trừ khi có thiết lập `pool-set-auto-stream --on`).

- **Hệ thống sẽ kiểm tra xem đó có phải là cùng một cảm biến hay không trước tiên.** Hệ thống sẽ gửi yêu cầu đến địa chỉ mà cảm biến từng sử dụng (không thay đổi bất kỳ thiết lập nào) để lấy tên, địa chỉ MAC và số seri của cảm biến. Nó cũng gửi yêu cầu dưới tên `daq-e-<id>.local` của cảm biến, nhưng chỉ tin tưởng câu trả lời đó khi nó đến từ bộ điều hợp mạng (MAC) của chính cảm biến. Bất kỳ thiết bị nào khác trả lời đều không được kết nối lại hoặc ghi dữ liệu vào.
- **Nó kết nối lại với các cài đặt của phiên:** thời gian tích hợp, trung bình khung hình, phơi sáng tự động và cấu hình giới hạn như ban đầu, không phải các giá trị mặc định khi bật nguồn của cảm biến. Một cảm biến đã bật PTP và quay lại với chế độ tắt sẽ được bật lại. Sau đó, nó khởi động lại luồng dữ liệu và trình nhận trên máy tính dành cho luồng đó.
- **Nó sẽ thử lại với các khoảng thời gian ngày càng dài:** ngay lập tức, sau đó là sau 5, 10, 20 và 40 giây, rồi cứ sau 60 giây trong thời gian cảm biến không hoạt động. Mỗi lần thử chỉ yêu cầu cảm biến xác nhận danh tính. Một cảm biến trả lời nhưng vẫn không gửi dữ liệu sau ba lần kết nối lại sẽ được để yên và báo cáo (`auto-reconnect GAVE UP`). `pool-stream --start` sẽ thử lại.
- **`pool-list` hiển thị tình trạng hiện tại.** Trong khi cảm biến im lặng, dòng `STREAM FAULT` cho biết quá trình kết nối lại đang diễn ra như thế nào. Dòng này biến mất cùng với phổ đầu tiên, và cảm biến sau đó hiển thị `reconnected automatically N time(s)`.
- **Quá trình ghi dữ liệu tiếp tục trong cùng một `.daq`.** Tệp ghi lại mỗi khoảng trống bằng hai dòng: `event_type` 5 tại phổ cuối cùng trước khoảng trống và 6 tại phổ đầu tiên sau đó, mỗi dòng chỉ chứa dấu thời gian. `pool-record --duration` hiển thị `!! STREAM LOST …` khi cảm biến vắng mặt và `stream resumed after …` khi cảm biến quay lại. `pool-record --stop` liệt kê từng khoảng trống và đưa ra cảnh báo, giống như đối với một tệp trống (một bản ghi không chứa bất kỳ giá trị đo nào sẽ thoát khỏi `1`).
- **Một bản ghi chỉ giữ một giá trị hiệu chuẩn.** Nếu cảm biến quay lại với giá trị hiệu chuẩn khác trong khi bản ghi đang chạy, phần còn lại của bản ghi sẽ giữ nguyên giá trị hiệu chuẩn ban đầu, và cảnh báo dừng sẽ thông báo điều này. `pool-disconnect` + `pool-connect` sẽ áp dụng thông số hiệu chuẩn mới.
- **Lệnh của bạn sẽ được ưu tiên.** Một cảm biến mà bạn đã thực hiện lệnh `pool-disconnect` hoặc tạm dừng bằng lệnh `pool-stream --stop` sẽ không bao giờ được kết nối lại, ngay cả khi quá trình kết nối lại đang diễn ra.
- **`pool-info` trên cảm biến im lặng** sẽ đọc dữ liệu từ cảm biến trên kết nối mới và thêm `!! stream NOT reaching this computer`. Một cảm biến không phản hồi sẽ được báo cáo ngay lập tức. `pool-list` không còn hiển thị trạng thái PTP cho cảm biến đã ngừng phản hồi.
- DAQ-U và DAQ-M không được đề cập. DAQ-M tự động kết nối lại qua Bluetooth, còn DAQ-U bị ngắt kết nối khỏi USB cần thực hiện lại lệnh `pool-connect`.

### DAQ-E-S — bộ khuếch tán cố định và cảm biến nghiêng

**DAQ-E-S** là phiên bản DAQ-E có hai điểm khác biệt quan trọng đối với phần mềm. Tất cả các tính năng còn lại — phát hiện, kết nối, PTP, ghi dữ liệu, các cài đặt thiết bị dưới đây — đều hoạt động chính xác như trên DAQ-E.**Bộ khuếch tán của nó là cố định, do đó không cần hiệu chỉnh nắp.** Bộ khuếch tán kiểu Sunshine được tích hợp sẵn và đã có trên thiết bị khi thực hiện hiệu chuẩn tại nhà máy, do đó hiệu chuẩn này đã hiển thị chính xác bức xạ hướng xuống. Chloros đặt nắp của nó thành `as_recorded` (không có cấu hình) bất kể yêu cầu nào: một giới hạn được đặt tên riêng cho thiết bị này (`sunshine_cosine`, `fov_*`, `none`) sẽ bị bỏ qua kèm theo thông báo, và các bản ghi của thiết bị lưu trữ giá trị `cap_id_source=model`, do đó cảnh báo về giới hạn giả định sẽ không bao giờ kích hoạt đối với nó. Cũng không nên áp dụng giới hạn cho dữ liệu của nó bằng tay: nếu đặt `sunshine_cosine` ở trên cùng, giá trị đọc sẽ cao gấp khoảng 11 lần.

**Nó được trang bị cảm biến gia tốc 3 trục.** Đáp ứng cosin của bộ khuếch tán ánh sáng mặt trời chỉ chính xác khi thiết bị ở trạng thái cân bằng, do đó cảm biến bị nghiêng sẽ báo cáo giá trị bức xạ sai — và hệ số phản xạ cũng kế thừa lỗi này — mà không có triệu chứng nào khác. Do đó, mỗi giá trị đọc từ DAQ-E-S đều chứa thông tin về tư thế của thiết bị:

| Trường | Ý nghĩa |
| --- | --- |
| `tilt_deg`, `roll_deg`, `pitch_deg` | Góc tính bằng độ. `None` khi phần mềm nhúng đánh dấu góc là không xác định (rơi tự do, trục chết). |
| `x_mg`, `y_mg`, `z_mg`, `mag_mg` | Gia tốc tính bằng milli-g. Giá trị `mag_mg` cách xa ~1000 có nghĩa là thiết bị đang di chuyển và các góc không có ý nghĩa đối với mẫu đó. |
| `cal_applied` | Chế độ "Set": các trục và góc được hiển thị theo khung tham chiếu của bộ khuếch tán (đã áp dụng hiệu chuẩn lắp đặt của thiết bị). Chế độ "Clear": khung tham chiếu gốc của bo mạch, có thể hiển thị độ nghiêng lên đến ~2,3° nhưng không phản ánh thực tế. |
| `sample_age_ms` | Thời gian trôi qua kể từ khi mẫu gia tốc kế được ghi lại cho đến khi phổ dữ liệu được gửi đi. |
| `temp_raw` | **Xu hướng** nhiệt độ chip chưa hiệu chuẩn — chỉ hiển thị sự chênh lệch, không bao giờ là giá trị đo trực tiếp từ cảm biến nhiệt. |

`daq pool-latest --json` trả về các giá trị này dưới khóa `imu` của từng khung (`null` trên DAQ-U, DAQ-M hoặc DAQ-E thông thường). Dữ liệu `.daq` được ghi từ DAQ-E-S lưu trữ các giá trị tương tự trong các cột `imu_*` (`imu_tilt_deg`, `imu_roll_deg`, `imu_pitch_deg`, `imu_mag_mg`, `imu_cal_applied`, …) bên cạnh mỗi phổ, nhờ đó người đọc có thể phân biệt được hiện tượng mây với trường hợp chân máy bị va đập. Trên các mẫu khác, các cột này đều trống.

Dòng dữ liệu tư thế được báo cáo riêng biệt: `daq pool-list` hiển thị `!! ATTITUDE STREAM FAULT (spectra unaffected)` khi thiết bị ngừng hoạt động, và số lượng lỗi mà nó đã khắc phục sau đó. Một lỗi ở đây không bao giờ gây ra khoảng trống trong bản ghi phổ. `daq pool-discover` liệt kê thiết bị là `DAQ-E-S` (hoặc `DAQ-E` nếu thiết bị không trả lời kiểm tra danh tính trong khoảng 2 giây).

### Cài đặt thiết bị DAQ-E

```bash
chloros-cli daq pool-info --sensor-id daq-e-def330

# A fixed address on the network this computer is on (192.168.1.x)
chloros-cli daq pool-set-network --sensor-id daq-e-def330 \
  --static 192.168.1.50 --gateway 192.168.1.1

# Back to the factory setting: DHCP, falling back to a 169.254.x.x
# link-local address when no DHCP server answers
chloros-cli daq pool-set-network --sensor-id daq-e-def330 --auto

chloros-cli daq pool-update-firmware --sensor-id daq-e-def330
```

- **`--static` mà không có `--gateway` sẽ không lưu cổng kết nối** (bất kỳ cổng kết nối nào được thiết lập trước đó sẽ bị xóa), điều này phù hợp với mạng camera không có bộ định tuyến.
- **Máy tính này phải có địa chỉ tĩnh trên mạng của nó.** Chloros sẽ từ chối địa chỉ có mạng con không bao gồm địa chỉ của máy tính này trên mạng của cảm biến: sau khi khởi động lại, không thể kết nối với cảm biến để khôi phục cài đặt. Chỉ nên sử dụng `--force` khi máy tính này sẽ được gán lại địa chỉ hoặc được định tuyến sang mạng con mới.
- **Việc khởi động lại sẽ bị từ chối trong khi cảm biến đang ghi hình.** Hãy dừng ghi hình trước (`pool-record --stop`). Quá trình ghi hình bắt đầu ngay trước khi khởi động lại sẽ bị đóng lại bởi quá trình này, và các mã lỗi `pool-set-network`, `pool-reboot` và `pool-update-firmware` sẽ kết thúc bằng một dòng `Warning:` ghi tên tệp ghi lại (kể cả khi cảm biến sau đó không quay trở lại).
- **Sau khi khởi động lại, Chloros chỉ kết nối lại chính cảm biến đó.** Nó sẽ chờ tại vị trí cảm biến đã từng ở -- và sau dòng `--auto` cũng dưới tên `daq-e-<id>.local` của nó -- để nhận phản hồi chứa tên cảm biến, tên máy chủ và mẫu, từ bộ điều hợp mạng (MAC) mà nó từng có, và kết nối lại tại địa chỉ đã trả lời. Phản hồi dưới tên đó chỉ được chấp nhận với địa chỉ MAC đó, do đó từ một máy tính kết nối với cảm biến qua bộ định tuyến, hoặc từ WSL2 trong mạng NAT mặc định của nó, một cảm biến đã được DHCP chuyển sang địa chỉ mới sẽ không được kết nối lại: lệnh sẽ thông báo điều này và hiển thị lệnh `pool-connect --eth-host <its address> …` để khôi phục cảm biến với các cài đặt độ nhạy và phơi sáng ban đầu.
- **`pool-update-firmware` từ chối cảm biến mà nó không thể đọc được phiên bản firmware.** Một cảm biến mất thời gian lâu hơn thời gian chờ để kết nối lại sau khi nháy đèn không phải là đã gặp sự cố: một khi nó trả lời, nó sẽ được kết nối lại với các thiết lập của phiên bản firmware mới được điền vào, và lệnh sẽ cảnh báo rằng quá trình này diễn ra chậm.
- **Việc cập nhật firmware yêu cầu một kết nối TCP đến.** Cảm biến kết nối trở lại với máy tính này để thu thập hình ảnh. Chloros lắng nghe trên bộ điều hợp chuyển tiếp đến cảm biến (trên mọi bộ điều hợp chỉ khi không thể xác định chính xác bộ điều hợp nào) và chỉ phục vụ địa chỉ riêng của cảm biến; bất kỳ kết nối nào khác sẽ bị đóng mà không được phục vụ. Trên Windows, quy tắc tường lửa `Chloros Backend (TCP-In)` của trình cài đặt cho phép kết nối này đi qua (xem [Windows (.exe)](#windows-exe)); tường lửa chặn kết nối này sẽ khiến quá trình cập nhật thất bại với mã lỗi `… device never connected back on TCP <port> …`.
- **Trong môi trường WSL2, hãy sử dụng mạng phản chiếu** (`networkingMode=mirrored` trong `%UserProfile%\.wslconfig`). Trong chế độ NAT mặc định, địa chỉ của máy tính này trên mạng của cảm biến chính là địa chỉ của chính WSL, do đó mọi địa chỉ tĩnh đều bị từ chối (trừ trường hợp `--force`), luồng dữ liệu trực tiếp không thể đến được WSL và quá trình cập nhật firmware không thể hoàn tất, vì cảm biến kết nối trở lại với máy tính để nhận hình ảnh.
- **ID cảm biến và tên máy chủ không thay đổi.** Dù địa chỉ của nó là gì, một thiết bị DAQ-E vẫn là `daq-e-<id>` và `daq-e-<id>.local`, do đó `--eth-host daq-e-<id>.local` vẫn có thể tìm thấy nó.
- **Giao diện người dùng (GUI) có các thiết lập giống nhau**: tab Cảm biến Ánh sáng, thiết lập của cảm biến (biểu tượng bánh răng), Mạng. GUI duy trì kết nối riêng của mình với cảm biến, do đó nếu thay đổi cảm biến mà GUI đang mở từ chính GUI: việc khởi động lại từ CLI sẽ kết thúc phiên làm việc của GUI với cảm biến đó.
- Các mã tương ứng trong SDK là `DAQSensorSession.device_info()`, `set_network()`, `set_auto_stream()`, `reboot()` và `update_firmware()`.

---

## `chloros-cli project`

Mở, kết nối và điều khiển một dự án Chloros đã lưu (một thư mục chứa `cameras.json` + `sensors.json` + `project.json`). Mọi thao tác đều được xử lý qua hệ thống nền nên giao diện người dùng (GUI) và giao diện dòng lệnh (CLI) sẽ hiển thị trạng thái phần cứng giống hệt nhau.

### Tham chiếu lệnh con

| Lệnh con | Mục đích |
| --- | --- |
| `project open PATH [--relink FOLDER]` | In danh sách thiết bị của dự án (camera, mảng, cảm biến), đọc từ các tệp của dự án và kiểm tra các tệp nguồn của nó (xem [Dự án đã di chuyển](#a-moved-project---relink)). Lệnh này không mở dự án trong hệ thống nền: dự án vẫn mở ở đó, và các thiết bị cùng bản ghi của nó vẫn giữ nguyên trạng thái. Thoát khỏi `1` khi không tìm thấy bất kỳ tệp nguồn nào của dự án. |
| `project devices PATH [--reconnect]` | Liệt kê hoặc chạy lại quá trình phát hiện. |
| `project connect PATH [--cameras-only] [--sensors-only] [--packed {auto,on,off}]` | Kết nối mọi camera / mảng / cảm biến đã lưu. Mỗi mảng được kết nối theo cách tương tự như `lattice array-connect`: tối đa 600 giây, các bước thực hiện được in ra stderr (`[array <id>]    <s> s: <step>`; stdout là báo cáo JSON), và mảng nào không được kết nối trong khoảng thời gian đó sẽ bị hủy — các camera của mảng đó sẽ được đưa trở lại trạng thái ban đầu — và được báo cáo dưới `errors`, không bao giờ được giữ trạng thái đã kết nối. `--packed` thiết lập giao thức truyền tải 12-bit nén cho mỗi mảng (mặc định là `auto`; một dự án không lưu cài đặt riêng cho từng mảng; xem [Chế độ truyền tải 12-bit đóng gói](#packed-12-bit-transport---packed)). Mỗi cảm biến ánh sáng được mở trong nhóm DAQ của backend tại địa chỉ đã lưu kèm theo thời gian tích hợp, trung bình khung hình, phơi sáng tự động và giới hạn đã lưu: một cảm biến không chọn giới hạn sẽ nhận được `sunshine_cosine`, một cảm biến được lưu ở trạng thái trần (`none`) sẽ giữ nguyên trạng thái trần, và DAQ-E-S không bao giờ áp dụng giới hạn. Bảng điều khiển DAQ trên GUI không thuộc nhóm này: hãy đóng cảm biến tại đó trước. |
| `project capture PATH NAME [-o DIR] [--format FMT] [--exposure US] [--gain DB] [--prefix P] [--orient]` | Chụp đơn từ một camera hoặc mảng có tên (`--prefix` mặc định là `capture`). `--orient` (mảng): mảng lưu dữ liệu thô, giữ nguyên định dạng gốc — điều này ghi nhận rằng các sản phẩm mà `process` tạo ra từ đó phải được định hướng, như `array-capture --orient`. Mặc định: định dạng gốc. `project burst` / `stream` và các thành phần `capture` / `burst` / `stream` (`orient: true`) đều sử dụng cùng một tùy chọn. |
| `project burst PATH NAME [-n N] [-i S] [-o DIR] [--format FMT] [--exposure US] [--gain DB] [--prefix P] [--orient]` | Chuỗi N khung hình từ một camera hoặc mảng được chỉ định (`-n/--count` mặc định là 5; `-i/--interval` khoảng thời gian giữa các khung hình tính bằng giây, mặc định là 0). Chụp liên tiếp theo mảng sẽ loại bỏ các nhóm đồng bộ lặp lại (cơ chế phát hiện độ cũ) để mảng hoạt động không đầy đủ chu kỳ không thể trả về N bản sao của một khung hình; in kết quả cho mỗi lần lặp. Việc ghi hình bị lỗi giữa chừng sẽ dừng chuỗi chụp và thoát khỏi `1`, đồng thời thông báo số lượng khung hình đã được lưu trước đó. |
| `project stream PATH NAME [-n N] [--fps F] [-o DIR] [--format FMT] [--exposure US] [--gain DB] [--orient]` | Ghi dữ liệu vào đĩa theo vòng lặp: `--fps F` ghi một giây (mặc định là 3), một khung hình mỗi lần ghi từ một camera, một nhóm khung hình đồng bộ từ một mảng (với cơ chế giám sát độ cũ tương tự như `project burst`), cho đến khi `-n N` ghi (mặc định `0` = cho đến khi nhấn Ctrl+C, lúc đó chương trình sẽ hiển thị số khung hình đã lưu và thoát khỏi `0`, hoặc `1` khi không có khung hình nào được lưu hoặc tệp gặp lỗi; nếu nhấn Ctrl+C trước khi `-n N` bắt được dữ liệu thì chương trình sẽ thoát khỏi `130`). Việc ghi lại bị lỗi giữa chừng sẽ dừng luồng và thoát khỏi `1`, đồng thời thông báo số khung hình đã được lưu trước đó. `--poll-interval` đã bị loại bỏ và bị bỏ qua: nó vẫn được chấp nhận để các tập lệnh cũ vẫn chạy, kèm theo một lưu ý trên stderr. |
| `project sensor read PATH NAME [--json]` | Khung phổ mới nhất. |
| `project sensor log PATH NAME [--seconds SEC] [-o DIR] [--device-name NAME]` | Ghi tệp .daq cho `--seconds` (mặc định là 10). Bản ghi không chứa dữ liệu đo sẽ được báo cáo trên stderr và chương trình sẽ thoát khỏi `1`. |
| `project run PATH RECIPE.yaml` | Thực thi công thức ghi YAML/JSON. `--dry-run` kiểm tra tính hợp lệ mà không cần chạy. |
| `project align calibrate PATH NAME [--method M] [--model M] [--frames N] [--reference SN] [--max-features N] [--ratio-threshold F] [--ransac-threshold-px F] [--min-matches N] [--max-reproj-err-px F] [--checkerboard RxC] [--name PROFILE]` | Tính toán sự căn chỉnh cho một mảng — xem [bảng tùy chọn bên dưới](#project-align-calibrate-options). |
| `project align status PATH NAME [--json]` | Hiển thị cấu hình căn chỉnh hiện tại. |
| `project align clear PATH NAME` | Xóa cấu hình được lưu trong bộ nhớ đệm. |
| `project align tweak PATH NAME --serial SN [--dx N] [--dy N] [--rotation-deg N] [--scale N]` | Điều chỉnh biến đổi của một máy con (mặc định là `0`, `0`, `0`, `1.0`: không thay đổi). |
| `project align export PATH NAME --to FILE` | Lưu cấu hình dưới dạng JSON. |
| `project align import PATH NAME --from FILE [--no-validate]` | Tải cấu hình đã lưu. |

#### Tùy chọn `project align calibrate`

| Cờ | Mặc định | Mô tả |
| --- | --- | --- |
| `--method {feature_orb, feature_akaze, phase_correlation, checkerboard, manual}` | `feature_orb` | Phương pháp căn chỉnh. **Cách viết này khác với `lattice align-calibrate`**, vốn chấp nhận các dạng viết tắt `orb` / `akaze` / `phase`; hai lệnh này không thể thay thế cho nhau khi sử dụng cờ này. |
| `--model {translation, rigid, affine, homography}` | `affine` | Biến đổi mô hình để vừa khít. |
| `--frames N` | `1` | Đồng bộ hóa các ảnh chụp khung hình với giá trị trung bình. |
| `--reference SN` | máy chủ | Số sê-ri máy ảnh tham chiếu; mọi thành viên khác đều được biến dạng để phù hợp với nó. |
| `--max-features N` | `5000` | Giới hạn số lượng đặc trưng ORB. |
| `--ratio-threshold F` | `0.75` | Kiểm tra tỷ lệ. |
| `--ransac-threshold-px F` | `3.0` | Ngưỡng điểm trong của RANSAC. |
| `--min-matches N` | `15` | **Ngưỡng chất lượng** — từ chối kết quả giải nếu số điểm nội nằm dưới ngưỡng này. |
| `--max-reproj-err-px F` | `4.0` | **Ngưỡng chất lượng** — từ chối kết quả giải nếu sai số tái chiếu RMS vượt quá giá trị này. |
| `--checkerboard RxC` | — | Hình học bảng cho `--method checkerboard`, ví dụ: `9x6`. |
| `--name PROFILE` | trống | Tên cấu hình được nhúng trong tệp JSON đã lưu. **Không phải tên mảng** — đó là vị trí `NAME`. |

Hai tiêu chí chất lượng là lý do khiến quá trình hiệu chuẩn có thể giải quyết thành công nhưng vẫn
từ chối lưu: một cấu hình không đáp ứng bất kỳ tiêu chí nào trong số đó sẽ âm thầm gây sai lệch đăng ký cho mọi
lần chụp sau đó, do đó nó bị từ chối thay vì được lưu lại.

### Dự án đã di chuyển (`--relink`)

Tên `project.json` của dự án chỉ định tên từng tệp nguồn — mọi hình ảnh và bản ghi cảm biến ánh sáng
— bằng đường dẫn đầy đủ của chúng. Nếu di chuyển thư mục dự án hoặc toàn bộ thư mục dự án
sang một ổ đĩa khác, các đường dẫn đó vẫn trỏ đến vị trí cũ.

Khi mở dự án, ứng dụng sẽ tự động khắc phục những gì có thể: tệp từng nằm trong thư mục
của dự án sẽ được tìm kiếm trong thư mục mà dự án hiện đang nằm, và mọi đường dẫn được tìm thấy ở đó
sẽ được điều chỉnh lại và lưu lại (`project.json` trước tiên được sao lưu thành
`project.json.before-relink-<date>-<time>`). Quá trình này diễn ra mỗi khi mở — ứng dụng,
`chloros-cli project open`, `chloros-cli process` vào một dự án hiện có, các thư mục của SDK và `chloros-mcp`. Đường dẫn chỉ được điều chỉnh lại khi tệp đó thực sự tồn tại tại vị trí đó.

Các tệp nằm bên ngoài thư mục dự án — phiên Hub, nhập qua tùy chọn “Thêm thư mục” —
sẽ không thể tìm thấy theo cách đó. Hãy đặt tên cho thư mục mà chúng đang nằm:

```bash
chloros-cli project open "N:/Chloros Projects/Field_A" --relink "E:/backup/Hub Projects"
```

`--relink` sẽ lấy thư mục mà các tệp đã được chuyển đến, hoặc bất kỳ thư mục nào ở cấp trên của nó: thư mục của mỗi tệp bị thiếu
sẽ được tìm kiếm bên dưới thư mục đó dựa trên tên thư mục ở cuối
(`Hub Projects/<session>/raw/<serial>`, sau đó là `<session>/raw/<serial>`, ...), và các
tệp trong thư mục đó sẽ được lấy từ vị trí tìm thấy nhiều tệp nhất. `project open` hiển thị
những gì đã được chỉ định lại, số lượng tệp nguồn được tìm thấy và các thư mục chứa phần còn lại. Vẫn còn thiếu một số: hiển thị cảnh báo, thoát khỏi `0` -- dự án sẽ mở với phần còn lại. Không
tìm thấy: hiển thị lỗi về tên các thư mục đó và `--relink`, thoát khỏi `1`; phần nền cũng
từ chối mở dự án như vậy cho `process` hoặc `project devices --reconnect` (lỗi HTTP 409 `PROJECT_SOURCES_MISSING`).

### Ví dụ

```bash
# Open a project and see what it knows about
chloros-cli project open "/home/user/Chloros Projects/Field_A"

# Its hub captures moved to another drive: point it at them
chloros-cli project open "/home/user/Chloros Projects/Field_A" --relink "/media/backup/Hub Projects"

# Connect everything saved in the project
chloros-cli project connect "/home/user/Chloros Projects/Field_A"

# Capture from a named camera (defined in cameras.json)
chloros-cli project capture "/home/user/Chloros Projects/Field_A" FrontLeft \
  -o output/ --format tiff

# Capture from a named array
chloros-cli project capture "/home/user/Chloros Projects/Field_A" main_rig \
  -o output/ --format tiff

# Capture with overrides
chloros-cli project capture "/home/user/Chloros Projects/Field_A" main_rig \
  --exposure 5000

# Read a spectrum
chloros-cli project sensor read "/home/user/Chloros Projects/Field_A" Sky --json

# Record a DAQ log
chloros-cli project sensor log "/home/user/Chloros Projects/Field_A" Sky \
  --seconds 120 -o ~/Documents/spectra/

# Align an array (live)
chloros-cli project align calibrate "/home/user/Chloros Projects/Field_A" main_rig
chloros-cli project align status "/home/user/Chloros Projects/Field_A" main_rig

# Run a recipe
chloros-cli project run "/home/user/Chloros Projects/Field_A" recipe.yaml
```

### Ngôn ngữ kịch bản (Recipe DSL)

`project run RECIPE.yaml` chấp nhận tệp YAML hoặc JSON mô tả một chuỗi các hành động:

```yaml
# recipe.yaml
overrides:
  cameras:
    FrontLeft:
      exposure_us: 5000
      target_brightness: 80

stop_on_error: true
actions:
  - apply:
      name: FrontLeft
      settings:
        auto_exposure: "Off"
        gain_db: 6.0
        auto_gain: "Off"
  - wait: 2s
  - capture:
      name: FrontLeft
      output: pose_a/
      format: tiff
  - stream:
      name: main_rig
      count: 60
      fps: 5
      output: stream/
  - burst:
      name: main_rig
      count: 10
      interval: 0.5
      output: burst_a/
      format: tiff
  - sensor:
      name: Sky
      action: read
```

Các hành động được hỗ trợ: `apply`, `wait`, `capture`, `stream`, `burst`, `sensor`. `apply` lấy các khóa `cameras.json`, `settings` — `exposure_us`, `auto_exposure`, `gain_db`, `auto_gain`, `target_brightness`, … (các tên cũ như `exposure_time`, `exposure_auto`, `gain`, `gain_auto` vẫn được chấp nhận như các tên gọi khác). Một thiết lập phơi sáng, độ khuếch đại hoặc độ sáng mục tiêu được thiết lập theo cách này sẽ được coi là của bạn: hệ thống điều khiển phơi sáng tự động của Chloros (bao gồm cả phơi sáng được đo bằng DAQ) sẽ không can thiệp vào thiết lập đó. Một lần phơi sáng thủ công sẽ giữ nguyên mức độ khuếch đại như ban đầu: ở chế độ tự động, máy ảnh sẽ tiếp tục điều chỉnh mức độ khuếch đại (trong toàn bộ phạm vi) hướng tới độ sáng mục tiêu cho đến khi bạn thiết lập `gain_db` hoặc `auto_gain: "Off"`. Lệnh `auto_exposure: "Continuous"` sau đó sẽ chuyển quyền kiểm soát phơi sáng trở lại chế độ tự động, và lệnh `target_brightness_auto: true` sẽ chuyển mục tiêu đã khóa trở lại chế độ mục tiêu tự động củChloros. Hành động `burst` yêu cầu tham số `name` (bắt buộc), `count` (mặc định là 5), `interval` (giây, mặc định 0), `output`, `format`, và `settings` (cấu hình thiết lập cho từng camera giống như `apply`); các đợt chụp liên tiếp theo mảng sử dụng bộ giám sát nhóm đồng bộ mới giống như `project burst`.

Đối với bước `capture`, `burst` hoặc bước `stream`, giá trị mặc định của `output` là `output`. Một `output` tương đối — giá trị mặc định được bao gồm — sẽ được giải quyết dựa trên thư mục mà bạn chạy `chloros-cli project run` từ đó, chứ không phải thư mục của tệp công thức hay thư mục dự án (trong phiên bản 1.2.0, nó nằm trong thư mục cài đặt của backend).

Chạy lệnh:

```bash
chloros-cli project run "/path/to/project" recipe.yaml

# Dry-run to validate without firing hardware
chloros-cli project run "/path/to/project" recipe.yaml --dry-run
```

**Mỗi lần chạy đều được kiểm tra trước tiên dựa trên dự án.**Mỗi hành động phải có chính xác một trong các động từ trên làm khóa — `- capture: {name: FrontLeft}`, không phải `- action: capture` hay `- type: capture` — với `name`, dự án có `cameras.json` / `sensors.json`, một camera hoặc mảng cảm biến cho `apply` / `capture` / `burst` / `stream` và một cảm biến ánh sáng cho `sensor`; một `stream` yêu cầu `count` > 0 và một `wait` yêu cầu thời gian thực thi. Một công thức có bất kỳ lỗi nào như vậy sẽ**không** thực hiện bất kỳ hành động nào (`[recipe] error: …`, `[recipe] not run: …`, thoát `1`), ngay cả khi có `stop_on_error: false`. Các phím mà một bước không sử dụng là `[recipe] warning:`, cũng như mọi mảng hoặc cảm biến ánh sáng mà công thức điều khiển nhưng hiện không được kết nối. Một công thức không bao giờ kết nối thiết bị: hãy chạy `chloros-cli project connect PATH` trước. `--dry-run` in ra công thức, sau đó hiển thị các lỗi và cảnh báo tương tự, và `[recipe] valid: …` / `NOT valid: …`, không thực thi bất kỳ lệnh nào và thoát khỏi `1` khi công thức gặp lỗi.

---

## Biến môi trường

| Biến | Tác dụng |
| --- | --- |
| `CHLOROS_BACKEND_URL` | Ghi đè URL của backend (mặc định là `http://127.0.0.1:5000`) — **chỉ được hỗ trợ bởi các nhóm lệnh `lattice`, `project` và `daq pool-*`** (`lattice network-analysis` sử dụng `--backend-url` thay thế). Các lệnh cốt lõi (`process`, `login`, `logout`, `status`, `export-status`, `time-sync`, `selftest`) sẽ cố định giá trị của `http://127.0.0.1:<port>` và bỏ qua biến này (giá trị IPv4 cố định giúp tránh được hình phạt khoảng 2 giây cho mỗi yêu cầu (Windows `localhost`→`::1`)), do đó chúng luôn nhắm đến máy cục bộ. |
| `CHLOROS_ARRAY_ALLOW_OVERSUBSCRIBED` | Được thiết lập trong môi trường của backend. `1` giảm mức độ từ chối do quá tải — một kết nối mảng với `pin_resolution`, hoặc một [lệnh đa camera tạm thời](#multi-camera-transient-sessions) (`error_code` `ARRAY_OVERSUBSCRIBED`) — thành một cảnh báo mạnh, chấp nhận mất gói tin GVSP. Chỉ dành cho mục đích thử nghiệm — xem [Mô hình fps & burst của mảng](#array-fps--burst-model). |
| `CHLOROS_ARRAY_PACKED12` | Đặt trong môi trường của backend: `on` hoặc `off` đặt giá trị mặc định là [vận chuyển 12-bit đóng gói](#packed-12-bit-transport---packed) cho mọi mảng (mặc định là `auto`). Một thiết lập rõ ràng bằng `--packed` vẫn có ưu tiên cao hơn. |
| `CHLOROS_DISABLE_UPDATE_CHECK` | `1` (hoặc `true` / `yes`) sẽ tắt tính năng [kiểm tra cập nhật tự động](#chloros-cli-update) của Linux CLI khi khởi động. |
| `CHLOROS_CLI_MODE` | Được thiết lập bởi chính CLI; yêu cầu backend kích hoạt xử lý song song. |
| `CHLOROS_GVSP_PROBE_FALLBACK` | `0` bỏ qua lệnh kiểm tra dự phòng GVSP (chỉ kết quả ICMP). **Thiết lập này tắt tính năng jumbo, không chỉ đơn thuần là giảm bớt thông báo trong nhật ký** — camera chỉ trả lời các ping DF tối đa 1500 trên mỗi đường dẫn, do đó, kiểm tra này là cách duy nhất để phát hiện jumbo. Tiết kiệm khoảng 1 giây cho mỗi camera trên mỗi lần kết nối; tiêu tốn khoảng 1,45 lần giới hạn băng thông nếu mạng *có thể* truyền gói jumbo. SDK sẽ cảnh báo khi bạn thiết lập tùy chọn này. |
| `CHLOROS_GVSP_SESSION_REUSE` | Cài đặt trong môi trường backend: `0` đo kích thước gói tin trên mỗi lần kết nối thay vì một lần cho mỗi camera trên mỗi phiên backend. |
| `CHLOROS_GVSP_PACKET_SIZE_FORCE` | Khóa kích thước gói tin GVSP ở N byte; bỏ qua hoàn toàn quá trình dò tìm. Nên ưu tiên thiết lập theo từng lệnh (`CHLOROS_GVSP_PACKET_SIZE_FORCE=9000 chloros-cli …`) thay vì thiết lập vĩnh viễn: kích thước được cố định sẽ ngừng thích ứng với mạng phía trước, và việc cố định giá trị 9000 trên một đường dẫn không thể truyền gói jumbo sẽ khiến **mọi** lần ghi hình đều hết thời gian chờ với `SC_ERR_TIMEOUT -1011`. |
| `CHLOROS_TRANSIENT_AE_SETTLE_S` | Đặt trong môi trường của backend: số giây tối đa cho một [lệnh đa camera tạm thời](#multi-camera-transient-sessions), hoặc một `lattice capture` của camera không được giữ lại, dành thời gian để thiết lập độ phơi sáng cho từng camera từ cảnh trước khi nó lưu giữ một khung hình (mặc định `5`). `0` tắt tính năng này: chế độ phơi sáng tự động riêng của từng camera sẽ hoạt động từ khung hình đầu tiên, bắt đầu từ giới hạn phơi sáng của camera đó. |
| `CHLOROS_LOG_DIR` | Cài đặt trong môi trường của backend: thư mục chứa nhật ký chi tiết. Mặc định là `%LOCALAPPDATA%\Chloros\logs` trên Windows, `~/.cache/chloros/logs` trên Linux (`$XDG_CACHE_HOME/chloros/logs` khi tham số đó được thiết lập). Dịch vụ gốc của gói .deb thiết lập `/var/log/chloros`, có thể đọc được bởi nhóm `adm` — xem [Chạy backend dưới dạng dịch vụ](#running-the-backend-as-a-service-headless). |
| `TMPDIR` (Linux) | Vị trí lưu trữ các tệp tạm thời. Giao diện dòng lệnh (CLI) sẽ tự động sử dụng `/mnt/ssd/tmp` nếu có. |

---

## Mã thoát

| Mã | Ý nghĩa |
| --- | --- |
| `0` | Thành công. |
| `1` | Lỗi chung (hầu hết các lỗi của lệnh con). Lệnh ghi lại cũng sẽ thoát với mã `1` khi bất kỳ tệp nào được yêu cầu ghi không thể ghi được, ngay cả khi các tệp khác đã được lưu, và `array-burst` / `array-record` thoát khỏi `1` khi trình ghi báo cáo lỗi — xem [Các tệp không ghi được](#files-that-fail-to-write). `process` cũng thoát khỏi `1` khi không thể xác minh các tệp xuất của một lần chạy, hoặc khi mất một hình ảnh, mặc dù các sản phẩm khác đã được ghi. |
| `2` | Lỗi tham số (cờ không xác định hoặc thiếu; cũng bao gồm `time-sync set-priority` khi không có mức ưu tiên nào). |
| `130` | Bị ngắt bởi Ctrl+C: bất kỳ lệnh nào bị ngắt đột ngột — `lattice`, `project`, `daq` và `process` đều như vậy (`process` sau khi phần nền đã dừng quá trình chạy, xem [Ghi chú về `process`](#notes)). Lệnh có phím tắt dừng thông thường là Ctrl+C — `lattice array-capture --continuous` / `--interval`, `lattice viewer`, `lattice continuous`, `multi-stream`, `align-stream`, `array-record`, `array-burst` và `project stream` với `-n 0` — sẽ thoát khỏi chương trình dựa trên hành động thực hiện: `0` khi mọi thứ đã được ghi xong, `1` khi có bất kỳ lỗi nào xảy ra. |
| `143` / `129` | Bị ngắt bởi tín hiệu SIGTERM / SIGHUP (128 cộng với mã tín hiệu), sau khi quá trình dọn dẹp của lệnh hoàn tất: công việc đang chạy dừng lại và các camera được thu hồi, quá trình xử lý trong nền cũng dừng lại. Một `lattice continuous` hoặc bản ghi khác có cách dừng thông thường là Ctrl+C cũng sẽ thoát khỏi `143`, chứ không phải `0`. |

`lattice network-analysis` sẽ thoát khỏi `1` đối với các kết quả `needs_force_slip` và `error`, và `selftest` sẽ thoát khỏi `1` trừ khi cả bảy kiểm tra đều thành công.

---

## Hướng dẫn khắc phục sự cố

- **"Yêu cầu đăng nhập"** → Chạy `chloros-cli login EMAIL PASSWORD` một lần trên máy này.
- **"Thư mục dự án quá sâu đối với máy tính này"** → Tính năng đường dẫn dài của Windows bị tắt trên máy này (cài đặt mặc định của Windows), do đó không đường dẫn nào được vượt quá 260 ký tự, và các sản phẩm nằm cách thư mục dự án tối đa khoảng 130 ký tự. Không có dữ liệu nào được xử lý. Thông báo này hiển thị thư mục dự án dài nhất vẫn đáp ứng yêu cầu: hãy sử dụng thư mục ngắn hơn (`-o` hoặc `chloros-cli set-project-folder`), hoặc bật tính năng đường dẫn dài củWindows và khởi động lại Chloros. Trình cài đặt Windows cung cấp tùy chọn này dưới tên "Bật đường dẫn dài của Windows", được chọn mặc định; nếu không, hãy sử dụng Chính sách Nhóm: *Cấu hình Máy tính > Mẫu Quản trị > Hệ thống > Hệ thống Tệp > Bật đường dẫn dài Win32*.
- **"backend unreachable"** → Khởi động ứng dụng máy tính để bàn Chloros (Windows), hoặc tự khởi động backend: `"C:\Program Files\Chloros\backend\chloros-backend.exe"` trên Windows (thư mục `backend` trong thư mục cài đặt của bạn; nó không nằm trong thư mục `PATH`), `chloros-backend` trên Linux. Hoặc kiểm tra `CHLOROS_BACKEND_URL` nếu truy cập từ xa.
- **Các lệnh `lattice` báo lỗi "Không tìm thấy trình điều khiển camera LATTICE"** → Phần backend không thể tải được runtime camera LATTICE đi kèm khi cài đặt. Mọi lệnh `lattice` đều được thực thi trên backend; chính giao diện dòng lệnh (CLI) không chứa trình điều khiển camera. Cài đặt lại Chloros: trình cài đặt Windows và gói .deb sẽ cài đặt runtime bên cạnh backend.
- **Mục "Array connect" / "Array Settings" hiển thị thông báo "FRAMES WILL DROP" hoặc "Reduce ROI to enable"** → Vòng nhận (receive ring) của NIC máy chủ quá nhỏ (thường được đặt lại về 32 sau khi cập nhật trình điều khiển NIC). Xem [Cài đặt & Tối ưu hóa NIC máy chủ](#host-nic-setup--tuning-lattice-arrays) — thiết lập `ReceiveBufferLen=256`, `PendingReceives=64`.
- **Máy bị treo khi khởi động lại/tắt máy, sau đó WMI `Invalid class` / NIC không kích hoạt được / thiếu ổ USB** → Trình điều khiển bộ điều hợp USB 10GbE lỗi thời gây ra lỗi `DRIVER_POWER_STATE_FAILURE` (Màn hình xanh `0x9F`). Cập nhật trình điều khiển bộ điều hợp — xem [Cài đặt & Tối ưu hóa NIC Máy chủ](#host-nic-setup--tuning-lattice-arrays).
- **Hệ điều hành Linux: các bản ghi thuộc quyền sở hữu của người dùng root, do đó bạn không thể xóa chúng, và nhật ký của backend không nằm trong thư mục của bạn `~/.cache`** → Backend chạy với quyền root thông qua `chloros-backend.service`. Chuyển sang `chloros-backend@<user>.service`, nơi dịch vụ backend chạy dưới quyền của bạn — xem [Chạy dịch vụ backend dưới dạng dịch vụ](#running-the-backend-as-a-service-headless). Cho đến lúc đó, nhật ký của dịch vụ root nằm trong `/var/log/chloros` (có thể đọc được bởi nhóm `adm`).
- **Hệ điều hành Linux: một đơn vị backend là `failed` với thông báo "Một phiên bản khác đang chạy" (trạng thái thoát 98)** → Một đơn vị backend khác đã chiếm cổng này: chỉ có một đơn vị chạy tại một thời điểm. Nếu hai đơn vị backend được kích hoạt, hãy giữ lại một đơn vị: `sudo systemctl disable --now <unit>` cho đơn vị bạn không sử dụng — xem [Chạy backend dưới dạng dịch vụ](#running-the-backend-as-a-service-headless). Nếu chỉ có đơn vị này được kích hoạt, một backend được khởi động bên ngoài systemd (bằng `chloros-backend`, `chloros-cli` hoặc một skript SDK) đang chiếm cổng đó: dừng backend đó, sau đó chạy `sudo systemctl start <unit>`.
- **Cảnh báo về vùng hoán đổi trên Jetson** → Thêm vùng hoán đổi dựa trên tệp; giao diện dòng lệnh (CLI) sẽ hiển thị chính xác các lệnh `fallocate` / `swapon`.
- **"backend không thể truy cập" từ lệnh `lattice`, `project` hoặc `daq`** → không có backend nào đang chạy: các lệnh này không bao giờ khởi động backend. Chạy lệnh `chloros-cli backend status` và xem [Khởi động Backend](#start-the-backend).
- **"`daq net ...` không khả dụng trong Chloros"** (Linux và Jetson) → Một số cài đặt riêng của DAQ-E được dành riêng cho dịch vụ MAPIR. Cài đặt mạng, khởi động lại và cập nhật firmware tương ứng với các lệnh `pool-set-network`, `pool-reboot` và `pool-update-firmware`; liên hệ bộ phận hỗ trợ MAPIR để được hỗ trợ thêm.
- **"Các lệnh điều khiển phần cứng trực tiếp của DAQ yêu cầu gói daq đầy đủ"** (Windows) → Sử dụng `pool-*`, cho phép điều khiển cùng một cảm biến thông qua phần mềm nền, hoặc `chloros_sdk.connect_daq_sensor()` từ Python.

---

## Xem thêm

- [Tham khảo SDK Python](sdk-reference.md) — tương đương lập trình của mọi lệnh CLI.
- [Cảm biến ánh sáng DAQ](https://mapir.gitbook.io/chloros/daq-light-sensors) trong tài liệu hướng dẫn Chloros — các cảm biến, giới hạn và phạm vi hiệu chuẩn của chúng, ghi dữ liệu, kết nối mạng DAQ-E.
- [Camera LATTICE](https://mapir.gitbook.io/chloros/lattice-cameras/lattice) trong tài liệu hướng dẫn Chloros — kết nối, cài đặt camera, chế độ chụp, mảng camera.
- Tài liệu trực tuyến: `https://mapir.gitbook.io/chloros/cli`
