---
id: KB-4D140EFC
subject: A client-chosen user API key with leading/trailing spaces is accepted and stored as-is; it then authenticates only via the URL-encoded query string, never via the api_key header
plane: experiential
question: Are user API key values trimmed or rejected when they have surrounding whitespace?
questions:
  - text: Why does an API key with spaces get 401 in the api_key header?
  - text: Does PUT users/apikeys validate whitespace in apiKey?
status: active
appliesTo:
  - axis: auth
    value: api_key
  - axis: surface
    value: rest-api
anchors:
  - coordinate: PUT /api/platform/security/users/apikeys
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182
    defect: suspected
    at: 2026-11-28T11:38:31.599Z
    by: session:d73f2151
    who: kutasinaelena
---
PUT /api/platform/security/users/apikeys with apiKey = two spaces + 28 chars + two spaces (32 total) answered 200; the stored hint keeps the spaces ("  xx…yy  "). Sent in the api_key header the key gets 401 (HTTP strips surrounding whitespace, trimmed form also 401); sent as ?api_key=%20%20…%20%20 it gets 200. Admin Shell renders the hint with spaces collapsed; toggling the key off/on there kept it working. Conversely, a normal key sent in the header with surrounding spaces works (stripped), and the same with %20 padding in the query gets 401.
