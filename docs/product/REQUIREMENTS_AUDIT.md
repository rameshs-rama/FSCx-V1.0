# Requirements Audit — Corrections and Simplifications

This document records the important corrections made while auditing the early dashboard concepts.

## 1. Do not build a dashboard-only product

The system is an operational application with dashboards.

Dashboards summarize data. They are not the place where every user performs every task.

Required operational workspaces include:

- Production
- QC
- Packaging
- Stores/Inventory
- Dispatch
- Customer Complaints
- CAPA
- User Management

## 2. Avoid duplicate data ownership

Each business fact needs one primary owner.

Recommended ownership:

- Production plan / actuals → Production
- QC result / hold / release → QC
- Packaging completion → Packaging
- Inventory movement → Stores/Inventory
- Dispatch demand → Sales
- Allocation / shipment execution → Inventory/Logistics
- Complaint intake → Customer Support
- CAPA investigation/closure → QC / configured approver
- User/permission configuration → Administrator

Other modules should consume that information rather than asking another user to re-enter it.

## 3. Separate demand from stock movement

A sales dispatch request is not an inventory movement.

A stock allocation is not a physical dispatch.

A physical dispatch is not the same as an order request.

Keep these states separate.

## 4. Separate operational state from approval state

Example:

Inventory adjustment status:
- Draft
- Submitted
- Approved
- Rejected
- Posted

Do not overload one generic "status" field to mean both operational and approval state.

## 5. Do not let role names become hard-coded people

Jaykumar, Sairam, Arun, Kishore, and Vijay Bhaskar are current responsibility mappings.

Application logic must use roles/permissions, not employee-name checks.

## 6. User Management must control access and approval

The initial dashboard concepts omitted enough user-management detail.

V1 requires:

- users
- active/inactive
- department
- role
- permissions
- approval level
- optional brand/market scope
- audit trail
- password/security controls

## 7. Both manual and bulk entry are required

Do not replace manual entry with Excel upload.

Do not force all users to manually enter high-volume data.

Where relevant, show two obvious choices:

- Add Single Entry
- Upload Excel/CSV

## 8. Bulk upload is not a bypass

Bulk data uses the same:

- validation
- authorization
- approval
- duplication checks
- audit rules

as manual data.

## 9. Complaint intake should be explicit, not email-driven in MVP

Earlier thinking considered automatically reading complaint emails.

Final V1 decision:

Customer Support owns structured complaint entry/upload.

This reduces:
- email parsing errors
- privacy exposure
- duplicate intake
- dependency on mailbox structures

Email integration can be reconsidered only after the structured process is stable.

## 10. Correct brand/market logic

Do not use RAMA for every market in sample screens.

Baseline:
- India → RAMA
- UK → Phoenix
- USA → Phoenix
- France → Phoenix

The data model should still keep brand and market configurable rather than hard-coding only these combinations.

## 11. Correct role ownership in production entry

An earlier mockup showed a Stores user on a Production Entry screen.

Production data entry should default to Production/Sairam's team.

Stores should consume posted production receipts rather than own production actuals.

## 12. Filter set quantity must be derived

Do not ask operators to update a filter-set quantity.

Track inner and outer chamber counts independently.

A complete-set quantity, when needed, is a derived calculation based on compatible available components.

## 13. Quality history must distinguish legacy exposure

Recent complaint date does not necessarily mean current production failed.

Quality analysis needs:
- manufacturing batch/date
- corrective-action effective date/batch
- confirmed failure mode

This avoids reopening a CAPA simply because old marketplace stock generated a new complaint.

## 14. Internal validation is not market effectiveness

For corrected products not yet deployed to customers:

Correct wording:
- corrective action implemented
- internal validation passed
- controlled release / market deployment pending
- field effectiveness not yet measurable

Avoid displaying "0 recurrence" as evidence of market effectiveness before exposure exists.

## 15. Inventory should be ledger-based

An editable current-stock figure is too fragile.

Use transactions and calculate balances.

This is essential for:
- yesterday movement
- 7-day movement
- 30-day movement
- audit
- reconciliation
- root-cause investigation

## 16. Keep dashboards exception-driven

A factory operator should not scan dozens of charts.

Show:
- what needs my action
- what is blocked
- what is overdue
- what is at risk

Management can have broader KPIs.

## 17. Minimize mandatory fields

Only fields needed for:
- traceability
- control
- downstream workflow
- legal/compliance/business reporting

should be mandatory.

Do not make optional commentary mandatory just because the database has a column.

## 18. Prefer controlled lists

Use dropdowns/master data for:
- SKU
- brand
- market
- complaint category
- QC reason
- rework reason
- warehouse
- status

Use free text only for explanatory notes.

## 19. State transitions must be controlled

Users should not be able to select arbitrary future statuses from a generic dropdown.

Example:
- QC Held cannot jump directly to Dispatched.
- Rejected stock cannot become Available without a valid rework/release process.
- CAPA cannot close without effectiveness evidence.

## 20. Reporting must come from operational data

Do not maintain separate manual "dashboard numbers."

KPIs should be computed from the same records used by the operational modules.

## 21. Simplicity test

Before accepting any screen, ask:

Can a new employee understand:
1. what this screen is for,
2. what they need to do now,
3. what will happen next,
4. what went wrong if validation fails,

without reading a long manual?

If not, simplify the screen.
