---
id: KB-310EDF3F
subject: BL-SR-004 Money resolves to one currency (`currencyCode` → store default → platform primary) and echoes it
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:26.931Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-004: Money resolves to one currency (`currencyCode` → store default → platform primary) and echoes it `[P1-data]`

- **Rule:** All monetary statistics are converted to a single currency chosen by `currencyCode`, falling back to the store default then the platform primary; the resolved `currencyCode` is echoed back and `formattedAmount` is localized by `cultureName`. Mixed-currency underlying orders are aggregated into that one currency.
- **Verify:** Pass `currencyCode:"EUR"` → response `currencyCode:"EUR"`, `formattedAmount` uses the € symbol; omit → store default. (Live-confirmed 2026-07-23: EUR echoed, `€816.00`.) Assert the echoed code + symbol, **not** a specific converted amount (FX rate is env-dependent — vcst-qa EUR ≈ 1.0).
- **Violation signal:** `currencyCode` not echoed; `formattedAmount` unlocalized; per-currency figures returned unconverted / double-counted.
- **Agents:** qa-backend-expert
- **Source:** module README (Money "converted to `currencyCode` → store default → platform primary"); live probe 2026-07-23.
- **Promoted:** 2026-07-23 (TLC-2026-07-23-1943); restored 2026-07-28.
