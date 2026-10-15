# Buổi Đầu: Từ File Java Đến Chương Trình Chạy Được

> **Làm rõ trước khi học:** Java 21 chạy được file nguồn bằng java Main.java; không bắt buộc tự chạy javac trước trong cách này. JShell dành cho các khối ngắn ở bài sau.

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

## 1. Ý chính

JDK là bộ công cụ để viết và chạy Java; hôm nay chỉ cần tự chạy một file rồi đổi đầu vào.

## 2. Giải thích code

```java
public class Main { // Lưu đúng tên Main.java
    public static void main(String[] args) { // Điểm bắt đầu chương trình
        String title = " Java "; // String là văn bản
        System.out.println(title.trim()); // In Java, bỏ khoảng trắng hai đầu
    }
}
```

## 3. Vì sao thiết kế như vậy

Riêng ví dụ này: tạo Main.java trong thư mục bài tập mới, chạy `java Main.java` bằng Java 21. Hoặc `javac Main.java` để compile (kiểm tra và dịch), rồi `java Main` để chạy file đã dịch. **Quy tắc ngón tay cái:** kiểm đúng file và đúng JDK trước khi sửa code.

## 4. Liên hệ với frontend

main là điểm chạy đầu tiên, gần file khởi động script JS. Java tổ chức method (hàm) trong class (bản mô tả dữ liệu/hành vi).

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Thử cú pháp bằng file đơn.
- Tập đọc lỗi trước khi học Spring.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Tên file khác public class.
- Chạy file class cũ sau khi sửa source.
- IDE và terminal dùng hai JDK khác nhau.

## 7. Thuật ngữ mới

**public**: cho phép truy cập từ ngoài. **static**: gọi method không cần tạo object. **void**: không trả kết quả. **String[] args**: mảng tham số dòng lệnh, chưa dùng trong bài.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Đổi title thành chuỗi khác; cố ý xóa dấu ; rồi đọc lỗi.

**Gợi ý:** Trong IntelliJ dùng New → Java Class → Main; chọn Project SDK 21 và Run cạnh main.

## Đọc thêm sau

JVM là máy thực thi Java; bytecode là mã sau khi dịch. Maven là công cụ quản lý thư viện và các bước kiểm tra/đóng gói project. Tìm hiểu Maven khi bắt đầu test; xem [unit test đầu tiên](04-first-unit-test.md).
