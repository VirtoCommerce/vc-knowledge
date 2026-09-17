---
id: KB-8DEE8C5B
subject: BL-LOY-020 The store's loyalty balance calculation mode selects the OWNER SCOPE of the balance; no surface may resolve a different one
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:24.161Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-020: The store's loyalty balance calculation mode selects the OWNER SCOPE of the balance; no surface may resolve a different one `[P0-revenue]`

- **Rule:** The store setting `Loyalty.LoyaltyBalanceCalculationMode` (`Customer` | `Organization`, platform default `Customer`) selects which owner a loyalty balance belongs to, and every surface that reads or writes that balance — cart/order validation, earn, redeem, the account-page balance and points history, missions — MUST resolve the same scope: the individual user in `Customer` mode, the user's organization (pooled across every member) in `Organization` mode. Switching the mode changes which balance is *resolved* on the next read; it MUST NOT alter, migrate or destroy any existing `LoyaltyBalanceOperationLog` row — a user-scope row (`OrganizationId == null`) and an org-scope row (`OrganizationId == <id>`) are permanently disjoint ledgers, never two views onto one total.
- **Verify:** Read the org's pooled balance in `Organization` mode → flip the store to `Customer` → same session, one reload, no re-login → the org figure becomes invisible and each member's own user-scope balance reads independently (0 for a member who never earned in `Customer` scope) → flip back to `Organization` → reload → the pooled figure returns EXACTLY equal to the pre-flip reading. Confirm the ledger itself is untouched across the flip (row count, signed sum, every row's `OrganizationId` unchanged).
- **Violation signal:** A balance reading changes (grows, shrinks or zeroes) across a mode flip with no order placed in between; a user-scope read includes an org-scope row or vice versa; restoring the prior mode does not restore the prior figure exactly; any ledger row's `OrganizationId` or amount changes as a side effect of the settings write.
- **Agents:** qa-backend-expert, qa-frontend-expert
- **Source:** vc-module-loyalty PR #17 (`feat/VCST-5024-org-level`, head `973e7c9`) — write side: `LoyaltyProgramHandler.cs` `RedeemLoyaltyProductsAsync` / `EarnProductPointsAsync` / `EarnLoyaltyProgramAsync`, each gated `if (store.IsOrganizationBalanceCalculationMode()) { loyaltyContext.OrganizationId = order.OrganizationId; }` — the order's own organization decides the write scope, with no contact/member lookup. Read side: `LoyaltyBalanceOperationLogSearchService.cs` `BuildOwnerQuery` (new file lines ~60-86) — `predicate.Or(x => x.UserId == criteria.UserId && x.OrganizationId == null)` OR'd with `predicate.Or(x => x.OrganizationId == criteria.OrganizationId)`, so a user-scope query explicitly excludes org-scoped rows and an org-scope query never falls back to a member's personal rows. This is the unwritten premise **BL-LOY-008** already leans on parenthetically ("the store's loyalty balance calculation mode").
- **Docs:** N/A — unreleased: PR #17 is open, not yet merged or shipped in the Loyalty module, and neither VirtoOZ `PlatformUserGuide` nor `StorefrontUserGuide` documents a balance-calculation-mode setting as of this audit (both queried directly — the loyalty pages cover `Loyalty enabled`, `Loyalty mode`, `Loyalty currency`, product-points factors and points history, nothing about a calculation-mode/owner-scope setting). Unlike BL-LOY-016/017's permanent "project-specific extension, the guides describe no such semantics," this is a temporal gap tied to the feature being unshipped — re-check this axis once PR #17 merges and ships.
- **Live:** Reproduced on `localhost` (`VirtoCommerce.Loyalty@3.1008.0-pr-17-973e`, confirmed via `GET /api/platform/modules` to be PR #17's HEAD OID exactly), independently across two regression runs — `REG-2026-09-14-0928` and `REG-2026-09-16-1624`, both `LOYORG-E2E-005` PASS. 2026-09-14: `BAL_ORG = 163,660` in `Organization` mode → store settings PUT to `Customer`, read-back confirmed effective → same session, one reload → `BAL_CUST = 0` (member's own ledger genuinely empty, not a stale cache) → PUT back to `Organization`, read-back confirmed → `BAL_RESTORED = 163,660`, exactly equal to `BAL_ORG`. Store setting confirmed as `allowedValues: ["Customer","Organization"]`, `defaultValue: "Customer"`, `valueType: ShortText`, `moduleId: VirtoCommerce.Loyalty`, `groupName: Loyalty|Missions`. The flip was independently confirmed a no-op on the ledger itself (ticket VCST-5024, 2026-09-16 run): 33 rows, signed sum 128,593, every row `organizationId=null`, byte-identical before/after, mission progress unchanged. Corroborated by an independent read of the same source in `test-data/aliases.json` (test-data-engineer fixture notes for `ORG_LOY_A`/multi-org-balance fixtures): "A user-scope read is UserId==x AND OrganizationId==null while an organization read is OrganizationId==x, so the two ledgers are independent and an organization-mode earn does NOT credit the earner's own balance."
- **Promoted:** 2026-09-16 (auto-applied, triangulated — BL-AUDIT-2026-09-16; source is an unmerged open PR, see Docs note above).

---
