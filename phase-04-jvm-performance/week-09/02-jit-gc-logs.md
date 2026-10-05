# JIT, Garbage Collector Và GC Log

## JIT

JIT compiler tối ưu code nóng khi app chạy.

Hệ quả:

- Benchmark lần chạy đầu dễ sai vì warm-up.
- Code chạy lâu có thể nhanh hơn sau khi JIT optimize.
- Microbenchmark nên dùng JMH, không tự đo bằng `System.currentTimeMillis` rồi kết luận mạnh.

## Benchmark Pitfalls

Tránh:

- Đo một lần rồi kết luận.
- Không warm-up.
- Để JVM optimize mất code không dùng kết quả.
- Dùng input quá nhỏ.
- So sánh trong môi trường đang chạy tác vụ khác.

## Garbage Collector

GC thu hồi object không còn reachable.

Reachable nghĩa là object còn được tham chiếu từ GC roots, ví dụ:

- local variable trên stack.
- static field.
- thread.
- JNI reference.

## G1

G1 là default phổ biến:

- chia heap thành region.
- cân bằng throughput và pause time.
- phù hợp nhiều app server thông thường.

## ZGC

ZGC hướng tới pause time thấp:

- hợp heap lớn.
- hợp latency-sensitive workload.
- không tự nhiên làm code logic nhanh hơn.

## GC Log

Chạy app với:

```bash
java -Xlog:gc* -jar app.jar
```

Set heap nhỏ để quan sát:

```bash
java -Xms128m -Xmx128m -jar app.jar
```

Khi đọc log, để ý:

- GC type.
- heap before/after.
- pause time.
- frequency.
- full GC có xuất hiện không.

## Bài Tập Nhanh

Viết `AllocationPressureDemo`:

- tạo nhiều object ngắn hạn.
- tạo một list giữ object để tăng heap.
- chạy với heap nhỏ.
- ghi lại khi nào GC thường xuyên hơn.
