# Checklist Và AI Review Tuần 6

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Checklist

- [ ] Phân biệt `start()` và `run()`.
- [ ] Giải thích được vì sao `count++` không atomic.
- [ ] Dùng được `synchronized`.
- [ ] Biết lock object nào đang được dùng.
- [ ] Biết `volatile` giải quyết visibility nhưng không thay lock.
- [ ] Có ví dụ race condition tự chạy được.
- [ ] `wait` được gọi trong `while`.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review bài concurrency tuần 6.

Tập trung vào:
- race condition còn tồn tại không
- synchronized có lock đúng object không
- volatile có bị dùng sai cho atomicity không
- wait/notify có dùng trong while loop không
- giải thích happens-before của tôi có đúng không
- test/demo có đủ để quan sát bug không

Code và learning log:
```
