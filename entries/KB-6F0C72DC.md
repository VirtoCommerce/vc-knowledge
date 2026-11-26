---
id: KB-6F0C72DC
subject: Site Builder designer section tree shows icon-only badges (clock = Not published, circle-x = Not found) at the 260 px panel and icon plus label at the 300 px panel; text contrast 5.4 to 10.8:1 in light and dark
plane: experiential
question: How do the Not published and Not found badges look in the Site Builder designer section tree at 1280 and 1440 px viewports?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/site-builder-shell
  - coordinate: GET /api/site-builder/template
evidence:
  - method: observation
    deployment: vcst_dev
    build: 858d3d28d721a302
    conditions: module:VirtoCommerce.SiteBuilder=3.1003.0-pr-5-01b3; role=ADMIN
    at: 2026-11-26T15:48:15.764Z
    by: session:c8a6829d
    who: ivan-kalachikov
---
On B2B-store, a page draft placing a never-published custom section type and a missing type was opened via Draft pages list > row > Edit in designer (/apps/site-builder-shell/#/site-builder-draft/site-builder-designer/<pageId>). At a 1280 px viewport the tree panel is 259 px: each badge is 24 px, icon only (lucide clock for Not published, circle-x for Not found), the label is display:none, the icon is aria-hidden and the meaning is carried by the title attribute and a tw-sr-only span. At 1440 px the panel is 299 px and the label is shown (badges 101 and 80 px). Measured contrast light: 6.00:1 and 5.40:1; dark: 10.75:1 and 6.57:1. The Discard dialog confirm is vc-button-danger (Discard and delete, 6.27:1 light, 5.76:1 dark) only while the never-published type is ticked, vc-button-primary disabled otherwise. Opening the designer with designPath equal to the page id in the URL gives a 404 on /api/site-builder/template and an empty tree.
