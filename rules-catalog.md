# Rules

Constraints written down by people, not observations made by agents. 217 active rules, in 25 domains.

A rule is identified by its ID and by nothing else. Two rules about one coordinate are the
normal case, so the coordinate rule that identifies a fact would refuse half of these.

READ THE TRUST COLUMNS. `confirmations` counts parties that have SEEN this rule hold on a
deployment, and a rule carried over from a page starts at zero however confident the page was.
`disputed` means somebody observed the opposite here; those are the rules to read first, and
a disagreement between a rule and an observation is a finding rather than a mistake.

This catalog is a reference. It is ordered by domain and by id so it can be looked up in, not
scanned top to bottom: read the domain you are working in, with `kb rules <domain>`.

## BL-A11Y — 4

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-A11Y-001`](rules/KB-504415B1.md) | Keyboard operability and focus management | P1-data | 0 | no | no |
| [`BL-A11Y-002`](rules/KB-F26CF242.md) | Accessible naming and label association | P1-data | 0 | no | no |
| [`BL-A11Y-003`](rules/KB-3BE3C452.md) | Color contrast and non-color status differentiation | P1-data | 0 | no | no |
| [`BL-A11Y-004`](rules/KB-37F56F87.md) | Programmatic status, state, and role correctness (axe-clean) | P1-data | 0 | no | no |

## BL-AUTH — 17

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-AUTH-001`](rules/KB-B419E079.md) | Session expiry during checkout | P0-revenue | 0 | no | no |
| [`BL-AUTH-002`](rules/KB-5CDF1F98.md) | Email verification gate | P1-data | 0 | no | no |
| [`BL-AUTH-003`](rules/KB-A75575A5.md) | Account lockout after N failed attempts | P1-data | 0 | no | no |
| [`BL-AUTH-004`](rules/KB-AD80DA75.md) | Returning vs new customer defaults | P2-ux | 0 | no | no |
| [`BL-AUTH-005`](rules/KB-70B9AFE3.md) | RBAC 6-permission model | P1-data | 0 | no | no |
| [`BL-AUTH-006`](rules/KB-185F068A.md) | Role hierarchy | P1-data | 0 | no | no |
| [`BL-AUTH-007`](rules/KB-67DFD94B.md) | Storefront logout UX — popup-only `[P1-ux]` `[GOLDEN RULE]` | P1-ux | 0 | no | no |
| [`BL-AUTH-008`](rules/KB-07DEC824.md) | Self-impersonation must have a defined non-circular outcome | P1-data | 0 | no | no |
| [`BL-AUTH-009`](rules/KB-A06463B6.md) | Nested impersonation forbidden — no silent path from impersonated session | P0-security | 0 | no | no |
| [`BL-AUTH-010`](rules/KB-28BAA7E4.md) | Impersonation banner must persist across SPA navigation | P1-ux | 0 | no | no |
| [`BL-AUTH-011`](rules/KB-CDFF053B.md) | Stop Impersonation must restore operator session without sign-in round-trip | P1-data | 0 | no | no |
| [`BL-AUTH-012`](rules/KB-2E5BD401.md) | Org-scoped lockout does not touch the global account | P0-revenue | 0 | no | no |
| [`BL-AUTH-013`](rules/KB-4CFD39E7.md) | Org-scoped access refusal is distinct from global lockout, and per-cause | P1-data | 0 | no | no |
| [`BL-AUTH-014`](rules/KB-F312A25F.md) | Admin/Platform API cookie-auth challenge returns a status code, never a login-page redirect | P1-data | 0 | no | no |
| [`BL-AUTH-015`](rules/KB-7E2DB992.md) | Active organization resolves by a fixed 5-step chain over *accessible* orgs only | P0-revenue | 0 | no | no |
| [`BL-AUTH-016`](rules/KB-C00F73B8.md) | An org refusal is a single code, lock-first, and only when no fallback remains | P0-revenue | 0 | no | no |
| [`BL-AUTH-017`](rules/KB-B48B6031.md) | A malformed REST request body yields 400, never 500 | P1-data | 0 | no | no |

## BL-B2B — 13

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-B2B-001`](rules/KB-B5E612CD.md) | Org switching isolates cart, addresses, and lists | P0-revenue | 0 | no | no |
| [`BL-B2B-002`](rules/KB-FF7A24E6.md) | Organization-specific pricing overrides store default | P0-revenue | 0 | no | no |
| [`BL-B2B-003`](rules/KB-E98ECDD4.md) | Quote expiry makes quote non-convertible | P1-data | 0 | no | no |
| [`BL-B2B-004`](rules/KB-7DC9FF9A.md) | Pre-purchase approval is quote-based; no native per-order spending limit | P0-revenue | 0 | no | no |
| [`BL-B2B-005`](rules/KB-B152EF7F.md) | Member role determines feature visibility | P1-data | 0 | no | no |
| [`BL-B2B-006`](rules/KB-B4C607F5.md) | White labeling resolution order `[P1-data]` → superseded by Domain 19 (BL-WL) | P1-data | 0 | no | no |
| [`BL-B2B-007`](rules/KB-0FEC9A1E.md) | Per-org JWT permission set is org-scoped; pageContext must match it | P0-revenue | 0 | no | no |
| [`BL-B2B-008`](rules/KB-271C766B.md) | Org-scoped role change mutates only the target org's membership | P1-data | 0 | no | no |
| [`BL-B2B-009`](rules/KB-CCA79536.md) | Inviting a member creates a per-org membership, not a global role | P1-data | 0 | no | no |
| [`BL-B2B-010`](rules/KB-96DC8C1D.md) | Self-service company registration grants org-membership roles only, never global roles | P1-data | 0 | no | no |
| [`BL-B2B-011`](rules/KB-D9D148C8.md) | Org role whitelist scopes assignable roles; enforcement is a planned server-side gate | P1-data | 0 | no | no |
| [`BL-B2B-012`](rules/KB-9C1C25C8.md) | Declining or revoking an invite changes a status — it never deletes the membership row | P1-data | 0 | no | no |
| [`BL-B2B-013`](rules/KB-0D10F432.md) | Membership status resolves per-org-first, then contact, then `Approved` | P1-data | 0 | no | no |

## BL-BOPIS — 8

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-BOPIS-001`](rules/KB-B1603F48.md) | Cart-level Pickup toggle assigns a single pickup shipment to all items | P1-data | 0 | no | no |
| [`BL-BOPIS-002`](rules/KB-C356AA26.md) | BOPIS pickup always has $0 shipping cost | P0-revenue | 0 | no | no |
| [`BL-BOPIS-003`](rules/KB-D0F4D597.md) | FFC availability label matches actual stock level | P1-data | 0 | no | no |
| [`BL-BOPIS-004`](rules/KB-7F57B1FE.md) | BOPIS store-selector modal is view-only on PDP | P1-data | 0 | no | no |
| [`BL-BOPIS-005`](rules/KB-0CD692B0.md) | Inactive or closed pickup locations excluded from selector | P1-data | 0 | no | no |
| [`BL-BOPIS-006`](rules/KB-0B50A3FF.md) | BOPIS checkout requires billing address, skips shipping address | P1-data | 0 | no | no |
| [`BL-BOPIS-007`](rules/KB-1FDE65F5.md) | BOPIS store-selector map does not collapse on no-results search | P2-ux | 0 | no | no |
| [`BL-BOPIS-008`](rules/KB-88B8F127.md) | Confirmed cart pickup location always returned at items[0] of cartPickupLocations | P1-data | 0 | no | no |

## BL-CART — 15

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-CART-001`](rules/KB-8B9AE2D8.md) | Max quantity enforcement | P0-revenue | 0 | no | no |
| [`BL-CART-002`](rules/KB-4FFECD14.md) | Out-of-stock mid-session | P0-revenue | 0 | no | no |
| [`BL-CART-003`](rules/KB-1390E170.md) | Coupon + sale interaction | P0-revenue | 0 | no | no |
| [`BL-CART-004`](rules/KB-EF3B5E1D.md) | Currency switching recalculates primary-currency lines | P0-revenue | 0 | no | no |
| [`BL-CART-005`](rules/KB-A9B09E18.md) | Cart isolation per organization | P1-data | 0 | no | no |
| [`BL-CART-006`](rules/KB-8F560F4E.md) | Pack size enforcement | P1-data | 0 | no | no |
| [`BL-CART-007`](rules/KB-1B86FAE2.md) | Same product adds quantity, not duplicate line | P1-data | 0 | no | no |
| [`BL-CART-008`](rules/KB-6B419C42.md) | Cart persistence across sign-out / sign-in | P1-data | 0 | no | no |
| [`BL-CART-009`](rules/KB-3880EFC1.md) | Storefront cart enforces a single active coupon slot | P1-data | 0 | no | no |
| [`BL-CART-010`](rules/KB-CA616E21.md) | Configuration-item selection reprices the parent configurable lineItem | P0-revenue | 0 | no | no |
| [`BL-CART-011`](rules/KB-BE1C0B7F.md) | Unmatched section key in batch selection is a silent no-op | P1-data | 0 | no | no |
| [`BL-CART-012`](rules/KB-EB21931A.md) | Configuration-item selection mutations are scoped to one `lineItemId`; "all" never crosses lineItem boundaries | P1-data | 0 | no | no |
| [`BL-CART-013`](rules/KB-47FB8513.md) | No-change short-circuit on configuration-item selection mutations | P1-data | 0 | no | no |
| [`BL-CART-014`](rules/KB-B33E8579.md) | Configuration-section identification — `(sectionId, type)` for Text/File; `option.productId` required for Variation | P1-data | 0 | no | no |
| [`BL-CART-015`](rules/KB-1960CBCD.md) | Configuration items survive a Saved-for-Later round trip | P1-data | 0 | no | no |

## BL-CAT — 12

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-CAT-001`](rules/KB-B3DC5FBD.md) | Stock zero disables purchase | P0-revenue | 0 | no | no |
| [`BL-CAT-002`](rules/KB-06A612F4.md) | Virtual catalog inherits physical catalog changes | P1-data | 0 | no | no |
| [`BL-CAT-003`](rules/KB-E8D781F6.md) | Search index lag window | P2-ux | 0 | no | no |
| [`BL-CAT-004`](rules/KB-CE3BF15F.md) | Category visibility toggle | P2-ux | 0 | no | no |
| [`BL-CAT-005`](rules/KB-E2644091.md) | Product requires virtual catalog assignment for storefront | P1-data | 0 | no | no |
| [`BL-CAT-006`](rules/KB-27A299E4.md) | Configurable product requires all sections filled | P0-revenue | 0 | no | no |
| [`BL-CAT-007`](rules/KB-19AAE1DB.md) | Multi-FFC inventory aggregation | P1-data | 0 | no | no |
| [`BL-CAT-008`](rules/KB-6D71C5D0.md) | Unit-of-measure CRUD integrity | P2-ux | 0 | no | no |
| [`BL-CAT-009`](rules/KB-6F6165CA.md) | Category CRUD & cascade-delete integrity | P1-data | 0 | no | no |
| [`BL-CAT-010`](rules/KB-57A286A4.md) | Catalog link-permission enforcement (RBAC) | P1-data | 0 | no | no |
| [`BL-CAT-011`](rules/KB-661760AB.md) | Cross-catalog move cascades CatalogId to owned entities, not linked | P1-data | 0 | no | no |
| [`BL-CAT-012`](rules/KB-DDA0DAE6.md) | Category dictionary-value & metadata management | P2-ux | 0 | no | no |

## BL-CHK — 8

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-CHK-001`](rules/KB-96EC4E8E.md) | Guest vs authenticated checkout | P0-revenue | 0 | no | no |
| [`BL-CHK-002`](rules/KB-99AC7281.md) | Double-submit prevention (Place Order idempotency) | P0-revenue | 0 | no | no |
| [`BL-CHK-003`](rules/KB-7045B967.md) | Address validation by country | P1-data | 0 | no | no |
| [`BL-CHK-004`](rules/KB-44C7F852.md) | Payment retry after decline | P0-revenue | 0 | no | no |
| [`BL-CHK-005`](rules/KB-CA40289C.md) | Shipping method depends on address | P1-data | 0 | no | no |
| [`BL-CHK-006`](rules/KB-63659AC3.md) | Order total formula | P0-revenue | 0 | no | no |
| [`BL-CHK-007`](rules/KB-657B2F54.md) | Minimum order amount enforcement | P0-revenue | 0 | no | no |
| [`BL-CHK-008`](rules/KB-3E25464F.md) | Address-popup State/Province facet renders only when result set contains regionId values | P1-data | 0 | no | no |

## BL-CR — 9

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-CR-001`](rules/KB-BF9D0424.md) | New review defaults to unmoderated "New" status, never auto-approved | P1-data | 0 | no | no |
| [`BL-CR-008`](rules/KB-CCF500F3.md) | Anonymous visitors cannot access the review-submission control | P1-data | 0 | no | no |
| [`BL-CR-009`](rules/KB-3C9E18B9.md) | Leave-feedback eligibility is a three-part AND | P2-ux | 0 | no | no |
| [`BL-CR-010`](rules/KB-B96C8E4F.md) | The storefront review surface exposes only Approved reviews | P1-data | 0 | no | no |
| [`BL-CR-012`](rules/KB-FB80DC6A.md) | Admin review search filters by status, entity type, rating range, and keyword | P2-ux | 0 | no | no |
| [`BL-CR-013`](rules/KB-720C7D42.md) | Moderation actions transition review status and take effect immediately | P1-data | 0 | no | no |
| [`BL-CR-016`](rules/KB-460D6586.md) | Product rating aggregates only Approved reviews and recomputes on status change | P1-data | 0 | no | no |
| [`BL-CR-017`](rules/KB-639A4566.md) | Product reviews are a store-scoped, toggleable feature | P1-data | 0 | no | no |
| [`BL-CR-018`](rules/KB-ECD247CD.md) | Every admin review-moderation action is gated by its own named permission | P0-security | 0 | no | no |

## BL-CROSS — 12

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-CROSS-001`](rules/KB-AC1CC576.md) | Price list deletion → storefront unavailability | P0-revenue | 0 | no | no |
| [`BL-CROSS-002`](rules/KB-1E428341.md) | Catalog change → search lag → cart price mismatch window | P0-revenue | 0 | no | no |
| [`BL-CROSS-003`](rules/KB-E4368E42.md) | Module disable → API 404, Admin section removal, dependent degradation | P1-data | 0 | no | no |
| [`BL-CROSS-004`](rules/KB-2DC8B393.md) | Currency switch triggers multi-system recalculation | P0-revenue | 0 | no | no |
| [`BL-CROSS-005`](rules/KB-117D86EF.md) | Order placement triggers multi-system side effects | P0-revenue | 0 | no | no |
| [`BL-CROSS-006`](rules/KB-0C220B7F.md) | Feature flag toggle → immediate behavior change | P1-data | 0 | no | no |
| [`BL-CROSS-007`](rules/KB-01813C60.md) | Admin entity deletion → cascade cleanup | P1-data | 0 | no | no |
| [`BL-CROSS-008`](rules/KB-8D53D686.md) | Organization switch → full context swap | P0-revenue | 0 | no | no |
| [`BL-CROSS-009`](rules/KB-DCE51A2F.md) | Eventual consistency is bounded | P1-data | 0 | no | no |
| [`BL-CROSS-010`](rules/KB-84200737.md) | Idempotency on all checkout mutations | P0-revenue | 0 | no | no |
| [`BL-CROSS-011`](rules/KB-CD13F8FB.md) | Graceful degradation when dependent service is down | P1-data | 0 | no | no |
| [`BL-CROSS-012`](rules/KB-23687E78.md) | Admin entity deletion never creates $0 products | P0-revenue | 0 | no | no |

## BL-GQL — 4

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-GQL-001`](rules/KB-585C0342.md) | GraphQL error contract | P1-data | 0 | no | no |
| [`BL-GQL-002`](rules/KB-AF977827.md) | GraphQL query performance thresholds | P2-ux | 0 | no | no |
| [`BL-GQL-003`](rules/KB-E511F7C2.md) | GraphQL response data integrity | P1-data | 0 | no | no |
| [`BL-GQL-004`](rules/KB-7476468F.md) | GraphQL resolver auth gating | P0-security | 0 | no | no |

## BL-IMPEX — 4

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-IMPEX-001`](rules/KB-FEB9A87F.md) | CSV import is idempotent | P1-data | 0 | no | no |
| [`BL-IMPEX-002`](rules/KB-BE0CAC7A.md) | Export matches admin grid filters | P1-data | 0 | no | no |
| [`BL-IMPEX-003`](rules/KB-983E195B.md) | Large import does not timeout silently | P1-data | 0 | no | no |
| [`BL-IMPEX-004`](rules/KB-E0973183.md) | Import validates data integrity before commit | P1-data | 0 | no | no |

## BL-LOY — 19

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-LOY-001`](rules/KB-937D0E3A.md) | Mixed Cart — promotion/coupon evaluation scoped to primary-currency lines only | P0-revenue | 0 | no | no |
| [`BL-LOY-002`](rules/KB-7D92A872.md) | Mixed Cart — `addItem(itemCurrencyCode)` pins the line currency; no cross-currency merge | P1-data | 0 | no | no |
| [`BL-LOY-003`](rules/KB-3BEC0AF1.md) | Mixed Cart — `cartTotals` exposes one entry per distinct line currency | P1-data | 0 | no | no |
| [`BL-LOY-004`](rules/KB-056FA89A.md) | Mixed Cart — loyalty lines excluded from the CART-LEVEL promotion context even when selected for checkout | P0-revenue | 0 | no | no |
| [`BL-LOY-005`](rules/KB-9ACD7DDC.md) | Mixed Cart — a loyalty-currency line shows no "earn points" indicator | P2-ux | 0 | no | no |
| [`BL-LOY-006`](rules/KB-8C4C18AE.md) | Mixed Cart — currency switch converts primary lines, preserves loyalty lines | P1-data | 0 | no | no |
| [`BL-LOY-007`](rules/KB-7BD8A834.md) | Mixed Cart order — points earned and redeemed exactly once, dedup per operation type | P0-revenue | 0 | no | no |
| [`BL-LOY-008`](rules/KB-E0FFF335.md) | Insufficient loyalty balance blocks order creation with a typed `LOYALTY_INSUFFICIENT_BALANCE` error | P0-revenue | 0 | no | no |
| [`BL-LOY-009`](rules/KB-CB434645.md) | Mixed Cart earn — only cash-currency lines earn points; loyalty-currency lines earn zero | P1-data | 0 | no | no |
| [`BL-LOY-010`](rules/KB-ADCFB593.md) | Mixed Cart — a points-only cart is rejected; at least one cash line is required | P1-data | 0 | no | no |
| [`BL-LOY-012`](rules/KB-3A3B568C.md) | The loyalty payment gateway is only valid in Payment Method mode | P1-data | 0 | no | no |
| [`BL-LOY-013`](rules/KB-A3921997.md) | Mixed Cart order — `order.orderTotals` exposes one entry per distinct line currency | P1-data | 0 | no | no |
| [`BL-LOY-014`](rules/KB-AFF2E7D1.md) | Mixed Cart order — Admin SPA Line items blade shows per-currency totals independently | P2-ux | 0 | no | no |
| [`BL-LOY-015`](rules/KB-F442A0C6.md) | A subsystem that settles many entities per event MUST expose per-entity attribution of the settlement | P0-revenue | 0 | no | no |
| [`BL-LOY-016`](rules/KB-BA86F4EB.md) | A mission goal measures the ORDER TOTAL — shipping and tax included, net of discount | P0-revenue | 0 | no | no |
| [`BL-LOY-017`](rules/KB-A8C8D498.md) | Mission accrual counts cash-currency spend only — loyalty-currency lines contribute nothing | P0-revenue | 0 | no | no |
| [`BL-LOY-018`](rules/KB-2B7AF3FA.md) | A mission grants at most once PER PROGRESS OWNER, and an order contributes at most once | P0-revenue | 0 | no | no |
| [`BL-LOY-019`](rules/KB-04CF02A4.md) | A cancelled order's mission contribution and its granted reward MUST be reversed | P0-revenue | 0 | no | no |
| [`BL-LOY-020`](rules/KB-8DEE8C5B.md) | The store's loyalty balance calculation mode selects the OWNER SCOPE of the balance; no surface may resolve a different one | P0-revenue | 0 | no | no |

## BL-NOTIF — 7

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-NOTIF-001`](rules/KB-C772609B.md) | Order confirmation email sent exactly once | P1-data | 0 | no | no |
| [`BL-NOTIF-002`](rules/KB-9476D6D4.md) | Email content matches order data | P1-data | 0 | no | no |
| [`BL-NOTIF-003`](rules/KB-35731294.md) | Notification failure does not block order | P0-revenue | 0 | no | no |
| [`BL-NOTIF-004`](rules/KB-D3E27898.md) | An admin Save must be observable through the API | P1-data | 0 | no | no |
| [`BL-NOTIF-005`](rules/KB-5F638C96.md) | Editing a shipped predefined template warns before it replaces the default, and stays restorable | P1-data | 0 | no | no |
| [`BL-NOTIF-006`](rules/KB-D4BC0550.md) | A code editor must not lose user content to a single undo | P1-data | 0 | no | no |
| [`BL-NOTIF-007`](rules/KB-9748CEFF.md) | A user-input error in a template must not surface as a server fault | P2-ux | 0 | no | no |

## BL-ORD — 10

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-ORD-001`](rules/KB-D4D4DC1B.md) | Order state machine guards | P0-revenue | 0 | no | no |
| [`BL-ORD-002`](rules/KB-C963BFF2.md) | Cancellation restores inventory conditionally | P1-data | 0 | no | no |
| [`BL-ORD-003`](rules/KB-0446051B.md) | Partial fulfillment rules | P1-data | 0 | no | no |
| [`BL-ORD-004`](rules/KB-784902AD.md) | Refund conditions | P0-revenue | 0 | no | no |
| [`BL-ORD-005`](rules/KB-C559CD24.md) | Order number format and uniqueness | P1-data | 0 | no | no |
| [`BL-ORD-006`](rules/KB-54BB544B.md) | Payment state machine (detailed) | P0-revenue | 0 | no | no |
| [`BL-ORD-007`](rules/KB-9D9AFB7A.md) | Shipment state machine (detailed) | P1-data | 0 | no | no |
| [`BL-ORD-008`](rules/KB-97404ABA.md) | Audit trail completeness | P1-data | 0 | no | no |
| [`BL-ORD-009`](rules/KB-5EC6F68D.md) | Order status vocabulary | P1-data | 0 | no | no |
| [`BL-ORD-010`](rules/KB-EBC9FE62.md) | Order totals — one entry per distinct line currency, unique default-currency flag | P1-data | 0 | no | no |

## BL-PAY — 3

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-PAY-001`](rules/KB-210A024C.md) | Client-side card validation gates order submission | P0-revenue | 0 | no | no |
| [`BL-PAY-003`](rules/KB-D7BD6FF7.md) | Successful card payment creates a paid order with a recorded transaction | P0-revenue | 0 | no | no |
| [`BL-PAY-004`](rules/KB-6077C78F.md) | AllowCartPayment renders the card form inline on /cart in single-step checkout only; multistep checkout redirects to the payment page | P0-revenue | 0 | no | no |

## BL-PLAT — 3

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-PLAT-001`](rules/KB-CE835C55.md) | Role and permission changes take effect immediately; deleting an assigned role is safe | P1-data | 0 | no | no |
| [`BL-PLAT-002`](rules/KB-E7EC859D.md) | Account lock or deletion is authoritative over every credential and session tied to it | P1-data | 0 | no | no |
| [`BL-PLAT-004`](rules/KB-2F7E5406.md) | A dynamic property's value type governs which capabilities it may declare | P2-ux | 0 | no | no |

## BL-PRICE — 9

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-PRICE-001`](rules/KB-69EC6E89.md) | Discount stacking order | P0-revenue | 0 | no | no |
| [`BL-PRICE-002`](rules/KB-F530EA1F.md) | Tax calculation position | P0-revenue | 0 | no | no |
| [`BL-PRICE-003`](rules/KB-4C5316B9.md) | Price rounding | P0-revenue | 0 | no | no |
| [`BL-PRICE-004`](rules/KB-45864656.md) | Tiered/volume pricing boundaries | P0-revenue | 0 | no | no |
| [`BL-PRICE-005`](rules/KB-2450A505.md) | Currency-specific price lists | P0-revenue | 0 | no | no |
| [`BL-PRICE-006`](rules/KB-1C4F8F5E.md) | Price list deletion behavior | P1-data | 0 | no | no |
| [`BL-PRICE-007`](rules/KB-0A16FC51.md) | Organization-specific (contract) pricing | P0-revenue | 0 | no | no |
| [`BL-PRICE-008`](rules/KB-AA8545F4.md) | No floating-point money arithmetic | P0-revenue | 0 | no | no |
| [`BL-PRICE-009`](rules/KB-DDCF990E.md) | `discountPercent` is a 4-decimal fraction, rounded away-from-zero, independent of the money rounding policy | P2-ux | 0 | no | no |

## BL-PROFILE — 1

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-PROFILE-001`](rules/KB-A94ADBF9.md) | Silent duplicate-skip on `updateMemberAddresses` and matching `checkDuplicateAddress` detection | P1-data | 0 | no | no |

## BL-SEO — 4

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-SEO-001`](rules/KB-DED21333.md) | Slug uniqueness enforced | P1-data | 0 | no | no |
| [`BL-SEO-002`](rules/KB-CEEFAEF4.md) | Deleted product returns proper HTTP status | P2-ux | 0 | no | no |
| [`BL-SEO-003`](rules/KB-47A5CF56.md) | Canonical URL set on all pages | P2-ux | 0 | no | no |
| [`BL-SEO-004`](rules/KB-C137EBBF.md) | SEO link type controls URL format | P1-data | 0 | no | no |

## BL-SHIP — 4

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-SHIP-001`](rules/KB-9D01A540.md) | Ship-to address determines available methods | P0-revenue | 0 | no | no |
| [`BL-SHIP-002`](rules/KB-4626CDE8.md) | BOPIS requires store pickup location | P1-data | 0 | no | no |
| [`BL-SHIP-003`](rules/KB-806C0235.md) | Free shipping threshold recalculates on cart change | P0-revenue | 0 | no | no |
| [`BL-SHIP-004`](rules/KB-C6F6B354.md) | Shipping method selection persists through checkout edits | P1-data | 0 | no | no |

## BL-SR — 32

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-SR-001`](rules/KB-37F93EEF.md) | Statistics periods are inclusive UTC instants, no server truncation; omitted bounds → all-time | P1-data | 0 | no | no |
| [`BL-SR-002`](rules/KB-F03D8439.md) | Statistics are creator + membership scoped — no cross-rep / unserved-org leak | P0-security | 0 | no | no |
| [`BL-SR-003`](rules/KB-E6015617.md) | Comparison returns the delta; `*ChangePercent` is NULL when the previous baseline is 0 | P1-data | 0 | no | no |
| [`BL-SR-004`](rules/KB-310EDF3F.md) | Money resolves to one currency (`currencyCode` → store default → platform primary) and echoes it | P1-data | 0 | no | no |
| [`BL-SR-005`](rules/KB-0A1EE3DC.md) | Statistics scope excludes flag-cancelled / prototype orders unconditionally | P1-data | 0 | no | no |
| [`BL-SR-006`](rules/KB-85FA9A0B.md) | Cart statistics are currency-scoped; item quantity is the shipped primary metric | P1-data | 0 | no | no |
| [`BL-SR-007`](rules/KB-F3B80F74.md) | Customer counts — `assignedCustomers` is a period-independent scalar; period counts never exceed it | P1-data | 0 | no | no |
| [`BL-SR-008`](rules/KB-9AF70DCE.md) | Top sellers ranked by named sort over a period; `take` clamps at 10 (never errors); rows are a line-item snapshot | P1-data | 0 | no | no |
| [`BL-SR-009`](rules/KB-74880CE9.md) | One named filter rule per axis; omit → baseline; unknown name fails CLOSED (no data, no error) | P1-data | 0 | no | no |
| [`BL-SR-010`](rules/KB-5D402811.md) | One named sort rule per axis; unknown name → default ordering; unsupported direction → ERROR; `customerSalesReps` exempt | P1-data | 0 | no | no |
| [`BL-SR-011`](rules/KB-B28DC677.md) | Sales-rep storefront UI requires permission + module enabled; org membership is gated per-route, not uniformly | P0-security | 0 | no | no |
| [`BL-SR-012`](rules/KB-472DE655.md) | Filter-aware empty states distinguish "no data" from "nothing matched the filter/search" | P2-ux | 0 | no | no |
| [`BL-SR-013`](rules/KB-1A912F29.md) | Rep-facing status / money / rule vocabulary localizes by `cultureName`; raw enum/key never surfaces | P2-ux | 0 | no | no |
| [`BL-SR-014`](rules/KB-A2DAE212.md) | Embedded Sales Rep Admin app gates on customer-member + platform-security permissions, not on `sales-rep:access` | P1-data | 0 | no | no |
| [`BL-SR-015`](rules/KB-520CEAF1.md) | Configurable layout is keyed by rep + surface + optional store; a never-saved key resolves null; per-user isolation | P1-data | 0 | no | no |
| [`BL-SR-016`](rules/KB-B0F19D42.md) | `saveSalesRepLayout` is a full-document replace, never a merge | P1-data | 0 | no | no |
| [`BL-SR-017`](rules/KB-CCC1A552.md) | Persisted block order and `hidden` are independent, verbatim round-trip fields | P2-ux | 0 | no | no |
| [`BL-SR-018`](rules/KB-9F0A872D.md) | Save mutation echoes the persisted document, including a fresh UTC `modifiedDate` | P2-ux | 0 | no | no |
| [`BL-SR-019`](rules/KB-0153CC01.md) | `SalesRepLayoutSetting.value` (`AnyValue`) preserves its scalar JSON type across the round trip | P2-ux | 0 | no | no |
| [`BL-SR-020`](rules/KB-CEDE7DA9.md) | `scope` and `region.id` are free-form strings, not enums; an unrecognized value fails silently to a different (empty) document | P1-data | 0 | no | no |
| [`BL-SR-021`](rules/KB-44E8C279.md) | Both layout operations require an authenticated caller | P0-security | 0 | no | no |
| [`BL-SR-022`](rules/KB-E5708F9B.md) | Required layout-input fields are schema-enforced; a non-scalar setting `value` is rejected | P1-data | 0 | no | no |
| [`BL-SR-023`](rules/KB-6540D03C.md) | The customer-profile layout is scope-wide — one document per rep, not per customer | P1-data | 0 | no | no |
| [`BL-SR-024`](rules/KB-6DE6DDA8.md) | Configurable-layout changes persist only on explicit Save, are scoped to the rep's account, and survive reload and re-authentication | P1-data | 0 | no | no |
| [`BL-SR-025`](rules/KB-DFEA31B8.md) | The block registry owns structure and region placement; the saved document owns only order and hidden, with unknown types dropped and missing blocks appended | P1-data | 0 | no | no |
| [`BL-SR-026`](rules/KB-3F78BD8E.md) | A layout key that was never saved (`null`) is not a failure — registry defaults render with editing enabled | P1-data | 0 | no | no |
| [`BL-SR-027`](rules/KB-980B9B7B.md) | The two widget columns are structurally separate drag groups — cross-column drag is impossible in either direction | P2-ux | 0 | no | no |
| [`BL-SR-028`](rules/KB-25F7AAB1.md) | Stat cards park/restore by drag or keyboard; widgets hide via a dismiss control and restore only from a hidden-items tray | P2-ux | 0 | no | no |
| [`BL-SR-029`](rules/KB-941DD6BA.md) | Keyboard grab-and-move announces every transition via `aria-live`, with position for reorder and without position for park/restore; however a grab ended by a pointer interruption is a tracked violation | P2-ux | 0 | no | no |
| [`BL-SR-030`](rules/KB-B7D8FA00.md) | A save already in flight cannot be duplicated by a rapid repeat trigger | P2-ux | 0 | no | no |
| [`BL-SR-031`](rules/KB-B7EC4779.md) | A hidden widget's data query does not fire; a hidden stat card's page-level statistics query still fires unchanged | P2-ux | 0 | no | no |
| [`BL-SR-032`](rules/KB-0B0601A9.md) | The rail region mounts only while it holds at least one visible block, and unmounts structurally (not just visually) when empty | P2-ux | 0 | no | no |

## BL-SRCH — 5

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-SRCH-001`](rules/KB-FE9D9996.md) | Facet counts match filtered results | P1-data | 0 | no | no |
| [`BL-SRCH-002`](rules/KB-EAD716B0.md) | Zero-result query shows an intact empty state | P2-ux | 0 | no | no |
| [`BL-SRCH-003`](rules/KB-4C9DDC26.md) | Search index consistency after catalog change | P1-data | 0 | no | no |
| [`BL-SRCH-004`](rules/KB-D501C59D.md) | Search respects store and catalog scope | P1-data | 0 | no | no |
| [`BL-SRCH-005`](rules/KB-B689B818.md) | Special characters in search queries | P2-ux | 0 | no | no |

## BL-STORE — 1

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-STORE-001`](rules/KB-7535C57F.md) | Store configuration is independent per store, keyed by store id | P1-data | 0 | no | no |

## BL-UI — 7

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-UI-001`](rules/KB-1F495B55.md) | Layout stability on initial render | P2-ux | 0 | no | no |
| [`BL-UI-002`](rules/KB-0024D60E.md) | Spacing grid compliance | P2-ux | 0 | no | no |
| [`BL-UI-003`](rules/KB-E2C5A66A.md) | No state-induced layout shift | P2-ux | 0 | no | no |
| [`BL-UI-004`](rules/KB-0232E3B5.md) | Content boundary | P2-ux | 0 | no | no |
| [`BL-UI-005`](rules/KB-72341A73.md) | Alignment in horizontal groups | P2-ux | 0 | no | no |
| [`BL-UI-006`](rules/KB-817A3144.md) | Touch target size and spacing | P1-data | 0 | no | no |
| [`BL-UI-007`](rules/KB-2F710C2D.md) | Admin editor chrome is keyboard-operable and exposes its state | P1-data | 0 | no | no |

## BL-WL — 6

| id | rule | severity | confirmations | attested | disputed |
|---|---|---|---|---|---|
| [`BL-WL-001`](rules/KB-7EC91419.md) | Branding is org-context & post-auth; no enabled config → platform defaults | P2-ux | 0 | no | no |
| [`BL-WL-002`](rules/KB-CBD11DE1.md) | Org & store settings merge per-field, org-preferred (NOT whole-object override) | P1-data | 0 | no | no |
| [`BL-WL-003`](rules/KB-9F08A399.md) | Two enable layers — store master switch (storefront) vs per-record IsEnabled (xAPI) | P1-data | 0 | no | no |
| [`BL-WL-004`](rules/KB-30B4AB91.md) | Link lists resolve by name; missing → empty array, no error; footer legacy fallback | P2-ux | 0 | no | no |
| [`BL-WL-005`](rules/KB-22B2C306.md) | A WL setting binds to exactly one of Store XOR Organization | P2-ux | 0 | no | no |
| [`BL-WL-006`](rules/KB-4D7A8B0F.md) | Distinct allowed upload types — logo vs favicon | P2-ux | 0 | no | no |
