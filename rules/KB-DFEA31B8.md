---
id: KB-DFEA31B8
subject: BL-SR-025 The block registry owns structure and region placement; the saved document owns only order and hidden, with unknown types dropped and missing blocks appended
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:31.701Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-025: The block registry owns structure and region placement; the saved document owns only order and hidden, with unknown types dropped and missing blocks appended `[P1-data]`

- **Rule:** Which block types exist and which region each renders in is decided by the frontend's block registry alone — never by the saved document. The document contributes only per-block order and a hidden flag. On load: a persisted block type absent from the registry is dropped silently (no error, no placeholder, no orphan entry); a registered block type absent from the document is appended after the document's own blocks; a registered block whose document region disagrees with the registry's region for that type renders in the registry's region, not the document's. Every such deviation self-heals on the next save — the round-trip payload always matches the registry's current structure.
- **Verify:** Plant a document containing an unregistered block type → received by the client but rendered nowhere, no console error, no gap in the layout; plant a document omitting a registered type → it appears, appended after the document's survivors; plant a document placing a registered block in the wrong region → it renders in its registry region, not the document's; save afterward and reload → the unknown type is gone and every block sits in its registry region.
- **Violation signal:** An unregistered type renders as a blank slot, an error, or a stray entry; a block absent from the document stays missing rather than being appended; a block renders in the document's region rather than the registry's; a save fails to correct a planted deviation.
- **Agents:** qa-backend-expert, qa-frontend-expert
- **Source:** module load-path reconciliation logic (pure function reconciling persisted blocks against the registry). Live-confirmed: two unregistered block types received by the client rendered nowhere; two registered blocks missing from a planted document were appended after the document's survivors; blocks planted in the wrong region were re-homed to their registry region (leaving that region empty); a subsequent save purged the unknown types and reset every block to its registry region, and reload showed no oscillation.
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04 re-audit; source + live CONFIRM; docs N/A).
