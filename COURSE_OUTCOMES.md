# Chuẩn Đầu Ra: Từ FE Đến Fullstack Java

## Mục tiêu của khóa

Tự triển khai và giải thích một tính năng nhỏ xuyên FE → Java API → PostgreSQL (cơ sở dữ liệu). API là phần chương trình khác gọi; backend chịu trách nhiệm kiểm dữ liệu, quyền và lưu trữ. Đánh giá bằng việc làm được, không bằng số file đã đọc.

## Sáu năng lực và bằng chứng

| Năng lực | Bằng chứng nhỏ, quan sát được |
| --- | --- |
| Java | Tạo Task, kiểm đầu vào, tìm theo id, đọc được lỗi |
| HTTP/Spring | Gọi tạo/lấy task và giải thích 201/400/404 |
| Dữ liệu | Tự viết SQL; dựng DB mới và kiểm hủy thao tác lỗi |
| FE tích hợp | Form gửi dữ liệu, hiển thị lỗi, refresh vẫn đúng |
| Kiểm quyền | Tài khoản B không đọc/sửa/xóa task của A |
| Chạy lại và test | Người khác chạy theo README; test bắt được lỗi cố ý |

## Bài nghiệm thu cuối

### Thêm một tính năng

Trên Task Manager đang có, thêm dueDate (ngày hết hạn) và lọc theo ngày/trạng thái. Viết dữ liệu vào/ra trước, cập nhật cấu trúc DB, Java và FE.

### Kiểm trường hợp lỗi

Thử title sai, id không có, người dùng khác, yêu cầu thiếu CSRF token theo cấu hình session và cập nhật từ version cũ. CSRF token là dữ liệu giúp kiểm yêu cầu thay đổi từ browser dùng cookie; version là số phiên bản giúp phát hiện cập nhật cũ.

### Demo độc lập

Dựng từ DB bài tập mới, đăng nhập, tạo/sửa/lọc, refresh và logout. Tự giải thích một lỗi từ response tới server và DB; không cần bắt đầu ứng dụng khác.

## Cách quyết định đạt

Mỗi năng lực: **Đạt** hoặc **Cần củng cố**, kèm một bằng chứng. Cần làm được, giải thích được và thử được trường hợp thất bại. Giao diện đẹp không bù cho truy cập trái quyền hoặc mất dữ liệu ngoài dự định.

## Thời lượng thực tế và hướng tiếp theo

24 tuần × 8 giờ là kế hoạch, không phải hạn phải chạy theo. Có thể thêm 4–8 tuần củng cố tùy thực hành; ưu tiên bỏ bài tự chọn thay vì bỏ kiểm thử. Sau khóa, tiếp tục tính năng nhỏ có người review; không coi tài liệu này là bảo đảm chức danh hay chuyển việc.

## Đọc thêm sau

Đo hiệu năng, vận hành quy mô lớn và hệ thống phân tán học qua nhu cầu công việc sau nền tảng. [Roadmap](ROADMAP.md), [Task Manager](learning-path/03-task-manager-project.md).
