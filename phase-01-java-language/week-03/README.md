# Module Week-03: Record Và Exception Từng Bước

Bài này giúp bạn mang dữ liệu bằng record và hiểu luồng chạy khi method gặp lỗi. Không cần làm JSON parser hoặc hiểu sealed hierarchy để hoàn thành lượt đầu.

Đây là mã thư mục cũ, tương ứng **tuần 5 của [roadmap mới](../../ROADMAP.md)**. Tuần 3 mới vẫn ưu tiên composition và unit test. Khi vào module này, cần biết class/constructor/accessor, enum và cách chạy test cơ bản.

## Chia nhỏ thành bốn buổi

| Buổi | Nội dung | Điều phải tự làm được |
| --- | --- | --- |
| 1, tối đa 2h | [Record](01-modern-java.md), phần A | Tạo record, gọi accessor, so equals; nói rõ record không tự validate null |
| 2, tối đa 2h | [Switch](01-modern-java.md), phần B; bài thực hành 1 | Chuyển if/else label thành switch, đọc lỗi thiếu case |
| 3, tối đa 2h | [Exception](02-exception-handling.md), phần A rồi B | Vẽ luồng throw/catch và phân biệt throws |
| 4, tối đa 2h | [Bài thực hành nhỏ](03-task-practice.md), [checklist](04-checklist-and-ai-review.md) | Ghép record/enum/validation và test được input sai |

Trong tuần 5 mới còn có Optional/Stream/time ở module khác: các thời lượng trên là trần để chia bài, không phải quota phải dùng hết. Nếu module này đã chiếm đủ 8h, dời phần còn lại sang buổi kế tiếp; không nén tất cả để chạy theo lịch.

## Mỗi ví dụ học theo một vòng

Đọc yêu cầu → đoán output → gõ và chạy → thay đúng một đầu vào → giải thích chỗ khác biệt. Chỉ sang khái niệm mới khi bạn có thể gọi lại ví dụ mà không nhìn code mẫu.

## Phần để sau

[Sealed type, pattern matching, text block, try-with-resources và custom exception](06-advanced-reference.md) là tham khảo. Try-with-resources sẽ cần khi làm I/O/JDBC; các phần khác chọn theo nhu cầu. [Capstone thư viện](03-capstone-library-port.md) và [mentor guide cũ](05-deep-dive-study-guide.md) không phải bài bắt buộc của lượt đầu.

Tiếp theo: sau checklist, quay về [roadmap](../../ROADMAP.md) để học HTTP và hoàn thiện Task Manager theo đúng chặng.
