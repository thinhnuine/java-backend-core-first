# Mentor Guide Tuần 2: Generics, Enum Và Nested Class

## 1. Ý chính

Tuần này học cách Java biểu diễn type linh hoạt mà vẫn an toàn. `Generics` giúp bạn viết class/method dùng được với nhiều type nhưng compiler vẫn bắt lỗi sớm. `enum` giúp mô hình hóa tập giá trị hữu hạn, còn `nested class` giúp đặt class phụ vào đúng ngữ cảnh.

Nếu đến từ TypeScript, bạn có thể xem generics Java giống `Array<T>` hoặc `Promise<T>`, nhưng Java có thêm giới hạn runtime do `type erasure`.

## 2. Giải thích code

Ví dụ generic repository:

```java
package dev.thinh.javacore;

import java.util.HashMap;
import java.util.Map;
import java.util.Optional;

public final class InMemoryRepository<ID, T> {
    private final Map<ID, T> storage = new HashMap<>();

    public void save(ID id, T value) {
        if (id == null) {
            throw new IllegalArgumentException("id is required");
        }
        if (value == null) {
            throw new IllegalArgumentException("value is required");
        }
        storage.put(id, value);
    }

    public Optional<T> findById(ID id) {
        return Optional.ofNullable(storage.get(id));
    }
}
```

Giải thích:

- `InMemoryRepository<ID, T>`: class có hai type parameter. `ID` là type của id, `T` là type của value.
- `Map<ID, T>`: map có key type là `ID`, value type là `T`.
- `new HashMap<>()`: diamond operator, Java tự suy luận type từ vế trái.
- `save(ID id, T value)`: method nhận đúng type đã chọn khi tạo repository.
- `Optional<T>`: kết quả có thể có hoặc không có value.
- `Optional.ofNullable(...)`: nếu value null thì trả `Optional.empty()`, nếu không null thì trả `Optional` chứa value.

Cách gọi:

```java
InMemoryRepository<String, UserId> users = new InMemoryRepository<>();
users.save("u1", new UserId("u1"));

UserId userId = users.findById("u1")
    .orElseThrow(() -> new IllegalArgumentException("user not found"));
```

Ví dụ enum:

```java
public enum WorkflowStatus {
    TODO,
    IN_PROGRESS,
    DONE;

    public boolean isTerminal() {
        return this == DONE;
    }
}
```

- `enum`: khai báo tập giá trị hữu hạn.
- `TODO`, `IN_PROGRESS`, `DONE`: các instance cố định.
- `this == DONE`: enum có thể so sánh bằng `==` vì mỗi enum constant là singleton.

## 3. Vì sao thiết kế như vậy

Nếu không dùng generics, bạn sẽ phải dùng `Object` và cast thủ công:

```java
Map storage = new HashMap();
storage.put("u1", new UserId("u1"));

String value = (String) storage.get("u1"); // runtime ClassCastException
```

Code compile được nhưng chạy lỗi vì object thật là `UserId`, không phải `String`. Generics chuyển lỗi này thành lỗi compile-time.

Nếu không dùng enum, bạn có thể dùng string:

```java
String status = "done";
```

Vấn đề:

- typo như `"donne"` vẫn compile.
- không biết toàn bộ status hợp lệ.
- logic transition dễ rải rác nhiều nơi.

Enum gom tập giá trị và behavior vào một nơi.

## 4. Liên hệ với Frontend

TypeScript generic:

```ts
type Repository<ID, T> = {
  save(id: ID, value: T): void
  findById(id: ID): T | undefined
}
```

Java generic tương tự ở ý tưởng, nhưng cú pháp verbose hơn và có `type erasure`: runtime không luôn biết `T` là gì.

TypeScript union:

```ts
type Status = "TODO" | "IN_PROGRESS" | "DONE"
```

Java thường dùng enum:

```java
WorkflowStatus status = WorkflowStatus.TODO;
```

Enum Java có thể chứa method, nên mạnh hơn string union ở phần behavior.

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| Generic class | Class lưu/xử lý nhiều type khác nhau | Khi type cố định, không cần linh hoạt |
| Generic method | Chỉ một method cần type linh hoạt | Khi làm signature khó đọc |
| `? extends T` | Collection là producer để đọc ra T | Khi bạn cần add item vào collection |
| `? super T` | Collection là consumer để ghi T vào | Khi bạn cần đọc ra subtype cụ thể |
| Enum | Tập giá trị hữu hạn có ý nghĩa domain | Giá trị đến từ user/config không cố định |
| Static nested class | Class phụ chỉ có nghĩa trong class ngoài | Khi class phụ cần tái sử dụng rộng |

## 6. Bẫy hay gặp

- Dùng `List<Object>` rồi nghĩ có thể thay `List<String>`.
- Không hiểu `? extends` đọc tốt nhưng ghi hạn chế.
- Gọi `Optional.get()` mà không check.
- Dùng string cho status thay vì enum.
- Dùng inner class trong khi static nested class là đủ.
- Cố kiểm tra `instanceof List<String>` dù Java không cho vì type erasure.

## 7. Thuật ngữ mới

- `generic`: cơ chế dùng type parameter như `T`, `ID`.
- `type parameter`: tên type giả định trong class/method generic.
- `wildcard`: dấu `?`, nghĩa là type chưa biết cụ thể.
- `type erasure`: Java xóa nhiều thông tin generic ở runtime.
- `enum`: tập instance cố định, hữu hạn.
- `nested class`: class khai báo bên trong class khác.
- `static nested class`: nested class không giữ reference tới outer object.

## 8. Bài tập nhỏ để tự gõ lại

Tự gõ:

1. `InMemoryRepository<ID, T>` với `save`, `findById`, `existsById`, `deleteById`.
2. `WorkflowStatus` enum có `canMoveTo` và `isTerminal`.
3. `LruCache<K, V>` bản đầu dùng `LinkedHashMap`.

Không dùng AI sinh code. Sau khi viết xong, hãy hỏi AI review với prompt trong `04-checklist-and-ai-review.md`.

Học tiếp theo: đọc lại `01-generics.md`, rồi làm `03-exercises.md`. Nếu wildcard vẫn mơ hồ, chỉ cần nắm PECS ở mức đọc/ghi trước, đừng cố thuộc mọi trường hợp.
