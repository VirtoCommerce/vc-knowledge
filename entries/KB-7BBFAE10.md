---
id: KB-7BBFAE10
subject: a UCP-audience token needs resource set to the storefront's /ucp/mcp and must be minted at the storefront
plane: experiential
question: How do I mint a token /connect/token will issue with an audience the UCP /ucp/mcp endpoint accepts?
questions:
  - text: How do I get a login token that the AI commerce endpoint will accept?
  - text: Why does the agent commerce endpoint reject my token with an audience of resource_server?
  - text: Which origin and resource value must a token grant use so its audience matches the storefront MCP endpoint?
  - text: What causes invalid_target versus invalid_token when minting and using a token for the agent endpoint?
concepts:
  - id: access-token
  - id: ucp
status: active
appliesTo:
  - axis: surface
    value: rest
  - axis: surface
    value: ucp
anchors:
  - coordinate: POST /connect/token
  - coordinate: /ucp/mcp
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-09-25T11:16:30.692Z
    by: session:memimpor
    who: Lenajava1
    build: e7c4faf10f640e7c
    buildFrom: deploy-history
  - method: observation
    deployment: vcptcore_regression
    at: 2026-10-13T13:14:01.653Z
    by: session:624d35a1
    who: kutasinaelena
    note: Password grant at the storefront /connect/token with storeId=B2B-store and resource={storefront}/ucp/mcp returned 200 for two B2B buyers on UCP 3.1010.0; link_buyer_identity accepted the token.
    build: ad2e031743ee94a0
    buildFrom: deploy-history
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.UCP=3.1011.0-pr-13-5de2; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; grant=password at storefront /connect/token, resource=/ucp/mcp, no client_id
    at: 2026-11-10T07:38:45.455Z
    by: session:d6dc0a6d
    bodyHash: a40f6e256e4d
    who: Lenajava1
    contradicts: true
    contested: platform 3.1096.0 vs 3.1073.0-pr-3121-9965-vcst-4464-onx-mcp-auth-99655e69
    note: On 2026-11-10 the repo runner (suites:machine path, graphql-runner.ts --case on suite 102 UCPA-009, role ACME_ADMIN) minted the storefront password-grant token (grant_type=password, storeId, resource={storefront}/ucp/mcp, no client_id) with no mint failure logged, then POST /ucp/mcp tools/call link_buyer_identity AND search_products both returned 401 with body code identity_required, content "The access token was not issued to an MCP client. Connect the MCP client with a user's sign-in." So on this build the storefront password-grant UCP token is refused. The entry stays true for UCP 3.1010.0 and earlier (before ucp#13 moved /ucp/mcp into the MCP module).
---
A plain token grant can mint a token UCP accepts, with no authorization-code round trip, if it is requested at the storefront origin with resource set to the storefront's /ucp/mcp; the token then carries that audience and the storefront as issuer. Without resource the token mints but its audience is resource_server, which UCP rejects. Requesting at the back-office origin, or naming a back-office /ucp/mcp resource from either origin, returns 400 invalid_target, because only the storefront resource is registered. Mixing origins between token and call gives 401 invalid_token.
