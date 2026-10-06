# Type System Trong Java

File này viết theo kiểu mentor giải thích chậm. Bạn không cần đọc hết một lượt. Mỗi phần nên đọc xong rồi tự gõ code trong `java-core-lab`, chạy thử, nhìn lỗi, sửa lỗi.

## Primitive Vs Reference Type

### 1. Ý chính

Java chia type thành hai nhóm rất quan trọng: `primitive type` và `reference type`. `primitive` giữ giá trị đơn giản như số hoặc boolean, còn `reference` giữ tham chiếu tới object trong memory. Nắm chỗ này sẽ giúp bạn hiểu vì sao có cái được `null`, có cái không, và vì sao so sánh object bằng `==` dễ sai.

### 2. Giải thích code

Ví dụ:

```java
package dev.thinh.javacore;

public class TypePlayground {
    public static void main(String[] args) {
        int a = 10;
        int b = 10;
        System.out.println(a == b);

        String x = new String("hello");
        String y = new String("hello");
        System.out.println(x == y);
        System.out.println(x.equals(y));
    }
}
```

Giải thích từng phần:

- `package dev.thinh.javacore;`: file này thuộc package `dev.thinh.javacore`. Package giống namespace để gom class theo nhóm.
- `public class TypePlayground`: khai báo class tên `TypePlayground`. `public` nghĩa là class này có thể được dùng từ package khác.
- `public static void main(String[] args)`: entry point để chạy chương trình Java bằng nút Run trong IDE hoặc bằng command line.
- `int a = 10;`: `int` là primitive type. Biến `a` giữ trực tiếp giá trị `10`.
- `int b = 10;`: biến `b` cũng giữ trực tiếp giá trị `10`.
- `a == b`: với primitive, `==` so sánh giá trị, nên kết quả là `true`.
- `String x = new String("hello");`: `String` là reference type. `x` không giữ trực tiếp toàn bộ object string, nó giữ reference trỏ tới object.
- `String y = new String("hello");`: tạo một object khác có cùng nội dung `"hello"`.
- `x == y`: với reference type, `==` hỏi "hai biến này có trỏ tới cùng một object không?", nên là `false`.
- `x.equals(y)`: `.equals()` hỏi "hai object này có bằng nhau theo logic của class không?", với `String` là so sánh nội dung, nên là `true`.

Output:

```text
true
false
true
```

### 3. Vì sao thiết kế như vậy

Java tách primitive và reference để xử lý dữ liệu đơn giản hiệu quả hơn, đồng thời vẫn có object cho các mô hình phức tạp. Primitive như `int`, `long`, `boolean` không cần object wrapper nên nhẹ hơn và không thể `null`. Reference type thì linh hoạt hơn, có method, có thể dùng trong collection/generic, nhưng phải cẩn thận với `null` và identity.

Ví dụ sai phổ biến:

```java
String currency = new String("USD");

if (currency == "USD") {
    System.out.println("same currency");
} else {
    System.out.println("different currency");
}
```

Bạn có thể kỳ vọng in ra `same currency`, nhưng kết quả thường là:

```text
different currency
```

Lý do: `==` đang so sánh reference, không so sánh nội dung. Cách đúng:

```java
if ("USD".equals(currency)) {
    System.out.println("same currency");
}
```

Viết `"USD".equals(currency)` còn giúp tránh `NullPointerException` nếu `currency` là `null`.

### 4. Liên hệ với Frontend

Trong JavaScript, bạn cũng từng gặp chuyện tương tự:

```javascript
{} === {} // false
```

Hai object literal có nội dung giống nhau nhưng là hai object khác nhau. Java reference type cũng có tinh thần như vậy. Khác biệt là Java buộc bạn rõ ràng hơn: primitive như `int` không phải object, còn `String`, `Money`, `User` là reference type.

Với TypeScript, type như `number` gần với primitive concept hơn, còn object/interface gần với reference concept hơn. Nhưng Java strict hơn nhiều về `null`, method call và generic.

### 5. Khi nào dùng và không dùng

| Tình huống | Nên dùng | Vì sao |
| --- | --- | --- |
| Số bắt buộc có giá trị | `int`, `long`, `double` | Không cần `null`, đơn giản hơn |
| Boolean bắt buộc có giá trị | `boolean` | Tránh `Boolean` bị `null` |
| Text | `String` | Text là object, có nhiều method |
| Domain object | class như `Money`, `UserId` | Cần gom dữ liệu và behavior |
| Giá trị có thể vắng mặt | wrapper hoặc `Optional` tùy context | Cần biểu diễn "không có giá trị" |

Cảnh báo: đừng dùng wrapper như `Long`, `Integer`, `Boolean` chỉ vì nhìn giống object hơn. Nếu field bắt buộc có giá trị, primitive thường rõ hơn.

### 6. Bẫy hay gặp

- Dùng `==` để so sánh `String`.
- Gọi method trên reference có thể `null`.
- Dùng wrapper như `Integer` rồi bị `NullPointerException` khi unboxing.
- Không phân biệt "hai object có cùng nội dung" và "hai biến trỏ tới cùng object".

### 7. Thuật ngữ mới

- `primitive type`: type đơn giản, giữ giá trị trực tiếp, ví dụ `int`, `long`, `boolean`.
- `reference type`: type trỏ tới object, ví dụ `String`, array, class tự định nghĩa.
- `object identity`: danh tính object, tức có phải cùng một object trong memory không.
- `value equality`: bằng nhau theo giá trị hoặc logic, thường qua `.equals()`.
- `null`: trạng thái reference không trỏ tới object nào.

### 8. Bài tập nhỏ để tự gõ lại

Tạo file:

```text
src/main/java/dev/thinh/javacore/TypePlayground.java
```

Tự gõ lại code sau, không copy:

```java
package dev.thinh.javacore;

public class TypePlayground {
    public static void main(String[] args) {
        int a = 10;
        int b = 10;
        System.out.println(a == b);

        String x = new String("hello");
        String y = new String("hello");
        System.out.println(x == y);
        System.out.println(x.equals(y));
    }
}
```

Sau đó đổi `"hello"` thành `"USD"` và tự giải thích vì sao `==` vẫn không nên dùng cho `String`.

## Boxing Và Unboxing

### 1. Ý chính

`boxing` là khi Java chuyển primitive thành object wrapper, ví dụ `int` thành `Integer`. `unboxing` là chiều ngược lại, từ wrapper về primitive. Bạn cần hiểu phần này vì collection/generic trong Java không dùng primitive trực tiếp.

### 2. Giải thích code

```java
package dev.thinh.javacore;

import java.util.ArrayList;
import java.util.List;

public class BoxingPlayground {
    public static void main(String[] args) {
        Integer count = 10;
        int value = count;

        List<Integer> numbers = new ArrayList<>();
        numbers.add(1);
        numbers.add(2);

        int first = numbers.get(0);
        System.out.println(first + value);
    }
}
```

Giải thích:

- `Integer count = 10;`: `10` là `int`, nhưng biến là `Integer`, nên Java tự boxing `int` thành `Integer`.
- `int value = count;`: `count` là `Integer`, nhưng biến là `int`, nên Java tự unboxing.
- `List<Integer>`: generic trong Java không dùng `List<int>`, nên phải dùng wrapper `Integer`.
- `numbers.add(1);`: `1` là `int`, được boxing thành `Integer`.
- `int first = numbers.get(0);`: `numbers.get(0)` trả `Integer`, được unboxing thành `int`.

### 3. Vì sao thiết kế như vậy

Primitive giúp Java chạy hiệu quả, nhưng generic/collection cần làm việc với object. Wrapper type là cầu nối giữa hai thế giới đó.

Ví dụ sai:

```java
Integer value = null;
int number = value;
System.out.println(number);
```

Code compile được, nhưng khi chạy sẽ lỗi:

```text
NullPointerException
```

Lý do: Java cố unbox `null` thành `int`, nhưng `null` không có giá trị số nào để chuyển.

Trong bài `Money`, field này nên là primitive:

```java
private final long amount;
```

Không nên dùng:

```java
private final Long amount;
```

trừ khi business thật sự cho phép amount không có giá trị.

### 4. Liên hệ với Frontend

Trong JavaScript, `number` có thể nằm trong array bình thường:

```javascript
const numbers = [1, 2, 3];
```

Bạn không cần nghĩ tới boxing. Java thì khác: `List<int>` không hợp lệ, nên bạn phải dùng `List<Integer>`. Đây là một trong những chỗ Java verbose hơn JS/TS.

### 5. Khi nào dùng và không dùng

| Trường hợp | Nên dùng | Ghi chú |
| --- | --- | --- |
| Field số bắt buộc có | `int`, `long`, `double` | Rõ ràng, không `null` |
| Generic/Collection | `Integer`, `Long`, `Double` | Vì generic cần reference type |
| Field database có thể null | wrapper như `Long` | Tùy mapping và business rule |
| Boolean bắt buộc | `boolean` | Tránh 3 trạng thái `true/false/null` |

Cảnh báo: wrapper type có thể `null`, nên đừng dùng nếu bạn không cần trạng thái vắng mặt.

### 6. Bẫy hay gặp

- `Integer value = null; int x = value;` gây `NullPointerException`.
- So sánh wrapper bằng `==` thay vì `.equals()` trong một số case.
- Dùng wrapper cho field bắt buộc rồi phải check null khắp nơi.
- Quên rằng `List<int>` không tồn tại.

### 7. Thuật ngữ mới

- `boxing`: chuyển primitive thành wrapper object.
- `unboxing`: chuyển wrapper object thành primitive.
- `wrapper type`: class bọc primitive, ví dụ `Integer`, `Long`, `Boolean`.
- `generic`: cơ chế viết class/method làm việc với type linh hoạt, ví dụ `List<Integer>`.

### 8. Bài tập nhỏ để tự gõ lại

Tạo `BoxingPlayground.java`, thử ba việc:

1. Tạo `Integer count = 10`.
2. Gán `int value = count`.
3. Thử đổi `count = null` rồi chạy lại để thấy lỗi.

Sau khi lỗi xảy ra, ghi lại bằng một câu: lỗi xảy ra ở bước boxing hay unboxing?

## `String` Và Immutability

### 1. Ý chính

`String` trong Java là `immutable`, nghĩa là tạo xong thì nội dung không đổi. Mỗi thao tác như `toUpperCase()` tạo ra string mới thay vì sửa string cũ. Hiểu `String` immutable sẽ giúp bạn hiểu cách thiết kế object an toàn như `Money`.

### 2. Giải thích code

```java
package dev.thinh.javacore;

public class StringPlayground {
    public static void main(String[] args) {
        String name = "Java";
        String upper = name.toUpperCase();

        System.out.println(name);
        System.out.println(upper);
    }
}
```

Giải thích:

- `String name = "Java";`: tạo biến `name` trỏ tới string `"Java"`.
- `name.toUpperCase()`: không sửa object `"Java"` cũ. Nó trả về object string mới.
- `String upper = ...`: biến `upper` trỏ tới string mới `"JAVA"`.
- `System.out.println(name);`: vẫn in `Java`.
- `System.out.println(upper);`: in `JAVA`.

Output:

```text
Java
JAVA
```

### 3. Vì sao thiết kế như vậy

`String` được dùng rất nhiều: key trong map, config, class name, file path, HTTP header, token. Nếu `String` mutable, rất nhiều object có thể bị đổi từ bên ngoài mà không biết.

Ví dụ nếu string mutable, code này sẽ nguy hiểm:

```java
String currency = "USD";
Money money = new Money(100, currency);
```

Nếu caller sửa được nội dung `currency` sau khi tạo `Money`, object `Money` có thể bị đổi state. Vì `String` immutable, `Money` an toàn hơn.

Một bẫy khác: nối chuỗi trong loop lớn.

```java
String result = "";
for (int i = 0; i < 1000; i++) {
    result = result + i;
}
```

Mỗi lần `result + i` có thể tạo string mới. Với loop lớn, dùng:

```java
StringBuilder builder = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    builder.append(i);
}
String result = builder.toString();
```

### 4. Liên hệ với Frontend

Trong React, bạn thường không mutate state trực tiếp:

```javascript
setItems([...items, newItem]);
```

Bạn tạo array mới thay vì sửa array cũ để React dễ nhận biết thay đổi và code dễ dự đoán. Immutable object trong Java cũng có tinh thần giống vậy: thay vì sửa object cũ, tạo object mới với state mới.

### 5. Khi nào dùng và không dùng

| Tình huống | Nên dùng immutable? | Vì sao |
| --- | --- | --- |
| Value object như `Money` | Có | An toàn, dễ test |
| DTO/read model | Có | Dữ liệu rõ ràng, ít side effect |
| Config | Có | Tránh bị đổi runtime ngoài ý muốn |
| Entity có lifecycle phức tạp | Tùy | Có thể mutable hợp lý hơn |
| Collection lớn cần update liên tục | Tùy | Copy nhiều có thể tốn chi phí |

Cảnh báo: immutable không có nghĩa là lúc nào cũng tốt hơn. Nhưng với value object khi học Java core, immutable là lựa chọn rất tốt.

### 6. Bẫy hay gặp

- Nghĩ `toUpperCase()` sửa string cũ.
- Dùng `String` nối trong loop lớn thay vì `StringBuilder`.
- Nghĩ field `final` luôn làm object immutable tuyệt đối.
- Field `final List<String>` vẫn có thể trỏ tới list có nội dung bị sửa.

### 7. Thuật ngữ mới

- `immutable`: object không đổi state sau khi tạo.
- `mutable`: object có thể đổi state sau khi tạo.
- `StringBuilder`: class dùng để build string hiệu quả khi cần append nhiều lần.
- `side effect`: tác động làm thay đổi state bên ngoài hoặc object hiện có.

### 8. Bài tập nhỏ để tự gõ lại

Tạo `StringPlayground.java` và thử:

1. In `name` sau khi gọi `toUpperCase()`.
2. Tự dự đoán output trước khi chạy.
3. Viết loop nối chuỗi bằng `StringBuilder`.

Sau đó trả lời: method `toUpperCase()` mutate string cũ hay trả string mới?

## Package, Class Và Access Modifier

### 1. Ý chính

`package` giúp tổ chức code Java thành namespace. `class` là nơi bạn định nghĩa data và behavior. `access modifier` như `public`, `private`, `protected` quyết định ai được dùng phần nào của class.

### 2. Giải thích code

```java
package dev.thinh.javacore.user;

public final class User {
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

Giải thích:

- `package dev.thinh.javacore.user;`: class nằm trong package `dev.thinh.javacore.user`.
- `public final class User`: class public, và `final` nghĩa là class này không cho subclass kế thừa.
- `private final String id;`: field chỉ dùng bên trong class, và chỉ được gán một lần.
- `private final String email;`: tương tự.
- `public User(...)`: constructor public, bên ngoài có thể tạo `User`.
- `this.id = id;`: gán parameter `id` vào field `id` của object hiện tại.
- `public String id()`: public accessor để đọc id.
- `return id;`: trả field `id`.

Cách gọi:

```java
User user = new User("u1", "a@example.com");
System.out.println(user.id());
System.out.println(user.email());
```

### 3. Vì sao thiết kế như vậy

Field nên để `private` để object tự bảo vệ state. Nếu để field public, bất kỳ code nào cũng sửa được, constructor validation không còn đủ ý nghĩa.

Ví dụ sai:

```java
public class BadMoney {
    public long amount;
    public String currency;
}
```

Code bên ngoài có thể làm:

```java
BadMoney money = new BadMoney();
money.amount = -100;
money.currency = "";
```

Object rơi vào state sai. Với class như `Money`, ta muốn chặn state sai ngay từ constructor và không cho sửa bừa bãi.

### 4. Liên hệ với Frontend

Trong React, component thường nhận `props` và không nên tự ý sửa props:

```javascript
function Price({ amount }) {
  return <span>{amount}</span>;
}
```

Bạn muốn data flow rõ ràng. Trong Java, `private final field + public method đọc` cũng tạo data flow rõ hơn: bên ngoài đọc được thứ cần đọc, nhưng không phá state bên trong.

### 5. Khi nào dùng và không dùng

| Modifier | Khi dùng | Cẩn thận |
| --- | --- | --- |
| `private` | Field/helper method nội bộ | Đây nên là default cho field |
| `public` | API thật sự cho bên ngoài dùng | Public rồi thì khó đổi |
| package-private | Code chỉ dùng trong cùng package | Không ghi modifier |
| `protected` | Cho subclass hoặc cùng package dùng | Dễ làm inheritance phức tạp |
| `final` class | Không muốn subclass thay behavior | Không dùng nếu thật sự cần extension |

Cảnh báo: đừng public mọi thứ chỉ để "gọi cho tiện". Public API càng lớn, càng khó refactor.

### 6. Bẫy hay gặp

- Field để `public`.
- Quên gán field `final` trong constructor.
- Dùng `protected` quá sớm.
- Class đặt sai package nên IDE import lộn.
- Public method quá nhiều làm class khó kiểm soát.

### 7. Thuật ngữ mới

- `package`: namespace tổ chức class.
- `class`: blueprint để tạo object.
- `field`: biến thuộc object/class.
- `constructor`: method đặc biệt chạy khi tạo object.
- `access modifier`: keyword điều khiển phạm vi truy cập.
- `public API`: phần class cho code bên ngoài dùng.

### 8. Bài tập nhỏ để tự gõ lại

Tạo class `UserId`:

- package `dev.thinh.javacore`
- class `final`
- field `private final String value`
- constructor validate `value` không null/blank
- method `public String value()`

Sau đó thử cố sửa `value` từ class khác và xem Java báo lỗi gì.

## Bài Tập Chính: `Money`

### 1. Ý chính

`Money` là bài tập value object đầu tiên. Nó gom `amount` và `currency` vào cùng một object để tránh truyền số tiền và đơn vị tiền rời rạc. Bài này giúp bạn luyện primitive/reference, constructor validation, immutable object và public API nhỏ.

### 2. Giải thích code mẫu

Bạn tự gõ class theo hướng này:

```java
package dev.thinh.javacore;

public final class Money {
    private final long amount;
    private final String currency;

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

    public long amount() {
        return amount;
    }

    public String currency() {
        return currency;
    }

    public Money add(Money other) {
        if (other == null) {
            throw new IllegalArgumentException("other is required");
        }
        if (!this.currency.equals(other.currency)) {
            throw new IllegalArgumentException("currency must match");
        }

        return new Money(this.amount + other.amount, this.currency);
    }
}
```

Giải thích:

- `public final class Money`: không cho subclass override behavior, giúp giữ immutable/invariant dễ hơn.
- `private final long amount`: amount bắt buộc có giá trị, không `null`.
- `private final String currency`: currency là text nên là reference type.
- `if (amount < 0)`: bảo vệ rule không có tiền âm trong bài này.
- `currency == null || currency.isBlank()`: check `null` trước, vì nếu gọi `isBlank()` trên `null` sẽ lỗi.
- `this.amount = amount`: gán parameter vào field.
- `amount()`, `currency()`: accessor public.
- `add(Money other)`: cộng hai object `Money`.
- `other == null`: caller không được truyền `null`.
- `!this.currency.equals(other.currency)`: chỉ cộng cùng currency.
- `return new Money(...)`: immutable object không sửa object cũ, mà trả object mới.

Cách gọi:

```java
Money a = new Money(100, "USD");
Money b = new Money(50, "USD");
Money total = a.add(b);

System.out.println(total.amount());
System.out.println(total.currency());
```

Output:

```text
150
USD
```

### 3. Vì sao thiết kế như vậy

Nếu không có `Money`, bạn có thể viết:

```java
long amount = 100;
String currency = "USD";
```

Nhưng khi truyền qua nhiều method, rất dễ truyền nhầm:

```java
pay(100, "USD");
pay(100, "USDD"); // typo
pay(-100, "USD"); // invalid
```

`Money` gom rule vào một chỗ. Object tạo ra luôn hợp lệ.

Ví dụ sai nếu `add` không check currency:

```java
Money usd = new Money(100, "USD");
Money vnd = new Money(100, "VND");
Money result = usd.add(vnd);
```

Nếu code cho phép cộng, kết quả `200 USD` hoặc `200 VND` đều sai về business. Vì vậy `add` phải reject khác currency.

### 4. Liên hệ với Frontend

Trong TypeScript, bạn có thể tạo type:

```ts
type Money = {
  amount: number;
  currency: string;
}
```

Nhưng type này chỉ mô tả shape. Nó không tự chặn `amount < 0` hoặc currency blank ở runtime.

Trong Java, class `Money` vừa mô tả data, vừa giữ behavior và validation. Nó giống một component nhỏ của domain model, không chỉ là object literal.

### 5. Khi nào dùng và không dùng

| Tình huống | Có nên tạo value object như `Money`? | Lý do |
| --- | --- | --- |
| Field có rule rõ | Có | Gom validation vào một chỗ |
| Hai field luôn đi cùng nhau | Có | Tránh truyền rời rạc |
| Logic domain quan trọng | Có | Method như `add` bảo vệ rule |
| Data tạm trong test nhỏ | Có thể chưa cần | Tránh over-engineering |
| Chỉ in demo hello world | Không cần | Class riêng sẽ quá nặng |

Cảnh báo: value object tốt khi nó bảo vệ rule thật. Đừng tạo class bọc mọi primitive nếu không có behavior/rule nào.

### 6. Bẫy hay gặp

- Quên `final` cho field.
- Quên gán field trong constructor.
- Check `currency.isBlank()` trước `currency == null`.
- So sánh currency bằng `==`.
- `add` sửa object hiện tại thay vì trả object mới.
- Không check currency khi cộng.

### 7. Thuật ngữ mới

- `value object`: object được định nghĩa bởi giá trị, ví dụ hai `Money(100, "USD")` nên được xem là bằng nhau về mặt domain.
- `invariant`: rule luôn phải đúng trong suốt vòng đời object.
- `constructor validation`: validate input khi tạo object.
- `public API`: constructor/method mà bên ngoài class được dùng.
- `domain model`: object mô tả khái niệm nghiệp vụ.

### 8. Bài tập nhỏ để tự gõ lại

Tự gõ lại `Money`, rồi thêm:

```java
public Money subtract(Money other)
public Money multiply(int factor)
```

Yêu cầu:

- `subtract` chỉ cho cùng currency.
- `subtract` không cho kết quả âm.
- `multiply` không cho factor âm.
- Cả hai method trả object mới.

Không dùng AI sinh code. Chỉ dùng AI review sau khi bạn đã tự viết.

## Hướng học tiếp theo

Sau khi làm xong `Money`, học tiếp [02-oop-and-object-contract.md](02-oop-and-object-contract.md). Ở đó bạn sẽ gặp `interface`, `abstract class`, `composition`, `equals/hashCode` và hiểu vì sao `Money` nên có object contract rõ ràng.
