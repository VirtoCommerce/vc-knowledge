---
id: KB-FF7A24E6
subject: BL-B2B-002 Organization-specific pricing overrides store default
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:07.127Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-B2B-002: Organization-specific pricing overrides store default `[P0-revenue]`

- **Rule:** When an organization has an assigned price list, those prices override the store's default price list for all members of that organization. The priority chain is: organization price list → store default price list → "Unavailable." If the org price list doesn't cover a product, the store default applies as fallback.
- **Verify:** Org A has custom price list (Product X = $50). Store default has Product X = $75. Sign in as Org A member → Product X shows $50. Sign in as non-org user → Product X shows $75. Check a product NOT in org's price list → should show store default price.
- **Violation signal:** Org member sees store default price instead of org price; non-org user sees org prices; product without org price shows "Unavailable" instead of falling back to store default.
- **Agents:** qa-frontend-expert (price display), qa-backend-expert (price list resolution API)
