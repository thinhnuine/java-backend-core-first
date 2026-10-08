# Collections: Danh Sách, Tập Hợp Và Tra Theo ID

Học trong tuần 4 mới, trước khi đọc internals. Cần hiểu class, constructor, method và cú pháp List<String> từ [generics](../../phase-01-java-language/week-02/01-generics.md). Mục tiêu lượt đầu là dùng đúng List/Set/Map, chưa cần benchmark hoặc concurrent collection.

## 1. Ý chính

List giữ một dãy phần tử có thứ tự và cho phép trùng. Set giúp biểu diễn tập phần tử không trùng theo equality. Map nối mỗi key với một value để tìm theo key, chẳng hạn tìm title bằng ID của task.

## 2. Giải thích code

Tạo CollectionsDemo.java trong thư mục ví dụ riêng, chưa cần package:

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class CollectionsDemo {
    public static void main(String[] args) {
        List<String> titles = new ArrayList<>();
        titles.add("Học Java");
        titles.add("Học Java");
        System.out.println(titles.size());

        Set<String> uniqueTitles = new HashSet<>(titles);
        System.out.println(uniqueTitles.size());

        Map<Long, String> titlesById = new HashMap<>();
        titlesById.put(1L, "Học Java");
        titlesById.put(2L, "Học SQL");
        System.out.println(titlesById.get(2L));
        System.out.println(titlesById.containsKey(99L));

        titlesById.put(2L, "Học HTTP");
        System.out.println(titlesById.get(2L));
    }
}
```

Chạy javac -encoding UTF-8 CollectionsDemo.java rồi java CollectionsDemo. Output:

```text
2
1
Học SQL
false
Học HTTP
```

Import đưa tên type từ thư viện vào file. List/Set/Map là interface; ArrayList/HashSet/HashMap là implementation dùng ở đây. add thêm phần tử vào list/set; size đếm phần tử. Constructor HashSet(titles) lấy các phần tử và bỏ trùng theo equals/hashCode. Map<Long, String> có key Long và value String; 1L là long được boxing. put cùng key thay value trước đó, không tạo thêm một key giống hệt.

Đoán rồi thử get(99L): kết quả là null. Ở bài này value luôn là String khác null, nên null được dùng để nhận biết không tìm thấy. HashMap thực tế cho phép null value; nếu ứng dụng cho phép điều đó, cần containsKey để phân biệt.

## 3. Vì sao thiết kế như vậy

Giữ hai task có title giống nhau trong List là hợp lệ: title không phải ID. Nếu dùng Set<String> để lưu mọi task chỉ theo title, bạn vô tình mất một task có cùng tên. Nếu dùng Map<String, Task> với title làm key, put title trùng ghi đè task cũ.

Mẫu sai về định danh:

```java
// Snippet đặt trong main: lần ghi thứ hai thay lần thứ nhất.
Map<String, Long> idByTitle = new HashMap<>();
idByTitle.put("Học Java", 1L);
idByTitle.put("Học Java", 2L);
System.out.println(idByTitle.size()); // 1
```

Chọn ID ổn định để tra task; tên hiển thị có thể trùng và có thể đổi.

## 4. Liên hệ với frontend

JS có Array/Set/Map với mục đích tương tự. Khác biệt quan trọng khi dùng object: JS Map/Set thường so object theo identity; Java HashMap/HashSet dựa vào equals/hashCode của key/phần tử. Vì vậy class UserId trong Java cần equality đúng nếu dùng làm key theo giá trị.

## 5. Khi nào dùng và không dùng

| Nhu cầu | Dùng |
| --- | --- |
| Duyệt task theo thứ tự đã lưu trong bộ nhớ | List<Task> |
| Kiểm một ID có nằm trong danh sách bị chặn không | Set<Long> |
| Tìm một task theo ID | Map<Long, Task> |
| Giữ insertion order của map | LinkedHashMap, học sau ví dụ này |

HashMap/HashSet không bảo đảm thứ tự duyệt. Không dùng index trong List làm ID bền vững: xóa phần tử trước làm index của phần tử sau đổi.

## 6. Bẫy hay gặp

Tìm bằng containsValue rồi tưởng nhanh như get(key); mutate key sau put; expose list nội bộ; dùng HashMap cho nhiều request ghi đồng thời mà không có cơ chế bảo vệ. Concurrent access sẽ học ở chặng API, chưa cần để làm demo này.

## 7. Thuật ngữ mới

Collection là nhóm cấu trúc chứa phần tử; Map thuộc Collections Framework nhưng không kế thừa interface Collection. Key là khóa tra cứu; value là dữ liệu tương ứng. Implementation là class thực hiện interface. Duplicate là phần tử lặp theo tiêu chí equality.

## 8. Bài tập nhỏ để tự gõ lại

1. Tạo List có ba title, trong đó hai title giống nhau; dự đoán size khi chuyển thành HashSet.
2. Thêm/xóa một entry map và thử đọc ID không tồn tại.
3. Khi đã có class Task, thay Map<Long, String> bằng Map<Long, Task>; viết save/find/delete cụ thể, chưa cần generic repository.
4. Tự trả lời: vì sao hai task cùng title vẫn phải có hai ID khác nhau?

Đạt khi bạn chọn collection cho ba nhu cầu trên và giải thích được put cùng key. Tiếp theo: [internals](01-list-set-map-internals.md), chỉ đọc ArrayList/HashSet/HashMap trước; [ordering](02-ordering-complexity-concurrent-map.md) khi cần thứ tự sort.
