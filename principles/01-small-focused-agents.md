# Principle 1 – Small, Focused Agents

> **One agent, one responsibility.**

Borrowed straight from the Single‑Responsibility Principle, this rule keeps cognitive agents predictable, testable, and reusable.

---

## Why it matters

| Pain when an agent does *everything*                    | Benefit of a tight scope                           |
| ------------------------------------------------------- | -------------------------------------------------- |
|  Prompt spaghetti; impossible to reason about behaviour |  Clear mental model (one‑sentence job description) |
|  Changing one feature breaks five others                |  Safer refactors & easy A/B swaps                  |
|  Debugging is whack‑a‑mole across hidden branches       |  Failures localised; faster MTTR                   |
|  Can’t compose agents into larger workflows             |  Lego‑block composition of expert agents           |

---

## Implementation checklist

* [ ] **Name reflects job** – e.g. `InvoiceClassifier`, not `MegaSupportBot`.
* [ ] Input & output schemas match the single concern.
* [ ] Unit tests cover the one responsibility & edge‑cases only.
* [ ] If more than one tool is needed → consider splitting into sub‑agents.

---

## Example split

| Anti‑pattern                                                  | CAA‑approved split                                                              |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `SupportAgent` → classify, search KB, draft reply, send email | `TicketClassifier`  → `ContextRetriever`  → `ReplyGenerator`  → `MessageSender` |

Each sub‑agent can now evolve, be reused elsewhere, or be replaced by deterministic code.

---

## Related concepts

* Unix philosophy: *“Do one thing well.”*
* Micro‑services & *service boundaries*.
* Mixture‑of‑Experts routing.

---

> *“Big agents impress in demos; small agents survive in prod.”*
