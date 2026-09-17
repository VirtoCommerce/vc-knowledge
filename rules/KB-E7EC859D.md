---
id: KB-E7EC859D
subject: BL-PLAT-002 Account lock or deletion is authoritative over every credential and session tied to it
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:37.100Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-PLAT-002: Account lock or deletion is authoritative over every credential and session tied to it `[P1-data]`

- **Rule:** A user's Locked or Deleted account state overrides every authentication surface bound to that account, regardless of that credential's own individual flag. An API key's own active flag is not sufficient to authenticate — the owning account must also exist, be allowed to sign in, and not be locked out; a locked or deleted owning account causes API-key authentication to fail even though the key itself is active. Locking (setting a non-empty lockout end) or deleting a user account immediately terminates every active session and token issued to that account — a currently-signed-in session does not survive an administrator locking or deleting the account.
- **Verify:** Generate an active API key for a test user → confirm it authenticates. Lock the user's account → the same active key now fails authentication. Delete the account → the key fails authentication. Separately: sign in as a test user in one session, then as an administrator lock (or delete) that account from a second session → the first session's tokens are revoked and further requests are rejected.
- **Violation signal:** An active API key still authenticates after its owning account is locked or deleted; a signed-in user's session or token remains valid after an administrator locks or deletes their account.
- **Agents:** qa-backend-expert (Admin SPA Security → Users, API-key widget, REST auth), qa-testing-expert (lock/unlock + API-key interaction)
- **Docs:** PlatformDeveloperGuide API Key Authentication — "each API key must be associated with a user account, as all requests with an API key will be authorized on behalf of the user"; Passwords Management — lockout duration is configurable.
- **Source:** vc-platform `src/VirtoCommerce.Platform.Web/Security/Authentication/ApiKeyAuthenticationHandler.cs` `HandleAuthenticateAsync()` — after resolving an active key it still calls `FindByIdAsync`, `CanSignInAsync(user)` and `IsLockedOutAsync(user)`, failing authentication on any of those checks; `src/VirtoCommerce.Platform.Security/Handlers/RevokeTokenUserChangedEventHandler.cs` `Handle()` — on a `UserChangedEvent`, revokes all sessions via `TerminateAllUserSessions` whenever `LockoutEnd` becomes non-empty or the entry is Deleted.
- **Amended:** 2026-08-24 (auto-applied, triangulated — BL-AUDIT-2026-08-24; MISSING → new entry. Docs + Source agree and are dispositive; the live axis rests on a previously-captured lock/unlock verification of the same mechanism rather than a fresh run — re-exercise on the next pass.)
