---
id: KB-5CDF1F98
subject: BL-AUTH-002 Email verification gate
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:04.737Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-AUTH-002: Email verification gate `[P1-data]`

- **Rule:** When `emailVerificationRequired = true` in store settings, newly registered users cannot access protected features (checkout, order history, account management) until they verify their email via the confirmation link. They can still browse the catalog and add items to cart. The verification link must expire after a configurable period.
- **Verify:** Register with `emailVerificationRequired = true` → attempt checkout → blocked with "Please verify your email" message. Click verification link → features unlocked. Test expired link → shows "Link expired, request a new one."
- **Violation signal:** Unverified user completes checkout; no verification prompt; expired link still works; verified status not persisted after sign-out/sign-in.
- **Agents:** qa-frontend-expert (registration + checkout), qa-backend-expert (auth API, store settings)
