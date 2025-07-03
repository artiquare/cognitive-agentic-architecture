# Prompt = Code  (Principle 5)

> **Prompts are system‑critical ‑ treat them like code, not copy‑paste.**

Too many agentic systems rely on brittle, inline strings with no versioning, tests, or error‑handling. In production that’s a liability.

---

## Rule of Thumb

| Attribute         | Requirement                                                          |
| ----------------- | -------------------------------------------------------------------- |
| **Versioned**     | Every change tracked, reviewable & rollback‑safe                     |
| **Parameterized** | Use templates + typed variables – never hard‑code values             |
| **Testable**      | Unit tests & eval harnesses cover edge‑cases & regressions           |
| **Fallback‑safe** | Graceful recovery when the LLM returns invalid JSON / low‑confidence |

## Why It Matters

Prompts *control* behaviour. If they drift, your system drifts. Treating prompts as code makes behaviour **reproducible**, **auditable**, and **continuously improvable**.

## Implementation Checklist

1. **Separate files** – store prompts in their own repo path, not inline.
2. **Schema annotation** – document inputs / outputs in the file header.
3. **Prompt registry** – map `prompt_id` → file path → semantic version.
4. **Automated tests** – snapshot tests, edge‑case fuzzing, schema validation.
5. **Promotion workflow** – PR review → staging eval → prod rollout.

```yaml
# eg: prompts/support/respond_v1.yaml
id: support/respond
version: 1.2.0
inputs:
  - user_query: str
  - ticket_context: TicketContext
outputs: SupportResponse
--- |
You are SupportBot …
{{ticket_context}}
Question: {{user_query}}
```

## Common Pitfalls

* String concatenation: `f"You are… {history} {query}"`
* Inline prompt edits during hot‑fixes, no tests → silent regressions.
* One giant prompt that does context building, planning & execution.

## Real‑World Analogy

If your backend logic lived in Markdown files with no version control, tests, or review — you’d never ship. Prompts deserve the same discipline.

> *“An agent is only as stable as its weakest prompt. Ship prompts like you ship code.”*
