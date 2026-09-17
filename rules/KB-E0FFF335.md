---
id: KB-E0FFF335
subject: BL-LOY-008 Insufficient loyalty balance blocks order creation with a typed `LOYALTY_INSUFFICIENT_BALANCE` error
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:21.929Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-008: Insufficient loyalty balance blocks order creation with a typed `LOYALTY_INSUFFICIENT_BALANCE` error `[P0-revenue]`

- **Rule:** When a cart's loyalty-currency total exceeds the **balance of the scope the store resolves for that cart** — the customer's own balance by default, the organization's pooled balance when the store calculates loyalty per organization (see the store's loyalty balance calculation mode) — `LoyaltyCartValidator` MUST surface a `LOYALTY_INSUFFICIENT_BALANCE` validation error with params `{required, available}` (`required > available`), and the order MUST NOT be created — the shortfall blocks checkout. `available` MUST equal the balance the same actor is shown on their own account page. The cart MUST remain intact and readable.
- **Verify:** Mixed cart whose loyalty total exceeds the resolved balance → cart validation returns `LOYALTY_INSUFFICIENT_BALANCE` with `required`/`available` present and `required > available`; order not created; cart still readable. Storefront surfaces the localized message (i18n `loyalty_insufficient_balance`). Then: read the points balance the actor is shown on their account page, build a cart whose points total exceeds it, and assert `available` equals that same figure — not 0 and not another scope's number.
- **Violation signal:** Order created despite a balance shortfall; missing/empty `required`/`available`; balance allowed to go negative; **or `available` disagrees with the balance the same actor is shown, so an actor who holds enough points is refused (or one who does not is allowed).**
- **Agents:** qa-backend-expert, qa-frontend-expert
- **Source:** vc-module-loyalty `ExperienceApi/Validators/LoyaltyCartValidator.cs:53-77` (rule 4 — the balance is selected by `store.IsOrganizationBalanceCalculationMode()`, and `GetOrganizationBalanceAsync` on an empty owner id yields 0 with no null guard); vc-frontend i18n `loyalty_insufficient_balance`.
- **Docs:** N/A — implementation-detail: typed validation-error code and its parameter contract.
- **Live:** re-verified 2026-09-14 on a deliberately NON-zero balance, so the check is discriminating rather than a trivial zero — a mixed cart whose points total exceeded the resolved balance returned exactly one validation error, `required > available`, with `available` equal to the actor's balance read from the admin API, and the cart stayed readable. Cosmetic, not a violation: `required` serialises as an integer string and `available` with decimal places, so a consumer comparing them as strings would be wrong.
- **Amended:** 2026-09-14 (auto-applied, triangulated — BL-AUDIT-2026-09-14) — "the user's loyalty balance" was stale; the validator compares against whichever scope the store resolves.
- **Promoted:** 2026-06-23.
