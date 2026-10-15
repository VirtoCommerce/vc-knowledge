---
id: KB-BC9F3CB1
subject: The sales-rep customer profile stat cards show no period-over-period change; the MTD card shows only '% of YTD'
plane: experiential
question: Do the customer profile stat cards on the storefront show a period-over-period $ or % change?
status: active
appliesTo:
  - axis: role
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/:organizationId
  - coordinate: Query.salesRepCustomerOrderStatistics
evidence:
  - method: observation
    deployment: vcst_qa
    build: e00c89f4da0e9c98
    conditions: platform=3.1083.0; theme=2.60.0-alpha.2579; module:VirtoCommerce.SalesRep=3.1014.0-pr-13
    at: 2026-10-14T17:52:06.574Z
    by: session:6630b76f
    who: Lenajava1
---
On /company/my-customers/{organizationId} the five stat cards are: Orders in “New” status (count, money total, 'of N created in the last 7 days'), Active carts (items, 'not for checkout', 'items this week'), Purchased · MTD (money total + '<p>% of YTD'), Orders placed · YTD (count + total), Avg order value (money + 'Average per order (YTD)'). None renders a signed $ or % period-over-period change. The SalesRepCustomerOrderStatistics query the page sends selects only ytdVsLastYear{countChangePercent} from the comparison API (null for an org with no prior-year orders), and no totalChange/countChange; a zero-order org renders 0 / $0.00 / '0% of YTD' everywhere, never NaN.
