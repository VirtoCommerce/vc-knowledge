---
id: KB-26E5AD6D
subject: An MCP operator without mcp:approve sees only an information panel on Agent approvals, not their own pending change
plane: experiential
question: Admin Shell #/mcp-approvals as an operator without mcp:approve — does the author see and can they withdraw their own pending agent change?
questions:
  - text: Can the author of a pending MCP change see it on Agent approvals without the mcp:approve permission?
  - text: What does Agent approvals show to a user who reviews no MCP scope?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /mcp-approvals
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; role=IT Platform Operator (no mcp:approve)
    defect: suspected
    at: 2026-11-10T19:47:40.197Z
    by: session:40688c0a
    who: Lenajava1
---
The Admin Shell Agent approvals screen (#/mcp-approvals) was opened by a back-office Manager holding the MCP operator role (no mcp:approve, no mcp:manage). That user's own agent had just called restart_platform and left a Pending change. The screen shows only an information panel reading 'You do not review agent changes in any MCP scope.'
- There is no grid.
- The author's own pending change is not listed, and there is no Reject or Withdraw control.
- Another user's pending change is not shown either.

The author can still withdraw the change over MCP from their agent, so the capability exists over MCP but not on the screen. The ticket decision of 2026-10-23 (option 2) says the author sees their own change without mcp:approve. This is therefore recorded as a suspected defect, pending the product owner's confirmation that the decision covers the screen. The REST call the screen sends was not captured.
