---
id: KB-A3A2CBEF
subject: Password sign-in as a sales rep with zero organizations intermittently fails with a 'Something went wrong' toast (GetMe errors on me.contact.organizationId) while GA still records a successful login; a repeat sign-in succeeds
plane: experiential
question: Does signing in on /sign-in as a sales rep with no organization ever fail, and what is sent to GA when it does?
questions:
  - text: Why does a zero-customer sales rep get 'Something went wrong' after signing in?
  - text: Is a failed storefront sign-in reported to GA4 as login success=true?
  - text: GetMe INVALID_OPERATION me.contact.organizationId after /connect/token 200
status: active
appliesTo:
  - axis: feature
    value: sign-in
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: Query.me
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: platform=3.1085.0; theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; role=Sales Representative; store=B2B-store
    defect: suspected
    at: 2026-10-20T00:32:28.955Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
---
Seen once in two attempts on vcptcore_qa. Right after signing out the rep Priya Rao, a password sign-in as the zero-customer rep (agent-test-sr-nocustomers, contact with 0 organizations) got /connect/token 200, but the GetMe query returned errors [INVALID_OPERATION "Error trying to resolve field 'organizationId'", path me.contact.organizationId]. The page showed a "Something went wrong" toast and stayed on the form, yet GA4 still received en=login with ep.success=true for that attempt. Clicking Sign in again succeeded. A controlled repeat of the same sequence (sign out Priya → sign in the zero-customer rep) in another session went clean: GetMe returned contact.organizationId null and organizations.totalCount 0 with no errors, landing on /account/dashboard and sending one login success=true from the landing page. The trigger is not isolated; carry-over of the previous user's organization context is a guess, not established.
