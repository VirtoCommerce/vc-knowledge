---
id: KB-FB1FC7EA
subject: In the /cart Pick points dialog, Cancel on the Pick point info card closes the card but leaves the clicked location's radio checked; the cart selection is unchanged
plane: experiential
question: After clicking a location and pressing Cancel on its Pick point info card, what state does the /cart Pick points dialog keep?
questions:
  - text: Does Cancel on the pickup info card uncheck the location radio?
  - text: Does clicking the map background close the Pick point info card?
status: active
appliesTo:
  - axis: feature
    value: bopis
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; role=personal customer
    at: 2026-11-26T02:39:57.904Z
    by: session:779b94fc
    who: kutasinaelena
---
Personal customer, B2B-store, Whole Foods Market Prospect Heights confirmed. In the reopened dialog, clicking Museum of Arts and Design opened its 'Pick point info' card (Cancel / Pick up here). Cancel closed the card, the dialog stayed open, but Museum of Arts and Design's radio remained [checked] while the cart row still showed the confirmed 57 7th Ave address; closing the dialog changed nothing. Clicking a map marker likewise checks that location's radio and opens its card (one card at a time). Markers that sit under the open card cannot be clicked until it is closed.
