# Principle – Structured Context

> **No string soup. Use typed inputs and versioned construction.**

---

## Why It Matters
Context that lives as a giant f‑string is a time‑bomb:

* 🔍 **Undebuggable** – you can’t tell which part of the soup broke.
* ♻️ **Un‑reusable** – every new workflow copies & pastes more goo.
* 🪢 **Brittle** – small wording changes ripple through hidden concat logic.
* 🧠 **Opaque** – you never really know what the model *saw*.

**Structured context** turns that mush into software‑quality data.

---

## Core Ideas

| Raw Prompt Soup 🥣            | Structured Context 🧩                              |
| ----------------------------- | -------------------------------------------------- |
| `f"You are a bot. {history}"` | `ContextObject(history=list[Msg], user_query=str)` |
| Mixed roles & data            | Clear system / user / tool message boundaries      |
| Version hidden in comments    | Context builders are version‑controlled modules    |
| Manual string ops             | Typed schemas (Pydantic / dataclass) + validators  |

---

## Implementation Guide

1. **Define Schema First**  – Start every agent with a `ContextObject` class.
2. **Modular Builders**      – Separate *source fetch* (e.g. RAG) from *prompt render*.
3. **Version Everything**    – `support/v1`, `support/v2`; prompts are code, ship them as such.
4. **Test in Isolation**     – Unit‑test context builders without invoking the LLM.

```python
class SupportContext(BaseModel):
    ticket: Ticket
    history: list[Message]
    user_query: str

ctx = build_support_context(ticket_id="T‑123", query=text)
rendered = prompt_renderer.render("support/agent_v1", ctx)
```

---

## Antipatterns to Avoid

* Inline concatenation inside business logic.
* Relying on the LLM to *parse* raw JSON blobs embedded in messages.
* Mixing tool descriptions and user history in the same section.

---

## Benefits

* **Predictability** – Same inputs → same rendered prompt.
* **Observability** – Traces show each field; diffs are meaningful.
* **Composability** – Plug new memory or data sources without rewriting prompts.
* **Governance** – Review & audit prompts like code; roll back bad versions fast.

---

> *“Structure your context, or your context will structure your failures.”*
