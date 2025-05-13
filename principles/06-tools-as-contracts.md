# Principle 6: Tools as Contracts

## Definition

In Cognitive Agentic Architecture (CAA), tools are not generic callable functions or magic wrappers. They are contracts: clearly defined interfaces with specific intent, input/output expectations, and predictable behavior.

## Why It Matters

LLM agents frequently fail not because the language model is weak, but because the "tools" they call are brittle, untyped, or ambiguous. If a tool can return anything, accept anything, or behave differently depending on hidden context, your agent becomes untrustworthy and untestable.

Treating tools as contracts brings:

* **Reliability**: Typed inputs and outputs mean agents know what to expect.
* **Composability**: Tools can be reused across agents and systems.
* **Observability**: Tool calls are inspectable, auditable, and reproducible.
* **Testability**: Tools can be unit tested independently of LLM behavior.

## How to Apply It

1. **Typed Interfaces**: Define tool inputs and outputs using a schema language (e.g., Pydantic, JSON Schema). Avoid "dicts of anything."
2. **Declare Intent**: Tools must have clearly documented purpose and constraints. What they do, and what they don’t.
3. **Structured Returns**: Never return free-text. Return structured data with status, result, and optional metadata.
4. **Error Handling**: Tools must handle invalid inputs gracefully and return structured errors.
5. **No Side Effects Without Traceability**: If a tool performs external actions (e.g., writes to a DB, sends an email), it must log or emit events that can be traced and audited.

## Real-World Examples

* ❌ **Anti-Pattern**: Tool is a Python function taking arbitrary dict and returning a string.
* ✅ **Correct**: Tool is a callable with a typed input schema, a version tag, a defined output contract, and traceable logs.

## Key Benefits

* Reduces hallucinations by anchoring tool behavior in ground truth.
* Enables versioning and backward compatibility.
* Makes LLM + Tool interaction modular and upgradable.

---

> Treat tools like APIs, not prompts. Contracts, not magic.

Tools are not the magic behind your agent. They're the plumbing. And plumbing needs specs.
