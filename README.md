# Enterprise Data-Driven ERP — Multi-Company Manufacturing Platform

**Role:** ERP Architect & Technical Owner (as Head of IT & ERP)
**Organization:** Lira Group of Industries — polymer manufacturing, packaging, distribution, and warehousing
**Status:** In production since 2023, serving 8 companies; extended in 7 phases
**My work:** Requirement discovery, scope of work, full solution design (data model, functional design, screen design, security, reports, deployment), and technical leadership of freelance Java developers who built to my design

> Scope note: this write-up covers architecture *patterns*, technology choices, and functional capabilities. It does not include database schema, stored procedures, screen-level field specifications, or any client/transaction data — those remain internal to Lira Group.

📊 **[View the platform overview presentation (PDF)](./Platform_Overview_Presentation.pdf)** — opens directly in-browser via GitHub's PDF viewer.

---

## The problem

When I joined Lira Group in 2023, the group's previous business software had failed, and the companies needed a working system to keep sales, stock and receivables moving. Each business unit also tracked purchasing, sales and inventory in its own way, with no group-wide view.

## What I did

**Month one — a working system.** Within one month I took a basic Sales, Inventory and Accounts Receivable system live. I did the requirement analysis, scope of work, blueprint, reports and server deployment myself; freelance Java developers coded it to my design. It was meant as a temporary solution until a larger third-party ERP was chosen.

**Seven phases — a full platform.** Users adopted it well, so management asked me to keep extending it instead of buying a new ERP. Over 7 phases I grew it into a single, config-driven ERP that now serves 8 group companies. New companies are added through configuration, not new development.

### Core functional coverage
- **Accounts Receivable & Accounts Payable** — money receipts, customer/supplier adjustments, opening balances, direct receivable/payable entries, customer profile views
- **Sales & Distribution** — order-to-invoice, delivery tracking, invoice correction workflows, sales admin controls
- **Procurement** — purchase requisition → purchase order → GRN, with both direct and requisition-driven paths
- **Inventory** — multi-warehouse transfers, receive/issue, adjustments, store requisition and acknowledgement workflows, saleable-item and DSM (distributor stock management) views
- **Production** — manufacturing-side tracking integrated with inventory consumption
- **Field Force** — mobile-accessible tools for field sales/distribution staff
- **HR & Payroll** and **MIS dashboards**
- **Tally integration** — a data-exchange layer between the ERP and the group's Tally accounting

### Design principles behind it
- **Data-driven.** Menus, screens, user access, and business rules are configuration, not hardcoded per company.
- **Multi-company by design.** Every business unit is isolated by a business/organization key throughout the data model, so one platform instance serves all 8 companies without per-company forks of the codebase.
- **Admin & IT support separation.** A business administration panel (business profile, user/profile management, codes & parameters, data correction tools) sits apart from a separate IT support panel for troubleshooting, monitoring, and maintenance.
- **Security.** Role-based access control, audit fields on every table (create/update user and timestamp), and profile-based screen/field permissions.
- **Continuity.** Automated backups and a defined disaster-recovery path, because the platform runs live manufacturing and distribution operations.
- **Lightweight and mobile-responsive.** Usable from PC, tablet, or phone, and over branch/depot network conditions, not just head-office connections.
- **Reporting.** 150+ reports across sales, purchase, inventory, and finance.

### Technology stack
- **Language/Framework:** Java, Spring Boot
- **Reporting:** Crystal Reports
- **Database:** Microsoft SQL Server
- **Platform:** Windows Server, Apache Tomcat

## Delivery model

I ran requirement discovery directly with department stakeholders (sales, accounts, factory/warehouse teams), owned the design (data model, functional design, screen design, security model, reports), and directed freelance developers through build, testing, and bug-fixing. I led UAT, master-data and opening-balance preparation with each department, staff training, and each phase's go-live. Since 2026 I also deliver enhancements through AI-assisted development under strict change control (test server, backups, change reports, developer handover).

## Business problems it addresses

These came from stakeholders during requirement discovery across the phases:

- **Undelivered goods** — undelivered-order tracking with ageing analysis, so stuck deliveries surface instead of being found during reconciliation.
- **Division-wise period closing with incentive allocation** — handled within the platform's closing workflow instead of manual spreadsheets.
- **Field force hierarchy & data access** — hierarchy-based permissions (rep → area manager → regional manager) so each level sees only its own data.
- **Invoice correction** — a controlled workflow for correcting posted invoices without breaking downstream accounting and inventory records.
- **Inventory ageing** — ageing reports surface slow-moving and dead stock across depots.
- **Overdue tracking** — customer and supplier overdue balances are tracked and reported.
- **Sales promotions & pricing** — promotional pricing, discounts and special terms handled in a structured, auditable way.
- **Digital money receipts with SMS acknowledgement** — replacing a paper-heavy receipt process.
- **Configurable groups & units** — unlimited, configurable groups and units of measure for sales, purchase, and inventory.

## Outcome

- Started as a one-month stopgap; became the group's ERP through 7 phases
- **8 companies live** on a single platform, with new companies added by configuration
- **150+ reports** for operational and financial visibility across the group
- Works alongside the group's Tally accounting through a data-exchange layer

---

*Related: [ARCHITECTURE_AT_A_GLANCE.md](./ARCHITECTURE_AT_A_GLANCE.md) for a conceptual system diagram, and [CASE_STUDY.md](./CASE_STUDY.md) for the full narrative.*
