---
id: KB-2DB25BC2
subject: Publish all on a Multilanguage Page reports conflicting languages in a role=alert toast that auto-dismisses after about 3.2 s (paused while hovered) and stays English when the shell language is German
plane: experiential
question: How does the page builder shell report languages that Publish all could not publish because of a permalink conflict?
questions:
  - text: How long does the Not published toast stay on screen after Publish all?
  - text: Is the Publish all result message localized in the page builder shell?
  - text: Is the Publish all conflict message announced to screen readers?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/page-builder-multilanguage-pages/{id}/publish-all
  - coordinate: /apps/page-builder-shell
evidence:
  - method: observation
    deployment: vcptcore_qa
    build: ed1246f57f06adbd
    conditions: module:VirtoCommerce.PageBuilderModule=3.1041.0-pr-179-5e6c; module:VirtoCommerce.Pages=3.1013.0; module:VirtoCommerce.Xcms=3.1010.0-pr-28-35cb
    at: 2026-10-29T13:48:32.590Z
    by: session:9b657bc7
    who: Aleksandra-Mitricheva
---
Multilanguage Page with 12 language versions, 8 of them with a standalone Published page of the same culture at the same permalink. Clicking Publish all sent POST /api/page-builder-multilanguage-pages/{rootId}/publish-all, which answered 200 with processedCultureNames plus skipped[{cultureName, reason:"PermalinkConflict"}]. The shell built two toasts client-side: a role=status toast "Published: <cultures>. Skipped (no draft): <cultures or —>." and a role=alert error toast "Not published: ja-JP, nl-NL, pl-PL, pt-PT, it-IT, es-ES, fr-FR, de-DE. Another page is published at this permalink for the same visitors. Publish these languages one by one to replace it." Both were removed from the DOM about 3.2 s after appearing (MutationObserver timing); hovering the toast pauses the timer; a Dismiss notification button exists. With the shell language set to Deutsch, both toasts stayed in English while the permalink warning banner and the unpublish dialog body were German. The toast stack sits top-centre and covers the right blade's Save toolbar button at 1920 and 1280 px while it is shown.
