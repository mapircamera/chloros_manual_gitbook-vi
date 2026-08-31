# Các ngôn ngữ được hỗ trợ

Chloroscung cấp hỗ trợ giao diện đầy đủ bằng **38 ngôn ngữ trên toàn thế giới**, giúp người dùng khắp nơi trên thế giới có thể sử dụng dễ dàng. Bạn có thể chuyển đổi ngôn ngữ ngay lập tức cả trên giao diện người dùng máy tính để bàn (Desktop GUI) và trên trang web dành cho người dùng (CLI).

Chloroshỗ trợ các ngôn ngữ sau:

| # | Ngôn ngữ | Tên gốc | Mã CLI |
|---|----------|-------------|----------|
| 1 | 🇺🇸 Tiếng Anh | English | `en` |
| 2 | 🇪🇸 Tiếng Tây Ban Nha | Español | `es` |
| 3 | 🇵🇹 Tiếng Bồ Đào Nha | Português | `pt` |
| 4 | 🇫🇷 Tiếng Pháp | Français | `fr` |
| 5 | 🇩🇪 Tiếng Đức | Deutsch | `de` |
| 6 | 🇮🇹 Tiếng Ý | Italiano | `it` |
| 7 | 🇯🇵 Tiếng Nhật | 日本語 | `ja` |
| 8 | 🇰🇷 Tiếng Hàn | 한국어 | `ko` |
| 9 | 🇨🇳 Tiếng Trung (Giản thể) | 简体中文 | `zh` |
| 10 | 🇹🇼 Tiếng Trung (Chữ phồn thể) | 繁體中文 | `zh-TW` |
| 11 | 🇷🇺 Tiếng Nga | Русский | `ru` |
| 12 | 🇳🇱 Tiếng Hà Lan | Nederlands | `nl` |
| 13 | 🇸🇦 Tiếng Ả Rập | العربية | `ar` |
| 14 | 🇵🇱 Tiếng Ba Lan | Polski | `pl` |
| 15 | 🇹🇷 Tiếng Thổ Nhĩ Kỳ | Türkçe | `tr` |
| 16 | 🇮🇳 Tiếng Hindi | हिंदी | `hi` |
| 17 | 🇮🇩 Tiếng Indonesia | Bahasa Indonesia | `id` |
| 18 | 🇻🇳 Tiếng Việt | Tiếng Việt | `vi` |
| 19 | 🇹🇭 Thái Lan | ไทย | `th` |
| 20 | 🇸🇪 Thụy Điển | Svenska | `sv` |
| 21 | 🇩🇰 Tiếng Đan Mạch | Dansk | `da` |
| 22 | 🇳🇴 Tiếng Na Uy | Norsk | `no` |
| 23 | 🇫🇮 Tiếng Phần Lan | Suomi | `fi` |
| 24 | 🇬🇷 Tiếng Hy Lạp | Ελληνικά | `el` |
| 25 | 🇨🇿 Tiếng Séc | Čeština | `cs` |
| 26 | 🇭🇺 Tiếng Hungary | Magyar | `hu` |
| 27 | 🇷🇴 Tiếng Rumani | Română | `ro` |
| 28 | 🇺🇦 Tiếng Ukraina | Українська | `uk` |
| 29 | 🇧🇷 Tiếng Bồ Đào Nha Brazil | Português Brasileiro | `pt-BR` |
| 30 | 🇭🇰 Tiếng Quảng Đông | 粵語 | `zh-HK` |
| 31 | 🇲🇾 Tiếng Mã Lai | Bahasa Melayu | `ms` |
| 32 | 🇸🇰 Tiếng Slovak | Slovenčina | `sk` |
| 33 | 🇧🇬 Tiếng Bulgaria | Български | `bg` |
| 34 | 🇭🇷 Tiếng Croatia | Hrvatski | `hr` |
| 35 | 🇱🇹 Tiếng Litva | Lietuvių | `lt` |
| 36 | 🇱🇻 Tiếng Latvia | Latviešu | `lv` |
| 37 | 🇪🇪 Tiếng Estonia | Eesti | `et` |
| 38 | 🇸🇮 Tiếng Slovenia | Slovenščina | `sl` |

## Cách thay đổi ngôn ngữ

### Trên ứng dụng máy tính để bàn Chloros

1. Mở cài đặt ứng dụng
2. Truy cập vào menu chọn ngôn ngữ
3. Chọn ngôn ngữ ưa thích từ danh sách
4. Giao diện sẽ được cập nhật ngay lập tức

### Trên Chloros CLI

Sử dụng lệnh `language` để xem hoặc thay đổi ngôn ngữ giao diện CLI:

```bash
# View current language
chloros-cli language

# Change to Spanish
chloros-cli language es

# Change to Chinese (Simplified)
chloros-cli language zh

# Change to Brazilian Portuguese
chloros-cli language pt-BR

# List all available languages
chloros-cli language --list
```

Để biết thêm chi tiết, hãy tham khảo [CLI tài liệu](CLI.md).

## Phạm vi hỗ trợ

Tất cả 38 ngôn ngữ đều được hỗ trợ đầy đủ trên:

* **Chloros Desktop** - Dịch toàn bộ giao diện người dùng (GUI)
* **Chloros CLI** - Giao diện dòng lệnh và thông báo đầu ra

Python SDK API và [tài liệu tham khảo](reference/sdk-reference.md) của nó được cung cấp bằng tiếng Anh.

Hỗ trợ ngôn ngữ đảm bảo rằng người dùng trên toàn thế giới có thể làm việc hiệu quả bằng ngôn ngữ mẹ đẻ của mình mà không gặp rào cản.
