# Enterprise DMS — Microsoft 365 Architecture

This flow diagram represents the **second image**: business users work with a SharePoint Online-based Enterprise DMS, which connects to Microsoft 365 services and Microsoft Dynamics 365 Finance & Operations. Governance applies across the platform.

```mermaid
flowchart TB
    GOV["Governance layer<br/>Records retention schedule · Information classification<br/>CDPA [Chapter 12:07] · ISO 27001 · Data protection"]

    subgraph PLATFORM[Enterprise DMS — SharePoint Online]
        direction TB
        LIB[Document libraries & department repositories]
        META[Content types, metadata & term store]
        WORK[Approval workflows & notifications]
        RECORDS[Version history, retention & audit]
        LIB --> META --> WORK --> RECORDS
    end

    subgraph USERS[Business Users]
        direction TB
        LEGAL[Legal & Compliance]
        HR[Human Resources]
        FIN[Finance]
        PROC[Procurement]
        RISK[Risk]
        OPS[Operations]
        BOARD[Executive & Board]
    end

    subgraph M365[Microsoft 365 Services]
        direction TB
        PA["Power Automate<br/>Workflows & approvals"]
        PBI["Power BI<br/>Department dashboards"]
        TEAMS["Microsoft Teams<br/>Collaboration"]
        PURVIEW["Microsoft Purview<br/>Audit, DLP, retention"]
        ENTRA["Microsoft Entra ID<br/>Identity & access"]
    end

    D365["Microsoft Dynamics 365 Finance & Operations<br/>Documents linked to transactions, vendor and regulatory records"]

    GOV -. governs .-> PLATFORM
    USERS -->|Content in| PLATFORM
    PLATFORM -->|Content out| USERS
    PLATFORM <--> PA
    PLATFORM <--> PBI
    PLATFORM <--> TEAMS
    PLATFORM <--> PURVIEW
    ENTRA -->|Identity & access| PLATFORM
    PLATFORM <-->|Attach / link documents to records| D365

    classDef governance fill:#f3f5f8,stroke:#b8c2ce,color:#344054,stroke-width:1px;
    classDef platform fill:#dfebf7,stroke:#0780d4,color:#17365d,stroke-width:2px;
    classDef users fill:#f2f7fc,stroke:#a8c9ec,color:#17365d,stroke-width:1px;
    classDef services fill:#ffffff,stroke:#9cc5ef,color:#25364a,stroke-width:1px;
    classDef dynamics fill:#f1f7ff,stroke:#6ca8e8,color:#25364a,stroke-width:1.5px;

    class GOV governance;
    class LIB,META,WORK,RECORDS platform;
    class LEGAL,HR,FIN,PROC,RISK,OPS,BOARD users;
    class PA,PBI,TEAMS,PURVIEW,ENTRA services;
    class D365 dynamics;
```

**Flow summary:** Business users send content to and retrieve content from the DMS. The SharePoint Online platform organizes documents in departmental repositories, applies metadata and workflow, and maintains version and retention records. Microsoft 365 services support automation, reporting, collaboration, compliance, and identity. Documents can be attached or linked to Dynamics 365 records. The governance layer applies across the architecture.
