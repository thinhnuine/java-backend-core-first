# Project Tuần 5: Log Processor

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Mục Tiêu

Log processor là chương trình đọc thông tin hoạt động và tính thống kê. Bắt đầu 3 dòng; file lớn là mở rộng.

## Input Mẫu

```text
2026-10-15T10:00:00Z INFO action=login latencyMs=20
2026-10-15T10:00:01Z ERROR action=save latencyMs=50
2026-10-15T10:00:02Z INFO action=login latencyMs=30
```

latencyMs là thời gian thao tác tính bằng mili giây. **Tự làm:** thêm một dòng hợp lệ. **Gợi ý:** giữ cùng định dạng.

## Yêu Cầu

Tách đọc dòng và tính kết quả.

- Parse (đọc và chuyển) dòng thành dữ liệu.
- Đếm theo INFO/ERROR.
- Làm loop trước rồi Stream.

**Gợi ý:** Đếm đúng trước khi thêm trung bình thời gian.

**Đạt khi:** Hai cách cho cùng thống kê. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Error Handling

Chọn rõ: gặp dòng sai thì dừng, hoặc bỏ qua và đếm lỗi. **Tự làm:** thêm dòng thiếu action. **Gợi ý:** báo vị trí dòng để tìm lỗi, không nuốt lỗi âm thầm.

## Report

Report là ghi chép kết quả và cách chạy. Ghi input, output, cách xử lý dòng sai và chỗ loop/Stream dễ đọc hơn. **Tự làm:** viết kết luận chỉ dựa trên điều đã đo. **Gợi ý:** chưa đo bộ nhớ thì không khẳng định bản nào ít bộ nhớ hơn.

## Test Gợi Ý

Kiểm file rỗng, dòng đúng, dòng thiếu field và hỗn hợp. **Tự làm:** tự tính tổng ba dòng trước. **Gợi ý:** dùng kết quả tính tay làm expected, không gọi lại cùng implementation.

## Stretch

Stretch là mục mở rộng, chưa cần làm ở lượt đầu; xem phần cuối file.

## Đọc thêm sau

Thêm trung bình theo action, top 5 chậm nhất, tham số dòng lệnh và file lớn khi bản nhỏ đúng. Files.lines cần đóng; không gom toàn bộ nếu muốn xử lý tiết kiệm bộ nhớ. Dữ liệu lớn không tự biến phép đo thành benchmark công bằng.
