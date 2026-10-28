---
id: KB-F71D8D4D
subject: A passkey grant is accepted only when the POST carries an Origin header matching the assertion's origin; a refused attempt burns the challenge, a used assertion replays as login_failed, and a challenge 327 s old (options timeout 300000) was still accepted
plane: experiential
question: What must accompany a valid passkey assertion on POST /connect/token grant_type=passkey, is it single-use, and does the challenge expire at the 300 s options timeout?
questions:
  - text: Why does a captured valid passkey assertion fail when posted from a script?
  - text: Is a passkey assertion replay refused at /connect/token?
  - text: Does the server enforce passkey challenge expiry?
  - text: Does a failed passkey grant consume the challenge?
status: active
appliesTo:
  - axis: feature
    value: passkey-sign-in
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /connect/token
  - coordinate: POST /api/platform/security/sign-in
  - coordinate: Query.passkeyRequestOptions
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: a5a20e99d84d8f1b
    conditions: platform=3.1091.0-pr-3144-e637; module:VirtoCommerce.Passkeys=3.1000.0-pr-1-a25f; store=B2B-store
    at: 2026-10-28T14:12:29.024Z
    by: session:56fb99bb
    who: Lenajava1
---
Storefront /sign-in real-click passkey assertions were captured unconsumed (their /connect/token POST aborted in the browser) and then posted through the same browser context. Posted to the storefront-host /connect/token WITH Origin = storefront origin -> 200 token. Posted first WITHOUT an Origin header (twice, from a script and from the browser context) -> 400 invalid_grant/login_failed, and the same assertion then also failed with the correct Origin, so a refused attempt consumes the challenge. Re-posting an assertion that had just obtained a token -> 400 login_failed on storefront /connect/token, platform /connect/token and platform cookie sign-in; accessFailedCount stayed 0. An assertion over a challenge captured at 13:50:20Z posted 327 s later (options timeout 300000) -> 200 token: no server-side expiry at the client timeout. Assertions older than a later successful sign-in with the same credential fail (signCount). Consequence: API probes of the passkey grant must send the Origin header, or every assertion reads as invalid.
