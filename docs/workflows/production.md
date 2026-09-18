# Production Workflow

## Objective

Capture production plan and actual factory output without asking operators to maintain derived system quantities manually.

## Primary flow

Production Plan / Work Order
→ Start / Record Production
→ Enter Actual Output
→ Record Reject/Rework if any
→ Submit
→ Awaiting QC

## Input options

### Single entry

Use for routine one-off production records.

### Bulk upload

Use controlled Excel/CSV template for multiple lines/records.

Both paths apply the same validation.

## Key fields

- production date
- shift
- work order / plan reference
- SKU/product
- batch
- planned quantity
- produced quantity
- inner chamber quantity where applicable
- outer chamber quantity where applicable
- accessory/component production where applicable
- reject quantity
- rework quantity
- line/machine
- remarks
- submitted by

## Rules

- no manual filter-set quantity
- negative quantities invalid
- unknown SKU invalid
- duplicate controlled reference triggers review
- actuals are auditable
- corrections must preserve original history
- completed production moves into QC workflow as applicable

## Production dashboard

Keep simple:

- Today's Plan
- Produced Today
- Shortfall
- Awaiting QC
- Entries Needing Correction

Primary actions:

- Add Production Entry
- Upload Excel/CSV
- View Today's Entries
