---
id: KB-B8AE01A5
subject: A multi-org contact that never had an active organization is auto-placed in one of its organizations on first storefront sign-in; no organization selection prompt is shown
plane: experiential
question: What happens on the first storefront password sign-in of a multi-organization contact that has no current organization yet - is an organization picker shown?
questions:
  - text: Does the storefront ask a multi-org buyer to choose an organization after sign-in?
  - text: Which organization is active after first sign-in when contact.currentOrganizationId is empty?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_regression
    build: 9648699210e92bd7
    conditions: platform=3.1076.13; theme=2.59.0; store=B2B-store
    at: 2026-11-09T15:28:08.217Z
    by: session:cb5a6bb9
    who: Aleksandra-Mitricheva
---
Two fresh AGENT-TEST contacts were created over REST with 11 and 10 organizations in contact.organizations and never signed in before. On the first /sign-in password login each landed directly on the home page with an organization already active in the account button (11-org contact: its first listed membership, alphabetically first too; 10-org contact: a different, not-alphabetically-first one). No modal, prompt or selection step appeared; the account menu then offered the switcher as usual. So a 'select organization on sign-in' step does not exist on this build.
