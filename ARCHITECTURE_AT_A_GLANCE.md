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
DB[("Multi-Company Database<br/>(business-scoped)")]
BAK["Automated Backup<br/>+ Disaster Recovery"]
end

subgraph Infra["Infrastructure — Windows Server / Apache Tomcat"]
SRV["Application Server"]
end

subgraph External["External Systems"]
ACC["Tally Accounting<br/>(data exchange)"]
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

## Multi-company model

```mermaid
flowchart LR
PLATFORM["One Platform Instance"]

PLATFORM --> B1["Company 1"]
PLATFORM --> B2["Company 2"]
PLATFORM --> B3["Company 3"]
PLATFORM --> Bn["... Company N<br/>(added by configuration)"]

CONFIG["Configuration Layer<br/>(menus, screens, access, rules)"] -.->|"drives"| PLATFORM
```

*Adding a company is a configuration exercise, not a development project. 8 companies run on it today.*

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

subgraph Insight["Reporting & MIS"]
RPT2["150+ Reports"]
MIS["MIS Dashboards"]
end

PROC --> AP
SALES --> AR
INV --> PROC
INV --> SALES
PROD --> INV
FF --> SALES

Core --> RPT2
Finance --> RPT2
Core --> MIS
Finance --> MIS
```

## Design choices that let it grow

- **Isolation by design** — every business unit's data is scoped, so one company's data never bleeds into another's
- **Config over code** — no per-company forks of the codebase
- **Split admin/support responsibility** — business admins manage their own users, codes, and parameters; IT support has a dedicated panel for platform-level troubleshooting
- **Backup/DR as a requirement** — the platform runs live manufacturing and distribution operations

---

*Database schema, stored procedures, and screen-level field specifications are intentionally excluded from this diagram.*
