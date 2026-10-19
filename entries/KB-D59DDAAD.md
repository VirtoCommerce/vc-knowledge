---
id: KB-D59DDAAD
subject: A storefront organization switch that fails at /connect/token still stores the target org as last-used and signs the buyer out, so the next code sign-in sends the refused org
plane: experiential
question: When a storefront organization switch fails, does the storefront still remember that organization as the last-used one?
questions:
  - text: Does a failed header org switch overwrite the last-used organization in localStorage?
  - text: What happens in the storefront when switching to an organization whose membership was locked after the menu opened?
  - text: Why does the next code sign-in send an organization the buyer could not switch to?
status: active
appliesTo:
  - axis: feature
    value: organization-switcher
  - axis: persona
    value: multi-org
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: POST /connect/token
  - coordinate: /sign-in
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 44afaf683e598288
    conditions: platform=3.1084.0-pr-3140; theme=2.61.0-pr-2575; module:VirtoCommerce.Customer=3.1030.0-pr-321-c54f
    defect: suspected
    at: 2026-10-19T22:26:56.655Z
    by: session:1b3fc42e
    who: kutasinaelena
    note: "distinct: KB-0080ED6D covers a SUCCESSFUL switch being remembered, and KB-3CBA0D5F the server response. This entry records that a FAILED switch (400 on refresh_token) is remembered as well and signs the buyer out silently."
---
On vcptcore_qa1 a buyer in orgs A and B was signed in to A with the account menu open. Org B's membership was then locked by REST, so the menu still listed B as enabled. Clicking B sent POST /connect/token grant_type=refresh_token organization_id=B, which returned 400 invalid_grant "The specified refresh token is no longer valid." (membership writes revoke tokens). The storefront signed the buyer out with no message. The next code sign-in carried organization_id=B, so B had been stored as last-used even though the switch failed. The same happened for a switch into an org removed from the contact mid-session: the next sign-in sent that org. In theme source, useUser.switchOrganization awaits useAuth.refresh(), which catches the error, resets tokens and emits unauthorized without rethrowing. switchOrganization then writes localStorage organization-id-<userName> = target unless authErrors is set. Server-side fallback (VCST-6132) makes the next sign-in land on an accessible org anyway.
