# Small, Focused Agents

**Principle 1 of the Arti Agent Stack**

**Definition**

Each agent should have one clear responsibility. This principle draws directly from the Single Responsibility Principle in software engineering. An agent that tries to do everything becomes hard to control, test, debug, and evolve.

## Why It Matters

* **Predictability**: With tightly scoped behavior, an agent becomes easier to understand and anticipate.

* **Composability**: You can chain or orchestrate multiple small agents to achieve complex behavior, without creating entangled logic.

* **Debuggability**: Failures are easier to isolate when an agent has one job.

* **Reuse**: Agents designed with tight scope can be reused in different workflows or products.

## In Practice

* Don’t overload an agent with multiple roles (e.g., parsing input, making decisions, executing actions).

* If an agent needs to handle multiple steps, break those steps into dedicated sub-agents or modules.

* Keep the mental model simple: a good test is whether you can describe the agent's responsibility in one sentence.

## Example
**Bad**:

> A single SupportAgent that reads tickets, classifies them, retrieves context, formulates answers, and sends replies.

**Good**:

> A ClassifierAgent that assigns ticket category.

> A ContextBuilder that retrieves relevant history.

> A ResponderAgent that generates replies based on intent and context.


## Related Concepts

* Microservices architecture

* Unix philosophy: "Do one thing well."

* Mixture of experts (task-specific routing)

This principle is foundational to both scalability and maintainability. Small, focused agents are the atomic units of Cognitive Agentic Architecture.

