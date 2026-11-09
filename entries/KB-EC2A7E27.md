---
id: KB-EC2A7E27
subject: When the session dies on /company/info, the Company name watcher throws TypeError (reading 'name') once organization goes null; the error reaches only App Insights, and theme PR 2647 removes it
plane: experiential
question: Does /company/info throw a browser exception when the session expires, and how is it observed?
questions:
  - text: What App Insights exception does a dead session on the company info page produce?
  - text: Is the 'reading name' TypeError on CompanyInfo fixed?
  - text: Is the Company name field cleared before the sign-in redirect?
concepts:
  - id: organization
status: active
appliesTo:
  - axis: role
    value: organization-maintainer
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/info
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7477bc6a09fab490
    conditions: theme=2.62.0-pr-2620; role=Organization maintainer; store=B2B-store
    defect: VCST-6732
    at: 2026-11-09T08:38:00.813Z
    by: session:3888f60e
    who: Lenajava1
---
The setup: an organization maintainer on /company/info, with app.config.errorHandler wrapped in-page, because the App Insights Vue plugin owns it and the console never shows these errors. I set auth.expires_at in the past and auth.refresh_token to an invalid value, then switched the addresses table page. POST /connect/token returned 400 invalid_grant, me re-fetched as Anonymous, and the user's organization went null. Theme 2.62.0-pr-2620 then threw 'Cannot read properties of null (reading 'name')' from a watcher getter (Vue runtime-2) on 2 of 2 runs. App Insights buffered an Exception item with typeName TypeError and operation CompanyInfo within 1 ms of it. browser_console_messages showed only the /connect/token 400. Theme 2.62.0-pr-2647 (vc-frontend PR 2647: getter reads organization.value?.name, reset only when truthy) produced no such error on 2 of 2 runs, checked three ways: the wrapped handler, a sessionStorage mirror, and the App Insights buffer. On both builds the Company name input kept the org name until the page navigated away (20 ms sampling). C7, C5 and C6 on PR 2647 were clean: paging the addresses table with a valid session, renaming the company, and Logout.
