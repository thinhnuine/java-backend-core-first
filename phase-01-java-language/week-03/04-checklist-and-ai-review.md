# Checklist Và AI Review Tuần 3

## Checklist

- [ ] Dùng được record cho data carrier immutable.
- [ ] Biết record tự sinh method gì.
- [ ] Hiểu sealed class/interface dùng để giới hạn hierarchy.
- [ ] Dùng được pattern matching cho `instanceof`.
- [ ] Dùng được switch expression với enum hoặc sealed type.
- [ ] Biết khi nào dùng text block.
- [ ] Phân biệt checked và unchecked exception.
- [ ] Dùng được try-with-resources.
- [ ] Có bản capstone chạy được.
- [ ] Có README và test cho capstone.

## Prompt AI Review Capstone

```text
Bạn là mentor Java Backend. Hãy review capstone Java core của tôi.

Bối cảnh:
- Tôi là FE dev học Java.
- Đây là bài cuối Giai đoạn 1: Java language.
- Tôi muốn review sâu về API design và cách viết Java đúng tinh thần Java.

Hãy tập trung vào:
- public API có rõ và nhỏ không
- type system/generics dùng có hợp lý không
- record/sealed class/enum có dùng đúng chỗ không
- exception checked/unchecked có hợp lý không
- object có immutable đúng không
- equals/hashCode/toString có cần không
- test case còn thiếu
- code smell và refactor nhỏ

Không viết lại toàn bộ project. Hãy đưa findings theo mức độ nghiêm trọng và gợi ý từng bước sửa.

Code:
```

## Câu Hỏi Tự Vấn Cuối Giai Đoạn

- Nếu đưa thư viện này cho người khác dùng, họ có hiểu public API trong 5 phút không?
- Class nào đang làm quá nhiều việc?
- Có chỗ nào dùng inheritance trong khi composition tốt hơn không?
- Exception message có đủ giúp debug không?
- Test đang kiểm tra behavior hay kiểm tra implementation?
- Nếu rewrite lại từ đầu, bạn sẽ giữ quyết định thiết kế nào?
- Bạn vẫn còn mơ hồ nhất ở concept nào: generics, exception, OOP, record/sealed, hay immutability?
