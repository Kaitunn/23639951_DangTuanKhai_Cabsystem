# Test Case Suite - CAB System API

Bộ test case được xây dựng dựa trên API specification (OpenAPI) và các yêu cầu FR (Functional Requirement) / AC (Acceptance Criteria) tương ứng của từng domain.

## Quy ước

- **Ngữ cảnh (Context)**: một quy trình nghiệp vụ cụ thể (business process), là gốc để suy ra các test case.
- **Test case**: một tình huống kiểm thử cụ thể thuộc một Ngữ cảnh, gồm:
  - `ID`: mã test case, format `TC-<DOMAIN>-<STT>`
  - `Loại`: Positive (đúng quy trình) hoặc Negative (sai quy trình / dữ liệu lỗi / trường hợp biên)
  - `Input`: tham số/dữ liệu đầu vào
  - `Precondition`: điều kiện tiên quyết (trạng thái hệ thống trước khi test)
  - `Expected Output`: kết quả kỳ vọng (HTTP status + nội dung phản hồi)
  - `FR/AC liên quan`: yêu cầu nghiệp vụ tương ứng
- Mỗi Ngữ cảnh có **cả test case Positive và Negative** — không chỉ test happy path.
- Các test case đánh dấu **(*)** là trường hợp nghiệp vụ hợp lý nhưng **API specification hiện tại chưa định nghĩa response tương ứng** — cần bổ sung vào spec, tạm ghi nhận response đề xuất.

## Danh sách file

| File | Domain | FR liên quan |
|---|---|---|
| `01-auth.md` | Đăng ký / Đăng nhập | FR01, FR02, FR16, FR33 |
| `02-customer.md` | Hồ sơ khách hàng | FR03 |
| `03-driver.md` | Hồ sơ, trạng thái, vị trí tài xế | FR16-FR19 |
| `04-trip.md` | Đặt xe, phân công tài xế, theo dõi chuyến | FR04-FR15, FR20, FR35, FR36 |
| `05-payment.md` | Thanh toán | FR21-FR24 |
| `06-rating.md` | Đánh giá tài xế | FR37 |
| `07-admin.md` | Vận hành / Giám sát | FR27-FR32 |

## Thống kê

| Domain | Số Ngữ cảnh | Số test case |
|---|---|---|
| Auth | 2 | 11 |
| Customer | 1 | 7 |
| Driver | 4 | 18 |
| Trip | 5 | 36 |
| Payment | 1 | 8 |
| Rating | 1 | 8 |
| Admin | 3 | 9 |
| **Tổng** | **17** | **97** |
