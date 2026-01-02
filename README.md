# 🛡️ GeyserFloodgateFailSafe Plugin

## Plugin này làm gì?

Đây là plugin **bảo vệ server Minecraft** của bạn khỏi lỗi đăng nhập khi kết hợp Geyser + Floodgate + AuthMe.

### 📱 Giải thích đơn giản

Khi bạn chạy server cho phép cả **người chơi Java** (PC) và **người chơi Bedrock** (điện thoại, console) cùng chơi:

- **Geyser**: Cho phép người chơi Bedrock kết nối vào server Java
- **Floodgate**: Thêm prefix (ký tự đặc biệt) vào tên người chơi Bedrock, ví dụ: `PE_Steve` thay vì `Steve`
- **AuthMe**: Plugin đăng ký/đăng nhập tài khoản

### ⚠️ Vấn đề xảy ra khi không có plugin này

Đôi khi Floodgate bị lỗi và **không thêm prefix** vào tên người chơi Bedrock:

1. Người chơi Bedrock tên `Steve` đáng lẽ phải thành `PE_Steve`
2. Nhưng do lỗi, họ vẫn vào với tên `Steve` (không có prefix)
3. AuthMe nghĩ đây là người chơi Java thật
4. **Tai hại**: Họ có thể chiếm tài khoản của người chơi Java tên `Steve`!

### ✅ Plugin này giải quyết như thế nào?

Plugin sẽ **tự động kick (đuổi)** bất kỳ ai:
- Kết nối qua đường Geyser/Bedrock
- NHƯNG tên không có prefix đúng (như `PE_`)

**Kết quả**: Chỉ những người chơi Bedrock có prefix đúng mới được vào server → Bảo vệ tài khoản Java!

---

## 📦 Cài đặt

### Yêu cầu
- Server Minecraft (Spigot/Paper 1.16+)
- Plugin Geyser và Floodgate đã cài đặt

### Các bước cài đặt

1. **Tải file plugin** (file `.jar`)
2. **Copy vào thư mục `plugins`** của server
3. **Khởi động lại server**
4. **Mở file cấu hình** tại `plugins/GeyserFloodgateFailSafe/config.yml`
5. **Chỉnh prefix** cho đúng với Floodgate của bạn (xem bên dưới)

---

## ⚙️ Cấu hình quan trọng

Mở file `config.yml` và chỉnh các mục sau:

### 1️⃣ Prefix (BẮT BUỘC phải đúng!)

```yaml
floodgate-prefix: "PE_"
```

**Đây là prefix mà Floodgate thêm vào tên người chơi Bedrock.**

- Mở file cấu hình Floodgate của bạn để xem prefix là gì
- Thường là: `PE_`, `.`, `*`, `_`, v.v.
- ⚠️ **PHẢI GIỐNG HỆT** với cấu hình Floodgate!

**Ví dụ:**
| Cấu hình Floodgate | Cấu hình plugin này |
|-------------------|---------------------|
| `prefix: "PE_"` | `floodgate-prefix: "PE_"` |
| `prefix: "."` | `floodgate-prefix: "."` |
| `prefix: "*"` | `floodgate-prefix: "*"` |

### 2️⃣ Tin nhắn khi bị kick

```yaml
kick-message:
  - "&cLỗi đồng bộ prefix."
  - "&7Vui lòng vào lại sau."
```

Bạn có thể thay đổi nội dung tin nhắn. Dùng `&` để đổi màu:
- `&c` = đỏ
- `&7` = xám
- `&a` = xanh lá
- `&e` = vàng

### 3️⃣ Cho phép một số người bỏ qua kiểm tra

```yaml
bypass-usernames:
  - "admin"
  - "owner"
```

Những tên trong danh sách này sẽ **không bị kiểm tra** (cả Java và Bedrock).

### 4️⃣ Chế độ kiểm tra

```yaml
mode: "STRICT"
```

| Chế độ | Mô tả |
|--------|-------|
| `STRICT` | Kiểm tra chặt nhất (khuyến nghị) |
| `API_ONLY` | Chỉ dùng API Geyser/Floodgate |
| `HOST_ONLY` | Chỉ kiểm tra địa chỉ IP |

**Nếu không chắc, cứ để `STRICT`.**

---

## 🎮 Lệnh trong game

| Lệnh | Mô tả | Quyền cần có |
|------|-------|--------------|
| `/gffailsafe reload` | Tải lại cấu hình | `gffailsafe.admin` |
| `/gffailsafe status` | Xem trạng thái plugin | `gffailsafe.admin` |

---

## 🔐 Quyền (Permissions)

| Quyền | Mô tả |
|-------|-------|
| `gffailsafe.admin` | Dùng lệnh `/gffailsafe` |
| `gffailsafe.exempt` | Bỏ qua kiểm tra (không bị kick) |

---

## 📋 Xem log ai bị chặn

Plugin tự động ghi lại ai bị chặn vào file:
```
plugins/GeyserFloodgateFailSafe/blocked.log
```

Mỗi dòng ghi lại: thời gian, tên người chơi, IP, lý do bị chặn.

---

## ❓ Câu hỏi thường gặp

### Q: Người chơi Bedrock hợp lệ bị kick, phải làm sao?
**A:** Kiểm tra xem `floodgate-prefix` đã đúng với Floodgate chưa. Prefix phải **GIỐNG HỆT**.

### Q: Tôi muốn tắt plugin tạm thời?
**A:** Xóa file `.jar` khỏi thư mục plugins và restart server.

### Q: Prefix của tôi là dấu chấm `.` nhưng không hoạt động?
**A:** Trong file YAML, dấu chấm cần để trong ngoặc kép: `floodgate-prefix: "."`

### Q: Làm sao biết Floodgate prefix của mình là gì?
**A:** Mở file `plugins/floodgate/config.yml` và tìm dòng `prefix:`.

---

## 🔧 Gặp vấn đề?

1. Bật chế độ debug trong `config.yml`:
   ```yaml
   debug: true
   ```
2. Khởi động lại server
3. Xem log console để biết chi tiết

---

## 📝 Tóm tắt

| Tính năng | Mô tả |
|-----------|-------|
| 🛡️ Bảo vệ tài khoản | Ngăn người chơi Bedrock chiếm tài khoản Java |
| 🔄 Tự động | Hoạt động tự động, không cần can thiệp |
| 📋 Ghi log | Ghi lại tất cả lần chặn để kiểm tra |
| ⚙️ Linh hoạt | Nhiều tùy chọn cấu hình |
| 🎮 Dễ dùng | Chỉ cần cài và chỉnh prefix |

---

**Được tạo để bảo vệ server Minecraft của bạn! 🎮🛡️**
