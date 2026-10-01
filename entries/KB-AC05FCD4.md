---
id: KB-AC05FCD4
subject: The Push Messages audience-builder mode shown when a saved message is reopened is derived from the stored recipients query, not persisted alongside it.
plane: experiential
question: Does a push message saved in 'Advanced query' mode reopen in that mode?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.pushmessages
  - axis: surface
    value: admin-spa
  - axis: version
    value: 3.1006.0-pr-28
anchors:
  - coordinate: /api/push-message
  - coordinate: POST /api/push-message/preview-recipients
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T11:50:45.796Z
    by: session:8402b2ed
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T12:10:26.890Z
    by: session:8402b2ed
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T14:38:45.328Z
    by: session:8402b2ed
    who: kutasinaelena
---
In the Admin SPA Push Messages app (New push message / Push message details blade, Audience section) the builder offers four modes: Everyone, Specific people or companies, Match by conditions, Advanced query. A draft authored in 'Advanced query' with the raw phrase membertype:Employee, saved and reopened from Drafts, comes back in 'Match by conditions' with a Custom-conditions rule 'Customer type is Employee' and no raw-phrase textarea. A draft whose phrase the condition builder cannot express, membertype:Employee AND (status:Approved OR status:New), reopens in 'Advanced query' with the phrase intact. The same rule is visible while editing: the 'Back to conditions' link appears only while the typed phrase is condition-representable. So the blade re-derives the mode by trying to parse the stored member query back into conditions; the authoring mode is not stored. The recipient estimate round-trips unchanged either way. For tests: a round-trip assertion on 'Advanced query' must use a phrase the builder cannot represent, or the two modes are indistinguishable.
