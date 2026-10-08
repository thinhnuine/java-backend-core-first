# Checklist Và AI Review Tuần 1

> Theo [lộ trình mới](../../ROADMAP.md), đây là ngân hàng bài tập/checklist cho tuần 2–4. Ưu tiên Task Manager và test title trước; Money/Notification là bài thêm, ShoppingCart và abstract class có thể học sau. Không cần hoàn thành tất cả để qua tuần 1 mới.

## Checklist

- [ ] Phân biệt được primitive và reference type.
- [ ] Biết khi nào `==` đúng, khi nào phải dùng `equals`.
- [ ] Viết được class immutable đơn giản.
- [ ] Dùng được interface để tách contract khỏi implementation.
- [ ] Biết khi nào abstract class hợp lý.
- [ ] Override được `equals`, `hashCode`, `toString`.
- [ ] Biết vì sao composition thường an toàn hơn inheritance.
- [ ] Có ít nhất 1 bài tập có unit test.

## Prompt AI Review

Dùng prompt này sau khi tự code:

```text
Bạn là mentor Java Backend. Hãy review code sau cho một FE dev đang học Java core.

Tập trung vào:
- type system và null handling
- access modifier
- interface vs abstract class
- composition vs inheritance
- equals/hashCode/toString
- immutability
- test case còn thiếu

Không viết lại toàn bộ code. Hãy chỉ ra vấn đề theo mức độ nghiêm trọng, giải thích vì sao, rồi gợi ý hướng sửa.

Code:
```

## Câu Hỏi Tự Vấn

- Public API của class này có nhỏ không?
- Constructor có bảo vệ invariant không?
- Có field mutable nào bị expose ra ngoài không?
- Nếu object vào `HashSet`, behavior có đúng không?
- Nếu người khác đọc `toString`, có đủ thông tin debug không?
