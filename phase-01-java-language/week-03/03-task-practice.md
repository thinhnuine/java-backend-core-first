# Thực Hành Nhỏ: Tóm Tắt Task Và Báo Input Sai

Dành một buổi sau [record/switch](01-modern-java.md) và [exception](02-exception-handling.md). Bài chỉ chạy local, chưa có Spring, repository hoặc database. Làm trong thư mục riêng để không trùng class với ví dụ đã gõ.

## Bước 1 — Mang dữ liệu

Tạo enum TaskStatus với ba hằng TODO, IN_PROGRESS, DONE. Tạo record TaskSummary gồm `long id`, `String title`, `TaskStatus status`.

Tạo một object và in từng accessor. Đoán `equals` giữa hai record cùng giá trị trước khi chạy. Chưa thêm validation vào record ở bước này.

## Bước 2 — Viết method tạo summary

Trong một class thường, viết method với chữ ký:

```java
// Chỉ là chữ ký để bạn tự cài phần thân.
static TaskSummary createSummary(long id, String title, TaskStatus status)
```

Quy tắc theo thứ tự:

1. id phải dương.
2. title không null/blank; trim rồi kiểm chiều dài từ 1 tới 100.
3. status không null.
4. Hợp lệ thì trả record mới với title đã trim.
5. Sai thì ném IllegalArgumentException có message chỉ ra field liên quan.

Ở bài này validation đặt trong factory method để luyện luồng lỗi; gọi constructor record trực tiếp vẫn có thể bỏ qua quy tắc. Đây là giới hạn cần tự giải thích. Khi học constructor validation của record, có thể chuyển quy tắc vào đó để mọi đường tạo object đều được kiểm tra.

## Bước 3 — Chạy và quan sát lỗi

Viết main gọi createSummary với dữ liệu hợp lệ và sai. Dùng try/catch ở caller để in lỗi; không catch rồi trả null bên trong createSummary. Viết label bằng switch cho ba status, dùng nó khi in summary hợp lệ.

## Bước 4 — Tự viết test

| Case | Kỳ vọng |
| --- | --- |
| id=1, title=" Học Java ", status=TODO | Record có title="Học Java" |
| id=0 | IllegalArgumentException, message liên quan id |
| title=null | IllegalArgumentException, message liên quan title |
| title chỉ có dấu cách | IllegalArgumentException |
| title sau trim dài 100 ký tự | Thành công |
| title sau trim dài 101 ký tự | IllegalArgumentException |
| status=null | IllegalArgumentException, message liên quan status |

Dùng assertEquals cho kết quả, assertThrows cho lỗi; xem [bài chạy JUnit](../../learning-path/04-first-unit-test.md) nếu quên setup. Không cần so sánh chính xác toàn bộ message nếu điều đó không phải API contract.

## Đạt khi

Tự sửa thêm một điều kiện validation và test được nó; giải thích vì sao record không tự chặn input sai và caller chạy dòng nào sau khi có exception. Không yêu cầu generic result type, sealed class hoặc parser.

Tiếp theo: [checklist](04-checklist-and-ai-review.md), rồi [Task Manager](../../learning-path/03-task-manager-project.md) theo roadmap.
