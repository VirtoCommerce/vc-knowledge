---
id: KB-8CDB8E6B
subject: A Sales Rep task dialog left open when the session is revoked stays open over /sign-in, and its Save keeps sending the mutation anonymously
plane: experiential
question: What happens to an open Sales Rep Schedule call dialog when the rep's storefront session is revoked and the app routes to /sign-in?
questions:
  - text: Does the Schedule call modal close when the storefront signs the rep out?
  - text: What does Save in the task dialog do after the access token was revoked?
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
  - coordinate: /sign-in
  - coordinate: Mutation.createSalesRepTask
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: f1db890a6a93d133
    conditions: module:VirtoCommerce.SalesRep=3.1016.0-pr-26-92d4; module:VirtoCommerce.Customer=3.1029.0; theme=2.61.0-pr-2576-be2c; browser=firefox; store=B2B-store; role=Sales Representative
    at: 2026-10-27T19:03:17.658Z
    by: session:e75fe8a4
    who: Aleksandra-Mitricheva
---
Firefox, 1920 px. A Sales Representative opened Schedule call on /company/my-customers/{id} and picked a date; the admin then sent PUT /api/sales-rep removing that organization, which revoked the rep's access token. Save sent Mutation.createSalesRepTask (POST /graphql) and got HTTP 200 errors[0] "Anonymous access denied or access token has expired or is invalid." (code Unauthorized), GetMe returned userName Anonymous, and the app routed to /sign-in?returnUrl=/company/my-customers/{id}. The Schedule call dialog was NOT closed: it stayed on top of the sign-in page with the draft intact and Save enabled, and the page behind it was inert. A second Save sent createSalesRepTask again with the same Unauthorized result. No toast text appeared in either case, nothing was written, and the dialog only went away via Cancel. After signing back in, the organization's page read "Customer not found or not in your customers.".
