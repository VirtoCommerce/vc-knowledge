---
id: KB-DBA1110A
subject: The salesRepActivities logins category counts every GA `login` event of a customer session with no success filter, so a failed storefront sign-in (login with success=false) is counted as a login
plane: experiential
question: Does Query.salesRepActivities logins category on POST /graphql/sales-rep count failed storefront sign-ins (GA login event with success=false)?
questions:
  - text: Why does the sales-rep activity feed show more logins than successful sign-ins?
  - text: Is visitsCount in salesRepCustomerActivitySummary inflated by failed sign-ins?
status: active
appliesTo:
  - axis: feature
    value: customer-activity
  - axis: module
    value: sales-rep
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.salesRepActivities
  - coordinate: Query.salesRepCustomerActivitySummary
  - coordinate: POST /graphql/sales-rep
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; module:VirtoCommerce.GoogleEcommerceAnalytics=4.1005.0-pr-61-5267; store=B2B-store; role=Sales Representative
    defect: suspected
    at: 2026-10-20T18:15:30.944Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
    note: "distinct: KB-F615F682 is about the rep's OWN browsing leaking into a customer's feed (audience filter); this is about the login event's success parameter being ignored, so failed customer sign-ins are counted."
---
Source: the storefront sign-in form sends GA event `login` (method password) with success:false on a failed attempt and success:true on success (vc-frontend sign-in-form.vue); the sales-rep module's GA read (vc-module-sales-rep PR 15 at e5bcccb, AnalyticsSalesRepActivitySource / SalesRepAnalyticsScope) selects the logins category by event name `login` plus session_kind=self, is_sales_rep=customer and organization_id only — no success parameter filter. Live, vcptcore B2B-store: in the 2026-10-19 17:00 UTC hour, three members of one organization signed in successfully and one of them also had one failed attempt first (wrong password); a plain rep token on POST /graphql/sales-rep read the 17:00Z logins bucket for that organization as count 4 (salesRepCustomerActivitySummary.visitsCount also includes it), while the rep's own activity under the same organization was excluded by the is_sales_rep filter. The extra 1 is consistent with the failed attempt being counted; GA rows carry no per-user identity, so the attribution is from source plus the count, not from the row.
