---
status: final
updated: 2026-11-21
stepsCompleted:
  - step-01-validate-prerequisites
  - step-02-design-epics
  - step-03-create-stories
  - step-04-final-validation
inputDocuments:
  - _bmad-output/planning-artifacts/prds/prd-shoplite-2026-11-21/prd.md
  - _bmad-output/planning-artifacts/architecture/architecture-shoplite-2026-11-21/ARCHITECTURE-SPINE.md
  - _bmad-output/planning-artifacts/ux-designs/ux-shoplite-2026-11-21/DESIGN.md
  - _bmad-output/planning-artifacts/ux-designs/ux-shoplite-2026-11-21/EXPERIENCE.md
---

# ShopLite · Backlog vận hành đơn hàng

## Overview

Backlog cho lab nội bộ, từ tiếp nhận đơn, giao hàng và xử lý ngoại lệ đến khách xem tiến độ. Các quyết định
về phiên, tồn đầu kỳ, lịch giao lại và xác nhận tiền áp dụng cho ca lab sử dụng dữ liệu mẫu.

Đầu vào: [PRD](./prds/prd-shoplite-2026-11-21/prd.md),
[Architecture Spine](./architecture/architecture-shoplite-2026-11-21/ARCHITECTURE-SPINE.md),
[DESIGN.md](./ux-designs/ux-shoplite-2026-11-21/DESIGN.md) và
[EXPERIENCE.md](./ux-designs/ux-shoplite-2026-11-21/EXPERIENCE.md).

## Requirements Inventory

### Functional Requirements

- **FR-1** — Nhân viên tìm đơn Chờ xác nhận trong hàng đợi và mở đúng chi tiết.
- **FR-2** — Nhân viên đối chiếu ảnh, tên, số lượng, giá, phí và tổng từ snapshot. Phương thức và tình trạng thanh toán có nhãn riêng. Chủ đơn xem hướng dẫn chuyển khoản cho đến khi xác nhận tiền hoặc đơn không còn đủ điều kiện thanh toán.
- **FR-3** — Các vai trò dùng một vòng đời đơn theo thứ tự. Các bước sai hoặc thao tác chuyển mốc từ trạng thái kết thúc bị chặn.
- **FR-4** — Tiếp nhận chỉ giữ hàng khi đủ toàn bộ đơn. Tồn còn bán dùng chung cho trang sản phẩm và checkout. Đặt đơn chưa giữ hàng.
- **FR-5** — Nhân viên phân công, đổi người giao và bàn giao đúng điều kiện. Đổi sau bàn giao thu hồi quyền người cũ và yêu cầu xác nhận nhận kiện cho người mới.
- **FR-6** — Shipper hiện hành xác nhận đã giao sau bàn giao. Với COD, xác nhận đã thu tiền trong cùng thao tác.
- **FR-7** — Shipper ghi nhận giao hụt và lịch ngày tiếp theo. Nhân viên có thể hủy từ ngày D+3 theo Asia/Bangkok.
- **FR-8** — Nhân viên hủy ngoại lệ và xác nhận nhận lại hàng. Tồn sau bàn giao chỉ được hoàn khi đã nhận đúng, đủ và còn bán được ở từng dòng. Khoản thu xác nhận được giữ để đối soát.
- **FR-9** — Chủ đơn thấy năm mốc tiến độ, mốc hiện tại, nhánh hủy và giờ từ sự kiện đã lưu.
- **FR-10** — Trang và API kiểm tra quyền theo vai trò, chủ đơn và phân công. Bật gắn chủ cùng bảo vệ đường đọc và hủy. Khách chỉ tự hủy khi Chờ xác nhận và tải lại đơn để xác minh nếu mất phản hồi.

### NonFunctional Requirements

- **NFR-1** — Đơn, tồn, lịch sử và khoản thu nhất quán qua ghi đồng thời, gửi lặp, lỗi lưu và khởi động lại.
- **NFR-2** — Thông tin trả theo quyền hiện tại, kể cả API trực tiếp và các lệnh gửi lại.
- **NFR-3** — Tác vụ chính dùng được ở 390 px và bằng bàn phím. Nhãn chữ, focus và phản hồi có thể nhận biết.
- **NFR-4** — Lưu snapshot JSON với một tiến trình ghi. Tạo kho lab tách biệt bằng bước khởi tạo riêng. Runtime báo lỗi nếu vắng file. Bản lịch sử chỉ xem.

### Additional Requirements

- **AD-1–AD-3** — Giữ biên theo feature, cổng repository, xác thực dữ liệu ở server và snapshot tiền lúc đặt.
- **AD-4, AD-5, AD-7** — Dùng một vòng đời và một phép tính tồn. Lưu giữ hàng, giao, hủy và hoàn tồn cùng snapshot.
- **AD-6, AD-9, AD-10** — Kiểm tra phiên, quyền hiện tại và revision trước ghi. Kiểm tra quyền trước khi trả phản hồi cho request lặp.
- **AD-8, AD-11** — Dùng duy nhất một tiến trình ghi, kho tách biệt và ngày lịch Asia/Bangkok. Kiểm tra lỗi lưu, đồng thời và khôi phục.
- **AD-12** — Xác nhận chuyển khoản trước bàn giao. Xác nhận thu COD cùng lần giao thành công. Giữ khoản thu nếu đơn bị hủy.

### UX Design Requirements

- **UX-DR1** — Lối vào và màn hình theo vai trò. Đơn ngoài quyền hiện thông báo chung.
- **UX-DR2** — Hàng đợi có bộ lọc Chờ xác nhận, trạng thái rỗng và giữ vị trí khi trở về.
- **UX-DR3** — Thẻ chi tiết dùng dữ liệu lúc đặt. Phương thức và tình trạng thanh toán hiển thị riêng. Nhân viên thấy lý do khi chưa thể bàn giao. Chủ đơn vẫn thấy hướng dẫn chuyển khoản khi chưa xác nhận tiền và đơn chưa bàn giao hoặc kết thúc.
- **UX-DR4** — Màn có trạng thái đang tải, rỗng, lỗi, xung đột và thiếu quyền. Nếu mất phản hồi hủy, tải lại đơn để xác minh. Chỉ đổi dữ liệu hiển thị sau xác nhận lưu.
- **UX-DR5** — Tác vụ có nhãn rõ, xác nhận khi cần, khóa lúc gửi và trả focus khi đóng hộp xác nhận.
- **UX-DR6** — Trường chọn shipper có nhãn. Hiện Chờ bàn giao lại sau khi đổi người đang giao. Cấp dữ liệu giao và quyền ghi kết quả theo xác nhận nhận kiện của phân công hiện hành.
- **UX-DR7** — Ngày giao lại, điều kiện hủy, đối chiếu hàng nhận lại theo dòng và nhắc đối soát ở màn vận hành.
- **UX-DR8** — Timeline có năm mốc và nhánh hủy, đánh dấu mốc hiện tại và chỉ hiện thời điểm có căn cứ.
- **UX-DR9** — Bố cục ưu tiên 390 px, tiêu đề và badge theo DESIGN, vùng chạm 48 px cho tác vụ chính và thứ tự Tab rõ.
- **UX-DR10** — Lỗi thiếu hàng nêu dòng liên quan nếu có, cho phép chỉnh giỏ hoặc xử lý đơn hiện tại.

### FR Coverage Map

| Yêu cầu | Story |
| --- | --- |
| FR-1 | 1.7, 1.10, 4.2 |
| FR-2 | 1.8, 2.3, 4.1, 4.2 |
| FR-3 | 1.5, 1.9, 1.10, 2.2, 2.4, 2.6, 3.1, 3.2, 3.3, 4.1, 4.2, 4.3 |
| FR-4 | 1.1, 1.2, 1.4, 1.5, 1.9, 1.10, 1.11, 1.12, 2.6, 3.2, 3.4, 4.2, 4.3 |
| FR-5 | 1.5, 2.1, 2.2, 2.3, 2.4, 2.5, 4.2, 4.3 |
| FR-6 | 2.6, 4.2 |
| FR-7 | 3.1, 3.3, 4.3 |
| FR-8 | 1.6, 2.3, 3.2, 3.3, 3.4, 4.3 |
| FR-9 | 1.6, 2.6, 3.1, 4.1, 4.2, 4.3 |
| FR-10 | 1.1, 1.3, 1.4, 1.5, 1.6, 1.7, 1.8, 2.1, 2.5, 4.1, 4.2, 4.3 |

### NFR và UX Coverage Map

| Yêu cầu | Story |
| --- | --- |
| NFR-1 | 1.1, 1.2, 1.4, 1.5, 1.6, 1.9, 1.11, 1.12, 2.1, 2.2, 2.3, 2.4, 2.6, 3.1, 3.2, 3.3, 3.4, 4.2, 4.3 |
| NFR-2 | 1.3, 1.4, 1.5, 1.6, 1.7, 1.8, 2.1, 2.3, 2.4, 2.5, 2.6, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3 |
| NFR-3 | 1.3, 1.6, 1.7, 1.8, 1.10, 1.12, 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3 |
| NFR-4 | 1.1, 1.2, 1.11, 4.2, 4.3 |
| UX-DR1 | 1.3, 1.4, 1.6, 1.7, 2.5, 4.1, 4.2, 4.3 |
| UX-DR2 | 1.7 |
| UX-DR3 | 1.8, 2.3, 2.4, 2.6, 3.2, 4.1 |
| UX-DR4 | 1.3, 1.4, 1.5, 1.6, 1.7, 1.8, 1.9, 1.10, 1.11, 1.12, 2.3, 2.4, 2.5, 2.6, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3 |
| UX-DR5 | 1.5, 1.10, 2.1, 2.2, 2.3, 2.4, 2.6, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3 |
| UX-DR6 | 2.1, 2.2, 2.4, 2.5, 2.6, 3.1, 4.2, 4.3 |
| UX-DR7 | 3.1, 3.2, 3.3, 3.4, 4.3 |
| UX-DR8 | 1.6, 2.6, 4.1, 4.2, 4.3 |
| UX-DR9 | 1.3, 1.7, 1.8, 1.10, 2.1, 2.2, 2.3, 2.5, 4.1, 4.2, 4.3 |
| UX-DR10 | 1.9, 1.10, 1.11, 1.12 |

## Phạm vi theo nhóm việc

1. **Tiếp nhận đơn và giữ hàng**. Nhân viên có quyền tìm đúng đơn, đối chiếu tiền và giữ đủ hàng trước khi đưa vào xử lý. Khách đang mua hàng thấy cùng lượng tồn còn bán.
2. **Đóng gói và giao đúng người**. Nhân viên chuẩn bị hàng và bàn giao cho shipper được phân công. Shipper hiện hành hoàn tất giao để khách thấy kết quả và tồn phản ánh hàng đã bán.
3. **Xử lý giao thất bại và hoàn tồn**. Shipper ghi nhận lần giao hụt và lịch tiếp theo. Nhân viên hủy đúng thời điểm và đưa hàng về tồn bán khi cửa hàng thực sự nhận lại.
4. **Theo dõi tiến độ đơn của tôi**. Chủ đơn thấy đúng mốc đã xử lý và có thể tự hủy trong thời gian cho phép. Cửa hàng kiểm chứng được cả ca từ đặt đến giao trên dữ liệu mẫu.

Mỗi story được ước lượng cho khoảng 1–2 giờ. Làm story phụ thuộc trước và xác minh cả ca đi đúng luồng lẫn
ca biên. Tổ chức kiểm thử theo cổng server, trang và vai trò tương ứng với hành vi được thay đổi.

## Epic 1: Tiếp nhận đơn và giữ hàng

Nhân viên có quyền tìm đúng đơn, đối chiếu tiền và giữ đủ hàng trước khi đưa vào xử lý. Khách đang mua hàng thấy cùng lượng tồn còn bán.

**FRs covered:** FR-1, FR-2, FR-3, FR-4, FR-5, FR-8, FR-9, FR-10

### Story 1.1: Đọc đơn từ kho lab

Là kỹ thuật viên hỗ trợ cửa hàng, tôi cần đọc được đơn mới và bản sao đơn lịch sử để kiểm tra dữ liệu đầu ca.

**Phạm vi:** Đọc snapshot có version và mảng lịch sử. Có bước khởi tạo riêng cho người vận hành trước khi chạy ứng dụng.

**Yêu cầu:** FR-4, FR-10 · NFR-1, NFR-4.
**Kiến trúc:** AD-2, AD-5, AD-8, AD-11.
**Phụ thuộc:** Không có.

**Acceptance Criteria**

**AC 1 · Kho mới**

- **Given** người vận hành chọn một đường dẫn mới, tách biệt dành cho kho lab
- **When** người vận hành chủ động chạy bước khởi tạo trước khi mở ứng dụng
- **Then** tạo snapshot có version với kho trống và tồn gốc theo dữ liệu mẫu. Nếu chạy lại trên file đã có, từ chối ghi đè.

**AC 2 · Kho lịch sử**

- **Given** `ORDERS_FILE` trỏ vào bản sao mảng JSON lịch sử
- **When** repository nạp dữ liệu
- **Then** giữ được dữ liệu đơn cũ để xem, đánh dấu chỉ đọc và không suy ra lượng giữ hàng từ trạng thái.

**AC 3 · Kho lỗi**

- **Given** file tồn tại nhưng JSON hỏng hoặc version lạ
- **When** repository đọc file
- **Then** trả lỗi vận hành, giữ nguyên file và từ chối coi dữ liệu lỗi là kho trống.

**AC 4 · Vắng file khi ứng dụng chạy**

- **Given** `ORDERS_FILE` trỏ tới đường dẫn chưa khởi tạo hoặc kho trước đó đã có đơn và file vừa bị mất
- **When** ứng dụng nhận thao tác đọc hoặc ghi đơn
- **Then** báo kho không khả dụng với 503, chặn ghi và giữ nguyên đường dẫn trống để người vận hành khôi phục hoặc chuẩn bị kho riêng.

### Story 1.2: Lưu đơn và tồn nguyên tử

Là nhân viên, tôi cần thấy bản đơn và tồn nhất quán khi nhiều người thao tác hoặc máy chủ khởi động lại.

**Phạm vi:** Giao dịch snapshot, khóa theo đích, lưu bằng file tạm và kiểm thử hợp đồng repository JSON/in-memory.

**Yêu cầu:** FR-4 · NFR-1, NFR-4.
**Kiến trúc:** AD-2, AD-5, AD-8, AD-11.
**Phụ thuộc:** 1.1.

**Acceptance Criteria**

**AC 1 · Ghi cùng kho**

- **Given** hai lệnh cùng cập nhật snapshot của một đường dẫn
- **When** cả hai đọc, kiểm tra và lưu tuần tự dưới khóa
- **Then** lệnh sau nhìn trạng thái đã commit của lệnh trước và dữ liệu vẫn đọc được sau khởi động lại.

**AC 2 · Lỗi lưu**

- **Given** snapshot tốt đã tồn tại
- **When** ghi file tạm hoặc công bố bản mới thất bại
- **Then** snapshot trước vẫn đọc được, bản cập nhật không được báo thành công và API nhận lỗi vận hành 503.

**AC 3 · Kho chỉ đọc**

- **Given** nguồn đang ở dạng lịch sử
- **When** một lệnh nghiệp vụ yêu cầu ghi
- **Then** kho từ chối ghi và bản sao lịch sử giữ nguyên.

### Story 1.3: Đăng nhập vận hành

Là nhân viên hoặc shipper, tôi muốn mở ca bằng tài khoản đã cấp để vào đúng khu vực công việc.

**Phạm vi:** Trang đăng nhập nội bộ, xác minh thông tin cấp sẵn, phiên riêng và kiểm tra vai trò dùng chung.

**Yêu cầu:** FR-10 · NFR-2, NFR-3 · UX-DR1, UX-DR4, UX-DR9.
**Kiến trúc:** AD-1, AD-6, AD-9, AD-10.
**Phụ thuộc:** Không có.

**Acceptance Criteria**

**AC 1 · Vào đúng khu vực**

- **Given** tài khoản nhân viên hoặc shipper hợp lệ
- **When** người dùng đăng nhập
- **Then** server tạo phiên nội bộ và đưa người dùng đến khu vực ứng với vai trò đã xác minh.

**AC 2 · Phiên không hợp lệ**

- **Given** phiên đã hết hạn, thông tin sai hoặc gửi kèm vai trò khác trong request
- **When** màn vận hành hoặc API kiểm tra phiên
- **Then** không cấp vai trò dựa vào request, không trả dữ liệu đơn và dẫn về đăng nhập nội bộ hoặc trả 401.

**AC 3 · Sai vai trò**

- **Given** shipper đang có phiên hợp lệ
- **When** mở màn dành cho nhân viên
- **Then** màn báo thiếu quyền và API tương ứng trả 403.

### Story 1.4: Gắn đơn mới với phiên khách

Là khách, tôi muốn đơn vừa đặt được lưu cho phiên của mình ngay cả khi gửi lại do mất kết nối.

**Phạm vi:** Cấp phiên trước POST đầu tiên, gắn định danh chủ và khóa lần đặt, giữ giá và phí do server tính.

**Điều kiện phát hành:** Bật cùng Story 1.6 trên môi trường dùng chung. Cả trang và API đọc, hủy đơn phải áp dụng chính sách chủ đơn khi bật tạo đơn có chủ.

**Yêu cầu:** FR-4, FR-10 · NFR-1, NFR-2 · UX-DR1, UX-DR4.
**Kiến trúc:** AD-3, AD-5, AD-6, AD-9, AD-10.
**Phụ thuộc:** 1.2.

**Acceptance Criteria**

**AC 1 · Đặt đơn lần đầu**

- **Given** khách vào checkout và đã có phiên được server cấp
- **When** khách đặt giỏ hàng hợp lệ
- **Then** lưu đúng chủ, snapshot giá và một đơn Chờ xác nhận. Tồn chưa bị giữ.

**AC 2 · Gửi lại**

- **Given** hai POST dùng cùng phiên, khóa và giỏ hàng
- **When** khách gửi lại trước hoặc sau khi phản hồi đầu về
- **Then** cả hai nhận cùng mã đơn và chỉ có một đơn được lưu.

**AC 3 · Đổi nội dung cùng khóa**

- **Given** đơn đã commit với một khóa
- **When** request thứ hai dùng khóa đó với giỏ khác
- **Then** server trả 409 và đơn gốc giữ nguyên.

### Story 1.5: Kiểm tra lệnh trước khi ghi

Là người thao tác đơn, tôi muốn máy chủ từ chối lệnh sai bước hoặc từ màn hình cũ để tránh xử lý nhầm.

**Phạm vi:** Hàm vòng đời dùng chung và biên xử lý revision, quyền, dấu nội dung và replay. Các lệnh nghiệp vụ cụ thể ở những story sau.

**Yêu cầu:** FR-3, FR-4, FR-5, FR-10 · NFR-1, NFR-2 · UX-DR4, UX-DR5.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-9.
**Phụ thuộc:** 1.2, 1.3.

**Acceptance Criteria**

**AC 1 · Chặn nhảy bước**

- **Given** đơn đang `pending` hoặc đã kết thúc
- **When** lệnh yêu cầu bỏ qua bước hoặc chuyển vòng đời từ mốc kết thúc
- **Then** trả 409 và giữ nguyên trạng thái, revision và lịch sử.

**AC 2 · Revision cũ**

- **Given** lệnh đủ quyền nhưng đơn có revision mới hơn
- **When** gửi một `requestId` chưa xử lý với revision cũ
- **Then** trả 409. UI có thể tải bản mới để người dùng xác nhận lại.

**AC 3 · Quyền trước replay**

- **Given** shipper đã mất quyền trên đơn nhưng có `requestId` từng xử lý thành công
- **When** shipper gửi lại lệnh cũ
- **Then** server trả 404 và không trả dữ liệu đã cache. Actor còn quyền gửi lại đúng nội dung không tạo thêm hiệu ứng.

### Story 1.6: Bảo vệ quyền xem và tự hủy

Là khách, tôi muốn chỉ mình xem được đơn đã đặt và tự hủy nếu cửa hàng chưa tiếp nhận.

**Phạm vi:** Dùng chung chính sách cho trang danh sách, trang chi tiết và API cũ. Nối thao tác tự hủy vào use case phía server và khôi phục màn hình khi mất phản hồi. Bật cùng Story 1.4 theo điều kiện phát hành của story đó.

**Yêu cầu:** FR-8, FR-9, FR-10 · NFR-1, NFR-2, NFR-3 · UX-DR1, UX-DR4, UX-DR8.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-9, AD-10, AD-12.
**Phụ thuộc:** 1.3, 1.4, 1.5.

**Acceptance Criteria**

**AC 1 · Chủ đơn**

- **Given** khách dùng phiên đặt hai đơn của mình, kho còn đơn của người khác
- **When** mở trang Đơn của tôi và gọi API danh sách
- **Then** chỉ thấy hai đơn của mình. Link chi tiết và API chi tiết cùng trả 404 cho đơn khác chủ hoặc đơn legacy thiếu chủ.

**AC 2 · Chờ xác nhận**

- **Given** chủ đơn mở đơn của mình vẫn đang `pending`
- **When** gửi payload hủy cũ đã được hỗ trợ
- **Then** đơn sang Đã hủy, tồn giữ nguyên và bản ghi thanh toán đã xác nhận được bảo toàn nếu có. Màn nhân viên hiển thị nhắc đối soát cho đơn đã xác nhận tiền.

**AC 3 · Tranh thao tác**

- **Given** nhân viên vừa tiếp nhận trong lúc khách đang mở xác nhận hủy
- **When** khách gửi yêu cầu hủy
- **Then** server kiểm tra bản mới, trả 409 và không hoàn lượng đang giữ của đơn.

**AC 4 · Mất phản hồi sau khi đã lưu**

- **Given** khách gửi hủy đơn và server đã lưu hủy nhưng phản hồi bị mất
- **When** trang nhận biết yêu cầu mạng lỗi và tải lại đơn bằng phiên hiện tại
- **Then** rời trạng thái đang gửi và hiện **Đã hủy** theo bản ghi mới. Trang không gửi lại lệnh hủy.

**AC 5 · Chưa xác minh được kết quả hủy**

- **Given** phản hồi hủy bị mất và lần tải lại tự động cũng lỗi
- **When** khách chọn **Tải lại đơn**
- **Then** trang mở lại thao tác hủy nếu bản ghi mới vẫn **Chờ xác nhận** và khách còn quyền. Nếu đơn đã sang mốc khác, hiện mốc mới và ẩn thao tác hủy. Nếu vẫn chưa đọc được, giữ thao tác hủy bị khóa và cho thử tải lại.

**AC 6 · Bật quyền cùng lần phát hành**

- **Given** môi trường dùng chung chuẩn bị bật tạo đơn có chủ
- **When** kiểm tra từ một phiên khách khác trên đúng phiên bản sẽ phát hành
- **Then** trang và API danh sách, chi tiết, hủy đều áp dụng quyền trước khi bản có thể đưa vào sử dụng. GET không trả nội dung của đơn khác chủ và PATCH không đổi đơn đó.

### Story 1.7: Tìm đơn cần tiếp nhận

Là nhân viên, tôi muốn lọc đơn Chờ xác nhận trong hàng đợi để biết đơn nào cần làm đầu ca.

**Phạm vi:** Danh sách nhân viên, lọc Chờ xác nhận, quyền truy cập và trang chi tiết tối thiểu có mã và trạng thái.

**Yêu cầu:** FR-1, FR-10 · NFR-2, NFR-3 · UX-DR1, UX-DR2, UX-DR4, UX-DR9.
**Kiến trúc:** AD-1, AD-4, AD-6, AD-10.
**Phụ thuộc:** 1.3, 1.6.

**Acceptance Criteria**

**AC 1 · Lọc đơn**

- **Given** hàng đợi có đơn ở nhiều trạng thái
- **When** nhân viên chọn Chờ xác nhận và mở một thẻ
- **Then** chỉ hiện đơn phù hợp và chi tiết mở đúng mã. Khi quay lại, bộ lọc và vị trí xem được phục hồi.

**AC 2 · Không có kết quả**

- **Given** không có đơn Chờ xác nhận
- **When** nhân viên áp dụng bộ lọc
- **Then** hiện trạng thái rỗng có nút Xem tất cả và thao tác được bằng bàn phím ở 390 px.

**AC 3 · Ngoài quyền**

- **Given** người dùng thiếu quyền nhân viên
- **When** mở đường dẫn hàng đợi
- **Then** không nhận dữ liệu đơn trên màn hình hoặc API.

### Story 1.8: Đối chiếu sản phẩm và tiền

Là nhân viên, tôi muốn đối chiếu sản phẩm, số lượng và số tiền đúng lúc đặt trước khi lấy hàng.

**Phạm vi:** Mở rộng chi tiết tối thiểu của Story 1.7 với thẻ sản phẩm, tiền và trạng thái thanh toán ở chế độ xem.

**Yêu cầu:** FR-2, FR-10 · NFR-2, NFR-3 · UX-DR3, UX-DR4, UX-DR9.
**Kiến trúc:** AD-3, AD-6, AD-10, AD-12.
**Phụ thuộc:** 1.7.

**Acceptance Criteria**

**AC 1 · Giá khi đặt**

- **Given** giá danh mục đổi sau lúc khách tạo đơn
- **When** nhân viên mở chi tiết
- **Then** ảnh, tên, số lượng, đơn giá, phí và tổng hiển thị theo snapshot đã lưu. Phương thức và tình trạng thanh toán có nhãn riêng.

**AC 2 · Dữ liệu cũ**

- **Given** đơn lịch sử có phương thức chuyển khoản nhưng thiếu bản ghi xác nhận
- **When** nhân viên mở bản sao đối soát
- **Then** hiện Chưa xác minh và Đơn lịch sử · Chỉ xem. Nội dung dài xuống dòng trên 390 px.

### Story 1.9: Giữ đủ hàng khi tiếp nhận

Là nhân viên, tôi muốn tiếp nhận đơn chỉ khi còn đủ mọi dòng để tránh hứa giao hàng không còn.

**Phạm vi:** Use case tiếp nhận nguyên tử, lịch sử, tồn đã giữ và phản hồi thiếu hàng theo dòng.

**Yêu cầu:** FR-3, FR-4 · NFR-1 · UX-DR4, UX-DR10.
**Kiến trúc:** AD-4, AD-5, AD-7, AD-9.
**Phụ thuộc:** 1.5, 1.8.

**Acceptance Criteria**

**AC 1 · Tiếp nhận thành công**

- **Given** đơn Chờ xác nhận, mọi dòng đều còn đủ
- **When** nhân viên tiếp nhận
- **Then** một commit ghi Đang xử lý, một mốc lịch sử và lượng giữ cho toàn đơn.

**AC 2 · Tranh món cuối**

- **Given** hai đơn Chờ xác nhận tranh một món
- **When** hai người tiếp nhận cùng lúc
- **Then** chỉ một đơn được giữ hàng. Đơn còn lại nhận 409 và dòng thiếu, giữ nguyên trạng thái lẫn tồn các dòng khác.

**AC 3 · Lệnh lặp**

- **Given** đơn đã được giữ bằng một `requestId`
- **When** nhân viên gửi lại đúng request
- **Then** không tạo thêm lần giữ hàng hoặc mốc tiến độ.

### Story 1.10: Hiện kết quả tiếp nhận

Là nhân viên, tôi muốn biết đơn đã được nhận hay còn thiếu món để hành động tiếp trong ca.

**Phạm vi:** Nút Tiếp nhận và giữ hàng, xác nhận, khóa trong lúc chờ và thông báo kết quả trên chi tiết.

**Yêu cầu:** FR-1, FR-3, FR-4 · NFR-3 · UX-DR4, UX-DR5, UX-DR9, UX-DR10.
**Kiến trúc:** AD-4, AD-5, AD-9.
**Phụ thuộc:** 1.7, 1.9.

**Acceptance Criteria**

**AC 1 · Phản hồi xác nhận**

- **Given** đơn đủ điều kiện và đang Chờ xác nhận
- **When** nhân viên xác nhận tiếp nhận
- **Then** nút báo đang xử lý. Chỉ sau phản hồi lưu thành công mới hiện Đang xử lý và mốc đã lưu.

**AC 2 · Giữ hàng bị từ chối**

- **Given** server trả tranh tồn hoặc revision cũ
- **When** phản hồi về trang chi tiết
- **Then** tải lại và hiện dòng thiếu hoặc thông báo đơn vừa đổi. Hộp xác nhận hoạt động bằng bàn phím và trả focus khi đóng.

### Story 1.11: Kiểm tra tồn lúc đặt đơn

Là khách, tôi muốn checkout kiểm tra tồn vừa cập nhật của cửa hàng trước khi lưu đơn.

**Phạm vi:** Nối checkout với lượng bán được từ snapshot, giữ tính phí, validation và khóa gửi hiện có.

**Yêu cầu:** FR-4 · NFR-1, NFR-4 · UX-DR4, UX-DR10.
**Kiến trúc:** AD-3, AD-5, AD-7, AD-9.
**Phụ thuộc:** 1.4, 1.9.

**Acceptance Criteria**

**AC 1 · Đơn còn hàng**

- **Given** giỏ có đủ hàng trong snapshot và phí do server tính
- **When** khách gửi checkout
- **Then** đơn Chờ xác nhận dùng đúng giá, phí và tồn vẫn chưa giữ hàng.

**AC 2 · Giỏ lạc hậu**

- **Given** tồn vừa bị cửa hàng giữ sau khi khách mở giỏ
- **When** khách xác nhận đặt
- **Then** checkout trả thiếu hàng theo snapshot hiện tại, giữ nội dung giỏ để khách chỉnh và không tạo đơn.

### Story 1.12: Hiện tồn còn bán

Là khách, tôi muốn thấy lượng còn bán giống nhau khi xem các trang sản phẩm.

**Phạm vi:** Dùng chung phép tính tồn ở trang chủ, danh mục, chi tiết và API sản phẩm. Giữ kiểm tra cuối ở checkout.

**Yêu cầu:** FR-4 · NFR-1, NFR-3 · UX-DR4, UX-DR10.
**Kiến trúc:** AD-5, AD-7.
**Phụ thuộc:** 1.11.

**Acceptance Criteria**

**AC 1 · Cùng nguồn dữ liệu**

- **Given** nhân viên vừa giữ món cuối
- **When** khách mở lại danh mục, trang chi tiết và gọi API sản phẩm
- **Then** cả ba cùng trả hết hàng theo snapshot mới. Thẻ trên trang chủ áp dụng cùng dữ liệu.

**AC 2 · Kho không đọc được**

- **Given** snapshot không khả dụng
- **When** trang hoặc API sản phẩm cần tồn
- **Then** hiện trạng thái lỗi có thể thử lại thay cho việc hiển thị tồn mẫu như tồn thực.

## Epic 2: Đóng gói và giao đúng người

Nhân viên chuẩn bị hàng và bàn giao cho shipper được phân công. Shipper hiện hành hoàn tất giao để khách thấy kết quả và tồn phản ánh hàng đã bán.

**FRs covered:** FR-2, FR-3, FR-4, FR-5, FR-6, FR-8, FR-9, FR-10

### Story 2.1: Phân công và đổi shipper

Là nhân viên, tôi cần biết và cập nhật đúng người giao của một đơn.

**Phạm vi:** Lệnh gán hoặc đổi một tài khoản shipper nội bộ hợp lệ. Tạo lần phân công mới khi đổi người và hiển thị trạng thái chờ nhận kiện.

**Yêu cầu:** FR-5, FR-10 · NFR-1, NFR-2, NFR-3 · UX-DR5, UX-DR6, UX-DR9.
**Kiến trúc:** AD-4, AD-6, AD-9, AD-10.
**Phụ thuộc:** 1.5, 1.8, 1.9.

**Acceptance Criteria**

**AC 1 · Phân công**

- **Given** đơn đã được tiếp nhận và chưa kết thúc
- **When** nhân viên xác nhận shipper từ danh sách
- **Then** server lưu đúng một shipper hiện hành và UI đọc lại người được chọn.

**AC 2 · Đổi người đang giao**

- **Given** đơn Đã gửi đơn đã giao cho Minh
- **When** nhân viên đổi sang người giao khác
- **Then** đơn giữ **Đã gửi đơn** và mốc bàn giao đầu tiên. Phân công mới ở **Chờ bàn giao lại**, chỉ thấy thông tin công việc. Lệnh đến sau từ Minh bị từ chối, kể cả khi dùng mã yêu cầu đã từng thành công.

**AC 3 · Đơn kết thúc**

- **Given** đơn đã giao hoặc đã hủy
- **When** nhân viên gửi đổi shipper trực tiếp
- **Then** trả 409, giữ nguyên người đã được ghi nhận.

**AC 4 · Chọn lại cùng người**

- **Given** shipper hiện hành đã nhận kiện
- **When** nhân viên xác nhận lại đúng người đang được phân công
- **Then** giữ lần phân công, xác nhận đã nhận kiện và quyền hiện tại. Lịch sử không có sự kiện đổi người mới.

### Story 2.2: Xác nhận đóng gói

Là nhân viên, tôi muốn ghi nhận lúc hàng được đóng gói để biết đơn nào sẵn sàng bàn giao.

**Phạm vi:** Thao tác Đóng gói hoàn tất trên chi tiết, kèm guard trạng thái và phân công.

**Yêu cầu:** FR-3, FR-5 · NFR-1, NFR-3 · UX-DR5, UX-DR6, UX-DR9.
**Kiến trúc:** AD-4, AD-6, AD-9.
**Phụ thuộc:** 2.1.

**Acceptance Criteria**

**AC 1 · Đủ điều kiện**

- **Given** đơn Đang xử lý có shipper hiện hành
- **When** nhân viên xác nhận đóng gói
- **Then** lưu một mốc Đóng gói hoàn tất và giữ nguyên lượng đã phân bổ.

**AC 2 · Thiếu phân công**

- **Given** đơn Đang xử lý chưa có shipper
- **When** nhân viên tìm thao tác đóng gói hoặc gọi lệnh trực tiếp
- **Then** trang giải thích cần chọn shipper. Máy chủ từ chối cập nhật và không sinh mốc.

### Story 2.3: Xác nhận đã nhận chuyển khoản

Là nhân viên, tôi muốn ghi nhận khoản chuyển khoản đã đối chiếu để biết đơn nào đủ điều kiện giao.

**Phạm vi:** Một lệnh xác nhận thanh toán thủ công và phản hồi trên chi tiết. Phương thức và khoản xác nhận lưu riêng.

**Yêu cầu:** FR-2, FR-5, FR-8 · NFR-1, NFR-2, NFR-3 · UX-DR3, UX-DR4, UX-DR5, UX-DR9.
**Kiến trúc:** AD-3, AD-6, AD-9, AD-12.
**Phụ thuộc:** 1.5, 1.8.

**Acceptance Criteria**

**AC 1 · Đã đối chiếu**

- **Given** đơn chuyển khoản chưa bàn giao và chưa kết thúc
- **When** nhân viên xác nhận khoản thu đã đối chiếu
- **Then** lưu người xác nhận và giờ, rồi hiện Đã xác nhận. Trạng thái vòng đời và tồn giữ nguyên.

**AC 2 · Không đúng đối tượng**

- **Given** đơn là COD, đã kết thúc hoặc actor không phải nhân viên
- **When** có yêu cầu xác nhận chuyển khoản trực tiếp
- **Then** từ chối và giữ nguyên dữ liệu thanh toán. Lệnh lặp hợp lệ chỉ trả lại kết quả đã lưu.

### Story 2.4: Bàn giao đơn đủ điều kiện

Là nhân viên, tôi muốn bàn giao đơn đã đóng gói cho đúng shipper để bắt đầu giao có kiểm soát.

**Phạm vi:** Cùng hợp đồng kiểm tra thanh toán và người nhận cho bàn giao lần đầu và bàn giao lại. Lưu dấu vết riêng phục vụ quyền và hoàn tồn.

**Yêu cầu:** FR-3, FR-5 · NFR-1, NFR-2, NFR-3 · UX-DR3, UX-DR4, UX-DR5, UX-DR6.
**Kiến trúc:** AD-4, AD-6, AD-9, AD-10, AD-12.
**Phụ thuộc:** 2.2, 2.3.

**Acceptance Criteria**

**AC 1 · Bàn giao hợp lệ**

- **Given** đơn Đóng gói hoàn tất có shipper và là COD hoặc đã xác nhận chuyển khoản
- **When** nhân viên xác nhận bàn giao
- **Then** lưu sự kiện bàn giao đầu tiên gắn với lần phân công hiện tại, chuyển Đã gửi đơn và mở quyền ghi kết quả cho người đã nhận kiện.

**AC 2 · Chuyển khoản chưa xác nhận**

- **Given** đơn chuyển khoản chưa có bản ghi thu
- **When** nhân viên mở thao tác hoặc gửi lệnh bàn giao
- **Then** UI chỉ ra điều kiện thiếu. Lệnh trực tiếp bị từ chối và đơn giữ nguyên mốc, tồn, người giao và lịch sử.

**AC 3 · Đổi người trước khi xác nhận**

- **Given** shipper được đổi sau khi hộp xác nhận mở
- **When** nhân viên gửi revision cũ
- **Then** trả 409 và yêu cầu người thao tác xem lại người được giao trên bản mới.

**AC 4 · Xác nhận bàn giao lại**

- **Given** đơn **Đã gửi đơn** đang **Chờ bàn giao lại** và shipper hiện hành đã nhận kiện thực tế
- **When** nhân viên xác nhận bàn giao lại cho đúng người và revision hiện tại
- **Then** lưu xác nhận nhận kiện cho lần phân công mới, mở dữ liệu giao và thao tác ghi kết quả. Giữ nguyên **Đã gửi đơn**, tồn và mốc bàn giao đầu tiên. Gửi lặp không thêm một lần bàn giao.

### Story 2.5: Xem đơn được giao

Là shipper, tôi muốn thấy công việc của mình và thông tin cần để giao sau khi đã nhận hàng.

**Phạm vi:** Danh sách, chi tiết và dữ liệu theo phân công hiện tại, với nội dung khác nhau trước và sau khi lần phân công đó được xác nhận nhận kiện.

**Yêu cầu:** FR-5, FR-10 · NFR-2, NFR-3 · UX-DR1, UX-DR4, UX-DR6, UX-DR9.
**Kiến trúc:** AD-6, AD-9, AD-10.
**Phụ thuộc:** 2.4.

**Acceptance Criteria**

**AC 1 · Trước và sau bàn giao**

- **Given** đơn được gán cho Minh
- **When** Minh xem chi tiết trước rồi sau khi bàn giao
- **Then** trước bàn giao hiện mốc công việc và ẩn thông tin liên hệ. Sau bàn giao hiện dữ liệu liên hệ cần giao và các thao tác thuộc quyền.

**AC 2 · Thu hồi quyền**

- **Given** Minh còn mở chi tiết khi nhân viên đổi người giao
- **When** Minh tải lại hoặc gọi API đơn
- **Then** trả 404 cho Minh và bỏ đơn khỏi danh sách của anh. Người mới chỉ thấy thông tin công việc và nhãn **Chờ bàn giao lại**. Mở dữ liệu liên hệ và thao tác ghi kết quả sau xác nhận nhận kiện cho người mới.

**AC 3 · Chưa có đơn**

- **Given** shipper có phiên nhưng chưa được phân công
- **When** mở danh sách ở 390 px
- **Then** hiện Chưa có đơn được phân công và đọc được bằng công cụ hỗ trợ.

### Story 2.6: Ghi nhận giao thành công

Là shipper đang giao, tôi muốn xác nhận đã giao và đã thu COD nếu áp dụng để cửa hàng và khách thấy kết quả thật.

**Phạm vi:** Một lệnh hoàn tất giao với hành vi cho COD và chuyển khoản, cùng hộp xác nhận trên chi tiết shipper.

**Yêu cầu:** FR-3, FR-4, FR-6, FR-9 · NFR-1, NFR-2, NFR-3 · UX-DR3, UX-DR4, UX-DR5, UX-DR6, UX-DR8.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-9, AD-10, AD-12.
**Phụ thuộc:** 2.5.

**Acceptance Criteria**

**AC 1 · Giao thành công**

- **Given** shipper hiện hành đã được xác nhận nhận kiện ở lần phân công hiện tại và khách đã nhận đơn
- **When** shipper gửi xác nhận kèm đã thu tiền cho COD
- **Then** một commit ghi Đã giao, chuyển lượng giữ thành đã bán và ghi khoản COD nếu có. Lượng còn bán không giảm lần hai.

**AC 2 · Thiếu xác nhận COD**

- **Given** đơn COD đang Đã gửi đơn và shipper hiện hành đã được xác nhận nhận kiện
- **When** shipper xác nhận giao nhưng chưa chọn Đã thu COD
- **Then** báo thiếu xác nhận và giữ nguyên trạng thái, khoản thu, lượng giữ và lịch sử.

**AC 3 · Lệnh đến muộn**

- **Given** quyền bị thu hồi, đơn đã hủy hoặc lần phân công hiện hành chưa được xác nhận nhận kiện, kể cả đơn Chờ bàn giao lại
- **When** shipper gửi xác nhận trực tiếp
- **Then** server từ chối và giữ nguyên trạng thái, khoản thu, tồn và lịch sử. Lệnh hợp lệ gửi lặp giữ một mốc Đã giao.

## Epic 3: Xử lý giao thất bại và hoàn tồn

Shipper ghi nhận lần giao hụt và lịch tiếp theo. Nhân viên hủy đúng thời điểm và đưa hàng về tồn bán khi cửa hàng thực sự nhận lại.

**FRs covered:** FR-3, FR-4, FR-7, FR-8, FR-9

### Story 3.1: Hẹn giao lại sau thất bại

Là shipper, tôi muốn ghi nhận lần giao hụt để cửa hàng biết ngày giao lại.

**Phạm vi:** Một lệnh kết quả giao thất bại, đồng hồ có thể kiểm thử, lịch hẹn và thông báo cho vai trò có quyền.

**Yêu cầu:** FR-3, FR-7, FR-9 · NFR-1, NFR-2, NFR-3 · UX-DR4, UX-DR5, UX-DR6, UX-DR7.
**Kiến trúc:** AD-4, AD-6, AD-9, AD-10, AD-11.
**Phụ thuộc:** 2.5.

**Acceptance Criteria**

**AC 1 · Hẹn hôm sau**

- **Given** shipper hiện hành đã được xác nhận nhận kiện và thời điểm giao thất bại là 23:59 ngày D ở Asia/Bangkok
- **When** shipper lưu kết quả
- **Then** đơn vẫn Đã gửi đơn. Ngày hẹn là D+1, còn khi tải lại và chỉ hiện cho nhân viên cùng shipper hiện hành.

**AC 2 · Thất bại lần nữa**

- **Given** đơn có D là ngày thất bại đầu tiên
- **When** shipper ghi thất bại hợp lệ vào một ngày tiếp theo
- **Then** lưu lần mới và hẹn ngày lịch tiếp theo, giữ D làm mốc xét hủy. Gửi lại cùng yêu cầu không thêm lần giao.

**AC 3 · Chưa được giao**

- **Given** chưa xác nhận nhận kiện cho lần phân công hiện hành hoặc quyền của shipper cũ đã bị thu hồi
- **When** shipper mới chưa nhận kiện hoặc shipper cũ gửi kết quả thất bại
- **Then** yêu cầu bị từ chối và lịch giao giữ nguyên, kể cả khi đơn vẫn Đã gửi đơn.

### Story 3.2: Hủy trước bàn giao

Là nhân viên, tôi muốn hủy đơn thiếu hàng hoặc có ngoại lệ trước khi bàn giao để trả phần đã giữ về kho.

**Phạm vi:** Lệnh hủy của nhân viên trước bàn giao, lý do, cập nhật tồn, xác nhận và nhắc đối soát nếu có.

**Yêu cầu:** FR-3, FR-4, FR-8 · NFR-1, NFR-2, NFR-3 · UX-DR3, UX-DR4, UX-DR5, UX-DR7.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-9, AD-12.
**Phụ thuộc:** 1.9, 2.3.

**Acceptance Criteria**

**AC 1 · Đơn đã giữ hàng**

- **Given** đơn chưa bàn giao đã giữ hàng hoặc đã xác nhận tiền
- **When** nhân viên xác nhận hủy với lý do
- **Then** lưu Đã hủy, giải phóng lượng đang giữ đúng một lần và giữ bản ghi thu. Nếu có khoản thu, hiện nhắc đối soát.

**AC 2 · Đơn chưa giữ**

- **Given** đơn thiếu hàng vẫn Chờ xác nhận
- **When** nhân viên hủy với lý do
- **Then** đơn sang Đã hủy và tồn còn bán giữ nguyên.

**AC 3 · Mốc vừa thay đổi**

- **Given** đơn đã bàn giao ngay trước lệnh hủy dùng revision cũ
- **When** nhân viên gửi yêu cầu theo màn trước bàn giao
- **Then** trả 409, không hoàn hàng đang ở ngoài kho.

### Story 3.3: Hủy khi giao quá hạn

Là nhân viên, tôi muốn hủy đơn giao hụt sau thời hạn của lab để kiểm soát ngoại lệ và biết còn phải nhận hàng về.

**Phạm vi:** Guard thời điểm hủy sau bàn giao, hộp xác nhận và dấu chờ nhận hàng.

**Yêu cầu:** FR-3, FR-7, FR-8 · NFR-1, NFR-2, NFR-3 · UX-DR4, UX-DR5, UX-DR7.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-9, AD-11, AD-12.
**Phụ thuộc:** 3.1, 3.2.

**Acceptance Criteria**

**AC 1 · Đến hạn**

- **Given** đơn Đã gửi đơn thất bại lần đầu ngày D và vẫn chưa giao, có thể đang Chờ bàn giao lại
- **When** nhân viên xác nhận hủy từ 00:00 ngày D+3 theo Asia/Bangkok
- **Then** đơn sang Đã hủy, hiện chờ nhận lại và giữ lượng ngoài tồn bán. Với đơn đã xác nhận tiền, giữ bản ghi thu và hiện nhắc đối soát trên màn nhân viên.

**AC 2 · Chưa đến hạn**

- **Given** cùng đơn được xem ở 23:59 ngày D+2 hoặc chưa từng giao thất bại
- **When** nhân viên cố gửi lệnh hủy ngoại lệ
- **Then** UI giải thích điều kiện. Máy chủ từ chối và giữ thông tin lịch, tồn và trạng thái.

**AC 3 · Giao thắng tranh chấp**

- **Given** shipper vừa hoàn tất giao hợp lệ trước khi lệnh hủy được kiểm tra
- **When** nhân viên gửi lệnh từ màn hình cũ
- **Then** giữ Đã giao và từ chối hủy. Kết quả không tạo khoản hoàn tồn.

### Story 3.4: Nhận lại hàng và hoàn tồn

Là nhân viên, tôi muốn xác nhận hàng đã quay về trước khi đưa số lượng đó ra bán.

**Phạm vi:** Nhân viên đối chiếu đúng sản phẩm, đủ lượng đang giữ và tình trạng bán được theo từng dòng. Lưu xác nhận và hoàn tồn trong một commit khi tất cả dòng đạt.

**Yêu cầu:** FR-4, FR-8 · NFR-1, NFR-2, NFR-3 · UX-DR4, UX-DR5, UX-DR7.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-9, AD-11.
**Phụ thuộc:** 3.3.

**Acceptance Criteria**

**AC 1 · Hàng đã về**

- **Given** đơn Đã hủy sau bàn giao và đang chờ nhận lại
- **When** nhân viên xác nhận đã nhận đúng và đủ lượng còn bán được ở từng dòng đang giữ
- **Then** lưu lần nhận lại và giải phóng toàn bộ lượng giữ đúng một lần, với trạng thái đơn vẫn **Đã hủy**. Tải lại thấy đã nhận và tồn mới.

**AC 2 · Nhận lại hai lần**

- **Given** đơn đã có lần nhận lại được lưu
- **When** nhân viên gửi lại cùng yêu cầu hoặc gửi thao tác mới
- **Then** không cộng thêm tồn. UI hiển thị đã nhận lại.

**AC 3 · Sai điều kiện**

- **Given** đơn chưa hủy, đã giao hoặc đã hủy trước bàn giao
- **When** có yêu cầu nhận lại
- **Then** máy chủ từ chối và giữ nguyên lượng còn bán.

**AC 4 · Hàng thiếu, hỏng hoặc sai sản phẩm**

- **Given** đơn Đã hủy sau bàn giao và còn ít nhất một dòng thiếu, hỏng hoặc sai sản phẩm
- **When** nhân viên đối chiếu từng dòng rồi xác nhận nhận lại
- **Then** server trả 409 và giữ toàn bộ lượng của đơn ngoài tồn bán. Màn nhân viên vẫn hiện chờ nhận lại và chỉ rõ dòng cần đối soát. Khi nhân viên xác nhận đủ hàng bán được, áp dụng AC 1.

## Epic 4: Theo dõi tiến độ đơn của tôi

Chủ đơn thấy đúng mốc đã xử lý và có thể tự hủy trong thời gian cho phép. Cửa hàng kiểm chứng được cả ca từ đặt đến giao trên dữ liệu mẫu.

**FRs covered:** FR-1, FR-2, FR-3, FR-4, FR-5, FR-6, FR-7, FR-8, FR-9, FR-10

### Story 4.1: Xem tiến độ đơn của mình

Là khách, tôi muốn đọc tiến độ đơn qua từng mốc và thấy rõ khi đơn đã hủy.

**Phạm vi:** Timeline, hướng dẫn chuyển khoản trong chi tiết của chủ đơn và trạng thái tự hủy dựa trên bản ghi đã lưu.

**Yêu cầu:** FR-2, FR-3, FR-9, FR-10 · NFR-2, NFR-3 · UX-DR1, UX-DR3, UX-DR4, UX-DR5, UX-DR8, UX-DR9.
**Kiến trúc:** AD-4, AD-6, AD-9, AD-10, AD-11, AD-12.
**Phụ thuộc:** 1.6, 2.6, 3.3.

**Acceptance Criteria**

**AC 1 · Đúng tiến độ**

- **Given** chủ đơn mở chi tiết sau khi một bước hợp lệ được lưu
- **When** khách tải lại trang
- **Then** thấy đủ năm mốc với đúng một mốc hiện tại, thời gian theo sự kiện thực và nhánh Đã hủy nếu có.

**AC 2 · Giao hụt hoặc lịch sử thiếu**

- **Given** đơn đang giao lại hoặc dữ liệu mốc thiếu giờ
- **When** khách đọc tiến độ ở 390 px bằng bàn phím hoặc công cụ đọc màn hình
- **Then** mốc hiện tại có nhãn và aria-current, giờ thiếu để trống, lịch nội bộ và lý do giao hụt không xuất hiện.

**AC 3 · Mất phiên**

- **Given** khách mở đơn từ trình duyệt không còn phiên đã đặt
- **When** trang chi tiết kiểm tra quyền
- **Then** hiện thông báo chung và hướng dẫn liên hệ, không lộ mã, địa chỉ, số tiền hay tình trạng đơn.

**AC 4 · Chuyển khoản sau khi đơn được tiếp nhận**

- **Given** chủ đơn chọn chuyển khoản và khoản thu chưa được xác nhận
- **When** chủ đơn mở chi tiết ở mỗi trạng thái Chờ xác nhận, Đang xử lý hoặc Đóng gói hoàn tất
- **Then** vẫn thấy hướng dẫn chuyển khoản, số tiền từ snapshot của đơn và lời nhắc chờ xác nhận nếu đã chuyển khoản.

**AC 5 · Ẩn hướng dẫn khi hết điều kiện**

- **Given** tiền chuyển khoản đã được xác nhận, hoặc đơn đã bàn giao, đã giao, đã hủy hay chọn COD
- **When** chủ đơn tải lại chi tiết
- **Then** không hiện hướng dẫn chuyển khoản. Tình trạng thanh toán và tiến độ dựa trên bản ghi đã lưu.

### Story 4.2: Kiểm chứng ca giao thành công

Là người hướng dẫn lab, tôi muốn chạy lại một ca trên dữ liệu mẫu để thấy các màn hình và quyền khớp từ đầu đến cuối.

**Phạm vi:** Một kịch bản tích hợp từ checkout đến khách nhận đơn, dùng dữ liệu và tài khoản mẫu.

**Yêu cầu:** FR-1, FR-2, FR-3, FR-4, FR-5, FR-6, FR-9, FR-10 · NFR-1, NFR-2, NFR-3, NFR-4 · UX-DR1, UX-DR4, UX-DR5, UX-DR6, UX-DR8, UX-DR9.
**Kiến trúc:** AD-3, AD-4, AD-5, AD-6, AD-7, AD-8, AD-9, AD-10, AD-12.
**Phụ thuộc:** 1.12, 2.6, 4.1.

**Acceptance Criteria**

**AC 1 · Ca giao thành công**

- **Given** ca test có kho tách biệt, khách, Lan và Minh
- **When** khách đặt, Lan giữ hàng và bàn giao, Minh xác nhận giao
- **Then** Hà tải lại thấy Đã giao với đủ mốc, tồn được ghi đúng một lần và thao tác chính chạy được ở 390 px.

**AC 2 · Giữ luồng mua hiện có**

- **Given** người dùng đặt giỏ hợp lệ với giao tiêu chuẩn
- **When** kiểm thử mốc 500.000 ₫ và tải lại đơn mới
- **Then** phí và số tiền tuân thủ quy tắc hiện tại, đơn vẫn truy cập được từ phiên đặt hợp lệ.

### Story 4.3: Kiểm chứng ca giao thất bại

Là người hướng dẫn lab, tôi muốn chạy lại một ca trên dữ liệu mẫu để thấy các màn hình và quyền khớp từ đầu đến cuối.

**Phạm vi:** Một kịch bản tích hợp giao hụt với đồng hồ kiểm soát, xác nhận hạn hủy, hoàn tồn và thu hồi quyền.

**Yêu cầu:** FR-3, FR-4, FR-5, FR-7, FR-8, FR-9, FR-10 · NFR-1, NFR-2, NFR-3, NFR-4 · UX-DR1, UX-DR4, UX-DR5, UX-DR6, UX-DR7, UX-DR8, UX-DR9.
**Kiến trúc:** AD-4, AD-5, AD-6, AD-8, AD-9, AD-10, AD-11, AD-12.
**Phụ thuộc:** 3.4, 4.1.

**Acceptance Criteria**

**AC 1 · Ca giao hụt**

- **Given** một đơn khác đã bàn giao gặp thất bại và đồng hồ test đi tới D+3
- **When** nhân viên hủy rồi ghi nhận hàng quay về
- **Then** khách thấy Đã hủy, tồn chỉ tăng sau nhận lại. Kiểm thử khẳng định yêu cầu trực tiếp từ sai chủ hoặc shipper cũ bị chặn.

**AC 2 · Thử gần hạn**

- **Given** đơn đã bàn giao và thất bại lần đầu ngày D
- **When** chạy ca ở 23:59 ngày D+2 rồi ở 00:00 ngày D+3
- **Then** trước hạn lệnh hủy thất bại. Từ đúng hạn, hủy được nếu đơn còn Đã gửi đơn và chỉ hoàn tồn sau khi nhận lại.
