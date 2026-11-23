---
id: KB-A26A8ACC
subject: "After theme PR 2667 a multi-org buyer's desktop account button reads \"<current org> / <user name>\" and the Organizations control is named \"Organizations: <current org>\"; a single-accessible-org buyer gets the user name only and no Organizations control"
plane: experiential
question: What text do the header account button and the Organizations button carry for a multi-organization buyer, and what does a buyer with Approved+Invited memberships see?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: user
    value: multi-organization-member
anchors:
  - coordinate: Query.me.contact.organizations
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7f9888707d648724
    conditions: theme=2.63.0-pr-2667-0444-0444048a; store=B2B-store
    at: 2026-11-23T19:03:22.149Z
    by: session:7ce2b8e4
    who: Lenajava1
    note: "distinct: Covers the account-button and organizations-button texts, the 2-org panel without search, and the single-accessible-org state; KB-3F1B8798 only covers the /403 page and the account popup."
---
On vcst (theme 2.63.0-pr-2667, en, desktop 1920 wide, Firefox lane) a two-organization buyer: button 'Account menu' (data-test-id account-button) holds spans data-test-id organization-name-label (title = org name), a '/', and customer-name-label (full name); button data-test-id organizations-button has accessible name 'Organizations: <org name>' (visible caption 'Organizations'); after switching the org both texts changed to the new org, user name unchanged. Panel with 2 orgs has no search field; with 11 it has a combobox 'Search organizations' and a scrolling listbox of option rows, current org first and selected. Buyer Approved in A and Invited in B: account button shows the user name only, no Organizations control, the org name sits beside the logo; Approved + locked B: control shown, B option disabled. GraphQL totalCount: Invited 1, Rejected 1, Deleted 1, Approved+locked 2.
