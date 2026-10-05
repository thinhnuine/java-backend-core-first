# Type System Trong Java

## Cách Học File Này

Đừng đọc một mạch từ đầu tới cuối như đọc blog. Với mỗi section, hãy tạo một class nhỏ trong `java-core-lab`, chạy thử, sửa code cho lỗi xuất hiện, rồi mới đọc tiếp.

Mục tiêu của bài này là nắm được 5 câu hỏi:

- Value này có thể `null` không?
- Mình đang so sánh giá trị hay so sánh object identity?
- Field này có được đổi sau khi tạo object không?
- Constructor có bảo vệ object khỏi state sai không?
- Public API của class có đủ nhỏ không?

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

Nếu bạn đến từ TypeScript, điểm khác rất lớn là Java phân biệt rõ primitive và object/reference hơn. `int` không phải object, không thể `null`, không gọi method được. `Integer` là object, có thể `null`, dùng được với generic như `List<Integer>`.

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

### Mental Model

Với primitive, biến giống như giữ trực tiếp giá trị:

```text
a -> 10
b -> 10
```

Với reference, biến giống như giữ địa chỉ trỏ tới object:

```text
x -> object String("hello")
y -> object String("hello")
```

Hai object có cùng nội dung chưa chắc là cùng object. Vì vậy `==` và `.equals()` khác nhau.

### Ví Dụ Dễ Sai

```java
String currency = new String("USD");

if (currency == "USD") {
    System.out.println("same");
}
```

Code này có thể làm bạn tưởng đúng, nhưng `==` đang so sánh reference. Với `String`, hãy dùng:

```java
if ("USD".equals(currency)) {
    System.out.println("same");
}
```

Viết literal ở bên trái giúp tránh `NullPointerException` nếu `currency` là `null`.

### Bài Tập Nhỏ

Tạo class `TypePlayground` và thử:

```java
package dev.thinh.javacore;

public class TypePlayground {
    public static void main(String[] args) {
        int a = 1000;
        int b = 1000;
        System.out.println(a == b);

        String x = new String("USD");
        String y = new String("USD");
        System.out.println(x == y);
        System.out.println(x.equals(y));
    }
}
```

Tự giải thích từng dòng output trước khi hỏi AI.

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

### Khi Nào Gặp Boxing Trong Thực Tế

Generic trong Java không dùng primitive trực tiếp:

```java
// List<int> numbers = new ArrayList<>(); // không hợp lệ
List<Integer> numbers = new ArrayList<>();
```

Khi bạn add `int` vào `List<Integer>`, Java boxing ngầm:

```java
numbers.add(10); // int -> Integer
```

Khi lấy ra gán vào `int`, Java unboxing:

```java
int first = numbers.get(0); // Integer -> int
```

### Lỗi Kinh Điển

```java
Integer value = null;
if (value > 0) {
    System.out.println("positive");
}
```

`value > 0` cần unbox `Integer` thành `int`, nhưng `value` là `null`, nên lỗi runtime.

Trong domain object như `Money`, `amount` là bắt buộc có giá trị, nên dùng:

```java
private final long amount;
```

Không cần:

```java
private final Long amount;
```

trừ khi business thật sự cho phép amount vắng mặt.

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

### Vì Sao String Immutable Quan Trọng

Nếu `String` mutable, code kiểu này sẽ rất nguy hiểm:

```java
String currency = "USD";
Money money = new Money(100, currency);
```

Nếu caller có thể sửa nội dung `currency` sau khi tạo `Money`, object `Money` có thể bị đổi state từ bên ngoài. Vì `String` immutable, `Money` an toàn hơn.

### Immutability Không Chỉ Là `final`

`final` trên field nghĩa là field không thể trỏ sang object khác sau khi gán.

Nhưng nếu field trỏ tới object mutable, nội dung object đó vẫn có thể đổi.

Ví dụ:

```java
public final class BadBasket {
    private final List<String> items;

    public BadBasket(List<String> items) {
        this.items = items;
    }

    public List<String> items() {
        return items;
    }
}
```

`items` là `final`, nhưng caller vẫn có thể sửa list. Với object immutable thật sự, phải copy list và không expose mutable list trực tiếp. Phần này sẽ gặp lại ở Collections.

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

### Public API Là Gì?

Public API là phần class cho bên ngoài dùng. Ví dụ với `Money`, API hợp lý là:

```java
public Money(long amount, String currency)
public long amount()
public String currency()
public Money add(Money other)
```

Field không nên public:

```java
public long amount; // tránh
```

Vì nếu field public, người khác có thể sửa trực tiếp, object mất quyền kiểm soát invariant.

### Constructor Bảo Vệ Invariant

Invariant là rule luôn đúng trong suốt vòng đời object.

Với `Money`, invariant là:

- `amount >= 0`
- `currency != null`
- `currency` không blank

Constructor phải chặn object sai ngay từ đầu:

```java
public Money(long amount, String currency) {
    if (amount < 0) {
        throw new IllegalArgumentException("amount must be non-negative");
    }
    if (currency == null || currency.isBlank()) {
        throw new IllegalArgumentException("currency is required");
    }

    this.amount = amount;
    this.currency = currency;
}
```

Nếu constructor cho object sai lọt qua, mọi method sau đó đều phải phòng thủ nhiều hơn.

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

## 6. Hướng Dẫn Làm Bài `Money` Theo Từng Bước

### Bước 1: Tạo Class Rỗng

```java
package dev.thinh.javacore;

public final class Money {
}
```

Chạy compile để chắc package/path đúng.

### Bước 2: Thêm Field

```java
private final long amount;
private final String currency;
```

Bạn sẽ thấy Java bắt constructor phải gán field `final`.

### Bước 3: Thêm Constructor

```java
public Money(long amount, String currency) {
    this.amount = amount;
    this.currency = currency;
}
```

Compile lại. Sau đó mới thêm validation.

### Bước 4: Thêm Getter Kiểu Java Hiện Đại

```java
public long amount() {
    return amount;
}

public String currency() {
    return currency;
}
```

Ở Java truyền thống bạn cũng sẽ thấy style `getAmount()`, `getCurrency()`. Trong bài core này dùng `amount()` và `currency()` cho gọn, giống record accessor.

### Bước 5: Thêm Validation

Thêm check:

- amount âm
- currency null
- currency blank

Throw `IllegalArgumentException` vì caller truyền argument sai contract.

### Bước 6: Thêm `add`

Luồng của `add`:

```text
Nếu other null -> lỗi
Nếu currency khác -> lỗi
Nếu hợp lệ -> trả Money mới
```

Nhớ: immutable object không tự sửa state.

## 7. Lỗi Thường Gặp

### Quên Gán Field `final`

```text
Field 'amount' might not have been initialized
```

Nghĩa là constructor chưa gán `this.amount`.

### So Sánh String Bằng `==`

Sai:

```java
this.currency == other.currency
```

Đúng:

```java
this.currency.equals(other.currency)
```

### Viết Method Làm Đổi State

Sai với immutable:

```java
this.amount = this.amount + other.amount;
```

Đúng:

```java
return new Money(this.amount + other.amount, this.currency);
```

### Cho Field Public

Sai:

```java
public long amount;
```

Đúng:

```java
private final long amount;
```

## 8. Câu Hỏi Tự Kiểm Tra

- Vì sao `amount` dùng `long`, không dùng `Long`?
- Vì sao `currency` có thể `null` còn `amount` thì không?
- `==` khác `.equals()` như thế nào?
- `final` field giúp gì?
- Immutable khác với chỉ có `final` field ở đâu?
- Constructor của `Money` đang bảo vệ invariant nào?
- Vì sao `add` trả object mới?
