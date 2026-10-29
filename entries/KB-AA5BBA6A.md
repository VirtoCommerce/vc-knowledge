---
id: KB-AA5BBA6A
subject: The storefront has no organization search page; the only org search is the text box inside the org switcher, and picking a result switches the active organization
plane: experiential
question: Is there an organization search page with industry / location filters on the B2B storefront, or only the org switcher search?
questions:
  - text: Where can a multi-organization user search organizations on the storefront?
  - text: Does selecting an organization in the storefront org search open an organization detail page?
status: active
appliesTo:
  - axis: store
    value: b2b-store
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/dashboard
evidence:
  - method: observation
    deployment: vcst_qa
    build: 33c85ff2161bc1fc
    conditions: platform=3.1090.0; theme=vc-theme-b2b-vue-2.61.0-alpha.2605; role=Organization maintainer
    at: 2026-10-29T07:55:50.502Z
    by: session:ce3cfc2a
    who: Aleksandra-Mitricheva
---
On vcst (theme 2.61.0-alpha.2605), signed in as a contact linked to 12 organizations: no standalone organization-search page and no industry or location filters exist. The only organization search is the text box in the account menu's organization switcher; typing filters the org list, and selecting a result switches the active organization (no detail page opens). Suite 005 SRCH-009/SRCH-010, which assume an org-search page with industry/location filters, cannot run as written.
