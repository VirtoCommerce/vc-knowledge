---
id: KB-BE1C0B7F
subject: BL-CART-011 Unmatched section key in batch selection is a silent no-op
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:01.767Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CART-011: Unmatched section key in batch selection is a silent no-op `[P1-data]`

- **Rule:** When `selectCartConfigurationItems` or `unSelectCartConfigurationItems` receives a `configurationSections[]` list where one or more keys do not match any `ConfigurationItem` on the target lineItem, those unmatched keys MUST be silently skipped. The mutation MUST still process all matched keys, MUST return HTTP 200, and MUST return `errors[]` as empty. Unmatched keys leave no trace in the response.
- **Verify:** Obtain a configurable lineItem with 2 known section keys (A and B). Call `selectCartConfigurationItems` with `configurationSections: [keyA, {sectionId: 'does-not-exist', type: 'Product'}]`. Assert HTTP 200 and `errors[]` is empty. Assert `configurationItems[keyA].selectedForCheckout = true` (matched item flipped). Assert no error entry or warning appears for the unmatched key.
- **Violation signal:** HTTP 4xx or `errors[]` non-empty due to unmatched section key; entire batch aborted and no items flipped.
- **Agents:** qa-backend-expert (xAPI batch mutations)
- **Source:** vc-module-x-cart PR #114 §Validation errors; `CartAggregateTests` "unmatched section no-op" unit test.
