---
title: "ShopLite | Vận hành đơn hàng"
status: draft
created: 2026-11-21
updated: 2026-11-21
---

# Product brief · Vận hành đơn hàng ShopLite

## Vấn đề và mục tiêu

ShopLite đã có luồng mua hàng trên web, từ danh mục và giỏ hàng đến đặt và xem đơn. Sau khi đặt, đơn dừng ở **Chờ xác nhận**. Nhân viên thiếu công cụ để tiếp nhận, đóng gói và bàn giao đơn. Khách không thấy tiến độ giao, còn tồn kho hiển thị không giảm theo các đơn đã được cửa hàng nhận xử lý.

MVP giúp một cửa hàng vận hành trọn ca trên ShopLite. Nhân viên biết đơn nào cần xử lý, shipper biết đơn nào được giao cho mình, khách biết đơn đang ở bước nào, và cửa hàng phân bổ đúng số lượng còn bán. Đây là tính năng mở rộng của ShopLite hiện tại. Phạm vi triển khai dành cho buổi học nội bộ, với yêu cầu nghiệp vụ và quyền truy cập như một sản phẩm thực tế.

## Người dùng và phạm vi MVP

| Người dùng | Việc cần làm |
| --- | --- |
| Nhân viên cửa hàng | Xem hàng đợi đơn, nhận biết trạng thái thanh toán, tiếp nhận và đánh dấu đóng gói hoàn tất. Chọn hoặc đổi shipper, ghi nhận bàn giao và xử lý đơn thiếu hàng hoặc giao thất bại. |
| Shipper | Chỉ xem đơn được phân công. Ghi nhận giao thành công hoặc thất bại. Khi thất bại, hẹn giao lại vào ngày hôm sau. |
| Khách hàng | Xem trạng thái đơn của mình trên trang chi tiết. Tiếp tục tự hủy khi đơn còn **Chờ xác nhận**, theo quy tắc hiện tại. |

Luồng chính đi qua **Chờ xác nhận → Đang xử lý → Đóng gói hoàn tất → Đã gửi đơn → Đã giao**. Khách thấy đầy đủ các mốc và thấy **Đã hủy** khi đơn kết thúc theo nhánh hủy. Nhân viên chọn shipper khi đóng gói. Sau khi đổi người giao, quyền xem và xác nhận chuyển sang người mới. Shipper được phân công xác nhận giao thành công sau khi giao tận tay khách.

Khi nhận xử lý, ShopLite kiểm tra và giữ hàng trong cùng thao tác, đồng thời giảm số lượng còn bán. Nếu hai đơn tranh món cuối, chỉ một đơn được giữ hàng. Khi thiếu hàng, nhân viên có thể hủy đơn và liên hệ khách. Hủy trước khi bàn giao sẽ giải phóng số lượng đã giữ. Nếu hàng đã bàn giao, cửa hàng chỉ đưa hàng về tồn bán được sau khi xác nhận nhận lại.

**Phạm vi giai đoạn sau** gồm phiếu hoàn trả, xác nhận nơi trả, yêu cầu đóng gói hàng trả và hàng đợi ưu tiên hoàn trả. Luồng tự chọn món thay thế và voucher cũng thuộc giai đoạn sau.

## Điều kiện thành công đề xuất

- Trong một ca thử, nhân viên và shipper hoàn tất một đơn từ **Chờ xác nhận** đến **Đã giao**. Khách thấy đúng từng mốc trên đơn của mình.
- Với hai đơn cùng yêu cầu một sản phẩm chỉ còn một món, tồn bán được không âm, chỉ một đơn được giữ hàng và thao tác gửi lại không trừ tồn lần hai.
- Khi đổi shipper, người cũ mất quyền xác nhận. Khách chỉ xem được đơn của mình và shipper chỉ thấy đơn được giao, kể cả khi gọi API trực tiếp.
- Kịch bản hủy trước bàn giao và kịch bản trả hàng sau giao thất bại đều cập nhật tồn đúng lúc, kể cả sau khi tải lại ứng dụng.

## Ràng buộc và quyết định trước PRD

ShopLite hiện lưu đơn bằng JSON và dùng dữ liệu mẫu cho danh mục. MVP không thêm cơ sở dữ liệu và không có ngân sách cho dịch vụ ngoài. Cần giữ luồng mua hàng đang chạy và xác thực quyền ở máy chủ.

- **Giao lại và hủy:** Tạm tính ba ngày từ lần giao thất bại đầu tiên. Cần chốt cách tính ngày, cơ chế tự động hay nhân viên xác nhận hủy, và việc khách có thấy lịch giao lại hay không.
- **Quyền truy cập:** Chốt cách nhận diện nhân viên, shipper và chủ đơn trong điều kiện hiện có, kể cả với đơn cũ.
- **Tồn kho và lưu trữ:** Chốt tồn ban đầu, cách xử lý đơn cũ và yêu cầu nhất quán giữa tồn với trạng thái đơn khi cập nhật đồng thời hoặc lưu lỗi.
- **Thanh toán:** Chốt đơn chưa thanh toán nào được giao, cách nhận diện COD và quy trình xử lý tiền đối với đơn đã thanh toán rồi bị hủy.

## Nguồn

Phần chốt MVP trong cuộc trò chuyện ngày 21/11/2026 và [README ShopLite](../../../../README.md). Tệp `brainstorm-intent.md` được chỉ định và `AGENTS.md` chưa có trong workspace khi soạn.
