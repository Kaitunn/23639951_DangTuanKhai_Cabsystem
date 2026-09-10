# Test Case - Driver

## Ngữ cảnh 1: Đăng ký tài khoản tài xế
**Nghiệp vụ**: Tài xế mới đăng ký thông tin cá nhân + phương tiện.
**Endpoint**: `POST /drivers/register`
**FR/AC**: FR16, AC12

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-DRV-01 | Positive | fullName, phone, vehiclePlate="51F-12345", vehicleType="car4" | phone và vehiclePlate chưa tồn tại | 201, trả về driverId + thông tin đã đăng ký |
| TC-DRV-02 | Negative | Thiếu field `vehiclePlate` | - | 400, báo thiếu field bắt buộc |
| TC-DRV-03 | Negative | phone đã tồn tại (trùng tài xế khác) | Tài xế với phone này đã đăng ký | 409, báo số điện thoại đã tồn tại |
| TC-DRV-04 | Negative | vehiclePlate đã tồn tại (trùng biển số xe) | Tài xế khác đã đăng ký biển số này | 409, báo biển số đã được sử dụng |
| TC-DRV-05 | Negative | vehicleType="xe_may" (ngoài enum bike/car4/car7) | - | 400 (*) — spec hiện dùng `type: string` tự do, cần enum hóa |

## Ngữ cảnh 2: Cập nhật hồ sơ và phương tiện tài xế
**Endpoint**: `PUT /drivers/{driverId}`
**FR/AC**: FR17, AC13

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-DRV-06 | Positive | fullName mới, vehiclePlate mới | driverId tồn tại, có token | 200, trả về hồ sơ đã cập nhật |
| TC-DRV-07 | Negative | driverId không tồn tại | - | 404, báo không tìm thấy tài xế |
| TC-DRV-08 | Negative | Không kèm token | driverId tồn tại | 401, báo chưa xác thực |
| TC-DRV-09 | Negative | vehiclePlate trùng với biển số của tài xế khác | driverId tồn tại | 409 (*) — spec chưa định nghĩa, cần bổ sung |

## Ngữ cảnh 3: Bật/tắt trạng thái sẵn sàng nhận chuyến
**Nghiệp vụ**: Tài xế chuyển online (available=true) để bắt đầu nhận cuốc, hoặc offline (available=false) khi nghỉ.
**Endpoint**: `PATCH /drivers/{driverId}/availability`
**FR/AC**: FR18, AC14

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-DRV-10 | Positive | available=true | driverId tồn tại, đang offline | 200, available=true |
| TC-DRV-11 | Positive | available=false | driverId tồn tại, đang online, không có chuyến đang chạy | 200, available=false |
| TC-DRV-12 | Negative | available=false | driverId đang trong 1 chuyến `in_progress` | 409 (*) — không cho tắt trạng thái khi đang chở khách, cần bổ sung vào spec |
| TC-DRV-13 | Negative | Thiếu field `available` | driverId tồn tại | 400, báo thiếu field bắt buộc |
| TC-DRV-14 | Negative | driverId không tồn tại | - | 404, báo không tìm thấy tài xế |

## Ngữ cảnh 4: Cập nhật vị trí tài xế theo chu kỳ
**Endpoint**: `PATCH /drivers/{driverId}/location`
**FR/AC**: FR19, AC16

| ID | Loại | Input | Precondition | Expected Output |
|---|---|---|---|---|
| TC-DRV-15 | Positive | lat=10.7769, lng=106.7009 | driverId tồn tại | 200, trả về lat/lng/updatedAt vừa ghi nhận |
| TC-DRV-16 | Negative | lat=200 (ngoài khoảng -90..90) | driverId tồn tại | 400 (*) — spec chưa validate range, cần bổ sung `minimum/maximum` |
| TC-DRV-17 | Negative | Thiếu field `lng` | driverId tồn tại | 400, báo thiếu field bắt buộc |
| TC-DRV-18 | Negative | driverId không tồn tại | - | 404, báo không tìm thấy tài xế |
