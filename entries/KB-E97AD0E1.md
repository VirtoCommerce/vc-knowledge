---
id: KB-E97AD0E1
subject: In the German storefront locale the account sidebar 'Saved credit cards' entry renders the raw key Skyflow.saved_credit_cards.menu_title
plane: experiential
question: Is the Saved credit cards account-menu entry translated when the storefront language is German?
questions:
  - text: Why does the account sidebar show Skyflow.saved_credit_cards.menu_title?
  - text: Does the Skyflow storefront plugin ship a de locale for its account menu item?
status: active
appliesTo:
  - axis: locale
    value: de-de
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /de/account/orders
  - coordinate: /de/account/saved-credit-cards
evidence:
  - method: observation
    deployment: vcst_qa
    build: 1fe74fdd17a1b14e
    conditions: platform=3.1101.0; theme=2.63.0-alpha.2681; browser=chromium
    defect: suspected
    at: 2026-11-26T02:12:03.289Z
    by: session:779b94fc
    who: kutasinaelena
---
Signed-in buyer, switched the header language to Deutsch: on /de/account/orders the account sidebar link to /de/account/saved-credit-cards is labelled with the literal key 'Skyflow.saved_credit_cards.menu_title'. The rest of the page is German (Bestellungen, Filter). Switching to español on the same session renders the same link as 'Tarjetas de crédito guardadas', and English as 'Saved credit cards', so the gap is specific to the German translation of the Skyflow plugin's menu item.
