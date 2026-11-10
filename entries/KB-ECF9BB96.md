---
id: KB-ECF9BB96
subject: UCP buyer sign-in runs through the storefront /sign-in and a consent page; signed-in cart tools act on the buyer's storefront default cart
plane: experiential
question: How does a buyer link an MCP client to UCP, and which cart do list_carts and create_cart use once signed in?
questions:
  - text: Which login page does the UCP OAuth authorize redirect to?
  - text: What does the UCP consent page show?
  - text: Does signed-in create_cart create a new cart or add to the storefront cart?
  - text: Does link_buyer_identity return the organization?
status: active
appliesTo:
  - axis: buyer
    value: authenticated-b2b
  - axis: module
    value: virtocommerce.ucp 3.1011.0-pr-13
  - axis: surface
    value: ucp
anchors:
  - coordinate: /connect/authorize
  - coordinate: /sign-in
  - coordinate: /ucp/mcp
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: module:VirtoCommerce.Ucp=3.1011.0-pr-13-5de2; module:VirtoCommerce.Mcp=3.1000.0-pr-1-df74; theme=2.62.0-pr-2629; store=B2B-store
    at: 2026-11-09T19:06:33.697Z
    by: session:d6dc0a6d
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; module:VirtoCommerce.UCP=3.1011.0-pr-13-5de2; theme=2.62.0-pr-2629-8fcc; store=B2B-store
    at: 2026-11-10T14:21:07.143Z
    by: session:40688c0a
    bodyHash: c6be18d45b2b
    who: Lenajava1
    note: "2026-11-10 round 2, second buyer (first authorization for this client): same storefront /sign-in -> consent 'Claude Code — Shop (QA)' -> Allow -> token; link_buyer_identity buyer/org matched. A buyer who had already allowed this client (round 1) went from /sign-in straight to the callback with no consent page (README: explicit consent once per user and agent). list_carts returned the storefront default cart with organization_id."
---
UCP 3.1011.0-pr-13, B2B-store. {storefront}/connect/authorize?client_id=claude-code-ucp-qa&resource={storefront}/ucp/mcp -> /oauth/authorize -> storefront /sign-in?returnUrl=... (the regular storefront sign-in page: email code by default, 'Sign in with a password instead'). After sign-in the browser returned to /connect/authorize showing a consent page titled 'Allow Claude Code — Shop (QA)': '<client name> wants to access your account', the user's email, 'Work with your permissions / Everything your account is allowed to do at {host}/ucp/mcp', 'Know who you are', 'Stay signed in / Until you revoke access', Cancel and Allow. Allow -> code -> token aud={storefront}/ucp/mcp, client_id claude-code-ucp-qa, 30-min access + refresh. link_buyer_identity then returned linked:true, buyer_id (= user id), organization_id, user_name, buyer_name, organization_name, identity_source platform_oauth - matching the storefront header (name, organization). Signed-in list_carts with no cart_type returned exactly one cart: the storefront default cart (same id and line count as the storefront cart badge). Signed-in create_cart with no cart_type returned that same cart id and ADDED the requested quantity to the existing line (2 -> 4). Signing in on the storefront merged the browser's anonymous handed-off cart into that default cart. logout_buyer revoked the MCP token but left the storefront browser session signed in.
