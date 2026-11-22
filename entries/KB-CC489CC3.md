---
id: KB-CC489CC3
subject: Switching the storefront currency on /compare reloads the page and re-sends the CompareProducts query with the new currencyCode; price cells render in the new currency symbol
plane: experiential
question: What happens to /compare prices when the storefront currency is switched?
questions:
  - text: Does the compare page re-request compareList with currencyCode after a currency change?
  - text: Do compare price cells keep the old currency symbol after switching currency?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /compare
  - coordinate: Query.compareList
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 6cf3e54e8335fd8d
    conditions: theme=2.63.0-pr-2635-8318; role=guest
    at: 2026-11-22T20:57:02.111Z
    by: session:fcdb91ae
    who: kutasinaelena
---
Guest, Chromium 1920px, two plain products in one tab (USD $49.99/$59.99). Selecting EUR in the header currency selector reloaded the page (URL stayed /compare); a CompareProducts request (compareList with cultureName and currencyCode=EUR) was sent and both price cells rendered with the euro symbol (EUR 0.00 on a stand without an EUR price list, Add to cart disabled). Switching back to USD restored the original strings exactly. No GraphQL errors.
