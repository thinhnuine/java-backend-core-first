# Flyway, Integration Test Và N+1

> **Làm rõ trước khi học:** H2 test thành công không chứng minh tương thích PostgreSQL. EAGER (yêu cầu tải sớm) không bảo đảm hết N+1. Integration test không bắt buộc luôn bao toàn bộ ứng dụng.

## Flyway

Flyway chạy migration, tức các file thay đổi cấu trúc database có thứ tự phiên bản, để môi trường mới dựng lại được cùng cấu trúc.

**Quy tắc ngón tay cái:** Thay schema bằng migration mới, không sửa lịch sử đã áp dụng.

**Tự làm:** Đặt tên V1__create_tasks.sql rồi V2__add_due_date.sql. **Gợi ý:** Hai dấu gạch dưới phân cách phiên bản với mô tả.

## Lazy Loading

Lazy loading là trì hoãn đọc dữ liệu liên quan đến khi cần truy cập nó.

**Quy tắc ngón tay cái:** Biết lúc nào dữ liệu thật được tải.

**Tự làm:** Vẽ lúc đọc task và lúc truy cập project của task. **Gợi ý:** Đừng trả entity thẳng ra HTTP rồi để chuyển JSON bất ngờ kích hoạt truy vấn.

## N+1

N+1 là một truy vấn đầu kéo theo nhiều truy vấn phụ theo từng phần tử, dễ làm thao tác đọc chậm.

**Quy tắc ngón tay cái:** Đọc số câu SQL thực tế, không đoán từ số method Java.

**Tự làm:** Giả sử 5 task khác project: liệt kê truy vấn lấy task và các project. **Gợi ý:** Dữ liệu đã tải có thể được dùng lại; không phải lúc nào cũng đúng 1+N câu.

## Integration Test

Integration test kiểm nhiều thành phần thật phối hợp; bài này cần một luồng HTTP đến PostgreSQL hoạt động đúng.

**Quy tắc ngón tay cái:** Test database nào thì chỉ kết luận về database đó.

**Tự làm:** Tạo task qua HTTP rồi đọc lại từ database test riêng. **Gợi ý:** Unit test dùng fake không chứng minh migration hay transaction đúng.

## Bài Tập Nhanh

Kiểm tra lưu trữ trên database bài tập mới.

- Chạy migration từ DB trống.
- Gọi tạo và lấy task.
- Gây lỗi ở bước ghi thứ hai để kiểm rollback.

**Gợi ý:** Kiểm dữ liệu sau khi transaction nghiệp vụ kết thúc; không chỉ dựa vào rollback của test.

**Đạt khi:** Không có dữ liệu ghi dở và không dùng database thật của người dùng. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Fetch join (lấy dữ liệu quan hệ trong truy vấn), entity graph (mô tả phần cần tải), DTO projection (chỉ lấy cột cần), phân trang với quan hệ và Testcontainers (chạy dịch vụ test trong container) học khi đã có query cần tối ưu. Để một công cụ quản lý thay schema, dùng Hibernate validate để đối chiếu thay vì tự update schema.
