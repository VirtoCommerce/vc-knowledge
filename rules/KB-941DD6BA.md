---
id: KB-941DD6BA
subject: BL-SR-029 Keyboard grab-and-move announces every transition via `aria-live`, with position for reorder and without position for park/restore; however a grab ended by a pointer interruption is a tracked violation
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:32.650Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-SR-029: Keyboard grab-and-move announces every transition via `aria-live`, with position for reorder and without position for park/restore; however a grab ended by a pointer interruption is a tracked violation `[P2-ux]`

- **Rule:** Grabbing a block for keyboard reordering, moving it, and dropping it all announce via `aria-live`, including position (e.g. "position N of M") for reorder moves and edge cases; park and restore append to the end of their target zone and announce without a position — this asymmetry is intentional. Ending a grab must always restore the pre-grab state and announce the cancellation, regardless of what ends it — an explicit keyboard cancel (Escape / blur / tab-out) or any other interaction that terminates the grab.
- **Verify:** Grab a block via keyboard, move with arrow keys → announcements include position; drive a block to an edge → the edge announcement is distinct; park then restore via keyboard → announcements omit position; end a grab via Escape → the pre-grab position is restored and the cancellation announced; end a grab via any other interaction (e.g. a pointer action on another block) → the pre-grab position must still be restored and the cancellation still announced.
- **Violation signal:** A reorder or edge announcement omits position; a park/restore announcement includes one; ending a grab by any means other than Escape/blur/tab commits the moved position without restoring it or announcing the cancellation.
- **Agents:** qa-frontend-expert
- **Source:** module keyboard-sort composable (`moved`/`edge` signal payloads carry position; `parked`/`restored` carry only an id). Live-confirmed: eight verbatim announcement strings captured across grab/move/edge/park/restore/drop; Escape correctly restored on all three regions and a neutral-area click also restored correctly. The same live pass also found a **pointer press on another draggable block while a grab is live silently ends the grab without restoring the pre-grab position and without an announcement**, committing the in-progress move by accident — a confirmed violation of this Rule, tracked as a separate defect rather than reflected as intended behavior.
- **Promoted:** 2026-08-04 (BL-AUDIT-2026-08-04 re-audit; source + live CONFIRM; docs N/A). **Note:** the Rule is stated as the full intended contract — restore-and-announce on ANY grab-ending interaction — precisely so the pointer-interrupt path stays a tracked violation rather than being silently blessed as acceptable behavior once fixed.
