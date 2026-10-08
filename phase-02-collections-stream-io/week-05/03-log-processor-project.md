# Project Tuần 5: Log Processor

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Mục Tiêu

Xử lý file log lớn bằng hai cách: vòng lặp và Stream. Sau đó so sánh rõ ràng thay vì kết luận cảm tính.

## Input Mẫu

```text
2026-10-05T10:00:01Z INFO user=1 action=login latencyMs=32
2026-10-05T10:00:03Z ERROR user=2 action=checkout latencyMs=180
2026-10-05T10:00:04Z INFO user=1 action=view_product latencyMs=45
```

## Yêu Cầu

Tạo domain:

- `LogLine`.
- `LogSummary`.
- `LogLineParser`.
- `LoopLogProcessor`.
- `StreamLogProcessor`.

Processor cần tính:

- Số dòng theo level.
- Top 5 action chậm nhất theo latency.
- Latency trung bình theo action.
- Tổng số dòng parse thành công.
- Tổng số dòng parse lỗi.

## Error Handling

Bạn tự chọn:

- Bỏ qua dòng lỗi và count vào parse error.
- Hoặc fail-fast khi gặp dòng lỗi.

Ghi rõ tradeoff trong report.

## Report

Tạo `log-processor-report.md`:

- Cách chạy.
- File test size bao nhiêu.
- Loop version dễ đọc ở điểm nào.
- Stream version dễ đọc ở điểm nào.
- Bản nào dễ debug hơn.
- Bản nào dùng memory tốt hơn.
- Kết luận: lần sau bạn chọn cách nào cho case tương tự.

## Test Gợi Ý

- Parse line hợp lệ.
- Parse line thiếu field.
- Count level đúng.
- Average latency đúng.
- Top slow action đúng.
- File rỗng.
- File có dòng lỗi.

## Stretch

- Generate file log 1M dòng.
- Thêm CLI args: input file path, output report path.
- Dùng `Files.newBufferedReader` và đo memory.
