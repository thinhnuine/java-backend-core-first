# Capstone Concurrency: Thread Pool, Producer-Consumer, Rate Limiter

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## Bài 1: Simple Thread Pool

Yêu cầu:

- Constructor nhận số worker.
- Method `submit(Runnable task)`.
- Worker lấy task từ queue.
- Có `shutdown`.
- Không nhận task mới sau shutdown.
- Không làm mất task đã submit trước shutdown.

Test:

- Submit task chạy thành công.
- Nhiều task được chạy.
- Shutdown xong reject task mới.
- Task throw exception không làm chết toàn bộ pool.

## Bài 2: Producer-Consumer

Yêu cầu:

- Bounded queue.
- Producer block khi queue đầy.
- Consumer block khi queue rỗng.
- Dùng `wait/notifyAll` hoặc `Condition`.

Test:

- `take` chờ khi queue rỗng.
- `put` chờ khi queue đầy.
- Nhiều producer/consumer không mất item.

## Bài 3: Rate Limiter

Yêu cầu:

- Cho phép tối đa N request trong một window thời gian.
- Có test đơn luồng.
- Có test nhiều thread.
- Ghi rõ tradeoff thuật toán.

Chọn một thuật toán:

- Fixed window: dễ viết, biên window có thể burst.
- Sliding window: chính xác hơn, phức tạp hơn.
- Token bucket: linh hoạt, hợp rate ổn định có burst nhỏ.

## Deliverable

Tạo `concurrency-capstone-notes.md`:

- API public của từng component.
- Bug concurrency bạn gặp.
- Cách test.
- Tradeoff shutdown/blocking.
- Phần còn chưa chắc.
