# Customer Complaint Intake Workflow

## Objective

Give Customer Support a simple way to send structured complaint information into the factory quality process.

## MVP source

Customer Support enters complaints directly.

No automatic email-reading dependency.

## Input paths

### Single complaint

Simple form.

### Bulk complaint upload

Excel/CSV controlled template.

## Mandatory minimum

- ticket number
- complaint date
- brand
- market
- product/SKU
- complaint category
- complaint description
- severity

Capture where available:

- order number
- customer/channel reference
- purchase date
- batch number
- photo/evidence
- replacement action

## Flow

Customer Support Entry
→ Factory/QC Review
→ Batch Mapping
→ Classification
→ Investigation
→ CAPA if required
→ Resolution
→ Customer Support visibility

## Permissions

Customer Support can see status and factory response but cannot change QC conclusion or close CAPA.

## Classification reminder

A recent complaint can still belong to old marketplace inventory. Complaint date does not prove a current factory recurrence.
