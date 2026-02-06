# 🔔 FrozenNotify

**FrozenNotify** là một plugin Minecraft mạnh mẽ giúp bạn gửi thông báo tự động đến Discord thông qua webhook. Plugin hỗ trợ tạo các loại thông báo tùy chỉnh với embed Discord đẹp mắt và dễ dàng cấu hình.

---

## ✨ Tính năng chính

- 🎨 **Embed Discord tùy chỉnh** - Tạo thông báo đẹp mắt với màu sắc, hình ảnh, và fields
- 🔧 **Hệ thống Type linh hoạt** - Tạo nhiều loại thông báo khác nhau cho từng mục đích
- 🎯 **Placeholder thông minh** - Tự động điền thông tin người chơi, server, thời gian
- 🔄 **PlaceholderAPI** - Tích hợp với PlaceholderAPI để sử dụng placeholder từ plugin khác
- ⚡ **Rate Limiting** - Tránh spam webhook với hệ thống giới hạn tần suất
- 🔁 **Auto Retry** - Tự động thử lại khi gửi webhook thất bại
- 🌐 **Hỗ trợ Folia** - Tương thích với cả Paper và Folia

---

## 📦 Cài đặt

1. Tải file `.jar` của plugin
2. Đặt vào thư mục `plugins/` của server
3. Khởi động lại server
4. Cấu hình webhook Discord trong các file type

---

## 🚀 Hướng dẫn sử dụng

### 1️⃣ Tạo Webhook Discord

1. Vào server Discord của bạn
2. Chọn channel muốn nhận thông báo
3. Vào **Settings** → **Integrations** → **Webhooks**
4. Click **New Webhook** hoặc **Create Webhook**
5. Đặt tên và avatar cho webhook (tùy chọn)
6. Click **Copy Webhook URL**

### 2️⃣ Cấu hình Type

Mở file type trong `plugins/FrozenNotify/types/` (ví dụ: `xu.yml`, `client.yml`)

```yaml
meta:
  enabled: true  # Bật type này

webhook:
  url: "https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_TOKEN"  # Dán URL webhook
```

### 3️⃣ Reload Plugin

Sau khi chỉnh sửa cấu hình, chạy lệnh:
```
/fn reload
```

### 4️⃣ Gửi thông báo

**Cú pháp cơ bản:**
```
/fn <type> key=value key2=value2 ...
```

**Ví dụ thực tế:**

**Thông báo giao dịch xu:**
```
/fn xu amount=100 balance=500 type=deposit reason="Mua item"
```

**Thông báo phát hiện client:**
```
/fn client player=Steve client="Meteor Client" version="1.0"
```

---

## 📋 Các lệnh có sẵn

| Lệnh | Mô tả |
|------|-------|
| `/fn <type> key=value ...` | Gửi thông báo với type đã cấu hình |
| `/fn types` | Xem danh sách tất cả các type |
| `/fn help <type>` | Xem hướng dẫn chi tiết của type |
| `/fn reload` | Reload lại cấu hình plugin |

---

## 🎨 Tùy chỉnh Embed Discord

### Cấu trúc cơ bản

```yaml
payload:
  content: "Nội dung text thường"  # Text hiển thị trên embed
  embeds:
    - title: "📌 Tiêu đề"
      description: "Mô tả chi tiết"
      color: 3447003  # Màu viền bên trái (decimal)
      
      thumbnail:
        url: "{avatar}"  # Ảnh nhỏ bên phải
      
      fields:
        - name: "Tên field"
          value: "Giá trị"
          inline: true  # Hiển thị cùng hàng
      
      footer:
        text: "Footer text"
        icon_url: "URL icon"
      
      timestamp: true  # Thêm timestamp tự động
```

### Chọn màu cho Embed

Sử dụng [SpyColor](https://www.spycolor.com/) để chuyển đổi màu HEX sang Decimal:

| Màu | HEX | Decimal |
|-----|-----|---------|
| 🔴 Đỏ | #E74C3C | 15158332 |
| 🟢 Xanh lá | #2ECC71 | 3066993 |
| 🔵 Xanh dương | #3498DB | 3447003 |
| 🟡 Vàng | #F1C40F | 15844367 |
| 🟣 Tím | #9B59B6 | 10181046 |
| ⚫ Đen | #2C3E50 | 2899536 |

### Thêm Icon/Emoji

Bạn có thể thêm emoji Unicode vào title, description, và fields:

```yaml
title: "⚠️ **CẢNH BÁO**"
fields:
  - name: "👤 Người chơi"
    value: "{player}"
  - name: "💰 Số tiền"
    value: "{amount}"
```

### Sử dụng Markdown

Discord hỗ trợ markdown trong embed:

```yaml
description: |
  **Bold text**
  *Italic text*
  __Underline__
  ~~Strikethrough~~
  `Code`
  ```Code block```
  > Quote
  [Link](https://example.com)
```

---

## 🔧 Placeholder có sẵn

### Placeholder tự động (cho player)

Khi người chơi chạy lệnh, các placeholder sau tự động được điền:

- `{p}` / `{player}` - Tên người chơi
- `{uuid}` - UUID của người chơi
- `{avatar}` - Avatar thông minh (crafatar nếu có uuid, minotar nếu không)
- `{avatar_crafatar}` - Avatar từ Crafatar (cần UUID)
- `{avatar_minotar}` - Avatar từ Minotar (dùng tên)

### Placeholder hệ thống

- `{server}` - Tên server (từ config.yml)
- `{time}` - Thời gian hiện tại

### Placeholder tùy chỉnh

Bạn có thể truyền bất kỳ giá trị nào qua lệnh:

```
/fn xu amount=100 balance=500 custom_field="Giá trị tùy chỉnh"
```

Sau đó sử dụng `{custom_field}` trong template.

### Giá trị mặc định

Sử dụng cú pháp `{key|default}` để đặt giá trị mặc định:

```yaml
value: "{reason|Không có lý do}"  # Nếu không có reason, hiển thị "Không có lý do"
```

---

## 📚 Tài liệu tham khảo

### Discord Embed Documentation
- [Discord Webhook Guide](https://discord.com/developers/docs/resources/webhook)
- [Discord Embed Visualizer](https://leovoel.github.io/embed-visualizer/) - Test embed trước khi áp dụng
- [Discord Markdown Guide](https://support.discord.com/hc/en-us/articles/210298617-Markdown-Text-101)

### Công cụ hữu ích
- [SpyColor](https://www.spycolor.com/) - Chuyển đổi màu HEX sang Decimal
- [Emoji Cheat Sheet](https://www.webfx.com/tools/emoji-cheat-sheet/) - Danh sách emoji Unicode
- [Minotar](https://minotar.net/) - Avatar Minecraft
- [Crafatar](https://crafatar.com/) - Avatar Minecraft chất lượng cao

---

## ⚙️ Cấu hình nâng cao

### Rate Limiting

Trong `config.yml`:

```yaml
ratelimit:
  enabled: true
  per_sender_seconds: 5  # Mỗi người chơi chỉ gửi 1 lần/5 giây
  per_type_seconds: 2    # Mỗi type chỉ gửi 1 lần/2 giây
```

### Webhook Settings

```yaml
webhook:
  timeout_ms: 5000       # Timeout cho mỗi request
  max_queue: 100         # Số lượng webhook tối đa trong hàng đợi
  concurrency: 5         # Số webhook gửi đồng thời
  max_retries: 3         # Số lần thử lại khi thất bại
  backoff_base_ms: 1000  # Thời gian chờ giữa các lần retry
```

### PlaceholderAPI

```yaml
papi:
  enabled: true          # Bật PlaceholderAPI
  require_context: false # Yêu cầu context cho placeholder
```

---

## 🎯 Ví dụ Type mẫu

### Type: Giao dịch xu

**File:** `types/xu.yml`

**Sử dụng:**
```
/fn xu amount=100 balance=500 type=deposit reason="Mua item"
```

**Kết quả:** Gửi embed thông báo giao dịch xu với đầy đủ thông tin người chơi, số tiền, số dư.

### Type: Phát hiện client

**File:** `types/client.yml`

**Sử dụng:**
```
/fn client player=Steve client="Meteor Client" version="1.0"
```

**Kết quả:** Gửi cảnh báo khi phát hiện người chơi sử dụng client không hợp lệ.

---

## 🛠️ Tạo Type mới

1. Copy một file type mẫu trong `plugins/FrozenNotify/types/`
2. Đổi tên file (ví dụ: `my-notification.yml`)
3. Chỉnh sửa cấu hình:
   - Đặt `meta.enabled: true`
   - Thêm webhook URL
   - Tùy chỉnh embed
   - Định nghĩa `required_args`
4. Chạy `/fn reload`
5. Test với `/fn my-notification key=value`

---

## ❓ Câu hỏi thường gặp

**Q: Webhook không gửi được?**
- Kiểm tra URL webhook có đúng không
- Kiểm tra `meta.enabled: true`
- Xem console có lỗi gì không
- Chạy `/fn reload` sau khi sửa config

**Q: Placeholder không hoạt động?**
- Kiểm tra tên placeholder có đúng không
- Đảm bảo truyền đủ required_args
- Bật debug mode trong config.yml để xem log

**Q: Làm sao để test embed trước khi áp dụng?**
- Sử dụng [Discord Embed Visualizer](https://leovoel.github.io/embed-visualizer/)
- Copy cấu hình embed của bạn và test trực quan

**Q: Có thể gửi nhiều embed trong 1 message không?**
- Có! Thêm nhiều embed trong mảng `embeds`:
```yaml
embeds:
  - title: "Embed 1"
  - title: "Embed 2"
```

---

## 📝 Quyền (Permissions)

| Permission | Mô tả | Mặc định |
|------------|-------|----------|
| `frozennotify.use` | Sử dụng lệnh /fn | OP |
| `frozennotify.types` | Xem danh sách types | OP |
| `frozennotify.reload` | Reload plugin | OP |

---

## 💡 Tips & Tricks

1. **Sử dụng thumbnail cho avatar người chơi:**
   ```yaml
   thumbnail:
     url: "https://minotar.net/avatar/{player}/128.png"
   ```

2. **Tạo màu gradient bằng cách thay đổi color:**
   - Mỗi embed có thể có màu khác nhau

3. **Sử dụng inline fields để tiết kiệm không gian:**
   ```yaml
   fields:
     - name: "Field 1"
       value: "Value 1"
       inline: true
     - name: "Field 2"
       value: "Value 2"
       inline: true
   ```

4. **Thêm link trong description:**
   ```yaml
   description: "[Click here](https://example.com) để xem thêm"
   ```

---

## 🤝 Hỗ trợ

Nếu bạn gặp vấn đề hoặc cần hỗ trợ, hãy:
- Kiểm tra console log để xem lỗi chi tiết
- Bật `debug: true` trong config.yml
- Đọc kỹ hướng dẫn trong file type mẫu

---

## 📄 License

Plugin này được phát triển bởi **Frozen**.

---

**Chúc bạn sử dụng plugin vui vẻ! 🎉**
