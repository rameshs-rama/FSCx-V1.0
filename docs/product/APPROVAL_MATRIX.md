# Approval Matrix

Approval policy must be configurable from User Management / Settings.

The system should support permission levels such as:

- View
- Create
- Edit Own Draft
- Edit Department
- Submit
- Approve
- Reject
- Release
- Close
- Administer

## Baseline approval flows

| Action | Initiator | Default Approver / Authority | Notes |
|---|---|---|---|
| Production entry | Production | Production lead/manager if configured | Routine actuals may auto-post if approved policy allows |
| Production correction | Production | Factory Manager / configured approver | Preserve original audit |
| QC hold | QC | QC authority | Immediate control action |
| QC release | QC | QC authority | Cannot be performed by Sales |
| QC reject/rework | QC | QC authority | Must include reason |
| Packaging completion | Packaging | Packaging role | Only QC-released goods |
| Stock adjustment | Stores | Configurable approver | Threshold-based approval recommended |
| Dispatch request | Sales | May not require approval | Represents demand |
| Stock allocation | Inventory/Stores | Inventory permission | Only usable stock |
| Dispatch release | Inventory/Logistics | Configurable | Must meet QC/packaging gates |
| Physical dispatch confirmation | Logistics/Stores | Designated dispatch permission | Capture shipment reference |
| CAPA open | QC/authorized role | QC/Factory Manager policy | Based on severity/rules |
| CAPA closure | QC | Configurable approval | Effectiveness evidence required |
| User creation | Admin | Admin | Audit required |
| Role/permission change | Admin | Optional higher admin approval | Audit before/after |

## Recommended configurable thresholds

Examples:

- stock adjustment above X units requires Factory Manager
- stock adjustment above higher threshold requires Senior Management
- critical QC release exception requires Factory Manager acknowledgement
- CAPA severity S1 closure requires Factory Manager
- user granted Admin role requires second admin approval if available

Threshold values must not be hard-coded in business logic.
