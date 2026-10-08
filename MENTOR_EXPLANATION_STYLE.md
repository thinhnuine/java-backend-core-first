# Mentor Explanation Style

Use this style when writing or rewriting lesson files in this repo.

The learner has four years of frontend experience, is new to Java and backend, and studies about eight hours per week. Do not assume prior Python, SQL, server-side Node.js, Docker, or authentication knowledge. Use the frontend stack they already know for comparisons. The writing should feel like a Java Backend mentor explaining slowly enough that the learner does not need to leave the document and ask AI again for the basics.

## Required Structure For Each Concept

### 1. Ý chính

Explain in 2-3 plain Vietnamese sentences:

- What the concept is.
- What problem it solves.
- Why the learner should care.

Keep English technical terms intact when they are standard terms, such as `primitive`, `reference`, `immutable`, `interface`, `implements`, `override`, `invariant`, `defensive copy`.

### 2. Giải thích code

Walk through the code from the lesson:

- Explain important lines.
- Explain keywords such as `final`, `private`, `public`, `implements`, `extends`, `@Override`, `protected`.
- When useful, show a tiny runnable example of how the code is called.

### 3. Vì sao thiết kế như vậy

Explain the design reason behind the code.

Include one "if we do not do this, what goes wrong" example:

- Show the wrong code.
- Explain the bug or bad result.

### 4. Liên hệ với Frontend

Compare with JS/TS/React only when the comparison is real and useful.

Good comparisons:

- Java `final` field vs JS `const` binding.
- Java `interface` vs TypeScript `interface`.
- Immutable object vs React immutable state update.
- Constructor validation vs validating props/input before using them.

Do not force a frontend analogy when it makes the concept less clear.

### 5. Khi nào dùng và không dùng

Use a short list or table.

Include:

- When to use it.
- When not to use it.
- Warning when overused.

### 6. Bẫy hay gặp

List common mistakes for beginners.

Prefer concrete mistakes:

- using `==` for `String`
- forgetting to initialize a `final` field
- exposing mutable lists
- using inheritance just for code reuse

### 7. Thuật ngữ mới

Define new terms briefly.

Examples:

- `invariant`
- `immutable`
- `defensive copy`
- `reference`
- `boxing`
- `unboxing`

### 8. Bài tập nhỏ để tự gõ lại

Give a small exercise the learner should type manually.

Requirements:

- Do not provide the full final solution unless the lesson is explicitly an answer key.
- Provide enough steps that the learner can proceed.
- Encourage running code/tests locally.

## Tone

- Detailed but easy to understand.
- Practical, not academic.
- Friendly mentor voice.
- Use tables for comparisons.
- Use code blocks for code.
- Java examples should be short and valid for Java 21.
- If source material is inaccurate or misleading, call that out clearly.
- End lesson files with 1-2 next study directions.

## Short Answer Exception

When the learner asks "ngắn gọn thôi" or asks a quick factual question, answer briefly instead of using the full structure.

## Learning Path And Prerequisites

- Follow ROADMAP.md for the new sequence. Old week-* directory names identify modules, not the current study calendar.
- Introduce unfamiliar backend terms before using them; distinguish required material from optional depth.
- State prerequisites, where each runnable example goes, how to run it, expected output, and one observable completion criterion.
- Label incomplete code as a snippet and name its missing context. Never imply every block compiles alone.
- Build Task Manager incrementally. Use library/parser/thread-pool projects as optional practice after their prerequisites.
- Teach basic tests alongside Java behavior, SQL before JPA, and HTTP before framework annotations.
- Explain the limits of JS/TS analogies, especially nullability, String equality, shared state, and shallow immutability.

## Course Outcomes And Evidence

- COURSE_OUTCOMES.md defines required competencies. A local DB API is a midpoint, not the end of this fullstack course.
- Use one eight-hour budget for the selected material each new week. Do not imply every old module, checklist, or lab is required on top of that budget.
- A race demo passing repeatedly is not a proof of thread safety. Explain synchronization and separate observed evidence from correctness reasoning.
- Distinguish complete runnable examples, partial class sketches, intentionally invalid examples, and framework snippets with external prerequisites.
- Record what was actually verified. Reading or compiling a snippet does not establish API/DB/security/container behavior.
