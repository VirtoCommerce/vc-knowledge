---
id: KB-E7DF9B84
subject: the storefront footer renders top-level link-list entries as section headers and only their children as links
plane: experiential
question: Why do white-label footer links render as non-clickable text on the storefront?
questions:
  - text: Why are the links at the bottom of the store just plain text that I cannot click?
  - text: How must a white-label footer link list be structured so its entries render as clickable links?
  - text: Why does the mobile footer accordion open to nothing for a flat link list?
  - text: How are child link lists nested under a footer header, and is the name matching scoped per list or global?
concepts:
  - id: footer
  - id: white-labeling
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.whiteLabelingSettings
  - coordinate: client-app/shared/layout/components/footer/_internal/footer-links.vue
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:04.972Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: theme=2.63.0; platform=3.1101.0; store=B2B-store
    at: 2026-11-27T13:36:10.744Z
    by: session:d2ee4088
    bodyHash: 2316f563bd59
    who: Lenajava1
    note: "2026-11-27, org member with a two-level footer list (Help, Legal with two children each): desktop headers are plain text, not focusable; the children are the only footer links in the WL columns (keyboard order Warranty, Support, Terms of Service, Privacy Policy). At 375px the headers are accordion buttons (280x40) that reveal their children."
  - method: observation
    deployment: vcst_qa
    build: 56f07d69d90abb81
    conditions: theme=2.63.0; platform=3.1101.0; store=B2B-store
    at: 2026-11-27T14:09:07.486Z
    by: session:d2ee4088
    bodyHash: 2316f563bd59
    who: Lenajava1
    note: "Re-seen 2026-11-27 (REG-2026-11-27-1452-WLV2): desktop headers Help/Legal are SPANs, not focusable; children Warranty, Support, Terms of Service, Privacy Policy are the only WL footer links; at 375px headers are 280x40 accordion buttons."
---
The storefront footer is two-level: a top-level link-list entry renders as a bold, non-clickable section header (on mobile, the accordion toggle), and only its childItems render as clickable links. A flat footer link list - top-level entries with no children - therefore renders as empty header columns, plain text on desktop and an accordion that reveals nothing on mobile. That is a content-structure issue, not a product defect. Child lists are nested under a header by link-list name, the same mechanism as the main menu, and that name matching is global across lists.
