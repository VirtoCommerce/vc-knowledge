---
id: KB-ECD247CD
subject: BL-CR-018 Every admin review-moderation action is gated by its own named permission
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:36.616Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CR-018: Every admin review-moderation action is gated by its own named permission `[P0-security]`

- **Rule:** Each admin review-management REST action requires a distinct permission from the CustomerReviews permission group: search/list requires read; approve/reject/reset/create-or-update require update; delete requires delete; reading the aggregate rating requires the rating-read permission; triggering a store-wide rating recalculation requires the rating-recalculate permission. A caller lacking the specific permission for an action receives HTTP 403, regardless of whether they hold other CustomerReviews permissions.
- **Verify:** With a token holding only the read permission, call search → succeeds; call approve/reject/reset/delete/recalculate → each returns 403. With a token holding no CustomerReviews permissions, every one of these endpoints returns 403.
- **Violation signal:** An action succeeds for a caller who holds only a different CustomerReviews permission (e.g. a read-only token can approve); any of these endpoints is reachable with no permission at all.
- **Agents:** qa-backend-expert (REST endpoints, RBAC)
- **Docs:** N/A — the specific permission-to-endpoint mapping is an implementation detail (§1a); the general RBAC model is documented but not per-module.
- **Source:** vc-module-customer-review `Core/ModuleConstants.cs` (the five `customerReviews:*` permission constants) + `CustomerReviewsModuleController.cs` and `CustomerReviewsModuleRatingController.cs` (`[Authorize(...)]` on every admin endpoint).
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; MISSING → new entry. Docs N/A per §1a; Source + Live agree — the Admin role-assignment panel lists exactly these five permissions.)

---
