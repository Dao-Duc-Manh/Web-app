# Hệ Thống Tin Nhắn Mã Hóa

## 📋 Mô tả

Hệ thống tin nhắn mã hóa sử dụng Flask và WebSocket, cho phép người dùng gửi và nhận tin nhắn được mã hóa end-to-end. Hệ thống sử dụng mã hóa RSA cho việc trao đổi khóa và bảo mật thông tin.

## 🚀 Tính năng chính

- ✅ Đăng ký người dùng với public key
- ✅ Gửi tin nhắn mã hóa
- ✅ Nhận tin nhắn real-time qua WebSocket
- ✅ Quản lý danh sách người dùng
- ✅ API RESTful đầy đủ
- ✅ Hỗ trợ CORS cho frontend

## 🛠️ Cài đặt và Chạy

### Yêu cầu hệ thống

- Python 3.7+
- pip (Python package manager)

### Bước 1: Cài đặt dependencies

```bash
pip install flask flask-socketio
```

### Bước 2: Chạy ứng dụng

```bash
cd web_app
python app.py
```

Ứng dụng sẽ chạy tại: `http://localhost:5001`

## 📡 API Endpoints

### 1. Đăng ký người dùng

**POST** `/register`

```json
{
  "username": "alice",
  "rsaPublicKey": "base64_encoded_rsa_public_key",
  "signPublicKey": "base64_encoded_signature_public_key"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "User alice đã được đăng ký/ghi đè."
}
```

### 2. Gửi tin nhắn

**POST** `/send`

```json
{
  "recipient": "bob",
  "sender": "alice",
  "packet": "encrypted_message_packet"
}
```

**Response:**

```json
{
  "status": "success",
  "message": "Đã gửi tin nhắn."
}
```

### 3. Nhận tin nhắn

**GET** `/receive/<username>`

**Response:**

```json
{
  "status": "success",
  "messages": [
    {
      "sender_username": "alice",
      "packet": "encrypted_message_packet"
    }
  ]
}
```

### 4. Lấy danh sách người dùng

**GET** `/get_users`

**Response:**

```json
{
  "status": "success",
  "users": {
    "alice": {
      "rsaPublicKey": "base64_encoded_rsa_public_key",
      "signPublicKey": "base64_encoded_signature_public_key"
    },
    "bob": {
      "rsaPublicKey": "base64_encoded_rsa_public_key",
      "signPublicKey": "base64_encoded_signature_public_key"
    }
  }
}
```

## 🔌 WebSocket Events

### Sự kiện nhận tin nhắn mới

Khi có tin nhắn mới, server sẽ emit event `new_message`:

```javascript
socket.on("new_message", function (data) {
  console.log("Tin nhắn mới từ:", data.message.sender_username);
  console.log("Gói tin:", data.message.packet);
});
```

## 📁 Cấu trúc dự án

```
web_app/
├── app.py              # Server chính
├── templates/          # HTML templates (tự tạo)
├── static/            # CSS, JS files (tự tạo)
└── README.md          # File này
```

## 🔐 Bảo mật

### Mã hóa

- Sử dụng mã hóa RSA cho trao đổi khóa
- Public key được lưu trữ trên server
- Private key được giữ bí mật bởi client
- Tin nhắn được mã hóa end-to-end

### Lưu ý bảo mật

- Server chỉ lưu trữ public key, không có private key
- Tin nhắn được mã hóa trước khi gửi đến server
- Server không thể đọc nội dung tin nhắn

## 🧪 Test API

### Sử dụng curl

```bash
# Đăng ký user
curl -X POST http://localhost:5001/register \
  -H "Content-Type: application/json" \
  -d '{"username":"alice","rsaPublicKey":"test_key","signPublicKey":"test_sign"}'

# Lấy danh sách users
curl http://localhost:5001/get_users

# Gửi tin nhắn
curl -X POST http://localhost:5001/send \
  -H "Content-Type: application/json" \
  -d '{"recipient":"bob","sender":"alice","packet":"encrypted_data"}'
```

### Sử dụng Postman

1. Import các request vào Postman
2. Set base URL: `http://localhost:5001`
3. Test từng endpoint

## 🐛 Troubleshooting

### Lỗi thường gặp

1. **Port 5001 đã được sử dụng**
   - Thay đổi port trong `app.py` dòng cuối
2. **Module not found**

   - Chạy: `pip install flask flask-socketio`

3. **CORS errors**
   - Server đã được cấu hình CORS cho tất cả origins

## 📝 Log

Server sẽ in log ra console:

- Khi user đăng ký: `Registered (overwritten): username`
- Khi nhận tin nhắn: `Received message from sender for recipient`

## 🤝 Đóng góp

Đây là bài tập lớn môn An toàn và Bảo mật thông tin.

## 📄 License

Dự án này được tạo cho mục đích học tập.
