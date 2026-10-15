# Bài Tập Nhỏ: Từ Biết Đọc Đến Tự Viết

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1 — Dùng kiểu có sẵn

Dùng generics có sẵn: danh sách chỉ chứa kiểu bạn chọn.

- Tạo `List<String>` với ba title.
- Đọc từng title và in độ dài.

**Gợi ý:** Dùng ArrayList nếu cần thêm/xóa; dùng for trước.

**Đạt khi:** Thử thêm số bị compiler từ chối. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 2 — Tự dùng Box

Box giữ quan hệ kiểu lúc đưa vào và lấy ra.

- Tạo `Box<String>`.
- Tạo `Box<Integer>`.
- Gọi get và dùng đúng kiểu kết quả.

**Gợi ý:** T là tên đại diện, không phải any.

**Đạt khi:** Không cần cast (ép kiểu) kết quả get. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3 — Trạng thái task

Enum giới hạn lựa chọn trạng thái.

- Tạo TODO, IN_PROGRESS, DONE.
- Viết label cho từng trạng thái.
- Tự quy định cách xử lý null.

**Gợi ý:** Dùng if/else nếu chưa học switch.

**Đạt khi:** Giải thích được vì sao không truyền String thay TaskStatus. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 4 — Ghép hai điều đã học

Ghép danh sách và enum sau khi hiểu từng phần.

- Tạo `List<TaskStatus>`.
- Duyệt và đếm DONE.

**Gợi ý:** So enum bằng ==; không cần Stream.

**Đạt khi:** Số đếm đúng với danh sách rỗng và có trạng thái lặp. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Tự kể lại sau khi làm

Tự trả lời: T ở `Box<String>` là gì? Enum chặn kiểu lỗi nào? **Gợi ý:** tạo một dòng cố ý sai và giải thích lỗi compiler, rồi bỏ dòng đó.

## Đọc thêm sau

Chỉ mở rộng sau khi bài nhỏ chạy đúng. Ghi một lỗi đã gặp và cách bạn tự tìm nguyên nhân.
