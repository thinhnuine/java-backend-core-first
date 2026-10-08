# Bài Tập Nhỏ: Từ Biết Đọc Đến Tự Viết

Làm lần lượt sau hai bài [generics](01-generics.md) và [enum](02-enum-and-nested-class.md). Không có yêu cầu repository hay LRU ở lượt đầu. Mỗi bài dành khoảng 20–40 phút; nếu vướng hơn 15 phút, ghi input, điều dự đoán và lỗi thực tế trước khi hỏi mentor.

## Bài 1 — Dùng kiểu có sẵn

Tạo List<String>, thêm hai title, in title đầu tiên. Thử thêm một số nguyên, ghi lỗi compiler rồi khôi phục.

Tự trả lời: vì sao compiler biết `get(0)` trả String? Danh sách rỗng mà gọi get(0) thì sao? Generics có bảo đảm danh sách không rỗng không?

## Bài 2 — Tự dùng Box

Không nhìn code mẫu, viết lại Box<T> với constructor và get. Tạo class TaskTitle có field text và accessor, rồi đặt một TaskTitle vào Box.

Kết quả cần quan sát: `box.get().text()` in đúng title bạn truyền; gán `box.get()` vào Integer bị compiler từ chối. Không cần setter hoặc generic method.

## Bài 3 — Trạng thái task

Dùng enum gồm TODO, IN_PROGRESS, DONE. Viết method label bằng if/else, với bảng kết quả:

| Input | Output |
| --- | --- |
| TODO | Cần làm |
| IN_PROGRESS | Đang làm |
| DONE | Hoàn thành |
| null | IllegalArgumentException với thông báo rõ |

Bài này chỉ ánh xạ nhãn, không cần cài state machine hoặc cấm chuyển từ DONE về TODO.

## Bài 4 — Ghép hai điều đã học

Tạo class Task tối giản với title và TaskStatus, constructor và accessor; chưa cần ID, DB hoặc HTTP. Tạo List<Task> chứa hai task, dùng vòng for in title và label status của mỗi task.

Nếu đã học JUnit, viết test cho label với ba status và null. Nếu chưa biết cách chạy test, làm [unit test đầu tiên](../../learning-path/04-first-unit-test.md) trước. Không cần Mockito.

## Tự kể lại sau khi làm

Giải thích trong 3–5 câu: generics kiểm tra điều gì, enum giới hạn điều gì, và vì sao chúng không tự kiểm tra mọi quy tắc dữ liệu như title không rỗng.

Tiếp theo: [checklist cơ bản](04-checklist-and-ai-review.md). [Bài repository/LRU](07-optional-exercises.md) để sau khi đã học collections và Optional.
