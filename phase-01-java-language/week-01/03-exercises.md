# Bài Tập Tuần 1

> Theo [lộ trình mới](../../ROADMAP.md), đây là ngân hàng bài tập/checklist cho tuần 2–4. Ưu tiên Task Manager và test title trước; Money/Notification là bài thêm, ShoppingCart và abstract class có thể học sau. Không cần hoàn thành tất cả để qua tuần 1 mới.

## Bài 1: Money Value Object

Tạo class `Money`.

Yêu cầu:

- Immutable.
- Field:
  - `amount`: `long`.
  - `currency`: `String`.
- Constructor validate:
  - amount không âm.
  - currency không null, không blank.
- Method:
  - `add(Money other)`.
  - `subtract(Money other)`.
  - `multiply(int factor)`.
- Chỉ cộng/trừ được cùng currency.
- Override `equals`, `hashCode`, `toString`.

Test cần có:

- Tạo money hợp lệ.
- Không cho amount âm.
- Không cho currency blank.
- Cộng cùng currency thành công.
- Cộng khác currency thất bại.
- Hai object cùng amount/currency thì equals true.

## Bài 2: Notification Sender

Thiết kế notification module nhỏ.

Yêu cầu:

- Interface `NotificationSender`.
- Implementation:
  - `EmailNotificationSender`.
  - `SmsNotificationSender`.
- Class `NotificationService` nhận `NotificationSender` qua constructor.
- Không dùng inheritance nếu không cần.

Câu hỏi tự trả lời:

- Vì sao `NotificationService` phụ thuộc interface thay vì concrete class?
- Nếu thêm Slack sender thì sửa file nào?
- Test `NotificationService` thế nào nếu không muốn gửi email thật?

## Bài 3: Shopping Cart

Thiết kế giỏ hàng đơn giản.

Yêu cầu:

- `ProductId` là value object immutable.
- `CartItem` chứa product id, name, quantity, unit price.
- `ShoppingCart` có danh sách item.
- Không expose mutable list trực tiếp.
- Có method:
  - `addItem`.
  - `removeItem`.
  - `total`.

Điểm cần chú ý:

- Nếu dùng `List`, khi trả list ra ngoài phải tránh bị caller sửa internal state.
- Nếu `ShoppingCart` immutable, mỗi method trả cart mới.
- Nếu `ShoppingCart` mutable, API phải rõ ràng và test side effect.

## Learning Log Cuối Tuần

Viết ngắn vào file note riêng của bạn:

- Java type system khác TypeScript ở đâu?
- Bạn đã dùng interface ở bài nào và vì sao?
- Bài nào nên dùng composition?
- Bug khó nhất tuần này là gì?
- Có đoạn code nào AI gợi ý nhưng bạn không dùng không? Vì sao?

## Quy ước Money cho lab

amount dùng đơn vị nhỏ nhất đã thống nhất (USD: cent, VND: đồng), không phải số dollar có phần thập phân. multiply không nhận factor âm; subtract không cho số dư âm. Dùng Math.addExact/subtractExact/multiplyExact để phát hiện overflow, rồi kiểm điều kiện nghiệp vụ. Thêm test null, khác currency, factor âm, kết quả trừ âm và giá trị biên long khi chọn bài tự luyện này.
