# Deep Dive Tuần 11: Testing Với JUnit, AssertJ Và Mockito

## Cách Học Tuần Này

Testing không phải để đạt coverage đẹp. Testing là cách bạn khóa behavior để tự tin sửa code. Với Java backend, test tốt giúp bạn refactor service/domain mà không run app thủ công liên tục.

## Test Behavior

Test nên đọc như câu mô tả behavior:

```java
void rejectsNegativeAmount()
void evictsLeastRecentlyUsedEntry()
void returnsEmptyWhenUserNotFound()
```

Tên test kiểu `testAdd` thường thiếu thông tin.

## Arrange, Act, Assert

```java
// arrange
Money usd100 = new Money(100, "USD");
Money usd50 = new Money(50, "USD");

// act
Money result = usd100.add(usd50);

// assert
assertThat(result.amount()).isEqualTo(150);
```

Nếu test khó chia 3 phần, có thể code đang làm quá nhiều.

## AssertJ

AssertJ giúp assertion rõ hơn JUnit assert truyền thống.

```java
assertThat(users)
    .extracting(User::email)
    .containsExactly("a@example.com");
```

Exception:

```java
assertThatThrownBy(() -> new Money(-1, "USD"))
    .isInstanceOf(IllegalArgumentException.class)
    .hasMessageContaining("amount");
```

## Parameterized Test

Khi cùng behavior với nhiều input:

```java
@ParameterizedTest
@ValueSource(strings = {"", " ", "\t"})
void rejectsBlankCurrency(String currency) {
}
```

Nó giảm duplicate mà vẫn rõ.

## Mockito

Mock dependency khó hoặc chậm:

- API client
- database gateway
- email sender
- payment gateway

Đừng mock value object hoặc class quá đơn giản.

Nếu fake dễ viết, fake thường dễ hiểu hơn mock.

## Test Pyramid Nhỏ

Ở giai đoạn này:

- nhiều unit test cho domain class
- một số service test với fake/mock
- Spring phase mới thêm integration test

Đừng bật Spring context cho mọi test nếu không cần.

## Bài Tập Theo Bước

1. Viết test cho `Money`.
2. Viết test cho `LruCache`.
3. Thêm parameterized test cho invalid input.
4. Viết fake sender.
5. Viết cùng test bằng Mockito.
6. So sánh fake vs mock trong learning log.

## Lỗi Thường Gặp

- Test implementation detail.
- Mock quá nhiều.
- Assertion chung chung.
- Test name không nói behavior.
- Test phụ thuộc thứ tự không cần thiết.

## Câu Hỏi Tự Kiểm Tra

- Test fail thì message có dễ hiểu không?
- Test đang bảo vệ behavior nào?
- Mock này có cần thiết không?
- Có edge case null/blank/empty/boundary chưa?
- Refactor code có làm test vẫn pass không?
