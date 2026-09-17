---
id: KB-CCF500F3
subject: BL-CR-008 Anonymous visitors cannot access the review-submission control
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:34.876Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CR-008: Anonymous visitors cannot access the review-submission control `[P1-data]`

- **Rule:** The storefront never offers the "leave feedback" control (button or inline form) to an unauthenticated visitor — feedback eligibility is computed only after authentication, so it defaults to unavailable for anonymous sessions. This is reinforced server-side: a review requires a completed order tied to the requesting user's id, which an anonymous session structurally cannot have.
- **Verify:** As an anonymous visitor, open a product page that has approved reviews → the reviews are readable, but no "leave feedback" button or review form is present anywhere in the widget.
- **Violation signal:** An anonymous visitor sees a "leave feedback" button or an open review form; a review is created client-side without the user ever authenticating.
- **Agents:** qa-frontend-expert (storefront gating)
- **Docs:** N/A — client-side gating mechanic; no guide states this in these terms (§1a).
- **Source:** vc-frontend `client-app/modules/customer-reviews/components/product-reviews.vue` — `feedbackAvailable` (ref, defaults `false`) is only assigned inside `onActivated`'s `if (isAuthenticated.value) { … }`; the "Leave feedback" button's `v-if` requires `isAuthenticated && feedbackAvailable`.
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; MISSING → new entry. Docs N/A per §1a; Source + Live agree — an anonymous product-page visit rendered approved reviews with no leave-feedback control.)
