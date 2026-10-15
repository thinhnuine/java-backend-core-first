# Bài Tập Tự Chọn Sau Collections

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Generic In-Memory Repository

Repository tổng quát là kho dùng được với nhiều kiểu id và dữ liệu.

- Thiết kế save/findById/deleteById/findAll/existsById.
- Lưu bằng HashMap<ID,T>.
- Từ chối id/value null.

**Gợi ý:** Optional<T> biểu thị không tìm thấy; copy danh sách khi trả ra.

**Đạt khi:** Lưu-đọc-xóa đúng, caller không sửa cấu trúc kho qua findAll. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 2: LRU Cache

LRU cache là bộ lưu tạm loại mục ít được dùng gần đây khi đầy.

- Chọn capacity (số mục tối đa) dương.
- Viết put/get.
- Chốt get và cập nhật khóa cũ có đổi thứ tự dùng không.

**Gợi ý:** LinkedHashMap có access order, phải chọn chế độ đó rõ ràng.

**Đạt khi:** Kiểm một chuỗi đọc/ghi khiến mục cần loại khác với mục thêm đầu tiên. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Workflow Status

Workflow là các bước trạng thái được phép chuyển.

- Vẽ TODO → IN_PROGRESS → DONE.
- Thêm CANCELLED theo quy tắc bạn chọn.
- Viết canMoveTo.

**Gợi ý:** Đây là bài mở rộng; không áp nhầm quy tắc mới lên Task Manager cơ bản.

**Đạt khi:** Có ví dụ được chuyển và bị từ chối. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Learning Log Cuối Tuần

Ghi một giới hạn của generic repository và một quyết định về cache. **Gợi ý:** dữ liệu trả ra có dùng chung reference hay là bản sao?

## Đọc thêm sau

Wildcard/PECS và tự viết danh sách liên kết cho cache chỉ học khi cần. Không cần hoàn thành ba bài này trước Spring.
