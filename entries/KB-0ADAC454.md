---
id: KB-0ADAC454
subject: A completed mission card on /account/missions draws a 56px circle-check directly on a 0.55-alpha success scrim over the banner art, so the check's contrast depends on the art
plane: experiential
question: How does the storefront mark a completed mission card on /account/missions, and does the check icon have its own background?
questions:
  - text: What overlay does a completed loyalty mission card show on the Missions & challenges page?
  - text: Is the completed-mission check icon on an opaque disc or on a translucent scrim?
  - text: Does the completed mission card overlay carry aria-hidden?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/missions
  - coordinate: Query.loyaltyMissionProgress
evidence:
  - method: observation
    deployment: vcst_qa
    build: f6080d062139e9f0
    conditions: theme=3.1.0-pr-11-d67b-d67b0e0a; store=B2B-store; viewport=1440x900; browser=chromium
    defect: VCST-6142
    at: 2026-10-20T15:07:59.064Z
    by: session:54002df0
    who: Aleksandra-Mitricheva
---
Signed in as a loyalty member with one completed Order-count mission (3 of 3 orders), /account/missions page 2 renders that card with a .mission-card__done overlay inside .mission-card__banner, exactly covering the banner image (352x208 at 1440px), aria-hidden="true". Its only child is a 56px vc-icon (lucide circle-check) with no background of its own. The overlay background is the success colour at alpha 0.55 (light: rgb(59,119,84)/0.55, icon white; dark: rgb(110,178,137)/0.55, icon near-black). The card footer reads "Mission completed". Incomplete cards (0-99%) render no overlay. Because the scrim is translucent, the check-vs-background contrast follows the banner art: measured from a screenshot over light art, median 2.34:1 (light theme); analytically 2.26:1 over white art (light) and 2.31:1 over black art (dark), below the 3:1 non-text minimum. A fix that wraps the icon in an opaque disc (vc-frontend PR #2590) was pending at observation time.
