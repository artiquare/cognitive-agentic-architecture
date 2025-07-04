Execution Flow: Retry → Escalate → Resume

```mermaid
sequenceDiagram
    participant U as User / External Event
    participant A as Agent (Behavior Layer)
    participant T as Typed Tool (Execution Layer)
    participant H as Human Supervisor (Collaboration)
    participant O as Observability Bus

    U->>A: Task request (ContextObject)
    A->>T: Call tool 〈v1〉
    T-->>A: Error (timeout)
    A->>O: Trace: tool_error
    A->>T: Retry tool 〈v2〉
    T-->>A: Error (invalid params)
    A->>O: Trace: retry_failed
    A->>H: Escalate / ask approval
    H-->>A: Approve with fix
    A->>T: Call tool 〈v3 – corrected〉
    T-->>A: Success (StateChange)
    A->>O: Trace: success
    A-->>U: Done (Result)
```

Legend

ContextObject → structured, typed input from Context Layer

Tool 〈v⟩ → versioned, contract‑driven API

Trace events flow into Observability for replay & metrics

This diagram demonstrates explicit control flow, structured retries, human‑in‑the‑loop escalation, and full observability — core tenets of CAA.
