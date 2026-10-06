# Mentor Guide Tuần 10: Tooling, Heap Dump, Reflection Và Proxy

## 1. Ý chính

Tuần này học cách quan sát JVM và hiểu cơ chế phía sau framework. `jcmd`, JFR, VisualVM, heap dump giúp bạn debug runtime. `reflection`, `annotation`, `dynamic proxy` giúp bạn hiểu vì sao Spring có thể scan class, inject bean, bọc transaction.

## 2. Giải thích code

Annotation:

```java
package dev.thinh.javacore;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MyComponent {
}
```

Giải thích:

- `@interface`: khai báo annotation.
- `@Retention(RetentionPolicy.RUNTIME)`: annotation còn tồn tại ở runtime để reflection đọc được.
- `@Target(ElementType.TYPE)`: annotation dùng trên class/interface/enum.

Reflection đọc annotation:

```java
public class ComponentScanner {
    public boolean isComponent(Class<?> type) {
        return type.isAnnotationPresent(MyComponent.class);
    }
}
```

- `Class<?>`: metadata của class.
- `isAnnotationPresent`: kiểm tra class có annotation không.

## 3. Vì sao thiết kế như vậy

Nếu annotation không có runtime retention:

```java
@Retention(RetentionPolicy.SOURCE)
public @interface MyComponent {
}
```

Reflection runtime sẽ không thấy annotation. Scanner của bạn sẽ trả false dù class có annotate trong source.

Dynamic proxy giúp bọc behavior:

```text
caller -> proxy -> target
```

Spring transaction cũng theo tinh thần này: trước khi gọi method thì mở transaction, method xong thì commit, lỗi thì rollback.

## 4. Liên hệ với Frontend

Decorator trong TypeScript/NestJS có cảm giác gần annotation:

```ts
@Controller()
class UserController {}
```

Spring annotation cũng dùng metadata:

```java
@RestController
public class UserController {
}
```

Proxy thì giống wrapper function/HOC ở frontend:

```javascript
const withLogging = fn => (...args) => {
  console.log(args)
  return fn(...args)
}
```

## 5. Khi nào dùng và không dùng

| Công cụ | Khi dùng | Khi tránh |
| --- | --- | --- |
| `jcmd` | Cần thread dump/heap info nhanh | Không biết PID/process |
| Heap dump | Memory leak/OOM analysis | App production nhạy cảm, dump lớn |
| Reflection | Framework/tooling, metadata | Business logic bình thường |
| Annotation | Metadata declarative | Thay thế mọi config/logic |
| Proxy | Logging, transaction, security cross-cutting | Logic domain cốt lõi |

## 6. Bẫy hay gặp

- Annotation thiếu `RUNTIME`.
- Reflection nuốt exception.
- Proxy làm mất root cause.
- Heap dump chỉ nhìn object lớn mà không nhìn ai giữ reference.
- Nghĩ Spring chỉ là annotation, quên container/proxy/lifecycle.

## 7. Thuật ngữ mới

- `reflection`: đọc/gọi metadata class runtime.
- `annotation`: metadata gắn vào code.
- `retention`: annotation tồn tại tới giai đoạn nào.
- `target`: annotation được gắn lên loại element nào.
- `proxy`: object đứng giữa caller và target.
- `heap dump`: snapshot object trên heap.

## 8. Bài tập nhỏ để tự gõ lại

1. Tạo `@MyComponent`.
2. Tạo `UserService` có annotation.
3. Tạo `PlainHelper` không có annotation.
4. Viết scanner nhận `Class<?>` và check annotation.
5. Viết dynamic proxy log method call cho interface `Calculator`.

Học tiếp theo: khi vào Spring, hãy nhớ annotation chỉ là metadata; container mới là thứ đọc metadata và tạo behavior.
