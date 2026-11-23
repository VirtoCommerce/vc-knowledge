---
id: KB-7DE1F1A5
subject: The header Organizations panel search is a wrapper div (data-test-id organizations-search) around an input whose placeholder and aria-label are both "Search organizations", and it renders only for buyers with more than 10 organizations
plane: experiential
question: What are the placeholder and the DOM shape of the search field in the storefront header Organizations panel, and when does it render?
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
    conditions: theme=2.63.0-pr-2667-0444-0444048a; store=B2B-store
    at: 2026-11-23T13:49:12.347Z
    by: session:7ce2b8e4
    who: Lenajava1
---
On vcst (theme 2.63.0-pr-2667, en) a 31-org buyer opened the panel from organizations-button: data-test-id organizations-search is a DIV, its input has placeholder "Search organizations" and aria-label "Search organizations", a "Clear" button appears once text is typed. A 2-org buyer's panel has organizations-list but no organizations-search; organizations-empty-list is absent unless a search returns nothing. Escape with focus inside or outside the open panel closes it (aria-expanded=false); from inside, focus returns to organizations-button.
