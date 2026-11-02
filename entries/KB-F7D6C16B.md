---
id: KB-F7D6C16B
subject: Storefront footer column titles (.footer-links__title) compute text-transform uppercase in design classic and none in design soft (vc-frontend PR 2600)
plane: experiential
question: Are the storefront footer column titles uppercase, and does that depend on the design?
questions:
  - text: What text-transform do footer link-list headers have on the storefront?
  - text: Does the soft design change the footer column title case?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 1920
anchors:
  - coordinate: /cart
  - coordinate: /account/orders/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    build: 16bf801dd4d62d5a
    conditions: theme=2.62.0-pr-2600-a4f2-a4f29534; design=classic; soft-comparison=local production build of the same sha
    at: 2026-11-02T08:33:17.508Z
    by: session:a1013b77
    who: Lenajava1
---
At 1920 px, every visible footer column title span.footer-links__title (7 columns: Account details, About us, Popular categories, Online resources, External links, Customer support, Brands) computes text-transform uppercase, Lato 700 14px, on vcst_qa (theme 2.62.0-pr-2600, design classic), on the home page and /cart. The wrapper span.footer-links__header is text-transform none. On a production build of the same PR with design soft, proxied to the same backend, the same 7 titles compute text-transform none in Inter, on the home page and an order details page. The case of the titles comes from the design, not from the footer component.
