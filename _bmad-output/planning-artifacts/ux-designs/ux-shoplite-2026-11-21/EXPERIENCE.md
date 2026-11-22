---
name: 'ShopLite · Vận hành đơn hàng'
status: final
updated: 2026-11-21
sources:
  - '../../prds/prd-shoplite-2026-11-21/prd.md'
  - '../../architecture/architecture-shoplite-2026-11-21/ARCHITECTURE-SPINE.md'
  - '../../briefs/brief-shoplite-2026-11-21/shoplite-demo.html'
---

# ShopLite · Hành vi và luồng vận hành đơn hàng

## Foundation

Web tiếng Việt, ưu tiên điện thoại 390 px cho nhân viên và shipper, dùng được với bàn phím. Kế thừa
Tailwind CSS, các thành phần dùng chung và trang xem đơn của ShopLite. [DESIGN.md](./DESIGN.md) quy định
màu, kiểu chữ và thành phần. [PRD](../../prds/prd-shoplite-2026-11-21/prd.md) xác định phạm vi lab. Các
quyết định trong [Architecture Spine](../../architecture/architecture-shoplite-2026-11-21/ARCHITECTURE-SPINE.md)
quy định cách kiểm tra quyền và ghi nhận thay đổi.

Phiên xác thực và quyền từ server quyết định nội dung, đơn được xem và thao tác được thực hiện.
Bản thử HTML dùng dữ liệu mẫu và có nút đổi vai để minh họa. Giao diện thật lấy vai trò từ phiên.
Nhân viên và shipper dùng trang đăng nhập nội bộ. Nếu khách mất phiên trình duyệt, màn hình đơn hiện
thông báo chung và hướng dẫn liên hệ cửa hàng.

## Information Architecture

| Màn hình | Người dùng đi vào từ đâu | Nội dung và hành động | Nguồn |
| --- | --- | --- | --- |
| Hàng đợi đơn | Lối vào vận hành của nhân viên có quyền | Danh sách có mã, trạng thái và lối mở chi tiết. Có cách xem các đơn Chờ xác nhận khi danh sách lẫn nhiều mốc. | FR-1, FR-10 |
| Chi tiết đơn cho nhân viên | Chọn đơn trong hàng đợi | Sản phẩm, ảnh, số lượng, phí, tổng tiền, thông tin thanh toán, tiến độ và shipper hiện tại. Thao tác theo mốc gồm giữ hàng, phân công, đóng gói, bàn giao, hủy và nhận lại hàng theo quyền. | FR-2–FR-5, FR-7, FR-8 |
| Đơn được giao | Lối vào vận hành của shipper có quyền | Danh sách đơn được phân công cho chính shipper. Chọn đơn để mở chi tiết. | FR-5, FR-10 |
| Chi tiết giao hàng | Chọn đơn trong Đơn được giao | Trạng thái và dữ liệu công việc. Chỉ hiện dữ liệu cần giao cùng thao tác ghi kết quả sau khi xác nhận nhận kiện cho lần phân công hiện tại. | FR-5–FR-7, FR-10 |
| Đơn của tôi | Mục đơn hàng hiện có | Chỉ hiện đơn của chủ đơn và lối mở chi tiết. | FR-9, FR-10 |
| Tiến độ đơn của khách | Đơn của tôi hoặc luồng xác nhận đặt hàng hiện có | Chi tiết giao dịch, hướng dẫn chuyển khoản khi đủ điều kiện, năm mốc tiến độ và nhánh Đã hủy. Chỉ hiện tự hủy khi đơn còn Chờ xác nhận và khách có quyền. | FR-2, FR-3, FR-9, FR-10 |

## Voice and Tone

Gọi tên đúng việc sắp xảy ra bằng **Tiếp nhận và giữ hàng**, **Đóng gói hoàn tất**, **Xác nhận bàn giao**,
**Xác nhận đã giao**, **Ghi nhận giao thất bại** và **Xác nhận đã nhận lại hàng**. Kết quả cần cho biết
đơn đang ở mốc nào hoặc người dùng có thể làm gì tiếp theo.

| Tình huống | Nội dung mẫu |
| --- | --- |
| Thiếu hàng khi tiếp nhận | “Chưa thể tiếp nhận đơn. Kiểm tra các sản phẩm thiếu hàng bên dưới.” |
| Dữ liệu đã thay đổi | “Đơn vừa được cập nhật. Kiểm tra lại trạng thái trước khi thao tác tiếp.” |
| Giao thất bại | “Đã ghi nhận lần giao thất bại. Dự kiến giao lại: [ngày do server trả về].” |
| Đơn ngoài quyền hoặc không tồn tại | “Không tìm thấy đơn hàng.” |
| Tình trạng thanh toán chưa xác minh | “Chưa xác minh” ở trường tình trạng, cạnh trường phương thức COD hoặc chuyển khoản. |
| Chuyển khoản chưa đủ điều kiện bàn giao | “Xác nhận đã nhận chuyển khoản để bàn giao đơn.” |
| Khách xem đơn chuyển khoản chưa xác minh | “Nếu bạn đã chuyển khoản, vui lòng chờ cửa hàng xác nhận.” |
| Đổi shipper sau bàn giao | “Chờ bàn giao lại. Xác nhận khi shipper mới đã nhận kiện.” |
| Hàng nhận lại thiếu hoặc hỏng | “Chưa hoàn tồn. Kiểm tra sản phẩm, số lượng và tình trạng từng dòng.” |
| Mất phản hồi khi hủy | “Chưa biết kết quả hủy. Tải lại đơn để kiểm tra trạng thái.” |
| Kho không khả dụng | “Kho dữ liệu tạm thời không khả dụng. Vui lòng thử lại sau.” |
| Hủy đơn đã xác nhận tiền | “Đơn đã hủy. Kiểm tra khoản thanh toán và đối soát với khách.” |

## Component Patterns

| Thành phần | Hành vi |
| --- | --- |
| Thẻ đơn và lọc trạng thái | Nhãn mở chi tiết nêu được mã đơn. Bộ lọc Chờ xác nhận cập nhật kết quả và có tùy chọn xem lại tất cả. Khi quay về từ chi tiết, ưu tiên phục hồi bộ lọc và vị trí đang xem. |
| Timeline đơn | Danh sách có thứ tự Chờ xác nhận → Đang xử lý → Đóng gói hoàn tất → Đã gửi đơn → Đã giao. Đồng bộ badge và mốc hiện tại theo dữ liệu server. Với đơn đã hủy, giữ mốc có căn cứ và đánh dấu Đã hủy là kết quả hiện tại. Mốc không đạt vẫn hiện là chưa hoàn tất. Thiếu lịch sử thì bỏ phần thời điểm chưa biết. |
| Chọn / đổi shipper | Hiện người được giao và yêu cầu xác nhận khi đổi. Nếu chưa có shipper, đưa người dùng về trường chọn và giải thích điều kiện đóng gói. Sau khi đổi sau bàn giao, giữ **Đã gửi đơn**, hiện **Chờ bàn giao lại** và rút quyền của người cũ. Nhân viên thấy nút **Xác nhận bàn giao lại** kèm tên người sẽ nhận kiện. |
| Thanh toán | Chủ đơn chuyển khoản thấy hướng dẫn và tổng lúc đặt khi khoản thu chưa xác nhận ở **Chờ xác nhận**, **Đang xử lý** hoặc **Đóng gói hoàn tất**. Ẩn hướng dẫn khi xác nhận tiền, đã bàn giao hoặc đơn kết thúc. Nhân viên thấy nút **Xác nhận đã nhận chuyển khoản** trên đơn đủ điều kiện. Shipper đã nhận kiện thấy ô **Đã thu COD** khi xác nhận giao đơn COD. |
| Nhóm thao tác | Mỗi mốc có tối đa một nút chính. Nút phụ và nút hủy đặt riêng. Trong lúc gửi, khóa thao tác liên quan, giữ nhãn tiến trình và xử lý kết quả theo bản ghi server trả về. |
| Thông báo lỗi hoặc kết quả | Đặt sát trạng thái hoặc thao tác liên quan. Lỗi thiếu hàng chỉ rõ dòng bị thiếu nếu server có dữ liệu. Lỗi lưu giữ thông tin được xác nhận gần nhất và cho phép kiểm tra lại kết quả. |

## State Patterns

| Trạng thái | Hành vi |
| --- | --- |
| Đang tải | Giữ khung và tiêu đề màn hình, hiển thị “Đang tải đơn hàng” và chưa mở nút thao tác. |
| Rỗng | Hàng đợi đã lọc hiện “Không có đơn Chờ xác nhận” và nút **Xem tất cả**. Shipper chưa có đơn thấy “Chưa có đơn được phân công”. |
| Lỗi đọc hoặc ghi | Hiện thông báo và hành động **Thử lại** hoặc **Tải lại đơn**. Chỉ cập nhật badge, tồn và lịch sử khi server xác nhận đã lưu. Nếu kho không khả dụng, hiện thông báo chung và giữ khóa các thao tác ghi. |
| Chưa rõ kết quả hủy | Rời trạng thái đang gửi và tải lại đơn. Nếu đã hủy, cập nhật mốc **Đã hủy**. Nếu đơn vẫn **Chờ xác nhận** và khách còn quyền, mở lại thao tác hủy. Nếu lần tải lại cũng lỗi, hiện nút **Tải lại đơn** và giữ thao tác hủy bị khóa. |
| Xung đột hoặc lịch sử vừa đổi | Tải lại nếu server báo 409. Với tranh tồn, hiện thiếu hàng và giữ đơn ở Chờ xác nhận nếu server trả kết quả đó. Với dữ liệu cũ, bỏ trạng thái lạc hậu và để người dùng chọn tiếp trên bản mới. Nếu quyền bị thu hồi, xóa nội dung đơn cũ và quay về danh sách phù hợp. |
| Thiếu phiên hoặc thiếu quyền | Nhân viên hoặc shipper thiếu phiên được đưa về đăng nhập nội bộ. Khách thiếu phiên thấy thông báo chung và hướng dẫn liên hệ cửa hàng. Vai trò sai ở cấp màn hình thấy “Bạn không có quyền truy cập”. Đơn ngoài quyền trả cùng thông báo “Không tìm thấy đơn hàng” như đơn không tồn tại (AD-6, AD-9, AD-10). |
| Đơn lịch sử | Nhân viên xem dữ liệu có sẵn, thấy nhãn **Đơn lịch sử · Chỉ xem** và không có nhóm thao tác ghi (AD-5). |
| Chờ bàn giao lại | Nhân viên thấy tên người được chỉ định và nút xác nhận bàn giao lại. Shipper mới chỉ thấy thông tin công việc cho tới khi nhân viên xác nhận nhận kiện. Khách tiếp tục thấy **Đã gửi đơn**. |
| Đã giao | Hiển thị xác nhận và lịch sử có thể chứng minh. Khóa các thao tác thay đổi vòng đời. |
| Đã hủy | Hiển thị Đã hủy và dừng thao tác chuyển mốc. Với đơn hủy sau bàn giao, nhân viên đối chiếu từng dòng trước khi **Xác nhận đã nhận lại hàng**. Hàng thiếu, hỏng hoặc sai sản phẩm hiện lý do và tiếp tục chờ nhận lại. Chỉ hiện đã nhận lại sau khi toàn bộ dòng đạt và server lưu thành công. |

## Interaction Primitives

1. **Tiếp nhận.** Hiển thị xác nhận giữ hàng trước khi gửi. Nếu giữ thành công, đợi bản ghi mới để
   hiện **Đang xử lý**. Nếu một dòng thiếu, hiện các dòng thiếu và tình trạng hiện tại. Nhân viên có thể
   liên hệ khách hoặc hủy đơn theo FR-8.
2. **Đóng gói và bàn giao.** Chọn shipper trước khi hoàn tất đóng gói. Sau khi đã đóng gói, dùng
   **Xác nhận bàn giao** và kiểm tra shipper hiện tại trên xác nhận. Sau khi đổi người đang giao,
   dùng **Xác nhận bàn giao lại** khi người mới đã nhận kiện. Shipper chỉ nhận thông tin cần giao
   và thao tác ghi kết quả khi lần phân công của mình đã được bàn giao theo AD-10. Cho phép bàn giao COD chưa thu. Với
   chuyển khoản, giải thích điều kiện còn thiếu và giữ thao tác bàn giao ở trạng thái chưa khả dụng
   cho đến khi server xác nhận tiền (AD-12).
3. **Kết quả giao hàng.** Shipper hiện hành đã nhận kiện ghi nhận đã giao hoặc giao thất bại. Khi thất bại,
   giữ **Đã gửi đơn** và hiện lịch giao lại dự kiến ngày hôm sau cho nhân viên và shipper. Khách
   tiếp tục thấy mốc Đã gửi đơn. Từ 00:00 ngày D+3 theo giờ nghiệp vụ, nhân viên có quyền xác nhận
   hủy. D là ngày thất bại đầu tiên. Với giao thành công đơn COD, shipper phải xác nhận đã thu tiền.
4. **Hủy và nhận lại.** Hủy trước lần bàn giao đầu tiên giải phóng lượng đã giữ. Đơn từng bàn giao
   chờ nhận lại, kể cả khi đang chờ bàn giao lại. Nhân viên đối chiếu từng dòng theo sản phẩm, số lượng
   và tình trạng bán được. Nếu còn dòng chưa đạt, hiển thị lý do và giữ toàn bộ lượng ngoài tồn bán.
   Khách giữ hộp xác nhận tự hủy ở Chờ xác nhận. Với đơn đã xác nhận khoản thu, nhắc nhân viên đối
   soát ngoài ShopLite.
5. **Gửi lại.** Khi mất phản hồi hủy, tải lại đơn và thông báo kết quả bằng trạng thái mới. Nếu trạng
   thái chưa xác minh được, cho thử lại việc đọc trước khi cho gửi lệnh hủy mới. Dùng bản ghi server để
   thông báo một lần giữ hàng, một mốc giao hoặc một lần hoàn tồn theo AD-5 và AD-9.

## Accessibility Floor

- Mọi trường chọn có nhãn hiển thị. Nút, link, bộ lọc và hộp xác nhận dùng được bằng bàn phím, có
  focus nhìn thấy rõ và thứ tự Tab trùng thứ tự đọc.
- Hộp xác nhận có tên mô tả, giữ focus bên trong khi mở, hỗ trợ Escape và trả focus về nút đã mở
  khi đóng. Với thao tác hủy, đặt focus ban đầu ở lựa chọn quay lại.
- Vùng đọc trạng thái dùng `role="status"` cho phản hồi đang xử lý hoặc thành công. Thông báo lỗi cần
  xử lý ngay dùng `role="alert"`. Timeline dùng `ol` và `aria-current="step"` cho đúng mốc hiện tại.
- Nhãn chữ biểu thị trạng thái, lỗi và trạng thái vô hiệu. Giữ chữ dễ đọc trên nền tương ứng, vùng
  chạm chính tối thiểu {spacing.tap-target}. Ảnh hoặc icon minh họa có tên truy cập khi mang thông tin.
- Ở 390 px và khi phóng to, kiểm tra mã đơn dài, tên sản phẩm dài, nút có nhãn hai dòng và hộp
  xác nhận. Nội dung xuống dòng, vùng chính cuộn dọc và nút không che thông báo hoặc focus.

## Responsive & Platform

Ở 390 px, hàng đợi và timeline xếp dọc. Các thao tác có nhãn dài dùng hết bề ngang khả dụng. Trên
màn rộng, có thể đặt tiến độ cạnh chi tiết sản phẩm nhưng giữ cùng trình tự đọc. Kiểm tra bố cục trong
[bản thử HTML hiện có](../../briefs/brief-shoplite-2026-11-21/shoplite-demo.html). Phản hồi quyền,
trạng thái và thanh toán trong tài liệu này là căn cứ khi dựng màn hình.

## Key Flows

### UJ-1 · Lan xử lý một đơn đầu ca

1. Lan mở **Hàng đợi đơn**, chọn xem các đơn Chờ xác nhận và vào một đơn. Cô đối chiếu các dòng,
   số lượng, phí giao, tổng tiền và tình trạng thanh toán.
2. Lan chọn **Tiếp nhận và giữ hàng**. Khi lưu thành công, màn hình hiển thị Đang xử lý và giữ hàng
   cho tất cả dòng. Nếu món cuối đã bị giữ cho đơn khác, lỗi nêu dòng thiếu, đơn ở Chờ xác nhận và
   Lan có thể liên hệ khách hoặc hủy.
3. Lan chọn shipper và ghi nhận Đóng gói hoàn tất. Nếu là chuyển khoản, cô đối chiếu và xác nhận
   tiền trước khi chọn **Xác nhận bàn giao**. Mốc chính của ca là đơn sang Đã gửi đơn với shipper
   đúng, sau khi server lưu thành công. FR-1–FR-5, FR-8.

### UJ-2 · Minh giao đơn được phân công

1. Minh mở **Đơn được giao** và chọn một đơn. Trước bàn giao, anh thấy trạng thái công việc. Khi
   cửa hàng xác nhận bàn giao, anh thấy thông tin cần để giao và có thao tác ghi kết quả.
2. Sau khi khách nhận hàng, Minh chọn **Xác nhận đã giao**, kèm xác nhận đã thu nếu là COD. Màn hình
   hiển thị Đã giao sau khi lưu. Nếu giao thất bại, Minh ghi nhận và thấy lịch dự kiến ngày hôm sau,
   đơn vẫn ở Đã gửi đơn.
3. Nếu Lan đổi người giao giữa lúc Minh đang xem và gửi thao tác, server từ chối thao tác cũ.
   Màn hình bỏ chi tiết khỏi phiên của Minh. Người mới thấy **Chờ bàn giao lại** và có thể ghi
   kết quả sau khi Lan xác nhận đã giao kiện cho mình. FR-5–FR-7, FR-10.

### UJ-3 · Hà kiểm tra tiến độ đơn

1. Hà mở **Đơn của tôi** hoặc đi tiếp từ xác nhận đặt hàng, rồi vào chi tiết của chính mình.
2. Hà đọc năm mốc và nhận ra ngay mốc hiện tại khi tải lại. Với đơn thiếu lịch sử, các mốc chưa rõ
   thời điểm để trống giờ. Nếu đơn chuyển khoản chưa xác minh và cửa hàng vừa tiếp nhận hoặc đóng gói,
   Hà vẫn thấy hướng dẫn, tổng lúc đặt và lời nhắc chờ xác nhận nếu đã chuyển khoản.
3. Nếu đơn còn Chờ xác nhận, Hà có thể xác nhận tự hủy và thấy Đã hủy. Nếu nhân viên đã tiếp nhận
   trong lúc Hà mở hộp xác nhận, màn hình tải lại mốc hiện tại. Nếu mất phản hồi hủy, Hà thấy trạng
   thái đã lưu hoặc nút **Tải lại đơn**. FR-2, FR-3, FR-9, FR-10.

## Phạm vi sử dụng

Các luồng trên áp dụng cho ca lab với tài khoản cấp sẵn, đơn mới và tồn mẫu. Việc khôi phục quyền trên
đơn cũ, đối soát tồn thật và xử lý hoàn tiền ngoài ShopLite theo phạm vi đã ghi trong PRD.
