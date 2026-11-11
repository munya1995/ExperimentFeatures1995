flowchart TD
    A([Start]) --> B[Create or upload document]
    B --> C[Validate content type, mandatory metadata, and sensitivity label]
    C --> D{Validation successful?}

    D -->|No| E[Display validation errors]
    E --> B

    D -->|Yes| F[Initiate approval workflow]
    F --> G[Route to approver based on document type]
    G --> H[Notify approver by email or Teams]
    H --> I[Review document and add comments]
    I --> J{Approved?}

    J -->|No| K[Record rejection reason and comments]
    K --> L[Return document to author]
    L --> M[Revise document and resubmit]
    M --> C

    J -->|Yes| N[Record approval decision]
    N --> O[Lock approved version]
    O --> P[Apply audit trail, retention label, and access controls]
    P --> Q[Notify document owner]
    Q --> R[Store in approved department library]

    R --> S{Retention period reached?}
    S -->|No| T[Continue retention and monitoring]
    T --> S

    S -->|Yes| U{Legal hold active?}
    U -->|Yes| V[Maintain document under legal hold]
    V --> U

    U -->|No| W[Initiate disposition review]
    W --> X{Disposition approved?}
    X -->|No| R
    X -->|Yes| Y[Archive or dispose document]
    Y --> Z([End])
