---
id: KB-1B86FAE2
subject: BL-CART-007 Same product adds quantity, not duplicate line
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:01.303Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CART-007: Same product adds quantity, not duplicate line `[P1-data]`

- **Rule:** Adding the same SKU to the cart a second time increments the existing line item's quantity — it does not create a duplicate line. This applies regardless of whether the add came from PDP, quick-add, or xAPI. Exception: different product configurations (variants) create separate lines.
- **Verify:** Add Product A (qty 1) → go back to listing → add Product A again → cart shows 1 line with qty 2, not 2 lines with qty 1.
- **Violation signal:** Duplicate line items for the same SKU; quantity not incremented on re-add; line count increases on every add.
- **Agents:** qa-frontend-expert (cart UI), qa-backend-expert (addToCart mutation)
- **Docs:** N/A — implementation-detail (merge-vs-duplicate mechanics; VirtoOZ guides do not narrate this, per §1a).
- **Source:** vc-module-x-cart `CartAggregate.InnerAddLineItemAsync` / `FindExistingLineItemBeforeAdd` — merges by incrementing the existing non-configured line's quantity; configured/variant items bypass the merge lookup entirely (`IsConfigured ? null : ...`), always creating a new line.
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; CONFIRMED — Source anchor added, Rule unchanged. Live: same SKU added from PDP then from a listing produced one line at qty 2.)
