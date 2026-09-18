# Navigation and UX Baseline

## Goal

Keep the application visually simple even though the underlying controls are strict.

## Global layout

Recommended:

- left navigation
- clear page title
- one primary action
- small number of KPI cards
- action queue/table
- filters only when useful
- help text beside unfamiliar actions

## Role-based home screens

### Senior Management
Home:
- Control Tower
- Reports

May view other modules read-only.

### Factory Manager
Home:
- Factory Overview
- Exceptions / Approvals
- Production
- QC
- Inventory
- Dispatch
- CAPA

### Production
Home:
- Today's Production
- Add Production Entry
- Upload Production
- Production History

### QC
Home:
- QC Queue
- Holds
- Rework/Reject
- CAPA
- Quality Reports

### Packaging
Home:
- Ready to Pack
- Packaging Entry
- Packaging Exceptions

### Stores / Inventory
Home:
- Stock Overview
- Receipts / Issues
- Adjustments
- Allocations
- Dispatch Queue
- Procurement

### Sales
Home:
- Dispatch Orders
- New Dispatch Order
- Upload Dispatch Orders
- Dispatch Status

### Customer Support
Home:
- Complaints
- New Complaint
- Upload Complaints
- Complaint Status

### Administrator
Home:
- Users
- Roles
- Permissions
- Approval Rules
- Master Data
- Audit / Security Settings

## Entry-screen principle

Where both methods exist, show:

[ Add Single Entry ]   [ Upload Excel / CSV ]

Do not hide bulk upload in an obscure settings page.

## Status language

Use short human-readable terms.

Prefer:
- Draft
- Awaiting QC
- Hold
- Rework
- Passed
- Packed
- Ready
- Allocated
- Dispatched
- Closed

Avoid internal codes in the visible UI unless needed.

## Error handling

Bad:
"Validation error 422."

Good:
"3 rows cannot be imported. SKU is missing in rows 7, 11 and 18."

Every error should tell the user what to fix.

## Confirmation

Require confirmation for high-impact actions such as:

- post stock adjustment
- release QC hold
- reject stock
- confirm dispatch
- close CAPA
- change user permissions

Avoid confirmation dialogs for harmless navigation.
