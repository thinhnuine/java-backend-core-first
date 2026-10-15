# Capstone: Port Một Thư Viện Nhỏ Sang Java

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Mục Tiêu

Capstone là bài tổng hợp; port là viết lại hành vi thư viện từ ngôn ngữ khác. Chỉ chọn một phần nhỏ có input/output rõ sau Task Manager cơ bản.

## Chọn 1 Trong 3 Đề

Chọn **một**: cache LRU (loại dữ liệu ít dùng gần đây), event emitter (đăng ký và gọi bên nghe sự kiện), hoặc parser JSON (đọc văn bản thành dữ liệu). **Tự làm:** viết 3 ví dụ input/output trước code. **Gợi ý:** chọn event emitter đơn giản nếu chưa biết cấu trúc dữ liệu cho cache.

## Cấu Trúc Project Gợi Ý

Trong Maven project: code ở src/main/java, test ở src/test/java. README ghi mục đích, cách chạy, giới hạn và ví dụ sử dụng. **Tự làm:** tạo một public method nhỏ trước. **Gợi ý:** API public là phần bên ngoài được gọi, không cần mở mọi method.

## Checklist Capstone

- [ ] Chỉ làm phạm vi đã chọn.
- [ ] Có input đúng, sai và trường hợp biên.
- [ ] Test thất bại khi cố ý làm sai hành vi.
- [ ] README đủ để chạy lại.

**Tự làm:** nhờ một người đọc README thử chạy. **Gợi ý:** ghi lại bước họ phải hỏi thêm.

## Đọc thêm sau

Parser đầy đủ cần xử lý escape (ký tự thoát), số, lồng nhau và lỗi vị trí. Event emitter cần quy định thứ tự gọi, gỡ đăng ký và lỗi listener. Cache cần xác định thứ tự truy cập và cập nhật khóa cũ. Không ngầm hứa tương thích toàn bộ thư viện gốc.
