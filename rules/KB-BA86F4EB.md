---
id: KB-BA86F4EB
subject: BL-LOY-016 A mission goal measures the ORDER TOTAL — shipping and tax included, net of discount
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:23.321Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-016: A mission goal measures the ORDER TOTAL — shipping and tax included, net of discount `[P0-revenue]`

- **Rule:** An `OrderValueGoal`'s progress MUST accrue the order's grand total — merchandise plus shipping plus tax, net of any discount — as the measure of a customer's spend toward the goal. A spend target is a promise about total spend, not about goods alone: shipping and tax the customer actually pays count toward it, and a discount that reduces what the customer actually spent correspondingly reduces progress.
- **Verify:** Place ONE order whose merchandise value falls BELOW the goal's target while its order total rises ABOVE it — a target placed strictly between the two readings is what makes the case decidable (`.claude/rules/test-data.md` §SECOND RULE). Read `loyaltyMissionProgress` for that user: `currentValue` must equal the order total, and the mission must reach `Completed` once that total meets or exceeds the target.
- **Violation signal:** `currentValue` equals the order's merchandise value alone, excluding shipping/tax; a mission fails to complete on an order whose total cleared the target even though its goods did not; a discount that lowered what the customer paid does not lower `currentValue`.
- **Agents:** qa-backend-expert
- **Docs:** N/A — project-specific extension: Loyalty Missions has no VirtoOZ surface. Re-checked 2026-09-14 across PlatformUserGuide, StorefrontUserGuide and the xAPI developer reference; the last ENUMERATES the storefront loyalty surface as `loyaltyBalance` + `loyaltyPointsHistory` and omits the mission queries, so the absence is positive evidence rather than a failed search.
- **Source:** `LoyaltyMissionLogicService.ApplyContribution` — `case OrderValueGoal: return order.Total;` (`:410` at `1be73b4`, `:417` on the deployed `da8abc6`). No `SubTotal` / `DiscountTotal` / `ShippingTotal` / `TaxTotal` appears anywhere in the mission path.
- **Live:** re-measured 2026-09-14 on a multi-organization buyer, three orders against their `OrderValueGoal` progress rows — merchandise/tax/total → accrued `currentValue`: 180/36/**216** → **216**; 420/84/**504** → **504**; 120/24/**144** → **144**. `currentValue` equals the order total exactly in all three, and differs from merchandise every time. This is a direct equality on the Verify's own observable, not a threshold inference. The earlier 2026-09-01 capture (`MSN_E2E_ORDERVALUE_003`: merchandise 45.00, shipping 150.00, tax 39.00, total **234.00** against goal target 49.5 → `Completed`, `currentValue` 234) is retained for the SHIPPING limb only; its fixtures have since been re-seeded and it is NOT re-derivable.
- **Status:** SATISFIED. The implementation accrues `order.Total`, matching the intended behaviour.
- **Amended:** 2026-09-01 — by product decision (tester/PO), not by new evidence: the invariant previously read "merchandise value, not order total" (VIOLATED); no acceptance criterion on VCST-5319 had ever stated which figure a goal measures, so it was written the other way round in the absence of a declared intent. The decision now states `OrderValueGoal` is intended to accrue the order total. The Source, Live and tax-model measurements below are the same measurement as before the amendment and are unchanged — they now evidence satisfaction rather than violation.
- **Measured tax model (refines the arithmetic, not the verdict):** tax is levied at **20% of (subtotal + shipping)** on this environment, across four orders — `$5/$150/$31/$186`, `$20/$150/$34/$204`, `$60/$150/$42/$252`, `$5/$150/$31/$186` (merchandise / shipping / tax / total). So the gap between the two readings widens with the shipping method, not just with the goods: on `CO260901-00040` it is **60 merchandise against 252 accrued**.
- **Amended:** 2026-09-14 (auto-applied, triangulated — BL-AUDIT-2026-09-14) — Rule and Status unchanged; the Live axis was re-derived on current fixtures because the 2026-09-01 measurements rest on re-seeded ones.
- **Evidence basis, stated rather than implied:** the 2026-09-14 re-derivation confirms the `currentValue == order.Total` identity including tax (all three orders carried zero shipping and zero discount). The shipping and discount consequences below remain corollaries of `return order.Total` in source; their only live measurements are from 2026-09-01 fixtures that have since been re-seeded and are not re-derivable.
- **Accepted consequences of this rule (measured, not speculative — now properties of the declared rule, not defects):**
  - **Shipping is a lever.** On `CO260901-00040`, $60 of goods plus $150 shipping completed a $49.50 goal at **currentValue 252** against 60 of merchandise — a customer can reach a spend mission faster by choosing costlier delivery.
  - **A discount moves the customer away from the goal.** `order.Total` is net of discount, so applying a promo code *reduces* progress toward an `OrderValueGoal`.
  - **Tax makes completion jurisdiction-dependent.** At 20% of (subtotal + shipping) on this environment, the same basket can complete a mission in one region and not in another with a different tax rate.
- **Open flag — NOT resolved by this decision** *(historical, not re-derivable — captured 2026-09-01 on fixtures since re-seeded; an equivalent could not be reconstructed on 2026-09-14, so the internal-consistency question stands but its measurement does not)*: the module still accrues spend on two different bases for the same order. On `MSN_E2E_ORDERVALUE_008` the mission advanced to `currentValue` **204** (order total) while the loyalty-PROGRAM path credited **20** for the same purchase — 20 being the merchandise subtotal. Both figures sit in one `loyaltyPointsHistory` read: the two `20` rows carry `{type: CustomerOrder, orderNumber: CO260901-00031 / CO260901-00023}`, and the ten mission-grant rows carry `object: null`. Now that this entry declares the mission path's basis (order total) as intended, the remaining question is a plain internal-consistency one — is the loyalty-PROGRAM path's subtotal-only basis also intended, or should it also move to order total? — and it is **not** answered by the 2026-09-01 decision above. This divergence also bears on BL-LOY-017 (currency filtering) and BL-LOY-015 (attribution); if the two paths turn out to be intentionally different, it may warrant its own invariant rather than living here. Independently read 2026-09-01 (`post-state-independent-audit.json`).
- Evidence artifact: `reports/regression/REG-2026-09-01-1750/post-state-independent-audit.json` — live state re-derived independently against seed generation `20260901153647-7b25` before those fixtures were re-seeded, carrying `missionId` / `userId` per row so the reading is re-checkable rather than merely reported.
- **Promoted:** 2026-09-01 (via `/qa-review-oracles bl`).

---
