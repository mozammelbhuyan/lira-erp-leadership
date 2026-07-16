# Architecture at a Glance

A conceptual view of the platform — technology choices and structural patterns, without database schema, stored procedures, or screen-level specifications.

## System layers

```mermaid
flowchart TB
    subgraph Client["Access Layer"]
        PC["Desktop / Browser"]
        TAB["Tablet"]
        MOB["Mobile"]
    end

    subgraph App["Application Layer — Java / Spring Boot"]
        API["Spring Boot Services"]
        AUTH["Role-Based Access Control"]
        RPT["Crystal Reports Engine<br/>(150+ reports)"]
        ADMIN["Administration Panel"]
        ITOPS["IT Support Panel"]
    end

    subgraph Data["Data Layer — MS SQL Server"]
        DB[("Multi-Tenant Database<br/>(business-scoped)")]
        BAK["Automated Backup<br/>+ Disaster Recovery"]
    end

    subgraph Infra["Infrastructure — Windows Server / IIS+Apache"]
        SRV["Application Server"]
    end

    subgraph External["External Systems"]
        ACC["Existing Accounting Software<br/>(data sync, not replaced)"]
    end

    PC & TAB & MOB --> API
    API --> AUTH
    API --> RPT
    API --> ADMIN
    API --> ITOPS
    API --> DB
    DB --> BAK
    API -.->|"sync layer"| ACC
    API --> SRV
```

## Multi-company scaling model

```mermaid
flowchart LR
    PLATFORM["One Platform Instance"]

    PLATFORM --> B1["Company 1"]
    PLATFORM --> B2["Company 2"]
    PLATFORM --> B3["Company 3"]
    PLATFORM --> Bn["... Company N<br/>(no code changes needed)"]

    CONFIG["Configuration Layer<br/>(menus, screens, access, rules)"] -.->|"drives"| PLATFORM
```

*Adding company #9 is a configuration exercise, not a development project — this is the core design bet that made "unlimited company scalability" real rather than aspirational. 8 companies are running on it today.*

## Functional module map

```mermaid
flowchart TB
    subgraph Core["Transactional Core"]
        PROC["Procurement"]
        SALES["Sales & Distribution"]
        INV["Inventory"]
        PROD["Production"]
        FF["Field Force"]
    end

    subgraph Finance["Finance"]
        AR["Accounts Receivable"]
        AP["Accounts Payable"]
    end

    subgraph Intelligence["Planning & Insight"]
        FC["Forecasting"]
        RPT2["150+ Reports"]
    end

    PROC --> AP
    SALES --> AR
    INV --> PROC
    INV --> SALES
    PROD --> INV
    FF --> SALES

    Core --> RPT2
    Finance --> RPT2
    SALES --> FC
    INV --> FC
```

## Why this held up in production

- **Isolation by design** — every business unit's data is scoped, so issues in one company's data never bleed into another's
- **Config over code** — the biggest scaling risk in multi-company ERPs is usually "just fork it per client"; this platform deliberately avoided that trap
- **Split admin/support responsibility** — business admins manage their own users, codes, and parameters; IT support has a dedicated panel for platform-level troubleshooting — keeping business users out of technical territory and vice versa
- **Backup/DR treated as a first-class requirement**, not an afterthought, given this platform runs live manufacturing and distribution operations 24/7

---

*Database schema, stored procedures, and screen-level field specifications are intentionally excluded from this diagram.*
