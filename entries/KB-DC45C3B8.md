---
id: KB-DC45C3B8
subject: On vc-frontend-next 3.3.0-pr-22 every VcInput, VcSelect and VcTextarea field label renders 12px / #5B5147 / 700 in light mode (#C0B7B0 in dark), while standalone VcLabel group captions stay 14px / #312720 / 700
plane: experiential
question: What font size and color do form field labels (input, select, textarea) render with on the vc-frontend-next storefront, and do standalone group captions match them?
questions:
  - text: Do select and textarea labels match input labels in size and color on /account/addresses?
  - text: Why does the checkout Shipping details row mix 14px and 12px labels?
  - text: What color is a field label in dark mode on vc-frontend-next?
status: active
appliesTo:
  - axis: feature
    value: form-field-labels
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend-next
anchors:
  - coordinate: /account/addresses
  - coordinate: /account/profile
  - coordinate: /account/lists
  - coordinate: /account/orders
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: b821ec42ca1ee2e5
    conditions: theme=3.3.0-pr-22-f709-f709d8ef; store=B2B-store; viewport=1920x1080
    at: 2026-11-10T07:30:16.170Z
    by: session:c0597658
    who: Aleksandra-Mitricheva
---
Measured with getComputedStyle at 1920px, three fresh loads each: /account/addresses "New address" modal (10 labels: 8 VcInput + Country/State VcSelect), /account/profile (First/Last name, Email VcInput + Default language/currency VcSelect), /account/lists "Create list" modal (VcInput + Description VcTextarea), /account/orders filter (Created date VcSelect + Start/End date VcInput) all render class vc-label--size--xs, 12px, rgb #5B5147, weight 700; in dark mode (html.dark) the address modal labels are #C0B7B0. Clicking a label focuses its control (VcSelect label opens the listbox). Standalone VcLabel captions use vc-label--size--sm, 14px #312720 700: orders filter "Select order status", cart page "Delivery option", "Shipping address*", "Billing address*" — so the cart's Shipping details row shows a 14px caption beside a 12px "Delivery method*" select label. In the address modal at 1280px City (229px wide) sits beside State/Province in one row; at 1100px all right-column fields stack at one left edge, 420px wide. The previous build 3.2.0-pr-19 rendered the select/textarea labels at 14px #31271F (ticket baseline).
