# Bài Tập Tự Chọn Sau Collections

Không nằm trong checklist lượt đầu. Cần học List/Map, Optional, unit test trước repository; học LinkedHashMap trước LRU. Workflow dưới đây có rule riêng, không thay thế rule Task Manager cơ bản.

## Bài 1: Generic In-Memory Repository

Implement generic repository.

Interface:

```java
public interface Repository<ID, T> {
    void save(ID id, T value);
    Optional<T> findById(ID id);
    List<T> findAll();
    boolean existsById(ID id);
    void deleteById(ID id);
}
```

Implementation:

- `InMemoryRepository<ID, T>`.
- Dùng `HashMap<ID, T>`.
- Không trả mutable internal collection trực tiếp.
- Validate id/value không null.

Test cần có:

- Save rồi find được.
- Find id không tồn tại trả `Optional.empty`.
- Delete id xong không còn tồn tại.
- `findAll` không cho caller sửa state bên trong.

## Bài 2: LRU Cache

Viết `LruCache<K, V>`.

Yêu cầu:

- Constructor nhận `capacity`.
- `put(K key, V value)`.
- `Optional<V> get(K key)`.
- Khi quá capacity, remove item ít được dùng gần đây nhất.
- Không nhận null key.
- Có test cho eviction order.

Gợi ý:

- Cách dễ: dùng `LinkedHashMap`.
- Cách học sâu hơn: tự viết doubly linked list + `HashMap<K, Node<K, V>>`.
- Nếu muốn port thư viện nhỏ cuối giai đoạn, bài này là ứng viên tốt.

## Bài 3: Workflow Status

Dùng enum để mô hình hóa trạng thái task.

Yêu cầu:

- Enum `WorkflowStatus`.
- Method `canMoveTo(WorkflowStatus next)`.
- Method `isTerminal()`.
- Method `label()` trả text hiển thị.
- Test state transition.

Rule gợi ý:

- `TODO -> IN_PROGRESS/CANCELLED`.
- `IN_PROGRESS -> BLOCKED/DONE/CANCELLED`.
- `BLOCKED -> IN_PROGRESS/CANCELLED`.
- `DONE` không chuyển nữa.
- `CANCELLED` không chuyển nữa.

## Learning Log Cuối Tuần

Trả lời:

- Khi nào dùng generic class, khi nào dùng generic method?
- `? extends T` khác `? super T` ở đâu?
- Type erasure ảnh hưởng gì tới runtime?
- Enum Java mạnh hơn enum/string union trong TypeScript ở điểm nào?
- Bạn chọn cách nào cho LRU cache và vì sao?
