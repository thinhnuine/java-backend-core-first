# Module Week-02: Generics Và Enum Từng Bước

Bạn không cần học hết generics để viết Java. Bài cũ ghép quá nhiều khái niệm mới; lượt học đầu này chỉ tập trung vào ràng buộc kiểu và trạng thái task.

**Lưu ý lịch:** đây là thư mục week-02 cũ, tương ứng phần nền ở **tuần 4 của [roadmap mới](../../ROADMAP.md)**. Nếu bạn đang ở tuần 2 mới, hãy học type system/class trước. Lịch dưới đây là cách chia một module thành bốn buổi, không cộng thêm một tuần bắt buộc vào roadmap.

## Kiểm tra đầu vào

Trước khi học, thử tự viết một class có field String, constructor nhận String và method trả field đó; tạo object trong main rồi chạy bằng terminal. Nếu còn vướng, xem [buổi đầu](../../learning-path/00-first-java-program.md) và [type system](../week-01/01-type-system.md). Không cần biết Spring hoặc database.

## Bốn buổi, mỗi buổi tối đa 2 giờ

| Buổi | Đọc và làm | Đạt khi |
| --- | --- | --- |
| 1 | [Generics](01-generics.md): List<String>, chạy Box<String>/Box<Integer> | Giải thích get trả kiểu gì; tạo được một lỗi compile có chủ đích |
| 2 | Tự gõ lại Box, làm bài 1–2 trong [bài tập](03-exercises.md); bổ sung List/Map từ roadmap | Không cần cast; dùng được List<Task> khi đã có Task |
| 3 | [Enum](02-enum-and-nested-class.md), làm label bằng if/else | Phân biệt String và TaskStatus |
| 4 | Bài 3–4, unit test nếu đã học JUnit, [checklist](04-checklist-and-ai-review.md) | Giải thích bằng code của mình thay vì nhắc lại định nghĩa |

Mỗi buổi chia khoảng 20 phút đọc, 50 phút tự gõ, 30 phút đổi input/tạo lỗi và 20 phút ghi lại điều hiểu được. Nếu chưa hiểu ví dụ, dừng tại đó; không cần đọc tiếp để đủ trang.

## Chưa cần học ở lượt đầu

Wildcard `?`, PECS, bounded type, erasure, generic method, nested/inner/anonymous class, LRU cache và repository tổng quát. Chúng có trong [tra cứu nâng cao](06-advanced-reference.md), [bài tự chọn](07-optional-exercises.md) và [mentor guide cũ](05-deep-dive-study-guide.md).

Tiếp theo: qua checklist cơ bản rồi học [record/exception](../week-03/README.md) khi tới tuần 5 mới.
