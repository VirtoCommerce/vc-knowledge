---
id: KB-67DFD94B
subject: BL-AUTH-007 Storefront logout UX — popup-only `[P1-ux]` `[GOLDEN RULE]`
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
  - coordinate: /sign-out
  - coordinate: /logout
evidence:
  - method: observation
    at: 2026-09-17T15:21:05.410Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-007: Storefront logout UX — popup-only `[P1-ux]` `[GOLDEN RULE]`

- **Rule:** The storefront exposes logout **only** inside the account-menu popup in the top header. There is no `/sign-out` page, no `/logout` page, and no standalone logout icon in the header. Correct sequence: (1) click the account button `data-test-id="account-button"` in the top-right header — opens the account-menu popup (`data-test-id="account-menu"`); (2) click the logout button **inside that popup**, selector `data-test-id="sign-out-button"`. Note the storefront test attribute is `data-test-id` (hyphenated) with the **flat** value `sign-out-button` — `data-testid` is not used in vc-frontend and there is no dotted-path value. (vc-frontend source: `client-app/shared/layout/components/header/_internal/top-header.vue`; composable `useSignMeOut`; routes `client-app/router/routes/main.ts` + `constants.ts` confirm no `/sign-out` or `/logout` route — logout calls `signMeOut` which clears the session and reloads through the auth guard.)
- **Verify:** Navigating to `/sign-out` and `/logout` must not resolve to a logout page (they fall through to the catch-all 404). Header nav must not contain a top-level logout button. Clicking `data-test-id="account-button"` opens the popup; clicking `data-test-id="sign-out-button"` inside it signs the user out, and the auth guard redirects to the sign-in route carrying a `returnUrl` for the page that was open. The popup control is an **icon-only** button — its visible content is a logout glyph and its accessible name/title is the localized "Logout" label — so locate it by `data-test-id="sign-out-button"`, never by visible text.
- **Violation signal:** A `/sign-out` route renders a page; a header-level logout button exists; logout works only via a URL (no popup); the popup logout selector `data-test-id="sign-out-button"` (inside `data-test-id="account-menu"`) is missing.
- **Applies to:** All test cases whose Steps say "sign out", "log out", "Click logout button", or "Navigate to /sign-out" — agents MUST execute the popup sequence and reviewers MUST reject the loose/wrong Step text in favor of the popup sequence.
- **Agents:** qa-frontend-expert (storefront), qa-testing-expert (execution), test-management-specialist (CSV review)
- **Source:** `vc-frontend` `client-app/shared/layout/components/header/_internal/top-header.vue` — `account-button` → `account-menu` popup → `sign-out-button` (an icon-only button titled with the localized logout label) wired to `signMeOut` from `useSignMeOut`; `client-app/router/routes/main.ts` declares only a sign-in page and a repo-wide search of the router directory finds no sign-out/logout route. Live-confirmed on the environment: both URLs render the 404 catch-all, the authenticated header carries no logout control, and the popup control signs the user out through the auth guard. Docs axis: N/A — the storefront user guide has no sign-out topic, and the invariant's substance is the route-absence + selector contract (QA methodology).
- **Amended:** 2026-08-05 (auto-applied, triangulated — BL-AUDIT-2026-08-05). Rule unchanged; added the source/live anchor and the icon-only-control precision to `Verify`.
