# User Roles

Permissions must be configurable. The roles below are baseline defaults, not hard-coded identities.

## Senior Management

Purpose:
- read-only enterprise/factory visibility
- exception monitoring

Can:
- view dashboards
- view production
- view inventory
- view QC
- view packaging
- view dispatch
- view complaints/CAPA
- view reports

Default:
- no routine operational data entry

## Factory Manager — Jaykumar

Can:
- view all factory operational modules
- review production vs plan
- view stock
- view QC/packaging/dispatch status
- view CAPA
- approve configured stock adjustments
- approve configured exceptions
- intervene on dispatch/quality escalations

Should not be the routine data-entry owner.

## Production Planning / Inputs — Sairam

Can:
- create production schedule
- create/update work orders or production plan
- enter production actuals
- bulk-upload production data
- reconcile planned vs actual
- record production shortfall/reason

Cannot by default:
- release QC
- close CAPA
- perform unrestricted stock adjustments

## QC — Arun

Can:
- record QC inspection
- place batch on hold
- pass/release batch
- reject
- assign rework
- record NCR
- open/update CAPA
- perform effectiveness check
- close CAPA if approval policy permits

Cannot by default:
- alter production history
- directly manipulate inventory balance

## Packaging — Kishore

Can:
- record packaging quantities
- record pack-out checks
- record packaging damage/rejection
- mark packaging complete
- upload packaging data where enabled

Cannot by default:
- release QC-held stock
- close CAPA
- approve inventory adjustments

## Stores & Procurement — Vijay Bhaskar

Can:
- record receipts/issues
- manage SFG/FG/store movements
- monitor stock
- manage procurement tracking
- record supplier/material availability
- create stock adjustments subject to approval
- allocate available QC-passed stock when dispatch permission is granted
- create picklists

Physical dispatch confirmation may be a separate permission.

## Customer Support

Can:
- create complaint ticket
- bulk-upload complaint tickets
- attach evidence
- view ticket/factory investigation status

Cannot:
- modify QC conclusion
- change manufacturing batch history
- close CAPA
- adjust inventory

## Sales

Can:
- create dispatch request/order
- update customer/channel requirement
- set required dispatch date
- set priority
- bulk-upload dispatch demand

Cannot:
- release QC stock
- perform inventory adjustment
- override allocation controls

## Inventory / Logistics

Can when assigned:
- allocate stock
- create picklist
- mark ready for dispatch
- record carrier/AWB/LR
- confirm physical dispatch

Cannot:
- bypass QC release
- change sales order requirement without explicit permission

## Administrator / IT

Can:
- create/deactivate users
- assign roles
- manage permissions
- manage approval levels
- configure security settings
- manage masters/settings where authorized

Administrative access must not automatically imply authority to make business approvals unless explicitly configured.

## Cartridge Process Owner — Lokesh

Operational responsibility may include:
- cartridge batch/process data
- process quality records
- material/media lot linkage
- particulate/carbon leakage controls
- crack/chip/process deviations
- corrective-action implementation evidence

System permissions should be assigned according to actual implementation responsibilities rather than title alone.
