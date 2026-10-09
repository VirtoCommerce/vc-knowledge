---
id: KB-DE6BB786
subject: /account/missions shows a loyalty balance banner (balance + Points history link) and a Redeem your points banner whose Rewards catalog link opens /loyalty-catalog
plane: experiential
question: What banners does the storefront Missions & challenges page show above the mission cards for a member with a loyalty balance?
status: active
appliesTo:
  - axis: feature
    value: loyalty-missions
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/missions
  - coordinate: /account/points-history
  - coordinate: /loyalty-catalog
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T10:56:17.099Z
    by: session:df9d131f
    who: Lenajava1
---
On theme vc-frontend-next 3.0.0-alpha.2685 (Loyalty.Enable and Loyalty.Missions.Enable on, balance mode Customer), a member with a positive balance sees above the mission grid two banners: one titled with the program name + 'balance' showing the points total (thousands separator, e.g. 19,902 points) and a 'Points history' link to /account/points-history, and one 'Redeem your points' / 'Trade points for products in the rewards catalog.' with a 'Rewards catalog' link to /loyalty-catalog (the loyalty catalog was available and listed products priced in PTS). The points-history page heading shows the same balance without a separator ('Balance: 19902'). Both banners render readable in light and dark appearance. The banner-hidden state when no loyalty catalog exists was not reachable without a settings write.
