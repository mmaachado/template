---
name: karpathy-guidelines
description: Behavioral guidelines to reduce common programming errors when using LLMs. Apply these when writing, reviewing, or refactoring code to avoid over-complication, make surgical changes, state assumptions explicitly, and define verifiable success criteria.
license: MIT
---

# Karpathy Guidelines

Behavioral guidelines to reduce common programming errors when using LLMs, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on the pitfalls of programming with LLMs.

**Commitment:** These guidelines prioritize caution over speed. Use common sense for trivial tasks.

## 1. Think before you code

**Don't make assumptions. Don't hide confusion. Surface trade-offs.**

Before implementing:

- State your assumptions explicitly. If in doubt, ask.
- If multiple interpretations exist, present them—don't choose silently.
- If a simpler approach exists, say so. Push back when appropriate.
- If something isn't clear, stop. Name what is confusing. Ask.

## 2. Simplicity first

**The minimum amount of code to solve the problem. Nothing speculative.**

- No features beyond what was requested.
- No abstractions for single-use code.
- No unrequested "flexibility" or "configurability."
- No error handling for impossible scenarios.
- If you write 200 lines but it could be done in 50, rewrite it.

Ask yourself: "Would a senior engineer consider this overly complex?" If the answer is yes, simplify it.

## 3. Surgical changes

**Touch only what is necessary. Clean up only your own mess.**

When editing existing code:

- Do not "improve" adjacent code, comments, or formatting.
- Do not refactor what isn't broken.
- Follow the existing style, even if you would do it differently.
- If you notice unrelated dead code, mention it—do not delete it.

When your changes create orphans:

- Remove the imports/variables/functions that YOUR changes rendered useless.
- Do not remove pre-existing dead code unless requested.

The test: every changed line must be directly linked to the user request

## 4. Goal-oriented execution

**Define success criteria. Repeat the cycle until verification.**

Turn tasks into verifiable goals:

- "Add validation" → "Write tests for invalid inputs and then make them pass"
- "Fix the bug" → "Write a test that reproduces it and then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, define a brief plan:

```
1. [Step] → verify: [checked]
2. [Step] → verify: [checked]
3. [Step] → verify: [checked]
```

Robust success criteria allow for independent iterations. Fragile criteria ("make it work") require constant clarification.
