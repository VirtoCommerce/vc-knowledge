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
    at: 2026-09-15T10:00:00.000Z
    by: round1-arm-C
---

They are the same instant in two timezones. Round one's arm C read Sep 15, 2026 5:51:39 AM on the Admin order screen for the instant the REST payload reports as 2026-09-15T09:51:39.6897076Z - a four-hour offset, that session's browser being at UTC-4. Admin renders in the viewer's local time and prints no offset with it; the API returns UTC with an explicit Z. So comparing a timestamp off a screenshot with one off a payload, without converting, shows a discrepancy of however many hours the reader happens to be from UTC, and it is not a discrepancy. This bites hardest when a screenshot is the evidence in a bug report and the reader is in a different timezone from the person who took it.
