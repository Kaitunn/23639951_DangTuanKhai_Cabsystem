# Test Case - Auth

## Ngữ cảnh 1: Đăng ký tài khoản khách hàng
**Nghiệp vụ**: Người dùng mới cung cấp họ tên, số điện thoại, mật khẩu để tạo tài khoản khách hàng.
**Endpoint**: `POST /auth/register`
**FR/AC**: FR01, AC01

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-AUTH-01 | Positive | fullName="Nguyen Van A", phone="0901234567", password="matkhau123" | Số điện thoại chưa tồn tại | 201, body chứa customerId, fullName, phone |
| TC-AUTH-02 | Negative | Thiếu field `password` | - | 400, message báo thiếu field bắt buộc |
| TC-AUTH-03 | Negative | phone="abc123" (sai định dạng số điện thoại VN) | - | 400, message báo sai định dạng phone |
| TC-AUTH-04 | Negative | phone="0901234567" (đã tồn tại trong hệ thống) | Tài khoản với phone này đã đăng ký trước | 409, message báo số điện thoại đã được sử dụng |
| TC-AUTH-05 | Negative | password="123" (dưới minLength=8) | - | 400, message báo mật khẩu quá ngắn |
| TC-AUTH-06 | Negative | fullName rỗng ("") | - | 400 (*) — spec hiện chưa validate minLength cho fullName |

## Ngữ cảnh 2: Đăng nhập
**Nghiệp vụ**: Người dùng (customer/driver/operator) đăng nhập bằng phone + password để lấy access token.
**Endpoint**: `POST /auth/login`
**FR/AC**: FR02, AC02, AC29

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-AUTH-07 | Positive | phone="0901234567", password="matkhau123" | Tài khoản tồn tại, đúng mật khẩu | 200, body chứa accessToken, role đúng với loại tài khoản |
| TC-AUTH-08 | Negative | phone="0901234567", password="saimatkhau" | Tài khoản tồn tại | 401, message báo sai thông tin đăng nhập |
| TC-AUTH-09 | Negative | phone="0909999999" (không tồn tại), password bất kỳ | Không có tài khoản với phone này | 401, message báo sai thông tin đăng nhập (không tiết lộ phone không tồn tại) |
| TC-AUTH-10 | Negative | Thiếu field `phone` | - | 400 (*) — spec hiện chưa định nghĩa 400 cho login, cần bổ sung |
| TC-AUTH-11 | Negative | Đăng nhập nhiều lần sai liên tiếp (brute-force) | Tài khoản tồn tại | 429 Too Many Requests hoặc khóa tạm thời (*) — nghiệp vụ bảo mật khuyến nghị bổ sung vào spec |
