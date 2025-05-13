# Principle 5: Prompt = Code

Prompts are not magic. They are system-critical components and must be treated as first-class code artifacts.

Too many agentic systems rely on fragile, copy-pasted prompt strings with no versioning, no testing, and no error handling. This breaks in production.

**Our Principle:** Prompts are code. They must be:

* **Versioned**: Each change should be tracked, reviewable, and rollback-safe. Treat them like any other module in your system.
* **Parameterized**: Use prompt templates with clearly defined variables. Never hardcode.
* **Testable**: Every prompt should be covered by unit tests and runtime evaluations. Know how it behaves under different inputs.
* **Fallback-safe**: Handle prompt failures gracefully. Define recovery behavior for unclear outputs or hallucinations.

### Why It Matters

Prompts control agent behavior. They're part of the execution logic. If they fail, your system fails. Treating prompts as code makes behavior reproducible, testable, and improvable over time.

### Design Recommendations

* Store prompts in dedicated files with schema and metadata.
* Annotate inputs and expected outputs.
* Write tests for edge cases and regressions.
* Use a prompt registry to manage versions.
* Link each prompt to specific tool interfaces and context expectations.

### Common Pitfalls

* String concatenation without structure.
* Inline prompts inside logic code.
* Blind updates without regression testing.
* No differentiation between similar prompt variants.

### Real-World Analogy

If your backend logic lived in markdown files without version control, tests, or review — you wouldn't trust it. The same applies to prompts.

Agentic systems are only as stable as their weakest prompts. Make prompt management a discipline.
