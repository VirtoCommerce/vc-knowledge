---
id: KB-696E8EBE
subject: Client-chosen API key values are trimmed of .NET whitespace (spaces, tab, CR/LF, NBSP, U+3000) and then must be 32–128 visible ASCII; zero-width space, BOM, look-alikes and inner spaces are refused 400
plane: experiential
question: How does POST/PUT /api/platform/security/users/apikeys validate and normalise a client-supplied apiKey value (trim, charset, length)?
questions:
  - text: Does the platform trim a client API key with leading or trailing whitespace?
  - text: Is a zero-width space or BOM around an API key stripped or refused?
  - text: What is the maximum length of a client-chosen API key?
  - text: Can an API key contain quotes, backslashes or angle brackets?
  - text: What does PUT users/apikeys do with apiKey set to an ellipsis character?
status: active
appliesTo:
  - axis: module
    value: platform-security
  - axis: surface
    value: rest-api
anchors:
  - coordinate: POST /api/platform/security/users/apikeys
  - coordinate: PUT /api/platform/security/users/apikeys
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 07060bece0224de7
    conditions: platform=3.1107.0-pr-3182-2232; role=non-admin with platform:security:update
    at: 2026-11-28T19:11:03.260Z
    by: session:d73f2151
    who: kutasinaelena
    note: "distinct: KB-B323BA37 covers masked/empty/short handling on the earlier build (no trim, no upper bound, no charset rule); this entry is the new build's normalisation: which characters Trim removes, the 128 upper bound, the visible-ASCII charset refusal and its exact messages."
---
On build 2232d606 a non-admin with platform:security:update saved client values by POST/PUT users/apikeys. Values wrapped in spaces, tab, CR/LF, NBSP (U+00A0) or ideographic space (U+3000) returned 200; the response echoed the trimmed value, the hint was derived from it, the trimmed value authenticated by api_key header and ?api_key=, and the untrimmed value sent by ?api_key= got 401. 128 chars + trailing spaces stored as 128 (200); 129 chars and a value trimming to 31 returned 400 "The API key must be 32 to 128 characters long. Send an empty value to have the platform generate one." Zero-width space U+200B and BOM U+FEFF at the edges are NOT trimmed and returned 400 "The API key may contain only visible ASCII characters, without spaces. …"; the same 400 for Cyrillic/fullwidth look-alikes, inner space, NUL, DEL and emoji. A value with " \ { } < > & ` ~ | ' is accepted and authenticates; hints such as "<b>X…</b>" and "{{7*…*7}}" render as escaped text in both the AngularJS blade and the Admin Shell list. PUT with apiKey "…" on a normal key returned 400 "The API key looks masked…"; "..." returned the length 400; the key's own hint plus a trailing space kept the key (200). A PUT carrying a new value together with isActive=false applied both (old value 401, new value 401 while inactive; re-activating with an empty value made the new value work). A 36-char GUID with hyphens is accepted as a client value. The 31-char length message of earlier builds ("must be at least 32 characters long") is now "must be 32 to 128 characters long".
