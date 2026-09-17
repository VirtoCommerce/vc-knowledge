---
id: KB-CEDE7DA9
subject: BL-SR-020 `scope` and `region.id` are free-form strings, not enums; an unrecognized value fails silently to a different (empty) document
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:30.543Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-020: `scope` and `region.id` are free-form strings, not enums; an unrecognized value fails silently to a different (empty) document `[P1-data]`

- **Rule:** Neither the layout surface identifier (`scope`) nor a region id is validated against a fixed vocabulary — both are plain `String` arguments. An unrecognized value never errors; it addresses a document that has never been saved (`null` on read; a new, independent document on write).
- **Verify:** Query/save with a `scope` (or `region.id`) value no known surface/region uses → HTTP 200; read resolves `null`; write creates a new, independent document, never the "real" surface's (SR-GQL-105/114).
- **Violation signal:** An unrecognized `scope`/region id errors instead of addressing an empty document, or is silently coerced to a known value.
- **Agents:** qa-backend-expert
- **Source:** `vc-module-sales-rep` `InputSalesRepLayoutType`/`SalesRepLayoutQuery` (`Scope`/region id typed `StringGraphType`, no enum/validator); `LayoutService.BuildNameParts` (raw string concatenation, no allow-list check).
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04; source + live CONFIRM; docs N/A).
