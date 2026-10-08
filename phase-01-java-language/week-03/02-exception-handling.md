# Exception: Khi Method Không Thể Hoàn Thành Công Việc

**Lượt đầu: hai lượt 45–60 phút.** Đọc phần A, chạy đủ case rồi mới sang B. Cần hiểu method nhận tham số/trả kết quả và cách đọc lỗi compile. Chưa cần file I/O, custom exception hay Spring.

## A. Hiểu Luồng Chạy Trước Khi Phân Loại Exception

### 1. Ý chính

Method có thể không hoàn thành vì đầu vào sai hoặc một thao tác thất bại. Exception mang thông tin lỗi và làm luồng chạy chuyển tới nơi xử lý phù hợp. Ba từ khóa cần phân biệt: `throw` ném lỗi, `try` bao đoạn có thể lỗi, `catch` xử lý một loại lỗi.

### 2. Giải thích code

Tạo `ExceptionDemo.java` trong thư mục ví dụ riêng:

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        System.out.println("1. Bắt đầu");
        try {
            String title = normalizeTitle("   ");
            System.out.println("2. Title: " + title);
        } catch (IllegalArgumentException exception) {
            System.out.println("3. Lỗi: " + exception.getMessage());
        }
        System.out.println("4. Kết thúc");
    }

    static String normalizeTitle(String title) {
        if (title == null || title.isBlank()) {
            throw new IllegalArgumentException("title is required");
        }
        return title.trim();
    }
}
```

Chạy:

```sh
javac -encoding UTF-8 ExceptionDemo.java
java ExceptionDemo
```

**Đoán trước:** dòng số 2 có được in ra không?

Output:

```text
1. Bắt đầu
3. Lỗi: title is required
4. Kết thúc
```

`new IllegalArgumentException(...)` tạo object mô tả lỗi. `throw` làm normalizeTitle dừng theo luồng bình thường; method không tới return. Lỗi đi về caller, bỏ qua dòng in số 2, rồi gặp catch phù hợp. Biến `exception` giữ lỗi đó, `getMessage()` đọc thông báo. Khi catch hoàn thành, code tiếp tục ở dòng số 4.

Đổi đầu vào thành `" Học Java "`: output sẽ có dòng 1, dòng 2 với title đã trim và dòng 4; catch không chạy. Exception không tự quay lại vị trí throw để thử lại.

### 3. Vì sao thiết kế như vậy

normalizeTitle chịu trách nhiệm quy tắc title; caller quyết định thông báo lỗi ra đâu. Sau này caller có thể là API controller thay vì main.

Ví dụ xử lý sai, snippet thay cho catch ở trên:

```java
catch (IllegalArgumentException exception) {
    // Không làm gì.
}
```

Chương trình bỏ qua lỗi mà không báo cho người gọi. Một lỗi khác là catch rồi in “Tạo task thành công” dù chưa tạo được task. Phải đảm bảo nhánh thành công chỉ chạy khi thao tác thật sự thành công.

### 4. Liên hệ với frontend

JS cũng có throw/try/catch. Với Java đồng bộ trong bài này, bạn có thể lần từng lời gọi method như một hàm JS đồng bộ. Không suy từ ví dụ này rằng try/catch hiện tại sẽ tự bắt mọi lỗi trên thread khác hoặc mọi async task.

### 5. Khi nào dùng và không dùng

Dùng exception khi không thể thực hiện contract của method, ví dụ title bắt buộc nhưng rỗng. Không cần ném lỗi chỉ vì danh sách task chưa có phần tử; trả danh sách rỗng là kết quả bình thường. Caller chỉ catch nếu có việc có ý nghĩa để làm: báo lỗi, phục hồi hoặc chuyển thành lỗi ở ranh giới phù hợp.

### 6. Bẫy hay gặp

- Nghĩ code ngay sau throw vẫn chạy.
- Catch mọi Exception để app “không bao giờ lỗi”, rồi giấu nguyên nhân.
- Catch và trả null khiến nơi khác phát sinh NullPointerException khó tìm hơn.
- Log lặp cùng lỗi ở mọi method thay vì xác định nơi xử lý.

### 7. Thuật ngữ mới

**Throw** là ném lỗi; **catch** là bắt lỗi phù hợp; **caller** là nơi gọi method; **stack trace** mô tả chuỗi lời gọi liên quan tới lỗi.

### 8. Bài tập nhỏ để tự gõ lại

Chạy với title hợp lệ, blank và null. Bỏ try/catch, gọi trực tiếp với blank, rồi quan sát stack trace và việc dòng “Kết thúc” không chạy. Sau đó khôi phục và viết test bằng `assertThrows` từ [bài unit test](../../learning-path/04-first-unit-test.md).

**Đạt khi:** vẽ được thứ tự các dòng chạy trong case thành công và thất bại.

## B. Checked/Unchecked Và Sự Khác Nhau Giữa Throw/Throws

### 1. Ý chính

Một số exception được compiler yêu cầu caller catch hoặc khai báo truyền tiếp bằng `throws`; đó là checked exception. Unchecked exception không có yêu cầu bắt buộc này. Cả hai đều có thể xảy ra lúc chạy; “checked” không có nghĩa là lỗi xảy ra khi compile.

### 2. Giải thích code

Để chỉ tập trung vào quy tắc compiler, ví dụ này **giả lập** lỗi đọc file, không đọc file thật. Tạo `CheckedDemo.java`:

```java
import java.io.IOException;

public class CheckedDemo {
    public static void main(String[] args) {
        try {
            loadTitle();
        } catch (IOException exception) {
            System.out.println("Không đọc được: " + exception.getMessage());
        }
    }

    static String loadTitle() throws IOException {
        throw new IOException("file unavailable (demo)");
    }
}
```

Chạy `javac -encoding UTF-8 CheckedDemo.java`, rồi `java CheckedDemo`. Output:

```text
Không đọc được: file unavailable (demo)
```

`import` cho phép gọi ngắn tên IOException từ thư viện chuẩn. `throws IOException` ở chữ ký method thông báo method có thể phát sinh lỗi loại này; nó **không xử lý lỗi**. `throw new IOException(...)` trong thân method mới thực sự ném lỗi. Demo luôn throw, nên dù khai báo trả String, không có lượt chạy thành công nào để return.

Thử bỏ try/catch và chỉ gọi `loadTitle()` trong main: compiler sẽ từ chối vì IOException chưa được xử lý hoặc khai báo. Bạn có thể thêm `throws IOException` vào chữ ký main để compile, nhưng khi chạy exception vẫn thoát ra và kết thúc luồng main.

### 3. Vì sao thiết kế như vậy

Checked exception làm nghĩa vụ xử lý/khai báo hiện rõ trong API. Tuy vậy, phân loại dựa trên cây kiểu exception, không dựa đơn giản vào “có phục hồi được không”. `IOException` là checked; `IllegalArgumentException` kế thừa RuntimeException nên unchecked. Trong Java, RuntimeException, Error và các subclass của chúng là unchecked; không có nghĩa nên bắt Error để tiếp tục chạy bình thường.

Mẫu sai về kỳ vọng:

```java
// Snippet chữ ký method: throws không có nghĩa là đã catch lỗi.
static String loadTitle() throws IOException
```

Nếu method chỉ khai báo throws, lỗi vẫn đi tới caller khi xảy ra. Không thêm throws một cách máy móc rồi coi như đã có thông báo lỗi tốt cho người dùng.

### 4. Liên hệ với frontend

JavaScript không có cơ chế checked exception buộc khai báo throws như Java. Đây là khác biệt của compiler, không phải một cách viết Promise hoặc async/await.

### 5. Khi nào dùng và không dùng

Trong lượt học đầu, dùng đúng contract của API bạn gọi: API yêu cầu xử lý IOException thì chọn catch hoặc truyền tiếp có chủ đích. Chưa cần tự thiết kế hệ thống custom exception. Unchecked exception vẫn phải được xử lý ở nơi phù hợp khi xây API, dù compiler không ép catch.

### 6. Bẫy hay gặp

Nhầm throw với throws; nghĩ checked là compile-time error thay vì exception có nghĩa vụ compile-time; catch ngay ở mọi tầng dù không biết xử lý gì; bỏ nguyên nhân gốc khi chuyển sang một exception khác.

### 7. Thuật ngữ mới

**Checked exception** là exception có nghĩa vụ catch/declare được compiler kiểm tra. **Unchecked exception** không có nghĩa vụ đó. **Propagate** là để lỗi truyền về caller. **Cause** là lỗi gốc dẫn tới lỗi được bọc bên ngoài.

### 8. Bài tập nhỏ để tự gõ lại

1. Thử hai cách: catch IOException tại main và khai báo throws tại main; so output/stack trace.
2. Tự giải thích vì sao IllegalArgumentException ở phần A không bắt buộc có throws.
3. Viết 3 câu phân biệt throw, throws và catch; mỗi câu gắn với một dòng code đã chạy.

**Đạt khi:** dự đoán được case nào compiler từ chối, case nào chạy rồi báo lỗi.

Tiếp theo: [bài thực hành nhỏ](03-task-practice.md). Try-with-resources và custom exception ở [tra cứu sau](06-advanced-reference.md), học lúc bắt đầu đọc file/JDBC.
