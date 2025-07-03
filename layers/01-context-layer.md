# Layer 1 – Context

> **Mission ‑ Turn raw inputs into a typed, versioned *ContextObject*.**

Everything an agent decides (and later does) starts with an accurate, structured understanding of the situation. The Context layer is that understanding. It owns *nothing else*: no planning, no tool calls, no side‑effects.

---

## Why a Dedicated Context Layer?

| Pain if missing                         | Benefit of separation                  |
| --------------------------------------- | -------------------------------------- |
| Prompt spaghetti & hidden string concat | Typed schemas → validation & reuse     |
| Different teams mutate prompts ad‑hoc   | Version‑controlled context builders    |
| Hard to unit‑test “what the agent saw”  | Deterministic `ContextObject` snapshot |

---

## Canonical Inputs & Outputs

| Item                                                | Format               | Notes                           |
| --------------------------------------------------- | -------------------- | ------------------------------- |
| **In**  Raw user text, events, db rows, sensor data | Any                  | Gathered by adapters / gateways |
| **Out** `ContextObject`                             | Pydantic / dataclass | Passes to Behavior layer        |

```python
class ContextObject(BaseModel):
    user_intent: str            # e.g. "open_ticket"
    entities:   dict[str, Any]  # extracted IDs, dates, etc.
    short_mem:  list[str]       # last‑turn tool results
    long_mem:   list[str]       # persisted domain facts
    meta:       dict[str, Any]  # version, locale, model hints
```

---

## Building Context – Core Responsibilities

### 1️⃣ Typed Input Schemas

* Every adapter (Slack, API webhook, IoT stream) maps raw payloads → strongly‑typed fields.
* **Fail‑fast** if validation errors; feed errors to Observability for alerting.

### 2️⃣ Semantic Enrichment

* Entity extraction, ontology mapping, unit normalization.
* Example: “next Tuesday” → ISO date; “press 42 bar” → `{pressure: 4.2 MPa}`.

### 3️⃣ Memory Overlays

* **Short‑term scratchpad**: last tool output, running notes.
* **Long‑term memory**: domain facts, SOP links, previous tickets.
* Overlay user‑specific or session‑specific layers without mutating ground truth.

### 4️⃣ Versioning & Diff

* Each `ContextObject` carries a `context_version` GUID.
* Diffs between versions are logged → enables deterministic replay.

---

## Principles Embodied

* **Context is Typed & Structured** – No raw strings.
* **Prompt ≠ Context** – Prompting happens in Behavior; Context just **stores facts**.
* **Observable Everything** – Context snapshots are first‑class artefacts for debugging.

---

## Checklist for Production

* [ ] JSON‑schema / Pydantic validation on every input.
* [ ] Ontology registry kept in version control.
* [ ] Unit tests covering edge inputs & locale variants.
* [ ] Telemetry: context size, entity extraction accuracy, validation error rate.

---

> *“If the inputs are a mystery, the outputs will be a surprise.”*
