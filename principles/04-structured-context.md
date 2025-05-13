# Principle: Structured Context

**No string soup. Use typed inputs and versioned construction.**

### Why It Matters

Agent systems break when context becomes a blob of concatenated strings with unclear boundaries. This "string soup" makes it:

* Impossible to debug
* Hard to reuse
* Fragile to small prompt changes
* Difficult to reason about what the agent actually sees

Structured context is the foundation of robust, testable, and maintainable agentic systems.

### What It Looks Like

Instead of feeding your agent a big prompt with hard-coded string concatenations, define context as typed data models with:

* **Clear input fields**: Defined via schemas (e.g., Pydantic models)
* **Modular construction**: Composed from reusable prompt modules
* **Explicit roles**: System/user/assistant/message types
* **Version control**: Prompt modules are versioned and tracked like code

### Implementation Principles

* Treat context like an API contract: strict inputs, no surprises
* Separate source data from prompt construction (e.g., don’t build prompts inside tools)
* Normalize context sources: metadata, user history, state, memory should all be structured inputs
* Build prompt construction pipelines: input -> structure -> template -> message

### Antipatterns

* `f"You are a helpful assistant. {history} {query}"`
* Mixing retrieval, templating, and logic inline
* Manually appending strings to build chat history

### Examples

**Bad:**

```python
prompt = f"""
You are helping with a support issue.

History:
{ticket_history}

Query:
{user_query}
"""
```

**Good:**

```python
class SupportContext(BaseModel):
    ticket_history: List[TicketEntry]
    user_query: str

support_prompt = prompt_builder.build("support/v1", SupportContext(...))
```

### Summary

Structured context is not an optimization. It's a requirement. Without it, you’re building black boxes held together by string formatting and hope.

> Structure your context, or your context will structure your failures.
