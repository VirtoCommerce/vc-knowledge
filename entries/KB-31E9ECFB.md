---
id: KB-31E9ECFB
subject: On theme 2.60.0-pr-2548 /account/missions renders 12 mission-card skeletons while GetLoyaltyMissionProgress is pending (full load and page change); page CLS 0.012 at 1920x1080
plane: experiential
question: Does the /account/missions card grid show skeleton cards while missions load on the PR-2548 theme, and what CLS remains?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: viewport
    value: 1920
anchors:
  - coordinate: /account/missions
  - coordinate: Query.loyaltyMissionProgress
evidence:
  - method: observation
    deployment: vcst_qa
    conditions: theme=2.60.0-pr-2548-6fff-6fff28ab; served_by=local-container-proxy
    at: 2026-10-12T21:33:27.702Z
    by: session:813274dc
    who: Aleksandra-Mitricheva
---
Theme 2.60.0-pr-2548-6fff-6fff28ab (served from a local container proxying /graphql to vcst-qa), Chromium 1920x1080. A MutationObserver on .missions__cards logged 12 .mission-card-skeleton children, then 0 skeletons and 12 (or 7 on the last page) .mission-card children ~600 ms later, on 3/3 full reloads and on 3/3 pagination clicks (2, 3, 1). Buffered layout-shift observer: CLS 0.012 on 3/3 reloads (0.0119 from the sidebar widgets, 0.0001 mega menu), 0 on page change; with the query delayed 3 s CLS was 0.0066. The skeleton grid is 1630 px tall vs 1726 px for 12 real cards (about 24 px shorter per row), so the footer still moves 2063 -> 2236 px, but below the fold at 1080. Earlier builds (2.59.0-pr-2524) rendered no skeleton (KB-C4597892).
