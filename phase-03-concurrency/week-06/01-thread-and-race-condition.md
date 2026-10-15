# Thread Và Race Condition

> **Làm rõ trước khi học:** Demo có thể cho đúng 20000 do cách các luồng được chạy trong lần đó; không phải bằng chứng thread-safe (an toàn khi nhiều luồng dùng chung).

**Cách chạy ví dụ:** dùng JDK 21 (bộ công cụ Java). Mở terminal, gõ `jshell`; gõ từng khối Java độc lập, dùng `/reset` trước khối tiếp theo và `/exit` để thoát. JShell chạy được khai báo class, method (hàm của Java) và lệnh trực tiếp, không cần khung main. Các comment `//` giải thích dòng code và kết quả cần quan sát.

## Thread Cơ Bản

Thread là một luồng thực thi, cho phép một công việc tiến hành cùng lúc với công việc khác.

```java
Thread worker = new Thread(() -> System.out.println("Worker")); // Lambda: việc cần chạy
worker.start(); // Bắt đầu luồng riêng
worker.join(); // Chờ worker kết thúc
System.out.println("Done"); // Luôn sau Worker trong ví dụ này
```

**Khi nào dùng**

- Khi học nhiều công việc cùng chạy.
- Khi cần hiểu request đồng thời ở server.

**Lỗi hay gặp**

- Gọi run và tưởng đã tạo luồng mới.
- Quên chờ việc xong trước khi đọc kết quả.

**Quy tắc ngón tay cái:** start để chạy luồng mới; join để chờ kết thúc.

**Tự gõ, không dùng AI sinh code:** In tên luồng trong worker và ở ngoài. **Gợi ý:** Dùng Thread.currentThread().getName().

## Thread Lifecycle Tối Giản

Lifecycle là vòng đời thread từ lúc tạo đến lúc kết thúc.

```java
Thread worker = new Thread(() -> {});
System.out.println(worker.getState()); // NEW: chưa bắt đầu
worker.start();
worker.join();
System.out.println(worker.getState()); // TERMINATED: đã kết thúc
```

**Khi nào dùng**

- Xem thread đã bắt đầu/kết thúc chưa.
- Đọc trạng thái khi tìm lỗi.

**Lỗi hay gặp**

- Trông chờ trạng thái ngay sau start luôn giống nhau.

**Tự gõ, không dùng AI sinh code:** Thử in trạng thái ngay sau start, trước join. **Gợi ý:** Kết quả lúc đó phụ thuộc thời điểm; đừng cố học thuộc một output.

## Race Condition

Race condition là lỗi khi kết quả phụ thuộc thứ tự các luồng xen vào nhau. count++ gồm đọc số cũ → cộng → ghi; hai luồng cùng đọc 0 có thể cùng ghi 1.

**Quy tắc ngón tay cái:** Có nhiều luồng sửa chung dữ liệu: phải có quy tắc phối hợp.

**Tự làm:** Vẽ hai luồng cùng tăng từ 0 nhưng cuối cùng chỉ thành 1. **Gợi ý:** Viết từng thao tác đọc, cộng, ghi thành ba dòng riêng.

## Demo `UnsafeCounter`

UnsafeCounter là ví dụ bộ đếm không bảo vệ việc cập nhật chung giữa các luồng.

```java
class UnsafeCounter { int count; void increment() { count++; } }
UnsafeCounter counter = new UnsafeCounter();
Runnable work = () -> { for (int i = 0; i < 10000; i++) counter.increment(); };
Thread first = new Thread(work); Thread second = new Thread(work);
first.start(); second.start();
first.join(); second.join(); // Chờ cả hai trước khi đọc
System.out.println(counter.count); // Không được bảo đảm bằng 20000
```

**Khi nào dùng**

- Quan sát dữ liệu dùng chung bị cập nhật sai.
- So bản chưa khóa với bản đã bảo vệ.

**Lỗi hay gặp**

- Thấy đúng vài lần rồi kết luận an toàn.

**Quy tắc ngón tay cái:** Một lần chạy đúng không chứng minh code đồng thời đúng.

**Tự gõ, không dùng AI sinh code:** Chạy lại và ghi số dự kiến cùng số thực tế. **Gợi ý:** Không thấy sai vẫn phải giải thích lịch xen kẽ ở phần trước.

## Shared Mutable State

Shared mutable state là dữ liệu có thể sửa được nhiều luồng cùng dùng; giảm dữ liệu dùng chung giúp giảm lỗi.

```java
Runnable work = () -> {
    int local = 0; // Biến riêng cho mỗi lần chạy
    for (int i = 0; i < 3; i++) local++;
    System.out.println(local); // Mỗi luồng tự in 3
};
Thread worker = new Thread(work); worker.start(); worker.join();
```

**Khi nào dùng**

- Hai request cùng dùng một service.
- Nhiều worker sửa cùng một bộ đếm.

**Lỗi hay gặp**

- Nghĩ biến local chứa reference làm object được trỏ tới tự riêng biệt.

**Tự gõ, không dùng AI sinh code:** Chạy hai thread dùng work và giải thích vì sao local không dùng chung. **Gợi ý:** Biến nằm trong lần gọi, không nằm ở field object dùng chung.

## Bài Tập Nhanh

Tập phân biệt lỗi quan sát được với lý do code đúng.

- Chạy UnsafeCounter.
- Ghi một lịch xen kẽ gây mất lần tăng.
- Đọc bài synchronized rồi sửa.

**Gợi ý:** Đợi join trước khi đọc count.

**Đạt khi:** Giải thích được vì sao bản sửa không cho xen kẽ thao tác tăng. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Java Memory Model (quy tắc nhìn thấy dữ liệu giữa luồng), trạng thái BLOCKED/WAITING và chi phí tạo luồng học trong bài tiếp theo. Không dùng sleep hoặc println làm cơ chế đồng bộ.
