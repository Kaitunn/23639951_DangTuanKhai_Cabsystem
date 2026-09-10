# Test Case - Customer

## Ngữ cảnh 1: Xem và cập nhật hồ sơ khách hàng
**Nghiệp vụ**: Khách hàng đã đăng nhập xem thông tin cá nhân và chỉnh sửa fullName/email.
**Endpoint**: `GET/PUT /customers/{customerId}`
**FR/AC**: FR03, AC03

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-CUST-01 | Positive | GET, customerId hợp lệ, có token | Token hợp lệ của chính customerId đó | 200, trả về đầy đủ customerId, fullName, phone, email |
| TC-CUST-02 | Negative | GET, customerId không tồn tại | - | 404, message báo không tìm thấy khách hàng |
| TC-CUST-03 | Negative | GET, không kèm token | - | 401, message báo chưa xác thực |
| TC-CUST-04 | Positive | PUT, fullName="Nguyen Van B", email="b@mail.com" | customerId tồn tại, có token hợp lệ | 200, trả về hồ sơ đã cập nhật |
| TC-CUST-05 | Negative | PUT, customerId không tồn tại | - | 404, message báo không tìm thấy khách hàng |
| TC-CUST-06 | Negative | PUT, email="khong-hop-le" (sai định dạng) | customerId tồn tại | 400 (*) — spec hiện chưa validate format email, cần bổ sung |
| TC-CUST-07 | Negative | PUT bằng token của customer khác (A sửa hồ sơ của B) | customerId B tồn tại, token của A | 403 (*) — spec hiện chưa kiểm tra quyền sở hữu, cần bổ sung |
