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
---
Personal account, /cart, Shipping, edit Shipping address -> Select address popup with 2 addresses. Typed 'Los Angeles' + Enter: GetCurrentUserAddresses keyword 'Los Angeles' -> totalCount 1, list shows 1 row and a Clear (x) button. Then Ctrl+A + Backspace in the input: the input is empty, the Clear (x) button disappears, the list still shows only the 1 row and NO GraphQL request is sent. Pressing Enter on the empty input sends keyword '' and restores both rows. Source (vc-frontend select-address-filter.vue, identical at 2.61.0 and dev): the keyword is applied only on @clear, @keyup.enter and the magnifier click; VcInput renders Clear only while the model is non-empty. VCST-4578 fixed only the Clear (x) click path; keyboard deletion was never handled. Present since 2.48.0 (VCST-4710 wired the shared filter into the checkout popup).
