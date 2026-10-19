---
id: KB-21EE2EB6
subject: Storefront sign-out does not reset GA4 user properties; anonymous hits after logout reuse the same GA client and session and carry no up.* override
plane: experiential
question: After a member signs out of the storefront, are the GA4 organization_id / contact_id / session_kind user properties cleared for that browser?
status: active
appliesTo:
  - axis: feature
    value: google-analytics-user-properties
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: /search
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T02:32:56.504Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T12:21:33.779Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    note: After Logout, page_view _s=1 and an anonymous header search reused the same cid and sid with no uid and no up.*. On B2B-store the anonymous search redirects to /sign-in, but the search event is still sent.
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.GoogleEcommerceAnalytics=4.1005.0-pr-61-5267; store=B2B-store
    at: 2026-10-19T17:46:52.989Z
    by: session:68b36f2b
    bodyHash: 4667b828e37d
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.61.0-pr-2458-3f0b-3f0b7284 vs 2.59.0-pr-2458-8f42-8f42bcd0; virtocommerce.fileexperienceapi 3.1008.0 vs 3.1006.0; virtocommerce.profileexperienceapimodule 3.1021.0 vs 3.1020.0; virtocommerce.xapi 3.1027.0 vs 3.1026.0; virtocommerce.xcart 3.1042.0 vs 3.1039.0; …
    note: "On theme 2.61.0-pr-2458-3f0b, 5 member sign-outs (Logout in the account menu) on B2B-store: the reloaded page's first hit (_s=1 page_view) carried up.contact_id='' up.organization_id='' up.is_sales_rep=false up.session_kind=anonymous and no uid; the following header search event (redirected to /sign-in) carried the typed search_term on that same anonymous page. The cid and sid stayed the same as during the signed-in session. So sign-out now overrides the identity user properties with an anonymous set; it no longer leaves them unset."
---
On a vc-frontend 2.59 build (theme PR 2458), after Logout from the account menu the following hits (page_view on the next load, and a header search event typed while signed out) went out with uid empty and no up.* parameters at all: no empty or anonymous values were sent to overwrite contact_id / organization_id / organization_name / session_kind / is_sales_rep. The _ga client id and GA session id stayed the same as during the signed-in session. GA4 keeps the last user-property values for a client until they are set again, so signed-out activity on that browser is probably still reported under the previous member's organization with session_kind=self. That attribution is not yet confirmed in GA reports; the wire behaviour is confirmed.
