---
id: KB-BCF37E85
subject: Org switcher search (me.contact.organizations searchPhrase) also matches an organization's description, so orgs whose NAME lacks the typed text can be listed
plane: experiential
question: Why does the storefront org switcher search list organizations whose name does not contain the typed text?
questions:
  - text: Does me.contact.organizations searchPhrase match only the organization name?
  - text: Which fields does the org switcher search filter on?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.me.contact.organizations
  - coordinate: POST /api/members/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: theme=2.62.0-alpha.2647; store=B2B-store
    at: 2026-11-12T09:36:04.099Z
    by: session:9c36bcd2
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcst_qa
    build: 7f9888707d648724
    conditions: theme=2.63.0-pr-2667-0444-0444048a; store=B2B-store
    at: 2026-11-23T18:33:53.958Z
    by: session:7ce2b8e4
    bodyHash: 1b8c3d0be41a
    who: Lenajava1
    note: "Header Organizations panel (theme 2.63.0-pr-2667): typing 'ACM' listed the 3 ACME Store orgs plus '50% Off Redwood Provisions' and the pinned current org; filtering happens while typing, Enter changes nothing."
---
Signed in as a contact with 12 organizations, opened the account-menu org switcher and typed 'ACM'. The list showed the three 'ACME Store' orgs plus two orgs whose names do not contain 'ACM' ('(parentheses) Lone Star Outfitters', '50% Off Redwood Provisions'). Admin POST /api/members/search with keyword 'ACM' returns the same orgs; their description starts with 'ACME ...'. The GetOrganizations request carries searchPhrase quoted, first:30, statuses:[Approved]; the search goes through the Member index keyword, which covers description as well as name. The active org is always listed regardless of the term. Name-only expectations in switcher-search cases must pick a token absent from org descriptions.
