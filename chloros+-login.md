# Đăng nhập Chloros+

## Đăng nhập qua giao diện đồ họa (GUI)

Menu bên cạnh <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> cho phép bạn đăng nhập vào tài khoản Chloros+ của mình và mở khóa các tính năng bổ sung.

**Bạn chỉ cần đăng nhập một lần trên mỗi máy.** GUI, CLI và Python SDK chia sẻ cùng một phiên làm việc được lưu trong bộ nhớ đệm — việc đăng nhập qua GUI trên máy tính để bàn cũng sẽ kích hoạt CLI và SDK trên máy tính đó (và ngược lại thông qua `chloros-cli login`).

Khi đã đăng nhập, thông tin tài khoản của bạn sẽ được hiển thị:

<figure><img src=".gitbook/assets/user_account.JPG" alt="" width="375"><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: re-shoot the logged-in user account panel in Chloros 1.3.0 — plan name display and the registered-device list UI may have changed; must show plan name, expiration, and device list. -->

## Các gói dịch vụ

| Gói | `plan_id` | Loại |
| --- | --- | --- |
| Iron | `0` | Miễn phí |
| Copper | `1` | Trả phí (Chloros

+) |
| Bronze | `2` | Trả phí (Chloros

+) |
| Silver | `3` | Trả phí (Chloros

+) |
| Vàng | `4` | Trả phí (Chloros

+) |

Xem [các gói và giá cả](https://cloud.mapir.camera/pricing) để biết chi tiết về những gì mỗi gói trả phí bao gồm.

### Quyền truy cập CLI

/SDK

yêu cầu gói trả phí

Việc truy cập CLI

và Python

SDK

yêu cầu **bất kỳ gói Chloros

+ trả phí nào (từ Copper trở lên)**. Quy định này được áp dụng**từ phía máy chủ** — mọi yêu cầu CLI

/SDK

đều phải kèm theo cả phiên hoạt động và gói trả phí:

| Trạng thái HTTP

| `error_code` | Ý nghĩa | Cách khắc phục |
| --- | --- | --- | --- |
| `401` | `AUTH_REQUIRED` | Chưa đăng nhập trên máy này | `chloros-cli login <email> <password>` |
| `403` | `PLAN_UPGRADE_REQUIRED` | Đã đăng nhập, nhưng cấp độ gói quá thấp (cấp độ Iron miễn phí) | Nâng cấp lên bất kỳ gói Chloros

+ trả phí nào |

`chloros-cli status` vẫn có thể truy cập được ở gói miễn phí, vì vậy bạn luôn có thể xem gói dịch vụ hiện tại của mình và lý do tại sao quyền truy cập bị từ chối.

### Giới hạn thiết bị kết nối theo từng gói

Mỗi gói dịch vụ đều giới hạn số lượng camera LATTICE và cảm biến ánh sáng DAQ có thể kết nối trực tiếp cùng lúc:

| Gói | Camera LATTICE | Cảm biến ánh sáng DAQ |
| --- | --- | --- |
| Iron (miễn phí / chưa đăng nhập) | 4 | 2 |
| Copper / Bronze | 6 | 3 |
| Silver | 10 | 6 |
| Gold | 20 | 12 |

## Đăng nhập CLI



Đăng nhập bằng thông tin đăng nhập Chloros

+ của bạn để kích hoạt xử lý CLI

. Trên hệ điều hành Linux

(không có giao diện đồ họa), đây là cách duy nhất để kích hoạt giấy phép của bạn.

**Cú pháp:**

```bash
chloros-cli login <email> <password>
```

{% hint style="info" %}
**Người dùng SDK**: Python

SDK

cũng cung cấp phương thức lập trình `logout()` để xóa thông tin đăng nhập được lưu trong bộ nhớ đệm. Xem [Tài liệu tham khảo SDK

](reference/sdk-reference.md) để biết thêm chi tiết.
{% endhint %}

**Ví dụ:**

```powershell
chloros-cli login user@example.com 'MyP@ssw0rd123'
```

{% hint style="warning" %}
**Ký tự đặc biệt**: Sử dụng dấu nháy đơn để bao quanh mật khẩu chứa các ký tự như `$`, `!` hoặc khoảng trắng.
{% endhint %}

**Kết quả:**

<figure><img src=".gitbook/assets/cli login_w.JPG" alt=""><figcaption></figcaption></figure>
<!-- SCREENSHOT-UPDATE: re-shoot the CLI login output — the banner now prints "Chloros CLI 1.3.0"; capture a successful login with the current output format. -->

### Lưu trữ thông tin đăng nhập

Thông tin đăng nhập và cấu hình được lưu trong thư mục `.chloros` thuộc thư mục chính của người dùng trên **tất cả các nền tảng**:

| Nền tảng | Đường dẫn bộ nhớ đệm thông tin đăng nhập |
| --- | --- |
| **Hệ điều hành Windows** | `%USERPROFILE%\.chloros\` |
| **Hệ điều hành Linux** | `~/.chloros/` |

Việc cài đặt phiên bản mới hơn lên phiên bản hiện có sẽ giữ lại thông tin đăng nhập của bạn trên Windows

(trình cài đặt) và trên Linux

(thư mục `.deb`, bao gồm cả `chloros-cli update`). Việc gỡ cài đặt Chloros

— hoặc xóa gói Linux

— sẽ xóa thông tin đăng nhập được lưu trong bộ nhớ đệm, do đó bạn phải đăng nhập lại sau khi cài đặt mới.

### Hết hạn gói dịch vụ và thời gian ân hạn ngoại tuyến

Thời hạn hết hạn gói dịch vụ hiển thị trên giao diện người dùng (GUI) cho biết thời điểm giấy phép của bạn sẽ hết hiệu lực. Đối với các gói đăng ký hàng tháng, thời hạn hết hiệu lực là vào cuối tháng; đối với các gói đăng ký hàng năm, thời hạn hết hiệu lực là một năm kể từ khi bạn bắt đầu đăng ký.

ChChloros

xác thực giấy phép của bạn trực tuyến, nhưng việc sử dụng ngoại tuyến vẫn được hỗ trợ trong thời gian ân hạn:

* Các lần xác thực máy chủ thành công được lưu trong bộ nhớ đệm trong **5 phút**, do đó việc sử dụng bình thường sẽ rất ít khi yêu cầu xác thực giấy phép.
* Bộ nhớ đệm giấy phép đã ký và gắn với máy tính sẽ hỗ trợ các khoảng thời gian ngoại tuyến dài hơn: **30 ngày đối với các gói hàng tháng**, và**cho đến ngày hết hạn đăng ký của bạn (tối đa 365 ngày) đối với các gói hàng năm**.
* Khi thời gian ân hạn kết thúc, gói dịch vụ sẽ chuyển sang cấp độ Iron miễn phí cho đến khi máy có thể kết nối với máy chủ giấy phép ít nhất một lần; quyền truy cập sẽ được khôi phục sau lần kiểm tra thành công tiếp theo.

### Giới hạn thiết bị

Mỗi gói Chloros

+ cung cấp số lượng thiết bị đã đăng ký khác nhau. Mỗi thiết bị bạn đăng nhập bằng tài khoản Chloros

+ sẽ được tính vào số lượng thiết bị đã đăng ký của bạn. Bạn có thể đổi tên và xóa thiết bị trên trang tài khoản MAPIR

Cloud của mình.

<table><thead><tr><th width="168.5999755859375" align="right">Gói Chloros</th><th align="center">COPPER</th><th align="center">BRONZE</th><th align="center">SILVER</th><th align="center">GOLD</th></tr></thead><tbody><tr><td align="right">Các thiết bị được hỗ trợ</td><td align="center">2</td><td align="center">2</td><td align="center">5</td><td align="center">10</td></tr></tbody></table>

Số lượng thiết bị tối đa mà tài khoản của bạn được phép sử dụng được hiển thị trên trang tài khoản MAPIR Cloud của bạn. Việc đăng xuất khỏi một thiết bị sẽ giải phóng vị trí của thiết bị đó một cách đáng tin cậy, và một thiết bị đã được đăng ký luôn có thể đăng nhập lại ngay cả khi tài khoản đã đạt đến giới hạn số lượng thiết bị.
