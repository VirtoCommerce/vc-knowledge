---
id: KB-8B9AE2D8
subject: BL-CART-001 Max quantity enforcement
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:00.608Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CART-001: Max quantity enforcement `[P0-revenue]`

- **Rule:** Per-product max quantity is enforced by available stock (or configured min/max). There is no automatic silent cap to available stock on either surface — but the two entry surfaces differ in where the enforcement lands, because the platform backend never refuses to persist an out-of-range quantity; it always accepts the requested value and attaches an advisory per-line validation error. **On the Product Detail Page quantity control** (a product not yet in the cart, or its own stepper), the storefront enforces the limit client-side before any mutation commits: entering a quantity outside the allowed range disables the Increase control, replaces it with an inline range affordance (e.g. "Order from X to Y item(s)"), and no line is added — the cart item count is unchanged. **On an existing cart-line quantity input** (the cart page), the storefront submits whatever quantity is entered via the quantity-change mutation, and the backend persists it as entered — the line's quantity, line total, and cart subtotal/tax/total all reflect the out-of-range value — while attaching a per-line validation message (e.g. "You can order maximum N item(s)") that the storefront renders inline and that keeps "Place order" disabled until the quantity is corrected. This out-of-range state survives a full page reload (it is server-persisted, not merely an optimistic client artifact).
- **Verify:** PDP: for a product not yet in cart, enter a quantity above available stock → assert the inline range message, Increase disabled, and cart item count unchanged (no line committed). Cart page: for an existing line, enter a quantity above available stock into the line's quantity input and submit → assert the persisted quantity equals the entered value (not capped), line total = qty × unit price, cart subtotal reflects it, an inline validation message renders, and "Place order" stays disabled; reload the page → assert the same out-of-range quantity, total, and message persist.
- **Violation signal:** On either surface, the quantity is silently capped to available stock with no error/message; OR the out-of-range quantity is accepted with no validation signal and "Place order" becomes enabled; OR the PDP path lets an over-limit line commit; OR an order is placed for more units than in inventory.
- **Agents:** qa-frontend-expert (PDP stepper + cart-line UI), qa-backend-expert (`addItem`/`changeCartItemQuantity` mutation response, per-line validation)
- **Docs:** N/A — implementation-detail (quantity-reject-vs-auto-cap UX mechanics; VirtoOZ guides do not narrate this, per §1a).
- **Source:** vc-module-x-cart `CartLineItemValidator.ValidateMinMaxQuantity` → `CartErrorDescriber.ProductMinMaxQuantityError`/`ProductMaxQuantityError`/`ProductQtyChangedError` (FluentValidation `AddFailure` — attaches a per-line validation error but does not block the mutation). `AddCartItemCommandHandler.Handle` and `ChangeCartItemQuantityCommandHandler.Handle` (`src/VirtoCommerce.XCart.Data/Commands/`) both call `cartAggregate.AddItemAsync`/`ChangeItemQuantityAsync` with the requested quantity and save **unconditionally**, with no branch on the validator's outcome — the backend never refuses to persist an out-of-range quantity on either mutation. vc-frontend `locales/en.json` `validation_error.PRODUCT_MAX_QTY`/`PRODUCT_MIN_MAX_QTY`/`PRODUCT_QTY_CHANGED` (rendered inline via the error translator).
- **Amended:** 2026-07-27 (auto-applied, triangulated — BL-AUDIT-2026-07-27; DRIFT — corrected: the backend does not reject/refuse persistence on either surface; distinguished the PDP client-side pre-commit block from the cart-line accept-persist-flag-and-block-completion behavior, independently reconfirmed live including across a page reload; source + live agree, both fresh this run; docs N/A per §1a). Supersedes the 2026-07-22 amendment, which attributed a backend "reject" behavior to `CartLineItemValidator`/`CartErrorDescriber.ProductQtyChangedError` — that source anchor is real but only emits an advisory validation error; it does not block a mutation from persisting.
