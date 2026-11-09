---
id: KB-E2CD66B4
subject: Storefront order detail offers no invoice/packing-slip PDF download; the orders list Invoice column is plain text
plane: experiential
question: Can a buyer download an invoice PDF (or packing slip) from the storefront order pages?
questions:
  - text: Is there a Download Invoice button on /account/orders/{id}?
  - text: Is the Invoice column on /account/orders a link to a PDF?
status: active
appliesTo:
  - axis: feature
    value: orders
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/orders/{id}
  - coordinate: /account/orders
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; theme=2.59.0
    at: 2026-11-09T14:46:59.738Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
---
Signed in as a personal B2B-store buyer on theme 2.59.0, Completed order detail pages (/account/orders/{id}) show only the actions 'Print order' and 'Reorder all' (plus 'Request return' when returnable); there is no 'Download invoice' or packing-slip control anywhere on the page. The /account/orders list has an 'Invoice' column whose cells hold the payment/invoice number (e.g. PI2610xx-00001) as plain text, not a link. Invoice PDF exists only in the Admin SPA order blade.
