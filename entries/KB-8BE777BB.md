---
id: KB-8BE777BB
subject: Switching organization from the storefront account menu BLANKS the header account control instead of relabelling it; the new org name only appears after a full page reload.
plane: experiential
question: After a multi-org buyer switches organization from the account menu, does the header show the new organization name?
questions:
  - text: After I switch to another company in the account menu, why does the top bar no longer say which company I'm buying for?
  - text: Does a multi-organization buyer's header button show the new organization name straight after switching, or only after reload?
  - text: Why does the account menu button lose its text and accessible label after a client-side cart or organization update?
  - text: How does the header organization slot differ between a buyer in one organization and a buyer in several?
  - text: Should an end-to-end case reload the page before asserting the header shows the organization chosen via changeOrganization?
concepts:
  - id: multi-organization
  - id: site-header
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
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T12:21:33.732Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    contradicts: true
    note: "On vcptcore_qa (theme 2.59.0-pr-2458) the header was relabelled immediately when a multi-org sales rep switched org from the account menu while on the home page (Purple Pink123) or on /company/my-customers/{id} (Watermelon123): \"Org / Alla Volkova\" plus the logo label rendered right after the switch reload. The blank header happened only when the switch reload landed on a product page (3 of 3)."
---
On virtostart (storefront Ver. 2.58.0, store B2B-store) a buyer belonging to several organizations opened the account menu, which renders an "Organizations" section with a type-ahead search plus a listbox of the member organizations (the current one carrying the selected state; typing narrows the list correctly). Before the switch the header account button read "Franklin Electric / TestAndrew TestCook". After choosing a different organization from that listbox the context DID switch correctly — the per-organization white-label theme swapped (a different store logo and product-page layout rendered) and the cart badge cleared — but the header account button lost all of its text and rendered as a bare chevron, with an accessibility tree showing button "Account menu" containing only an image and no accessible label. It stayed blank across in-app navigation. A full page reload restored it, correctly reading "Gordon Electric / TestAndrew TestCook". The same blanking was observed right after a plain add-to-cart, so the trigger is a client-side state update generally, not the organization switch specifically. Consequence: between the switch and the next full load, nothing on screen tells the buyer which organization they are ordering as, on the one control whose job that is. A case asserting "header shows the new org name after switching" will fail unless it reloads first. Note the header org-name SLOT also differs by membership count: a multi-org buyer gets "OrgName / UserName" inside the account button, while a single-org buyer gets the org name in a separate slot beside the store logo and only their own name in the account button.
