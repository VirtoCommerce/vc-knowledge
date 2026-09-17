---
id: KB-5EC6F68D
subject: BL-ORD-009 Order status vocabulary
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:04.219Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-ORD-009: Order status vocabulary `[P1-data]`

- **Rule:** The order status vocabulary is an **admin-editable, localizable dictionary** (`Order.Status` setting, `IsDictionary = true`, `IsLocalizable = true`) — **not** a fixed enum. Every value in the dictionary is **settable** via the Admin Order → Status dropdown (the dropdown is populated from the dictionary), and a deployment may add, rename, or remove values. The platform ships a default seed of settable values — `New`, `Not payed`, `Pending`, `Processing`, `Ready to send`, `Cancelled`, `Partially sent`, `Completed` — which deployments commonly customize (e.g. `Payment required`, `Ready for pickup`, `Custom`). The exact list and count are therefore environment-configurable; the invariant is the dictionary mechanism, not a fixed set or count.
- **`Processing` is a normal settable dictionary value — NOT read-only/computed.** It is one of the seeded `Order.Status` values and is selectable from the Status dropdown. It also serves as the default `Order.InitialProcessingStatus` (the status auto-assigned when order processing begins — mirroring `Order.InitialStatus`, default `New`, at creation), but auto-assignment does not make it read-only: an admin can set it manually and it persists.
- **System value vs display label:** The dictionary stores system values (e.g. `ReadyForPickup`), while Admin UI and storefront render localized labels (`Ready for pickup`). Tests must assert against the correct surface — see `project_order_status_vocab` memory.
- **Storefront labels may differ:** Storefront applies user-facing relabeling on top of platform status (e.g. admin `Pending` + shipment `Send` → storefront "Shipped"). Do not assume 1:1 label mapping between admin and storefront.
- **Verify:** Open Admin → Orders → any order → the Status dropdown lists the deployment's configured `Order.Status` dictionary values, with `Processing` among the selectable options. Set an `AGENT-TEST-` order to `Processing` → Save → reopen → status persists as `Processing` in the editable Status control. Confirm the settable set matches Settings → Orders → General settings → order status dictionary.
- **Violation signal:** Status dropdown does not reflect the `Order.Status` dictionary; a configured dictionary value is missing from the dropdown; a saved value does not persist; storefront shows the raw system value (`ReadyForPickup`) instead of the localized label.
- **Agents:** qa-backend-expert (order API + `Order.Status` dictionary setting), qa-testing-expert (Admin SPA Status dropdown, storefront order history labels)
- **Source:** vc-module-order `ModuleConstants.cs` — `CustomerOrderStatus` + `Settings.General.OrderStatus` (`IsDictionary=true`, `AllowedValues` = the 8 seed values incl. `Processing`) + `OrderInitialStatus` (default `New`) / `OrderInitialProcessingStatus` (default `Processing`). Docs: PlatformUserGuide "Order management → Settings → General settings" (order statuses are admin-configurable). Live-verified: an order persists in `Processing`, shown in the editable Status control.
- **Amended:** 2026-07-22 (auto-applied, triangulated — BL-AUDIT-2026-07-22: corrected the stale "Processing is read-only / exactly 7 settable" claim — `Processing` is a settable dictionary value and the set is an env-configurable dictionary, not a fixed enum; 3/3 docs+source+live).
