# Ordering, Complexity Và Concurrent Collections

## `TreeMap`

`TreeMap` dựa trên balanced tree.

- `get`, `put`, `remove`: O(log n).
- Key được sort theo natural order hoặc `Comparator`.
- Phù hợp khi cần sorted key hoặc range query.

Ví dụ:

```java
Map<LocalDate, List<Task>> tasksByDate = new TreeMap<>();
```

## `TreeSet`

`TreeSet` tương tự `TreeMap` nhưng lưu value unique.

Cần chú ý `Comparator` phải nhất quán với equality theo cách bạn mong muốn.

```java
Set<String> names = new TreeSet<>(String.CASE_INSENSITIVE_ORDER);
```

Case này `"java"` và `"JAVA"` bị coi là trùng theo comparator.

## Big-O Cần Nhớ

| Thao tác | ArrayList | HashSet | HashMap | TreeSet | TreeMap |
| --- | --- | --- | --- | --- | --- |
| get(index) | O(1) | Không có | Không có | Không có | Không có |
| contains(element) | O(n) | Trung bình O(1) | Dùng containsKey/value bên dưới | O(log n) | Dùng containsKey/value bên dưới |
| get(key), containsKey(key) | Không có | Không có | Trung bình O(1) | Không có | O(log n) |
| containsValue(value) | Không có | Không có | O(n) | Không có | O(n) |
| Thêm cuối / add / put | Amortized O(1) | Trung bình O(1) | Amortized trung bình O(1) | O(log n) | O(log n) |
| Xóa theo value / key | O(n) | Trung bình O(1) | Trung bình O(1) theo key | O(log n) | O(log n) theo key |
| Duyệt theo thứ tự sort | Cần tự sort | Không bảo đảm | Không bảo đảm | Có | Có theo key |

Các chi phí trên giả sử hash/equals/comparator có chi phí hằng số và hash phân bố hợp lý. **Map tìm theo key khác với tìm theo value**: `containsValue` phải quét, không được suy từ tốc độ `get(key)`. O(1) không có nghĩa là luôn nhanh hơn trên mọi dữ liệu nhỏ. [HashMap API](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html).

## `ConcurrentHashMap`

`ConcurrentHashMap` dùng cho concurrent access.

Nó không biến mọi flow thành thread-safe. Operation đơn lẻ an toàn, nhưng logic nhiều bước có thể vẫn race.

Sai:

```java
Integer current = counts.get(key);
counts.put(key, current == null ? 1 : current + 1);
```

Tốt hơn:

```java
counts.merge(key, 1, Integer::sum);
```

Hoặc:

```java
cache.computeIfAbsent(key, ignored -> loadValue(key));
```

## Khi Nào Dùng Concurrent Collection

Dùng khi:

- Nhiều thread cùng đọc/ghi.
- Bạn hiểu operation nào atomic.
- Bạn chấp nhận model consistency của collection đó.

Không dùng để:

- Né thiết kế ownership state.
- Thay transaction.
- Bảo vệ invariant nhiều object mà không có lock/coordination.

## Bài Tập Nhanh

Viết `WordCounter`:

- Single-thread version dùng `HashMap`.
- Multi-thread version dùng `ConcurrentHashMap.merge`.
- Tạo input nhiều dòng text.
- So sánh code clarity và correctness.
