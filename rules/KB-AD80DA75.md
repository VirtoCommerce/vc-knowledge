---
id: KB-AD80DA75
subject: BL-AUTH-004 Returning vs new customer defaults
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:05.007Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-004: Returning vs new customer defaults `[P2-ux]`

- **Rule:** Shipping-address pre-fill at checkout is **config-gated and off by default.** The store's shipping-address policy setting has exactly two values — a disabled value (the **default**) and a previous-order value. Only under the previous-order value does checkout pre-fill the shipping address, and it then copies the **shipping (or billing-and-shipping) address of the customer's most recent order** — not the customer's saved address book — and only when the cart's shipment has **no** delivery address yet. Under the disabled value a returning customer sees **no** pre-filled shipping address, exactly like a first-time customer, and must select one; that is by design, not a defect. A new customer (first order) always sees empty address forms and no saved payment methods. Saved payment methods are a separate per-customer store of tokenized cards, offered only by card processors that support saving, and are **not** governed by the shipping-address policy. In every configuration the system must not show addresses or payment methods belonging to another account, even if the email was reused across organizations.
- **Verify:** Read the store's shipping-address policy setting first — the expected result depends on it. Disabled value (default) → sign in as a customer **with** prior orders → checkout → the shipping section prompts for an address selection and nothing is pre-filled. Previous-order value → same customer → checkout → the shipping address matches the most recent order's shipping address; then set a delivery address on the shipment first and re-enter checkout → the existing address is **not** overwritten. Any value → brand-new account with no orders → empty address form, no saved cards. Sign in as a different user → no cross-contamination of addresses.
- **Violation signal:** Checkout pre-fills a shipping address while the policy is at its disabled value, or fails to pre-fill while it is at the previous-order value; pre-fill overwrites a delivery address already chosen on the shipment; pre-fill draws from the saved address book rather than the most recent order; a brand-new customer sees pre-filled data; addresses or saved cards from another account are displayed. NOTE: a returning customer seeing an empty address form under the **default** (disabled) policy is **expected** — do not file it.
- **Agents:** qa-frontend-expert (checkout forms), qa-backend-expert (store settings, cart shipment context)
- **Source:** `vc-module-x-order` `src/VirtoCommerce.XOrder.Core/ModuleConstants.cs` (`ShippingAddressPolicy` descriptor — two allowed values, default = the disabled one) + `src/VirtoCommerce.XOrder.Data/Middlewares/ShipmentContextMiddleware.cs` (`GetShippingPolicy` plus the early return unless the policy is the previous-order value, the `shipment?.DeliveryAddress != null` short-circuit, and the single-result last-order lookup taking the billing-and-shipping/shipping address). Docs: storefront user guide, Checkout → Shipping — "If Shipping address policy is enabled in the Platform, the shipping address on the Shipping page will be prefilled with the most recently used address." Live-confirmed on the environment: the policy read back as its disabled value, and a signed-in customer's checkout showed no pre-filled shipping address while a customer with no order history saw an empty form. The previous-order branch is source+docs-grounded only — it was not exercised live (it requires the store setting to be changed).
- **Amended:** 2026-08-05 (auto-applied, triangulated — BL-AUDIT-2026-08-05). Corrected the unconditional "returning customer sees pre-filled saved addresses" claim: pre-fill is gated by the store's shipping-address policy (default **off**), sources the **most recent order's** address rather than the saved address book, and skips a shipment that already carries a delivery address. The cross-account isolation clause is unchanged.
