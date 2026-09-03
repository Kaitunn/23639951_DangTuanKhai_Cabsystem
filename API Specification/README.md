# CAB System – API Design (chia file theo domain)

## Cấu trúc thư mục

```
openapi/
├── openapi.yaml              # File gốc, chỉ gom các file khác lại bằng $ref
├── components/
│   └── schemas.yaml          # Schema dùng chung (Trip, Driver, Customer)
└── paths/
    ├── auth.yaml              # POST /auth/register, /auth/login (FR01, FR02, FR16, FR33)
    ├── customer.yaml          # GET/PUT /customers/{id} (FR03)
    ├── trip.yaml              # POST/GET /trips, assign-driver, driver-response (FR04-FR15, FR20, FR35, FR36)
    ├── driver.yaml            # /drivers/... (FR16-FR19)
    ├── payment.yaml           # POST /trips/{id}/payment (FR21-FR24)
    ├── rating.yaml            # POST /trips/{id}/rating (FR37)
    └── admin.yaml             # /admin/... (FR27-FR32)
```

## Vì sao chia file riêng?

- Mỗi thành viên trong nhóm có thể chỉnh sửa 1 file API (VD: bạn A phụ trách `trip.yaml`, bạn B phụ trách `payment.yaml`) mà không đụng code của nhau khi làm việc trên Git.
- Dễ review Pull Request theo từng module.
- File `openapi.yaml` gốc chỉ đóng vai trò "mục lục", không chứa logic chi tiết.

## Cách xem trên Swagger Editor (Docker)

Swagger Editor chạy trong Docker (`docker run -d -p 8080:8080 swaggerapi/swagger-editor`)
chỉ đọc được **1 file duy nhất** dán trực tiếp vào — không tự đọc `$ref` tới file trên máy.

Vì vậy trước khi xem/preview, cần **bundle** (gộp) các file lại thành 1 file duy nhất bằng lệnh:

```bash
npx @redocly/cli bundle openapi/openapi.yaml -o bundled.yaml
```

Sau đó copy nội dung `bundled.yaml` dán vào Swagger Editor tại `http://localhost:8080`.

(File `bundled.yaml` này chính là bản đầy đủ giống file `openapi.yaml` gộp mà mình gửi bạn ở câu trả lời trước — chỉ khác là giờ source code được quản lý tách file trên GitHub.)

## Đẩy lên GitHub

```bash
git add openapi/
git commit -m "Split API design into separate files per domain (Auth, Trip, Driver, Payment, Rating, Admin)"
git push
```
