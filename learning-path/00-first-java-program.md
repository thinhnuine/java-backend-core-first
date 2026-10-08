# Buổi Đầu: Từ File Java Đến Chương Trình Chạy Được

**Học ở tuần 1 mới.** Cần biết biến, hàm, điều kiện, vòng lặp từ FE. Chưa cần OOP nâng cao hoặc Spring. Dành buổi đầu 2 giờ cho ví dụ này, các buổi còn lại để tự sửa và debug.

## 1. Ý chính

JDK là bộ công cụ phát triển Java: có compiler `javac` và lệnh `java` để chạy chương trình trên JVM. Compiler biến source `.java` thành bytecode `.class`; JVM thực thi bytecode. Maven sẽ giúp quản lý dependency, build và test khi project lớn hơn.

```text
Main.java --javac--> Main.class --java/JVM--> kết quả
```

Kiểm tra trong terminal:

```sh
java -version
javac -version
mvn -version
```

Dùng JDK 21 cho bài. `java` và `javac` cần cùng major version; `mvn -version` cho biết Maven đang dùng JDK nào. Nếu thiếu lệnh, cài JDK/Maven bằng công cụ quản lý của hệ điều hành hoặc JDK manager rồi mở terminal mới. Trong IDE, chọn Project SDK 21; kiểm tra JDK dùng cho Maven cũng khớp.

## 2. Giải thích code

Tạo thư mục `java-core-lab`, tạo file `Main.java` trong đó:

```java
public class Main {
    public static void main(String[] args) {
        String title = "  Học Java  ";
        String normalized = normalizeTitle(title);
        System.out.println(normalized);
    }

    static String normalizeTitle(String title) {
        if (title == null || title.isBlank()) {
            throw new IllegalArgumentException("title is required");
        }
        return title.trim();
    }
}
```

Chạy từ thư mục chứa `Main.java`:

```sh
javac -encoding UTF-8 Main.java
java Main
```

Kết quả:

```text
Học Java
```

- Tên file trùng với `public class Main`. `public` cho phép truy cập class từ bên ngoài.
- `main` là điểm bắt đầu của ví dụ. `static` nghĩa là gọi method mà không cần tạo object `Main`; `void` nghĩa là không trả kết quả.
- `String[] args` là mảng tham số dòng lệnh. Bài này chưa dùng đến.
- `normalizeTitle` nhận một `String`, trả một `String`; không viết từ khóa `function` như JavaScript.
- `||` short-circuit: nếu `title == null` đúng, Java không gọi `isBlank()` trên null.
- `throw` dừng luồng bình thường bằng exception. `IllegalArgumentException` biểu thị tham số không hợp lệ.
- `trim()` trả chuỗi đã bỏ khoảng trắng ở hai đầu; không sửa chuỗi gốc. Với khoảng trắng Unicode, tìm hiểu thêm `strip()` sau.

Đổi `title` thành `"   "`, compile và chạy lại. Bạn cần thấy `IllegalArgumentException: title is required`. Trong stack trace, tìm dòng có `Main.normalizeTitle` và số dòng trong code của mình, rồi lần về `Main.main`.

## 3. Vì sao thiết kế như vậy

Tách xử lý title ra method để có thể gọi từ terminal, unit test và API sau này. Quy tắc “title không được rỗng” không phụ thuộc giao diện nhập liệu.

Ví dụ sai:

```java
// Đoạn minh họa nằm trong một method, không phải file chạy độc lập.
if (title.isBlank() || title == null) {
    throw new IllegalArgumentException("title is required");
}
```

Với `title = null`, lỗi xảy ra ngay ở `isBlank()`. Bạn không tới được phần kiểm tra null và không nhận thông báo nghiệp vụ mình muốn.

## 4. Liên hệ với frontend

`normalizeTitle` tương tự một utility function trong JS. Khác biệt trước mắt là bạn khai báo kiểu tham số và kiểu trả về, đặt method trong class và compile trước khi chạy theo cách minh họa này. TypeScript type không validate JSON từ bên ngoài; Java DTO cũng cần validation runtime khi làm API.

| FE quen thuộc | Vai trò gần tương ứng trong Java | Giới hạn phép so sánh |
| --- | --- | --- |
| `package.json` | `pom.xml` | Maven có lifecycle riêng, không dùng npm scripts |
| thư mục mã nguồn | `src/main/java` | Package và vị trí file phải được tổ chức nhất quán |
| Jest/Vitest test | `src/test/java`, JUnit | Cú pháp assertion và cách chạy khác |
| Browser/Node chạy JS | JVM chạy bytecode | JVM không phải web server |

## 5. Khi nào dùng và không dùng

Dùng file đơn để nhìn rõ compile/run và thử cú pháp. Khi bắt đầu dependency hoặc unit test ở tuần 3, tạo Maven project trong IDE với JDK 21; đặt code dưới `src/main/java/dev/thinh/task`, test dưới `src/test/java/dev/thinh/task`, thêm `package dev.thinh.task;` ở đầu file. Học Maven lifecycle `test`, `package`, `verify`; `package` tạo artifact, `verify` chạy các bước kiểm tra đã cấu hình.

Không cần tạo Spring project ngay buổi đầu: hôm nay đầu ra cần đạt là hiểu chương trình chạy từ đâu.

## 6. Bẫy hay gặp

- Sửa `.java` nhưng quên compile lại nên đang chạy `.class` cũ.
- Gõ `java Main.class` thay vì `java Main` trong quy trình trên.
- Thiếu dấu `;`: compile error; `title = null` rồi gọi method: runtime error. Hai loại lỗi cần cách đọc khác nhau.
- IDE chạy được nhưng terminal lỗi: kiểm tra thư mục hiện tại và JDK ở cả hai nơi.
- Nghĩ `int / int` luôn trả số thập phân: `5 / 2` là `2`, `5 / 2.0` là `2.5`.

## 7. Thuật ngữ mới

- **Compile**: kiểm tra và dịch source sang bytecode.
- **Runtime**: lúc chương trình đang chạy.
- **Stack trace**: chuỗi lời gọi method dẫn tới lỗi.
- **Breakpoint**: vị trí debugger tạm dừng để bạn xem biến.
- **Dependency**: thư viện hoặc object mà code cần để hoạt động.

## 8. Bài tập nhỏ để tự gõ lại

1. Chạy ví dụ cho title hợp lệ, rỗng và null; ghi kết quả dự đoán trước khi chạy.
2. Thêm điều kiện giới hạn 100 ký tự; tự xác định kiểm tra trước hay sau trim và giải thích.
3. Viết method nhận mảng title, dùng `for` in các title hợp lệ. Quy định rõ khi gặp title sai thì dừng hay bỏ qua.
4. Đặt breakpoint ở `return title.trim()`, xem giá trị `title`, dùng Step Over và Step Into để phân biệt.
5. Xóa một dấu `;`, đọc lỗi compiler, rồi khôi phục. Không dùng AI sửa ngay.

**Đạt khi:** bạn tự chạy lại được mà không nhìn lệnh mẫu, chỉ ra được dòng gây lỗi và giải thích vì sao cần kiểm tra null trước.

Khi tới tuần 3, dùng [unit test đầu tiên](04-first-unit-test.md) để tạo POM và chạy test.

Tiếp theo: [type system](../phase-01-java-language/week-01/01-type-system.md), đọc phần Package/Class trước rồi quay về primitive/reference.
