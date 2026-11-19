---
id: KB-3C3BFDC2
subject: "On the home page of an organization with its own white-labeling, two Builder.io blocks keep a fixed light background in dark mode while their headings take the inverted theme colour: 1.16:1 and 1.1:1"
plane: experiential
question: Are the headings of the Builder.io home page blocks readable in storefront dark mode for an organization with its own white-labeling?
status: active
appliesTo:
  - axis: component
    value: builder-block
  - axis: scheme
    value: dark
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: GET /api/v3/content/page
evidence:
  - method: observation
    deployment: vcst_qa
    build: 694f1bf7279cdd13
    conditions: theme=2.63.0-pr-2676-6b5e-6b5e8717; store=B2B-store; browser=firefox; role=organization member
    defect: suspected
    at: 2026-11-19T18:05:06.259Z
    by: session:51ac2fe3
    who: Lenajava1
---
On vcst_qa (theme 2.63.0-pr-2676, store B2B-store, Firefox, 1920 px), signed in as a member of an organization with its own white-labeling and with the Theme toggle set to dark (html.dark), the home page (route /, title "Main page", content from GET cdn.builder.io /api/v3/content/page with userAttributes urlPath=/ and the organizationId) renders the 24 px heading "Gifts for sweetheart. Sale" as rgb(238,233,250) on a builder-block with background rgb(253,252,249): 1.16:1, and the h2 "Discounts. Loyalty cards" as rgb(238,233,250) on rgb(225,225,225): 1.1:1 (large text needs 3:1). The heading colour is the dark-scheme value of a theme neutral token (--color-additional-950 resolves to #eee9f4 in dark) while the block background is a literal colour authored in the page content, so it does not flip. In light the same headings are near-black on the same background and readable. Same class as the page-builder slider block with a token background and literal white text: one side of the pair is a token that inverts, the other is a fixed colour.
