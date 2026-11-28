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
  - method: observation
    deployment: vcptcore_qa1
    build: 07060bece0224de7
    conditions: platform=3.1107.0-pr-3182-2232
    at: 2026-11-28T19:11:09.538Z
    by: session:d73f2151
    bodyHash: 0439f10f5d6a
    who: kutasinaelena
    contradicts: true
    contested: platform 3.1107.0-pr-3182-2232 vs 3.1105.0-pr-3182; platform 3.1107.0-pr-3182-2232 vs 3.1105.0-pr-3182-27bb
    note: "On platform 3.1107.0-pr-3182 (build 2232d606) a value with leading/trailing spaces (also tab, CR/LF, NBSP, U+3000) is trimmed before storing: 200, response echoes the trimmed value, the trimmed value authenticates by api_key header and ?api_key=, and the untrimmed value via ?api_key= gets 401. Detail in KB-696E8EBE."
  - method: observation
    deployment: vcptcore_qa1
    build: 07060bece0224de7
    conditions: platform=3.1107.0-pr-3182-2232
    at: 2026-11-28T19:16:26.857Z
    by: session:d73f2151
    bodyHash: 0439f10f5d6a
    who: kutasinaelena
    contradicts: true
    contested: platform 3.1107.0-pr-3182-2232 vs 3.1105.0-pr-3182; platform 3.1107.0-pr-3182-2232 vs 3.1105.0-pr-3182-27bb
    note: "On 3.1107.0-pr-3182-2232, POST users/apikeys with two spaces + a 36-char visible-ASCII value + two spaces returned 200. The key was stored trimmed: the hint is the inner value's first4…last4, and the inner value authenticates in the api_key header (200). The padded form sent in the query string (%20-encoded) gets 401. An inner space, a tab, an NBSP, é or a Cyrillic letter inside the value is rejected with 400 \"The API key may contain only visible ASCII characters, without spaces. Send an empty value to have the platform generate one.\""
---
PUT /api/platform/security/users/apikeys with apiKey = two spaces + 28 chars + two spaces (32 total) answered 200; the stored hint keeps the spaces ("  xx…yy  "). Sent in the api_key header the key gets 401 (HTTP strips surrounding whitespace, trimmed form also 401); sent as ?api_key=%20%20…%20%20 it gets 200. Admin Shell renders the hint with spaces collapsed; toggling the key off/on there kept it working. Conversely, a normal key sent in the header with surrounding spaces works (stripped), and the same with %20 padding in the query gets 401.
