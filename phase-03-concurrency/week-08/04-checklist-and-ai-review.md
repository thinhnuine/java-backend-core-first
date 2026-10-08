# Checklist Và AI Review Tuần 8

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Checklist

- [ ] Giải thích được virtual thread phù hợp bài toán nào.
- [ ] Không dùng virtual thread để né shared state.
- [ ] Thread pool có shutdown đúng.
- [ ] Producer-consumer không dùng `if` thay `while` khi wait.
- [ ] Rate limiter có test concurrent.
- [ ] Learning log có ít nhất 3 bug concurrency đã gặp.
- [ ] Biết dùng thread dump ở mức khái niệm để tìm deadlock.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review bài tổng hợp concurrency của tôi.

Tập trung vào:
- race condition
- deadlock
- visibility bug
- shutdown behavior
- wait/notify hoặc Condition có đúng pattern không
- test concurrent có đủ thuyết phục không
- virtual thread được hiểu đúng chưa
- rate limiter algorithm có tradeoff rõ không

Code và learning log:
```
