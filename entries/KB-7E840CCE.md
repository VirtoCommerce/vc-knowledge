---
id: KB-7E840CCE
subject: On the vc-frontend-next 3.0 storefront the mobile header bell button's accessible name is only the unread count; desktop reads '<count> Alerts'
plane: experiential
question: What accessible name does the vc-frontend-next storefront mobile header give the push-notifications bell button?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: theme
    value: vc-frontend-next-3.0
  - axis: viewport
    value: mobile
anchors:
  - coordinate: /account/notifications
  - coordinate: push-messages-mobile.vue
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-09T11:26:08.703Z
    by: session:df9d131f
    who: Lenajava1
    build: a6e179f178104063
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: fcea7ce160c179b6
    conditions: theme=vc-frontend-next-3.3.0-pr-23-7be3; store=B2B-store; browser=chromium; viewport=375x812
    at: 2026-11-16T12:57:44.333Z
    by: session:4cdfee7c
    bodyHash: 92db9db7b579
    who: Lenajava1
    contradicts: true
    contested: virtocommerce.fileexperienceapi 3.1008.0 vs 3.1007.0-pr-25-ff70; virtocommerce.profileexperienceapimodule 3.1024.0 vs 3.1020.0; virtocommerce.xapi 3.1031.0 vs 3.1026.0; virtocommerce.xcart 3.1046.0 vs 3.1040.0-pr-149-9fe0; virtocommerce.xcatalog 3.1024.0 vs 3.1023.0; …
    note: "On vc-frontend-next 3.3.0-pr-23-7be3 (later than 3.0.0-alpha, carries upstream vc-frontend 2.62.0), Chromium 375x812: the mobile bell button is named 'Notifications: 1' with 1 unread (badge rendered as a dot, no number) and 'Notifications' with 0 unread. Desktop reads 'Alerts: 2' for two unread."
---
On vc-frontend-next 3.0.0-alpha.2685, Firefox at 375x812, three signed-in B2B accounts with 13, 3 and 9 unread push messages: the accessibility tree names the mobile header bell button '13', '3' and '9' respectively on the home page (the bell img child has no name; the VcBadge text is the only name source). At 1920x1080 the same account's desktop header bell is named '13 Alerts'. The header is global, so this holds on every page. Source push-messages-mobile.vue renders a <button v-bind=triggerProps> holding a VcIcon and a VcBadge (v-if unreadCount) with no aria-label, identical to upstream vc-frontend apart from an icon class; with zero unread the badge is not rendered, so the name would be empty (inferred from source, not observed: no zero-unread account was reachable without marking messages read).
