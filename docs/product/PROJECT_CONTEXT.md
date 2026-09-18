# Project Context

## Business context

Rama Pure Water Pvt. Ltd. operates the manufacturing process for gravity water filtration products sold under the RAMA brand in India and Phoenix-branded products for international markets including the UK, USA, and France.

The factory operation needs one simple system connecting:

Production → Inventory → QC → Packaging → Dispatch → Customer Complaints → CAPA → Management Reporting.

The same manufacturing operation may support different brand/market labels. Brand and market must therefore be explicit data dimensions.

## Core problem

Operational information is spread across spreadsheets, departmental updates, customer support information, production records, and manual communication.

Management needs one reliable application that can answer:

1. What stock do we have now?
2. What changed yesterday / 7 days / 30 days?
3. What is being produced?
4. What is short?
5. What is blocked?
6. What failed QC?
7. What is packed?
8. What must dispatch and by when?
9. What customer complaints are reaching the factory?
10. What CAPA is open?
11. Has a corrective action actually worked?
12. Which department/user owns the next action?

## User profile

Expected use:

- approximately 20 users
- normally fewer than 10 concurrent users
- desktop-first web application
- factory and office users with mixed technical skill levels

The product must be easy to learn and hard to misuse.

## Simplicity requirement

The interface must be understandable by a first-time, low-technical-skill factory operator.

The system should make the correct action obvious.

Examples:

- "Add Production Entry"
- "Upload Excel"
- "Send to QC"
- "Put on Hold"
- "Release Batch"
- "Allocate Stock"
- "Confirm Dispatch"
- "Upload Complaints"
- "Open CAPA"
- "Close CAPA"

Do not require users to understand database terms or ERP jargon.

## Data entry principle

Where operational records are created, support both:

1. Single/manual entry
2. Excel/CSV bulk upload using a controlled template

Bulk upload must not bypass validation or approval rules.

## Complaint intake principle

The system must not depend on reading Gmail for complaint intake.

Customer Support is responsible for entering complaints into the application by:

- single complaint entry, or
- Excel/CSV bulk upload.

Ticket number is mandatory.

## Reporting principle

The dashboard must answer:

- current state
- what changed
- why it changed
- what needs attention
- who owns the action

Dashboards are not merely charts. They must drive action.

## Development/deployment direction

Development:
- local computer
- Docker
- localhost

MVP:
- Hostinger VPS

Future:
- AWS or equivalent dedicated/managed infrastructure

The same application codebase must remain portable across environments.
