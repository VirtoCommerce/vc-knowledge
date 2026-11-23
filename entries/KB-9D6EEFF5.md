---
id: KB-9D6EEFF5
subject: In the desktop header Organizations panel (theme PR 2667), a search phrase that matches other orgs keeps the current organization pinned first, but a phrase that matches nothing shows an empty list with "No results found" and drops the current organization
plane: experiential
question: Does the header Organizations panel search keep the current organization in the list when the phrase does not match it?
questions:
  - text: Why does the current organization appear in the org switcher search results when its name does not match?
  - text: What does the header Organizations panel show when the search phrase matches no organization?
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
    build: 2b7c46e6924efca5
    conditions: theme=2.63.0-pr-2667-0444-0444048a; platform=3.1099.0; store=B2B-store
    at: 2026-11-23T08:32:25.912Z
    by: session:7ce2b8e4
    who: Lenajava1
    note: "distinct: KB-BCF37E85 is the old account-menu switcher and says the active org is always listed regardless of the term; on the new header panel the current org is listed only when the phrase matches at least one org, and it is pinned without a description match."
---
On vcst (theme 2.63.0-pr-2667-0444-0444048a, de locale) a buyer with 11 organizations opened the header Organizations panel (data-test-id organizations-button, role=dialog) with Bence-and-Family current and typed 'Hillcrest' in the search combobox. The list showed Bence-and-Family (selected, first) plus Hillcrest-Holdings. Admin POST /api/members/search keyword 'Hillcrest' returns only Hillcrest-Holdings and the two orgs share the same description, so Bence-and-Family was not a description match: the current org is pinned by the UI. Typing 'zzqqxx' instead gave an empty listbox and the status 'Keine Ergebnisse gefunden' (No results found) without the current org. Clearing with the x button restored the full 11-org list with focus kept in the search field. The panel's GetOrganizations is sent with first:30, sort name:asc, statuses:[Approved].
