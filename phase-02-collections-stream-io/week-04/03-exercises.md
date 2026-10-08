# Bài Tập Tuần 4

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Bài 1: Collection Playground

Tạo chương trình `CollectionPlayground`.

Yêu cầu:

- Tạo 100k phần tử.
- So sánh lookup bằng:
  - `ArrayList.contains`
  - `HashSet.contains`
  - `TreeSet.contains`
- So sánh insert/remove ở đầu/cuối `ArrayList`.
- Ghi nhận kết quả và nhận xét.

Không cần benchmark chuẩn tuyệt đối. Mục tiêu là quan sát xu hướng và giải thích đúng.

## Bài 2: HashMap Key Bug

Tạo class key custom:

```java
public final class UserKey {
    private String email;
}
```

Làm 2 version:

- Version sai: equals/hashCode nhất quán nhưng cùng dựa trên email mutable; có method đổi email. Đây là bug do mutate key, không trộn với bug quên hashCode.
- Version đúng: immutable, `equals/hashCode` đúng.

Yêu cầu:

- Put key vào `HashMap`.
- Mutate key sai sau khi put.
- Ghi hashCode trước/sau đổi email; chọn hai email cho hash khác nhau và quan sát get có thể không tìm lại được key. Nếu kết quả chưa đổi, phân tích bucket/hash thay vì kết luận mutable key an toàn.
- Viết note giải thích.

## Bài 3: Collection Choice Notes

Tạo file `collection-choice-notes.md` trong project luyện tập.

Với mỗi case, chọn collection và giải thích:

- Search user by id.
- Sort task theo due date.
- Remove duplicate email.
- Preserve insertion order.
- Count frequency.
- Cache cần LRU.

## Bài 4: Concurrent Word Counter

Viết word counter:

- Single-thread dùng `HashMap`.
- Multi-thread dùng `ConcurrentHashMap`.
- Dùng `merge` hoặc `compute`.

Test:

- Input nhỏ dễ kiểm.
- Input có word lặp.
- Multi-thread chạy nhiều lần kết quả vẫn đúng.
