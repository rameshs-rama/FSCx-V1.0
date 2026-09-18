# Dispatch Workflow

## Objective

Separate customer/sales demand from stock allocation and physical shipment confirmation.

## Flow

Sales Demand
→ Availability Check
→ Allocation
→ Picklist
→ QC/Packaging Readiness Check
→ Release for Dispatch
→ Physical Dispatch
→ Shipment Reference

## Sales actions

- create dispatch request
- update requirement
- set required date
- set priority
- bulk-upload dispatch orders
- put request on hold when permitted

## Inventory/Logistics actions

- allocate stock
- deallocate
- create picklist
- confirm ready
- record carrier/AWB/LR
- confirm physical dispatch

## Rules

- allocation must use usable QC-passed stock
- QC Hold cannot be dispatched
- required quantity and allocated quantity remain separate
- partial allocation is allowed if business policy permits
- international sample/demo data must use Phoenix brand where appropriate
- India sample/demo data should use RAMA where appropriate

## Dashboard

Show:

- total open dispatch orders
- ready to ship
- due today
- due in 7 days
- stock shortage
- QC/packaging hold
- dispatched this week
