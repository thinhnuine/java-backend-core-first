# Type System Trong Java

## 1. Primitive Vs Reference Type

Java có 2 nhóm type lớn:

- Primitive: `byte`, `short`, `int`, `long`, `float`, `double`, `char`, `boolean`.
- Reference: object, array, `String`, class tự định nghĩa, interface type.

Khác biệt quan trọng:

- Primitive giữ giá trị trực tiếp.
- Reference giữ tham chiếu tới object.
- Reference có thể là `null`, primitive thì không.
- So sánh primitive bằng `==` là so sánh giá trị.
- So sánh reference bằng `==` là so sánh cùng trỏ tới một object hay không.

Ví dụ:

```java
int a = 10;
int b = 10;
System.out.println(a == b); // true

String x = new String("hello");
String y = new String("hello");
System.out.println(x == y);      // false
System.out.println(x.equals(y)); // true
```

## 2. Boxing Và Unboxing

Mỗi primitive có wrapper type:

- `int` -> `Integer`
- `long` -> `Long`
- `double` -> `Double`
- `boolean` -> `Boolean`

Boxing là primitive thành object. Unboxing là object về primitive.

```java
Integer count = 10; // boxing
int value = count;  // unboxing
```

Điểm dễ lỗi:

```java
Integer count = null;
int value = count; // NullPointerException
```

Quy tắc thực dụng:

- Dùng primitive cho field/value bắt buộc có giá trị.
- Dùng wrapper khi cần biểu diễn "không có giá trị", làm việc với generic, hoặc mapping database.

## 3. `String` Và Immutability

`String` là immutable. Mỗi thao tác tạo chuỗi mới thay vì sửa chuỗi cũ.

```java
String name = "Java";
String upper = name.toUpperCase();

System.out.println(name);  // Java
System.out.println(upper); // JAVA
```

Khi nối chuỗi trong vòng lặp lớn, dùng `StringBuilder`.

```java
StringBuilder builder = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    builder.append(i);
}
String result = builder.toString();
```

## 4. Package, Class Và Access Modifier

Một class thường nằm trong package:

```java
package dev.thinh.javacore.user;

public class User {
    private final String id;
    private final String email;

    public User(String id, String email) {
        this.id = id;
        this.email = email;
    }

    public String id() {
        return id;
    }

    public String email() {
        return email;
    }
}
```

Access modifier:

- `public`: ai cũng gọi được.
- `protected`: cùng package hoặc subclass.
- Không ghi gì: package-private, chỉ cùng package.
- `private`: chỉ trong class.

Quy tắc thực dụng:

- Field gần như luôn `private`.
- Nếu object nên immutable, field dùng `private final`.
- Public API càng nhỏ càng dễ giữ ổn định.

## 5. Bài Tập Nhanh

Viết class `Money`:

- Có `amount` kiểu `long`.
- Có `currency` kiểu `String`.
- Immutable.
- Không cho phép amount âm.
- Không cho phép currency null hoặc blank.
- Có method `add(Money other)`.
- Chỉ cộng được nếu cùng currency.

Sau khi viết xong, trả lời:

- Field nào nên là primitive, field nào nên là reference?
- Method nào nên public?
- Trường hợp lỗi nên throw exception gì?
