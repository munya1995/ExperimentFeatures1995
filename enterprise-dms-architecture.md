# Enterprise DMS Platform Architecture

This diagram shows the shared platform services governing the Enterprise Document Management System (DMS), departmental workspaces using the central repository, and records/compliance controls applied to managed content.

```mermaid
flowchart TB
    subgraph SERVICES[Common Platform Services]
        direction LR
        CT[Content Types]
        TAX[Enterprise Taxonomy]
        RBAC[Role-Based Access Control]
        RET[Retention Policies]
        AUDIT[Audit and Reporting]
    end

    DMS[(Enterprise DMS<br/>Department Site Hub<br/>Central Document Repository<br/>Department-Level Libraries<br/>Permissions and Governance)]

    subgraph DEPARTMENTS[Departmental Workspaces]
        direction LR
        LEGAL["Legal & Compliance<br/>Contracts · Regulatory Filings<br/>Board Services · Policies"]
        HR["Human Resources<br/>Personnel Files · Recruitment<br/>Employment Contracts"]
        FIN["Finance<br/>Invoices · Statements<br/>Tax Records · Close Packs"]
        PROC["Procurement<br/>Tenders · Bid Evaluations<br/>Vendor Records"]
        RISK["Risk<br/>Risk Registers · Incident Reports<br/>BCP Plans"]
        OPS["Operations<br/>SOPs · Asset Records<br/>Maintenance Records · Permits"]
        BOARD["Executive & Board<br/>Board Packs · Minutes<br/>Resolutions · Statutory Registers"]
    end

    subgraph RECORDS[Records & Compliance]
        direction LR
        LABELS[Retention Labels]
        HOLD[Legal Hold]
        DISPOSITION[Disposition Review]
        REPORT[Audit Reporting]
    end

    SERVICES -->|Configure and govern| DMS
    DMS <-->|Create, access, and manage documents| DEPARTMENTS
    DMS -->|Apply lifecycle and compliance controls| RECORDS

    classDef service fill:#f1f6fc,stroke:#3478c8,color:#102a43,stroke-width:1.5px;
    classDef department fill:#ffffff,stroke:#7b9dbd,color:#102a43,stroke-width:1.5px;
    classDef compliance fill:#fff8e9,stroke:#e49a00,color:#5b3b00,stroke-width:1.5px;
    classDef repository fill:#24577f,stroke:#123d60,color:#ffffff,stroke-width:2px;

    class CT,TAX,RBAC,RET,AUDIT service;
    class LEGAL,HR,FIN,PROC,RISK,OPS,BOARD department;
    class LABELS,HOLD,DISPOSITION,REPORT compliance;
    class DMS repository;
```

**Document flow:** Departments create and manage content in their workspaces. The central DMS provides the shared repository and permissions, using common platform services. Retention, legal hold, disposition, and audit controls govern records throughout their lifecycle.
