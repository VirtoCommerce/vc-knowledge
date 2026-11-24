---
id: KB-B64A3F9D
subject: With a storefront build that has the Site Builder flag off, the Vue designer shows 'This storefront does not support the new Site Builder designer.' instead of the preview, and the section tree has no HEADER/FOOTER groups
plane: experiential
question: What does the Site Builder designer show when the store's storefront is a build without the Site Builder flag?
questions:
  - text: Why does the Site Builder designer preview not load on a flag-off storefront?
  - text: What message does the designer show when the storefront lacks Site Builder support?
  - text: Does the Site Builder designer work against a storefront built with the Site Builder flag off?
status: active
appliesTo:
  - axis: surface
    value: admin-designer
anchors:
  - coordinate: /apps/site-builder-shell
  - coordinate: GET /api/site-builder/catalog
evidence:
  - method: observation
    deployment: vcst_qa
    build: 54b87331b2bf6e07
    conditions: module:VirtoCommerce.SiteBuilder=3.1000.0-pr-1-644f; theme=2.63.0-pr-2630-ecaf-ecafc40d (flag off) vs 2.63.0-pr-2676-f03c-f03c41eb (flag on); store=B2B-store
    at: 2026-11-24T14:22:27.997Z
    by: session:bd155cea
    who: Lenajava1
---
B2B-store, storefront theme 2.63.0-pr-2630-ecaf (Site Builder flag off at build time): opening a page in /apps/site-builder-shell showed 'Loading the site…' and then, about 15 s later, an icon with 'This storefront does not support the new Site Builder designer.' in the preview area. The Sections tab listed only Page settings and the page's own sections (no HEADER, TEMPLATE or FOOTER groups), and GET /api/site-builder/catalog?storeId=B2B-store answered with no catalog. After the storefront was repinned to the flag-on build 2.63.0-pr-2676-f03c and the lane's browser cache cleared, the same page loaded the preview, the HEADER/TEMPLATE/FOOTER groups and the catalog. The flag is set at storefront build time, so the message is the cue to deploy the flag-on build rather than a module fault.
