# Test Case - Trip (đặt xe, phân công tài xế, theo dõi chuyến)

## Ngữ cảnh 1: Khách hàng gửi yêu cầu đặt xe
**Nghiệp vụ**: Khách hàng nhập điểm đón, điểm đến, loại xe để tạo yêu cầu đặt xe.
**Endpoint**: `POST /trips`
**FR/AC**: FR04-FR07, AC04, AC05

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-TRIP-01 | Positive | pickupLocation={10.77,106.70}, dropoffLocation={10.80,106.75}, vehicleType="car4" | Khách hàng đã đăng nhập | 201, trả về tripId, status="searching" |
| TC-TRIP-02 | Positive | Có thêm preferHighRatingDriver=true | Khách hàng đã đăng nhập | 201, status="searching", cờ ưu tiên được ghi nhận |
| TC-TRIP-03 | Negative | Thiếu pickupLocation | Đã đăng nhập | 400, báo thiếu điểm đón |
| TC-TRIP-04 | Negative | Thiếu dropoffLocation | Đã đăng nhập | 400, báo thiếu điểm đến |
| TC-TRIP-05 | Negative | vehicleType="taxi7cho" (ngoài enum bike/car4/car7) | Đã đăng nhập | 400, báo loại xe không hợp lệ |
| TC-TRIP-06 | Negative | pickupLocation trùng hoàn toàn dropoffLocation | Đã đăng nhập | 400 (*) — nghiệp vụ nên chặn đặt xe khi điểm đón = điểm đến, cần bổ sung vào spec |
| TC-TRIP-07 | Negative | Không kèm token | - | 401, báo chưa xác thực |
| TC-TRIP-08 | Negative | lat/lng ngoài khoảng hợp lệ (vd lat=999) | Đã đăng nhập | 400 (*) — spec chưa validate range tọa độ |

## Ngữ cảnh 2: Hệ thống tìm và phân công tài xế tự động
**Nghiệp vụ**: Sau khi trip ở trạng thái "searching", hệ thống tìm tài xế phù hợp (cùng vehicleType, đang available, gần điểm đón nhất) để đề xuất.
**Endpoint**: `POST /trips/{tripId}/assign-driver`
**FR/AC**: FR08-FR15, AC06-AC11

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-TRIP-09 | Positive | tripId hợp lệ | Có ít nhất 1 tài xế cùng vehicleType, đang available, trong bán kính tìm kiếm | 200, trả về driverId được đề xuất, trip chuyển status="assigned" (chờ tài xế phản hồi) |
| TC-TRIP-10 | Negative | tripId hợp lệ | **Không có tài xế nào available** cùng loại xe trong khu vực | 404, message "Không tìm được tài xế phù hợp" (AC08) |
| TC-TRIP-11 | Negative | tripId hợp lệ | Có tài xế available nhưng khác vehicleType (chỉ có bike, khách đặt car4) | 404, message "Không tìm được tài xế phù hợp" |
| TC-TRIP-12 | Negative | tripId hợp lệ | Tất cả tài xế phù hợp trong khu vực đã từ chối (decline) chuyến này trước đó | 404, message "Không tìm được tài xế phù hợp" |
| TC-TRIP-13 | Negative | tripId không tồn tại | - | 404, báo không tìm thấy chuyến đi |
| TC-TRIP-14 | Negative | Gọi lại assign-driver khi trip đã có tài xế (status="assigned" hoặc sau) | Trip đã được gán tài xế trước đó | 409 (*) — tránh gán trùng 2 tài xế cho 1 chuyến, cần bổ sung vào spec |
| TC-TRIP-15 | Positive (edge) | preferHighRatingDriver=true | Có tài xế rating cao và tài xế rating thấp cùng available | 200, driverId trả về ưu tiên tài xế có rating cao hơn |
| TC-TRIP-16 | Negative (edge) | preferHighRatingDriver=true | Không có tài xế rating cao, chỉ có tài xế thường | 200, hệ thống fallback về tài xế thường gần nhất (*) — hành vi fallback cần làm rõ và bổ sung mô tả vào spec, hiện không quy định |

## Ngữ cảnh 3: Tài xế phản hồi chấp nhận / từ chối chuyến
**Endpoint**: `POST /trips/{tripId}/driver-response`
**FR/AC**: FR13, AC09, AC10

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-TRIP-17 | Positive | driverId đúng tài xế được đề xuất, response="accept" | Trip đang chờ phản hồi từ driverId này | 200, trip chuyển status="assigned" xác nhận, driverId gắn cố định vào trip |
| TC-TRIP-18 | Positive | driverId đúng, response="decline" | Trip đang chờ phản hồi | 200, ghi nhận từ chối; hệ thống có thể tự động gọi lại assign-driver để tìm tài xế khác |
| TC-TRIP-19 | Negative | driverId khác với tài xế được đề xuất, response="accept" | Trip đang chờ phản hồi từ tài xế khác | 409 (*) — tài xế không được đề xuất cho chuyến này, cần bổ sung vào spec |
| TC-TRIP-20 | Negative | driverId đúng, response="accept" nhưng đã quá thời gian chờ phản hồi (timeout) | Trip đã tự động chuyển sang tìm tài xế khác do timeout | 409, message "Chuyến đi đã được gán cho tài xế khác hoặc đã hết hạn phản hồi" |
| TC-TRIP-21 | Negative | tripId không tồn tại | - | 404, báo không tìm thấy chuyến đi |
| TC-TRIP-22 | Negative | response="maybe" (ngoài enum accept/decline) | Trip đang chờ phản hồi | 400, báo giá trị response không hợp lệ |

## Ngữ cảnh 4: Cập nhật trạng thái chuyến đi theo hành trình
**Nghiệp vụ**: Tài xế cập nhật các mốc: đã đến điểm đón, đã đón khách, đang di chuyển, hoàn tất, hoặc hủy.
**Endpoint**: `PATCH /trips/{tripId}/status`
**FR/AC**: FR20, AC15

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-TRIP-23 | Positive | status="arrived_pickup" | Trip đang ở status="assigned" | 200, trip status="arrived_pickup" |
| TC-TRIP-24 | Positive | status="picked_up" | Trip đang ở status="arrived_pickup" | 200, trip status="picked_up" |
| TC-TRIP-25 | Positive | status="completed" | Trip đang ở status="in_progress" | 200, trip status="completed" |
| TC-TRIP-26 | Positive | status="cancelled" | Trip ở status="searching" hoặc "assigned" | 200, trip status="cancelled" |
| TC-TRIP-27 | Negative | status="completed" | Trip đang ở status="assigned" (bỏ qua các bước trung gian) | 400 (*) — nhảy trạng thái không hợp lệ, cần bổ sung state-machine validation vào spec |
| TC-TRIP-28 | Negative | status bất kỳ | Trip đã ở status="completed" hoặc "cancelled" (đã kết thúc) | 400 (*) — không cho cập nhật trip đã kết thúc |
| TC-TRIP-29 | Negative | status="flying" (ngoài enum) | Trip tồn tại | 400, báo giá trị trạng thái không hợp lệ |
| TC-TRIP-30 | Negative | tripId không tồn tại | - | 404, báo không tìm thấy chuyến đi |

## Ngữ cảnh 5: Xem chi tiết và lịch sử chuyến đi
**Endpoint**: `GET /trips/{tripId}`, `GET /trips`
**FR/AC**: FR35, FR36, AC31, AC32

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-TRIP-31 | Positive | GET /trips/{tripId} hợp lệ | Trip tồn tại | 200, trả về đầy đủ thông tin chuyến đi |
| TC-TRIP-32 | Negative | GET /trips/{tripId} không tồn tại | - | 404, báo không tìm thấy chuyến đi |
| TC-TRIP-33 | Positive | GET /trips?customerId=xxx | customerId có ít nhất 1 chuyến | 200, trả về danh sách chuyến, total > 0 |
| TC-TRIP-34 | Positive (edge) | GET /trips?customerId=xxx | customerId chưa từng đặt chuyến nào | 200, trả về `items: []`, `total: 0` (không phải lỗi) |
| TC-TRIP-35 | Positive | GET /trips?customerId=xxx&status=completed | customerId có cả chuyến completed và cancelled | 200, chỉ trả về các chuyến status=completed |
| TC-TRIP-36 | Negative | GET /trips thiếu query `customerId` (bắt buộc) | - | 400 (*) — spec khai báo required nhưng chưa định nghĩa response 400 tương ứng |
