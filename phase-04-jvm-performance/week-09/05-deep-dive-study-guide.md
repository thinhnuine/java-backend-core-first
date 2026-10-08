# Mentor Guide Tuần 9: JVM Memory, Class Loading, JIT Và GC

> **Chọn phạm vi:** làm phần được chỉ định trong [README module](README.md) và roadmap. Benchmark, log processor lớn, tự viết pool/queue/rate limiter, GC/reflection/proxy là bài mở rộng; checklist đầy đủ dưới đây không bắt buộc trước khi qua mốc học mới.

## 1. Ý chính

Tuần này bạn học Java như một runtime, không chỉ là syntax. JVM quản lý memory, load class, tối ưu code bằng JIT và dọn rác bằng GC. Hiểu phần này giúp bạn đọc lỗi production kiểu OOM, GC nhiều, app chậm, classpath lỗi.

## 2. Giải thích code

Ví dụ tạo nhiều object:

```java
package dev.thinh.javacore;

import java.util.ArrayList;
import java.util.List;

public class MemoryDemo {
    public static void main(String[] args) throws InterruptedException {
        List<byte[]> store = new ArrayList<>();

        while (true) {
            store.add(new byte[1024 * 1024]);
            System.out.println("Allocated " + store.size() + " MB");
            Thread.sleep(100);
        }
    }
}
```

Giải thích:

- `new byte[1024 * 1024]`: tạo mảng khoảng 1MB trên heap.
- `store.add(...)`: giữ reference, nên GC không thu hồi được.
- `while (true)`: allocation liên tục.
- Chạy với heap nhỏ có thể gây `OutOfMemoryError`.

Command:

```bash
java -Xmx128m '-Xlog:gc*' dev.thinh.javacore.MemoryDemo
```

- `-Xmx128m`: giới hạn heap max 128MB.
- `'-Xlog:gc*'`: bật GC log.

## 3. Vì sao thiết kế như vậy

Java tự quản lý memory bằng GC, nhưng GC chỉ thu hồi object không còn reachable. Nếu bạn vẫn giữ reference trong list/static cache, object không được dọn.

Ví dụ leak:

```java
private static final List<byte[]> STORE = new ArrayList<>();
```

Nếu app cứ add vào `STORE` mà không remove, object vẫn reachable từ static field. GC không dọn vì về mặt kỹ thuật object vẫn đang được dùng.

## 4. Liên hệ với Frontend

JavaScript cũng có garbage collector. Memory leak frontend thường là listener không remove, timer không clear, closure giữ reference. Java tương tự: object còn reachable thì GC không dọn.

Khác biệt là Java backend thường cần đọc heap dump, GC log, thread dump để debug production.

## 5. Khi nào dùng và không dùng

| Công cụ/khái niệm | Khi dùng | Khi tránh |
| --- | --- | --- |
| GC log | App pause/chậm/OOM | Đọc một dòng rồi kết luận |
| Heap dump | Cần biết object nào giữ memory | Dump production bừa bãi, file rất lớn |
| JIT awareness | Benchmark/performance | Tự benchmark thiếu warm-up |
| Classpath check | Lỗi class not found | Đoán mò dependency |

## 6. Bẫy hay gặp

- Nghĩ GC dọn mọi thứ không cần nữa theo ý mình.
- Không hiểu object còn reference thì không được dọn.
- Benchmark bằng một lần chạy.
- Nhầm stack overflow với heap OOM.
- Không đọc message OOM cụ thể.

## 7. Thuật ngữ mới

- `heap`: vùng chứa object.
- `stack`: vùng chứa call frame của thread.
- `metaspace`: vùng chứa metadata class.
- `GC`: garbage collector.
- `reachable`: còn được tham chiếu từ GC roots.
- `JIT`: compiler tối ưu code nóng lúc runtime.
- `classpath`: nơi JVM tìm class.

## 8. Bài tập nhỏ để tự gõ lại

1. Viết `MemoryDemo`.
2. Chạy với `-Xmx128m`.
3. Bật GC log.
4. Quan sát OOM.
5. Sửa code để không giữ object trong list nữa.
6. So sánh GC behavior.

Học tiếp theo: khi thấy OOM, câu hỏi đầu tiên là "OOM vùng nào?" và "object nào còn reachable?".
