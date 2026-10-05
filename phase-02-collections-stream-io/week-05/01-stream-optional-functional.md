# Stream, Optional Và Functional Interface

## Stream API

Stream phù hợp cho pipeline biến đổi dữ liệu:

```java
List<String> activeEmails = users.stream()
    .filter(User::active)
    .map(User::email)
    .sorted()
    .toList();
```

Tránh Stream khi:

- Logic có nhiều side effect.
- Cần debug từng bước phức tạp.
- Pipeline quá dài và khó đọc.
- Performance/memory đang là vấn đề nhưng chưa đo.

## Operation Hay Dùng

- `filter`: giữ item thỏa điều kiện.
- `map`: biến đổi item.
- `flatMap`: flatten nested collection/stream.
- `sorted`: sort.
- `distinct`: bỏ duplicate.
- `limit`: giới hạn số item.
- `anyMatch`, `allMatch`, `noneMatch`.
- `findFirst`, `findAny`.

## Collector

Các collector hay dùng:

- `toList`
- `toSet`
- `joining`
- `groupingBy`
- `partitioningBy`
- `toMap`

Ví dụ:

```java
Map<String, Long> countByStatus = tasks.stream()
    .collect(Collectors.groupingBy(Task::status, Collectors.counting()));
```

## Optional

`Optional<T>` tốt cho return value có thể vắng mặt.

Không nên:

- Dùng `Optional` cho field entity.
- Dùng `Optional` cho parameter.
- Gọi `.get()` mà không check.

Nên:

```java
return repository.findById(id)
    .map(UserResponse::from)
    .orElseThrow(() -> new UserNotFoundException(id));
```

## Functional Interface

Functional interface có đúng một abstract method.

Hay gặp:

- `Predicate<T>`: T -> boolean.
- `Function<T, R>`: T -> R.
- `Consumer<T>`: T -> void.
- `Supplier<T>`: () -> T.
- `Comparator<T>`.

## Lambda Và Method Reference

```java
users.stream()
    .filter(user -> user.active())
    .map(user -> user.email())
    .toList();
```

Có thể viết gọn:

```java
users.stream()
    .filter(User::active)
    .map(User::email)
    .toList();
```

Ưu tiên cách dễ đọc hơn, không phải cách ngắn nhất.

## Bài Tập Nhanh

Từ danh sách `Order`, tính:

- Tổng revenue.
- Revenue theo user.
- Top 3 order lớn nhất.
- Danh sách email user có order failed.
- Partition order thành paid/unpaid.
