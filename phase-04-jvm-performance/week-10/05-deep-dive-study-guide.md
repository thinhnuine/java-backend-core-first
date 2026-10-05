# Deep Dive Tuần 10: Tooling, Heap Dump, Reflection Và Proxy

## Cách Học Tuần Này

Tuần này giúp bạn bớt cảm giác framework là phép thuật. Spring làm được nhiều thứ vì Java runtime cho phép đọc metadata, tạo object, bọc object bằng proxy, và quan sát JVM bằng tooling.

## JFR

Java Flight Recorder ghi lại event runtime:

- CPU
- allocation
- GC
- lock
- thread
- exception

Bạn dùng JFR khi muốn timeline khá đầy đủ mà overhead thấp.

## VisualVM

VisualVM hợp cho học vì nhìn trực quan:

- heap usage
- thread
- sampler
- heap dump

Khi app ăn RAM, hãy nhìn object nào chiếm nhiều memory và ai giữ reference tới nó.

## `jcmd`

`jcmd` là công cụ dòng lệnh nói chuyện với JVM process.

Lệnh hữu ích:

```bash
jcmd
jcmd <pid> GC.heap_info
jcmd <pid> Thread.print
jcmd <pid> GC.heap_dump heap.hprof
```

## Heap Dump

Heap dump là snapshot object trong heap tại một thời điểm.

Khi đọc heap dump, hỏi:

- object type nào nhiều bất thường?
- object nào giữ nhiều memory?
- GC root nào giữ object sống?
- có static collection/cache/listener nào giữ mãi không?

## Reflection

Reflection cho phép đọc class/method/field runtime.

Bạn có thể:

- đọc annotation
- gọi method
- đọc constructor
- tạo object

Nhược điểm:

- chậm hơn direct call
- exception phức tạp
- dễ phá encapsulation
- refactor khó hơn

## Annotation

Annotation là metadata.

Nếu muốn đọc runtime, phải có:

```java
@Retention(RetentionPolicy.RUNTIME)
```

Nếu quên retention, scanner runtime sẽ không thấy annotation.

## Dynamic Proxy

Proxy đứng giữa caller và target:

```text
caller -> proxy -> target
```

Proxy có thể thêm:

- logging
- metrics
- transaction
- security

Spring transaction thường là proxy quanh service method. Trước method bắt đầu transaction, sau method commit/rollback.

## Bài Tập Theo Bước

1. Tạo `LeakDemo`.
2. Lấy heap dump.
3. Viết note object nào bị giữ.
4. Tạo `@MyComponent`.
5. Viết scanner đọc annotation.
6. Tạo dynamic proxy logging cho interface.
7. Ghi liên hệ với Spring.

## Lỗi Thường Gặp

- Annotation thiếu `RUNTIME`.
- Proxy nuốt mất exception cause.
- Reflection scanner quá tham vọng.
- Heap dump chỉ nhìn object lớn mà không nhìn reference path.
- Nghĩ Spring chỉ là annotation, quên container/proxy/lifecycle.

## Câu Hỏi Tự Kiểm Tra

- Heap dump trả lời câu hỏi gì?
- Leak trong Java thường do gì?
- Reflection đọc được gì?
- Annotation retention là gì?
- Proxy giúp Spring transaction như thế nào?
