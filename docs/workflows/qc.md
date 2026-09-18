# Quality Control Workflow

## Primary flow

Produced
→ Awaiting QC
→ Pass / Hold / Rework / Reject

Pass
→ Released to Packaging

Hold
→ Investigation
→ Pass / Rework / Reject / CAPA

## QC record

Capture:

- batch
- SKU
- inspected quantity
- passed quantity
- hold quantity
- reject quantity
- rework quantity
- defect/reason
- inspector
- date/time
- remarks
- attachments/evidence

## Authority

QC release is an explicit permission.

Frontend button visibility is not sufficient; authorization must be enforced server-side.

## Dashboard

Show:

- Awaiting QC
- Holds
- Hold Aging
- Rework
- Rejects
- Recurring Defects
- CAPA Surveillance

## Complaint linkage

When a customer complaint can be mapped to a manufacturing batch, QC should be able to view the corresponding production/QC history.

## Quality surveillance

Track legacy market exposure separately from confirmed post-CAPA recurrence.
