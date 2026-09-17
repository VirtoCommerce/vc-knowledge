---
id: KB-BE0CAC7A
subject: BL-IMPEX-002 Export matches admin grid filters
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:17.022Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-IMPEX-002: Export matches admin grid filters `[P1-data]`

- **Rule:** When exporting data from Admin (products, orders, customers), the exported file must contain exactly the records matching the current grid filter/search. Exporting without a filter exports all records. The export format (CSV columns, date format, encoding) must be consistent and documented. Export must handle large datasets without timeout — catalog CSV export always runs as a Hangfire background job (regardless of record count), returning an `ExportNotification` for progress and a blob download URL on completion.
- **Verify:** Filter products by category X (showing 30) → export → CSV contains exactly 30 rows. Clear filter → export → CSV contains all products. Check CSV encoding (UTF-8 BOM for Excel compatibility).
- **Violation signal:** Export includes records outside current filter; export count doesn't match grid count; large export times out with no error; encoding issues (garbled characters).
- **Agents:** qa-backend-expert (export API, Admin SPA)
