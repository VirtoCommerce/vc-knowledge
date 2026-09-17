---
id: KB-FB80DC6A
subject: BL-CR-012 Admin review search filters by status, entity type, rating range, and keyword
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:35.647Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CR-012: Admin review search filters by status, entity type, rating range, and keyword `[P2-ux]`

- **Rule:** The admin review search accepts an array of review statuses, an entity type, a rating range (start/end), and a keyword, narrowing the result set to exactly the matching rows; every visible column (Title, Rating, Created date, Status, Created by) is independently sortable.
- **Verify:** In Admin, filter by Status = Approved → only Approved rows shown; filter by Status = New → only New rows shown. Filter by a rating window → each window returns only in-range rows. Sort by Created date or Rating → the list reorders accordingly.
- **Violation signal:** A status/rating-range filter leaves out-of-scope rows in the result; sorting a column has no effect; the entity-type filter has no effect.
- **Agents:** qa-backend-expert (REST search), qa-testing-expert (Admin UI)
- **Docs:** PlatformUserGuide "Overview > Key features" ("use rating information for sorting and filtering review objects").
- **Source:** vc-module-customer-review `CustomerReviewsModuleController.cs` (`SearchCustomerReviews`, `[Authorize(CustomerReviewRead)]`) + `Core/Models/CustomerReviewSearchCriteria.cs` (`ReviewStatus[]`, `StartRating`/`EndRating`) + `CustomerReviewsQueryHandler.cs` (maps a parsed rating range onto `StartRating`/`EndRating`).
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; MISSING → new entry. Docs + Source + Live agree.)
