# SixExpress

Hệ thống giao hàng trực tuyến, hỗ trợ quản lý đơn hàng, giao nhận và điều hành vận hành.

## 1. Cấu trúc dự án

- `web/`: Giao diện Web sử dụng React.
- `backend/`: Backend sử dụng ASP.NET Core.
- `mobile/`: Ứng dụng di động sử dụng Flutter.
- `docs/`: Tài liệu hướng dẫn và tích hợp hệ thống.
- `DeliverySystem_Document/`: Tài liệu dự án.
- `.github/`: Cấu hình và tài liệu GitHub.

## 2. Chạy Web

Mở Terminal tại thư mục `web/`:

```bash
npm ci
```

Sao chép `.env.example` thành `.env`, sau đó cấu hình:

```dotenv
VITE_USE_MOCK=true
VITE_API_BASE_URL=https://localhost:7001
```

Khởi động Web:

```bash
npm run dev
```

## 3. Chế độ Mock và API thật

### Chế độ Mock

Khi `VITE_USE_MOCK=true`, hệ thống sử dụng dữ liệu thử nghiệm lưu trong localStorage và không cần Backend.

Tài khoản mẫu:

| Email | Mật khẩu | Vai trò |
|---|---|---|
| shop@example.com | Express123! | Shop |
| staff@express.com | Staff01@123 | Giao nhận |
| operations@example.com | Operations@123 | Điều hành |

Đây là tài khoản thử nghiệm, không sử dụng mật khẩu thật.

Các shop Mock dùng chung bộ đơn hàng mẫu, chưa cách ly dữ liệu theo tenant. Không sử dụng chế độ này như hệ thống bảo mật thực tế.

### Chế độ API thật

Khi Backend đã sẵn sàng:

```dotenv
VITE_USE_MOCK=false
VITE_API_BASE_URL=https://localhost:7001
```

Điều chỉnh URL theo địa chỉ Backend thực tế, sau đó khởi động lại Vite.

Lưu ý:
- API thật không tự động chuyển sang Mock khi xảy ra lỗi.
- Dữ liệu localStorage không tự động chuyển vào Database.

Các file API chính:

- `src/api.ts`: Chọn adapter API.
- `src/api.real.ts`: Gọi API HTTP thật.
- `src/api.mock.ts`: Xử lý dữ liệu mô phỏng.

## 4. JWT signing key (Backend)

Repository không chứa JWT signing key có thể sử dụng. Cần tạo secret riêng trên máy cá nhân.

Chạy PowerShell tại thư mục gốc dự án:

```powershell
$bytes = New-Object byte[] 48
$rng = [System.Security.Cryptography.RandomNumberGenerator]::Create()
$rng.GetBytes($bytes)
$rng.Dispose()
$jwt = [Convert]::ToBase64String($bytes)
dotnet user-secrets set "Jwt:Key" $jwt --project "backend/Back_DeliverySystem/Back_DeliverySystem/Back_DeliverySystem.csproj"
Remove-Variable jwt
```

Chạy Backend ở môi trường `Development` để sử dụng User Secrets.

Không đưa JWT key vào GitHub, Issues, Pull Requests hoặc tin nhắn công khai.

Nếu triển khai thực tế, hãy cấu hình `Jwt__Key` thông qua hệ thống quản lý secret của máy chủ.

## 5. Tài liệu tham khảo

- `docs/MOCK_MODE.md`
- `docs/AUTH_VA_TICH_HOP.md`

Tham khảo các tài liệu trên để tìm hiểu chế độ Mock, HTTP API và xác thực.

## 6. Quy trình GitHub

- `main`: Nhánh chính của repository SixExpress.
- Tạo branch riêng cho từng chức năng.
- Commit các thay đổi theo từng phần công việc.
- Tạo Pull Request để kiểm tra trước khi hợp nhất vào `main`.

