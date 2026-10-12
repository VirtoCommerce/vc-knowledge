---
id: KB-5B2E99AB
subject: /account/returns shows a second tab named after the selected organization to holders of xapi:my_organization:return:view
plane: experiential
question: What does the storefront Returns page show an organization maintainer who may view the organization's returns?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /account/returns
  - coordinate: /account/returns?scope=organization
  - coordinate: /account/returns/{id}
evidence:
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:53:23.327Z
    by: session:b6bd53fc
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T14:50:56.073Z
    by: session:62b7421e
    who: Lenajava1
    note: Theme 2.60.0-pr-2523-9e4e (merge of dev into the PR branch; returns files differ from 3f1b9c6 only by VCST-6107 contrast classes). A non-holder org member opening /account/returns?scope=organization saw no tab and the own list ("There are no returns yet"); console 0 errors.
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T16:14:37.131Z
    by: session:62b7421e
    who: Lenajava1
    note: "Fresh lane A run on theme 2.60.0-pr-2523-9e4e: tab present for global, membership and organization-level role holders; absent for non-holders and for locked/invited/rejected/deleted memberships; follows the switcher for a two-org contact; after withdrawing the organization-level role the tab survived a reload and opening it routed to /403; a fresh sign-in dropped it; re-granting needed a new sign-in."
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-09T20:39:59.663Z
    by: session:61a7b05a
    who: yuskithedeveloper
    note: "Re-observed on theme 2.60.0-pr-2523-466f with Return 3.1005.0-pr-28-fb4f: maintainer sees My returns plus a tab named after the selected organization with a Buyer name column and the '...product or buyer' placeholder; a colleague's return opens read-only (Requested by, no Cancel, no Edit); a member without the permission gets no tab and ?scope=organization shows the own list (only GetReturns sent); a two-organization holder's tab follows the switcher. The org-tab status filter no longer offers Draft."
---
On theme 2.59.0-pr-2523, /account/returns shows "My returns" plus a tab named after the selected organization (?scope=organization) when the token carries xapi:my_organization:return:view; the tab adds a sortable Buyer name column and an organization search placeholder. A colleague's return opens read-only with a "Requested by" row and no Cancel or Edit. Without the permission there is no tab and ?scope=organization silently shows the own list (client-side, no request). The tab follows the organization switcher. After a revoke the tab stays, even across a reload, until a new access token, and opening it routes to /403. At 375 px the organization label truncates with an ellipsis (about 108 px) and the full name is in title and the accessible name.
