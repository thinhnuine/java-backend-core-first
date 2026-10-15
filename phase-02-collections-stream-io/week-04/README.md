# Module week-04: Làm Quen List, Set, Map

Mục tiêu hiện tại: **đọc hiểu và tự viết được vài thao tác Java cơ bản**. Chưa cần phân tích hiệu năng hay cách collection hoạt động bên trong. Tên week-04 là mã module, không phải hạn phải học xong.

## Cần biết trước

Biến, String, cách gọi method và vòng for. `List<String>` nghĩa là danh sách chứa String; chưa cần tự viết generics (code dùng tham số kiểu).

## Lượt học bắt buộc hoặc phạm vi được chọn

Chỉ đọc [00: List, Set, Map](00-collections-first.md), rồi làm bài 1 và phần cơ bản của bài 3 trong [03: bài tập](03-exercises.md). Dùng [04: checklist](04-checklist-and-ai-review.md) để tự kiểm; [05: ôn tập](05-deep-dive-study-guide.md) nếu cần.

Mỗi lượt: gõ một ví dụ → đoán output → chạy → đổi một giá trị. Đọc thêm dòng nào chưa hiểu thì dừng ở dòng đó.

**Đủ để đi tiếp:** lưu ba title, loại trùng, tìm/xóa theo ID và hiểu kết quả khi ID không có. Chưa cần class Task, repository hay tự viết equals/hashCode.

## Đọc sau hoặc tự chọn

[01: các implementation](01-list-set-map-internals.md) dùng để tra cứu, không phải bài phải đọc hết. [02: ordering, Big-O, concurrency](02-ordering-complexity-concurrent-map.md) để sau khi đã quen Java. Các bài nâng cao trong 03 cũng để sau.

Nếu theo [roadmap](../../ROADMAP.md), 8 giờ/tuần là tổng thời gian, không phải cho từng file; có thể thêm buổi khi cần.

## Đọc thêm sau

- Internals, benchmark (đo hiệu năng), custom key và nhiều thread: chưa thuộc mục tiêu làm quen.
- LRU cache và TreeMap/TreeSet: chỉ đọc khi có nhu cầu cụ thể.
- [Chuẩn đầu ra](../../COURSE_OUTCOMES.md): dùng cho lộ trình dài hạn, không phải điều kiện phải đạt ngay ở lượt đầu.
- Thay đổi lần này: 00 tập trung thao tác; 01–02 để tra cứu; 03–04 bỏ yêu cầu nâng cao khỏi lượt đầu; 05 ôn lại một việc nhỏ.
