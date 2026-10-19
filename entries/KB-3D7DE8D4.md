---
id: KB-3D7DE8D4
subject: Password sign-in on the storefront sends no GA4 login hit; the page reloads before it is delivered, and identity user properties are only applied on the reloaded page
plane: experiential
question: Does the storefront GA4 login event reach Google Analytics with the member's organization_id after a password sign-in?
status: active
appliesTo:
  - axis: feature
    value: google-analytics-user-properties
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /sign-in
  - coordinate: /company/activities
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-07T18:36:55.390Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T02:06:25.317Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    note: "Re-observed for 2 multi-org members on 2026-10-08: no en=login hit after /sign-in; first hit of the reloaded page carries up.* for the member's last-selected organization (the sign-in form offers no organization choice)."
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T02:32:56.401Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    note: "Re-observed 2026-10-08 on 2.59.0-pr-2458: 21 /sign-in password sign-ins (6 accounts, 4 single-org + 2 multi-org, 3 each) captured with a context-level request listener that also records requests issued by an unloading page; 0 of 21 issued an en=login hit. Each sign-in shows form_start on /sign-in, then the next page load's _s=1 hit carries the member's contact_id/organization_id/session_kind=self/is_sales_rep=false."
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-08T12:21:33.802Z
    by: session:b9db3f52
    who: Aleksandra-Mitricheva
    note: "Advanced sales rep password sign-in: no en=login hit observed. Up.* first appeared on the _s=1 page_view of the reloaded home page."
    build: f960736e1109b3de
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.GoogleEcommerceAnalytics=4.1005.0-pr-61-5267; store=B2B-store
    at: 2026-10-19T17:46:53.533Z
    by: session:68b36f2b
    bodyHash: feff567e2d64
    who: Aleksandra-Mitricheva
    contradicts: true
    contested: theme 2.61.0-pr-2458-3f0b-3f0b7284 vs 2.59.0-pr-2458-8f42-8f42bcd0; virtocommerce.fileexperienceapi 3.1008.0 vs 3.1006.0; virtocommerce.profileexperienceapimodule 3.1021.0 vs 3.1020.0; virtocommerce.xapi 3.1027.0 vs 3.1026.0; virtocommerce.xcart 3.1042.0 vs 3.1039.0; …
    note: "On theme 2.61.0-pr-2458-3f0b, 6 successful /sign-in password sign-ins (1 sales rep, 5 members): each sent exactly one en=login ep.success=true ep.method=password hit from the page landed on after the reload (_s=2, right after that page's _s=1 page_view, which carried the member's own up.contact_id / organization_id / session_kind=self / is_sales_rep). No organization_name user property was sent on any hit. A wrong password sent one en=login ep.success=false hit from /sign-in."
---
Signed in 5 different accounts (4 org members, 1 sales rep) through the /sign-in form on a vc-frontend 2.59 build carrying theme PR 2458 (GA user properties). Captured every GA4 collect request across the navigation: the sign-in page sent page_view, scroll, form_start (no up.* params), then the app navigated (openReturnUrl sets location.href) and the next page sent page_view carrying up.contact_id/organization_id/organization_name/is_sales_rep/session_kind. No en=login hit was sent in any of the 5 runs. In source, sign-in-form.vue calls analytics('login') only after signIn() has already triggered the location.href navigation, and user properties are still the anonymous set at that moment. Consequence: GA-derived login counts for an organization stay at 0 / are never attributed to its organization_id. Separately observed: gtag sends up.* only on the first hit (_s=1) of each page load; later hits on the same page (search, view_search_results, view_item batched in POST bodies) carry no up.* on the wire.
