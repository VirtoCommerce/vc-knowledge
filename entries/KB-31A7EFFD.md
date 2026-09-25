---
id: KB-31A7EFFD
subject: the Lists page disables Create at the per-user list limit and shares only from the Settings dialog
plane: experiential
question: Why is Create list disabled on /account/lists, and where is a list's share link?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/lists
  - coordinate: /account/lists/{id}
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:14:23.127Z
    by: session:memimpor
    who: Lenajava1
---
On /account/lists the Create list button disables once the signed-in user owns as many lists as the store's per-user lists limit allows - an account already at the limit sees a permanently disabled button, BY DESIGN. The list Settings dialog is one shared component: opened from the list card's menu it is editable, opened from the button on the list detail page it is view-only with its fields disabled. There is no Share button on the list card or detail page; the share URL with its copy icon lives only inside the Settings dialog, shown when the list's sharing scope supports a link and the user is a corporate member.
