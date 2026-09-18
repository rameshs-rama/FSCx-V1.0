# Bulk Upload Templates

Create controlled templates during implementation for:

- Production
- Inventory Transactions
- Dispatch Orders
- Customer Complaints

Each template should include:

- template version
- clear column names
- required/optional indication
- sample row
- dropdown/reference guidance where practical
- date format guidance
- quantity format guidance

## Import behavior

Upload
→ Validate
→ Preview
→ Correct Errors
→ Confirm
→ Commit
→ Audit

The system must record:

- original filename
- template type/version
- uploaded by
- uploaded at
- total rows
- valid rows
- invalid rows
- committed rows
- import ID

Templates should never permit direct historical-balance overwrite.
