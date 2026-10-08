# Generics: Biết Trong Hộp Có Kiểu Dữ Liệu Gì

**Lượt đầu: khoảng 60–90 phút.** Cần biết biến, class, constructor, method và `new`. Nếu chưa tự chạy được một class có `main`, quay lại [buổi đầu](../../learning-path/00-first-java-program.md). Bài thuộc tuần 4 trong lịch mới; tên thư mục `week-02` là mã cũ.

Mục tiêu hôm nay: đọc được `List<String>`, tự dùng `Box<String>` và hiểu compiler đang giúp bắt lỗi gì. Chưa cần wildcard, PECS, bounded type hay generic repository.

## 1. Ý chính

Generics cho phép viết một class dùng được với nhiều kiểu dữ liệu nhưng vẫn kiểm tra kiểu lúc compile. Trong `Box<String>`, phần `<String>` nói rằng chiếc hộp này chứa chuỗi. Chữ `T` trong định nghĩa `Box<T>` là tên đại diện cho kiểu mà người dùng class sẽ chọn.

Trước tiên nhìn thứ quen thuộc hơn:

```java
// Snippet: đặt trong main, thêm import java.util.ArrayList;
// và import java.util.List; ở đầu file.
List<String> titles = new ArrayList<>();
titles.add("Học Java");
String first = titles.get(0);
System.out.println(first);
```

`List` là danh sách; `ArrayList` là một cách triển khai danh sách. `add` thêm phần tử; `get(0)` lấy phần tử đầu tiên, đánh số từ 0 như array JS. `<String>` ràng buộc kiểu phần tử, còn `<>` bên phải để compiler suy ra kiểu đó.

Tạm dừng và đoán: nếu thay `titles.add("Học Java")` bằng `titles.add(123)`, lỗi xảy ra lúc compile hay lúc chạy? Hãy thử để thấy compiler chặn ngay.

## 2. Giải thích code

Tạo thư mục riêng cho ví dụ, đặt **hai file cùng thư mục**, chưa dùng package để tập trung vào generics.

`Box.java`:

```java
public final class Box<T> {
    private final T value;

    public Box(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
```

`GenericDemo.java`:

```java
public class GenericDemo {
    public static void main(String[] args) {
        Box<String> titleBox = new Box<>("Học Java");
        String title = titleBox.get();
        System.out.println(title.toUpperCase());

        Box<Integer> countBox = new Box<>(3);
        int count = countBox.get();
        System.out.println(count + 1);
    }
}
```

Chạy từ thư mục chứa hai file:

```sh
javac -encoding UTF-8 Box.java GenericDemo.java
java GenericDemo
```

Kết quả:

```text
HỌC JAVA
4
```

Đọc từng phần:

| Code | Cách hiểu |
| --- | --- |
| `public final class Box<T>` | Class cho bên ngoài sử dụng; `final` không cho kế thừa; `T` là tham số kiểu |
| `private final T value` | Field chỉ truy cập trực tiếp bên trong class và chỉ gán một lần; kiểu của nó là T |
| `public Box(T value)` | Constructor nhận giá trị thuộc kiểu T; constructor không có kiểu trả về |
| `this.value = value` | Gán tham số vào field của object đang tạo |
| `public T get()` | Method trả về cùng kiểu T |
| `Box<String>` | Ở cách sử dụng này, compiler hiểu T là String |
| `Box<Integer>` | Ở cách sử dụng này, compiler hiểu T là Integer |

`Integer` là wrapper của `int`: generics trong Java 21 cần reference type, nên không viết `Box<int>`. Số `3` được boxing thành Integer; gán kết quả cho `int count` thì được unboxing. Không cần học implementation của boxing để làm bài này.

Bạn có thể đọc nhẩm `Box<T>` là “Box của kiểu T”, giống đọc tên tham số trong hàm. `T` không phải từ khóa bắt buộc; đổi nhất quán thành `ValueType` vẫn hợp lệ.

## 3. Vì sao thiết kế như vậy

Nếu hộp chỉ dùng `Object`, nó nhận được nhiều kiểu nhưng caller phải tự đoán kiểu thực tế. Ví dụ sau **cố ý sai**, đặt trong `main` để thử:

```java
Object value = 123;
String title = (String) value;
System.out.println(title);
```

`(String)` là cast: bạn yêu cầu Java coi giá trị đó là String. Đoạn này compile được nhưng chạy sẽ ném `ClassCastException`, vì object thực tế là Integer. Cast không biến số thành chuỗi.

Với generics, thử thêm dòng sau vào `GenericDemo`:

```java
// Cố ý không compile: Box<String> không nhận Integer.
Box<String> wrong = new Box<>(123);
```

Compiler phát hiện mâu thuẫn trước khi chạy. Xóa dòng sai sau khi đã đọc lỗi.

## 4. Liên hệ với frontend

Nếu đã dùng TypeScript, `Array<string>` khá gần `List<String>` ở mục đích ràng buộc kiểu phần tử. Khi dùng một type `Box<T>` trong TS, bạn cũng chọn T ở nơi sử dụng. Đây là sự tương đồng về kiểm tra type, không có nghĩa generics tự validate JSON từ API hoặc tự cấm null.

## 5. Khi nào dùng và không dùng

| Tình huống | Việc nên làm lúc này |
| --- | --- |
| Danh sách title | Dùng `List<String>` |
| Danh sách task | Sau khi có class Task, dùng `List<Task>` |
| Cấu trúc giống nhau chỉ khác kiểu dữ liệu | Có thể dùng generic class |
| Class chỉ phục vụ một nghiệp vụ Task rõ ràng | Cứ viết class cụ thể; chưa cần tổng quát hóa |

Box là ví dụ học cú pháp. Project thật không cần bọc mọi String vào một Box.

## 6. Bẫy hay gặp

- Viết `Box box` bỏ phần type, khiến mất kiểm tra kiểu hữu ích.
- Nghĩ `T` là một object để gọi `new T()`; ở đây T là tham số kiểu, không phải constructor cụ thể.
- Viết `Box<int>` thay vì `Box<Integer>`.
- Nghĩ `final T value` làm object bên trong immutable; nó chỉ ngăn gán lại field.
- Nghĩ `Box<String>` tự cấm null: ví dụ này vẫn nhận null, nên gọi method trên giá trị đó có thể lỗi.

## 7. Thuật ngữ mới

- **Type parameter**: tên đại diện cho kiểu, như T trong định nghĩa class.
- **Type argument**: kiểu được chọn lúc dùng, như String trong Box<String>.
- **Type inference**: compiler suy ra kiểu, ví dụ ở `new Box<>(...)`.
- **Cast**: yêu cầu coi một giá trị theo kiểu khác; có thể thất bại khi chạy.
- **Type-safe**: trong ngữ cảnh này, compiler kiểm tra cách dùng kiểu mà không cần caller cast thủ công.

## 8. Bài tập nhỏ để tự gõ lại

1. Chạy nguyên ví dụ, rồi tạo thêm `Box<Boolean>` chứa `true` và in giá trị.
2. Thử gán `titleBox.get()` vào biến Integer, đọc lỗi compiler rồi sửa lại.
3. Tạo class `TaskTitle` có constructor và method `text()`, rồi dùng `Box<TaskTitle>`. In nội dung qua `box.get().text()`.
4. Giải thích thành lời: “Với Box<TaskTitle>, get trả về kiểu gì? Vì sao không cần cast?”

**Đạt khi:** làm được bước 3–4 mà không xem lời giải; chưa cần tự viết generic method.

Tiếp theo: [enum](02-enum-and-nested-class.md). Chỉ khi đã thoải mái với Box và List mới mở [tra cứu nâng cao](06-advanced-reference.md).
