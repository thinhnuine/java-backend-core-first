# OOP Và Object Contract

File này giải thích OOP theo hướng thực dụng cho backend Java. Bạn không cần học thuộc định nghĩa hàn lâm. Mục tiêu là biết chọn đúng công cụ: khi nào dùng `interface`, khi nào dùng `abstract class`, vì sao nên ưu tiên composition, và vì sao `equals/hashCode/toString` quan trọng trong Java.

## Interface

### 1. Ý chính

`interface` là một contract: nó nói "class nào implement tôi thì phải có những method này". Interface giúp code phụ thuộc vào capability thay vì phụ thuộc vào class cụ thể. Trong backend, interface rất hữu ích khi bạn có nhiều implementation hoặc muốn test bằng fake/mock.

### 2. Giải thích code

Ví dụ:

```java
package dev.thinh.javacore;

public interface NotificationSender {
    void send(String recipient, String message);
}
```

Giải thích:

- `public interface NotificationSender`: khai báo một interface tên `NotificationSender`.
- `public`: code ở package khác có thể dùng interface này.
- `interface`: keyword để khai báo contract.
- `void send(...)`: class implement interface này phải có method `send`.
- `String recipient`: người nhận.
- `String message`: nội dung cần gửi.
- Method send ở đây không có body nên là public abstract. Interface cũng có thể có default/static/private method; đừng suy rằng mọi method interface đều abstract.

Class implement interface:

```java
package dev.thinh.javacore;

public class EmailNotificationSender implements NotificationSender {
    @Override
    public void send(String recipient, String message) {
        System.out.println("Send email to " + recipient + ": " + message);
    }
}
```

Giải thích:

- `implements NotificationSender`: class này cam kết thực hiện contract của `NotificationSender`.
- `@Override`: báo với compiler rằng method này đang override method từ interface/parent class.
- `public void send(...)`: implementation cụ thể của method `send`.
- Nếu quên method `send`, Java sẽ báo lỗi compile.

Cách gọi:

```java
package dev.thinh.javacore;

public class NotificationDemo {
    public static void main(String[] args) {
        NotificationSender sender = new EmailNotificationSender();
        sender.send("thinh@example.com", "Welcome to Java");
    }
}
```

Output:

```text
Send email to thinh@example.com: Welcome to Java
```

Điểm quan trọng: biến có type là `NotificationSender`, nhưng object thật là `EmailNotificationSender`. Đây là polymorphism ở mức rất thực dụng.

### 3. Vì sao thiết kế như vậy

Nếu service phụ thuộc trực tiếp vào `EmailNotificationSender`, sau này đổi sang SMS/Slack sẽ phải sửa service.

Ví dụ thiết kế cứng:

```java
public class NotificationService {
    private final EmailNotificationSender sender = new EmailNotificationSender();

    public void welcome(String email) {
        sender.send(email, "Welcome");
    }
}
```

Vấn đề:

- Service tự tạo dependency bằng `new`.
- Khó test vì lúc test cũng dùng email sender thật.
- Muốn đổi sang SMS phải sửa class này.

Thiết kế mềm hơn:

```java
public class NotificationService {
    private final NotificationSender sender;

    public NotificationService(NotificationSender sender) {
        this.sender = sender;
    }

    public void welcome(String recipient) {
        sender.send(recipient, "Welcome");
    }
}
```

Bây giờ service không quan tâm gửi bằng email, SMS hay fake sender. Nó chỉ cần một object biết `send`.

### 4. Liên hệ với Frontend

TypeScript interface cũng mô tả shape/capability:

```ts
interface NotificationSender {
  send(recipient: string, message: string): void
}
```

Khác biệt lớn:

| TypeScript | Java |
| --- | --- |
| Structural typing: object có đúng shape là được | Nominal typing: class phải khai báo `implements` |
| Interface biến mất ở runtime | Java interface có vai trò rõ trong type system runtime/class metadata |
| Ít ceremony hơn | Verbose hơn nhưng rõ contract hơn |

Trong React, bạn cũng hay truyền dependency qua props:

```tsx
<NotificationButton sender={emailSender} />
```

Java constructor injection cũng có tinh thần tương tự: dependency được đưa từ ngoài vào.

### 5. Khi nào dùng và không dùng

| Trường hợp | Có nên dùng interface? | Lý do |
| --- | --- | --- |
| Có nhiều implementation | Có | Email/SMS/Slack/Fake |
| Cần test bằng fake/mock | Có | Tách service khỏi dependency thật |
| Dependency gọi hệ thống ngoài | Có | Dễ thay thế trong test |
| Chỉ có một class nhỏ, chưa có biến thể | Chưa cần | Tránh over-engineering |
| Value object như `Money` | Thường không | Không cần abstraction |

Cảnh báo: đừng tạo interface cho mọi class chỉ vì "best practice". Interface tốt khi nó làm code dễ thay đổi hoặc dễ test hơn.

### 6. Bẫy hay gặp

- Tạo interface chỉ có một implementation và không có nhu cầu test/thay thế.
- Interface quá to, bắt class implement nhiều method không cần.
- Đặt tên interface chung chung như `Manager`, `Handler`, `Processor` nhưng không rõ contract.
- Quên `@Override`, làm sai signature mà không nhận ra ngay.
- Service vẫn tự `new EmailNotificationSender()` bên trong, làm interface mất tác dụng.

### 7. Thuật ngữ mới

- `interface`: contract mà class có thể implement.
- `implements`: keyword nói class thực hiện interface.
- `@Override`: annotation báo method đang override method từ interface/parent.
- `polymorphism`: cùng một interface nhưng nhiều implementation khác nhau.
- `dependency`: object/class mà class hiện tại cần dùng để làm việc.
- `fake`: implementation đơn giản dùng trong test.

### 8. Bài tập nhỏ để tự gõ lại

Tạo các file:

```text
NotificationSender.java
EmailNotificationSender.java
SmsNotificationSender.java
NotificationService.java
NotificationDemo.java
```

Yêu cầu:

- `NotificationSender` có method `send`.
- `EmailNotificationSender` và `SmsNotificationSender` implement interface.
- `NotificationService` nhận `NotificationSender` qua constructor.
- `NotificationDemo` tạo service với email sender, rồi đổi sang SMS sender.

Không dùng AI sinh code. Sau khi tự viết, hãy tự trả lời: đổi từ email sang SMS có phải sửa `NotificationService` không?

## Abstract Class

### 1. Ý chính

`abstract class` là class chưa hoàn chỉnh: nó có thể chứa code dùng chung, nhưng vẫn để subclass tự implement một vài phần. Nó hợp khi nhiều class có chung một flow hoặc state. Nếu bạn chỉ cần contract, thường dùng `interface` là đủ.

### 2. Giải thích code

Ví dụ:

```java
package dev.thinh.javacore;

public abstract class FileImporter {
    public final void importFile(String path) {
        validate(path);
        parse(path);
    }

    private void validate(String path) {
        if (path == null || path.isBlank()) {
            throw new IllegalArgumentException("path is required");
        }
    }

    protected abstract void parse(String path);
}
```

Giải thích:

- `public abstract class FileImporter`: class abstract, không tạo object trực tiếp bằng `new FileImporter()`.
- `public final void importFile(...)`: method public cho bên ngoài gọi. `final` nghĩa là subclass không được override method này.
- `validate(path)`: step dùng chung cho mọi importer.
- `parse(path)`: step để subclass tự định nghĩa.
- `private void validate(...)`: chỉ class này dùng được, subclass không gọi trực tiếp được.
- `protected abstract void parse(...)`: subclass bắt buộc implement method này. `protected` nghĩa là subclass thấy được.

Subclass:

```java
package dev.thinh.javacore;

public class CsvFileImporter extends FileImporter {
    @Override
    protected void parse(String path) {
        System.out.println("Parse CSV file: " + path);
    }
}
```

Cách gọi:

```java
FileImporter importer = new CsvFileImporter();
importer.importFile("users.csv");
```

Output:

```text
Parse CSV file: users.csv
```

### 3. Vì sao thiết kế như vậy

Ở đây ta muốn giữ flow cố định:

```text
validate -> parse
```

Subclass chỉ được thay phần `parse`, không được bỏ qua `validate`. Vì vậy `importFile` được đánh dấu `final`.

Nếu không dùng `final`, subclass có thể phá flow:

```java
public class BadCsvImporter extends FileImporter {
    @Override
    public void importFile(String path) {
        parse(path); // bỏ qua validate
    }

    @Override
    protected void parse(String path) {
        System.out.println("Parse " + path);
    }
}
```

Với code trên, path null/blank có thể lọt qua. Thiết kế ban đầu dùng `final` để bảo vệ invariant của flow.

Lưu ý: ví dụ `BadCsvImporter` sẽ không compile nếu `importFile` trong parent là `final`. Đó chính là điều ta muốn.

### 4. Liên hệ với Frontend

Trong React, bạn ít dùng inheritance để share behavior. Bạn thường dùng composition hoặc hook:

```tsx
function UserTable() {
  const users = useUsers()
}
```

Java cũng ngày càng ưu tiên composition. Abstract class vẫn có chỗ dùng, nhưng không phải lựa chọn đầu tiên cho mọi reuse.

Nếu so với frontend, abstract class hơi giống một "base component" có flow chung và component con chỉ override một phần. Nhưng trong React hiện đại, base component bằng inheritance thường không được ưa chuộng. Java cũng nên cẩn thận tương tự.

### 5. Khi nào dùng và không dùng

| Trường hợp | Có nên dùng abstract class? | Lý do |
| --- | --- | --- |
| Có template flow chung | Có thể | `validate -> parse -> save` |
| Có state/logic chung thật sự | Có thể | Nhưng cần cẩn thận |
| Chỉ cần contract method | Không, dùng interface | Nhẹ và linh hoạt hơn |
| Muốn reuse vài dòng code | Thường không | Composition/helper có thể tốt hơn |
| Hierarchy chưa rõ | Không | Dễ tạo kế thừa cứng |

Cảnh báo: abstract class dễ làm code cứng nếu business thay đổi. Dùng khi flow chung thật sự ổn định.

### 6. Bẫy hay gặp

- Dùng abstract class chỉ vì muốn reuse code.
- Cho subclass override method quan trọng làm hỏng flow.
- Dùng `protected` quá nhiều, làm subclass phụ thuộc internal detail.
- Tạo inheritance hierarchy sâu nhiều tầng.
- Nhầm "có chung vài method" với "có cùng bản chất".

### 7. Thuật ngữ mới

- `abstract class`: class chưa hoàn chỉnh, không instantiate trực tiếp.
- `abstract method`: method chưa có body, subclass phải implement.
- `protected`: access modifier cho phép subclass và cùng package truy cập.
- `template method`: pattern giữ flow chung ở parent, cho subclass thay một vài bước.
- `inheritance hierarchy`: cây kế thừa giữa parent/subclass.

### 8. Bài tập nhỏ để tự gõ lại

Tạo:

```text
FileImporter.java
CsvFileImporter.java
JsonFileImporter.java
FileImporterDemo.java
```

Yêu cầu:

- `FileImporter#importFile` validate path rồi gọi `parse`.
- `CsvFileImporter` in `"Parse CSV"`.
- `JsonFileImporter` in `"Parse JSON"`.
- Thử truyền path blank và xem exception.

Sau đó tự trả lời: nếu bỏ `final` ở `importFile`, subclass có thể phá rule nào?

## Composition Vs Inheritance

### 1. Ý chính

`inheritance` là quan hệ "A là một loại của B". `composition` là quan hệ "A có B để làm việc". Trong Java backend hiện đại, bạn thường nên ưu tiên composition vì nó linh hoạt hơn và ít làm class bị trói vào hierarchy cứng.

### 2. Giải thích code

Ví dụ composition:

```java
package dev.thinh.javacore;

public class OrderService {
    private final PaymentGateway paymentGateway;

    public OrderService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public void checkout(Money amount) {
        paymentGateway.charge(amount);
    }
}
```

Giải thích:

- `private final PaymentGateway paymentGateway`: `OrderService` có một dependency là `PaymentGateway`.
- Constructor nhận `PaymentGateway`: dependency được đưa từ ngoài vào.
- `checkout(Money amount)`: service xử lý checkout.
- `paymentGateway.charge(amount)`: service ủy quyền việc thu tiền cho gateway.

Interface đi kèm:

```java
package dev.thinh.javacore;

public interface PaymentGateway {
    void charge(Money amount);
}
```

Implementation:

```java
package dev.thinh.javacore;

public class ConsolePaymentGateway implements PaymentGateway {
    @Override
    public void charge(Money amount) {
        System.out.println("Charge " + amount.amount() + " " + amount.currency());
    }
}
```

Cách gọi:

```java
PaymentGateway gateway = new ConsolePaymentGateway();
OrderService service = new OrderService(gateway);
service.checkout(new Money(100, "USD"));
```

### 3. Vì sao thiết kế như vậy

`OrderService` không phải là `PaymentGateway`. Nó chỉ dùng `PaymentGateway`. Vì vậy composition đúng hơn inheritance.

Ví dụ inheritance sai:

```java
public class OrderService extends ConsolePaymentGateway {
    public void checkout(Money amount) {
        charge(amount);
    }
}
```

Vấn đề:

- `OrderService` bị gắn cứng với console gateway.
- Không đổi được sang Stripe/Paypal/Fake dễ dàng.
- Quan hệ "OrderService là PaymentGateway" không đúng về domain.

Composition giúp thay dependency dễ:

```java
OrderService service = new OrderService(new FakePaymentGateway());
```

### 4. Liên hệ với Frontend

React hiện đại cũng ưu tiên composition hơn inheritance. Bạn thường compose component:

```tsx
<Page>
  <Header />
  <Content />
</Page>
```

Bạn ít khi viết `class AdminPage extends Page`. Tư duy trong Java backend cũng tương tự: ghép object bằng dependency thay vì tạo cây kế thừa cứng.

### 5. Khi nào dùng và không dùng

| Lựa chọn | Khi dùng | Khi tránh |
| --- | --- | --- |
| Composition | Khi class cần dùng capability của object khác | Hầu như là default tốt |
| Inheritance | Khi quan hệ `is-a` rõ và ổn định | Khi chỉ muốn reuse code |
| Interface + composition | Khi cần thay implementation/test fake | Khi abstraction chưa có giá trị |
| Abstract class | Khi có flow chung ổn định | Khi hierarchy chưa rõ |

Cảnh báo: nếu bạn nói "class A extends B để dùng lại method", hãy dừng lại và nghĩ tới composition trước.

### 6. Bẫy hay gặp

- Dùng `extends` chỉ để reuse vài method.
- Tạo parent class quá nhiều responsibility.
- Subclass override làm phá behavior parent.
- Quan hệ domain không thật sự là `is-a`.
- Không inject dependency, mà tự `new` dependency trong service.

### 7. Thuật ngữ mới

- `inheritance`: kế thừa, class con nhận behavior/state từ class cha.
- `composition`: class chứa hoặc dùng object khác để làm việc.
- `is-a`: quan hệ "là một loại của".
- `has-a`: quan hệ "có một".
- `dependency injection`: đưa dependency từ ngoài vào thay vì tự tạo bên trong.

### 8. Bài tập nhỏ để tự gõ lại

Tạo `PaymentGateway`, `ConsolePaymentGateway`, `OrderService`, `OrderDemo`.

Yêu cầu:

- `OrderService` không được `extends ConsolePaymentGateway`.
- `OrderService` nhận `PaymentGateway` qua constructor.
- Demo gọi `checkout(new Money(100, "USD"))`.

Sau đó tạo `FakePaymentGateway` chỉ in `"fake charge"` và thay vào `OrderService` mà không sửa `OrderService`.

## `equals`, `hashCode` Và `toString`

### 1. Ý chính

Trong Java, nếu object cần so sánh theo giá trị hoặc dùng làm key trong `HashMap`/`HashSet`, bạn phải hiểu `equals` và `hashCode`. `toString` giúp object dễ debug hơn. Ba method này thường được gọi là một phần của object contract.

### 2. Giải thích code

Ví dụ value object `UserId`:

```java
package dev.thinh.javacore;

import java.util.Objects;

public final class UserId {
    private final String value;

    public UserId(String value) {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("value is required");
        }
        this.value = value;
    }

    public String value() {
        return value;
    }

    @Override
    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }
        if (!(other instanceof UserId userId)) {
            return false;
        }
        return Objects.equals(value, userId.value);
    }

    @Override
    public int hashCode() {
        return Objects.hash(value);
    }

    @Override
    public String toString() {
        return "UserId[value=" + value + "]";
    }
}
```

Giải thích:

- `import java.util.Objects;`: dùng helper `Objects.equals` và `Objects.hash`.
- `public final class UserId`: value object không cho subclass để giữ equality đơn giản.
- `private final String value`: giá trị bên trong.
- Constructor validate `value`.
- `@Override`: method đang override method có sẵn từ `Object`.
- `equals(Object other)`: mọi `equals` nhận `Object`, không nhận `UserId`, vì nó override method từ `Object`.
- `if (this == other)`: nếu cùng object trong memory, chắc chắn bằng nhau.
- `other instanceof UserId userId`: pattern matching, vừa check type vừa tạo biến `userId`.
- `Objects.equals(value, userId.value)`: so sánh value an toàn với null.
- `hashCode()`: trả hash dựa trên value.
- `toString()`: trả text dễ đọc khi log/debug.

Cách gọi:

```java
UserId a = new UserId("u1");
UserId b = new UserId("u1");

System.out.println(a == b);
System.out.println(a.equals(b));
System.out.println(a);
```

Output:

```text
false
true
UserId[value=u1]
```

### 3. Vì sao thiết kế như vậy

`HashSet` và `HashMap` dùng `hashCode` để tìm bucket, rồi dùng `equals` để xác nhận object có bằng nhau không.

Nếu override `equals` mà quên `hashCode`, bug sẽ rất khó chịu.

Ví dụ sai:

```java
public final class BadUserId {
    private final String value;

    public BadUserId(String value) {
        this.value = value;
    }

    @Override
    public boolean equals(Object other) {
        if (!(other instanceof BadUserId badUserId)) {
            return false;
        }
        return value.equals(badUserId.value);
    }
}
```

Test:

```java
Set<BadUserId> ids = new HashSet<>();
ids.add(new BadUserId("u1"));

System.out.println(ids.contains(new BadUserId("u1")));
```

Bạn kỳ vọng `true`, nhưng có thể nhận `false`, vì `hashCode` vẫn là identity-based từ `Object`.

### 4. Liên hệ với Frontend

Trong JavaScript:

```javascript
{ id: "u1" } === { id: "u1" } // false
```

Muốn so sánh theo value, bạn phải tự viết logic hoặc dùng thư viện. Java cũng vậy: object mặc định so sánh identity. Muốn so sánh value, override `equals`.

React cũng quan tâm identity khi so sánh dependency array, memo, props reference. Java `equals/hashCode` là một hệ thống rõ ràng hơn cho chuyện equality, đặc biệt trong collection.

### 5. Khi nào dùng và không dùng

| Object | Nên override `equals/hashCode`? | Lý do |
| --- | --- | --- |
| Value object như `UserId`, `Money` | Có | Bằng nhau theo value |
| Key dùng trong `HashMap` | Có | Collection cần hash đúng |
| DTO record | Record tự sinh | Thường đủ |
| Entity JPA | Cẩn thận | Có rule riêng, học sau |
| Service class | Không | Service thường so sánh identity |

Cảnh báo: nếu object mutable và field dùng trong `hashCode` bị đổi sau khi đưa vào `HashSet`, collection có thể hỏng behavior.

### 6. Bẫy hay gặp

- Override `equals` nhưng quên `hashCode`.
- Dùng field mutable trong `hashCode`.
- Viết `equals(UserId other)` thay vì `equals(Object other)`, khiến không override đúng.
- `toString` in ra secret như password/token.
- Dùng `==` thay vì `.equals()` cho value object.

### 7. Thuật ngữ mới

- `object contract`: các rule mà object nên tuân thủ, ví dụ equality/hash/debug text.
- `equals`: method so sánh object theo logic.
- `hashCode`: số hash dùng bởi hash-based collection.
- `HashMap`: map dựa trên hash của key.
- `HashSet`: set dựa trên hash của item.
- `identity-based`: dựa trên cùng object trong memory.

### 8. Bài tập nhỏ để tự gõ lại

Tạo `ProductId`:

- field `private final String value`
- constructor validate not null/blank
- accessor `value()`
- override `equals`
- override `hashCode`
- override `toString`

Sau đó tạo `HashSet<ProductId>`, add `new ProductId("p1")`, rồi check `contains(new ProductId("p1"))`.

## Immutability

### 1. Ý chính

Immutable object là object không đổi state sau khi tạo. Trong Java backend, immutable value object giúp code dễ hiểu, dễ test và an toàn hơn khi truyền qua nhiều layer. `Money`, `UserId`, `ProductId` là các ứng viên rất tốt để immutable.

### 2. Giải thích code

Ví dụ:

```java
package dev.thinh.javacore;

public final class Counter {
    private final int value;

    public Counter(int value) {
        this.value = value;
    }

    public Counter increment() {
        return new Counter(value + 1);
    }

    public int value() {
        return value;
    }
}
```

Giải thích:

- `public final class Counter`: không cho subclass thay behavior.
- `private final int value`: field private và chỉ gán một lần.
- Constructor gán state ban đầu.
- `increment()`: không sửa `this.value`, mà trả object `Counter` mới.
- `value()`: accessor để đọc value.

Cách gọi:

```java
Counter a = new Counter(1);
Counter b = a.increment();

System.out.println(a.value());
System.out.println(b.value());
```

Output:

```text
1
2
```

`a` không đổi. `b` là object mới.

### 3. Vì sao thiết kế như vậy

Mutable object dễ gây side effect khó thấy.

Ví dụ mutable:

```java
public class MutableCounter {
    private int value;

    public void increment() {
        value++;
    }

    public int value() {
        return value;
    }
}
```

Nếu nhiều nơi giữ cùng một `MutableCounter`, một nơi gọi `increment` thì nơi khác thấy value đổi. Có lúc đó là điều bạn muốn, nhưng với value object thường không.

Immutable giúp bạn tự tin hơn:

```java
Money total = price.add(tax);
```

Bạn biết `price` không bị đổi sau khi gọi `add`.

### 4. Liên hệ với Frontend

Trong React, bạn thường update state theo kiểu immutable:

```javascript
setItems([...items, newItem])
```

Bạn không làm:

```javascript
items.push(newItem)
setItems(items)
```

Vì mutation làm state khó theo dõi và React có thể không nhận ra thay đổi. Java immutable value object cũng giúp data flow dễ đoán như vậy.

### 5. Khi nào dùng và không dùng

| Trường hợp | Nên immutable? | Lý do |
| --- | --- | --- |
| Value object | Có | Dễ so sánh, dễ test |
| DTO/read model | Có | Dữ liệu ổn định |
| Config | Có | Tránh bị đổi runtime |
| Entity có lifecycle | Tùy | Có thể cần mutable |
| Object chứa collection lớn | Tùy | Copy nhiều có thể tốn |

Cảnh báo: `final` field chưa đủ nếu field trỏ tới object mutable.

Ví dụ:

```java
public final class BadTags {
    private final List<String> values;

    public BadTags(List<String> values) {
        this.values = values;
    }

    public List<String> values() {
        return values;
    }
}
```

Caller vẫn sửa được list bên trong. Cách tốt hơn:

```java
public final class Tags {
    private final List<String> values;

    public Tags(List<String> values) {
        this.values = List.copyOf(values);
    }

    public List<String> values() {
        return values;
    }
}
```

### 6. Bẫy hay gặp

- Nghĩ `final List<T>` nghĩa là list immutable.
- Có setter trong class muốn immutable.
- Method như `add` sửa object cũ thay vì trả object mới.
- Không defensive copy collection.
- Subclass có thể phá invariant vì class không `final`.

### 7. Thuật ngữ mới

- `immutable`: object không đổi state sau khi tạo.
- `mutable`: object có thể đổi state.
- `defensive copy`: copy input/output để bên ngoài không sửa state nội bộ.
- `state`: dữ liệu hiện tại của object.
- `side effect`: thay đổi state ngoài ý muốn hoặc khó thấy.

### 8. Bài tập nhỏ để tự gõ lại

Tạo `Tags`:

- field `private final List<String> values`
- constructor dùng `List.copyOf(values)`
- method `values()` trả list
- thử truyền một `ArrayList`, tạo `Tags`, rồi sửa `ArrayList` gốc
- kiểm tra `Tags` có bị đổi không

Sau đó thử gọi `tags.values().add("new")` và xem Java báo lỗi runtime gì.

## Bài Tập Chính: ShoppingCart

### 1. Ý chính

`ShoppingCart` là bài tập gom nhiều khái niệm: value object, composition, immutable object, interface và object contract. Bạn không cần làm bản production. Mục tiêu là tập thiết kế domain nhỏ sao cho object tự bảo vệ rule của nó.

### 2. Giải thích thiết kế gợi ý

Các class/interface nên có:

```text
ProductId
CartItem
ShoppingCart
DiscountPolicy
```

Các block field dưới đây là phác thảo, **chưa compile được**: bạn phải thêm constructor để khởi tạo final field, import List và các method theo yêu cầu. Không copy chúng như class đã hoàn chỉnh.

`ProductId` là value object:

```java
public final class ProductId {
    private final String value;
}
```

`CartItem` có product, quantity và unit price:

```java
public final class CartItem {
    private final ProductId productId;
    private final String name;
    private final int quantity;
    private final Money unitPrice;
}
```

`ShoppingCart` có nhiều item:

```java
public final class ShoppingCart {
    private final List<CartItem> items;
}
```

`DiscountPolicy` là interface:

```java
public interface DiscountPolicy {
    Money discountFor(ShoppingCart cart);
}
```

### 3. Vì sao thiết kế như vậy

Không nên để cart chỉ là:

```java
List<String> productIds = new ArrayList<>();
```

Vì bạn sẽ mất rule:

- product id có hợp lệ không?
- quantity có > 0 không?
- unit price là currency gì?
- cart có expose mutable list không?

Tạo object riêng giúp rule nằm gần data.

Ví dụ sai:

```java
CartItem item = new CartItem(productId, "Keyboard", -2, price);
```

Nếu constructor không validate quantity, cart có thể chứa số lượng âm. Sau này tính total sẽ sai.

### 4. Liên hệ với Frontend

Trong frontend, bạn có thể có state:

```ts
type CartItem = {
  productId: string
  name: string
  quantity: number
  unitPrice: Money
}
```

Nhưng TypeScript chỉ giúp compile-time. Java domain object có thể chặn runtime invalid state trong constructor. Đây là điểm backend domain modeling thường nhấn mạnh hơn frontend state shape.

### 5. Khi nào dùng và không dùng

| Thiết kế | Khi dùng | Khi tránh |
| --- | --- | --- |
| `ProductId` value object | ID có rule/ý nghĩa domain | Demo quá nhỏ không cần |
| Immutable `CartItem` | Item không nên đổi tùy tiện | Nếu cần lifecycle mutable phức tạp |
| Immutable `ShoppingCart` | Muốn data flow dễ đoán | Cart cực lớn, update liên tục |
| `DiscountPolicy` interface | Có nhiều policy | Chỉ có một phép trừ fixed đơn giản |

Cảnh báo: bài này để học design, nhưng đừng biến nó thành ecommerce platform mini.

### 6. Bẫy hay gặp

- `ShoppingCart` trả internal mutable list ra ngoài.
- `CartItem` cho quantity âm hoặc 0.
- `Money` khác currency nhưng vẫn cộng vào total.
- Tạo interface quá sớm cho mọi class.
- Quên `equals/hashCode` cho value object như `ProductId`.

### 7. Thuật ngữ mới

- `domain object`: object mô tả khái niệm nghiệp vụ.
- `value object`: object được định nghĩa bởi giá trị.
- `policy`: object/interface đại diện một rule có thể thay đổi.
- `total`: tổng tiền của cart.
- `invariant`: rule luôn đúng, ví dụ quantity > 0.

### 8. Bài tập nhỏ để tự gõ lại

Làm theo thứ tự:

1. Tạo `ProductId` có validation và `equals/hashCode/toString`.
2. Tạo `CartItem` immutable, validate quantity > 0.
3. Tạo `ShoppingCart` immutable, dùng `List.copyOf`.
4. Thêm method `addItem(CartItem item)` trả cart mới.
5. Thêm method `items()` không cho caller sửa internal state.
6. Tạo `DiscountPolicy` interface.

Sau khi xong, tự trả lời:

- Class nào là value object?
- Class nào dùng composition?
- Có chỗ nào expose mutable state không?
- Nếu quantity âm, lỗi được chặn ở đâu?

## Hướng học tiếp theo

Sau file này, bạn nên quay lại code `Money` và thêm `equals/hashCode/toString`. Tiếp theo học [03-exercises.md](03-exercises.md), làm bài `Money`, `Notification Sender`, rồi mới sang `ShoppingCart`.

List.copyOf chỉ bảo vệ cấu trúc list; với Tags chứa String immutable thì phù hợp. Nếu list chứa object mutable, caller vẫn có thể sửa object đó qua reference. Muốn toàn bộ model immutable phải bảo vệ cả phần tử.
