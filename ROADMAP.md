# Java Backend Core-First Roadmap

## Summary

This is a 14-week Java Backend roadmap for frontend developers. It assumes roughly **8 hours per week** and prioritizes Java core before Spring Boot.

The intended path is:

```text
Java language -> Collections/Stream/I/O -> Concurrency -> JVM/performance -> Testing/design -> Spring Boot core
```

AI is used as a mentor: explain concepts, review code, suggest missing tests, and help read stack traces. The learner still writes the implementation, debugs it, and records the design tradeoffs.

## Weekly Map

| Week | Focus | Start |
| --- | --- | --- |
| 01 | Type system and OOP-style Java | [week-01](phase-01-java-language/week-01/README.md) |
| 02 | Generics, enum, nested classes | [week-02](phase-01-java-language/week-02/README.md) |
| 03 | Modern Java, exceptions, capstone library | [week-03](phase-01-java-language/week-03/README.md) |
| 04 | Collections internals and complexity | [week-04](phase-02-collections-stream-io/week-04/README.md) |
| 05 | Stream, Optional, `java.time`, NIO.2, log processor | [week-05](phase-02-collections-stream-io/week-05/README.md) |
| 06 | Threads, race conditions, `synchronized`, `volatile` | [week-06](phase-03-concurrency/week-06/README.md) |
| 07 | ExecutorService, CompletableFuture, Lock, Atomic | [week-07](phase-03-concurrency/week-07/README.md) |
| 08 | Virtual threads and concurrency capstone | [week-08](phase-03-concurrency/week-08/README.md) |
| 09 | JVM memory, class loading, JIT, GC logs | [week-09](phase-04-jvm-performance/week-09/README.md) |
| 10 | JFR, heap dump, reflection, annotation, proxy | [week-10](phase-04-jvm-performance/week-10/README.md) |
| 11 | JUnit 5, AssertJ, Mockito, parameterized tests | [week-11](phase-05-testing-design/week-11/README.md) |
| 12 | SOLID, design patterns, refactoring | [week-12](phase-05-testing-design/week-12/README.md) |
| 13 | Spring IoC/DI, autoconfiguration, REST API | [week-13](phase-06-spring-boot-core/week-13/README.md) |
| 14 | JPA, transaction, Flyway, integration tests | [week-14](phase-06-spring-boot-core/week-14/README.md) |

## Phase Goals

### Phase 01: Java Language (Weeks 1-3)

Goal: write Java in a Java-native style instead of porting TypeScript habits into Java syntax.

You will study:

- Type system, primitive vs reference, boxing/unboxing, null handling.
- Interface, abstract class, composition vs inheritance.
- `equals`, `hashCode`, `toString`, immutability.
- Generics, wildcard, bounded type, type erasure.
- `enum`, nested/inner/anonymous class.
- Java 17/21: record, sealed class, pattern matching, switch expression, text block.
- Checked vs unchecked exceptions, custom exceptions, try-with-resources.

Deliverable: port a small JS/Python-style library into Java: JSON parser, LRU cache, or event emitter.

### Phase 02: Collections, Stream, and I/O (Weeks 4-5)

Goal: choose Java collections intentionally, use Stream when it improves clarity, and process files safely.

You will study:

- `ArrayList`, `HashMap`, `TreeMap`, `ConcurrentHashMap`.
- Complexity of common operations.
- Stream API, Collector, Optional, functional interfaces.
- `java.time`.
- NIO.2: `Path`, `Files`, streaming file reads.

Deliverable: log processor implemented once with loops and once with Stream, plus a comparison report.

### Phase 03: Concurrency (Weeks 6-8)

Goal: understand Java's multi-threaded model, especially the parts that feel unfamiliar coming from frontend JavaScript.

You will study:

- `Thread`, `Runnable`, lifecycle.
- Race condition, deadlock, visibility bug, starvation.
- `synchronized`, `volatile`, Java Memory Model, happens-before.
- `ExecutorService`, `CompletableFuture`, `Lock`, `Atomic*`, `ConcurrentHashMap`.
- Virtual threads and structured concurrency concepts.

Deliverable: simple thread pool, producer-consumer queue, and rate limiter.

### Phase 04: JVM and Performance (Weeks 9-10)

Goal: understand how Java runs so framework behavior and production issues feel less magical.

You will study:

- Heap, stack, metaspace.
- Class loading.
- JIT and benchmark pitfalls.
- G1/ZGC and GC logs.
- JFR, VisualVM, `jcmd`, heap dump.
- Reflection, annotations, dynamic proxy.

Deliverable: memory leak demo with heap analysis, mini annotation scanner, and dynamic proxy logger.

### Phase 05: Testing and Design (Weeks 11-12)

Goal: write Java code that is testable, maintainable, and close to real backend code style.

You will study:

- JUnit 5.
- AssertJ.
- Mockito.
- Parameterized tests.
- SOLID in practical terms.
- Builder, Strategy, Factory, Observer.
- Refactoring workflow.

Deliverable: refactor an earlier exercise with tests and a refactoring note.

### Phase 06: Spring Boot Core (Weeks 13-14)

Goal: use Spring Boot while understanding what the framework is doing.

You will study:

- IoC/DI.
- Bean lifecycle.
- Autoconfiguration.
- REST API layering.
- DTO vs entity.
- Validation and global error handling.
- JPA/Hibernate, lazy loading, N+1.
- Transaction boundaries.
- Flyway and integration tests.

Deliverable: small REST API with PostgreSQL, Flyway, validation, error responses, transaction, and integration tests.

## Weekly Study Rhythm

Each week follows the same 8-hour rhythm:

- **2h concept study**: read the lesson files and run tiny examples.
- **4h coding**: implement the weekly exercises yourself.
- **1h verification**: write tests, debug, benchmark, or refactor.
- **1h review**: use the AI review prompt and write a learning log.

## Completion Criteria

- Week 03: you can write a small Java library with a clear API and tests.
- Week 05: you can explain when to use `HashMap`, `TreeMap`, Stream, loop, and file streaming.
- Week 08: you can identify and fix race condition/deadlock/visibility issues in small programs.
- Week 10: you can use JVM tools to investigate memory or performance symptoms.
- Week 12: you can improve tests and refactor code with clear tradeoffs.
- Week 14: you can build a small Spring Boot REST API and explain DI, transaction, lazy loading, and N+1.

## Default Assumptions

- Study time: about 8h/week.
- Java version: Java 21, while understanding Java 17 compatibility.
- Build tool: Maven.
- Database for Spring phase: PostgreSQL.
- Spring Boot starts only after Java core, collections, concurrency, JVM, and testing foundations.
