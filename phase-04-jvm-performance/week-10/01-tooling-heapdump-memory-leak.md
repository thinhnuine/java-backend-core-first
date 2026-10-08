# Tooling, Heap Dump Và Memory Leak

## Công Cụ

- JFR: ghi event runtime, CPU, allocation, lock, GC.
- VisualVM: quan sát process, heap, thread, sampler/profiler.
- `jcmd`: gửi command tới JVM process.
- Heap dump: snapshot object trên heap.

## Lệnh `jcmd`

```bash
jcmd
jcmd <pid> VM.flags
jcmd <pid> GC.heap_info
jcmd <pid> Thread.print
jcmd <pid> GC.heap_dump heap.hprof
```

## Memory Leak Trong Java

Leak thường là object còn reachable ngoài ý muốn.

Ví dụ:

- Static collection giữ object mãi.
- Cache không eviction.
- Listener/subscription không unsubscribe.
- ThreadLocal không clear.
- Queue backlog không được drain.

## App Leak Có Chủ Đích

```java
import java.util.ArrayList;
import java.util.List;

public final class LeakDemo {
    private static final List<byte[]> STORE = new ArrayList<>();

    public static void main(String[] args) throws Exception {
        for (int i = 0; i < 32; i++) {
            STORE.add(new byte[1024 * 1024]);
        }
        System.out.println("PID=" + ProcessHandle.current().pid());
        Thread.sleep(120_000);
    }
}
```

Lưu file LeakDemo.java, chạy javac LeakDemo.java. Demo giữ khoảng 32 MiB payload và chờ 2 phút để bạn lấy dump trước khi process kết thúc. Chạy ở terminal thứ nhất, gọi jcmd ở terminal thứ hai, thay <pid> bằng PID đã in:

```bash
java -Xmx128m LeakDemo
jcmd <pid> GC.heap_dump heap.hprof
```

## Khi Phân Tích Heap Dump

Tìm:

- object nào chiếm nhiều memory.
- đường reference giữ object sống.
- static field nào giữ collection.
- số lượng object bất thường.

## Bài Tập Nhanh

Tạo `memory-leak-analysis.md`:

- triệu chứng.
- command lấy dump.
- object nghi ngờ.
- root reference giữ object.
- fix đề xuất.
