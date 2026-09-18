# Business Rules

## 1. Inventory

### 1.1 Transaction-derived balance

Current stock must be calculated from inventory movements, not maintained as an editable single number.

Concept:

Opening + Receipts + Production + Transfers In - Issues - Dispatch - Scrap - Transfers Out ± Approved Adjustments = Current Balance

### 1.2 Stock categories

At minimum distinguish:

- Semi-Finished Goods (SFG)
- Finished Goods (FG)
- QC Hold
- Rework
- Reject / Scrap
- Reserved / Allocated
- Dispatch Ready

### 1.3 Adjustments

Stock adjustments require:

- SKU
- quantity
- reason
- user
- timestamp
- supporting note/evidence where configured
- approval when threshold/rule requires it

Historical transactions are not deleted to hide errors.

## 2. Production

Factory operators do not manually maintain a "filter set quantity".

For filter systems, track relevant physical components independently, including inner chamber and outer chamber.

Daily production entry should capture:

- date
- shift if used
- work order / plan reference
- SKU/product
- batch
- planned quantity
- actual quantity
- component quantities where applicable
- reject/rework quantity
- line/machine where applicable
- remarks
- submitted by

Production updates must be available through single entry and bulk upload.

## 3. Quality

Typical lifecycle:

Produced → Awaiting QC → Pass / Hold / Rework / Reject → Released to Packaging

QC must control release authority.

A user without QC release permission cannot make QC-held stock dispatch-ready.

## 4. Packaging

Packaging is a separate operational gate.

Packaging should confirm:

- correct SKU/product
- correct market/brand pack
- required accessories
- visible condition
- packaging completeness
- quantity packed
- damaged/rejected quantity
- pack date/user
- batch linkage

## 5. Dispatch

Sales/dispatch demand and inventory allocation are separate concepts.

Sales may create/update demand.

Inventory/Stores may allocate stock if permission allows.

Physical dispatch confirmation is a separate action and may require a different permission.

Suggested dispatch quantity must never exceed usable available stock.

## 6. Customer complaints

Complaint intake fields must include:

- ticket number
- complaint date
- brand
- market
- customer/channel reference where appropriate
- order/reference number where available
- SKU/product
- complaint category
- complaint description
- severity
- batch number where visible/available
- attachments/evidence
- customer support owner

Ticket number is mandatory.

## 7. Complaint-to-CAPA logic

Complaint date alone does not determine whether a CAPA has failed.

The system should compare:

- complaint
- manufacturing batch/date
- CAPA effective date/effective batch
- confirmed failure mode

Possible classifications:

- Legacy Market Exposure
- Post-CAPA Watch
- Confirmed CAPA Recurrence
- New Failure Mode
- Installation / Usage
- Not Factory Related

## 8. Black particle issue

Known historical black-particle complaints may still arise from pre-fix marketplace stock.

Corrective action has been implemented in manufacturing, while corrected product had not yet fully reached the market at the time of the project decision.

Rule:

- pre-CAPA batch → Legacy Market Exposure
- post-CAPA batch + unconfirmed cause → Post-CAPA Watch
- post-CAPA batch + same root cause confirmed → Confirmed CAPA Recurrence

Do not treat every new complaint date as evidence that the corrective action failed.

## 9. POSTreat V2 rust issue

The historical POSTreat rust problem was investigated and corrective changes were internally validated.

At the time of the project decision, the newly validated configuration had not yet been broadly market deployed.

The system should support:

Internal Validation → Controlled Release → First Shipment → Market Observation → Effectiveness Closure

Field effectiveness is not meaningfully measurable before corrected product reaches customers.

## 10. Bulk upload

Required sequence:

Upload → Validate → Show Errors → Preview → Confirm → Commit → Audit

Rules:

- invalid rows are clearly identified
- error file/report can be downloaded
- duplicate detection applies where relevant
- users can correct and re-upload
- no silent row loss
- no direct overwrite of inventory history
- approval rules still apply

## 11. Audit

Audit sensitive actions including:

- login/security changes
- role/permission changes
- stock adjustments
- QC holds/releases
- dispatch release/confirmation
- CAPA status/closure
- master-data changes
- bulk imports

Audit records should capture before/after value where practical.
