# Test Case - Admin

## Ngữ cảnh 1: Operator theo dõi danh sách chuyến đang diễn ra
**Endpoint**: `GET /admin/trips`
**FR/AC**: FR27, AC24

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-ADM-01 | Positive | Không filter | Có role operator, có >=1 chuyến đang diễn ra | 200, trả về items + total |
| TC-ADM-02 | Positive | status="in_progress" | Có chuyến ở nhiều trạng thái khác nhau | 200, chỉ trả về các chuyến đúng status filter |
| TC-ADM-03 | Negative | Gọi bởi role customer/driver (không phải operator) | Token hợp lệ nhưng sai role | 403, báo không đủ quyền |
| TC-ADM-04 | Negative | Không kèm token | - | 401, báo chưa xác thực |

## Ngữ cảnh 2: Operator xử lý chuyến bị lỗi
**Nghiệp vụ**: Khi chuyến gặp sự cố (vd tài xế mất kết nối, không cập nhật trạng thái), operator can thiệp thủ công để đóng/xử lý chuyến.
**Endpoint**: `POST /admin/trips/{tripId}/resolve`
**FR/AC**: FR29, AC25, AC26

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-ADM-05 | Positive | tripId hợp lệ | Trip đang gặp sự cố, operator có đủ quyền | 200, ghi log thao tác, trả về resolvedBy + resolvedAt |
| TC-ADM-06 | Negative | tripId không tồn tại | - | 404, báo không tìm thấy chuyến đi |
| TC-ADM-07 | Negative | tripId hợp lệ | Gọi bởi operator không đủ quyền xử lý sự cố | 403, báo "Operator không đủ quyền" (AC26) |

## Ngữ cảnh 3: Operator xem báo cáo vận hành
**Endpoint**: `GET /admin/reports`
**FR/AC**: FR32, AC28

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-ADM-08 | Positive | from="2026-08-01", to="2026-08-31" | Có dữ liệu chuyến trong khoảng thời gian | 200, trả về totalTrips, totalRevenue, completionRate, cancellationRate |
| TC-ADM-09 | Negative | from="2026-09-01", to="2026-08-01" (from > to) | - | 400 (*) — spec chưa validate khoảng ngày hợp lệ, cần bổ sung |
