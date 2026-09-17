---
id: KB-FEB9A87F
subject: BL-IMPEX-001 CSV import is idempotent
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:16.846Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-IMPEX-001: CSV import is idempotent `[P1-data]`

- **Rule:** Re-importing the same CSV file must update existing records (matched by ID or code), not create duplicates. The import uses a unique identifier (product code, SKU, or explicit ID column) for matching. If the identifier is missing or ambiguous, the import must fail with a clear error — not silently create duplicates.
- **Verify:** Import CSV with 50 products → 50 products created. Re-import same CSV → still 50 products (updated, not 100). Modify one row → re-import → only that product updated, others unchanged.
- **Violation signal:** Re-import doubles the record count; duplicate products with same code; import succeeds without unique identifier; modified records not updated on re-import.
- **Agents:** qa-backend-expert (import API, Admin SPA)
