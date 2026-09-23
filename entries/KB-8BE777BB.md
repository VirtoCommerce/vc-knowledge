---
id: KB-8BE777BB
subject: Switching organization from the storefront account menu BLANKS the header account control instead of relabelling it; the new org name only appears after a full page reload.
plane: experiential
question: After a multi-org buyer switches organization from the account menu, does the header show the new organization name?
status: active
appliesTo:
  - axis: actor
    value: multi-org-buyer
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/members
  - coordinate: Query.me
  - coordinate: Mutation.changeOrganization
evidence:
  - method: observation
    deployment: virtostart
    at: 2026-09-23T10:13:24.367Z
    by: session:cdb27d99
    who: Dan-BV
---
On virtostart (storefront Ver. 2.58.0, store B2B-store) a buyer belonging to several organizations opened the account menu, which renders an "Organizations" section with a type-ahead search plus a listbox of the member organizations (the current one carrying the selected state; typing narrows the list correctly). Before the switch the header account button read "Franklin Electric / TestAndrew TestCook". After choosing a different organization from that listbox the context DID switch correctly — the per-organization white-label theme swapped (a different store logo and product-page layout rendered) and the cart badge cleared — but the header account button lost all of its text and rendered as a bare chevron, with an accessibility tree showing button "Account menu" containing only an image and no accessible label. It stayed blank across in-app navigation. A full page reload restored it, correctly reading "Gordon Electric / TestAndrew TestCook". The same blanking was observed right after a plain add-to-cart, so the trigger is a client-side state update generally, not the organization switch specifically. Consequence: between the switch and the next full load, nothing on screen tells the buyer which organization they are ordering as, on the one control whose job that is. A case asserting "header shows the new org name after switching" will fail unless it reloads first. Note the header org-name SLOT also differs by membership count: a multi-org buyer gets "OrgName / UserName" inside the account button, while a single-org buyer gets the org name in a separate slot beside the store logo and only their own name in the account button.
