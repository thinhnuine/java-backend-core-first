# Bài Tập Tuần 4

**Lượt đầu chỉ làm bài 1 và phần cơ bản bài 3.** Dùng JShell 21 như bài 00, tự gõ, đoán rồi chạy. Không có đáp án mẫu.

## Bài 1: Collection Playground

Dùng ba title, trong đó hai title trùng:

1. Thêm vào ArrayList, in size và phần tử đầu.
2. Tạo HashSet từ list, in size.
3. Tạo HashMap dùng ID String; đọc, cập nhật và xóa một ID.
4. Thử đọc ID không có, giải thích output.

Chưa cần 100k phần tử hay đo thời gian.

## Bài 2: HashMap Key Bug

**Để sau khi quen class và equality.** Bài này tìm hiểu vì sao key đổi trạng thái có thể không tra lại được; lượt đầu dùng String làm key.

## Bài 3: Collection Choice Notes

Phần cơ bản: ghi cấu trúc bạn chọn và một câu giải thích cho danh sách title, loại email trùng, tìm title theo ID. Chỉ cần ba dòng ghi chú.

## Bài 4: Concurrent Word Counter

**Để sau khi học thread.** Đây là bài nhiều luồng cùng đếm từ, không phải điều kiện để hoàn thành phần làm quen Java.

## Đọc thêm sau

- Bài 1 mở rộng: 100k phần tử, so contains của ArrayList/HashSet/TreeSet và thêm/xóa hai đầu ArrayList; ghi cách đo, xu hướng, không kết luận từ một lần đo.
- Bài 2 mở rộng: tự viết hai UserKey có equals/hashCode nhất quán theo email, một mutable và một immutable (trạng thái không đổi); put, đổi email, ghi hash trước/sau và get.
- Bài 2: chọn email có hash khác nhau; nếu vẫn get được, phân tích hash/bucket (vùng lưu theo hash), không kết luận key mutable an toàn; tách khỏi lỗi quên hashCode.
- Bài 3 mở rộng: collection-choice-notes.md cho due date, insertion order (thứ tự thêm), đếm tần suất và LRU cache (loại mục ít truy cập gần đây nhất).
- Bài 4 mở rộng: input nhiều dòng, bản một thread dùng HashMap, nhiều thread dùng ConcurrentHashMap.merge/compute; kiểm input nhỏ/từ lặp, so độ dễ đọc và tính đúng.
- Merge/compute là cập nhật qua hàm; phải hiểu thao tác atomic (như một bước không chia nhỏ); chạy đúng nhiều lần không chứng minh thread safety (tính đúng khi chạy đồng thời).
- Benchmark sâu, log processor, pool/queue/rate limiter, GC/reflection/proxy: không cần ở lượt đầu.
