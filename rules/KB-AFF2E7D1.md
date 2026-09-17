---
id: KB-AFF2E7D1
subject: BL-LOY-014 Mixed Cart order — Admin SPA Line items blade shows per-currency totals independently
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:22.921Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-LOY-014: Mixed Cart order — Admin SPA Line items blade shows per-currency totals independently `[P2-ux]`

- **Rule:** In the Admin SPA, the order **Line items** blade for a mixed-currency order MUST display one totals summary bar per currency (e.g. a USD bar and a PTS bar), and the line items table MUST carry a per-row **Currency** column so each line's currency is unambiguous. Neither currency's totals may be omitted. The order's top-level totals accordion legitimately shows the primary-currency total only — the per-currency split is surfaced in the Line items blade.
- **Verify:** Open the Admin (`{{BACK_URL}}`, `@td(ADMIN_DEFAULT)`), Orders → open a mixed-cart order (one PTS line + one USD line) → open the **Line items** accordion → assert two totals bars (one USD, one PTS) above the table, and a **Currency** column showing PTS for the loyalty line and USD for the cash line.
- **Violation signal:** The Line items blade shows only the primary-currency totals; the points-line total is absent from the summary bars despite PTS line items in the table; the Currency column is missing.
- **Agents:** qa-backend-expert
- **Source:** VCST-5104 Task 4 (Admin UI multi-currency totals), PR vc-module-order #497. UI-observed on the environment 2026-06-24 — `reports/ba/screenshots/vcst-5104/08-admin-order-line-items-split-currency.png` (USD 240.00 / PTS 10.00 bars + Currency column).
- **Promoted:** 2026-06-24 (via `/ba-analyze VCST-5104`).
