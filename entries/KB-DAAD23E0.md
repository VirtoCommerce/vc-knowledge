---
id: KB-DAAD23E0
subject: In the PDP Configuration checklist a filled optional Text section reads "{name} (completed)" with no value, a Product section reads "{name} — {product name} (completed)", and whitespace-only text in an optional Text section counts as completed and is sent to the cart as-is
plane: experiential
question: What does the Configuration checklist row show for a filled optional Text section, and how is whitespace-only text in an optional Text section treated?
questions:
  - text: Does the configuration checklist show the entered text value for a text section?
  - text: Is an optional text section with only spaces marked completed in the configuration checklist?
  - text: What customText does addItem send for an optional text section filled with spaces?
status: active
appliesTo:
  - axis: feature
    value: product-configuration-checklist
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /products-with-options/cfg-parents/agent-test-config-gift-box
  - coordinate: Mutation.addItem
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a (vc-frontend-next, merges vc-frontend 2.60.0); store=B2B-store; browser=firefox-1920
    at: 2026-10-20T13:59:27.380Z
    by: session:d992339e
    who: Lenajava1
---
B2B member, B2B-store, PDP of a configurable product with one optional Text section (Gift Message, maxLength 100). Empty: row "Gift Message — optional" + "Review" link. After typing text: row "Gift Message (completed)" — section name only, no value, no link. On another configurable product, Product sections read "Color — Green hat" / "Base — <product name> (completed)". Replacing the text with three spaces kept the row "Gift Message (completed)"; Add to cart sent AddItem with configurationSections[{type:Text, customText:"   "}], and the cart line's Components list shows a row "1. Gift Message:" with an empty value. A cold reload of the PDP with ?lineItemId restores "   " (3 / 100) and the completed row. Contrast: a REQUIRED Text section with whitespace stays required (KB-79949FFE).
