---
id: KB-19CD3A8D
subject: AngularJS admin API key Generate rotates the user's single key in place; Admin Shell Generate key adds a new row
plane: experiential
question: What does Generate on the AngularJS user API key widget do to an existing key, and how does it differ from Admin Shell Generate key?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/platform/security/users/apikeys
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 78242a4a9ed03804
    conditions: platform=3.1105.0-pr-3182; admin-shell=3.1102.0-pr-25-738b; admin-angularjs=3.1099.0-pr-7-56e2
    at: 2026-11-28T11:54:05.115Z
    by: session:d73f2151
    who: kutasinaelena
---
User with two keys. Admin Shell API keys blade: Generate key added a row and showed a banner with the full key once (Copy, Done, toast 'API key copied'). AngularJS API key widget, Generate then Yes in the confirm dialog: the full key is shown immediately (before OK) with a warning; afterwards the user's list held the same row id and creation time with a new hint, no extra row, so Generate rotates the displayed key in place. The widget keeps showing only the latest hint after reopen. The AngularJS ar and he locales render the dialog and labels in English; de is translated.
