---
id: KB-7E35E6BC
subject: Admin renders order timestamps in local time while the API returns UTC
plane: experiential
question: why does the Admin order screen show a different time from the one in the REST payload
status: active
refutableBy: observation
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: GET /api/order/customerOrders/{id}
arrivesAt:
  - coordinate: /account/orders/{id}
evidence:
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T12:28:48+04:00
    by: session:09e39416
    from: C:/_VIRTO/_comparison-logs/arm-C/report.md
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T12:29:23.274Z
    by: session:8ec21246
    attested: false
    whyNot: confirmed at the end of a pricing task; the run report names no timestamp anywhere. See C:/_VIRTO/_comparison-logs/round4/arm-C-catalog/REPORT.md
  - method: observation
    deployment: vcptcore_stable
    pin: c2f9c438eba4cd95
    platformVersion: 3.1007.26
    at: 2026-09-16T17:25:12.160Z
    by: session:26f59771
    contradicts: true
    note: "The offset is not the browser's. Admin renders order timestamps in the timezone stored on the operator's PLATFORM PROFILE: GET /api/platform/profiles/currentuser returns VirtoCommerce.Platform.UI.TimeZone=America/New_York, and the order blade printed Sep 15 2026 4:17:18 AM for a createdDate of 2026-09-15T08:17:18.308Z and Sep 10 2026 3:51:56 PM for 2026-09-10T19:51:56Z - both UTC-4 - while this machine's browser is Asia/Yerevan, UTC+4 (would have been 12:17 PM and 11:51 PM). Proof the browser is not the source: a cold deep link to the same blade, which loads before the profile does, printed the same instant as 2026-09-15 PM12:17:18 in the browser's zone and a ja-JP format. So two operators see different clock times on one order according to a per-user setting, and the same blade also prints its indexation widget in raw UTC (Indexed Sep 15 2026 8:21:52 am against modifiedDate 08:21:51Z)."
---

They are the same instant in two timezones. Round one's arm C read Sep 15, 2026 5:51:39 AM on the Admin order screen for the instant the REST payload reports as 2026-09-15T09:51:39.6897076Z - a four-hour offset, that session's browser being at UTC-4. Admin renders in the viewer's local time and prints no offset with it; the API returns UTC with an explicit Z. So comparing a timestamp off a screenshot with one off a payload, without converting, shows a discrepancy of however many hours the reader happens to be from UTC, and it is not a discrepancy. This bites hardest when a screenshot is the evidence in a bug report and the reader is in a different timezone from the person who took it.

**Disputed.** The offset is not the browser's. Admin renders order timestamps in the timezone stored on the operator's PLATFORM PROFILE: GET /api/platform/profiles/currentuser returns VirtoCommerce.Platform.UI.TimeZone=America/New_York, and the order blade printed Sep 15 2026 4:17:18 AM for a createdDate of 2026-09-15T08:17:18.308Z and Sep 10 2026 3:51:56 PM for 2026-09-10T19:51:56Z - both UTC-4 - while this machine's browser is Asia/Yerevan, UTC+4 (would have been 12:17 PM and 11:51 PM). Proof the browser is not the source: a cold deep link to the same blade, which loads before the profile does, printed the same instant as 2026-09-15 PM12:17:18 in the browser's zone and a ja-JP format. So two operators see different clock times on one order according to a per-user setting, and the same blade also prints its indexation widget in raw UTC (Indexed Sep 15 2026 8:21:52 am against modifiedDate 08:21:51Z). — observed on `vcptcore_stable`.
