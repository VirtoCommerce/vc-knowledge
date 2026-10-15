---
id: KB-85F9B1ED
subject: Markup-tracked Page Builder product blocks push one view_item_list per block when rendered in the DOM, including blocks far below the fold, with no scroll
plane: experiential
question: Do Page Builder products, products-carousel and favorite-products blocks send view_item_list on render or only when scrolled into view, and with which list ids?
status: active
appliesTo:
  - axis: element
    value: google-analytics
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /agent-test-ga4-product-lists
  - coordinate: window.dataLayer
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2543-0127-01276486; platform=3.1078.0
    at: 2026-10-13T13:53:37.114Z
    by: session:aa76d86f
    who: Lenajava1
    build: 942f354b4df8bb56
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: ca098b1f8643fb95
    conditions: theme=2.60.0-pr-2543-fdab-fdabd62d; browser=chromium
    at: 2026-10-14T09:28:00.566Z
    by: session:b0e90290
    bodyHash: 1c5d70dc4841
    who: Lenajava1
    note: "Held on fix build fdabd62 at 390/800/1920 cold loads: 5 view_item_list (products_block 4 + 3, products_carousel titled 5 / untitled 4 with no event-level item_list_name, favorite_products 4) = each block's full product set; titled carousel sends all 5 though 4 slides per view. Resize cycle and browser Back re-entry: still 1 per block per page view. Empty favorite-products block on /agent-test-ga4-html-product-lists: 0. Signed-in home 'Discounts. Loyalty cards ' trailing space still there. In EUR the blocks whose products have no EUR price render 0 cards and send nothing."
---
Guest, Chrome 1920 and 390, cold load of a Page Builder page holding a products block, a titled and an untitled products-carousel, a favorite-products block, a predefined-product-list (top ~6300 px) and an untitled carousel (top ~7200 px): window.dataLayer received exactly five view_item_list events with scrollY still 0 - products_block (one per products / predefined-product-list block, separate events, own items), products_carousel (titled carries item_list_name; untitled omits the key entirely), favorite_products. items[] equals the rendered cards of that block (each product once, including carousel slides not in view). demo-product-list and product-info blocks pushed nothing. A favorite-products block whose filter matches nothing pushed nothing. Card image-carousel arrows, block carousel arrows, Add/Remove to Compare and the quantity stepper pushed no select_item; a card title click pushed exactly one select_item with the block's list id. The home page products block (guest 'Daily Deals', signed-in 'Discounts. Loyalty cards ') behaves the same; the signed-in list name carries the CMS title's trailing space verbatim.
