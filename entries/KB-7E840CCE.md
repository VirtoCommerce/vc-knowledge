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
---
On vc-frontend-next 3.0.0-alpha.2685, Firefox at 375x812, three signed-in B2B accounts with 13, 3 and 9 unread push messages: the accessibility tree names the mobile header bell button '13', '3' and '9' respectively on the home page (the bell img child has no name; the VcBadge text is the only name source). At 1920x1080 the same account's desktop header bell is named '13 Alerts'. The header is global, so this holds on every page. Source push-messages-mobile.vue renders a <button v-bind=triggerProps> holding a VcIcon and a VcBadge (v-if unreadCount) with no aria-label, identical to upstream vc-frontend apart from an icon class; with zero unread the badge is not rendered, so the name would be empty (inferred from source, not observed: no zero-unread account was reachable without marking messages read).
