# Spring Tạo Object Và Nối Dependency Như Thế Nào?

Học tuần 7 mới, sau [HTTP](../../learning-path/01-http-and-backend.md), class/interface và constructor injection. Mục tiêu buổi đầu là khởi động server, gọi được một endpoint và giải thích ai tạo object; chưa cần thuộc bean lifecycle hooks.

## 1. Ý chính

Java thuần cho phép bạn tự new service và truyền dependency vào constructor. Với Spring, container có thể làm việc tạo và nối object đó dựa trên cấu hình. DI là cách đưa dependency từ bên ngoài; nó dùng được cả khi chưa có Spring. IoC là ý tưởng rộng hơn về việc framework nắm quyền điều phối.

## 2. Giải thích code

Tạo project ở [Spring Initializr](https://start.spring.io), chọn Maven, Java, JDK 21, một phiên bản Boot stable tương thích, group dev.thinh, artifact task-api, package dev.thinh.task. Chọn Spring Web và Validation; kiểm dependency test trong POM sinh ra. Ghi phiên bản đã chọn vào README và dùng tài liệu đúng phiên bản.

Tên starter web có thể khác giữa các major Boot; giữ POM Initializr sinh ra, tránh chép tên/version từ bài cũ. Dùng [hướng dẫn first application](https://docs.spring.io/spring-boot/tutorial/first-application/index.html) khi cần đối chiếu setup.

Giữ application class sinh ra tại src/main/java/dev/thinh/task/TaskApiApplication.java, hoặc dùng nội dung sau nếu tên class khớp:

```java
package dev.thinh.task;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TaskApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(TaskApiApplication.class, args);
    }
}
```

Tạo GreetingService.java cùng package:

```java
package dev.thinh.task;

import org.springframework.stereotype.Service;

@Service
public class GreetingService {
    public String message() {
        return "Task API is running";
    }
}
```

Tạo GreetingController.java cùng package:

```java
package dev.thinh.task;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {
    private final GreetingService service;

    public GreetingController(GreetingService service) {
        this.service = service;
    }

    @GetMapping("/hello")
    public String hello() {
        return service.message();
    }
}
```

Chạy ./mvnw spring-boot:run từ thư mục có POM; Windows dùng mvnw.cmd spring-boot:run. Ở terminal thứ hai, chạy curl -i http://localhost:8080/hello. Cần thấy 200 và body `Task API is running`. Nếu port bị dùng, cấu hình server.port và gọi đúng port. Dừng local server bằng Ctrl+C.

@SpringBootApplication là annotation khởi động cấu hình, auto-configuration và component scan mặc định. @Service đánh dấu service bean; @RestController đánh dấu controller trả body. Container tìm GreetingService, tạo object đó và đưa vào constructor của GreetingController. final field không tự inject; việc inject do Spring thực hiện. @GetMapping ánh xạ GET /hello tới hello().

Trong Java thuần bạn có thể viết `new GreetingController(new GreetingService())`; vẫn là DI, chỉ khác ai điều phối tạo object. Unit test service chưa cần bật server.

## 3. Vì sao thiết kế như vậy

Controller phụ thuộc capability của service qua constructor nên nhìn được dependency. Nếu viết `GreetingService service;` nhưng không khởi tạo hay inject, service.message() sẽ lỗi NullPointerException. @Service cũng không tạo bean nếu class nằm ngoài vùng scan mà chưa được cấu hình bổ sung.

Thử bỏ @Service rồi chạy lại để thấy startup lỗi thiếu bean. Sau đó khôi phục. Nếu có hai bean cùng một interface mà Spring chưa biết chọn bean nào, cần quyết định bằng cấu hình như @Qualifier/@Primary; chưa cần tạo tình huống này trong buổi đầu.

## 4. Liên hệ với frontend

Constructor injection gần việc truyền một dependency vào hàm/component, nhưng Spring container điều phối ở runtime. Annotation là metadata cho framework đọc; không phải chỉ gắn @Service là method tự chạy hoặc object tự thread-safe.

## 5. Khi nào dùng và không dùng

Dùng constructor injection cho dependency cần để object hoạt động. Dùng @Bean khi muốn tạo object bằng một configuration method, nhất là class từ thư viện ngoài; không cần vừa @Service vừa @Bean cho cùng object nếu không có chủ đích.

Bean scope mặc định là singleton trong mỗi container; cùng object có thể phục vụ nhiều request đồng thời. Singleton không có nghĩa là chỉ có một instance trên mọi máy/process. Không để title/current user của request trong field service. Auto-configuration xét dependency, property và các bean đã có, không chỉ xét annotation.

## 6. Bẫy hay gặp

Chọn SDK 21 nhưng Maven chạy JDK khác; đặt application class ở package con nên bỏ sót service; copy starter/test import từ major khác; tự new service rồi mong Spring proxy/transaction có hiệu lực; thiếu Validation dependency khi vào bài tiếp theo.

## 7. Thuật ngữ mới

Container là thành phần quản lý object; bean là object do container quản lý; scan là tìm component trong vùng package; scope xác định cách bean được chia sẻ; auto-configuration là cấu hình có điều kiện để dựng ứng dụng từ dependency/property/bean.

## 8. Bài tập nhỏ để tự gõ lại

1. Chạy endpoint /hello và dùng debugger dừng ở controller.
2. Đổi text trong service, restart và gọi lại.
3. Tạm bỏ @Service, đọc lỗi startup rồi khôi phục.
4. Viết unit test GreetingService.message() bằng new, không dùng Spring context.
5. Áp dụng constructor injection tương tự cho TaskService và repository đã viết; chưa đưa DB vào.

Đạt khi bạn giải thích được object nào do Spring tạo và request đi tới method nào. Tiếp theo: [REST/validation](02-rest-validation-error-handling.md), rồi [project in-memory](03-in-memory-task-api-project.md).
