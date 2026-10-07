---
id: KB-ED76FF4B
subject: Storefront return detail shows only the line reasonComment, never reasonCode or the legacy free-text reason
plane: experiential
question: Does /account/returns/:id show the return reason of each line to the buyer?
questions:
  - text: Will I see the reason I chose for returning each item on my return details page?
  - text: Does the buyer's return detail page display the reason code or only the free-text comment?
  - text: Why doesn't a return created by an admin show its reason to the customer on the storefront?
  - text: Does the return line type in GraphQL expose the legacy free-text reason field?
concepts:
  - id: return
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: /account/returns/{}
  - coordinate: Query.return
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-29T05:46:55.668Z
    by: session:p1804
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-01T19:55:44.242Z
    by: session:0cfc9f97
    who: kutasinaelena
  - method: observation
    deployment: vcst
    at: 2026-10-05T20:22:43.397Z
    by: session:51fcfb97
    who: kutasinaelena
    note: "REG-2026-10-05-1752 ORD-037 on theme 2.59.0-pr-2524: RET261005-00005 created with reason FaultyOnArrival; return detail shows no reason. return-item-summary.vue renders only reasonComment."
  - method: observation
    deployment: vcptcore_dev
    at: 2026-10-06T17:52:57.695Z
    by: session:b6bd53fc
    note: "Also true in the organization holder's read-only view of a colleague's return on Return 3.1005.0-pr-28-62f9: only reasonComment is shown."
---
On theme 2.59 (pr-2500) the buyer's return detail page renders, per line, the product name, SKU, reasonComment (if any), the per-line decline reason and attachments. It does not render reasonCode (a buyer line with reasonCode NoLongerNeeded and no comment shows no reason) and does not render the legacy free-text 'reason' that admin-created returns carry (an admin-created return with reason 'AGENT-TEST admin-created return' shows no reason). The xAPI ReturnLineItemType selected by GetReturn has reasonCode and reasonComment but no legacy reason field.
