---
id: KB-7DB10B1F
subject: On storefront cultures with no theme locale file (nl-NL, nl-BL) the whole Sales Rep hub, including the layout empty state, renders English strings with no raw i18n keys
plane: experiential
question: What do Sales Rep hub pages (/company/dashboard) show when the storefront language is Dutch (nl-NL / nl-BL)?
status: active
appliesTo:
  - axis: feature
    value: sales-rep-hub
  - axis: locale
    value: nl
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/dashboard
  - coordinate: /nl-NL/company/dashboard
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-07T14:43:40.922Z
    by: session:89850094
    who: Aleksandra-Mitricheva
---
Theme 2.59.0 PR build 2539, B2B-store whose language switcher lists two entries labelled "Nederlands" (cultures nl-BL and nl-NL) plus two labelled "English" (en-US, en-GB). Selecting either Dutch entry (in-session via switcher, which does a full reload to /nl-NL/... or /nl-BL/...) and cold-loading /nl-NL/company/dashboard both render html lang=nl-*, but every Sales Rep hub string (sidebar 'Sales Rep hub', H1 'Dashboard', the 'All blocks are hidden' empty state and its buttons) is the English text; no raw sales_rep.* keys appear. The same page in de/es/fr/it/ja/pl/pt/ru/zh renders translated strings. So Dutch is an English fallback, consistent across the hub, not a partial translation.
