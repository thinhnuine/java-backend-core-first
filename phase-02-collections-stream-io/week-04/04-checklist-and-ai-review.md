# Checklist Và AI Review Tuần 4

## Checklist

- [ ] Giải thích được vì sao `ArrayList.get` nhanh.
- [ ] Biết vì sao insert/remove giữa `ArrayList` tốn O(n).
- [ ] Biết vì sao `HashMap` cần `equals/hashCode`.
- [ ] Biết khi nào dùng `TreeMap`.
- [ ] Biết khi nào cần giữ insertion order.
- [ ] Không dùng `ConcurrentHashMap` để che mọi vấn đề concurrency.
- [ ] Có benchmark nhỏ kèm nhận xét, không chỉ số liệu.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review bài Collections tuần 4 của tôi.

Tập trung vào:
- benchmark có công bằng không
- tôi có hiểu đúng độ phức tạp không
- nhận xét có bị overclaim không
- collection được chọn có hợp lý không
- equals/hashCode của key custom có vấn đề không
- ConcurrentHashMap có được dùng đúng atomic operation không

Code, kết quả và ghi chú:
```

## Câu Hỏi Tự Vấn

- Nếu đổi data size từ 1k lên 1M thì collection đang chọn còn hợp không?
- Key trong `HashMap` có immutable không?
- Có cần sorted order thật không, hay chỉ cần sort lúc output?
- Concurrent collection có giải quyết đúng race mình đang gặp không?
