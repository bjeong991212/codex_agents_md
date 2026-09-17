# AGENTS.md

Behavioral guidelines for coding agents working in this repository.

These instructions prioritize correctness, small diffs, and maintainability over speed. For trivial tasks, use judgment and avoid unnecessary process.

## 1. Think Before Coding

Do not assume silently. Surface important tradeoffs.

Before implementing:
- State material assumptions explicitly.
- If multiple valid interpretations exist and the choice affects correctness, ask before editing.
- If the ambiguity is minor, make the smallest reasonable assumption and state it.
- If a simpler approach solves the request, prefer it.
- Push back when the requested approach seems unnecessarily complex, risky, or inconsistent with the existing codebase.

## 2. Simplicity First

Write the minimum code that solves the requested problem.

Avoid:
- Features beyond what was asked.
- Abstractions for single-use code.
- Flexibility or configurability that was not requested.
- Defensive error handling for scenarios that cannot occur based on the current code path, types, or documented inputs.
- Large rewrites when a small change would solve the issue.

If the solution is much longer than necessary, simplify before finishing.

## 3. Surgical Changes

Touch only what is required for the task.

When editing existing code:
- Do not improve adjacent code, comments, formatting, or structure unless required.
- Do not refactor unrelated code.
- Match the existing style, even if another style would be preferable.
- If unrelated dead code or issues are noticed, mention them instead of changing them.

When your changes create unused code:
- Remove imports, variables, functions, or comments that your own changes made obsolete.
- Do not remove pre-existing dead code unless explicitly asked.

Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

Turn tasks into verifiable goals.

Examples:
- "Add validation" → add or update tests for invalid inputs, then make them pass.
- "Fix the bug" → reproduce the bug with a test or minimal case, then fix it.
- "Refactor X" → preserve behavior and verify tests pass.

For multi-step tasks, use a brief plan:

```text
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]