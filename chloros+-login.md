# Cloros+ Đăng nhập

## Chloros và Chloros (Trình duyệt) Đăng nhập

Menu thanh bên <img src=".gitbook/assets/icon_user.JPG" alt="" data-size="line"> của người dùng cho phép bạn đăng nhập vào tài khoản Chloros+ của mình và mở khóa các tính năng bổ sung.

Khi đăng nhập chi tiết tài khoản của bạn sẽ được hiển thị:

<figure><img src=".gitbook/assets/user_account.JPG" alt="" width="375"><figcaption></figcaption></figure>

## CLI Đăng nhập

Đăng nhập bằng thông tin đăng nhập Chloros+ của bạn để kích hoạt xử lý CLI.

**Cú pháp:**

```bash
chloros-cli login <email> <password>
```

**Ví dụ:**

```powershell
chloros-cli login user@example.com 'MyP@ssw0rd123'
```

{% gợi ý style="warning" %}
**Ký tự đặc biệt**: Sử dụng dấu ngoặc đơn xung quanh mật khẩu chứa các ký tự như `$`, `!` hoặc dấu cách.
{% endhint %}

**Đầu ra:**

<figure><img src=".gitbook/assets/cli login_w.JPG" alt=""><figcaption></figcaption></figure>

### Gói hết hạn

Gói hết hạn trong GUI hiển thị khi giấy phép của bạn không còn hiệu lực. Đối với đăng ký định kỳ hàng tháng, thời gian hết hạn là vào cuối tháng. Đối với đăng ký hàng năm, thời gian là một năm sau khi bạn bắt đầu đăng ký. Việc kiểm tra giấy phép yêu cầu kết nối Internet hàng tháng để xác minh, với thời gian gia hạn 30 ngày.

### Giới hạn thiết bị

Mỗi gói Chloros+ cung cấp số lượng thiết bị đã đăng ký khác nhau. Mỗi thiết bị bạn đăng nhập bằng tài khoản Chloros+ sẽ được tính vào số lượng thiết bị đã đăng ký của bạn. Bạn có thể đổi tên và xóa thiết bị trên trang tài khoản Đám mây MAPIR của mình.

<table><thead><tr><th width="168.5999755859375" align="right">Chloros+ Plan</th><th align="center">COPPER</th><th align="center">BRONZE</th><th align="center">SILVER</th><th align="center">GOLD</th></tr></thead><tbody><tr><td align="right">Devices Supported</td><td align="center">2</td><td align="center">2</td><td align="center">5</td><td align="center">10</td></tr></tbody></table>
