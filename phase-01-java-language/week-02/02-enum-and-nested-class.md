# Enum: Chọn Trạng Thái Từ Một Tập Hợp Cố Định

**Lượt đầu: khoảng 45–60 phút.** Cần biết class, method, `if` và cách chạy Java. Tên file giữ nguyên để không làm hỏng liên kết, nhưng nested class chuyển sang phần đọc sau.

## 1. Ý chính

Enum định nghĩa một kiểu chỉ có những giá trị đã đặt tên. Task của bài này có ba trạng thái: TODO, IN_PROGRESS và DONE. Viết thành enum giúp compiler phát hiện việc dùng sai kiểu thay vì để một chuỗi sai chính tả đi sâu vào chương trình.

## 2. Giải thích code

Trong một thư mục ví dụ mới, tạo `TaskStatus.java`:

```java
public enum TaskStatus {
    TODO,
    IN_PROGRESS,
    DONE
}
```

Tạo `EnumDemo.java` cùng thư mục:

```java
public class EnumDemo {
    public static void main(String[] args) {
        TaskStatus status = TaskStatus.TODO;
        System.out.println(status);

        status = TaskStatus.DONE;
        System.out.println(isFinished(status));
    }

    static boolean isFinished(TaskStatus status) {
        return status == TaskStatus.DONE;
    }
}
```

Chạy:

```sh
javac -encoding UTF-8 TaskStatus.java EnumDemo.java
java EnumDemo
```

Kết quả:

```text
TODO
true
```

- `public enum TaskStatus`: khai báo một kiểu enum tên TaskStatus, giống như `class` khai báo một kiểu class.
- `TODO`, `IN_PROGRESS`, `DONE`: ba hằng enum được đặt tên; cách viết hoa là quy ước.
- `TaskStatus status`: biến có kiểu TaskStatus, không còn là String.
- `TaskStatus.TODO`: truy cập một giá trị đã định nghĩa; không viết `new TaskStatus()`.
- `status = TaskStatus.DONE`: đổi giá trị mà biến status giữ, không sửa định nghĩa enum.
- `static boolean isFinished(...)`: method không cần tạo EnumDemo để gọi, trả true hoặc false.
- Với enum, dùng `==` so sánh các hằng là đúng. Điều này không thay quy tắc dùng `.equals()` để so nội dung String.

## 3. Vì sao thiết kế như vậy

Ví dụ dùng chuỗi sai chính tả, đặt trong main để thử:

```java
String status = "DONNE";
System.out.println("DONE".equals(status));
```

Java vẫn compile, nhưng in false. Chương trình không biết DONNE là lỗi đánh máy.

Còn đoạn dưới cố ý không compile:

```java
TaskStatus status = TaskStatus.DONNE;
```

Vì enum không khai báo DONNE, compiler chỉ ra lỗi ngay. Tuy nhiên reference enum vẫn có thể null; khi đọc JSON vào Java, bạn vẫn phải validate đầu vào.

## 4. Liên hệ với frontend

Trong TS, bạn có thể viết `type TaskStatus = "TODO" | "IN_PROGRESS" | "DONE"`. Mục đích giới hạn giá trị khá giống nhau. Java enum tồn tại ở runtime và có thể có method; string union của TS không tự tạo một object runtime hay validate dữ liệu từ mạng.

## 5. Khi nào dùng và không dùng

Dùng enum cho status hoặc tập lựa chọn ổn định do code định nghĩa. Không dùng enum cho danh sách project, người dùng hay danh mục mà người quản trị có thể thêm tùy ý trong DB.

Lượt đầu chỉ cần biết khai báo, truyền tham số và so sánh enum. Constructor, field riêng và state transition trong enum có thể học sau. Task Manager cơ bản vẫn cho chuyển qua lại giữa ba trạng thái; chưa có rule “DONE không bao giờ đổi được”.

## 6. Bẫy hay gặp

- Gán `"DONE"` cho biến TaskStatus: String và TaskStatus là hai kiểu khác nhau.
- Lưu `ordinal()` làm mã trạng thái bền vững: đổi thứ tự hằng có thể làm thay đổi ý nghĩa số cũ.
- Gọi `TaskStatus.valueOf("done")` và mong tự đổi chữ hoa: tên không khớp gây IllegalArgumentException. Chưa cần dùng valueOf trong bài đầu.
- Ghép enum, switch expression và workflow phức tạp vào một bài khi chưa hiểu từng thứ.

## 7. Thuật ngữ mới

**Enum constant** là một giá trị đặt tên trong enum. **Tập hữu hạn** là tập có số lựa chọn xác định. **Runtime** là lúc code chạy, khác compile time là lúc kiểm tra và dịch code.

## 8. Bài tập nhỏ để tự gõ lại

1. Đoán rồi chạy `isFinished` với cả ba status.
2. Viết `label(TaskStatus status)` trả “Cần làm”, “Đang làm”, “Hoàn thành”; dùng if/else bạn đã biết.
3. Tự chọn cách xử lý null rõ ràng cho label; viết test cho lựa chọn đó nếu đã học JUnit.
4. Viết một method nhận TaskStatus, cố gọi bằng String rồi giải thích lỗi compile.

**Đạt khi:** truyền được enum qua method và giải thích vì sao nó chặn typo tốt hơn String.

Tiếp theo: [bài tập cơ bản](03-exercises.md). Nested/inner/anonymous class nằm trong [tra cứu nâng cao](06-advanced-reference.md), chưa phải điều kiện qua bài.
