# Test Case - Payment

## Ngữ cảnh 1: Ghi nhận thanh toán sau khi hoàn tất chuyến đi
**Nghiệp vụ**: Khi chuyến đi kết thúc, hệ thống ghi nhận thanh toán bằng tiền mặt hoặc thanh toán điện tử.
**Endpoint**: `POST /trips/{tripId}/payment`
**FR/AC**: FR21-FR24, AC17-AC20

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-PAY-01 | Positive | method="cash", amount=85000 | Trip status="completed", chưa thanh toán | 200, status="paid", trả về transactionId |
| TC-PAY-02 | Positive | method="e-payment", amount=120000 | Trip status="completed", cổng thanh toán điện tử hoạt động bình thường | 200, status="paid", trả về transactionId |
| TC-PAY-03 | Negative | method="e-payment", amount=120000 | Cổng thanh toán điện tử trả về lỗi (giả lập timeout/từ chối giao dịch) | 402, message báo thanh toán điện tử thất bại (AC20) |
| TC-PAY-04 | Negative | amount=-50000 (giá trị âm) | Trip status="completed" | 400, báo số tiền không hợp lệ |
| TC-PAY-05 | Negative | method="qr_code" (ngoài enum cash/e-payment) | Trip status="completed" | 400, báo phương thức thanh toán không hợp lệ |
| TC-PAY-06 | Negative | method="cash", amount=85000 | Trip đang ở status="in_progress" (chưa hoàn tất) | 400 (*) — không cho thanh toán khi chuyến chưa completed, cần bổ sung vào spec |
| TC-PAY-07 | Negative | tripId không tồn tại | - | 404, báo không tìm thấy chuyến đi |
| TC-PAY-08 | Negative | method="cash", amount=85000 | Trip đã được thanh toán trước đó (status="paid") | 409 (*) — tránh ghi nhận thanh toán trùng lặp, cần bổ sung vào spec |
