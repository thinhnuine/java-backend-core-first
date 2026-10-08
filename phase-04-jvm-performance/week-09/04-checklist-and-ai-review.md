# Checklist Và AI Review Tuần 9

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Checklist

- [ ] Phân biệt heap, stack, metaspace.
- [ ] Biết object thường nằm ở heap.
- [ ] Biết leak trong Java thường là object còn reachable.
- [ ] Biết vì sao benchmark cần warm-up.
- [ ] Đọc được vài dòng GC log cơ bản.
- [ ] Biết G1 và ZGC giải quyết nhóm vấn đề nào.
- [ ] Có note về ClassNotFound vs NoClassDefFound.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy giải thích GC log và ghi chú JVM của tôi.

Tập trung vào:
- tôi hiểu đúng heap/stack/metaspace chưa
- kết luận benchmark có quá tay không
- GC log cho thấy điều gì
- có dấu hiệu memory pressure không
- giải thích ClassNotFound/NoClassDefFound có đúng không
- nên quan sát thêm metric nào

Log và ghi chú:
```
