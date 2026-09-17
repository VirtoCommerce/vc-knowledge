---
id: KB-CD13F8FB
subject: BL-CROSS-011 Graceful degradation when dependent service is down
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:12.403Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-CROSS-011: Graceful degradation when dependent service is down `[P1-data]`

- **Rule:** When a dependent service is unavailable (Elasticsearch down, payment gateway timeout, email service failure, analytics endpoint unreachable), the platform must degrade gracefully: (1) search down → show "Search unavailable" message, catalog browsing via categories still works; (2) payment gateway down → show error at checkout, don't create orphan orders; (3) email down → order still placed, email queued for retry; (4) analytics down → order still placed, events lost (acceptable).
- **Verify:** Simulate each service outage → verify core flow continues or shows meaningful error → no 500 errors, no white screens, no data corruption.
- **Violation signal:** White screen / 500 error when a dependent service is down; orphan orders created when payment fails; order blocked because email service is down; silent data loss without logging.
- **Agents:** qa-backend-expert (API resilience), qa-testing-expert (fault injection), qa-frontend-expert (error UX)
