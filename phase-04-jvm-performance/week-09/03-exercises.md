# Bài Tập Tuần 9

## Bài 1: Memory Regions Demo

Tạo demo:

- method recursion gây `StackOverflowError`.
- allocation nhiều object gây GC.
- static list giữ object.

Ghi lại:

- lỗi nào xảy ra.
- command chạy.
- output quan sát được.

## Bài 2: GC Log Lab

Chạy app với:

```bash
java -Xms128m -Xmx128m -Xlog:gc* -jar app.jar
```

Thử:

- object ngắn hạn.
- object giữ trong list.
- tăng/giảm heap size.

Tạo `gc-log-notes.md`:

- command.
- tổng quan log.
- pause time đáng chú ý.
- kết luận cẩn trọng.

## Bài 3: Benchmark Warm-Up

Viết một function xử lý collection.

Đo:

- lần chạy đầu.
- sau 10 lần warm-up.
- sau 100 lần.

Ghi lại vì sao số đo có thể thay đổi.

## Bài 4: Class Loading Note

Tạo note:

- `ClassNotFoundException` khác `NoClassDefFoundError` thế nào.
- Classpath là gì.
- Vì sao dependency conflict có thể chỉ nổ runtime.
