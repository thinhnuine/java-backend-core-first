# Backend Nhận Request Và Chịu Trách Nhiệm Gì?

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Backend là chương trình nhận request (yêu cầu), kiểm tra và xử lý rồi trả response (phản hồi); HTTP là cách FE và server trao đổi các thông điệp đó.

## 2. Giải thích request/response

```java
record Request(String title) {} // Minh họa dữ liệu vào, chưa phải HTTP thật
record Response(int status, String message) {}
Request input = new Request("Java");
Response output = new Response(201, "Created " + input.title()); // 201: đã tạo
System.out.println(output.message()); // Created Java
```

## 3. Vì sao thiết kế như vậy

FE có thể bị bỏ qua bằng công cụ gọi trực tiếp nên server phải kiểm tra lại dữ liệu và quyền. **Quy tắc ngón tay cái:** client gửi yêu cầu, server quyết định có chấp nhận.

## 4. Liên hệ với frontend

Tab Network trong browser hiển thị request/response; khi làm backend bạn viết phần tạo response đó. fetch nhận 400/500 vẫn có response: cần kiểm status, không chỉ catch lỗi mạng.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Thiết kế API (điểm giao tiếp cho chương trình khác gọi).
- Chẩn đoán lỗi giữa FE và server.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Trả 200 cho mọi lỗi.
- Tin userId do FE gửi là danh tính đã xác thực.
- Nghĩ timeout (hết thời gian chờ) chứng minh server chưa lưu gì.

## 7. Thuật ngữ mới

**Endpoint**: method HTTP và đường dẫn, như POST /tasks. **JSON**: định dạng dữ liệu văn bản. **Contract**: thỏa thuận input/output/status giữa FE và BE.

## 8. Bài tập nhỏ để tự làm

**Tự gõ, không dùng AI sinh code:** Viết request và response cho tạo task đúng, title trắng, id không tồn tại.

**Gợi ý:** Dùng 201 tạo thành công, 400 đầu vào sai, 404 không tìm thấy; thêm GET đọc và DELETE xóa.

## Đọc thêm sau

GET để đọc; DELETE xóa thành công có thể trả 204 không body; PUT thay thế, PATCH sửa một phần theo contract. CORS là quy tắc browser đọc response khác origin (giao thức+tên máy+cổng), không phải kiểm quyền. Idempotency (tác động khi gọi lặp) và chống tạo trùng học sau. [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview).
