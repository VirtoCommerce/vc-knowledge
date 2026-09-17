---
id: KB-4CFD39E7
subject: BL-AUTH-013 Org-scoped access refusal is distinct from global lockout, and per-cause
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: POST /connect/token
  - coordinate: /sign-in
evidence:
  - method: observation
    at: 2026-09-17T15:21:06.253Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-013: Org-scoped access refusal is distinct from global lockout, and per-cause `[P1-data]`

- **Rule:** An org-scoped refusal from `/connect/token` MUST carry `error: invalid_grant` plus a **cause-specific `code`** — never a global lockout code (`user_is_locked_out` / `user_is_temporary_locked_out`). There are **two independent refusal axes**, each with its own code(s):
  - **LOCK axis** (`OrganizationMembership.IsLocked` + `LockoutEnd` → `IsCurrentlyLocked`): `user_is_locked_in_organization`.
  - **STATUS axis** (the membership's effective status, per BL-B2B-013) — one code per blocking status: `Invited` → `user_invitation_pending_in_organization`; `Rejected` → `user_is_rejected_in_organization`; `Deleted` → `user_is_removed_from_organization`.
  - A requested organization the user is **not associated with at all** yields `invalid_organization_id`, but **only on a non-`password` grant** (on a `password` grant it is silently substituted — see BL-AUTH-016).

  Every code is the snake_case of its describer method, so a renamed method silently renames the wire contract. The storefront sign-in form AND the org switcher MUST surface org-specific copy (e.g. "…access to this organization has been blocked…"), not the generic global-lockout message. Refusals are subject to BL-AUTH-016's fallback and no-org-resolved conditions — this invariant governs **which code** is returned when a refusal is returned, not **whether** one is.
- **Verify:** For each cause, put a **single-accessible-org** user into that state, then `/connect/token` (`grant_type=password`, a store identifier, and `organization_id` naming that org) → assert `error == "invalid_grant"` and the exact `code` from the table above; assert `errors[]` carries **exactly one** entry whose `code` matches (BL-AUTH-016). Do **not** assert on the response's `count` field — it is the token response's parameter count, not an error count, and reads greater than one on a single-error refusal. Storefront `/sign-in` or an org-switch into the refused org → assert org-specific copy, distinct from the global-lockout copy.
- **Violation signal:** A global lockout code is returned for an org-scoped refusal; two different causes collapse onto the same code (a caller cannot tell "invitation pending" from "removed"); a blocking status returns the LOCK code or vice-versa (see BL-AUTH-016 for the deliberate lock-wins precedence); the storefront shows the generic lockout message.
- **Agents:** qa-frontend-expert (sign-in form, org switcher), qa-backend-expert (token endpoint)
- **Source:** `vc-module-customer` `ErrorDescriber.cs:9-49` (all five describers; each `Code` is the snake_case of its method name, so renaming a method silently renames the wire contract) + `OrganizationIdRequestValidator.cs:118-127` (the status→code switch) and `:61-67` (the unassociated-organization path, gated to non-`password` grants), against the independent global-lockout path at `:38-41` and `:76-85`. Live-confirmed on the environment: each of the three blocking statuses returned its own distinct code from a single-org fixture on an explicit-org `password` grant, with exactly one entry in the error list; the unassociated-organization code was returned on a non-`password` grant and silently substituted on a `password` grant. The storefront-copy half of `Verify` was not re-observed this run. Docs axis: N/A — no published guide covers the token endpoint's org error codes; the contract shipped in the change under audit (waived).
- **Amended:** 2026-08-05 (auto-applied, triangulated — BL-AUDIT-2026-08-05). Rule and code table unchanged; the 2026-08-04 "Rejected / Deleted are source-only" caveat is **retired** — all three status codes were observed live this run. `Verify` gained the response-shape guard.
- **Amended:** 2026-08-04 (auto-applied, triangulated — BL-AUTH-2026-08-04). Extended from the LOCK axis only to **both** axes with the per-status code table, and scoped to "which code" rather than "whether a refusal occurs".
- **Source:** `vc-module-customer` `ErrorDescriber.cs` (all five describers; `Code = nameof(...).ToSnakeCase()`) + `OrganizationIdRequestValidator.cs` `GetStatusError` (the status→code switch) and `HandleUnavailableOrganizationAsync` (the `invalid_organization_id` path). Live: the `Invited` → `user_invitation_pending_in_organization` mapping was observed on the environment; the `Rejected` / `Deleted` codes are **source-only this run** (their live probe needs a single-org fixture at that status driven through an explicit-`organization_id` grant). Docs axis: N/A — no published guide covers the token endpoint's org error codes (waived).
