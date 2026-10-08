# List, Set Và Map Internals

## `ArrayList`

`ArrayList` backed by array.

- `get(index)`: O(1).
- Add cuối list: amortized O(1), thỉnh thoảng resize.
- Insert/remove ở giữa: O(n), vì phải dịch phần tử.
- Phù hợp cho danh sách đọc nhiều, thêm cuối nhiều.

Điểm cần nhớ:

```java
List<String> names = new ArrayList<>();
names.add("Java");
names.add("Spring");
System.out.println(names.get(0));
```

Nếu biết size lớn từ đầu, có thể set initial capacity:

```java
List<String> values = new ArrayList<>(100_000);
```

## `LinkedList`

`LinkedList` là doubly linked list.

- Thêm/xóa ở đầu/cuối là O(1); thêm/xóa qua `ListIterator` đã đứng đúng vị trí cũng không cần duyệt lại. API `LinkedList` không đưa node reference nội bộ cho caller.
- Truy cập hoặc xóa theo index vẫn cần duyệt tới vị trí đó, nên là O(n).
- Trong thực tế backend, `ArrayList` thường thắng vì cache locality tốt hơn.

Không chọn `LinkedList` chỉ vì "xóa giữa nhanh"; hãy đo hoặc có lý do rõ.

## `HashSet`

`HashSet` dựa trên hash table.

- `contains`, `add`, `remove`: trung bình O(1).
- Không giữ thứ tự.
- Item phải có `equals/hashCode` đúng.

Use case:

- Check duplicate.
- Membership lookup.
- Loại bỏ trùng lặp.

## `HashMap`

`HashMap` lưu key/value theo hash bucket.

- `get`, `put`, `remove`: trung bình O(1).
- Worst case xấu hơn nếu collision nhiều.
- Không giữ thứ tự insertion.
- Key mutable là nguồn bug kinh điển.

Ví dụ key sai:

```java
public final class UserKey {
    private String email;

    // Nếu email đổi sau khi put vào HashMap, map có thể không tìm lại được key.
}
```

Quy tắc:

- Key nên immutable.
- Override `equals/hashCode` nhất quán.
- Không mutate field dùng trong `equals/hashCode`.

## `LinkedHashMap`

`LinkedHashMap` giữ thứ tự insertion hoặc access order.

Nó rất hợp để làm LRU cache đơn giản:

```java
Map<String, Integer> cache = new LinkedHashMap<>(16, 0.75f, true);
```

## Bài Tập Nhanh

Tạo `CollectionChoice.md`, ghi collection bạn chọn cho từng case:

- Danh sách task hiển thị theo thứ tự tạo.
- Check user id có nằm trong danh sách blocked không.
- Lưu config key/value.
- Lấy item theo key và cần giữ thứ tự insert.
- Đếm số lần action xuất hiện trong log.

Nguồn: [LinkedList API Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedList.html).
