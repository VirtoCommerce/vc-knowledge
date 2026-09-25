---
id: KB-E7DF9B84
subject: the storefront footer renders top-level link-list entries as section headers and only their children as links
plane: experiential
question: Why do white-label footer links render as non-clickable text on the storefront?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.whiteLabelingSettings
  - coordinate: client-app/shared/layout/components/footer/_internal/footer-links.vue
evidence:
  - method: observation
    deployment: vcst-qa
    at: 2026-09-25T11:16:04.972Z
    by: session:memimpor
    who: Lenajava1
---
The storefront footer is two-level: a top-level link-list entry renders as a bold, non-clickable section header (on mobile, the accordion toggle), and only its childItems render as clickable links. A flat footer link list - top-level entries with no children - therefore renders as empty header columns, plain text on desktop and an accordion that reveals nothing on mobile. That is a content-structure issue, not a product defect. Child lists are nested under a header by link-list name, the same mechanism as the main menu, and that name matching is global across lists.
