---
id: KB-0BAA4DB6
subject: "The Customer activity widget's hourglass hint triggers are 14x14 px role=button spans named 'Tracked storefront activity: can take up to 48 hours to appear.' — the same name on every row — and clear the WCAG 2.5.8 spacing exception at 320 and 1920 px"
plane: experiential
question: How big are the hourglass tooltip triggers in the sales-rep Customer activity widget and do they have an accessible name?
questions:
  - text: Do the tracked-metric hourglass icons meet the 24x24 target size?
  - text: What do screen readers announce for the hourglass next to Total visits?
concepts:
  - id: sales-rep
status: active
appliesTo:
  - axis: feature
    value: sales-rep-customer-activity-widget
  - axis: module
    value: sales-rep
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: 5c581fd90c2dabb7
    conditions: theme=2.61.0-pr-2458-3f0b; module:VirtoCommerce.SalesRep=3.1015.0-pr-15-e5bc; role=Sales Representative
    at: 2026-10-20T18:21:25.773Z
    by: session:68b36f2b
    who: Aleksandra-Mitricheva
---
On /company/my-customers/{id} (rep Priya, served org), the Summary rows Last web login, Total visits, Last search term and Last viewed product each carry a span.tracked-metric-hint__trigger (role=button, tabindex=0, aria-expanded=false, aria-describedby=vc-tooltip-*) measuring 14x14 CSS px (getBoundingClientRect, both 1920 and 320 px viewports) with no padding. Its accessible name comes from its content, a role=img icon with aria-label 'Tracked storefront activity: can take up to 48 hours to appear.' (Playwright resolves it by getByRole('button', {name: 'Tracked storefront activity:'}); the aria snapshot prints the button without a name and the named img as its child). All four share that name. Spacing: centre-to-nearest-other-target edge distance 41.5 / 41.5 / 48.5 / 13 px (the last to the Last-viewed-product link), so a 24 px circle around each centre touches no other target — the WCAG 2.5.8 spacing exception holds; visual edge gap to that link is ~6 px. Source: tracked-metric-hint.vue renders VcIcon :size=14 inside the trigger.
