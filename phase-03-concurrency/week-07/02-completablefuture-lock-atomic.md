# CompletableFuture, Lock, Atomic Và ConcurrentHashMap

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## CompletableFuture

CompletableFuture nối các bước xử lý kết quả có thể đến sau, giúp diễn đạt việc A xong thì làm B.

```java
var result = java.util.concurrent.CompletableFuture.completedFuture("Java")
    .thenApply(text -> text + " backend"); // Biến đổi kết quả
System.out.println(result.join()); // Java backend
```

**Liên hệ FE:** thenApply gần then trong Promise, nhưng Java có quy tắc executor và chạy luồng riêng.

**Khi nào dùng**

- Nối bước xử lý kết quả.
- Ghép hai kết quả độc lập.

**Lỗi hay gặp**

- Nghĩ completedFuture tạo công việc bất đồng bộ mới.

**Tự gõ, không dùng AI sinh code:** Nối thêm bước lấy độ dài chuỗi. **Gợi ý:** Ví dụ completedFuture có kết quả sẵn, không tạo thread mới.

## Lock

Lock là khóa điều khiển quyền vào vùng dùng chung; ReentrantLock cho phép lấy và trả khóa chủ động.

```java
{ // Một khối để JShell thực hiện lấy và trả khóa trong cùng lần chạy
    var lock = new java.util.concurrent.locks.ReentrantLock();
    lock.lock(); // Lấy khóa
    try { System.out.println("protected"); }
    finally { lock.unlock(); } // Luôn trả khóa cả khi có lỗi
}
```

**Khi nào dùng**

- Cần chủ động lấy/trả khóa.
- Sau này cần thử lấy khóa có giới hạn thời gian.

**Lỗi hay gặp**

- Lấy khóa nhưng quên finally unlock.
- Lấy và trả ReentrantLock ở hai thread khác nhau.

**Quy tắc ngón tay cái:** Lấy Lock xong phải có finally trả khóa.

**Tự gõ, không dùng AI sinh code:** Đặt thao tác có thể lỗi trong try và kiểm tra lock đã được trả. **Gợi ý:** Giữ unlock trong finally.

## Atomic

Atomic là thao tác không bị quan sát ở trạng thái nửa chừng; AtomicInteger hỗ trợ cập nhật một số nguyên an toàn.

```java
var count = new java.util.concurrent.atomic.AtomicInteger();
count.incrementAndGet(); // Tăng và đọc như một thao tác không bị xen ngang
System.out.println(count.get()); // 1
```

**Khi nào dùng**

- Tăng một số đếm dùng chung.
- Cần một thao tác cập nhật số nguyên.

**Lỗi hay gặp**

- Tách get rồi set làm mất tính atomic.

**Quy tắc ngón tay cái:** Một số đếm: atomic tiện; nhiều field liên quan cần thiết kế thêm.

**Tự gõ, không dùng AI sinh code:** Tăng AtomicInteger từ hai thread rồi join trước khi đọc. **Gợi ý:** Dùng incrementAndGet, không tách get rồi set.

## ConcurrentHashMap

ConcurrentHashMap hỗ trợ cập nhật theo khóa khi nhiều luồng cùng dùng map.

```java
var counts = new java.util.concurrent.ConcurrentHashMap<String, Integer>();
counts.merge("java", 1, Integer::sum); // :: dùng method sum để cộng
counts.merge("java", 1, Integer::sum);
System.out.println(counts.get("java")); // 2
```

**Khi nào dùng**

- Nhiều thread đếm theo từ.
- Cập nhật giá trị một key dùng chung.

**Lỗi hay gặp**

- Tưởng nhiều khóa cập nhật cùng nhau như một transaction (nhóm thay đổi cùng thành công/thất bại).

**Tự gõ, không dùng AI sinh code:** Đếm hai từ bằng nhiều task. **Gợi ý:** Đợi mọi task kết thúc rồi so với bộ đếm một luồng.

## Deadlock

Deadlock là tình huống các luồng giữ khóa và chờ khóa của nhau nên không thể đi tiếp.

**Quy tắc ngón tay cái:** Nếu cần nhiều khóa, lấy chúng theo một thứ tự chung.

**Tự làm:** Vẽ A giữ khóa 1 đợi 2, B giữ 2 đợi 1; sau đó đổi cả hai cùng lấy 1 trước. **Gợi ý:** Dùng sơ đồ trước; không cần làm chương trình treo để hiểu vòng chờ.

## Bài Tập Nhanh

Ghép hai kết quả có thể đến độc lập.

- Tạo hai CompletableFuture có kết quả nhỏ.
- Ghép bằng thenCombine.
- Thử một nhánh lỗi.

**Gợi ý:** Trước hết dùng completedFuture; sau đó mới supplyAsync với executor được quản lý.

**Đạt khi:** Giải thích kết quả thành công và lỗi được chuyển tới đâu. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

thenCompose nối việc trả về Future; thenCombine ghép hai kết quả. Common pool là nhóm luồng dùng chung mặc định, không phù hợp mọi việc chờ I/O. tryLock, Condition, CAS (so sánh và cập nhật nguyên tử) và LongAdder để sau.
