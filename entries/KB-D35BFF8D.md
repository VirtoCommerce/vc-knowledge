---
id: KB-D35BFF8D
subject: A storefront organization switch made in one tab reloads every other open storefront tab of the same session into the new organization, closing any open Organizations panel there
plane: experiential
question: What happens to other open storefront tabs when a multi-organization buyer switches organization in one tab?
questions:
  - text: Does an organization switch propagate to other browser tabs on the storefront?
  - text: After switching organization in the header, do other tabs keep showing the old organization?
  - text: Is a second tab left stale after an organization switch?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /cart
  - coordinate: /account/orders
  - coordinate: POST /connect/token
evidence:
  - method: observation
    deployment: vcst_qa
    build: 2b7c46e6924efca5
    conditions: platform=3.1099.0; theme=2.63.0-pr-2667-0444-0444048a; design=classic; browser=firefox
    at: 2026-11-23T08:30:25.786Z
    by: session:7ce2b8e4
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 6feb83b80e6f1f12
    conditions: theme=2.63.0-alpha.2681; browser=edge; store=B2B-store
    at: 2026-11-26T07:55:23.026Z
    by: session:779b94fc
    bodyHash: 25e63b784571
    who: kutasinaelena
    note: "2026-11-26: two tabs, panel open in tab B; switch in tab A -> both tabs reloaded into the new org, tab B panel closed; switch back from tab B reloaded tab A as well."
---
Buyer with two Approved memberships (org-maintainer in one org, org-employee in the other), classic design. Tab A on / and tab B on /cart, both in org X; in tab B the header Organizations panel was left open. In tab A the buyer picked org Y in the header Organizations panel (desktop) — tab A reloaded in Y within ~1 s, and tab B also reloaded on its own: its header name, the Organizations control label and the account-menu name all showed Y, and its panel was closed. The reverse switch made from the phone menu (Corporate > My organizations) in tab B reloaded tab A into the chosen org the same way. A switch fired while an SPA navigation to /account/orders had just completed landed tab A on /account/orders in the new org. No console errors.
