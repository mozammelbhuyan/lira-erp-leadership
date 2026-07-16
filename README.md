# Enterprise Data-Driven ERP — Multi-Company Manufacturing Platform

**Role:** Sole Architect & Technical Owner (as Head of IT & ERP)
**Organization:** Lira Group of Industries — polymer manufacturing, packaging, distribution, and warehousing
**Status:** In stable production, serving 8 companies, zero major incidents since rollout
**My work:** Requirement discovery, full solution design (data model, functional design, screen/UX design, security, deployment architecture), technical leadership through build (by an external development team) and delivery

> Scope note: this write-up covers architecture *patterns*, technology choices, and functional capabilities. It does not include database schema, stored procedures, screen-level field specifications, or any client/transaction data — those remain internal to Lira Group.

📊 **[View the platform overview presentation](./Platform_Overview_Presentation.pptx)** — opens directly in-browser via GitHub's document viewer.

---

## The problem

Lira Group runs multiple manufacturing, packaging, distribution, and warehousing companies under one umbrella. Each business unit had its own disconnected way of tracking purchasing, sales, inventory, and receivables/payables — and critically, the group's finance team still relied on a **separate, existing accounting software package** that couldn't be replaced but had to stay in sync. There was no way to see the group's financial and operational position in real time, and adding a new business unit meant standing up yet another disconnected system.

## What I designed

A **single, config-driven ERP platform** built to scale to an unlimited number of companies without new development for each one — a business is *added*, not *built*. Eight are running on it today.

### Core functional coverage
- **Accounts Receivable & Accounts Payable** — full AR/AP cycle: money receipts, customer/supplier adjustments, opening balances, direct receivable/payable entries, customer profile views
- **Sales & Distribution** — order-to-invoice, delivery tracking, invoice correction workflows, sales admin controls
- **Procurement** — purchase requisition → purchase order → GRN, with both direct and requisition-driven paths
- **Inventory** — multi-warehouse transfers, receive/issue, adjustments, store requisition and acknowledgement workflows, saleable-item and DSM (distributor stock management) views
- **Production** — manufacturing-side tracking integrated with inventory consumption
- **Field Force** — mobile-accessible tools for field sales/distribution staff
- **Forecasting** — demand/sales forecasting to support planning
- **External accounting integration** — a dedicated data-exchange layer keeps the group's existing accounting software package synchronized with this platform, rather than replacing it outright — a pragmatic choice that avoided disrupting finance operations during rollout

### Design principles behind it
- **Fully data-driven.** Menus, screens, user access, and business rules are configuration, not hardcoded per company — this is what makes "add an unlimited number of companies" actually true rather than a marketing line.
- **Config-driven multi-tenancy.** Every business unit is isolated by a business/organization key throughout the data model, so one platform instance serves all 8 companies (and can take more) without per-company forks of the codebase.
- **Admin & IT support separation.** A distinct administration panel (business profile, user/profile management, codes & parameters, data correction tools) sits apart from a **separate IT support panel** for troubleshooting, monitoring, and operational maintenance — so day-to-day business admin and technical support don't collide.
- **Security.** Role-based access control, audit logging (every table carries create/update user and timestamp metadata), and profile-based screen/field permissions.
- **Reliability & continuity.** Built for 24/7 operation, with automated, fast backup and a defined disaster-recovery path — this isn't a "nice to have" for a platform running live manufacturing and distribution operations.
- **Performance & footprint.** Deliberately kept lightweight for responsiveness across branch/depot network conditions, not just head-office connections.
- **Mobile responsive.** Usable from PC, tablet, or phone — important given field force and warehouse users aren't sitting at desks.
- **Reporting depth.** 150+ reports across sales, purchase, inventory, and finance, giving group leadership real, current visibility instead of end-of-month reconciliation.

### Technology stack
- **Language/Framework:** Java, Spring Boot (with Spring Cloud for the distributed/service-oriented pieces)
- **Reporting:** Crystal Reports
- **Database:** Microsoft SQL Server
- **Platform:** Windows Server, served via IIS/Apache

## Delivery model

I ran requirement discovery directly with department stakeholders (sales, accounts, factory/warehouse teams), owned the full design (data model, functional design, screen design, security model), and directed an external development team through build, testing, and bug-fixing. I led UAT, master-data/opening-balance preparation with each department, staff training, and the phased go-live — starting with a pilot company and scaling out.

## Specific business problems this solved

Beyond the general platform capabilities above, this system was purpose-built to close real, recurring operational gaps that had cost the group time and money for years:

- **Undelivered goods problem** — sales orders that left the warehouse but weren't confirmed delivered had no systematic tracking, creating silent leakage. The platform added dedicated undelivered-order tracking with **ageing analysis**, so stuck deliveries surface automatically instead of being discovered during reconciliation.
- **Division-wise period closing with incentive allocation** — closing a month or year per division while correctly allocating sales incentives across teams was a manual, error-prone process. This is now handled systematically within the platform's closing workflow.
- **Field force hierarchy & data access control** — sales/field staff at different hierarchy levels (rep → area manager → regional manager, etc.) previously had no consistent, enforced boundary on what data they could see or touch. The platform enforces **hierarchy-based access permissions**, so each level sees exactly what it should — no more, no less.
- **Invoice correction problem** — correcting a posted invoice without breaking downstream accounting/inventory records was previously risky or manual. A controlled invoice-correction workflow now handles this safely.
- **Inventory ageing** — slow-moving and dead stock across depots was invisible until physical counts. Ageing reports now surface this continuously.
- **Overdue tracking** — customer and supplier overdue balances are tracked and surfaced proactively rather than discovered at month-end.
- **Sales promotion & pricing complexity** — running promotional pricing, discounts, and special terms across multiple companies and product lines was hard to manage consistently. The platform handles this in a structured, auditable way.
- **Digital money receipt & acknowledgement with SMS** — payment receipt and acknowledgement was a manual, paper-heavy process prone to disputes. This is now digital, with **SMS-based acknowledgement** giving both the company and the customer a real-time, verifiable record.
- **Unlimited sales/purchase/inventory groups & units** — the group's product and unit-of-measure structures don't fit a fixed, hardcoded taxonomy. The platform supports **unlimited, configurable groups and units** for sales, purchase, and inventory instead of forcing the business to fit a rigid schema.
- **One company, multiple national jurisdictions** — a single legal entity operating across more than one country/national context needed to handle jurisdiction-specific rules (tax, reporting, compliance) without being split into separate systems. The platform's config-driven, multi-scoped design accommodates this within one company record rather than requiring a workaround.

Each of these was a real, named pain point from stakeholders during requirement discovery — not a generic ERP feature checklist. Solving them is what actually earned the platform's adoption across all 8 companies.

## Outcome

- **8 companies live** on a single platform, architected to scale to more without re-development
- **150+ reports** giving real-time operational and financial visibility across the group
- **Zero major production incidents** since go-live — the platform has proven reliable under real day-to-day manufacturing and distribution load
- **24/7 uptime posture** with fast backup and disaster recovery in place
- Successfully **coexists with the group's existing accounting software** rather than requiring a disruptive rip-and-replace

---

*Related: [ARCHITECTURE_AT_A_GLANCE.md](./ARCHITECTURE_AT_A_GLANCE.md) for a conceptual system diagram, and [CASE_STUDY.md](./CASE_STUDY.md) for the full narrative.*
