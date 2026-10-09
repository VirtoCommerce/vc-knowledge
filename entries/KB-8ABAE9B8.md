---
id: KB-8ABAE9B8
subject: Removing the header search scope chip (mouse or keyboard Enter) drops document focus to BODY
plane: experiential
question: Where does keyboard focus go after the search-bar category scope chip is removed?
status: active
appliesTo:
  - axis: component
    value: header-search-bar
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: desktop
anchors:
  - coordinate: /printers
evidence:
  - method: observation
    deployment: local_vcfrontend_pr2522_on_vcst_qa
    at: 2026-10-09T13:29:53.143Z
    by: session:eb40dd6d
    who: Aleksandra-Mitricheva
---
On a vc-frontend 2.60 PR build, on a category page the chip "Remove: <category>" button is reachable by Tab (12 presses from page top on /printers). Activating it by mouse click or by Enter clears the scope (placeholder returns to "Search") but document.activeElement becomes BODY, not the search input or any element inside .search-bar, so a keyboard user loses their place.
