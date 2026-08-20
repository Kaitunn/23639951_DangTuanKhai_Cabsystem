### Xây dựng hệ thống cơ bản
**Đóng vai trò:** BA (Business Analyst)

#### Bước 1: Phân tích yêu cầu của khách hàng
Ở giai đoạn sơ khởi (giai đoạn 1), BA cần tập trung vào việc **phân tích và tìm hiểu yêu cầu của khách hàng**.
Mục tiêu chính là hiểu được **Business Context** - tức là **ngữ cảnh của nghiệp vụ**, bao gồm:
- **Khách hàng là ai?**
- **Doanh nghiệp đang giải quyết vấn đề gì?**
- **Mục tiêu kinh doanh (Business Goal) là gì?**
- **Quy trình nghiệp vụ hiện tại (As-Is) đang diễn ra như thế nào?**
- **Những vấn đề hoặc khó khăn đang tồn tại là gì?**
- **Hệ thống cần giải quyết vấn đề nào?**
- **Kết quả mong muốn (To-Be) của khách hàng là gì?**

**Trả lời cho dự án CAB System:**
- **Khách hàng là ai?**
  Công ty ABC – doanh nghiệp cung cấp dịch vụ đặt xe trực tuyến.
- **Doanh nghiệp đang giải quyết vấn đề gì?**
  Hệ thống hiện tại (tổng đài + app đơn giản) còn nhiều hạn chế: phân công tài xế thủ công, khách hàng khó theo dõi trạng thái chuyến đi, thông tin thanh toán chưa quản lý tập trung, khó mở rộng hệ thống khi quy mô tăng.
- **Mục tiêu kinh doanh (Business Goal) là gì?**
  Xây dựng một nền tảng CAB mới có khả năng phục vụ số lượng lớn khách hàng và tài xế, đồng thời có thể phát triển thêm tính năng trong tương lai mà không cần xây lại toàn bộ hệ thống.
- **Quy trình nghiệp vụ hiện tại (As-Is) đang diễn ra như thế nào?**
  Khách hàng liên hệ tổng đài hoặc dùng app đơn giản để yêu cầu xe → nhân viên phân công tài xế thủ công → khách hàng khó nắm được trạng thái chuyến → thanh toán và dữ liệu chưa tập trung, khó theo dõi.
- **Những vấn đề hoặc khó khăn đang tồn tại là gì?**
  - Phân công tài xế thủ công, chậm, không tối ưu.
  - Thiếu minh bạch về trạng thái chuyến đi cho khách hàng.
  - Thanh toán phân mảnh, chưa tích hợp cổng thanh toán bên ngoài an toàn.
  - Khó mở rộng hạ tầng khi tải tăng cao (giờ cao điểm).
  - Thiếu công cụ quản trị & báo cáo cho vận hành.
  - Chưa có kiểm soát bảo mật, phân quyền và audit trail rõ ràng.
- **Hệ thống cần giải quyết vấn đề nào?**
  Tự động hóa việc tìm và phân công tài xế, tập trung hóa quản lý thanh toán, cung cấp khả năng theo dõi chuyến đi theo thời gian thực, đảm bảo hệ thống có thể mở rộng độc lập theo module (kiến trúc modular/microservices) và không bị sập toàn bộ khi một thành phần (thanh toán, thông báo) gặp lỗi.
- **Kết quả mong muốn (To-Be) của khách hàng là gì?**
  Một nền tảng CAB hoàn chỉnh, đáp ứng toàn bộ quy trình từ tạo yêu cầu → tìm/phân công tài xế → thực hiện chuyến → tính cước → thanh toán → thông báo → đánh giá sau chuyến, có khả năng mở rộng linh hoạt (thêm dịch vụ, phương thức thanh toán, kênh thông báo) và đảm bảo bảo mật, ổn định khi vận hành ở quy mô lớn.

.....

#### Bước 2: Xác định Stakeholders (Các bên liên quan)

Sau khi hiểu được Business Context, BA cần xác định **ai là người liên quan đến dự án** và **mức độ ảnh hưởng/quan trọng** của từng người, để có kế hoạch giao tiếp và thu thập yêu cầu phù hợp.

**Danh sách Stakeholders**

| Stakeholder | Vai trò |
|---|---|
| Ban lãnh đạo / Ban giám đốc Công ty ABC | Người ra quyết định, phê duyệt ngân sách, định hướng chiến lược sản phẩm |
| Khách hàng (Customer) | Người sử dụng dịch vụ đặt xe, đặt chuyến, thanh toán, đánh giá tài xế |
| Tài xế (Driver) | Người thực hiện chuyến đi, nhận/từ chối yêu cầu, cập nhật trạng thái chuyến |
| Nhân viên vận hành (Operations Staff) | Quản trị hệ thống, xử lý sự cố, theo dõi chuyến, tra cứu báo cáo |
| Business Analyst (BA) | Phân tích, làm rõ yêu cầu, xác định phạm vi, tài liệu hóa nghiệp vụ |
| Đội phát triển (Development Team) | Thiết kế và xây dựng hệ thống dựa trên yêu cầu đã được làm rõ |
| Nhà cung cấp thanh toán bên ngoài (Payment Provider) | Đối tác tích hợp xử lý giao dịch thanh toán điện tử |
| Bộ phận bảo mật / Kiểm toán (Security/Audit) | Đảm bảo yêu cầu bảo mật, phân quyền, lưu vết thao tác được tuân thủ |


**Ma trận Stakeholders (Power/Interest Matrix)**

Ma trận thể hiện mức độ **quyền lực/ảnh hưởng (Power)** và **mức độ quan tâm (Interest)** của từng stakeholder đối với dự án, giúp BA xác định ưu tiên giao tiếp:

```mermaid
quadrantChart
    title Stakeholder Matrix - CAB System
    x-axis Low Interest --> High Interest
    y-axis Low Power --> High Power
    quadrant-1 Quản lý chặt chẽ
    quadrant-2 Giữ hài lòng
    quadrant-3 Giám sát tối thiểu
    quadrant-4 Giữ thông tin đầy đủ
    Ban lanh dao: [0.85, 0.9]
    Khach hang: [0.9, 0.4]
    Tai xe: [0.75, 0.35]
    Nhan vien van hanh: [0.6, 0.7]
    BA: [0.9, 0.65]
    Doi phat trien: [0.7, 0.55]
    Nha cung cap thanh toan: [0.3, 0.6]
    Bao mat Kiem toan: [0.4, 0.75]
```

**Diễn giải 4 nhóm trong ma trận:**

- **Quản lý chặt chẽ (High Power – High Interest):** Ban lãnh đạo, BA → cần giao tiếp thường xuyên, tham gia sâu vào các quyết định.
- **Giữ hài lòng (High Power – Low Interest):** Nhân viên vận hành, Bộ phận bảo mật → cần được thông báo và đảm bảo yêu cầu của họ được đáp ứng dù không tham gia hàng ngày.
- **Giữ thông tin đầy đủ (Low Power – High Interest):** Khách hàng, Tài xế, Đội phát triển → quan tâm trực tiếp đến kết quả, cần được cập nhật thông tin thường xuyên nhưng không có quyền quyết định phạm vi dự án.
- **Giám sát tối thiểu (Low Power – Low Interest):** Nhà cung cấp thanh toán bên ngoài → chỉ cần phối hợp ở mức tích hợp kỹ thuật, không cần tham gia sâu vào quá trình phân tích.
.....


#### Bước 3: Xác định Mục tiêu nghiệp vụ (Business Goals)

Từ Business Context và Business Purpose đã phân tích ở Bước 1, BA xác định các mục tiêu nghiệp vụ cụ thể mà hệ thống CAB cần đạt được:

| Mã | Mục tiêu nghiệp vụ (Business Goal) |
|---|---|
| BG01 | Tự động tìm và phân công tài xế phù hợp cho khách hàng |
| BG02 | Hỗ trợ thanh toán (cho phép thanh toán tiền mặt và thanh toán điện tử) |
| BG03 | Cung cấp khả năng theo dõi chuyến đi theo thời gian thực cho khách hàng |
| BG04 | Gửi thông báo tự động cho khách hàng và tài xế xuyên suốt vòng đời chuyến đi |
| BG05 | Cung cấp công cụ quản trị và báo cáo cho nhân viên vận hành |
| BG06 | Đảm bảo hệ thống có khả năng mở rộng (scalable) và hoạt động ổn định khi tải tăng cao |
| BG07 | Bảo vệ dữ liệu và kiểm soát truy cập theo đúng yêu cầu bảo mật |
| BG08 | Xây dựng kiến trúc linh hoạt, dễ mở rộng để bổ sung dịch vụ, phương thức thanh toán, kênh thông báo mới trong tương lai |
| BG09 | Nâng cao chất lượng dịch vụ thông qua cơ chế đánh giá tài xế sau chuyến đi |

**Diễn giải chi tiết:**

- **BG01 – Tự động tìm tài xế:** Khi khách hàng tạo chuyến, hệ thống tự động xác định tài xế phù hợp dựa trên vị trí, trạng thái sẵn sàng và tiêu chí vận hành, có cơ chế tìm tài xế khác nếu tài xế đầu tiên không phản hồi/từ chối, không yêu cầu khách hàng tạo lại yêu cầu.
- **BG02 – Hỗ trợ thanh toán:** Cho phép thanh toán tiền mặt và thanh toán điện tử qua nhà cung cấp thanh toán bên ngoài, không lưu trực tiếp thông tin nhạy cảm của thẻ/tài khoản trong hệ thống CAB, có cơ chế xử lý khi giao dịch điện tử thất bại.
- **BG03 – Theo dõi chuyến đi thời gian thực:** Khách hàng biết được trạng thái tìm tài xế, tài xế nhận chuyến, thời gian dự kiến đến và trạng thái hiện tại của chuyến đi.
- **BG04 – Thông báo:** Khách hàng nhận thông báo khi yêu cầu được tiếp nhận, tài xế nhận chuyến, tài xế đến điểm đón, chuyến hoàn thành, kết quả thanh toán; tài xế nhận thông báo về chuyến mới hoặc thay đổi liên quan.
- **BG05 – Công cụ quản trị & báo cáo:** Nhân viên vận hành quản lý khách hàng, tài xế, phương tiện, chuyến đi; xem báo cáo số lượng chuyến, doanh thu, tỷ lệ hoàn thành/hủy, hiệu quả hoạt động tài xế.
- **BG06 – Khả năng mở rộng & ổn định:** Các thành phần hệ thống scale độc lập khi tải tăng; lỗi ở một chức năng (thanh toán, thông báo) không làm sập toàn bộ hệ thống.
- **BG07 – Bảo mật:** Xác thực khách hàng/tài xế trước khi dùng chức năng yêu cầu tài khoản, kiểm soát quyền truy cập cho thao tác quản trị, bảo vệ dữ liệu cá nhân/vị trí/giao dịch, lưu vết (audit log) các thao tác quan trọng.
- **BG08 – Kiến trúc linh hoạt:** Cho phép bổ sung loại dịch vụ mới, phương thức thanh toán mới, nhà cung cấp thông báo mới hoặc thay đổi thành phần kỹ thuật mà không cần xây lại toàn bộ ứng dụng.
- **BG09 – Đánh giá tài xế:** Sau khi hoàn thành chuyến, khách hàng có thể đánh giá tài xế; dữ liệu đánh giá được dùng để cải thiện chất lượng dịch vụ và làm tiêu chí tham khảo trong hoạt động vận hành (ví dụ: theo dõi hiệu quả tài xế trong báo cáo — liên quan BG05).

.....

#### Bước 4: Xác định Phạm vi dự án (Scope)

Với thời gian triển khai này , BA cần xác định rõ phạm vi để đội phát triển tập trung xây dựng **MVP (Minimum Viable Product)** — đáp ứng đúng luồng nghiệp vụ cốt lõi mà khách hàng mô tả, đồng thời loại trừ các phần có thể mở rộng sau để tránh trễ tiến độ.

##### 4.1 Trong phạm vi (In-Scope)

**Module Khách hàng (Customer)**
- Đăng ký tài khoản, đăng nhập, cập nhật thông tin cá nhân
- Nhập điểm đón/điểm đến, chọn loại xe, gửi yêu cầu đặt xe
- Theo dõi trạng thái chuyến đi (đang tìm tài xế, tài xế đã nhận, thời gian dự kiến đến, trạng thái hiện tại)
- Xem lịch sử chuyến đi, số tiền phải trả
- Đánh giá tài xế sau khi hoàn thành chuyến

**Module Tài xế (Driver)**
- Đăng ký hoặc được nhân viên vận hành tạo tài khoản
- Cập nhật hồ sơ, thông tin phương tiện, trạng thái hoạt động (sẵn sàng/không sẵn sàng)
- Nhận thông báo yêu cầu chuyến phù hợp, chấp nhận/từ chối chuyến
- Cập nhật trạng thái chuyến: đã đến điểm đón, đã đón khách, đang di chuyển, hoàn thành
- Cập nhật vị trí để phục vụ tìm tài xế gần khách hàng

**Module Tìm & Phân công tài xế (Driver Matching)**
- Xác định tài xế phù hợp dựa trên vị trí, trạng thái sẵn sàng
- Cơ chế tìm tài xế thay thế khi tài xế được đề xuất không phản hồi/từ chối
- Thông báo cho khách hàng khi không tìm được tài xế

**Module Tính cước & Thanh toán (Fare & Payment)**
- Tính số tiền khách hàng phải trả dựa trên loại dịch vụ và thông tin chuyến đi
- Thanh toán tiền mặt
- Thanh toán điện tử (tích hợp với 1 nhà cung cấp thanh toán bên ngoài, không lưu thông tin thẻ nhạy cảm trong hệ thống CAB)
- Thông báo và cho phép xử lý lại khi giao dịch điện tử thất bại

**Module Thông báo (Notification)**
- Thông báo cho khách hàng: yêu cầu được tiếp nhận, tài xế nhận chuyến, tài xế đến điểm đón, chuyến hoàn thành, kết quả thanh toán
- Thông báo cho tài xế: chuyến mới, thay đổi liên quan đến chuyến đang thực hiện
- Kiến trúc cho phép mở rộng thêm kênh thông báo sau này (chỉ cần thiết kế sẵn, chưa cần triển khai nhiều kênh trong 7 tuần)

**Module Quản trị vận hành (Admin/Operations)**
- Quản lý khách hàng, tài xế, phương tiện, chuyến đi
- Xem các chuyến đang diễn ra, kiểm tra trạng thái tài xế
- Hỗ trợ xử lý chuyến bị lỗi, tra cứu lịch sử giao dịch
- Phân quyền cơ bản (nhân viên thường vs. thao tác nhạy cảm)
- Báo cáo cơ bản: số lượng chuyến, doanh thu, tỷ lệ hoàn thành/hủy, hiệu quả tài xế

**Yêu cầu phi chức năng (Non-Functional)**
- Xác thực người dùng (khách hàng, tài xế) trước khi dùng chức năng cần tài khoản
- Kiểm soát quyền truy cập cho thao tác quản trị
- Lưu vết (audit log) các thao tác quan trọng
- Kiến trúc cho phép các thành phần scale độc lập, cô lập lỗi (một module lỗi không kéo sập toàn hệ thống)

##### 4.2 Ngoài phạm vi (Out-of-Scope)

> Các mục dưới đây **không nên triển khai trong giai đoạn này**, do chưa được khách hàng chốt chi tiết hoặc không phải yêu cầu cốt lõi của MVP. BA cần ghi nhận và xác nhận lại với khách hàng để đưa vào roadmap giai đoạn sau.

- Tích hợp **nhiều** nhà cung cấp thanh toán cùng lúc (chỉ tích hợp 1 nhà cung cấp trong phạm vi MVP)
- Tích hợp **nhiều kênh thông báo** (SMS, email, push, in-app...) — chỉ cần 1 kênh chính, kiến trúc chừa sẵn khả năng mở rộng
- Tính năng khuyến mãi, mã giảm giá, chương trình khách hàng thân thiết
- Đặt xe hộ người khác, đặt xe theo lịch (đặt trước)
- Chat trực tiếp giữa khách hàng và tài xế trong ứng dụng
- Bản đồ nội bộ tự phát triển (dự kiến dùng dịch vụ bản đồ bên thứ ba có sẵn, không tự xây engine định vị/routing)
- Ứng dụng dành riêng cho nhiều ngôn ngữ/đa quốc gia (chỉ 1 ngôn ngữ/thị trường ban đầu)
- Chính sách hủy chuyến chi tiết, biểu phí phạt hủy (chưa được khách hàng chốt — cần làm rõ trước, xem mục Open Issues)
- Cách tính cước nâng cao (giờ cao điểm, surge pricing, khuyến mãi theo khu vực) — chỉ áp dụng công thức tính cước cơ bản trong MVP
- Xử lý chi tiết khi mất kết nối mạng (offline mode) — chưa được chốt, cần làm rõ
- Chính sách và thời gian lưu trữ dữ liệu dài hạn (data retention/archiving) — chưa được chốt, cần làm rõ
- Ứng dụng di động native (iOS/Android) đầy đủ — trong 7 tuần ưu tiên nền tảng chính (web hoặc 1 nền tảng di động), việc phát triển đa nền tảng đầy đủ để giai đoạn sau
- Tính năng phân tích nâng cao / AI dự đoán nhu cầu, tối ưu tuyến đường
....


#### Bước 5: Chuyển đổi thành Yêu cầu nghiệp vụ (Business Requirements)

Từ các Mục tiêu nghiệp vụ (Business Goals) đã xác định ở Bước 3, BA cụ thể hóa thành các **Yêu cầu nghiệp vụ (Business Requirements)** — mô tả những việc hệ thống phải làm được để đạt mục tiêu đề ra. Mỗi yêu cầu được ký hiệu bằng mã **BN**, được nhóm theo từng khối chức năng để dễ theo dõi.

##### Nhóm 1: Tài khoản & Hồ sơ người dùng

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN01 | Đăng ký & đăng nhập tài khoản | Khách hàng và tài xế có thể đăng ký tài khoản mới, đăng nhập vào hệ thống; tài xế có thể được nhân viên vận hành tạo tài khoản thay |
| BN02 | Quản lý thông tin cá nhân & hồ sơ | Khách hàng cập nhật thông tin cá nhân; tài xế cập nhật hồ sơ, thông tin phương tiện và trạng thái hoạt động |

##### Nhóm 2: Đặt xe & Tìm tài xế

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN03 | Đặt chuyến xe | Khách hàng nhập điểm đón, điểm đến, chọn loại xe và gửi yêu cầu đặt xe |
| BN04 | Tự động tìm tài xế | Hệ thống xác định tài xế phù hợp dựa trên vị trí, trạng thái sẵn sàng và tiêu chí vận hành; tự động tìm tài xế khác nếu tài xế đầu tiên không phản hồi/từ chối |
| BN05 | Thông báo khi không tìm được tài xế | Hệ thống thông báo rõ ràng cho khách hàng trong trường hợp không tìm được tài xế phù hợp |
| BN06 | Nhận/từ chối chuyến (tài xế) | Tài xế nhận thông báo yêu cầu chuyến phù hợp và có thể chấp nhận hoặc từ chối |

##### Nhóm 3: Thực hiện chuyến đi

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN07 | Cập nhật trạng thái chuyến đi | Tài xế cập nhật các trạng thái: đã đến điểm đón, đã đón khách, đang di chuyển, hoàn thành chuyến |
| BN08 | Theo dõi chuyến đi theo thời gian thực | Khách hàng theo dõi được trạng thái tìm tài xế, tài xế đã nhận chuyến, thời gian dự kiến đến và trạng thái hiện tại của chuyến |
| BN09 | Cập nhật vị trí tài xế | Hệ thống lưu và cập nhật vị trí tài xế để hỗ trợ tìm tài xế gần khách hàng và dự đoán thời gian đến |
| BN17 | Xem lịch sử chuyến đi | Khách hàng xem được lịch sử các chuyến đã thực hiện và số tiền đã trả |

##### Nhóm 4: Tính cước & Thanh toán

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN10 | Tính cước chuyến đi | Hệ thống xác định số tiền khách hàng phải trả dựa trên loại dịch vụ và thông tin chuyến đi sau khi hoàn thành |
| BN11 | Thanh toán tiền mặt | Khách hàng có thể thanh toán chuyến đi bằng tiền mặt |
| BN12 | Thanh toán điện tử | Khách hàng có thể thanh toán qua phương thức điện tử, tích hợp với nhà cung cấp thanh toán bên ngoài, không lưu thông tin nhạy cảm trong hệ thống CAB |
| BN13 | Xử lý lỗi thanh toán | Hệ thống thông báo cho khách hàng và cho phép xử lý lại khi giao dịch thanh toán điện tử thất bại |

##### Nhóm 5: Thông báo

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN14 | Thông báo cho khách hàng | Khách hàng nhận thông báo khi: yêu cầu được tiếp nhận, tài xế nhận chuyến, tài xế đến điểm đón, chuyến hoàn thành, kết quả thanh toán |
| BN15 | Thông báo cho tài xế | Tài xế nhận thông báo về chuyến mới hoặc thay đổi liên quan đến chuyến đang thực hiện |

##### Nhóm 6: Đánh giá dịch vụ

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN16 | Đánh giá tài xế | Khách hàng đánh giá tài xế sau khi hoàn thành chuyến đi |

##### Nhóm 7: Quản trị & Vận hành

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN18 | Quản trị khách hàng, tài xế, phương tiện, chuyến đi | Nhân viên vận hành quản lý thông tin khách hàng, tài xế, phương tiện và các chuyến đi trên giao diện quản trị |
| BN19 | Giám sát chuyến đi & xử lý sự cố | Nhân viên vận hành xem các chuyến đang diễn ra, kiểm tra trạng thái tài xế, hỗ trợ xử lý chuyến bị lỗi, tra cứu lịch sử giao dịch |
| BN20 | Phân quyền chức năng quản trị | Hệ thống phân quyền để nhân viên thông thường không thể thực hiện các thao tác quản trị nhạy cảm |
| BN21 | Báo cáo vận hành | Hệ thống cung cấp báo cáo về số lượng chuyến, doanh thu, tỷ lệ chuyến hoàn thành, tỷ lệ hủy và hiệu quả hoạt động của tài xế |

##### Nhóm 8: Bảo mật & Kiểm soát dữ liệu

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN22 | Xác thực người dùng | Khách hàng và tài xế phải được xác thực trước khi sử dụng các chức năng yêu cầu tài khoản |
| BN23 | Kiểm soát truy cập | Các thao tác quản trị phải được kiểm soát quyền truy cập |
| BN24 | Bảo vệ dữ liệu | Thông tin cá nhân, thông tin phương tiện, dữ liệu vị trí và dữ liệu giao dịch phải được bảo vệ |
| BN25 | Lưu vết thao tác (Audit log) | Hệ thống lưu vết các thao tác quan trọng để phục vụ kiểm tra khi có sự cố |

##### Nhóm 9: Kiến trúc & Khả năng mở rộng

| Mã | Tên yêu cầu | Diễn giải |
|---|---|---|
| BN26 | Khả năng mở rộng độc lập | Các thành phần của hệ thống có khả năng mở rộng (scale) độc lập khi tải tăng cao |
| BN27 | Cô lập lỗi giữa các module | Lỗi ở chức năng thanh toán hoặc thông báo không được làm toàn bộ hệ thống đặt xe ngừng hoạt động |
| BN28 | Triển khai từng phần (incremental deployment) | Các chức năng mới có thể được triển khai từng phần, hạn chế ảnh hưởng đến các chức năng đang hoạt động |
| BN29 | Kiến trúc mở rộng linh hoạt | Hệ thống có kiến trúc đủ linh hoạt để bổ sung loại dịch vụ mới, phương thức thanh toán mới, nhà cung cấp thông báo mới mà không cần xây lại toàn bộ ứng dụng |

**Ghi chú:** 29 yêu cầu nghiệp vụ được chia thành 9 nhóm, bám sát nội dung tài liệu yêu cầu gốc và có thể truy vết ngược về Business Goal tương ứng: Nhóm 2 → BG01; Nhóm 4 → BG02; Nhóm 3 (BN08) → BG03; Nhóm 5 → BG04; Nhóm 7 → BG05; Nhóm 9 → BG06 & BG08; Nhóm 8 → BG07; Nhóm 6 → BG09.









  
