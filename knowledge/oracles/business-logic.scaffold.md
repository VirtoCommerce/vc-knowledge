---
applicability: reference
applicability_rationale: "169 BLs (storefront + backend/xAPI/admin) covering pricing, cart, checkout, B2B, loyalty, payment, white-labeling, etc. Universal as a STARTING POINT (most BLs are platform-level invariants). Customer adapts: some BLs encode vcst-specific assumptions (specific currency, specific tier rules, specific role names). Customer's own BL-{CUSTOMER}-* IDs namespace alongside."
---

# Business Logic Invariants — Agent Reference

> **Virto-internal references.** This page cites paths (`regression/suites/...`, `scripts/...`,
> `.claude/...`) and commands (`/qa-test`, `/qa-design`, ...) that live in the VirtoCommerce QA
> repository, which is not part of a plugin install. They are PROVENANCE -- where a claim was
> checked -- never steps you have to follow. Every statement about the platform stands without them.

Testable business rules for the Virto Commerce B2B e-commerce platform. Use this file to judge correctness when specs are ambiguous, absent, or when cross-domain interactions create emergent behavior.

## How to Use This File

- Each invariant has an ID (`BL-DOMAIN-NNN`), a severity tag, a declarative **Rule**, a **Verify** instruction, and a **Violation signal**.
- When a test result is ambiguous, check this file before classifying as PASS or AMBIGUOUS.
- When writing test cases, each business invariant should map to at least one test case assertion.
- Cross-domain invariants (`BL-CROSS-*`) are the highest-value rules — they catch the bugs that single-domain testing misses.
- If observed behavior violates an invariant here, classify as **FAIL** regardless of whether a JIRA spec explicitly covers it.

### Severity Tags

| Tag | Meaning | Test Priority |
|-----|---------|---------------|
| `[P0-revenue]` | Directly impacts revenue, orders, or payments | Must pass before any deployment |
| `[P0-security]` | Security boundary (auth, authz, privilege escalation, data leakage) | Must pass before any deployment |
| `[P1-data]` | Data integrity, state correctness | Must pass before sprint release |
| `[P1-ux]` | UX rule with stakeholder or legal weight (e.g., golden-rule UI sequence, WCAG-overlapping) | Must pass before sprint release |
| `[P2-ux]` | User experience, display, non-blocking | Should pass; acceptable to defer with ticket |

---

## Domain 1: Pricing & Discounts (BL-PRICE)

<!--RULE BL-PRICE-001-->

<!--RULE BL-PRICE-002-->

<!--RULE BL-PRICE-003-->

<!--RULE BL-PRICE-004-->

<!--RULE BL-PRICE-005-->

<!--RULE BL-PRICE-006-->

<!--RULE BL-PRICE-007-->

<!--RULE BL-PRICE-008-->

<!--RULE BL-PRICE-009-->

## Domain 2: Cart (BL-CART)

<!--RULE BL-CART-001-->

<!--RULE BL-CART-002-->

<!--RULE BL-CART-003-->

<!--RULE BL-CART-004-->

<!--RULE BL-CART-005-->

<!--RULE BL-CART-006-->

<!--RULE BL-CART-007-->

<!--RULE BL-CART-008-->

<!--RULE BL-CART-009-->

<!--RULE BL-CART-010-->

<!--RULE BL-CART-011-->

<!--RULE BL-CART-012-->

<!--RULE BL-CART-013-->

<!--RULE BL-CART-014-->

<!--RULE BL-CART-015-->

## Domain 3: Checkout (BL-CHK)

<!--RULE BL-CHK-001-->

<!--RULE BL-CHK-002-->

<!--RULE BL-CHK-003-->

<!--RULE BL-CHK-004-->

<!--RULE BL-CHK-005-->

<!--RULE BL-CHK-006-->

<!--RULE BL-CHK-007-->

<!--RULE BL-CHK-008-->

## Domain 4: Orders & Fulfillment (BL-ORD)

<!--RULE BL-ORD-001-->

<!--RULE BL-ORD-002-->

<!--RULE BL-ORD-003-->

<!--RULE BL-ORD-004-->

<!--RULE BL-ORD-005-->

<!--RULE BL-ORD-006-->

<!--RULE BL-ORD-007-->

<!--RULE BL-ORD-009-->

<!--RULE BL-ORD-008-->

<!--RULE BL-ORD-010-->

## Domain 5: Users & Authentication (BL-AUTH)

<!--RULE BL-AUTH-001-->

<!--RULE BL-AUTH-002-->

<!--RULE BL-AUTH-003-->

<!--RULE BL-AUTH-004-->

<!--RULE BL-AUTH-005-->

<!--RULE BL-AUTH-006-->

<!--RULE BL-AUTH-007-->

<!--RULE BL-AUTH-008-->

<!--RULE BL-AUTH-009-->

<!--RULE BL-AUTH-010-->

<!--RULE BL-AUTH-011-->

<!--RULE BL-AUTH-012-->

<!--RULE BL-AUTH-013-->

<!--RULE BL-AUTH-014-->

<!--RULE BL-AUTH-015-->

<!--RULE BL-AUTH-016-->

<!--RULE BL-AUTH-017-->

## Domain 6: B2B / Organization (BL-B2B)

<!--RULE BL-B2B-001-->

<!--RULE BL-B2B-002-->

<!--RULE BL-B2B-003-->

<!--RULE BL-B2B-004-->

<!--RULE BL-B2B-005-->

<!--RULE BL-B2B-006-->

<!--RULE BL-B2B-007-->

<!--RULE BL-B2B-008-->

<!--RULE BL-B2B-009-->

<!--RULE BL-B2B-010-->

<!--RULE BL-B2B-011-->

<!--RULE BL-B2B-012-->

<!--RULE BL-B2B-013-->

## Domain 7: Catalog & Inventory (BL-CAT)

<!--RULE BL-CAT-001-->

<!--RULE BL-CAT-002-->

<!--RULE BL-CAT-003-->

<!--RULE BL-CAT-004-->

<!--RULE BL-CAT-005-->

<!--RULE BL-CAT-006-->

<!--RULE BL-CAT-007-->

<!--RULE BL-CAT-008-->

<!--RULE BL-CAT-009-->

<!--RULE BL-CAT-010-->

<!--RULE BL-CAT-011-->

<!--RULE BL-CAT-012-->

## Domain 8: Cross-Domain Invariants (BL-CROSS)

These invariants span multiple modules and are where the most expensive production bugs hide. Agents should prioritize these during regression testing.

<!--RULE BL-CROSS-001-->

<!--RULE BL-CROSS-002-->

<!--RULE BL-CROSS-003-->

<!--RULE BL-CROSS-004-->

<!--RULE BL-CROSS-005-->

<!--RULE BL-CROSS-006-->

<!--RULE BL-CROSS-007-->

<!--RULE BL-CROSS-008-->

<!--RULE BL-CROSS-009-->

<!--RULE BL-CROSS-010-->

<!--RULE BL-CROSS-011-->

<!--RULE BL-CROSS-012-->

## Domain 9: Search (BL-SRCH)

<!--RULE BL-SRCH-001-->

<!--RULE BL-SRCH-002-->

<!--RULE BL-SRCH-003-->

<!--RULE BL-SRCH-004-->

<!--RULE BL-SRCH-005-->

## Domain 10: Shipping & BOPIS (BL-SHIP)

<!--RULE BL-SHIP-001-->

<!--RULE BL-SHIP-002-->

<!--RULE BL-SHIP-003-->

<!--RULE BL-SHIP-004-->

## Domain 10a: BOPIS-Specific Rules (BL-BOPIS)

These invariants are extracted from BOPIS suite assertions (suites 036–038). They complement the general Shipping & BOPIS rules in Domain 10 with BOPIS-specific behavioral contracts.

<!--RULE BL-BOPIS-001-->

<!--RULE BL-BOPIS-002-->

<!--RULE BL-BOPIS-003-->

<!--RULE BL-BOPIS-004-->

<!--RULE BL-BOPIS-005-->

<!--RULE BL-BOPIS-006-->

<!--RULE BL-BOPIS-007-->

<!--RULE BL-BOPIS-008-->

## Domain 11: Notifications (BL-NOTIF)

<!--RULE BL-NOTIF-001-->

<!--RULE BL-NOTIF-002-->

<!--RULE BL-NOTIF-003-->
<!--RULE BL-NOTIF-004-->

<!--RULE BL-NOTIF-005-->

<!--RULE BL-NOTIF-006-->

<!--RULE BL-NOTIF-007-->

## Domain 12: Import / Export (BL-IMPEX)

<!--RULE BL-IMPEX-001-->

<!--RULE BL-IMPEX-002-->

<!--RULE BL-IMPEX-003-->

<!--RULE BL-IMPEX-004-->

## Domain 13: SEO & URLs (BL-SEO)

<!--RULE BL-SEO-001-->

<!--RULE BL-SEO-002-->

<!--RULE BL-SEO-003-->

<!--RULE BL-SEO-004-->

## Domain 14: Profile & Member Data (BL-PROFILE)

<!--RULE BL-PROFILE-001-->

## Domain 15: UI Display & Layout Stability (BL-UI)

These invariants hold for any rendered surface — Storybook stories, storefront pages, admin SPA blades — regardless of feature spec or Figma source. They turn "looks broken" into "measurably violates a rule." Violations are FAIL even when a JIRA ticket does not call them out, because the design system contract makes them implicit acceptance criteria. Canonical measurement helper: `scripts/lib/measure-layout.ts`. **No regression suite currently covers these invariants** — suite `048b-layout-stability.csv` (selection group `layout-stability`) was removed on 2026-07-25. Until a replacement exists they are audited on demand via ``/qa-design`` (`.claude/skills/qa-design/SKILL.md` in the vc-mcp-testing-module repo; not shipped in vc-fix) against the scope + audit protocols in [`critical-ui-scope.md`](critical-ui-scope.md).

<!--RULE BL-UI-001-->

<!--RULE BL-UI-002-->

<!--RULE BL-UI-003-->

<!--RULE BL-UI-004-->

<!--RULE BL-UI-005-->

<!--RULE BL-UI-006-->
<!--RULE BL-UI-007-->

## Domain 16: GraphQL xAPI Contract (BL-GQL)

Transport-layer invariants for the xAPI GraphQL endpoint at `{BACK_URL}/graphql`. These rules apply across every GraphQL operation regardless of resolver domain, and are enforced by `scripts/graphql/graphql-runner.ts` for runner-native test cases. See `graphql-schema.md` for the schema reference and `graphql-test-cases-runner.md` for the authoring contract.

<!--RULE BL-GQL-001-->

<!--RULE BL-GQL-002-->

<!--RULE BL-GQL-003-->

<!--RULE BL-GQL-004-->

## Domain 17: Loyalty & Mixed Cart (BL-LOY)

> Loyalty "Mixed Cart" mode (store setting `Loyalty.Mode = "Mixed Cart"`, `Loyalty.Currency` e.g. `PTS`) lets a single cart hold regular primary-currency lines and loyalty (points-currency) lines simultaneously, with one checkout. These invariants govern that model. Introduced with VCST-5101 / Epic VCST-5099 (vc-module-x-cart PR #120, vc-module-cart PR #188, vc-frontend PR #2310). See BL-CART-004 (currency switch, amended) and BL-CART-003 (coupon + sale).

<!--RULE BL-LOY-001-->

<!--RULE BL-LOY-002-->

<!--RULE BL-LOY-003-->

<!--RULE BL-LOY-004-->

<!--RULE BL-LOY-005-->

<!--RULE BL-LOY-006-->

<!--RULE BL-LOY-007-->

<!--RULE BL-LOY-008-->

<!--RULE BL-LOY-009-->

<!--RULE BL-LOY-010-->

<!--RULE BL-LOY-012-->

<!--RULE BL-LOY-013-->

<!--RULE BL-LOY-014-->


<!--RULE BL-LOY-015-->


<!--RULE BL-LOY-016-->

<!--RULE BL-LOY-017-->

<!--RULE BL-LOY-018-->


<!--RULE BL-LOY-019-->

<!--RULE BL-LOY-020-->

## Domain 18: Payment Processors (BL-PAY)

<!--RULE BL-PAY-001-->

<!--RULE BL-PAY-003-->

<!--RULE BL-PAY-004-->

## Domain 19: White Labeling (BL-WL)

Per-org / per-store branding resolved after sign-in by the White Labeling module's xAPI query
(`GetWhiteLabelingSettingsQueryHandler`). These supersede the WL-specific detail formerly carried by
`BL-B2B-006` (see the cross-reference there). Grounded in `vc-module-white-labeling` source + live
the environment verification (TLC-2026-07-02-2043).

<!--RULE BL-WL-001-->

<!--RULE BL-WL-002-->

<!--RULE BL-WL-003-->

<!--RULE BL-WL-004-->

<!--RULE BL-WL-005-->

<!--RULE BL-WL-006-->

## Domain 20: Sales Rep (BL-SR)

Scoped storefront GraphQL surface for sales representatives (`POST /graphql/sales-rep`) — the customers a rep serves, their orders, and dashboard/customer-profile **statistics** (order purchases, carts/projects, customer counters, top-selling products) with a server-owned filter+sort rule vocabulary. Grounded in `vc-module-sales-rep` (PR #4, epic VCST-5142; tickets VCST-5309/5362/5368/5485) README + live verification on vcst-qa (module `SalesRep_3.1000.0-pr-4`, TLC-2026-07-23-1943). Every query is authenticated and **creator + membership scoped**. Also covers the storefront hub-access gate (VCST-5494) and the embedded back-office Admin app's RBAC model (VCST-5293), audited separately on 2026-07-24 (TLC-2026-07-24-1906, BL-AUDIT-2026-07-24).

<!--RULE BL-SR-001-->

<!--RULE BL-SR-002-->

<!--RULE BL-SR-003-->

<!--RULE BL-SR-004-->

<!--RULE BL-SR-005-->

<!--RULE BL-SR-006-->

<!--RULE BL-SR-007-->

<!--RULE BL-SR-008-->

<!--RULE BL-SR-009-->

<!--RULE BL-SR-010-->

<!--RULE BL-SR-011-->

<!--RULE BL-SR-012-->

<!--RULE BL-SR-013-->

<!--RULE BL-SR-014-->

<!--RULE BL-SR-015-->

<!--RULE BL-SR-016-->

<!--RULE BL-SR-017-->

<!--RULE BL-SR-018-->

<!--RULE BL-SR-019-->

<!--RULE BL-SR-020-->

<!--RULE BL-SR-021-->

<!--RULE BL-SR-022-->

<!--RULE BL-SR-023-->

<!--RULE BL-SR-024-->

<!--RULE BL-SR-025-->

<!--RULE BL-SR-026-->

<!--RULE BL-SR-027-->

<!--RULE BL-SR-028-->

<!--RULE BL-SR-029-->

<!--RULE BL-SR-030-->

<!--RULE BL-SR-031-->

<!--RULE BL-SR-032-->

## Domain 21: Accessibility (BL-A11Y)

These invariants hold for any rendered customer-facing surface on the accessibility-gated storefront themes (and, per BL-UI-007, the Admin SPA), and are grounded directly in the WCAG 2.1/2.2 success criteria rather than in Virto documentation, which states no conformance target (bl-audit-criteria §1a class 2 — same basis as BL-UI-006/BL-UI-007). Exercised by `045-accessibility-tests.csv` via axe-core scans, keyboard-only traversal, and accessibility-tree observation.

<!--RULE BL-A11Y-001-->

<!--RULE BL-A11Y-002-->

<!--RULE BL-A11Y-003-->

<!--RULE BL-A11Y-004-->

## Domain 22: Customer Reviews (BL-CR)

> Added 2026-08-24 (BL-AUDIT-2026-08-24). 51 test cases across suites `086`/`087`/`088` were already citing `BL-CR-*` ids that did not exist — false traceability (BLC-002). The ids were confirmed free (never used, never retired), so each entry is added at the exact cited id, which makes every existing citation true. Nine of the eighteen cited ids are landed here; the other nine are evidenced on Source but were not live-exercised this run and are staged in `reports/ba/bl-proposals-2026-08-24.md` rather than guessed at.

<!--RULE BL-CR-001-->

<!--RULE BL-CR-008-->

<!--RULE BL-CR-009-->

<!--RULE BL-CR-010-->

<!--RULE BL-CR-012-->

<!--RULE BL-CR-013-->

<!--RULE BL-CR-016-->

<!--RULE BL-CR-017-->

<!--RULE BL-CR-018-->

## Domain 23: Platform Administration (BL-PLAT)

> Added 2026-08-24 (BL-AUDIT-2026-08-24). 92 test cases across suites `020`/`021` were citing `BL-PLAT-*` ids that did not exist (BLC-002 false traceability). The ids were confirmed free, so each entry is added at the exact cited id. **`BL-PLAT-003` is deliberately absent** — no case cites it, and the gap is real rather than an oversight.

<!--RULE BL-PLAT-001-->

<!--RULE BL-PLAT-002-->

<!--RULE BL-PLAT-004-->

## Domain 24: Store Management (BL-STORE)

> Added 2026-08-24 (BL-AUDIT-2026-08-24). 69 test cases across suites `034`/`035` were citing `BL-STORE-*` ids that did not exist (BLC-002). Only `BL-STORE-001` cleared the evidence bar. **`BL-STORE-002` was declined outright** — its sole citing case asserts plain CRUD, and the one plausible invariant behind it (a default language must remain within the store's available-languages list) is *refuted* by source: the store validator constrains only the store id and declares no cross-field rule. `BL-STORE-003` and `BL-STORE-004` are evidenced on docs alone and are staged in `reports/ba/bl-proposals-2026-08-24.md`.

<!--RULE BL-STORE-001-->

## Invariant Coverage Summary

P0 column rolls up `[P0-revenue]` + `[P0-security]`; P1 column rolls up `[P1-data]` + `[P1-ux]`.

| Domain | ID Range | Total | P0 | P1 | P2 |
|--------|----------|-------|----|----|----|
| Pricing & Discounts | BL-PRICE-001–008 | 8 | 7 | 1 | 0 |
| Cart | BL-CART-001–015 | 15 | 5 | 10 | 0 |
| Checkout | BL-CHK-001–008 | 8 | 5 | 3 | 0 |
| Orders & Fulfillment | BL-ORD-001–010 | 10 | 3 | 7 | 0 |
| Users & Auth | BL-AUTH-001–016 | 16 | 5 | 10 | 1 |
| B2B / Organization | BL-B2B-001–013 | 13 | 4 | 9 | 0 |
| Catalog & Inventory | BL-CAT-001–012 | 12 | 2 | 6 | 4 |
| Cross-Domain | BL-CROSS-001–012 | 12 | 7 | 5 | 0 |
| Search | BL-SRCH-001–005 | 5 | 0 | 3 | 2 |
| Shipping & BOPIS | BL-SHIP-001–004 | 4 | 2 | 2 | 0 |
| BOPIS-Specific | BL-BOPIS-001–008 | 8 | 1 | 6 | 1 |
| Notifications | BL-NOTIF-001–007 | 7 | 1 | 5 | 1 |
| Import / Export | BL-IMPEX-001–004 | 4 | 0 | 4 | 0 |
| SEO & URLs | BL-SEO-001–004 | 4 | 0 | 2 | 2 |
| Profile & Member Data | BL-PROFILE-001 | 1 | 0 | 1 | 0 |
| UI Display & Layout Stability | BL-UI-001–007 | 7 | 0 | 2 | 5 |
| GraphQL xAPI Contract | BL-GQL-001–004 | 4 | 1 | 2 | 1 |
| Loyalty & Mixed Cart | BL-LOY-001–014 (011 reserved) | 13 | 4 | 7 | 2 |
| Payment Processors | BL-PAY-001/003/004 | 3 | 3 | 0 | 0 |
| White Labeling | BL-WL-001–006 | 6 | 0 | 2 | 4 |
| Sales Rep | BL-SR-001–032 | 32 | 3 | 18 | 11 |
| **Total** | | **192** | **53** | **105** | **34** |
