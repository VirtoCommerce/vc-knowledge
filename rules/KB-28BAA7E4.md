---
id: KB-28BAA7E4
subject: BL-AUTH-010 Impersonation banner must persist across SPA navigation
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:05.820Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-010: Impersonation banner must persist across SPA navigation `[P1-ux]`

- **Rule:** Once an impersonation session is active, the banner `[operator name] + "logged in as" + [Account menu: target name]` must remain visible on every storefront page until the operator explicitly stops the impersonation. The banner must NOT disappear on route changes, modal opens, or async data loads. This includes navigation to: home, category pages, product detail, cart, checkout, account pages, search results, and CMS pages.
- **Verify:** Start impersonation → verify the banner on home → navigate by **clicking internal links** (not reloading) to the catalog, cart, checkout, and account pages → the banner is present at each, and its DOM element is the *same* node throughout (accessibility-tree references are stable across the transitions, not re-created). The banner is not a page-level bar: it is three top-header elements — `data-test-id="operator-name-label"` (the operator), the localized "logged in as" label, and `data-test-id="account-button"` carrying the target's name — rendered inside the persistent `data-test-id="top-header"` layout component, outside the router view.
- **Violation signal:** Banner disappears on any storefront route except the explicit Stop Impersonation action; banner re-renders inconsistently (flicker); banner missing on cart or checkout (revenue-critical pages); banner shows operator/target names that don't match the live session.
- **Applies to:** IMP-011 (suite 082-auth-impersonation). Cross-cutting regression for any new storefront layout/route changes.
- **Agents:** qa-frontend-expert (storefront layout + route persistence)
- **Source:** `vc-frontend` `client-app/shared/layout/components/header/_internal/top-header.vue` — the `v-if="operator"` block renders `operator-name-label` plus the localized logged-in-as label, with the target's name on `account-button`; all inside the persistent `top-header` element, so a route change cannot unmount it. `operator` is supplied by `useUser()`. Live-confirmed on the environment: across four click-driven route transitions (home, catalog, cart, account dashboard) the banner stayed rendered with stable DOM node identity. A mobile-menu counterpart exists in the header's mobile menu component but was **not** verified live — this invariant is asserted for the desktop top header. Docs axis: N/A — the published login-on-behalf guide covers only the Admin-side entry point, not the storefront banner.
- **Amended:** 2026-08-05 (auto-applied, triangulated — BL-AUDIT-2026-08-05). Rule unchanged; `Verify` now names the three banner elements and requires DOM-identity (not just presence) across click-driven SPA transitions, and the mobile surface is explicitly out of scope.
