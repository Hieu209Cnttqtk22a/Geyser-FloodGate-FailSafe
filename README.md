# 🛡️ GeyserFloodgateFailSafe Plugin

> **Hỗ trợ:** Spigot/Paper/Folia 1.20+

---

## 📖 Plugin này là gì?

Đây là plugin **bảo vệ tài khoản** cho server Minecraft khi bạn sử dụng **Geyser + Floodgate + AuthMe**.

---

## 🎮 Giải thích cho người mới (không cần biết code)

### Bối cảnh: Server "crossplay"

Bạn muốn cả **người chơi PC** và **người chơi điện thoại** cùng chơi chung 1 server:

| Loại người chơi | Thiết bị | Cách kết nối |
|-----------------|----------|--------------|
| **Java Edition** | PC, Mac, Linux | Kết nối trực tiếp |
| **Bedrock Edition** | Điện thoại, Xbox, Switch, PS | Cần qua **Geyser** |

### Các plugin liên quan

| Plugin | Công dụng |
|--------|-----------|
| **Geyser** | Cho phép người chơi Bedrock (điện thoại) kết nối vào server Java |
| **Floodgate** | Phân biệt người chơi Bedrock bằng cách thêm **ký tự đặc biệt** vào tên, ví dụ: `Steve` → `PE_Steve` |
| **AuthMe** | Hệ thống đăng ký/đăng nhập tài khoản |

### ⚠️ Vấn đề xảy ra (lý do cần plugin này)

**Đôi khi Floodgate bị lỗi** và không thêm ký tự đặc biệt vào tên người chơi Bedrock:

```
❌ Trường hợp lỗi:
   Người chơi Bedrock tên "Steve" 
   → Đáng lẽ phải thành "PE_Steve"
   → Nhưng vẫn giữ nguyên "Steve" (do Floodgate lỗi)
   
⚠️ Hậu quả:
   → AuthMe nghĩ đây là người chơi Java tên "Steve"
   → Nếu có người Java đã đăng ký tên "Steve"
   → Người Bedrock có thể đăng nhập vào tài khoản đó!
```

**Nói đơn giản:** Lỗi này có thể khiến người chơi Bedrock **vô tình hoặc cố ý chiếm tài khoản** của người chơi Java!

### ✅ Plugin này giải quyết như thế nào?

Plugin sẽ **kiểm tra mỗi người kết nối**:

```
📱 Người vào qua đường Geyser (Bedrock)?
   ├── Có prefix đúng (PE_Steve)? → ✅ Cho vào
   └── Không có prefix (Steve)?   → ❌ Kick ra ngay

💻 Người vào trực tiếp (Java)?
   ├── Tên bình thường (Steve)?      → ✅ Cho vào
   └── Tên có prefix (PE_Steve)?     → ❌ Kick (giả mạo Bedrock)
```

**Kết quả:** Bảo vệ tài khoản Java khỏi bị chiếm!

---

## 📦 Hướng dẫn cài đặt (5 bước)

### Bước 1: Kiểm tra yêu cầu

| Yêu cầu | Mô tả |
|---------|-------|
| Server | Spigot hoặc Paper phiên bản 1.16 trở lên |
| Java | Phiên bản 17 trở lên |
| Plugin | Geyser và Floodgate đã cài đặt |

### Bước 2: Tải plugin

Tải file `GeyserFloodgateFailSafe-1.0.5.jar`

### Bước 3: Cài đặt

1. Mở thư mục server của bạn
2. Vào thư mục `plugins`
3. Copy file `.jar` vào đây

### Bước 4: Khởi động server

1. Tắt server (nếu đang chạy)
2. Bật lại server
3. Plugin sẽ tự tạo file cấu hình

### Bước 5: Cấu hình (QUAN TRỌNG!)

1. Mở file `plugins/GeyserFloodgateFailSafe/config.yml`
2. Chỉnh `floodgate-prefix` cho đúng (xem phần bên dưới)
3. Lưu file
4. Gõ lệnh `/gffailsafe reload` trong game hoặc restart server

---

## ⚙️ Hướng dẫn cấu hình chi tiết

### 🔴 QUAN TRỌNG NHẤT: Chỉnh Prefix

Mở file `config.yml`, tìm dòng này:

```yaml
floodgate-prefix: "PE_"
```

**Prefix này PHẢI GIỐNG HỆT với cấu hình Floodgate của bạn!**

#### Cách tìm prefix của Floodgate:

1. Mở file `plugins/floodgate/config.yml`
2. Tìm dòng `prefix:`
3. Copy giá trị đó sang plugin này

#### Ví dụ:

| Trong Floodgate | Trong plugin này |
|-----------------|------------------|
| `prefix: "PE_"` | `floodgate-prefix: "PE_"` |
| `prefix: "."` | `floodgate-prefix: "."` |
| `prefix: "_"` | `floodgate-prefix: "_"` |
| `prefix: "*"` | `floodgate-prefix: "*"` |

⚠️ **Lưu ý:** Nếu prefix là ký tự đặc biệt (như `.` hoặc `*`), hãy để trong dấu ngoặc kép!

---

### 📋 Các tùy chọn khác trong config.yml

#### Chế độ kiểm tra (mode)

```yaml
mode: "STRICT"
```

| Chế độ | Mô tả | Khuyến nghị |
|--------|-------|-------------|
| `STRICT` | Kiểm tra toàn diện nhất | ✅ Mặc định, an toàn nhất |
| `API_ONLY` | Chỉ dùng API của Geyser/Floodgate | Dùng khi có lỗi false-positive |
| `HOST_ONLY` | Chỉ kiểm tra địa chỉ IP | Ít dùng |

**Nếu không chắc, cứ để `STRICT`!**

---

#### Tin nhắn khi bị kick

```yaml
kick-message:
  - "&cInvalid Connection."
  - "&7Vui lòng vào lại sau."
```

Thay đổi nội dung tùy ý. Mã màu:
- `&c` = 🔴 Đỏ
- `&e` = 🟡 Vàng  
- `&a` = 🟢 Xanh lá
- `&b` = 🔵 Xanh dương
- `&7` = ⚪ Xám
- `&f` = ⬜ Trắng

---

#### Danh sách người được bỏ qua (bypass)

```yaml
bypass-usernames:
  - "admin"
  - "owner"
```

Những tên này sẽ **KHÔNG bị kiểm tra**. Dùng cho admin hoặc người tin tưởng.

---

#### Ghi log ai bị chặn

```yaml
blocked-log:
  enabled: true
  file: "plugins/GeyserFloodgateFailSafe/blocked.log"
  include-ip: true
```

| Tùy chọn | Mô tả |
|----------|-------|
| `enabled` | `true` = ghi log, `false` = không ghi |
| `file` | Đường dẫn file log |
| `include-ip` | `true` = ghi cả địa chỉ IP |

---

#### Chế độ Debug

```yaml
debug: false
```

Đổi thành `true` khi cần xem chi tiết hoạt động của plugin (dùng để tìm lỗi).

---

## 🎮 Lệnh sử dụng

| Lệnh | Chức năng |
|------|-----------|
| `/gffailsafe reload` | Tải lại cấu hình (không cần restart server) |
| `/gffailsafe status` | Xem trạng thái plugin và API |

**Quyền cần có:** `gffailsafe.admin` (mặc định OP có sẵn)

---

## 🔐 Hệ thống quyền (Permissions)

| Quyền | Chức năng | Mặc định |
|-------|-----------|----------|
| `gffailsafe.admin` | Dùng lệnh `/gffailsafe` | OP |
| `gffailsafe.exempt` | Bỏ qua kiểm tra (không bao giờ bị kick) | Không ai |

---

## 📋 Xem ai đã bị chặn

Mở file:
```
plugins/GeyserFloodgateFailSafe/blocked.log
```

Mỗi dòng ghi lại:
- ⏰ Thời gian
- 👤 Tên người chơi
- 🌐 Địa chỉ IP (nếu bật)
- ❓ Lý do bị chặn

---

## ❓ Câu hỏi thường gặp (FAQ)

### ❓ Người chơi Bedrock hợp lệ bị kick liên tục?

**Nguyên nhân:** Prefix chưa đúng!

**Cách sửa:**
1. Mở `plugins/floodgate/config.yml` → tìm `prefix:`
2. Mở `plugins/GeyserFloodgateFailSafe/config.yml`
3. Chỉnh `floodgate-prefix:` cho GIỐNG HỆT
4. Gõ `/gffailsafe reload`

---

### ❓ Prefix là dấu chấm `.` nhưng không hoạt động?

**Cách sửa:** Trong file YAML, ký tự đặc biệt cần để trong ngoặc kép:

```yaml
# ❌ Sai
floodgate-prefix: .

# ✅ Đúng  
floodgate-prefix: "."
```

---

### ❓ Làm sao tắt plugin tạm thời?

**Cách 1:** Xóa/di chuyển file `.jar` khỏi thư mục plugins rồi restart server

**Cách 2:** Để prefix rỗng `""` → Plugin sẽ chặn TẤT CẢ Bedrock (chế độ an toàn tối đa)

---

### ❓ Plugin có ảnh hưởng người chơi Java không?

**Không** - Người chơi Java bình thường không bị ảnh hưởng.

**Ngoại trừ:** Nếu ai đó cố tình đặt tên có prefix (như `PE_Steve`) khi chơi Java → Sẽ bị kick (ngăn giả mạo Bedrock).

---

### ❓ Server dùng proxy (BungeeCord/Velocity)?

Plugin này được thiết kế cho **server đơn** (không có proxy phía trước). Nếu dùng proxy, có thể cần điều chỉnh cấu hình `geyser-source-hosts`.

---

## 🔧 Xử lý sự cố

### Bước 1: Bật chế độ Debug

Mở `config.yml`:
```yaml
debug: true
```

Sau đó gõ `/gffailsafe reload`

### Bước 2: Xem log

Mở console server hoặc file log để xem thông tin chi tiết mỗi lần có người kết nối.

### Bước 3: Kiểm tra trạng thái

Gõ `/gffailsafe status` để xem:
- Plugin có nhận diện được Geyser/Floodgate không
- Cấu hình hiện tại

---

## 📝 Tóm tắt tính năng

| Tính năng | Mô tả |
|-----------|-------|
| 🛡️ **Bảo vệ tài khoản** | Ngăn Bedrock chiếm tài khoản Java khi Floodgate lỗi |
| 🔒 **Chống giả mạo** | Ngăn Java giả làm Bedrock bằng prefix giả |
| 🔄 **Tự động hoàn toàn** | Chạy ngầm, không cần can thiệp |
| 📋 **Ghi log đầy đủ** | Lưu lại mọi lần chặn để kiểm tra |
| ⚡ **Hiệu suất cao** | Không gây lag server |
| 🎛️ **Cấu hình linh hoạt** | Nhiều tùy chọn điều chỉnh |

---

## 📌 Checklist sau khi cài đặt

- [ ] Đã copy file `.jar` vào thư mục `plugins`
- [ ] Đã restart server để tạo file `config.yml`
- [ ] Đã kiểm tra prefix trong `floodgate/config.yml`
- [ ] Đã chỉnh `floodgate-prefix` cho đúng
- [ ] Đã test thử với 1 tài khoản Bedrock

---

## 📞 Cần hỗ trợ?

1. Bật `debug: true` và xem log
2. Kiểm tra `/gffailsafe status`
3. Xem file `blocked.log` để biết ai bị chặn và lý do

---

**Được tạo để bảo vệ server Minecraft của bạn! 🎮🛡️**

