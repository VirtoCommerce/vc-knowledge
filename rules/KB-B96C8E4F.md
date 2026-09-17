---
id: KB-B96C8E4F
subject: BL-CR-010 The storefront review surface exposes only Approved reviews
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:35.403Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CR-010: The storefront review surface exposes only Approved reviews `[P1-data]`

- **Rule:** Every storefront-facing review read (the reviews list and its total count) is scoped server-side to `ReviewStatus = Approved` — New and Rejected reviews are never included, regardless of any client-supplied filter. The surface supports pagination and sorting by creation date.
- **Verify:** Seed a product with a mix of Approved, New, and Rejected reviews → the public query's total count and item list equal the Approved subset only. With more reviews than one page, navigate pages → each page returns the expected slice, total count stable across pages. Switch sort direction → item order reverses.
- **Violation signal:** A New or Rejected review appears in the public review list or inflates its total count; pagination returns duplicate or missing items across pages; sort direction has no effect.
- **Agents:** qa-backend-expert (GraphQL query), qa-frontend-expert (storefront widget)
- **Docs:** PlatformUserGuide "Overview > Key features" ("You can use rating information for sorting and filtering review objects. Ratings and reviews can be displayed to users upon request.").
- **Source:** vc-module-customer-review `CustomerReviewsQueryHandler.cs` `GetSearchCriteria` — `// XAPI only operates with approved reviews`, then `criteria.ReviewStatus = [CustomerReviewStatus.Approved];`, applied unconditionally before any client filter.
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; MISSING → new entry. Docs + Source + Live agree — a product's review widget rendered exactly its approved-review count, paginated and sortable.)
