---
name: 'ShopLite · Vận hành đơn hàng'
description: 'Quy cách giao diện cho nhân viên, shipper và khách trên web ShopLite.'
status: final
updated: 2026-11-21
sources:
  - '../../prds/prd-shoplite-2026-11-21/prd.md'
  - '../../../../src/app/layout.tsx'
  - '../../../../src/app/globals.css'
  - '../../../../src/components/ui/button.tsx'
  - '../../../../src/features/orders/components/order-status-badge.tsx'
colors:
  page: '#f8fafc'
  surface: '#ffffff'
  border: '#e2e8f0'
  control-border: '#cad5e2'
  text: '#0f172b'
  muted: '#45556c'
  primary: '#4f39f6'
  primary-text: '#ffffff'
  primary-tint: '#eef2ff'
  primary-strong: '#432dd7'
  neutral-tint: '#f1f5f9'
  waiting-tint: '#fffbeb'
  waiting-text: '#973c00'
  processing-tint: '#f0f9ff'
  processing-text: '#0069a8'
  packed-tint: '#f5f3ff'
  packed-text: '#7008e7'
  success-tint: '#ecfdf5'
  success-text: '#007a55'
  danger: '#ec003f'
  danger-text: '#a50036'
  danger-tint: '#fff1f2'
typography:
  title:
    fontFamily: 'Be Vietnam Pro, ui-sans-serif, system-ui, sans-serif'
    fontSize: '24px'
    fontWeight: '700'
    lineHeight: '1.35'
  body:
    fontFamily: 'Be Vietnam Pro, ui-sans-serif, system-ui, sans-serif'
    fontSize: '16px'
    fontWeight: '400'
    lineHeight: '1.5'
  label:
    fontSize: '14px'
    fontWeight: '600'
    lineHeight: '1.45'
  badge:
    fontSize: '12px'
    fontWeight: '600'
    lineHeight: '1.5'
rounded:
  control: '12px'
  card: '16px'
  pill: '9999px'
spacing:
  small-gap: '8px'
  row-gap: '12px'
  gutter: '16px'
  section-gap: '24px'
  tap-target: '48px'
components:
  button-primary:
    background: '{colors.primary}'
    foreground: '{colors.primary-text}'
    radius: '{rounded.control}'
    minHeight: '{spacing.tap-target}'
  button-danger:
    background: '{colors.danger}'
    foreground: '{colors.primary-text}'
    radius: '{rounded.control}'
    minHeight: '{spacing.tap-target}'
  order-card:
    background: '{colors.surface}'
    foreground: '{colors.text}'
    borderColor: '{colors.border}'
    radius: '{rounded.card}'
    padding: '{spacing.gutter}'
  status-badge:
    radius: '{rounded.pill}'
    fontSize: '{typography.badge.fontSize}'
    fontWeight: '{typography.badge.fontWeight}'
  timeline-current:
    background: '{colors.primary-tint}'
    foreground: '{colors.primary-strong}'
    radius: '{rounded.control}'
  notice-danger:
    background: '{colors.danger-tint}'
    foreground: '{colors.danger-text}'
    radius: '{rounded.control}'
---

# ShopLite · Quy cách giao diện vận hành đơn hàng

## Brand & Style

Kế thừa giao diện sáng của ShopLite với nền slate nhạt, thẻ trắng và nút indigo. Mỗi thẻ giúp người
dùng nhận ra mã đơn, trạng thái và hành động tiếp theo. Tên sản phẩm, số lượng và tổng tiền được đặt
cạnh nhau để nhân viên đối chiếu khi đóng gói. [EXPERIENCE.md](./EXPERIENCE.md) mô tả hành vi từng màn hình.

## Colors

Các giá trị hex là bản quy đổi sRGB của màu trong Tailwind CSS 4.3.3 theo lockfile. Triển khai dùng các
class Tailwind bên dưới để giữ cùng palette với ứng dụng.

- Nền trang và thẻ dùng `slate-50` và `white`. Nội dung chính dùng `slate-900`, nội dung phụ dùng
  `slate-600`, đường viền dùng `slate-200` và `slate-300`.
- Nút chính giữ `indigo-600`. Dùng nền `indigo-700` khi rê chuột trên nút tác vụ mới để chữ trắng vẫn
  rõ. Dùng `rose-600` cho hành động hủy và `rose-50` / `rose-800` cho thông báo lỗi.
- Trạng thái có nhãn tiếng Việt. Màu dưới đây hỗ trợ nhận diện nhanh.

| Trạng thái | Nền / chữ | Cách dùng |
| --- | --- | --- |
| Chờ xác nhận | `amber-50` / `amber-800` | Cùng nhóm màu amber của badge hiện có, chọn sắc chữ đậm cho màn vận hành. |
| Đang xử lý | `sky-50` / `sky-700` | Kế thừa màu badge `confirmed`, dùng nhãn theo PRD. |
| Đóng gói hoàn tất | `violet-50` / `violet-700` | Biến thể của `packed`, dùng màu sẵn có trong Tailwind. |
| Đã gửi đơn | `indigo-50` / `indigo-700` | Kế thừa màu badge `shipping`, dùng nhãn theo PRD. |
| Đã giao | `emerald-50` / `emerald-700` | Kế thừa màu badge `delivered`. |
| Đã hủy | `slate-100` / `slate-600` | Nhóm màu slate hiện có, dùng sắc chữ đậm cho màn vận hành. |

## Typography

Dùng Be Vietnam Pro từ [layout gốc](../../../../src/app/layout.tsx) với bộ ký tự tiếng Việt. Tiêu đề
màn hình dùng {typography.title.fontSize}, nội dung thường dùng {typography.body.fontSize}. Mã đơn và
tổng tiền dùng chữ đậm. Giá hiển thị bằng hàm định dạng tiền hiện có và số liệu đã lưu trên đơn.
Nhãn phương thức thanh toán và nhãn tình trạng thanh toán đứng riêng.

## Layout & Spacing

- Ở 390 px, dùng một cột với lề {spacing.gutter}. Trong thẻ, ảnh minh họa đứng cạnh tên và số lượng
  nếu vừa. Tổng tiền, cảnh báo, tiến độ và thao tác xếp dưới. Tên sản phẩm dài được xuống dòng.
- Danh sách đơn dùng thẻ. Trên màn rộng, màn chi tiết có thể đặt phần sản phẩm và tiến độ cạnh nhau
  từ `lg` nếu còn đủ chỗ cho nội dung. Thứ tự đọc vẫn là thông tin đơn, tiến độ rồi thao tác.
- Nút chính ở cuối phần thao tác, rộng hết cột trên điện thoại. Chiều cao vùng bấm chính là
  {spacing.tap-target}. Dùng {spacing.row-gap} giữa thành phần trong thẻ và {spacing.section-gap} giữa
  các nhóm nội dung. Khi phóng to, vùng thao tác cuộn cùng nội dung.

## Elevation & Depth

Dùng `shadow-sm` và `ring-1 ring-slate-200` theo thẻ đơn hàng hiện tại. Dùng đường viền nhẹ để chia
sản phẩm và tổng tiền, cùng nền nhạt để nhấn mốc hiện tại. Hộp xác nhận có lớp phủ tách khỏi màn sau.

## Shapes

Dùng {rounded.card} cho thẻ, {rounded.control} cho nút và trường chọn, {rounded.pill} cho nhãn trạng
thái. Ô ảnh giữ kiểu emoji trên nền gradient của ShopLite.

## Components

| Thành phần | Quy cách |
| --- | --- |
| Thẻ đơn | Mã đơn và badge ở phần đầu. Sản phẩm, số lượng và tổng tiền ở phần nội dung. Vùng mở chi tiết có nhãn rõ. |
| Dòng sản phẩm | Tên, số lượng, đơn giá và ảnh minh họa theo dữ liệu đơn. Phần tiền nằm cuối nhóm thông tin trên màn hẹp. |
| Trạng thái đơn | Dùng hình dạng của `OrderStatusBadge` và palette trên. Nhãn thanh toán hiển thị trong vùng thanh toán của đơn. |
| Thanh toán và ngoại lệ | Đặt phương thức và tình trạng thanh toán ở hai dòng có nhãn. Khối hướng dẫn chuyển khoản của khách nằm trong thẻ thanh toán, cạnh tổng tiền theo đơn. Dùng văn bản cạnh nút bàn giao để giải thích điều kiện còn thiếu. Dùng nhãn chữ cho Chờ bàn giao lại, chờ nhận lại và nhắc đối soát. |
| Timeline | Mỗi mốc có nhãn chữ, dấu hiệu đã qua hoặc hiện tại. Mốc hiện tại dùng {colors.primary-tint}. Thời điểm đặt sau nhãn và chỉ hiện khi có dữ liệu. |
| Nút và trường chọn shipper | Tái sử dụng [Button](../../../../src/components/ui/button.tsx) cho các biến thể `primary`, `secondary`, `ghost`, `danger`. Trường chọn có nhãn hiển thị và chiều cao thao tác ngang nút chính. |
| Thông báo / xác nhận | Dùng {colors.danger-tint} cho lỗi, {colors.success-tint} cho kết quả thành công. Đặt thông báo sát vùng liên quan và dùng nút có nhãn hành động trong hộp xác nhận. |

Tham chiếu bố cục và dữ liệu mẫu ở 390 px trong [bản thử HTML hiện có](../../briefs/brief-shoplite-2026-11-21/shoplite-demo.html).
Khi dựng giao diện, ưu tiên màu, font và quy cách trong tài liệu này.

## Do's and Don'ts

- Luôn kèm nhãn chữ với màu, icon và phản hồi cập nhật. Dùng focus ring hiện rõ cho mọi nút, link và trường chọn.
- Dành nút đậm nhất cho thao tác chính đang khả dụng. Đặt thao tác hủy cách nút chính một khoảng rõ ràng.
- Dùng nội dung thanh toán do server xác nhận. Với dữ liệu chưa xác minh, hiển thị đúng nhãn **Chưa xác minh**.
