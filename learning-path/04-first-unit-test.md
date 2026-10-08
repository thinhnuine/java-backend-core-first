# Unit Test Đầu Tiên Với Maven Và JUnit

**Học ở tuần 3 mới**, sau khi đã viết class, constructor và method. Bài này mất khoảng một buổi; chưa cần Spring, AssertJ hoặc Mockito.

## 1. Ý chính

Unit test chạy một hành vi nhỏ và so kết quả với điều mong đợi. Maven tải thư viện test và chạy test bằng plugin. Bạn sẽ kiểm tra quy tắc title của Task mà không mở browser hoặc chạy web server.

## 2. Giải thích code

Tạo Maven project riêng, hoặc chuyển `java-core-lab` từ tuần 1 sang cấu trúc sau. Nếu đã có `pom.xml`, hợp nhất cấu hình cần thiết, không ghi đè dependency đang dùng.

```text
java-core-lab/
  pom.xml
  src/main/java/dev/thinh/task/Task.java
  src/test/java/dev/thinh/task/TaskTest.java
```

`pom.xml` cho **lab Java thuần**; các phiên bản dưới đây cố định để tái lập ví dụ, không phải tuyên bố phiên bản mới nhất. Khi vào Spring, dùng dependency management của Spring project thay vì chép nguyên POM này.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>dev.thinh</groupId>
    <artifactId>java-core-lab</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <maven.compiler.release>21</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.11.4</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.5.2</version>
            </plugin>
        </plugins>
    </build>
</project>
```

`release` chọn mốc Java; `scope=test` chỉ cần thư viện ở test; compiler plugin compile code, Surefire tìm và chạy unit test. Đây là lý do thêm JUnit dependency thôi chưa đủ nếu plugin quá cũ.

`Task.java` là **mẫu tối thiểu để học cách test**, chưa phải lời giải đầy đủ của project:

```java
package dev.thinh.task;

public final class Task {
    private final String title;

    public Task(String title) {
        if (title == null || title.isBlank()) {
            throw new IllegalArgumentException("title is required");
        }
        this.title = title.trim();
    }

    public String title() {
        return title;
    }
}
```

`TaskTest.java`:

```java
package dev.thinh.task;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

class TaskTest {
    @Test
    void trimsTitle() {
        Task task = new Task("  Học Java  ");
        assertEquals("Học Java", task.title());
    }

    @Test
    void rejectsBlankTitle() {
        assertThrows(IllegalArgumentException.class, () -> new Task("   "));
    }
}
```

`@Test` đánh dấu method để JUnit chạy; `assertEquals(expected, actual)` so giá trị. `assertThrows` chạy lambda `() -> ...` và kiểm tra exception; lambda ở đây là đoạn hành vi hoãn lại, giống callback trong JS. Test cùng package giúp tổ chức code, không cần test class là public trong JUnit Jupiter.

Từ thư mục có POM:

```sh
mvn test
```

Lần đầu Maven cần mạng để tải dependency/plugin. Kết quả cần thấy **Tests run: 2, Failures: 0, Errors: 0** và BUILD SUCCESS. Báo cáo nằm ở `target/surefire-reports`. Nếu “No tests to run”, kiểm tra đường dẫn, tên `TaskTest` và annotation/import.

## 3. Vì sao thiết kế như vậy

Chỉ `println(task.title())` không tự phát hiện hồi quy: lần sau bạn phải nhìn bằng mắt. Assertion làm build fail khi behavior đổi ngoài dự định.

Ví dụ sai: `assertEquals(task.title(), task.title())` luôn so kết quả với chính nó. Dù code quên trim, test vẫn pass. Expected phải xuất phát từ quy tắc nghiệp vụ.

## 4. Liên hệ với frontend

`@Test` gần với `test(...)` của Jest/Vitest; assertion gần `expect`. Test này không cần DOM, HTTP, database hoặc Spring context, giống test một hàm xử lý dữ liệu thuần.

## 5. Khi nào dùng và không dùng

Dùng unit test cho validation, tính toán và chuyển trạng thái. Dùng integration test ở chặng sau khi cần chứng minh SQL, transaction và HTTP wiring hoạt động cùng nhau. Unit test pass không chứng minh dữ liệu được lưu vào DB.

## 6. Bẫy hay gặp

- Chỉ kiểm BUILD SUCCESS mà không xem số test thực sự đã chạy.
- Viết test gọi mạng/email thật cho bài Java thuần.
- Test private method làm test phụ thuộc chi tiết triển khai.
- Thêm Mockito trước khi biết có dependency nào cần thay thế.

## 7. Thuật ngữ mới

**Assertion** là điều kiện test kiểm tra; **regression/hồi quy** là behavior từng đúng bị hỏng sau sửa code; **test runner** là phần tìm và thực thi test; **test scope** giới hạn dependency cho code test.

## 8. Bài tập nhỏ để tự gõ lại

1. Bỏ `.trim()` khỏi constructor, chạy lại và đọc một test fail, sau đó khôi phục.
2. Tự thêm test truyền null.
3. Viết test title sau trim dài 101 ký tự phải bị từ chối; xem test fail trước khi sửa constructor.
4. Thêm hai case biên 1 và 100 ký tự hợp lệ. Đừng lấy toàn bộ implementation từ AI.

Tiếp theo: [JUnit và AssertJ](../phase-05-testing-design/week-11/01-junit5-assertj.md), rồi áp dụng test cho service Task Manager.
