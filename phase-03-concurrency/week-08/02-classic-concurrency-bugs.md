# Các Lỗi Concurrency Kinh Điển

Đây là trang nhận diện lỗi, không phải yêu cầu học hết năm lỗi trong một buổi.

## Race Condition

Race condition: hai lần cập nhật xen nhau làm kết quả phụ thuộc thời điểm.

**Quy tắc ngón tay cái:** Bảo vệ toàn bộ thao tác đọc-sửa-ghi.

**Tự làm:** Vẽ hai lần tăng cùng đọc 0. **Gợi ý:** Dùng synchronized hoặc AtomicInteger sau khi xác định dữ liệu chung.

## Deadlock

Deadlock: các luồng chờ khóa của nhau mãi.

**Quy tắc ngón tay cái:** Lấy nhiều khóa theo cùng thứ tự.

**Tự làm:** Vẽ vòng chờ hai khóa rồi phá vòng đó. **Gợi ý:** Không cần chạy code treo; nếu tự thử phải có cách kết thúc process riêng.

## Visibility Bug

Visibility bug: luồng đọc không được bảo đảm thấy thay đổi từ luồng ghi.

**Quy tắc ngón tay cái:** Có cơ chế đồng bộ rõ mới dựa vào dữ liệu mới.

**Tự làm:** Tìm field cờ được đọc ở một luồng và ghi ở luồng khác. **Gợi ý:** Đối chiếu volatile, synchronized hoặc join; sleep không thay chúng.

## Thread Starvation

Thread starvation: công việc chờ lâu vì không có luồng xử lý rảnh.

**Quy tắc ngón tay cái:** Đừng để worker chờ task con bị kẹt trong chính pool đã đầy.

**Tự làm:** Vẽ pool một worker: task cha chờ task con ở cùng pool. **Gợi ý:** Chỉ ra ai có thể lấy task con ra chạy.

## Lock Contention

Lock contention: nhiều luồng tranh một khóa nên phải xếp hàng.

**Quy tắc ngón tay cái:** Giữ vùng khóa đủ bảo vệ dữ liệu nhưng tránh việc chậm không cần thiết.

**Tự làm:** Đánh dấu lời gọi mạng trong vùng synchronized. **Gợi ý:** Tìm cách chuẩn bị dữ liệu trước hoặc xử lý ngoài khóa, rồi kiểm lại tính đúng.

## Bài Tập Nhanh

Viết ghi chú về một lỗi bạn hiểu rõ.

- Ghi triệu chứng.
- Vẽ thứ tự thao tác gây lỗi.
- Ghi cách sửa và lý do.

**Gợi ý:** Không chỉ ghi “thêm synchronized”; nêu object khóa và field được bảo vệ.

**Đạt khi:** Một người khác đọc ghi chú có thể giải thích lại lỗi. Tự gõ phần triển khai, không dùng AI sinh lời giải.

## Đọc thêm sau

Thread dump là ảnh chụp trạng thái các luồng; dùng khi cần tìm vòng chờ. Fairness (độ công bằng khi cấp quyền chạy) và phân chia khóa chỉ tối ưu sau khi code đúng.
