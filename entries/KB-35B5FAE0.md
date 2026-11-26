---
id: KB-35B5FAE0
subject: A vc-products tag in a Site Builder custom section shows each storefront viewer their own prices and follows sign-in and sign-out without a cache clear
plane: experiential
question: Does a vc-products tag inside a Site Builder custom section show the signed-in buyer's own prices, and does it drop them after sign-out?
questions:
  - text: Can the custom section renderer's cache leak one buyer's prices to the next viewer?
  - text: Which prices does a custom section's product list show to a guest versus an organization buyer?
  - text: After a buyer logs out, does a custom section still show contract prices?
status: active
appliesTo:
  - axis: module
    value: site-builder
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.searchProducts
  - coordinate: POST /graphql
evidence:
  - method: observation
    deployment: vcst_dev
    build: 1b6e861ce41cd6cb
    conditions: platform=3.1100.0; module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-6088; theme=2.63.0-pr-2698-faeb-faeba500; store=B2B-store; role=organization buyer in a VIP-priced user group
    at: 2026-11-26T13:57:11.037Z
    by: session:c8a6829d
    who: ivan-kalachikov
---
B2B-store, one browser tab. A published custom section type holding <vc-products filter="code:21A915,38DJ66,5ZU94" take="3"> on a page. As guest the section showed 38DJ66 $57.25 and 5ZU94 $5.40, equal to the guest PDPs. After signing in as a buyer whose contact is in a user group with its own price list, a reload showed $9.99 / $9.99, equal to that buyer's PDPs, and it stayed so after navigating away and back. After Logout, and again after a normal reload, the section showed $57.25 / $5.40; no buyer price remained. The section's own SearchProducts request carries the viewer's Authorization and userId (a new anonymous userId after sign-out), and the renderer's LRU cache holds compiled templates and sanitized rich text only, not product data. The variation product 21A915 showed "From $9.99" to both viewers, which is the separate minVariationPrice defect VCST-7037, not the section.
