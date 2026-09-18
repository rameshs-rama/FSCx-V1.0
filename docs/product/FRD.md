# Functional Requirements Document — FSCx V1.0

## 1. Purpose

FSCx V1.0 is a factory operations and quality-management application for Rama Pure Water Pvt. Ltd. It creates one operational source of truth across production, inventory, quality, packaging, dispatch, customer complaints, CAPA, and management reporting.

## 2. Primary objectives

The application shall:

1. provide current SFG/FG inventory visibility
2. show stock movement over time
3. record planned and actual production
4. control QC release/hold/rework/reject
5. record packaging completion
6. capture dispatch demand and allocation
7. record physical dispatch
8. capture customer complaints manually or in bulk
9. route relevant complaints into investigation/CAPA
10. distinguish legacy complaints from post-CAPA recurrence
11. provide user/role/access management
12. support configurable approvals
13. provide immutable auditability
14. provide simple management dashboards and exception queues
15. allow Excel/CSV import/export

## 3. User Management

### FR-UM-001
Administrator can create, edit, deactivate, and reactivate users.

### FR-UM-002
Users can be assigned one or more roles if enabled by policy.

### FR-UM-003
Permissions are enforced server-side.

### FR-UM-004
Permissions can be controlled by module and action.

### FR-UM-005
Approval levels are configurable.

### FR-UM-006
Role/permission changes are audited.

### FR-UM-007
The system can restrict visibility/actions by brand/market where required.

### FR-UM-008
Inactive users cannot log in or perform actions.

## 4. Masters

Maintain controlled master data including:

- brand
- market
- SKU
- product
- product category
- unit of measure
- warehouse/location
- component/accessory
- complaint category
- QC reason code
- rejection/rework reason
- dispatch status
- supplier where procurement tracking is implemented

Master changes must be auditable.

## 5. Production

### FR-PR-001
Production users can create a single production record.

### FR-PR-002
Production users can download a controlled Excel/CSV template and bulk-upload production data.

### FR-PR-003
System validates required fields, SKU existence, quantities, duplicate references, and dates.

### FR-PR-004
System shows import preview before commit.

### FR-PR-005
System shows row-level import errors.

### FR-PR-006
System records planned quantity and actual quantity separately.

### FR-PR-007
System records batch/work-order linkage.

### FR-PR-008
Inner and outer chamber quantities can be recorded separately where relevant.

### FR-PR-009
Factory users do not manually maintain a filter-set quantity.

### FR-PR-010
Production completion can create the appropriate inventory receipt/state subject to workflow design.

## 6. Inventory / Stores

### FR-IN-001
Inventory is transaction-ledger based.

### FR-IN-002
System supports receipts, issues, production receipt, dispatch issue, transfer, rework, reject/scrap, and adjustment transaction types.

### FR-IN-003
Current balance is derived from transactions.

### FR-IN-004
System distinguishes SFG, FG, QC Hold, Rework, Reject/Scrap, Reserved, and Dispatch Ready where applicable.

### FR-IN-005
Users can create a single inventory transaction if permitted.

### FR-IN-006
Authorized users can bulk-upload controlled inventory transactions.

### FR-IN-007
Stock adjustments capture mandatory reason and audit data.

### FR-IN-008
Approval can be required based on adjustment type/threshold.

### FR-IN-009
Dashboard shows today/current, yesterday movement, 7-day movement, and 30-day movement.

### FR-IN-010
System flags low-stock or shortage risk using configurable rules.

## 7. Quality Control

### FR-QC-001
Produced batch can enter Awaiting QC.

### FR-QC-002
QC can Pass, Hold, Rework, or Reject.

### FR-QC-003
QC result records inspector, date/time, batch, quantity, reason, remarks, and evidence where needed.

### FR-QC-004
QC-held stock cannot become dispatch-ready.

### FR-QC-005
QC release is restricted to authorized roles.

### FR-QC-006
System shows QC queue and aging.

### FR-QC-007
System supports NCR/CAPA initiation from QC findings.

## 8. Packaging

### FR-PK-001
Packaging user records packed quantity and date.

### FR-PK-002
Packaging confirms pack completeness and product/market correctness.

### FR-PK-003
Packaging can record damaged/rejected quantity and reason.

### FR-PK-004
Only QC-released goods can move through normal packaging completion unless exception workflow is explicitly configured.

### FR-PK-005
Packaging completion updates dispatch readiness state.

## 9. Dispatch

### FR-DI-001
Sales can create dispatch demand manually.

### FR-DI-002
Sales can bulk-upload dispatch demand.

### FR-DI-003
Dispatch order records brand, market, customer/channel, SKU, quantity, requested dispatch date, and priority.

### FR-DI-004
Inventory/Stores can allocate available QC-passed stock if authorized.

### FR-DI-005
Allocation cannot exceed usable available stock.

### FR-DI-006
System can generate/record picklist status.

### FR-DI-007
Physical dispatch confirmation records dispatch date and shipment reference.

### FR-DI-008
Creating demand, allocating stock, releasing dispatch, and confirming physical dispatch are separable permissions.

### FR-DI-009
System shows shortage and dispatch-risk alerts.

## 10. Customer Complaints

### FR-CC-001
Customer Support can create a complaint manually.

### FR-CC-002
Customer Support can bulk-upload complaints from Excel/CSV.

### FR-CC-003
Ticket number is mandatory.

### FR-CC-004
Complaint records brand, market, SKU/product, description, category, severity, complaint date, and evidence.

### FR-CC-005
Order/reference and batch are captured where available.

### FR-CC-006
Customer Support can view investigation/CAPA progress without being able to alter QC conclusions.

### FR-CC-007
The MVP does not automatically read Gmail for complaint intake.

## 11. CAPA

### FR-CA-001
Authorized user can open CAPA from complaint, QC, RFD/quality escape, or internal finding.

### FR-CA-002
CAPA records problem statement, containment, root cause, corrective action, preventive action where relevant, owner, target date, implementation date/effective batch, verification, and closure.

### FR-CA-003
CAPA closure requires effectiveness evidence.

### FR-CA-004
Complaint classification compares manufacturing batch/date with CAPA effective date/batch.

### FR-CA-005
Supported classifications include Legacy Market Exposure, Post-CAPA Watch, Confirmed CAPA Recurrence, New Failure Mode, Installation/Usage, and Not Factory Related.

### FR-CA-006
System reports post-CAPA recurrence separately from total historical complaint volume.

## 12. Dashboards

### Senior Management Control Tower

Show concise exception-driven KPIs such as:

- FG QC-passed
- SFG
- QC Hold
- Rework
- dispatch due/at risk
- low stock
- production vs plan
- open critical complaints/CAPA
- overdue CAPA
- post-CAPA recurrence
- legacy market complaint count

### Operational dashboards

Each role sees its own action queue first.

Examples:

Production:
- today's plan
- shortfall
- pending entries

QC:
- awaiting QC
- aging holds
- rework
- CAPA actions

Packaging:
- ready for packaging
- blocked
- packed today

Stores:
- receipts
- low stock
- allocations
- dispatches due

Customer Support:
- complaints submitted
- awaiting factory review
- under CAPA
- closed

## 13. Bulk Import

Every import must provide:

1. template download
2. upload
3. validation
4. error list
5. preview
6. confirmation
7. transactional commit
8. import result summary
9. audit record

Example result:

- 250 rows received
- 232 valid
- 18 errors

Invalid rows must be identifiable and downloadable for correction.

## 14. Audit & Security

Audit at minimum:

- login/security events
- user changes
- role/permission changes
- master changes
- imports
- production correction
- inventory adjustment
- QC hold/release
- dispatch release/confirmation
- CAPA closure

Passwords must be securely hashed.

MFA should be supported for Administrator, Factory Manager, and QC approvers at minimum when practical for the MVP environment.

## 15. Reports / Exports

Users with permission can export relevant filtered data to Excel/CSV.

Reports should include:

- stock movement
- current inventory
- production vs plan
- QC
- packaging
- dispatch
- complaints
- CAPA
- audit trail

## 16. Non-functional requirements

- responsive desktop web UI
- fast enough for fewer than 10 concurrent users
- role-based authorization
- clear form validation
- recoverable imports
- daily backup capability
- portable deployment
- modular code structure
- automated tests for critical business rules
- no dependency on a single developer's machine
