# Ghi chú chọn collection

Ghi chú gốc của bạn được giữ để đối chiếu:

1. LinkedHashMap. vì lưu thứ tự tạo, thứ tự insertion và access order
2. HashSet vì có contains, add, remove
3. HashMap lưu key/value
4. LinkedHashMap
5. HashSet

**Làm rõ:** LinkedHashMap mặc định giữ insertion order (thứ tự thêm khóa). Access order (thứ tự truy cập gần đây) là chế độ phải chọn riêng, không phải hai thứ tự đồng thời. HashSet dùng để loại trùng; HashMap tra value theo key.

**Tự rà lại:** ghi nguyên yêu cầu cạnh mỗi lựa chọn. Chỉ từ 5 tên collection chưa thể khẳng định đáp án đúng; cần biết yêu cầu về thứ tự, trùng lặp và cách tra cứu.

## Đọc thêm sau

Nếu năm câu theo thứ tự trong bài Collection Choice Notes: cần xem riêng yêu cầu sort theo due date, loại email trùng, giữ thứ tự thêm, đếm tần suất và cache LRU (loại phần tử ít được dùng gần đây). Giữ đáp án của bạn rồi bổ sung lý do, không thay bằng đáp án do AI sinh.
