# Inventory / Stores Workflow

## Objective

Create one reliable, auditable stock ledger for SFG, FG, components, allocations, and dispatch movements.

## Principle

Never maintain inventory by overwriting a current-stock cell.

Every movement is a transaction.

## Typical movements

- opening migration
- purchase/store receipt
- production receipt
- QC hold
- QC release
- issue to production
- rework movement
- reject/scrap
- stock transfer
- allocation/reservation
- deallocation
- dispatch
- approved adjustment

## Adjustment flow

Create Adjustment
→ Enter Reason
→ Attach Evidence if needed
→ Submit
→ Approval if rule requires
→ Post Adjustment
→ Audit

## Dashboard

Show:

- usable FG
- SFG
- QC Hold
- allocated
- dispatch ready
- low stock
- yesterday net movement
- 7-day movement
- 30-day movement

## Bulk upload

Bulk upload may create valid stock transactions but must never replace historical ledger balances.

## Reconciliation

System should support physical-vs-system reconciliation and approved variance adjustments.
