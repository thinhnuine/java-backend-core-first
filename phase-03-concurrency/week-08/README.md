# Module week-08: Virtual Threads Và Coordination

Tên week trong đường dẫn là mã tài liệu, không phải tuần theo lịch mới. Vị trí học: **mở rộng sau dự án** trong [ROADMAP.md](../../ROADMAP.md). Mỗi tuần mới có tổng 8 giờ cho mọi tài liệu được chọn, không phải 8 giờ cho mỗi module.

## Cần biết trước

Executor, lock, bounded queue và shutdown.

## Lượt học bắt buộc hoặc phạm vi được chọn

Hiểu virtual thread giảm chi phí blocking task, không tự tăng DB quota hoặc bảo vệ shared state.

Bắt đầu [bài thứ nhất](01-virtual-threads-structured-concurrency.md), sau đó [bài thứ hai](02-classic-concurrency-bugs.md) theo phần roadmap chỉ định. Đọc một ví dụ → đoán output → tự chạy → đổi input → giải thích bằng lời của mình. Có thể dành thêm buổi khi chưa qua mốc; không đọc hết để chạy theo thời hạn.

## Đọc sau hoặc tự chọn

Structured concurrency chỉ concept tại Java 21; custom thread pool/queue/rate limiter là project tự chọn.

Các file bài tập/checklist/mentor guide bên dưới dùng trong phạm vi được chọn, không phải yêu cầu làm hết:

- [03-capstone-threadpool-producer-consumer-rate-limiter.md](03-capstone-threadpool-producer-consumer-rate-limiter.md)
- [04-checklist-and-ai-review.md](04-checklist-and-ai-review.md)
- [05-deep-dive-study-guide.md](05-deep-dive-study-guide.md)

Tiếp theo: đối chiếu đầu ra tuần trong [roadmap](../../ROADMAP.md) và [chuẩn đầu ra](../../COURSE_OUTCOMES.md).
