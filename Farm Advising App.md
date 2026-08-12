# Farm Advising Web — Authentication Flow

One central path, top to bottom. Each decision feeds the next state on a straight line.
Failure and loop-back cases end in a rounded marker (↺ = returns to an earlier step) so no lines cross the main flow.

After a correct OTP the user is asked **"Trust this device?"** (Facebook-style). Choosing **yes** remembers the
device for **5 days** — no OTP for login or normal use in that window. Sensitive config changes still require
step-up even on a trusted device.

**Legend:** green = frictionless / success · orange = step-up & recovery · red = blocked

```mermaid
flowchart TD
    A([Open web app]) --> B[/Enter phone + PIN/]
    B --> C{PIN correct?}

    C -- No --> C1{Attempts<br/>remaining?}
    C1 -- Yes --> C2([&#8634; Back to PIN entry])
    C1 -- No --> C3[Soft lockout &#183; cooldown]
    C3 --> C4([&#8634; Retry after cooldown])

    C -- Yes --> D{Trusted device?<br/>within 5-day window}

    D -- No / expired --> E[Send OTP to<br/>registered number]
    E --> E1{OTP correct?}
    E1 -- No --> E2([Resend rate-limited<br/>or account recovery])
    E1 -- Yes --> T{Trust this device?}
    T -- Yes, 5 days --> E3[Remember device 5 days<br/>+ notify user]
    T -- Not now --> E4[Grant this session only<br/>OTP next login]
    E3 --> F
    E4 --> F

    D -- Yes --> F["READ session granted<br/>PIN + trusted device"]

    F --> G{Action type?}
    G -- View advice / log activity --> G1([Zero friction &#183; stay in READ])
    G -- Update farm / change number / delete account --> H[Step-up MFA<br/>OTP or passkey]
    H --> H1{Verified?}
    H1 -- No --> H2([Deny &#183; stay in READ])
    H1 -- Yes --> H3([Commit action &#10003;<br/>then back to READ])

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
