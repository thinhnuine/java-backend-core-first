# JVM Memory Và Class Loading

## Các Vùng Memory Chính

- Heap: nơi object thường sống.
- Stack: mỗi thread có stack riêng, chứa frame của method call.
- Metaspace: metadata class.
- String pool: quản lý string literal.
- Native memory: memory ngoài heap JVM, ví dụ direct buffer, thread stack.

## Heap

Heap chứa object được tạo bằng `new` trong đa số trường hợp.

Vấn đề thường gặp:

- `OutOfMemoryError: Java heap space`.
- GC chạy nhiều vì allocation quá nhiều.
- Object còn reachable ngoài ý muốn gây leak.

## Stack

Stack chứa call frame:

- local variable.
- operand stack.
- return address/metadata phục vụ method call.

Recursion quá sâu có thể gây:

```text
StackOverflowError
```

## Metaspace

Metaspace chứa class metadata. App load quá nhiều class hoặc classloader leak có thể gây OOM metaspace.

## Class Loading

JVM load class khi cần. Classpath quyết định JVM tìm class ở đâu.

Lỗi hay gặp:

- `ClassNotFoundException`: runtime không tìm được class theo tên.
- `NoClassDefFoundError`: compile có nhưng runtime thiếu hoặc init class fail.
- Version conflict giữa dependency.

## Class Initialization

Static field/static block chạy khi class initialize.

Cẩn thận:

- static mutable state dễ gây leak.
- static initialization nặng làm startup chậm.
- exception trong static init gây lỗi khó đọc.

## Bài Tập Nhanh

Viết chương trình:

- Tạo object liên tục.
- Tạo recursion sâu để thấy stack overflow.
- In classloader của vài class:

```java
System.out.println(String.class.getClassLoader());
System.out.println(MyClass.class.getClassLoader());
```

Ghi lại quan sát.

String pool ở đây là cơ chế intern/dùng chung String, không phải một vùng memory độc lập ngang với heap/stack/metaspace; String object vẫn ở heap. Metaspace dùng native memory. Khi nói local variable ở stack hoặc object ở heap, đây là mô hình để đọc code; JIT có thể tối ưu representation/allocation.
