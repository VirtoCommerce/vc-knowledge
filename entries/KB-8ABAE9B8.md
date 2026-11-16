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
    deployment: local
    at: 2026-10-09T13:29:53.143Z
    by: session:eb40dd6d
    who: Aleksandra-Mitricheva
    conditions: local=vcfrontend_pr2522_on_vcst_qa
  - method: observation
    deployment: vcst_qa
    build: b0d1ac6487d06555
    conditions: theme=vc-frontend-next-3.3.0-pr-23-7be3; browser=firefox; session=guest; viewport=1920x1080
    at: 2026-11-16T12:47:37.344Z
    by: session:4cdfee7c
    bodyHash: c5ce2891326e
    who: Lenajava1
    contradicts: true
    note: "On a vc-frontend-next PR build (3.3.0-pr-23-7be3) on /accessories/aliexpress/phones-and-accessories/holders-stands, removing the \"Remove: Holders & Stands\" chip by mouse click AND by keyboard (Shift+Tab from the input to the chip, Enter) left focus on the search input (placeholder back to \"Search products and parts\"), not BODY. Behaviour changed on this build (AC-29 of the release)."
---
On a vc-frontend 2.60 PR build, on a category page the chip "Remove: <category>" button is reachable by Tab (12 presses from page top on /printers). Activating it by mouse click or by Enter clears the scope (placeholder returns to "Search") but document.activeElement becomes BODY, not the search input or any element inside .search-bar, so a keyboard user loses their place.
