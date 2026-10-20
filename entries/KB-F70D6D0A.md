---
id: KB-F70D6D0A
subject: Storefront product lists push one GA4 view_item_list per render and one select_item per click, each with item_list_id and item_list_name; variation rows push no select_item
plane: experiential
question: Which item_list_id / item_list_name do the storefront's product lists send in GA4 view_item_list and select_item, and how many events fire per render?
questions:
  - text: Does clicking a product in Customers bought together push select_item?
  - text: What GA4 list id does the cart Recently browsed block use?
  - text: Does the variations list on a PDP push select_item when a variation is clicked?
  - text: Is view_item_list pushed twice for one category render?
status: active
appliesTo:
  - axis: integration
    value: ga4
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: /product/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; store=B2B-store; ga4=enabled
    at: 2026-10-20T14:14:49.817Z
    by: session:d992339e
    who: Lenajava1
---
Read window.dataLayer on vcst (theme 3.1.0-pr-11-d67b-d67b0e0a, Chromium 1920/1280). Category /printers/all-in-one as a guest: exactly one view_item_list (item_list_id 'category_printers/all-in-one_page_1', item_list_name 'Category "Inkjet Printers" (page 1)', related_type category), and a click on a product title pushed one select_item with the same id and name before view_item. PDP 'Customers bought together': one view_item_list with item_list_id 'recommended_products_bought-together' and name 'Customers bought together <product name>' (related_type product). A click pushed one select_item with the same id and name. PDP of a product with variations (AGENT-TEST-Variation-Stock-Master): one view_item_list with item_list_id 'variations' and name 'Product variations <name>'. Its items include the master itself next to the variation. Clicking the variation row link pushed only view_item on the variation page, no select_item. /cart signed in: one view_item_list with item_list_id 'recently_browsed_products', name 'Recently browsed', after view_cart and begin_checkout, which both fire on simply loading /cart. A click pushed one select_item with the same id and name, and the link opened in a new tab. No list pushed view_item_list twice for one render.
