# Mentor Explanation Style

Use this style when writing or rewriting lesson files in this repo.

The learner is a frontend developer with React/Next.js experience who knows JavaScript and Python, but is still new to Java. The writing should feel like a Java Backend mentor explaining slowly enough that the learner does not need to leave the document and ask AI again for the basics.

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
