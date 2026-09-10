# Test Case - Rating

## Ngữ cảnh 1: Khách hàng đánh giá tài xế sau chuyến đi
**Nghiệp vụ**: Sau khi chuyến hoàn tất, khách hàng chấm điểm (1-5 sao) và có thể để lại nhận xét cho tài xế.
**Endpoint**: `POST /trips/{tripId}/rating`
**FR/AC**: FR37, AC33

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-RATE-01 | Positive | score=5, comment="Tài xế thân thiện" | Trip status="completed", chưa được đánh giá | 200, trả về driverAverageRating được cập nhật |
| TC-RATE-02 | Positive | score=3 (không kèm comment) | Trip status="completed" | 200, comment có thể để trống |
| TC-RATE-03 | Negative | score=0 (dưới minimum=1) | Trip status="completed" | 400, báo score ngoài khoảng cho phép |
| TC-RATE-04 | Negative | score=6 (trên maximum=5) | Trip status="completed" | 400, báo score ngoài khoảng cho phép |
| TC-RATE-05 | Negative | score=4 | Trip đang ở status="in_progress" (chưa hoàn tất) | 400, message "Chuyến đi chưa hoàn tất, không thể đánh giá" |
| TC-RATE-06 | Negative | score=4 | Trip đã được đánh giá trước đó (đánh giá lần 2) | 409 (*) — tránh đánh giá trùng, cần bổ sung vào spec |
| TC-RATE-07 | Negative | tripId không tồn tại | - | 404, báo không tìm thấy chuyến đi |
| TC-RATE-08 | Negative | Thiếu field `score` (bắt buộc) | Trip status="completed" | 400, báo thiếu field bắt buộc |
