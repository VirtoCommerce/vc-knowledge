---
id: KB-CDFF053B
subject: BL-AUTH-011 Stop Impersonation must restore operator session without sign-in round-trip
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: POST /connect/token
evidence:
  - method: observation
    at: 2026-09-17T15:21:05.959Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-011: Stop Impersonation must restore operator session without sign-in round-trip `[P1-data]`

- **Rule:** Stopping impersonation must restore the original operator's authenticated session **without a sign-in round-trip** — no redirect to the sign-in route and no re-authentication prompt. The action is the **"Back to operator" row inside the account-menu popup** (`data-test-id="back-to-operator-row"`, label "Back to {operator name}"), NOT a button in the banner. It calls `useImpersonate().backToOperator()` → `revertImpersonate(...)` → `requestImpersonateToken("", ...)`, which POSTs `/connect/token` with `grant_type=impersonate` and an **empty `user_id`** — minting a **fresh operator session** (never `grant_type=password`). The restored operator tokens are written to storage and **then** the tab performs a full-page navigation (`location.href`) to the operator landing route (other tabs reload via broadcast). Because the operator token is persisted **before** the reload, no re-auth occurs. (vc-frontend source: `useImpersonate` `backToOperator`/`revertImpersonate`; account-menu popup row `back-to-operator-row`.)
- **Verify:** Operator starts impersonating target → confirms banner → clicks the "Back to operator" row (`data-test-id="back-to-operator-row"`) → the `/connect/token grant_type=impersonate` call carries an **empty `user_id`** (no `grant_type=password`); the URL does **NOT** go to the sign-in route; after the navigation the account menu shows the operator name (not target).
- **Violation signal:** Stopping impersonation redirects to the sign-in route (operator must re-authenticate); account menu shows "Sign in" instead of operator name (session lost); a `grant_type=password` call is made. NOTE: a full-page navigation to the operator landing route **is expected by design** — it is NOT a violation (the operator token is persisted before the reload, so the session survives).
- **Applies to:** IMP-012 (suite 082-auth-impersonation). Any regression that touches the impersonation token-stack restore logic.
- **Agents:** qa-frontend-expert (storefront stop-impersonation handler), qa-backend-expert (operator token re-activation)
- **Source:** `vc-frontend` `client-app/shared/account/composables/useImpersonate.ts` — `backToOperator()` → `revertImpersonate(<company-members landing route>)` → `requestImpersonateToken("")`, which POSTs the token endpoint with `grant_type=impersonate`, `scope=offline_access` and an **empty** `user_id`, writes all four token values to storage, and only **then** broadcasts and performs the full-page navigation (the ordering is called out as an invariant in the source comment). Control row `back-to-operator-row` in `client-app/shared/layout/components/header/_internal/top-header.vue`. Live-confirmed on the environment: the captured request body was exactly `grant_type=impersonate&scope=offline_access&user_id=` → HTTP 200, no password grant occurred, the tab landed on the company-members route rather than sign-in, and the account menu showed the operator with no impersonation banner and no re-authentication prompt. Docs axis: N/A — the token-grant and reload mechanics have no published guide coverage.
- **Amended:** 2026-08-05 (auto-applied, triangulated — BL-AUDIT-2026-08-05). Rule unchanged (every clause held, including the expected full-page navigation); the source anchor and the live request-body evidence were recorded.
