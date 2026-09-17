---
id: KB-4D7A8B0F
subject: BL-WL-006 Distinct allowed upload types — logo vs favicon
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:26.073Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-WL-006: Distinct allowed upload types — logo vs favicon `[P2-ux]`

- **Rule:** The **Logo** widget accepts **PNG / GIF / SVG**; the **Favicon** widget accepts **PNG / JPG / WEBP**. Other extensions are rejected with a "Filetype error" dialog. The sets are distinct — JPG/WEBP are favicon-only, GIF/SVG are logo-only.
- **Verify:** Upload `.gif` logo → accepted; `.jpg` logo → rejected ("Only PNG, GIF or SVG files are allowed"); `.webp` favicon → accepted; `.svg` favicon → rejected ("Only PNG, JPG, or WEBP files are allowed").
- **Violation signal:** Logo widget accepts JPG/WEBP; favicon widget accepts GIF/SVG; no filetype-error dialog on a disallowed extension.
- **Agents:** qa-backend-expert
- **Source:** `en.WhiteLabeling.json` — logo hint/filter (PNG/GIF/SVG) + favicon hint/filter (PNG/JPG/WEBP). Suite 067 WL-003/004/005.
- **Promoted:** 2026-07-02 (TLC-2026-07-02-2043).

---
