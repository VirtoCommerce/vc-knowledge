---
id: KB-9CEC2AB6
subject: Opening a UCP continue_url in a signed-out browser for a token-authenticated buyer redirects to /sign-in, then resumes on /cart/{id}?ucp_handoff=1 after sign-in
plane: experiential
question: What happens when a UCP checkout_and_handoff continue_url (/checkout?ucp_session=...) minted for an authenticated buyer is opened in a browser without a storefront session?
status: active
appliesTo:
  - axis: buyer
    value: authenticated-b2b
  - axis: module
    value: virtocommerce.ucp 3.1010.0
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /ucp/v1/internal/handoff/restore
  - coordinate: /cart/{id}
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T13:13:55.763Z
    by: session:624d35a1
    who: kutasinaelena
    build: ad2e031743ee94a0
    buildFrom: deploy-history
---
2026-10-13, UCP 3.1010.0, theme 2.59.0, B2B-store (guest checkout enabled): for a buyer whose MCP token came from a password grant with resource={storefront}/ucp/mcp, opening continue_url /checkout?ucp_session=<token> in a fresh signed-out browser made POST /ucp/v1/internal/handoff/restore return 401 and redirected to /sign-in?returnUrl=/checkout?ucp_resume=<guid>&reauthenticate=1. After UI sign-in as the same buyer, the browser went to /checkout?ucp_resume=<guid>, restore returned 401 then 200, and the page settled on /cart/{cart_id}?ucp_handoff=1 with the MCP cart's lines and totals. Total in a sign-in-first session: two 401s plus one 200 on restore. In an already-signed-in session: one 401 then 200, landing directly. Observed 3 times (two buyers). The resume step uses an opaque ucp_resume id, so the GA page_view after redirect carries ucp_resume, not ucp_session.
