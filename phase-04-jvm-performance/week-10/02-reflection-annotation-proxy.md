# Reflection, Annotation Và Dynamic Proxy

## Reflection

Reflection cho phép inspect class/method/field runtime.

```java
Class<?> type = user.getClass();
for (Method method : type.getDeclaredMethods()) {
    System.out.println(method.getName());
}
```

Dùng reflection cần cẩn thận:

- chậm hơn direct call.
- có thể phá encapsulation.
- exception nhiều và khó đọc hơn.
- refactor khó hơn nếu dùng string name.

## Annotation

Annotation gắn metadata vào code.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MyComponent {
}
```

Retention:

- SOURCE: chỉ source.
- CLASS: có trong class file, không nhất thiết runtime.
- RUNTIME: đọc được bằng reflection.

Target:

- TYPE.
- METHOD.
- FIELD.
- PARAMETER.

## Mini Scanner

Mục tiêu học:

- tạo annotation.
- gắn lên class.
- dùng reflection kiểm tra class có annotation không.

Bạn không cần viết classpath scanner hoàn chỉnh như Spring.

## Dynamic Proxy

Proxy bọc object để thêm behavior quanh method call.

Use case:

- logging.
- transaction.
- security check.
- metrics.

Ví dụ mental model:

```text
caller -> proxy -> target
```

Proxy có thể chạy code trước/sau target method.

## Liên Hệ Spring

Spring dùng annotation và reflection/proxy cho nhiều thứ:

- component scanning.
- dependency injection.
- request mapping.
- transaction proxy.
- security proxy.
- AOP.

"Phép màu" chủ yếu là metadata + object lifecycle + proxy + convention.

## Bài Tập Nhanh

Viết:

- `@MyComponent`.
- 2 class có annotation, 1 class không.
- scanner nhận list class và trả class được annotate.
- interface `PaymentService`.
- dynamic proxy log trước/sau method call.
