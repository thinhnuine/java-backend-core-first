# JUnit 5 Và AssertJ

Nếu chưa có project chạy test, làm [unit test đầu tiên](../../learning-path/04-first-unit-test.md) trước: có POM, file path, import và lệnh chạy. Trong lịch mới, học JUnit cơ bản ở tuần 3, phần test mở rộng ở tuần 15. Các snippet bên dưới cần class và thư viện tương ứng.

## JUnit 5 Basics

```java
class MoneyTest {
    @Test
    void addsMoneyWithSameCurrency() {
        Money result = new Money(10, "USD").add(new Money(5, "USD"));

        assertEquals(15L, result.amount());
        assertEquals("USD", result.currency());
    }
}
```

Tên test nên mô tả behavior.

## Arrange, Act, Assert

Một test dễ đọc thường có 3 phần:

- Arrange: chuẩn bị input/dependency.
- Act: gọi behavior đang test.
- Assert: kiểm kết quả.

```java
@Test
void rejectsNegativeAmount() {
    assertThatThrownBy(() -> new Money(-1, "USD"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageContaining("amount");
}
```

## AssertJ

AssertJ giúp assertion đọc tự nhiên hơn:

```java
assertThat(users)
    .extracting(User::email)
    .containsExactly("a@example.com", "b@example.com");
```

Hay dùng:

- `isEqualTo`.
- `containsExactly`.
- `containsExactlyInAnyOrder`.
- `extracting`.
- `isEmpty`.
- `hasSize`.
- `assertThatThrownBy`.

## Test Behavior

Test nên kiểm behavior public, không khóa implementation quá chặt.

Ví dụ tốt:

- LRU cache evict đúng key.
- Parser trả AST đúng.
- Service gọi dependency khi cần.

Ví dụ dễ brittle:

- Kiểm private method.
- Kiểm internal list class cụ thể.
- Verify interaction không quan trọng.

## Bài Tập Nhanh

Thêm test cho `Money`, `LruCache` hoặc `LogLineParser`:

- happy path.
- invalid input.
- edge case.
- exception message đủ rõ.
