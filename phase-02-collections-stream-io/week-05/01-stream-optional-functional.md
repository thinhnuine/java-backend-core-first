# Stream Và Optional: Đọc Một Pipeline Từng Bước

Học một ví dụ rồi dừng để tự sửa đầu vào. Các heading giữ nguyên để đối chiếu bản trước.

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## 1. Ý chính

Stream là chuỗi bước xử lý phần tử từ nguồn dữ liệu, giúp viết phép lọc và biến đổi rõ ràng.

## 2. Giải thích code

```java
var titles = java.util.List.of("Java", "SQL", "Spring");
var selected = titles.stream() // Bắt đầu một lượt xử lý
    .filter(title -> title.length() > 3) // Giữ chuỗi dài hơn 3
    .map(title -> title.toUpperCase(java.util.Locale.ROOT)) // Đổi chữ hoa
    .toList(); // Gom kết quả và thực hiện lượt xử lý
System.out.println(selected); // [JAVA, SPRING]
```

## 3. Vì sao thiết kế như vậy

filter chọn phần tử; map biến đổi; toList kết thúc lượt xử lý. Lambda `title -> ...` là đoạn hành vi nhận title, gần callback. **Quy tắc ngón tay cái:** đọc từng bước thành lời; khó đọc thì dùng vòng lặp.

## 4. Liên hệ với frontend

Gần Array.filter/map, nhưng Stream thường chờ thao tác kết thúc như toList mới xử lý; Array.filter/map chạy ngay khi gọi.

## 5. Khi nào dùng và không dùng

**Khi nào dùng**

- Lọc hoặc biến đổi danh sách.
- Tính kết quả mà không sửa nguồn.

## 6. Bẫy hay gặp

**Lỗi hay gặp**

- Tái dùng một stream đã xử lý xong.
- Gọi add trên kết quả toList, vốn không cho sửa cấu trúc.
- Giấu thay đổi dữ liệu ngoài trong map.

## 7. Thuật ngữ mới

**Pipeline**: chuỗi bước xử lý. **Terminal operation**: thao tác kết thúc như toList. **Optional**: object biểu diễn kết quả có thể thiếu; xem ví dụ riêng ở phần đọc sau.

## 8. Bài tập nhỏ để tự gõ lại

**Tự gõ, không dùng AI sinh code:** Viết vòng for lọc tên dài hơn 3 rồi so kết quả với Stream.

**Gợi ý:** Dùng cùng input; thử cả danh sách rỗng.

## Đọc thêm sau

[Optional và lambda theo từng ví dụ](06-stream-reference.md). java.util.Locale.ROOT chọn quy tắc đổi chữ không phụ thuộc ngôn ngữ máy. Collector (cách gom kết quả), flatMap (gộp nhiều nguồn), parallelStream (xử lý có thể song song) để sau.
