# Bài Tập Tuần 1

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Money Value Object

Money là value object: object có ý nghĩa bằng nhau theo dữ liệu tiền.

- Field private final: amount kiểu long, currency kiểu String.
- Constructor chặn số âm và currency null/trắng.
- add, subtract, multiply trả kết quả mới; cộng/trừ phải cùng currency.

**Gợi ý:** Làm add trước; viết một ví dụ đúng, một ví dụ khác currency, một ví dụ âm.

**Đạt khi:** Hai Money cùng amount/currency equals nhau; object cũ không đổi. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 2: Notification Sender

Notification Sender luyện thay cách gửi qua interface (cam kết về method).

- Tạo interface send.
- Hai implementation (cách thực hiện) Email và SMS chỉ in console.
- Service nhận sender qua constructor.

**Gợi ý:** Thay sender ở chỗ tạo service.

**Đạt khi:** Không sửa service khi đổi Email sang SMS. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Shopping Cart

Shopping Cart là giỏ chứa các item (mục hàng), học sau Money.

- Tạo ProductId, CartItem có quantity dương và unitPrice.
- Cart có thêm/xóa item và tính total.
- Chọn rõ cart mutable (có thể đổi) hay immutable (trả object mới).

**Gợi ý:** Làm tổng một item trước rồi hai item; không trộn currency.

**Đạt khi:** Caller không sửa list nội bộ ngoài API được cho phép. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Learning Log Cuối Tuần

Learning log là ghi chú sau buổi học. Viết ba câu: tôi làm được gì, ví dụ nào khiến tôi hiểu sai, lần sau tôi sẽ thử gì. **Gợi ý:** dùng input/output thật, không chép định nghĩa.

## Quy ước Money cho lab

Quy ước lab: amount là số nguyên theo đơn vị tiền nhỏ nhất đã chọn; currency phải khớp khi tính. Giữ cùng quy tắc chuẩn hóa currency trong constructor và phép so sánh. **Tự làm:** ghi quy ước vào README code của bạn. **Gợi ý:** nêu 100 nghĩa là 100 đồng hay 100 xu, không để caller tự đoán.

## Đọc thêm sau

Overflow là kết quả vượt giới hạn long: dùng Math.addExact/subtractExact/multiplyExact khi hoàn thiện và kiểm ArithmeticException. Cần phân biệt overflow với kết quả âm không được phép. Thêm equals/hashCode/toString sau khi hiểu [object contract](02-oop-and-object-contract.md).
