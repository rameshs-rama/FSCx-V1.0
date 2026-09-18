# Requirements traceability matrix

Status: proposed mappings and acceptance tests; nothing in this matrix is implemented or tested yet. Reviewed 2026-09-18 against `docs/product/FRD.md`, business rules, roles, approval matrix, decisions and all workflows.

Every numbered FRD requirement has one row below. `SUP-*` IDs are review-local identifiers for unnumbered requirements; they do not renumber the FRD. Entity names refer to the [domain proposal](ARCHITECTURE_PROPOSAL.md). Test IDs are planned test cases, not existing test files.

Role abbreviations: Admin = Administrator; FM = Factory Manager; Production = Production Planning/Inputs; CS = Customer Support; Logistics = Inventory/Logistics; Management = Senior Management. A conditional role capability is never a default grant. Permissions below describe business actions; exact permission keys are defined during implementation. Every action additionally requires an active authenticated user and applicable brand/market scope.

## User Management — Phase 1

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-UM-001 — Create/edit/deactivate/reactivate users | Identity / Users, User details | User, Session, AuditEvent | Admin / user lifecycle | Admin authority | Unique normalized username; preserve historical actor; active flag enforced | T-UM-001: lifecycle persists; deactivation invalidates an existing session |
| FR-UM-002 — Assign one or more roles when enabled | Access / User roles | UserRoleAssignment, Role | Admin / role assignment | Role-change policy if configured | Respect multi-role setting; reject unknown/inactive roles | T-UM-002: single-role mode rejects a second role; multi-role mode evaluates explicit grants |
| FR-UM-003 — Enforce permissions server-side | Access / all protected pages/endpoints | User, RolePermission, AssignmentScope | Every caller / explicit action grant | Per action | Direct API, server action, object lookup and exports checked | T-UM-003: hidden-button bypass via direct request returns denial with no mutation |
| FR-UM-004 — Module/action permissions | Access / Roles and permissions | Role, Permission, RolePermission | Admin / permission administration | Optional higher admin policy | Unknown permission denied; view does not imply edit or export | T-UM-004: view-only user cannot submit mutation or export |
| FR-UM-005 — Configurable approval levels | Approvals / Approval rules | ApprovalPolicyVersion, ApprovalStage | Admin / approval configuration | Configuration policy | Typed conditions; explicit stages; reject invalid/ambiguous configuration | T-UM-005: rule revisions preserved; invalid configuration cannot activate |
| FR-UM-006 — Audit role/permission changes | Audit / Audit history | AuditEvent, RolePermission, UserRoleAssignment | Admin / access administration; explicit audit-read | Same as underlying change | Record actor and safe before/after values atomically | T-UM-006: grant change and audit succeed/rollback together |
| FR-UM-007 — Brand/market restriction | Access / User scope | AssignmentScope, BrandMarket | Admin configures; scoped users consume | Access-change policy | Permission and scope evaluated through same assignment | T-UM-007: RAMA/India and Phoenix/UK grants cannot combine into unintended cross-scope write |
| FR-UM-008 — Block inactive users | Identity / Login and every request | User, Session | Active users only | None | Recheck activity/session validity | T-UM-008: inactive login and previously issued session both fail |

## Production — Phase 3; integrated acceptance after Phases 4–5

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-PR-001 — Single record | Production / Add Production Entry | ProductionEntry, ProductionEntryLine | Production / create actual | Production policy | Required references, dates, nonnegative quantities | T-PR-001: valid entry persists; Stores without grant denied |
| FR-PR-002 — Excel/CSV upload | Production / Upload Production | ImportJob, ImportRow, ProductionEntry | Production / import | Same as single entry | Supported template/version, columns, bounded rows | T-PR-002: XLSX and CSV sample templates produce equivalent entries |
| FR-PR-003 — Validate fields/SKU/quantity/reference/date | Production / Entry and import preview | SKU, WorkOrder, ManufacturingBatch, ImportError | Production / create or import | Same as posting | Unknown SKU, negative quantity, invalid date, controlled duplicate rejected | T-PR-003: each failure shown on form/row without mutation |
| FR-PR-004 — Preview before commit | Production / Import preview | ImportJob, ImportRow, ImportCommit | Production / confirm import | Production policy | Preview version and confirmation required; revalidate current data | T-PR-004: upload alone creates no production or stock |
| FR-PR-005 — Row-level errors | Production / Import errors | ImportError, ImportRow | Production / own scoped import read | None | Preserve original row numbers; downloadable error report | T-PR-005: mixed file reports every invalid row and original value |
| FR-PR-006 — Plan vs actual | Production / Plan, Actuals | ProductionPlan, WorkOrder, ProductionEntry | Production / plan and actual permissions | As configured | Never overwrite plan with actual | T-PR-006: plan 100/actual 80 retained with shortfall 20 |
| FR-PR-007 — Batch/work order links | Production / Entry details | WorkOrder, ManufacturingBatch, ProductionEntry | Production / create | Production policy | Valid references; batch manufacturing date preserved | T-PR-007: trace entry to work order and batch; invalid links rejected |
| FR-PR-008 — Separate inner/outer quantities | Production / Component output | ProductionEntryLine, Component | Production / create | Production policy | Separate physical lines and units | T-PR-008: inner 100/outer 80 remain distinct quantities |
| FR-PR-009 — No manual filter-set quantity | Production / Entry and templates | SKUComponent, ProductionEntryLine | No direct derived-set write | Not applicable | Reject manual derived field through UI, import and API | T-PR-009: payload cannot override a derived system quantity |
| FR-PR-010 — Production receipt/state | Production / Submit and history | ProductionEntry, InventoryTransaction, InventoryLeg | Production / submit | Configured production policy | Unique source posting; atomic receipt into approved workflow state | T-PR-010: retry posts once; failed receipt rolls back; QC gate retained |

## Inventory / Stores — Phase 4

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-IN-001 — Ledger basis | Inventory / Stock movement | InventoryTransaction, InventoryLeg | Stores / movement; readers scoped | Per movement | No authoritative editable balance | T-IN-001: balance equals posted ledger sum |
| FR-IN-002 — Movement types | Inventory / Add movement | InventoryTransaction, InventoryLeg, StockBucket | Stores / explicit movement type | Type-dependent | Signed legs, location/state rules, conservation for transfer | T-IN-002: receipts/issues/transfers/rework/scrap/adjustments yield expected balances |
| FR-IN-003 — Derived balance | Inventory / Stock overview | InventoryLeg, AllocationEvent | Scoped inventory-view | None | Posted-only movements; reservations not double-subtracted | T-IN-003: receipt 100, issue 20, reserve 10 gives on-hand 80 and available 70 |
| FR-IN-004 — Stock categories | Inventory / State filters | StockBucket, QualityHold, Allocation | Scoped inventory-view | State action policy | Separate physical, quality, packaging and reservation dimensions | T-IN-004: held FG appears in FG/hold breakdown without inflating total |
| FR-IN-005 — Single transaction | Inventory / Receipts and issues | InventoryTransaction, InventoryLeg | Stores / create authorized movement | Per type | Valid SKU/location/batch/quantity/reason | T-IN-005: unauthorized adjustment cannot be submitted as receipt |
| FR-IN-006 — Bulk transactions | Inventory / Upload movements | ImportJob, ImportRow, InventoryTransaction | Stores / movement import | Same as manual | No historical overwrite; shared validation | T-IN-006: imported movement audited; invalid rows cannot corrupt ledger |
| FR-IN-007 — Adjustment attribution/reason | Inventory / Adjustment | Adjustment, AuditEvent | Stores / create adjustment | Configured adjustment policy | Nonblank reason, actor, timestamp, evidence if required | T-IN-007: missing reason rejected; original movement remains intact |
| FR-IN-008 — Threshold approval | Approvals / Adjustment review | ApprovalPolicyVersion, ApprovalRequest, ApprovalDecision | Configured approver / approve adjustment | Explicit type/threshold | Exact threshold boundary and scope; no self-approval per accepted policy | T-IN-008: below/equal/above thresholds; missing rule cannot silently post |
| FR-IN-009 — Movement windows | Inventory / Overview and movement report | InventoryLeg | Scoped inventory-view | None | Business date cutoffs; reversals and net movement consistent | T-IN-009: today/yesterday/7/30-day totals at midnight boundaries |
| FR-IN-010 — Low stock/shortage | Inventory / Attention queue | SystemSetting, StockBucket, Allocation | Authorized threshold editor; inventory-view | Setting policy | Configurable threshold and usable-stock basis | T-IN-010: held/reserved stock does not conceal shortage |

## Quality Control — Phase 5

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-QC-001 — Awaiting QC | QC / Queue | ManufacturingBatch, Inspection, StockBucket | QC / queue view; Production submits source | Production posting policy | Valid produced quantity; no duplicate queue receipt | T-QC-001: posted production enters queue once |
| FR-QC-002 — Pass/Hold/Rework/Reject | QC / Inspection | InspectionDisposition, InventoryLeg | QC / each disposition | QC authority | Quantities conserve inspected stock under agreed rule | T-QC-002: split dispositions persist; over-disposition and negative quantities fail |
| FR-QC-003 — Complete inspection record | QC / Inspection details | Inspection, Attachment, ReasonCode | QC / record inspection | QC authority | Inspector/time/batch/quantity; reasons/evidence as required | T-QC-003: required record fields enforced with clear errors |
| FR-QC-004 — Hold blocks dispatch readiness | QC, Packaging, Dispatch / all transitions | QualityHold, StockBucket, DispatchRelease | All roles constrained | No ordinary QC bypass | Recheck current hold on readiness and shipment | T-QC-004: stock put on hold after allocation cannot ship |
| FR-QC-005 — Restricted release | QC / Release Batch | InspectionDisposition, AuditEvent | Explicit QC release permission | QC authority | Server-side release and scope check | T-QC-005: Sales and default Admin cannot release via direct API |
| FR-QC-006 — Queue aging | QC / Holds and aging | Inspection, QualityHold | QC / view; approved read roles | None | Agreed aging origin and business time | T-QC-006: hold aging remains correct after unrelated edits |
| FR-QC-007 — NCR/CAPA from QC | QC / Finding details | NCR, CAPASourceLink | QC / NCR and CAPA create | CAPA policy | Preserve source inspection/batch and evidence | T-QC-007: source traced; full CAPA linkage accepted after Phase 9 |

## Packaging — Phase 6

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-PK-001 — Packed quantity/date | Packaging / Packaging Entry | PackagingRun, PackagingLine | Packaging / create | Packaging authority | Date, batch, eligible quantity; prevent duplicate completion | T-PK-001: packed record links source and user |
| FR-PK-002 — Pack completeness/market | Packaging / Pack-Out Check | PackSpecification, PackagingCheck | Packaging / complete checks | Packaging authority | Versioned SKU/brand/market checklist | T-PK-002: wrong market pack or missing required accessory blocks completion |
| FR-PK-003 — Damage/rejection | Packaging / Exceptions | PackagingException, ReasonCode, InventoryLeg | Packaging / record exception | Relevant disposition policy | Quantity and reason; no silent stock disappearance | T-PK-003: damaged quantity enters traceable exception path |
| FR-PK-004 — QC gate | Packaging / Complete Packaging | InspectionDisposition, PackagingRun | Packaging / complete | Approved exception only if separately specified | Normal route requires QC-released stock | T-PK-004: held/rejected stock cannot complete normal packaging |
| FR-PK-005 — Readiness update | Packaging / Completion details | PackagingRun, StockBucket | Packaging / complete | Packaging authority | Atomic transition; QC gate still valid | T-PK-005: ready quantity matches eligible packed quantity; retry does not add stock |

## Dispatch — Phase 7

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-DI-001 — Manual demand | Dispatch / New Dispatch Order | DispatchOrder, DispatchOrderLine | Sales / create demand | Demand policy if configured | Required fields; demand causes no inventory movement | T-DI-001: demand creation leaves on-hand/reservations unchanged |
| FR-DI-002 — Bulk demand | Dispatch / Upload Orders | ImportJob, DispatchOrder | Sales / demand import | Same as manual | Template, references, duplicates, preview | T-DI-002: valid Excel/CSV rows match manual demand behavior |
| FR-DI-003 — Demand fields | Dispatch / Order details | BrandMarket, SKU, DispatchOrderLine | Sales / create/update demand | Demand policy | Brand, market, customer/channel, SKU, quantity, required date, priority | T-DI-003: missing fields rejected; Phoenix international sample mapping retained |
| FR-DI-004 — Authorized allocation | Dispatch / Allocate Stock | Allocation, AllocationEvent, StockBucket | Explicit Stores/Inventory allocation grant | Allocation authority | QC-passed usable stock and scoped action | T-DI-004: unassigned Stores user denied; held stock ineligible |
| FR-DI-005 — Available quantity ceiling | Dispatch / Allocation | InventoryLeg, Allocation | Explicit allocation permission | No over-allocation override | Recheck available under lock in one transaction | T-DI-005: simultaneous allocations cannot exceed available balance |
| FR-DI-006 — Picklist status | Dispatch / Picklist | Picklist, Allocation | Authorized Stores/Logistics / picklist | Picklist authority | Pick quantities cannot exceed active allocation | T-DI-006: picklist traced to allocation; status transition validated |
| FR-DI-007 — Physical shipment | Dispatch / Confirm Dispatch | Shipment, ShipmentLine, InventoryTransaction | Designated dispatch-confirm permission | Release policy | Actual dispatch date and shipment reference; readiness checked | T-DI-007: confirmation creates one issue; retry returns original result |
| FR-DI-008 — Separate permissions | Dispatch / Demand, Allocation, Release, Confirm | Permission, RolePermission, AuditEvent | Four distinct action grants | Action-specific | No implicit grant from another step | T-DI-008: Sales demand grant cannot allocate, release or confirm |
| FR-DI-009 — Dispatch risk | Dispatch / Due and at-risk queue | DispatchOrderLine, Allocation, QualityHold | Scoped dispatch-view | None | Shortage/date/readiness rules approved before release | T-DI-009: due order with insufficient ready quantity flagged correctly |

## Customer Complaints — Phase 8

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-CC-001 — Single complaint | Complaints / New Complaint | Complaint | CS / create complaint | No default intake approval specified | Mandatory minimum; actor attribution | T-CC-001: CS creates and sees ticket; unauthorized role denied |
| FR-CC-002 — Bulk complaints | Complaints / Upload Complaints | ImportJob, ImportRow, Complaint | CS / complaint import | Same as manual | Template, duplicate policy, dates, preview | T-CC-002: manual/XLSX/CSV have identical validation and permissions |
| FR-CC-003 — Mandatory ticket | Complaints / Entry and preview | Complaint | CS / create or import | Same as intake | Nonblank ticket; uniqueness scope awaits decision | T-CC-003: blank ticket rejected in form, API and import |
| FR-CC-004 — Complaint details | Complaints / Ticket details | Complaint, ComplaintCategory, Attachment | CS / intake and evidence | Intake policy | Brand/market/SKU/description/category/severity/date; evidence optionality per workflow | T-CC-004: minimum fields accepted without unavailable batch/photo; supplied evidence linked |
| FR-CC-005 — Optional order/batch | Complaints / References | ComplaintBatchLink, ManufacturingBatch | CS enters supplied reference; QC verifies mapping | QC mapping authority | Unknown batch text retained; no invented batch date | T-CC-005: ticket without batch accepted and queued for mapping |
| FR-CC-006 — View progress, protect conclusions | Complaints / Investigation status | Complaint, CAPA, Inspection | CS / progress read; QC / conclusions | QC/CAPA policy | Field-level writes and related-object access enforced | T-CC-006: CS reads response but cannot alter QC conclusion or close CAPA |
| FR-CC-007 — No Gmail intake | Complaints / Intake options | Complaint, ImportJob | CS / manual or import | None | No automatic email ingestion dependency | T-CC-007: both intake paths work with no email account/integration |

## CAPA — Phase 9

| Requirement | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| FR-CA-001 — Open from multiple sources | CAPA / Open CAPA | CAPA, CAPASourceLink, NCR, Complaint | QC / configured authorized opener | QC/FM severity policy | Valid source and scoped permission; clarify RFD terminology | T-CA-001: each approved source linked and readable by authorized user |
| FR-CA-002 — Complete lifecycle record | CAPA / Investigation and actions | CAPA, CAPAAction, CAPAVerification, CAPAClosure | QC; explicitly assigned action owner | Stage-specific policy | Stage-appropriate required fields, owner, dates, effective batch/date | T-CA-002: missing required stage evidence prevents transition |
| FR-CA-003 — Evidence before closure | CAPA / Effectiveness and Close | CAPAVerification, CAPAExposure, CAPAClosure | Explicit closer and configured approver | CAPA closure policy | Effectiveness evidence; field exposure where required | T-CA-003: action complete alone cannot close; no pre-exposure field-effectiveness claim |
| FR-CA-004 — Batch-based classification | Complaints/CAPA / Classification | ManufacturingBatch, CAPA, ComplaintClassificationHistory | QC / classify | QC authority | Compare manufacturing and effective batch/date; unresolved data goes to review | T-CA-004: new complaint from pre-fix batch remains legacy; unknown batch not auto-classified |
| FR-CA-005 — Six supported outcomes | Complaints/CAPA / Classification | ComplaintClassificationHistory | QC / classify | QC authority | Evidence and cause support outcome; preserve history | T-CA-005: post-fix unconfirmed = Watch; same confirmed cause = Recurrence; other outcomes validated |
| FR-CA-006 — Separate recurrence reporting | Reports / Quality surveillance | ComplaintClassificationHistory, CAPAExposure | Scoped quality/report-view | None | Separate legacy, watch and confirmed recurrence; exposure context visible | T-CA-006: legacy complaints do not inflate confirmed recurrence count |

## Supplemental requirements

| Requirement / source | Module / screen | Entities | Role / permission | Approval | Validation | Planned test |
|---|---|---|---|---|---|---|
| SUP-001 — Masters; FRD §4 | Masters / Controlled lists (Phase 2) | Brand, Market, SKU, UnitOfMeasure, Location, ReasonCode | Explicit master administration | Master-change policy | Unique codes, valid links, historical references retained | T-SUP-001: duplicate rejected; deactivate retains historical labels/references |
| SUP-002 — Master audit; FRD §4/14 | Audit / Master history | AuditEvent, master entities | Explicit audit-read | Same as master change | Actor/before/after in same transaction | T-SUP-002: master edit always attributable |
| SUP-003 — Management Control Tower; FRD §12 | Reports / Control Tower (Phase 10) | Operational read models | Management/FM / scoped dashboard view | None | Defined KPIs from canonical records, no invented counts | T-SUP-003: KPI totals reconcile to filtered operational records |
| SUP-004 — Role action queues; FRD §12, UX baseline | Home / Role workspace (foundation + module phases) | Permission, module read models | Documented role view grants | None | Show permitted implemented actions only | T-SUP-004: CS sees complaints queue; denied modules absent and unreachable |
| SUP-005 — Full import sequence; FRD §13 | Imports / Template, Preview, Result | ImportJob, ImportRow, ImportError, ImportCommit | Explicit module import grant | Same as manual operation | Upload, validate, errors, preview, confirm, commit, result, audit | T-SUP-005: all nine steps covered with accurate received/valid/error/committed counts |
| SUP-006 — No row loss/partial corruption; AGENTS 13–14 | Imports / Result and retries | ImportCommit, module records | Explicit import confirmation | Module policy | Approved atomicity policy, idempotency, stale preview check | T-SUP-006: mid-commit failure rolls back; retry never duplicates records |
| SUP-007 — Security events/password hashing; FRD §14 | Identity / Login/reset (Phase 1) | User, Session, AuditEvent | Account owner; Admin reset | Admin authority | Hash password; redact secret; expire/revoke sessions; rate limit | T-SUP-007: no plaintext secret in DB/logs; invalid/expired session denied |
| SUP-008 — Privileged MFA; FRD §14 | Identity / Security settings | Future MFA credential/recovery entities | Admin/FM/QC approvers | Enrollment/recovery policy TBD | MFA practicality/rollout decision required; not claimed delivered in Phase 1 | T-SUP-008: after implementation, missing second factor blocks required privileged session |
| SUP-009 — Sensitive-action audit; FRD §14 | Audit / History (all phases) | AuditEvent | Explicit audit-read; service writes | Underlying action policy | Append-only runtime privileges, redacted diffs and reasons | T-SUP-009: runtime update/delete denied; mutation/audit rollback together |
| SUP-010 — Filtered exports; FRD §15 | Reports / Export (Phase 10 and module increments) | Module records, export audit | Explicit module export grant | None unless configured | Scope and current filters; safe spreadsheet text; no excess fields | T-SUP-010: export matches authorized filtered view; formula-like text stays data |
| SUP-011 — Usable responsive UI; FRD §16, UX baseline | All screens | Not applicable | Role-specific | Confirm high-impact actions | Labels, keyboard navigation, short forms, clear empty/error states | T-SUP-011: desktop/narrow-screen entry, keyboard use and actionable validation verified |
| SUP-012 — Expected load; FRD §16/D-011 | Whole application | All persisted entities | Representative authorized roles | As applicable | Fewer than 10 concurrent users; latency target agreed before UAT | T-SUP-012: 10-user workload measured; no lost writes/oversubscribed stock |
| SUP-013 — Backup/restore; FRD §16 | Operations / Runbook | Database, attachment files | Authorized host operator | Operational policy TBD | Recover matching DB/files; isolated restore; daily capability | T-SUP-013: restore sample users/audit and later attachment links successfully |
| SUP-014 — Portability/modularity/tests; FRD §16/D-009/010 | Developer/operations setup | Migrations, config | Developer/operator | Phase gates | Reproducible versions; no machine-specific runtime paths | T-SUP-014: fresh environment migrates, builds and passes critical tests |
| SUP-015 — Immutable correction; AGENTS 4–5/15, D-004 | Production/Inventory / Correction | ProductionCorrection, InventoryTransaction, AuditEvent | Explicit correction/adjustment grant | Correction policy | Original record retained; reason and linked reversal | T-SUP-015: original cannot be deleted/overwritten; corrected balance traceable |
| SUP-016 — Operational vs approval status; Requirements Audit §4 | All approval-bearing screens | ApprovalRequest, module entity | Module actor and approver | Explicit staged policy | Distinct statuses; approval bound to record revision | T-SUP-016: approved but unposted adjustment does not affect balance; edit invalidates approval |
| SUP-017 — No implicit admin approval; User Roles | Access / Approval settings | RolePermission, ApprovalStage | Admin configures; designated business approver acts | Explicit grant only | Admin role alone insufficient | T-SUP-017: Admin denied stock/QC/CAPA approval without corresponding grant |
| SUP-018 — Physical reconciliation; Inventory workflow | Inventory / Reconciliation | Reconciliation, ReconciliationLine, Adjustment | Stores / reconcile | Variance adjustment policy | Count captured separately; variance posts through adjustment | T-SUP-018: entering physical count alone does not overwrite stock |
| SUP-019 — Market effectiveness lifecycle; D-006/Business Rules §9 | CAPA / Exposure and effectiveness | CAPAExposure, CAPAVerification, Shipment | QC / verify; configured closer | Closure policy | Internal validation, controlled release, first shipment, observation distinguished | T-SUP-019: no field-effectiveness success before corrected product exposure |
| SUP-020 — Single/bulk coverage; AGENTS 1, D-003 | Every operational entry workspace | ImportJob and target module | Same action grants, explicit import grant | Same as manual | Coverage decision R-01 before module acceptance | T-SUP-020: approved coverage checklist includes QC/Packaging/CAPA or explicit documented exception |
| SUP-021 — Attachments abstraction; AGENTS architecture | Evidence upload/download | Attachment, typed parent link | Authorized parent access | Parent action policy | File limits/type, safe storage keys, protected downloads, orphan cleanup | T-SUP-021: another scoped user cannot download guessed attachment ID |
| SUP-022 — Clear action ownership; Context/Audit §2 | Queues / Next action | User, module owner fields | Assigned documented role | Module policy | One primary owner per fact; no duplicate entry demanded | T-SUP-022: production receipt reused by Stores, not re-entered |
| SUP-023 — Controlled templates; templates README | Imports / Download Template | Template definition, ImportJob | Explicit module import access | None | Version, columns, required markers, examples, formats/reference guidance | T-SUP-023: downloaded CSV/XLSX template round-trips through validation |
| SUP-024 — Usability and high-impact confirmation; UX baseline | Adjustment/release/dispatch/CAPA/access screens | Module records, AuditEvent | Underlying action permission | Business approval remains separate | Human-readable consequences; confirmation is not authorization | T-SUP-024: cancel changes nothing; confirming revalidates server state |

## Acceptance tracking

For each implemented row, add the test file/path, latest result, and accepted decision IDs during its phase. Do not mark an entire module complete while any mandatory row, role-boundary test, approval-boundary test, validation, audit, empty/error state, single-entry flow, or required bulk flow is missing or failing. Documentation checks only establish coverage; they do not establish application behavior.
