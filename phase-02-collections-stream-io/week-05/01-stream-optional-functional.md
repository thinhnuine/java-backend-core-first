# Stream Và Optional: Đọc Một Pipeline Từng Bước

Học tuần 5 mới. Cần List, vòng for, method và [exception](../../phase-01-java-language/week-03/02-exception-handling.md). Dành khoảng một buổi cho ví dụ đầu; collector/flatMap và xử lý file nằm ở phần đọc sau.

## 1. Ý chính

Stream mô tả các bước xử lý phần tử: chọn phần tử cần, đổi thành kết quả rồi gom lại. Optional biểu diễn một kết quả có thể có hoặc không có, chẳng hạn kết quả tìm task. Stream không phải nơi lưu dữ liệu; nó đọc từ nguồn như List.

## 2. Giải thích code

Tạo StreamDemo.java trong thư mục ví dụ riêng:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class StreamDemo {
    public static void main(String[] args) {
        List<String> titles = List.of("Java", "SQL", "Spring");

        List<String> withLoop = new ArrayList<>();
        for (String title : titles) {
            if (title.length() > 3) {
                withLoop.add(title.toUpperCase());
            }
        }

        List<String> withStream = titles.stream()
            .filter(title -> title.length() > 3)
            .map(title -> title.toUpperCase())
            .toList();

        System.out.println(withLoop);
        System.out.println(withStream);

        Optional<String> firstLongTitle = titles.stream()
            .filter(title -> title.length() > 10)
            .findFirst();
        System.out.println(firstLongTitle.orElse("Không tìm thấy"));
    }
}
```

Chạy javac -encoding UTF-8 StreamDemo.java rồi java StreamDemo. Kết quả:

```text
[JAVA, SPRING]
[JAVA, SPRING]
Không tìm thấy
```

List.of tạo list không sửa được cấu trúc, phù hợp dữ liệu mẫu. Với loop, if chọn title dài hơn 3, add gom title đã đổi hoa. Với Stream, filter làm việc chọn đó; map biến String thành String mới; toList là terminal operation gom kết quả. `title -> ...` là lambda nhận title và trả kết quả: filter cần boolean, map cần giá trị mới.

findFirst trả Optional<String>: có phần tử thì có giá trị; không có thì Optional.empty. orElse chọn text dự phòng cho case rỗng. Đổi điều kiện > 10 thành > 3 để thấy nó chọn Java.

Khi quen lambda, `.map(String::toUpperCase)` tương ứng `.map(title -> title.toUpperCase())`. `::` là method reference, không phải đang gọi method ngay lúc khai báo pipeline.

## 3. Vì sao thiết kế như vậy

Stream là lazy: filter/map chỉ bắt đầu được thực hiện khi có terminal operation. Một stream chỉ nên được tiêu thụ một lần; muốn xử lý lại, tạo stream mới từ nguồn.

Ví dụ sai, snippet đặt trong main:

```java
Optional<String> missing = Optional.empty();
System.out.println(missing.get());
```

Đoạn này compile rồi ném NoSuchElementException. Optional không tự ép bạn xử lý an toàn; chọn orElse/orElseThrow/map theo nhu cầu. Nếu không tìm thấy task là lỗi theo contract, orElseThrow cung cấp exception phù hợp; danh sách chưa có task thường nên là list rỗng.

## 4. Liên hệ với frontend

Lambda gần callback JS; Stream filter/map gần Array.filter/map về ý nghĩa từng bước. Khác biệt là JS Array.filter/map thực hiện ngay và tạo array trung gian, còn Stream xử lý lazy qua terminal operation. Đừng dùng side effect trong map để gửi email/cập nhật DB rồi suy rằng pipeline luôn chạy mọi bước như lệnh tuần tự.

## 5. Khi nào dùng và không dùng

Dùng Stream cho phép lọc/biến đổi rõ ràng; loop cho flow có break, xử lý lỗi từng phần hoặc nhiều side effect. Optional phù hợp return value có thể thiếu; chưa dùng làm entity field hoặc request parameter trong khóa.

Stream.toList trả list không sửa được cấu trúc. Collectors.toList không có cam kết list mutable/implementation cụ thể; nếu cần ArrayList mutable rõ ràng, dùng Collectors.toCollection(ArrayList::new). Chưa cần parallelStream để hoàn thành bài.

## 6. Bẫy hay gặp

Gọi get trên Optional rỗng; dùng Optional.of(null) thay ofNullable; thiếu terminal operation; tái sử dụng stream đã tiêu thụ; gọi withStream.add rồi gặp UnsupportedOperationException. orElse tính đối số ngay, còn orElseGet nhận Supplier tính khi cần; chỉ cần quan tâm khác biệt khi fallback tốn chi phí hoặc có side effect.

## 7. Thuật ngữ mới

Pipeline là chuỗi bước xử lý. Intermediate operation như filter/map trả Stream để ghép tiếp; terminal operation như toList/findFirst kết thúc lượt xử lý. Predicate là hành vi nhận một phần tử và trả boolean; Supplier là hành vi không nhận tham số và cung cấp giá trị khi được gọi.

## 8. Bài tập nhỏ để tự gõ lại

1. Đoán output rồi đổi điều kiện lọc; so loop và Stream.
2. Trên List<Task> đang có, lọc DONE rồi lấy title: viết loop trước, Stream sau.
3. Tìm title dài hơn 100; chọn trả Optional, text dự phòng hoặc throw dựa trên contract bạn tự nêu.
4. Viết test để hai cách lọc trả cùng dữ liệu, gồm case không có phần tử khớp.

Đạt khi giải thích từng bước pipeline bằng lời của mình. Tiếp theo: [java.time](02-java-time-and-nio.md), chỉ học Instant/LocalDate ở lượt đầu. [Collector reference](06-stream-reference.md) để sau.

Đối chiếu behavior lazy/toList tại [Stream API Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html).
