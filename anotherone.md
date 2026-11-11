
flowchart TB

    subgraph COMMON["COMMON PLATFORM SERVICES"]
        direction LR
        A[Content Types]
        B[Enterprise Taxonomy]
        C[Role-Based Access Control]
        D[Retention Policies]
        E[Audit and Reporting]
    end

    subgraph DEPARTMENTS["DEPARTMENTAL WORKSPACES"]
        direction LR

        L["Legal & Compliance<br/>Contracts<br/>Regulatory Filings<br/>Board Services<br/>Policies"]

        H["Human Resources<br/>Personnel Files<br/>Recruitment<br/>Employment Contracts"]

        F["Finance<br/>Invoices<br/>Statements<br/>Tax Records<br/>Close Packs"]

        P["Procurement<br/>Tenders<br/>Bid Evaluations<br/>Vendor Records"]

        R["Risk<br/>Risk Registers<br/>Incident Reports<br/>BCP Plans"]

        O["Operations<br/>SOPs<br/>Asset Records<br/>Maintenance Records<br/>Permits"]

        X["Executive & Board<br/>Board Packs<br/>Minutes<br/>Resolutions<br/>Statutory Registers"]
    end

    HUB[("ENTERPRISE DMS<br/>Department Site Hub<br/>Central Document Repository<br/>Department-Level Libraries<br/>Permissions and Governance")]

    subgraph GOVERNANCE["RECORDS & COMPLIANCE"]
        direction LR
        G1[Retention Labels]
        G2[Legal Hold]
        G3[Disposition Review]
        G4[Audit Reporting]
    end

    COMMON --> HUB

    L <--> HUB
    H <--> HUB
    F <--> HUB
    P <--> HUB
    R <--> HUB
    O <--> HUB
    X <--> HUB

    HUB --> GOVERNANCE
