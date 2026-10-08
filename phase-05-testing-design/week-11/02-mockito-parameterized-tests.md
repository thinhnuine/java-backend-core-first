# Mockito Và Parameterized Tests

## Parameterized Test

```java
@ParameterizedTest
@ValueSource(strings = {"", " ", "\t"})
void rejectsBlankCurrency(String currency) {
    assertThatThrownBy(() -> new Money(10, currency))
        .isInstanceOf(IllegalArgumentException.class);
}
```

Dùng khi cùng một behavior cần test nhiều input.

Nguồn input:

- `@ValueSource`.
- `@CsvSource`.
- `@MethodSource`.

## Mockito

Dùng mock khi dependency:

- gọi network/database/file system.
- khó dựng state.
- cần verify interaction thật sự quan trọng.

Đừng mock:

- value object.
- collection.
- logic đơn giản tự tạo được.

## Ví Dụ Mockito

```java
PaymentGateway gateway = mock(PaymentGateway.class);
Money amount = new Money(100, "USD");
OrderService service = new OrderService(gateway);
service.checkout(amount);

verify(gateway).charge(amount);
```

## Mock Vs Fake

Fake là implementation đơn giản tự viết cho test.

Ví dụ:

```java
public class FakeEmailSender implements EmailSender {
    private final List<Email> sent = new ArrayList<>();

    @Override
    public void send(Email email) {
        sent.add(email);
    }
}
```

Fake thường dễ hiểu hơn mock nếu dependency đơn giản.

## Bài Tập Nhanh

Viết test cho `NotificationService`:

- dùng fake sender.
- dùng Mockito mock sender.
- so sánh cách nào dễ đọc hơn.

Ví dụ trên dùng PaymentGateway.charge(Money) trả void từ bài OOP, nên không dùng when(...).thenReturn(...). Đặt snippet trong @Test, có import static Mockito.mock/verify và dependency Mockito; lab JUnit thuần chưa tự có AssertJ/Mockito. Trong Spring project, dùng dependency test do Initializr/Boot quản lý. Với lab Java thuần, thêm dependency test tương ứng và ghi phiên bản đã chọn. ValueSource không cung cấp null; thêm @NullSource hoặc dùng MethodSource cho case null.
