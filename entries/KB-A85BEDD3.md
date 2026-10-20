---
id: KB-A85BEDD3
subject: In the Schedule call popup, typing a filter into the Customer combobox writes 'N results available' into that VcSelect's sr-only aria-live polite span, and clears it when the list closes
plane: experiential
question: Does the Customer picker (VcSelect autocomplete) in the Schedule call popup on /company/my-customers/{id} announce how many customers match the typed filter?
questions:
  - text: Does VcSelect autocomplete announce the number of filtered results to screen readers?
  - text: Which live regions exist inside the Schedule call dialog?
  - text: Is the customer search result count in the Schedule call popup announced?
status: active
appliesTo:
  - axis: module
    value: salesrep
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
  - coordinate: Query.salesRepCustomers
evidence:
  - method: observation
    deployment: local
    conditions: platform=3.1083.0; module:VirtoCommerce.SalesRep=3.1015.0-src-pr26-7007756; theme=2.60.0; browser=firefox-151; role=Sales Representative
    at: 2026-10-20T18:27:56.244Z
    by: session:70c763de
    who: yuskithedeveloper
---
Firefox 151, 1440x900, theme 2.60.0 with the SalesRep plugin built from vc-module-sales-rep PR #26, signed in as a Sales Representative serving two sample organizations. In the Schedule call popup the Customer field is a VcSelect with autocomplete, lazy-loaded through Query.salesRepCustomers (operation SalesRepCustomerOptions). Each VcSelect in the dialog (Customer, Time, Priority) renders an empty span.sr-only with aria-live="polite". Clicking Customer and typing 'Con' one key at a time put the text '2 results available' into the Customer span, which a MutationObserver recorded as a childList mutation in that live region. Escape closed the list without closing the dialog and emptied the span again. The message is set only while the list is open and a filter is typed: 'N results available', or 'no results found' when nothing matches (ui_kit.select.* keys in vc-select.vue). Tabbing through the field without typing writes nothing. Nothing was saved. The popup was cancelled.
