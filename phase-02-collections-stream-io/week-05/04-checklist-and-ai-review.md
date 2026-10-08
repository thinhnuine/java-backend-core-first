# Checklist Và AI Review Tuần 5

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Checklist

- [ ] Dùng được `filter/map/reduce/collect`.
- [ ] Biết dùng `groupingBy`.
- [ ] Không gọi `Optional.get()` bừa bãi.
- [ ] Biết chọn `Instant`/`LocalDate`/`ZonedDateTime`.
- [ ] Xử lý file lớn không load toàn bộ khi không cần.
- [ ] Có loop implementation.
- [ ] Có Stream implementation.
- [ ] Có report so sánh Stream vs loop.

## Prompt AI Review

```text
Bạn là mentor Java Backend. Hãy review Log Processor của tôi.

Tập trung vào:
- Stream pipeline có dễ đọc không
- loop version có rõ hơn không
- xử lý file lớn có tốn memory không
- Optional dùng có hợp lý không
- java.time type chọn đúng chưa
- error handling có rõ policy không
- benchmark/report có kết luận quá tay không

Code và report:
```

## Câu Hỏi Tự Vấn

- Stream đang làm code rõ hơn hay chỉ ngắn hơn?
- Có side effect nào ẩn trong stream pipeline không?
- Nếu file 5GB thì code hiện tại còn chạy được không?
- Timestamp dùng timezone đúng chưa?
- Error policy có phù hợp backend production không?
