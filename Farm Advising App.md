# Farm Advising Web — Authentication Flow

One central path, top to bottom. Each decision feeds the next state on a straight line.
Failure and loop-back cases end in a rounded marker (↺ = returns to an earlier step) so no lines cross the main flow.

**Legend:** green = frictionless / success · orange = step-up & recovery · red = blocked

```mermaid
flowchart TD
    A([Open web app]) --> B[/Enter phone + PIN/]
    B --> C{PIN correct?}

    C -- No --> C1{Attempts<br/>remaining?}
    C1 -- Yes --> C2([↺ Back to PIN entry])
    C1 -- No --> C3[Soft lockout · cooldown]
    C3 --> C4([↺ Retry after cooldown])

    C -- Yes --> D{Trusted device?}

    D -- No · new device --> E[Send OTP to<br/>registered number]
    E --> E1{OTP correct?}
    E1 -- No --> E2([Resend rate-limited<br/>or account recovery])
    E1 -- Yes --> E3[Bind device<br/>+ notify user]
    E3 --> F

    D -- Yes --> F["READ session granted<br/>PIN + trusted device"]

    F --> G{Action type?}
    G -- View / log / update farm --> G1([Zero friction · stay in READ])
    G -- Change number / delete account --> H[Step-up MFA<br/>OTP or passkey]
    H --> H1{Verified?}
    H1 -- No --> H2([Deny · stay in READ])
    H1 -- Yes --> H3([Commit action ✓<br/>then back to READ])

    classDef happy fill:#dcefdd,stroke:#3f8b52,color:#173d24;
    classDef warn  fill:#fbe6d2,stroke:#cc7a3b,color:#5a3410;
    classDef block fill:#f6d5d5,stroke:#b33a3a,color:#5a1414;
    class F,G1,E3,H3 happy;
    class C1,C4,E,E1,E2,H,H1 warn;
    class C3,H2 block;
```

**Also handled (kept out of the diagram to keep it clean):** forgot PIN → reset via OTP; lost SIM / number change → field-officer or knowledge-based recovery, then re-bind; session expiry → re-a[...]