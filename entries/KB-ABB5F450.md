---
id: KB-ABB5F450
subject: Admin Notification list search matches the notification type key, not the localized display name
plane: experiential
question: does the Admin SPA Notifications > Notification list keyword search match the localized (e.g. German) display name
status: active
appliesTo:
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: /workspace/notifications
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-09-30T15:03:02.315Z
    by: session:0cfc9f97
    who: kutasinaelena
---
With the Admin UI in German, Notifications > Notification list search 'Rück' returns 0 rows while 'Return' returns the 5 return notifications, whose display names read 'Rückgabe genehmigt', 'Rückgabe storniert', 'Rückgabe teilweise genehmigt', 'Rückgabe registriert', 'Rückgabe abgelehnt'. The keyword is matched against the type/English name, not the localized display name.
