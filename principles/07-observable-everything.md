# Principle 7 – Observable Everything

> **If you can’t trace it, you can’t trust it.**

Production‑grade agentic systems must expose **every decision, action, and failure**.  Full observability turns opaque AI behaviour into deterministic execution traces that operations teams can debug, audit and improve.

---

## Why It Matters

LLM‑based agents are probabilistic. They often produce plausible yet subtly wrong outputs.  Without visibility into reasoning, context and execution you’re left guessing:

| Hidden Reality                    | Impact on Ops                     |
| --------------------------------- | --------------------------------- |
| Silent prompt regression          | Undetected quality drops          |
| Tool time‑outs swallowed by loops | Latency spikes, runaway costs     |
| Hallucinated state updates        | Data corruption & compliance risk |

Observability provides:

* **Root‑cause analysis** – pinpoint which step failed.
* **Quality control** – measure task‑level success, not token counts.
* **Explainability** – show stakeholders *why* a result was produced.
* **Audit & compliance** – immutable traces for regulators.
* **Iterative improvement** – replay traces against new code or prompts.

---

## What Must Be Observable?

| Question                             | Example Data to Capture                       |
| ------------------------------------ | --------------------------------------------- |
| **What context did the agent have?** | Versioned context object, memory snapshot     |
| **Why did it choose that tool?**     | Planner reasoning trace, tool registry lookup |
| **What did it say vs. what it did?** | Raw LLM output, executed tool call & result   |
| **Where did it fail?**               | Exception type, retry count, fallback path    |

All events share a **correlation‑id** so a single request forms a complete lineage graph from input to final state.

---

## Implementation Blueprint

* **Trace IDs & Span IDs** – propagate through every layer.
* **Structured Logs** – emit JSON logs; avoid free‑text.
* **Deterministic Replay** – re‑run any trace offline for debugging.
* **Dashboards & Alerts** – task success‑rate, MTTR, human‑override frequency.
* **Failure Taxonomy** – classify errors (tool, prompt, plan, state) for analytics.

> "+1" rule: *Every new prompt or tool must emit at least one structured log entry.*

---

## Tools & Ecosystem

* **OpenTelemetry / Langfuse / Honeycomb** for distributed traces.
* **Replayer harness** to feed stored traces back through Context→…→State.
* **Eval pipelines** that score traces with LLM‑as‑judge or rule‑based metrics.

---

## Principles Embodied

* **Observable Everything** – nothing runs without leaving a breadcrumb.
* **Composable Error Handling** – failures propagate as typed events; monitoring hooks trigger retry / escalate flows.

---

### Checklist

* [ ] Structured trace for every layer event
* [ ] Correlation‑id across the full request
* [ ] Deterministic replay script
* [ ] Alert on error rates & cost anomalies
* [ ] KPI dashboard aligned to business success

> *“Telemetry is the contract between AI and operations.”*
