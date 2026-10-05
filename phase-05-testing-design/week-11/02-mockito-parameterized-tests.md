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
when(gateway.charge(any())).thenReturn(new PaymentResult.Success("tx1"));

OrderService service = new OrderService(gateway);
service.checkout(order);

verify(gateway).charge(any());
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
