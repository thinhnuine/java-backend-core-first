# Project Tuần 12: Refactor Một Bài Cũ

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Chọn Một Project

Ưu tiên TaskService đã có. Chỉ chọn cache, event emitter, log processor hay parser nếu bạn đã làm chúng. **Tự làm:** nêu một khó khăn hiện tại. **Gợi ý:** tránh tạo project mới chỉ để refactor.

## Yêu Cầu

Refactor giữ hành vi trong khi cải thiện tổ chức code.

- Viết kiểm tra trước.
- Sửa một phần nhỏ.
- Chạy lại và đối chiếu.

**Gợi ý:** Không thêm interface/pattern nếu chưa có nhu cầu.

**Đạt khi:** Public API dễ dùng; nếu đổi phải ghi cách chuyển caller. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## `refactoring-notes.md`

Ghi vấn đề → thay đổi → test bảo vệ → giới hạn. **Tự làm:** thêm một ví dụ trước/sau bằng lời. **Gợi ý:** “gọn hơn” chưa đủ; chỉ ra tên hoặc trách nhiệm dễ hiểu hơn.

## Gợi Ý Theo Project

TaskService: tách quy tắc title khỏi phần HTTP. Log processor: tách đọc dòng khỏi tính thống kê. **Tự làm:** chọn một chỗ. **Gợi ý:** không cần áp cùng cách tách cho mọi project.

## Acceptance

- [ ] Test hành vi vẫn qua.
- [ ] Thay đổi có lý do cụ thể.
- [ ] Không thêm khái niệm không cần thiết.

**Tự làm:** giải thích thay đổi trong ba câu. **Gợi ý:** nói về người đọc code tiếp theo.

## Đọc thêm sau

Chỉ mở rộng sau khi bài nhỏ chạy đúng. Ghi một lỗi đã gặp và cách bạn tự tìm nguyên nhân.
