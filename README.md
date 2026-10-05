# Java Backend Core-First

A 14-week Java Backend learning roadmap for frontend developers, focused on Java core, JVM, concurrency, testing, and Spring Boot fundamentals.

This repo is built for a React/Next.js developer who wants to move into Java Backend without jumping straight into framework magic. The path starts with Java language fundamentals, then moves through collections, concurrency, JVM internals, testing, design, and finally Spring Boot.

## Start Here

- Read the full roadmap: [ROADMAP.md](ROADMAP.md)
- Begin week 1: [phase-01-java-language/week-01](phase-01-java-language/week-01/README.md)
- Study each week in order: read the lesson files, do the exercises, then use the checklist and AI review prompt.

## Structure

| Phase | Weeks | Focus |
| --- | --- | --- |
| [Phase 01](phase-01-java-language/README.md) | 1-3 | Java language, OOP, generics, modern Java, exceptions |
| [Phase 02](phase-02-collections-stream-io/README.md) | 4-5 | Collections, Stream, Optional, `java.time`, NIO.2 |
| [Phase 03](phase-03-concurrency/README.md) | 6-8 | Threads, locks, executors, CompletableFuture, virtual threads |
| [Phase 04](phase-04-jvm-performance/README.md) | 9-10 | JVM memory, GC, profiling, reflection, proxy |
| [Phase 05](phase-05-testing-design/README.md) | 11-12 | JUnit 5, Mockito, AssertJ, SOLID, design patterns |
| [Phase 06](phase-06-spring-boot-core/README.md) | 13-14 | Spring Boot, REST API, JPA, transaction, integration testing |

## Weekly Rhythm

- 2h learning concepts.
- 4h coding exercises.
- 1h testing, debugging, benchmarking, or refactoring.
- 1h AI review and learning log.

The goal is not to finish files quickly. The goal is to write code, hit compiler/runtime errors, debug them, and explain the design decisions in your own words.

## Requirements

- Java 21, while keeping Java 17 compatibility in mind.
- Maven.
- Git.
- IntelliJ IDEA Community or another Java-friendly IDE.

## AI Usage

Use AI as a mentor and reviewer, not as a code generator for the full solution.

Good prompts:

- "Review this Java code for immutability and equals/hashCode correctness."
- "Explain this concurrency bug with a thread timeline."
- "What test cases am I missing?"
- "Does this Spring transaction boundary make sense?"

Avoid asking AI to write the whole exercise before you have tried it yourself.
