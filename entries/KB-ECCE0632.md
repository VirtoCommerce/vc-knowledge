---
id: KB-ECCE0632
subject: a configurable Text section's maxLength binds custom text only; a preset option label bypasses it
plane: experiential
question: Does a configurable product Text section maxLength reject a preset option whose label is longer than the limit on addItem?
questions:
  - text: Can I pick a ready-made engraving text that is longer than the character limit on a custom item?
  - text: Is a Text section's maximum length enforced on preset options or only on free text typed by the shopper?
  - text: How does the server tell a preset option from custom text when validating a configured cart line?
  - text: Why does updating a configured item with too-long custom text appear to do nothing?
  - text: Can two preset options with identical labels in one text section be distinguished?
concepts:
  - id: configured-line-item
  - id: cart-validation
status: active
appliesTo:
  - axis: surface
    value: xapi
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: CartConfigurationItemType.customText
  - coordinate: Mutation.changeCartConfiguredItem
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:14:21.238Z
    by: session:memimpor
    who: Lenajava1
---
For a configurable product Text section, maxLength is enforced on the Custom input path (free text the shopper types), both client-side and on the server. A preset option whose label is longer than maxLength is still accepted: the storefront sends the preset as customText equal to the preset label, and the server bypasses maxLength when customText matches a preset option's text; add and update of a configured line apply the same validation. Because presets are matched by label rather than by option id, two presets in one section with identical text cannot be told apart. When the server rejects an over-length custom value on update, the cart-level validation error is not surfaced as a toast, so the update looks like a silent no-op.
