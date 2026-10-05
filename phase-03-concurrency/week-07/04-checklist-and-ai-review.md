# Checklist Và AI Review Tuần 7

## Checklist

- [ ] Executor được shutdown đúng.
- [ ] Không block bừa bãi trong async pipeline.
- [ ] Biết dùng `thenCombine`, `exceptionally`, `handle`.
- [ ] Lock luôn unlock trong `finally`.
- [ ] Dùng đúng `AtomicInteger` cho state đơn giản.
- [ ] Dùng `merge/computeIfAbsent` với `ConcurrentHashMap`.
- [ ] Tạo và sửa được deadlock demo.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review code Executor/CompletableFuture tuần 7 của tôi.

Tập trung vào:
- thread pool có shutdown đúng không
- có nguy cơ deadlock/thread starvation không
- CompletableFuture compose có hợp lý không
- exception async được xử lý chưa
- ConcurrentHashMap update có atomic không
- Lock có unlock trong finally không
- deadlock fix có thật sự giải quyết gốc vấn đề không

Code:
```
