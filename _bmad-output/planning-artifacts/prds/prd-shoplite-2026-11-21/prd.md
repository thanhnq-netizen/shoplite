---
title: "ShopLite · Vận hành đơn hàng"
status: final
created: 2026-11-21
updated: 2026-11-21
---

# PRD: ShopLite · Vận hành đơn hàng

## 0. Document Purpose

Tài liệu xác định phạm vi MVP và tiêu chí nghiệm thu cho nhóm sản phẩm, thiết kế, phát triển và kiểm thử trong buổi lab nội bộ. Yêu cầu mang nhãn **MỚI** là phần cần xây dựng. Mục **HIỆN CÓ · GIỮ NGUYÊN** ghi các hành vi đã kiểm chứng và cần kiểm thử hồi quy. Căn cứ nghiệp vụ là [product brief](../../briefs/brief-shoplite-2026-11-21/brief.md) và phần chốt brainstorming trong cuộc trò chuyện. [HTML mobile](../../briefs/brief-shoplite-2026-11-21/shoplite-demo.html) là bản minh họa bằng dữ liệu mẫu. Chưa tìm thấy brief addendum, `phan-bien.md` hay `AGENTS.md` trong workspace khi soạn.

## 1. Vision

ShopLite đã cho khách xem sản phẩm, đặt và xem đơn. Đường tạo đơn hiện ghi **Chờ xác nhận** và chức năng cập nhật hiện chỉ cho khách hủy ở mốc đó. Cửa hàng cần đưa đơn qua các bước nhận xử lý, đóng gói, bàn giao và giao thành công. Tồn hiển thị hiện lấy từ danh mục mẫu.

MVP giúp một cửa hàng vận hành trọn ca trên ShopLite. Nhân viên thấy việc cần làm, shipper nhận đúng đơn, khách biết đơn đã đi tới đâu, và tồn còn bán phản ánh những đơn cửa hàng đã nhận xử lý.

## 2. Target User

### 2.1 Jobs To Be Done

- **Nhân viên cửa hàng** cần tìm đơn cần xử lý, đối chiếu sản phẩm và số tiền, giữ hàng, đóng gói, phân công và bàn giao.
- **Shipper** cần thấy đơn được giao và ghi nhận kết quả để cửa hàng biết khi nào cần giao lại.
- **Khách hàng** cần thấy tiến độ đơn của mình và có thể tự hủy khi đơn còn **Chờ xác nhận**.

### 2.2 Non-Users (v1)

Đối tác vận chuyển bên ngoài, nhân viên của cửa hàng khác và nhóm hậu mãi có quy trình đổi trả độc lập.

### 2.3 Key User Journeys

Các tên dưới đây dùng để minh họa các hành trình đã chốt trong brief.

- **UJ-1. Lan xử lý một đơn đầu ca.** Lan mở hàng đợi đã được cấp quyền, xem ảnh, số lượng, tổng tiền và thông tin thanh toán. Cô tiếp nhận đơn để giữ hàng, chọn shipper khi đóng gói rồi ghi nhận bàn giao. Nếu thiếu hàng khi tiếp nhận, Lan thấy dòng thiếu và đơn vẫn ở **Chờ xác nhận** để cô xử lý ngoại lệ. FR-1–FR-5, FR-8.
- **UJ-2. Minh giao đơn được phân công.** Minh mở danh sách của mình và xem đơn. Sau khi nhận kiện, Minh ghi nhận **Đã giao** hoặc lần giao thất bại để hẹn lại hôm sau. Nếu cửa hàng đổi shipper, Minh mất quyền xem và cập nhật đơn. Shipper mới chờ nhân viên xác nhận đã bàn giao lại kiện. FR-5–FR-7.
- **UJ-3. Hà kiểm tra tiến độ đơn.** Hà mở chi tiết đơn của mình và nhận biết mốc hiện tại. Nếu chọn chuyển khoản và tiền chưa được xác nhận, cô vẫn xem được cách thanh toán sau khi đơn được tiếp nhận hoặc đóng gói. Khi còn **Chờ xác nhận**, cô có thể tự hủy. Nếu mất phản hồi, Hà xem lại trạng thái đã lưu để biết kết quả. FR-2, FR-3, FR-9, FR-10.

## 3. Glossary

| Thuật ngữ | Định nghĩa |
| --- | --- |
| Đơn hàng | Giao dịch đã đặt, gồm các dòng sản phẩm, phí giao, tổng tiền và thông tin nhận hàng. |
| Tồn còn bán | Số lượng còn có thể phân bổ cho đơn khác. |
| Giữ hàng | Phân bổ số lượng cho một đơn khi tiếp nhận, làm giảm tồn còn bán. |
| Bàn giao | Nhân viên giao hàng cho shipper được phân công, đưa đơn sang **Đã gửi đơn**. |
| Giao lại | Lần giao dự kiến sau một lần giao thất bại. |
| Nhận lại hàng | Nhân viên đối chiếu đủ số lượng và tình trạng bán được theo từng dòng hàng đã giữ của đơn hủy sau bàn giao. |
| Phương thức thanh toán | COD hoặc chuyển khoản. Tình trạng thanh toán chỉ thành Đã xác nhận khi người có quyền ghi nhận khoản thu. |
| Chủ đơn | Khách hàng có quyền xem và tự hủy đơn của mình theo điều kiện nghiệp vụ. |

## 4. Features

### HIỆN CÓ · GIỮ NGUYÊN

| Hành vi | Điểm kiểm chứng |
| --- | --- |
| Khi tạo đơn, server dùng Zod, gộp dòng trùng sản phẩm, lấy giá danh mục, tính lại phí và tổng tiền, lưu đơn giá theo thời điểm đặt và tạo đơn ở trạng thái `pending`. Nguồn tồn để kiểm tra được nâng cấp theo FR-4. | [`placeOrder`, `mergeItems`](../../../../src/features/orders/order-service.ts), [`placeOrderSchema`](../../../../src/features/checkout/checkout-schema.ts) |
| Giao tiêu chuẩn 30.000 ₫, miễn phí từ 500.000 ₫. Giao nhanh 60.000 ₫. | [`calculateShippingFee`, `calculateOrderTotals`](../../../../src/features/checkout/pricing.ts) |
| Khách chỉ tự hủy khi đơn ở **Chờ xác nhận**. API hiện nhận `PATCH /api/orders/:id` với `{ "status": "cancelled" }`. FR-10 bổ sung kiểm tra chủ đơn ở server. | [`cancelOrder`](../../../../src/features/orders/order-service.ts), [Route Handler](../../../../src/app/api/orders/%5Bid%5D/route.ts) |
| Chi tiết đơn hiện có sản phẩm, giá, phí giao, tổng tiền và phương thức thanh toán. Danh sách và chi tiết hiện đọc theo request, nhưng chưa áp dụng quyền chủ đơn. | [Chi tiết đơn](../../../../src/app/orders/%5Bid%5D/page.tsx), [Danh sách đơn](../../../../src/app/orders/page.tsx), [API danh sách](../../../../src/app/api/orders/route.ts) |

### 4.1 Hàng đợi và vòng đời đơn

**Description:** Nhân viên xử lý đơn từ hàng đợi. Các vai trò dùng chung ý nghĩa của từng mốc. Thực hiện UJ-1 đến UJ-3.

#### FR-1: Hàng đợi đơn của nhân viên

**MỚI.** Nhân viên vào hàng đợi, xác định đơn ở **Chờ xác nhận** và mở chi tiết đơn cần xử lý. Thực hiện UJ-1.

**Consequences (testable):** Nhân viên có quyền tìm được đơn mới và biết trạng thái hiện tại, kể cả khi hàng đợi có đơn ở nhiều mốc.

#### FR-2: Thông tin cần cho một ca xử lý

**MỚI.** Màn nhân viên hiển thị ảnh, tên, số lượng, đơn giá, phí giao, tổng tiền của đơn và thông tin thanh toán. Thực hiện UJ-1.

**Consequences (testable):** Số tiền khớp với đơn đã đặt dù giá danh mục đổi. Phương thức và tình trạng thanh toán hiển thị riêng. Nếu chưa có xác nhận, hiện **Chưa xác minh**. Với đơn chuyển khoản ở **Chờ xác nhận**, **Đang xử lý** hoặc **Đóng gói hoàn tất**, chủ đơn thấy hướng dẫn và tổng tiền lúc đặt đến khi khoản thu được xác nhận. Hướng dẫn ẩn sau xác nhận tiền, khi đơn đã bàn giao hoặc đã kết thúc. Nhân viên xác nhận chuyển khoản đã đối chiếu trước khi bàn giao. Đơn COD được bàn giao khi chưa thu tiền. Lần xác nhận lưu người thực hiện và thời điểm.

#### FR-3: Chuyển trạng thái theo vòng đời

**MỚI.** Luồng chính là **Chờ xác nhận → Đang xử lý → Đóng gói hoàn tất → Đã gửi đơn → Đã giao**. **Đã hủy** là nhánh kết thúc. Thực hiện UJ-1 đến UJ-3.

**Consequences (testable):** Server từ chối bước bỏ qua và mọi chuyển trạng thái từ **Đã giao** hoặc **Đã hủy**. Giao thất bại được ghi trên đơn **Đã gửi đơn**. FR-8 cho phép xác nhận hàng đã quay về sau khi hủy mà trạng thái đơn vẫn là **Đã hủy**.

### 4.2 Tồn còn bán và ngoại lệ

**Description:** Việc tiếp nhận, hủy hoặc nhận lại hàng có tác động nhất quán đến tồn còn bán. Thực hiện UJ-1 và UJ-2.

#### FR-4: Giữ hàng khi tiếp nhận đơn

**MỚI.** Khi nhân viên đưa đơn sang **Đang xử lý**, hệ thống kiểm tra và giữ đủ mọi dòng sản phẩm. Danh mục và kiểm tra khi khách đặt đơn sử dụng tồn còn bán. Giữ hàng phát sinh ở bước tiếp nhận.

**Consequences (testable):** Hai đơn tranh món cuối chỉ có một đơn giữ được. Nếu một dòng thiếu, đơn vẫn **Chờ xác nhận** và tồn của mọi dòng giữ nguyên. Gửi lặp thao tác tiếp nhận không trừ tồn lần hai. Giao thành công không trừ thêm lượng đã giữ.

### 4.3 Phân công và giao hàng

**Description:** Nhân viên kiểm soát phân công và bàn giao. Shipper ghi nhận kết quả của đơn được giao. Thực hiện UJ-1 và UJ-2.

#### FR-5: Phân công, đổi shipper và bàn giao

**MỚI.** Nhân viên chọn shipper trước khi hoàn tất đóng gói và có thể đổi shipper khi đơn chưa kết thúc. Nhân viên ghi nhận **Đã gửi đơn** sau khi bàn giao.

**Consequences (testable):** Mỗi đơn có tối đa một shipper hiện hành. Khi đổi, shipper cũ mất quyền xem và cập nhật kể cả khi gửi lại thao tác đã thực hiện. Đổi sau bàn giao giữ nguyên **Đã gửi đơn** và đưa phân công mới vào trạng thái **Chờ bàn giao lại**. Shipper mới chỉ thấy thông tin công việc cho đến khi nhân viên xác nhận người đó đã nhận kiện. Sau xác nhận mới cấp thông tin cần giao và quyền ghi kết quả. Bàn giao và bàn giao lại đều kiểm tra shipper hiện hành và điều kiện thanh toán theo §8.4.

#### FR-6: Ghi nhận giao thành công

**MỚI.** Shipper được phân công xác nhận giao thành công sau khi khách nhận hàng.

**Consequences (testable):** Thao tác chỉ khả dụng với shipper hiện hành đã được xác nhận nhận kiện cho lần phân công đó. Với COD, shipper xác nhận đã thu tiền trong cùng thao tác hoàn tất giao. Thiếu xác nhận thu COD thì trạng thái đơn, tiền và tồn giữ nguyên. Sau khi lưu thành công, khách thấy **Đã giao**. Gửi lặp giữ một mốc hoàn tất và một lần chuyển lượng giữ thành đã bán.

#### FR-7: Ghi nhận giao thất bại và giao lại

**MỚI.** Shipper hiện hành đã được xác nhận nhận kiện có thể ghi nhận lần giao thất bại. Cửa hàng thấy lịch giao lại dự kiến vào ngày hôm sau.

**Consequences (testable):** Lần thất bại và ngày hẹn còn khi tải lại. Đơn vẫn **Đã gửi đơn**. Mỗi lần thất bại hẹn ngày lịch hôm sau. Từ 00:00 ngày thứ ba sau ngày thất bại đầu tiên, nhân viên có thể xác nhận hủy. Lần thất bại tiếp theo giữ nguyên mốc xét hủy. Lịch hẹn hiển thị cho nhân viên và shipper hiện hành theo §8.3.

#### FR-8: Hủy ngoại lệ và nhận lại hàng

**MỚI.** Nhân viên có thể hủy đơn trước bàn giao với lý do, gồm thiếu hàng, hoặc hủy đơn đã bàn giao khi đạt hạn tại FR-7. Nhân viên có thể liên hệ khách qua kênh đang dùng và xác nhận nhận lại hàng sau khi đơn đã bàn giao được hủy.

**Consequences (testable):** Hủy trước lần bàn giao đầu tiên giải phóng lượng đã giữ đúng một lần. Hủy đơn chưa từng giữ hàng giữ nguyên tồn. Hủy sau lần bàn giao đầu tiên, kể cả đang chờ bàn giao lại, giữ lượng hàng ngoài tồn bán. Chỉ hoàn tồn khi nhân viên xác nhận đúng và đủ số lượng đang giữ, còn bán được ở từng dòng. Hàng thiếu, hỏng hoặc sai sản phẩm tiếp tục chờ nhận lại để đối soát, với tồn bán giữ nguyên cho cả đơn. Xác nhận sớm, gửi lặp hoặc xác nhận cho đơn không cần nhận lại giữ nguyên tồn. Nếu thanh toán đã xác nhận, đơn hủy vẫn giữ bản ghi tiền và hiển thị nhắc nhân viên đối soát ngoài ShopLite.

### 4.4 Theo dõi và quyền theo đơn

**Description:** Quyền được kiểm tra tại server cho cả đường đọc và cập nhật. Thực hiện UJ-1 đến UJ-3.

#### FR-9: Theo dõi tiến độ cho khách

**MỚI.** Chủ đơn thấy toàn bộ năm mốc, mốc hiện tại và **Đã hủy** nếu có. Thông tin lịch sử chỉ hiển thị theo dữ liệu xác nhận được.

**Consequences (testable):** Sau mỗi thao tác hợp lệ, tải lại trang cho thấy trạng thái mới. Mốc chỉ có giờ khi có sự kiện đã lưu. Khi giao thất bại, khách tiếp tục thấy **Đã gửi đơn**, còn ngày hẹn thuộc màn hình vận hành.

#### FR-10: Quyền xem và cập nhật theo vai trò

**MỚI.** Khách chỉ xem đơn của mình. Shipper chỉ xem đơn được phân công. Nhân viên chỉ truy cập chức năng thuộc vai trò. **HIỆN CÓ · GIỮ NGUYÊN:** Chủ đơn chỉ tự hủy ở **Chờ xác nhận**.

**Consequences (testable):** Truy cập URL và gọi API trực tiếp đều áp dụng quyền. Khi bật gắn chủ đơn trên môi trường dùng chung, trang và API đọc, hủy đơn đã kiểm tra cùng chính sách chủ đơn. Đơn lịch sử thiếu chủ chỉ nhân viên có quyền mới đọc được. Shipper hiện hành thấy thông tin liên hệ cần giao sau khi nhận kiện ở lần phân công hiện tại. Khách tự hủy sau khi cửa hàng đã tiếp nhận bị từ chối ở server. Nếu mất phản hồi sau khi gửi hủy, trang dừng trạng thái đang gửi và tải lại đơn trước khi cho phép thử lại. Đơn đã hủy hiện **Đã hủy**. Khi việc tải lại cũng lỗi, khách có thể chọn **Tải lại đơn** và thao tác hủy vẫn khóa đến khi biết trạng thái.

## 5. Non-Goals (Explicit)

- Quản lý nhiều cửa hàng, mua hàng nhập kho và kiểm kê tổng thể.
- Tích hợp hãng vận chuyển, cổng thanh toán hoặc dịch vụ thông báo ngoài.
- Tái thiết kế danh mục, giỏ và checkout ngoài thay đổi cần thiết về tồn và quyền xem đơn.

## 6. MVP Scope

### 6.1 In Scope

- Hàng đợi, thông tin đơn, chuyển trạng thái và giữ hàng.
- Phân công/đổi shipper, xác nhận giao, giao lại và hủy ngoại lệ.
- Theo dõi theo chủ đơn, kiểm tra quyền phía server và giữ quy tắc tự hủy.
- Web trên ứng dụng ShopLite hiện tại, ưu tiên thao tác thuận tiện trên điện thoại cho nhân viên và shipper.

### 6.2 Out of Scope for MVP

- Giai đoạn sau gồm phiếu hoàn trả hậu mãi, nơi trả, phiếu đóng gói hàng trả và hàng đợi ưu tiên hoàn trả. Nhận lại hàng của đơn giao thất bại thuộc FR-8.
- Giai đoạn sau gồm tự chọn món thay thế và voucher khi thiếu hàng.

## 7. Success Metrics

**Primary**

- **SM-1** — Hoàn tất một ca kiểm thử xuyên suốt. Một đơn đi qua đủ năm mốc và chủ đơn thấy trạng thái đúng sau mỗi lần tải lại. Kiểm chứng FR-1–FR-6 và FR-9.

**Secondary**

- **SM-2** — Hai đơn tranh một món, đúng một đơn giữ được và tồn còn bán bằng 0. Kịch bản hủy trước bàn giao và nhận lại sau bàn giao cập nhật đúng một lần, kể cả sau tải lại. Kiểm chứng FR-4 và FR-8.
- **SM-3** — Tất cả ca thử truy cập khác chủ đơn, truy cập khác shipper và thao tác của shipper cũ đều bị chặn trên trang và API. Kiểm chứng FR-5, FR-6 và FR-10.

**Counter-metrics (do not optimize)**

- **SM-C1** — Số lần bỏ bước, ghi nhận giao thiếu bàn giao hoặc giảm tồn hai lần phải bằng 0. Dùng để cân bằng SM-1: rút ngắn thao tác vẫn phải giữ đủ các kiểm tra.

## 8. Quy tắc cho lab nội bộ

1. **Quyền truy cập (OQ-1).** Tài khoản nhân viên và shipper được cấp sẵn. Khách nhận phiên trình duyệt khi vào checkout. Khách mở đơn mới bằng phiên đã dùng khi đặt. Nếu mất phiên, lab hiển thị thông báo chung và để cửa hàng hỗ trợ ngoài ứng dụng. Đơn lịch sử chưa gắn chủ chỉ nhân viên có quyền xem.
2. **Tồn đầu kỳ (OQ-2).** Người vận hành chủ động tạo kho lab mới ở đường dẫn tách biệt trước khi chạy ứng dụng, dùng tồn ban đầu từ dữ liệu mẫu. Nếu thiếu file trong lúc ứng dụng chạy, báo kho tạm thời không khả dụng và chặn ghi đến khi khôi phục. Bản sao dữ liệu lịch sử dùng để xem và đối soát, với thao tác ghi bị chặn. Đối soát và nhập kho thật thuộc đợt triển khai sau.
3. **Giao lại (OQ-3).** Dùng ngày lịch `Asia/Bangkok`. Gọi ngày của lần thất bại đầu tiên là D. Nhân viên được xác nhận hủy từ 00:00 của D+3 theo giờ nghiệp vụ. Ví dụ, thất bại đầu tiên ngày 21/11 thì có thể xác nhận hủy từ 00:00 ngày 24/11. Mỗi lần thất bại hẹn ngày lịch kế tiếp và giữ nguyên D. Nhân viên và shipper hiện hành thấy ngày hẹn. Khách thấy trạng thái **Đã gửi đơn** cho đến khi giao hoặc hủy.
4. **Thanh toán (OQ-4).** Nhân viên đối chiếu và xác nhận chuyển khoản trước khi bàn giao. Với COD, bàn giao có thể thực hiện trước khi thu tiền. Shipper hiện hành xác nhận đã thu COD khi ghi nhận giao thành công. Khi hủy đơn có khoản thanh toán đã xác nhận, giữ nguyên bản ghi thu và hiển thị nhắc đối soát thủ công cho nhân viên.

## 9. Assumptions Index

- **A-1** (FR-7, §8.3) — Dùng ba ngày lịch và thao tác xác nhận của nhân viên như quy tắc buổi lab.
- **A-2** (FR-4, FR-10, §8.1–§8.2) — Dùng phiên khách theo trình duyệt và kho mẫu tách biệt. Phương án nhận lại quyền và đối soát dữ liệu thật cần được chốt khi có kế hoạch triển khai thật.
- **A-3** (FR-2, FR-5, FR-6, FR-8, §8.4) — Xác nhận thu tiền thủ công theo vai trò. Trường hợp cần hoàn tiền được xử lý ngoài ShopLite.

## 10. Cross-Cutting NFRs

- **NFR-1 · Nhất quán.** Lỗi lưu hoặc thao tác đồng thời giữ đơn và tồn nhất quán. Kết quả đã xác nhận vẫn đọc được sau khi khởi động lại. Gửi lặp một thao tác lưu tạo tối đa một hiệu ứng nghiệp vụ.
- **NFR-2 · Riêng tư.** Chỉ cấp thông tin cần thiết theo vai trò. Kiểm tra tại server cho đường đọc và ghi, kể cả yêu cầu API trực tiếp.
- **NFR-3 · Khả dụng trên web.** Trạng thái và lỗi có nhãn chữ. Tác vụ chính dùng được ở bề rộng 390 px và bằng bàn phím.
- **NFR-4 · Ràng buộc triển khai.** Tiếp tục lưu JSON với [`OrderRepository`](../../../../src/features/orders/order-repository.ts) và một tiến trình ghi. Chi tiết cơ chế ghi và khôi phục nằm trong [Architecture Spine](../../architecture/architecture-shoplite-2026-11-21/ARCHITECTURE-SPINE.md).
