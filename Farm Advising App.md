# Farm Advising Web — Authentication Flow

One central path, top to bottom. Each decision feeds the next state on a straight line.
Failure and loop-back cases end in a rounded marker (↺ = returns to an earlier step) so no lines cross the main flow.

After a correct OTP the user is asked **"Trust this device?"** (Facebook-style). Choosing **yes** remembers the
device for **5 days** — no OTP for login or normal use in that window. Sensitive config changes still require
step-up even on a trusted device.

**Legend:** green = frictionless / success · orange = step-up & recovery · red = blocked

```mermaid
%%{init: {'flowchart': {'nodeSpacing': 20, 'rankSpacing': 20}}}%%
flowchart TB
    A([Open]) --> B[/Phone + PIN/]
    B --> C{PIN ok?}

    C -- No --> C1{Attempts left?}
    C1 -- Yes --> C2([↺ Back to PIN])
    C1 -- No --> C3[Soft lockout]
    C3 --> C4([↺ Retry after cooldown])

    C -- Yes --> D{Trusted device?}

    D -- No/expired --> E[Send OTP]
    E --> E1{OTP ok?}
    E1 -- No --> E2([Resend / recovery])
    E1 -- Yes --> T{Trust device?}
    T -- Yes --> E3[Remember 5d + notify]
    T -- Not now --> E4[Session only]
    E3 --> F
    E4 --> F

    D -- Yes --> F[READ session]

    F --> G{Action type?}
    G -- View --> G1([Zero friction])
    G -- Update / Delete --> H[Step-up MFA]
    H --> H1{Verified?}
    H1 -- No --> H2([Deny])
    H1 -- Yes --> H3([Commit ✓])

    classDef happy fill:#dcefdd,stroke:#3f8b52,color:#173d24;
    classDef warn  fill:#fbe6d2,stroke:#cc7a3b,color:#5a3410;
    classDef block fill:#f6d5d5,stroke:#b33a3a,color:#5a1414;
    class F,G1,E3,H3 happy;
    class C1,C4,E,E1,E2,T,E4,H,H1 warn;
    class C3,H2 block;
```

**The 5-day trust window:** removes OTP at login and for all low-risk use while active. It does **not** bypass the
step-up gate — updating farm data, changing the registered number, or deleting the account always re-prompts for MFA, trusted device or not.
When the window expires, the next login re-sends an OTP and asks "Trust this device?" again.

**Also handled (kept out of the diagram to keep it clean):** forgot PIN → reset via OTP; lost SIM / number change →
field-officer or knowledge-based recovery, then re-bind; "Not now" or public/shared devices are never remembered.
Guardrails throughout: PIN attempt lockout, OTP rate-limiting, and an SMS alert whenever a new device is trusted.
