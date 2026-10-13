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
---
A plain token grant can mint a token UCP accepts, with no authorization-code round trip, if it is requested at the storefront origin with resource set to the storefront's /ucp/mcp; the token then carries that audience and the storefront as issuer. Without resource the token mints but its audience is resource_server, which UCP rejects. Requesting at the back-office origin, or naming a back-office /ucp/mcp resource from either origin, returns 400 invalid_target, because only the storefront resource is registered. Mixing origins between token and call gives 401 invalid_token.
