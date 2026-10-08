# Record Và Switch: Viết Gọn Điều Bạn Đã Hiểu

**Lượt đầu: khoảng 60–90 phút.** Cần biết constructor, field, accessor và enum. Đây là bài tuần 5 trong lịch mới, không phải yêu cầu học mọi tính năng Java 21.

Dùng hai thư mục ví dụ riêng cho record và switch. Các file bên dưới không có package; chạy với JDK 21, không cần preview flag. Sealed class, pattern matching và text block nằm trong [tra cứu sau](06-advanced-reference.md).

## A. Record Để Mang Dữ Liệu

### 1. Ý chính

Khi một object chủ yếu mang dữ liệu, Java thường cần constructor, accessor và các method so sánh/in thông tin. Record giúp khai báo ngắn phần đó. Bạn nên hiểu class bình thường trước để biết record đang viết hộ mình điều gì.

### 2. Giải thích code

Tạo `TaskSummary.java`:

```java
public record TaskSummary(long id, String title) {
}
```

Tạo `RecordDemo.java` cùng thư mục:

```java
public class RecordDemo {
    public static void main(String[] args) {
        TaskSummary task = new TaskSummary(1L, "Học Java");
        System.out.println(task.id());
        System.out.println(task.title());
        System.out.println(task.equals(new TaskSummary(1L, "Học Java")));
    }
}
```

Chạy:

```sh
javac -encoding UTF-8 TaskSummary.java RecordDemo.java
java RecordDemo
```

Output:

```text
1
Học Java
true
```

`long id, String title` khai báo hai component của record. Java sinh field private final, constructor tương ứng, accessor `id()`/`title()`, cùng `equals`, `hashCode`, `toString`. `1L` là literal long; `new TaskSummary(...)` vẫn tạo object như class thông thường.

Bạn đọc khai báo này là: “TaskSummary mang một id kiểu long và một title kiểu String”. Accessor là `title()`, không phải `getTitle()`.

### 3. Vì sao thiết kế như vậy

Record hữu ích khi dữ liệu cần được đọc và truyền đi mà không cần setter. Ví dụ sau cố ý không compile:

```java
// Snippet đặt trong main của RecordDemo.
task.title = "Học SQL";
```

Field vừa private vừa final. Nếu muốn giá trị mới, tạo record mới. Nhưng record **không tự validate input**: `new TaskSummary(1L, null)` vẫn hợp lệ với định nghĩa ở trên.

Record cũng không tự đóng băng object bên trong. Nếu có component `List<Task>` và caller sửa list đó, nội dung nhìn qua record vẫn đổi. Lượt đầu chỉ dùng long và String; defensive copy học khi cần chứa collection.

### 4. Liên hệ với frontend

Record có thể mang dữ liệu như một object có shape rõ trong TS. Khác với TS type alias, Java record là kiểu runtime có constructor và method thực sự. Đừng hiểu record là một lệnh JSON validation.

### 5. Khi nào dùng và không dùng

Dùng cho dữ liệu trả về của method hoặc DTO đơn giản. Chưa cần chuyển mọi class Task thành record: class có nghiệp vụ thay đổi state có thể phù hợp hơn. Record không phải lựa chọn mặc định cho JPA entity ở chặng database.

### 6. Bẫy hay gặp

- Tìm setter do nghĩ record là class mutable tự sinh getter/setter.
- Nghĩ record cấm null hoặc tự kiểm id dương.
- Nghĩ object lồng bên trong đều immutable.
- Nghĩ record thay mọi loại class.

### 7. Thuật ngữ mới

**Component** là mục dữ liệu trong khai báo record. **Accessor** là method để đọc giá trị. **DTO** là object mang dữ liệu qua ranh giới, chẳng hạn từ service tới phần trả response.

### 8. Bài tập nhỏ để tự gõ lại

Tạo record `TaskCount(String status, int count)`, tạo hai object cùng dữ liệu, kiểm `equals` và in từng accessor. Đổi count của object thứ hai bằng cách tạo object mới rồi dự đoán kết quả equals.

**Đạt khi:** chỉ ra được record sinh hộ những gì và những gì nó không kiểm tra.

## B. Switch Expression Để Chọn Một Kết Quả

### 1. Ý chính

Bạn đã viết if/else để đổi enum thành nhãn. Switch expression là cách diễn đạt một phép chọn kết quả theo nhiều trường hợp. Hãy học nó bằng cùng bài toán label, không cần ghép với sealed type.

### 2. Giải thích code

Trong thư mục ví dụ thứ hai, tạo lại `TaskStatus.java` với ba giá trị TODO, IN_PROGRESS, DONE như [bài enum](../week-02/02-enum-and-nested-class.md). Tạo `SwitchDemo.java`:

```java
public class SwitchDemo {
    public static void main(String[] args) {
        System.out.println(label(TaskStatus.IN_PROGRESS));
    }

    static String label(TaskStatus status) {
        if (status == null) {
            throw new IllegalArgumentException("status is required");
        }
        return switch (status) {
            case TODO -> "Cần làm";
            case IN_PROGRESS -> "Đang làm";
            case DONE -> "Hoàn thành";
        };
    }
}
```

Chạy `javac -encoding UTF-8 TaskStatus.java SwitchDemo.java`, rồi `java SwitchDemo`. Output là `Đang làm`.

`switch (status)` xem giá trị của status. Mỗi `case ... ->` chọn một String. `return` trả String được chọn về caller; dấu `;` sau `}` kết thúc return statement. Các nhánh mũi tên không chạy xuyên sang nhánh kế tiếp nên không cần break. Kiểm null trước switch để có thông báo rõ.

### 3. Vì sao thiết kế như vậy

Switch expression cần có kết quả cho mọi trường hợp có thể xảy ra. Thử xóa `case DONE` rồi compile: compiler sẽ báo không bao phủ đủ các giá trị. Đây là lỗi cố ý để bạn thấy compiler hỗ trợ phát hiện nhánh bị quên.

Nếu sau này thêm status BLOCKED, cần cập nhật mapping. Đừng tự động dùng `default -> "Không biết"` để che mọi trường hợp thiếu khi bạn muốn compiler nhắc mình sửa.

### 4. Liên hệ với frontend

Nó gần một switch hoặc phép tra cứu label trong JS/TS. Điểm cần chú ý là đây là expression tạo ra giá trị, và Java kiểm tra độ bao phủ cho enum trong ví dụ này.

### 5. Khi nào dùng và không dùng

Dùng khi ánh xạ một tập lựa chọn sang kết quả. If/else vẫn phù hợp cho điều kiện như “title quá dài và user chưa có quyền”; không cần đổi mọi if thành switch.

### 6. Bẫy hay gặp

Quên dấu `;` sau switch expression; truyền null mà chưa có chính sách xử lý; chép các status từ bài workflow nâng cao rồi quên cập nhật enum đang dùng.

### 7. Thuật ngữ mới

**Expression** là biểu thức tạo ra giá trị. **Exhaustive** nghĩa là bao phủ đủ các trường hợp cần xử lý.

### 8. Bài tập nhỏ để tự gõ lại

Chạy label với ba status; so kết quả với bản if/else đã viết. Thêm BLOCKED vào enum rồi sửa switch theo lỗi compiler. Sau bài tập, giữ enum mở rộng trong thư mục riêng để không vô tình đổi rule Task Manager cơ bản.

**Đạt khi:** viết được label bằng switch và giải thích tại sao nhánh bị thiếu làm compile fail.

Tiếp theo: [exception](02-exception-handling.md). Record có giới hạn shallow immutability như mô tả trong [Java 21 Record API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Record.html).
