---
id: KB-9748CEFF
subject: BL-NOTIF-007 A user-input error in a template must not surface as a server fault
plane: normative
status: active
refutableBy: observation
appliesTo:
anchors:
evidence:
  - method: observation
    at: 2026-09-17T15:21:16.665Z
    by: session:773c585d
    from: sources/business-logic-2026-09-17.md
    attested: false
    whyNot: transcribed from sources/business-logic-2026-09-17.md; nobody has yet watched this rule hold on a deployment
---

BL-NOTIF-007: A user-input error in a template must not surface as a server fault `[P2-ux]`

- **Rule:** An unparseable template body or unparseable sample document is client input, not a server error. A render/preview request carrying such input must be answered with a client-error status and a parse diagnostic — never a 5xx — and the diagnostic must be reported inline in the authoring surface, next to the input that caused it. An **empty** optional sample document is valid input, not a parse error. Every such message, including format/validate failure dialogs, is localized like the rest of the surface.
- **Verify:** Enter an unterminated template expression → trigger the preview/render request → assert the response status is a 4xx carrying a parse diagnostic (not a 5xx) and that the diagnostic renders inline in the preview pane. Clear the sample document entirely → assert no invalid-input indicator appears. Invoke the format action on malformed input → assert the dialog title and body resolve from the locale files, and repeat under a second UI language to confirm they change.
- **Violation signal:** The render endpoint returns `500 Internal server error` for a template the user is part-way through typing; a parse failure is reported only as a generic server error with no indication of which token failed; an empty sample document is flagged as invalid JSON; a format-failure dialog shows a hardcoded English title or message under a non-English UI.
- **Agents:** qa-backend-expert (render endpoint status + payload), qa-testing-expert (inline diagnostic, localization sweep)
- **Docs:** PlatformUserGuide → Notifications → Notification Templates (Preview step): "In case of errors, you will see a detailed report on them." — the inline detailed-report contract is documented; the HTTP status class itself is an implementation detail the guides do not narrate (bl-audit-criteria §1a class 1).
- **Source:** vc-module-notification `Controllers/NotificationsController.cs` `RenderingTemplate` (:106-124, `POST api/notifications/{type}/templates/{language}/rendercontent`) calls `_notificationTemplateRender.RenderAsync` with no input validation and no `try`/`catch`, and the controller declares no `BadRequest`/validation-problem path at all — a parse exception raised by user-authored template text propagates out of the action and becomes a 500. Client side, `Scripts/blades/notifications-edit-template.js` `updatePreview()` (:136-164) does surface the failure inline via `blade.previewError` (rendered at `notifications-edit-template.tpl.html` :101-106), but `formatSampleJson()` (:244-251) and `formatHtml()` (:278-291) raise dialogs with hardcoded English `title: 'JSON Error'` / `'HTML Error'` / `'Cannot format: …'`; `blade.isSampleValidJson()` (:661-670) correctly treats an empty sample as valid, so an empty-document "invalid" report originates in the lint gutter enabled at (:198-201), not in the module's own validity check.
- **Promoted:** 2026-07-29 (auto-applied, triangulated — BL-AUDIT-2026-07-29; MISSING → new invariant. Docs cover the inline detailed-report contract (status class N/A per §1a class 1); source + live agree — the render action has no validation or catch path, and live-OBSERVED an unterminated expression returning 500 with the inline hint present, English-only format dialogs, and an empty sample flagged invalid. A further clause — "error chrome must not occlude the surface's own controls" — is deliberately **excluded**: it lacked a platform source anchor this run and is held as a draft. Tracker: VCST-5610 / 5611 / 5612 / 5614.)


---
