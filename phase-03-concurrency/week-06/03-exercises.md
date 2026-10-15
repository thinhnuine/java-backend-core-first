# Bài Tập Tuần 6

Đây là đề để tự làm, không phải đáp án. Chọn từng mục theo README module; không cần làm hết trong một buổi. Tự gõ code, dùng AI để xin gợi ý sau khi đã thử.

## Bài 1: Counter Race

Counter race là lỗi nhiều luồng làm mất lần tăng.

- Làm counter thường và synchronized.
- Hai thread cùng tăng; join cả hai.
- Ghi số dự kiến và thực tế.

**Gợi ý:** Vẽ lịch xen kẽ để giải thích bản thường, không chờ lỗi tự xuất hiện.

**Đạt khi:** Nêu được vùng cần khóa và vì sao volatile không đủ. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 2: Stop Flag

Stop flag là cờ báo một worker (luồng làm việc) nên dừng.

- Dùng volatile boolean cho cờ đơn giản.
- Đặt cách kết thúc worker rõ.
- Ghi ai đọc và ai ghi.

**Gợi ý:** Không dùng println/sleep như cách bảo đảm nhìn thấy cờ.

**Đạt khi:** Demo kết thúc được; hiểu bản thiếu đồng bộ có thể treo. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Bài 3: Bounded Buffer Với `wait/notifyAll`

Bounded buffer là hàng đợi có số chỗ tối đa; wait/notifyAll là cách phối hợp chờ. Đây là bài nâng cao, hướng làm ở cuối file. **Tự làm trước:** vẽ lúc đầy và rỗng. **Gợi ý:** ai cần đợi, ai có thể làm điều kiện đổi?

## Learning Log

Ghi một lịch gây lỗi và một quy tắc bảo vệ bản sửa. **Gợi ý:** “chạy 10 lần đúng” là quan sát, chưa là lý do code luôn đúng.

## Quan sát và chứng minh là hai việc khác nhau

Chờ join của mọi worker trước khi đọc kết quả. Nếu thử cờ thiếu volatile, chạy process riêng có giới hạn thời gian và cách dừng. **Tự làm:** phân biệt đọc quá sớm với mất cập nhật. **Gợi ý:** hai lỗi này có thể cùng xuất hiện.

## Đọc thêm sau

Tự viết buffer: put chờ khi đầy, take chờ khi rỗng, không nhận null, kiểm điều kiện bằng while dưới cùng khóa, notifyAll khi điều kiện đổi. Xử lý interruption (yêu cầu ngắt) và kết thúc worker; thử BlockingQueue trước khi tự viết.
