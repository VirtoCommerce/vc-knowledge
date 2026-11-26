---
id: KB-CC551D3B
subject: "In the /cart Select address popup, deleting a submitted search keyword with the keyboard sends no query: the list stays filtered under an empty input with no Clear (x) until an empty submit"
plane: experiential
question: What happens to the /cart Select address popup list when the user deletes the search keyword with the keyboard instead of pressing Clear (x)?
questions:
  - text: Does emptying the address search box restore the full address list in the checkout popup?
  - text: Why does the address popup stay filtered after I erase the search text?
  - text: Does the Clear button stay visible after deleting the address search keyword?
status: active
appliesTo:
  - axis: component
    value: select-address-modal
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: Query.currentCustomerAddresses
evidence:
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: theme=2.62.0-alpha.2647; platform=3.1097.0; role=personal account; store=B2B-store
    defect: suspected
    at: 2026-11-12T07:49:07.612Z
    by: session:7e403256
    who: kutasinaelena
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; role=personal account; store=B2B-store
    at: 2026-11-26T01:07:49.496Z
    by: session:779b94fc
    bodyHash: 32e89e891c9f
    who: kutasinaelena
    note: "Re-observed on theme 2.63.0-alpha.2681 with 3 personal addresses: keyword 'Houston' + Search -> GetCurrentUserAddresses keyword 'Houston' totalCount 1; Ctrl+A + Backspace left the input empty, no request sent, list still 1 row."
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; role=personal account; store=B2B-store; browser=edge
    at: 2026-11-26T05:44:42.237Z
    by: session:779b94fc
    bodyHash: 32e89e891c9f
    defect: suspected
    who: kutasinaelena
    note: "Re-run 2026-11-26 on edge with 3 personal addresses, 3/3: Houston/Chicago submitted -> GetCurrentUserAddresses keyword sent, 1 row; then Ctrl+A+Backspace (x2) and char-by-char Backspace (x1) left the input empty, no GetCurrentUserAddresses request in 4 s, list still 1 row, Clear control gone. Empty Enter sends keyword '' and restores 3 rows. vc-frontend dev 5d7c54b: select-address-filter.vue L54-65 applies the keyword only on @clear / @keyup.enter / the magnifier."
---
Personal account, /cart, Shipping, edit Shipping address -> Select address popup with 2 addresses. Typed 'Los Angeles' + Enter: GetCurrentUserAddresses keyword 'Los Angeles' -> totalCount 1, list shows 1 row and a Clear (x) button. Then Ctrl+A + Backspace in the input: the input is empty, the Clear (x) button disappears, the list still shows only the 1 row and NO GraphQL request is sent. Pressing Enter on the empty input sends keyword '' and restores both rows. Source (vc-frontend select-address-filter.vue, identical at 2.61.0 and dev): the keyword is applied only on @clear, @keyup.enter and the magnifier click; VcInput renders Clear only while the model is non-empty. VCST-4578 fixed only the Clear (x) click path; keyboard deletion was never handled. Present since 2.48.0 (VCST-4710 wired the shared filter into the checkout popup).
