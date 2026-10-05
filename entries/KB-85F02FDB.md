---
id: KB-85F02FDB
subject: Admin Return blade field set and Line items columns for an admin-created return
plane: experiential
question: Which fields does the Admin SPA Return blade and its Line items blade show for a return created via Add new return?
questions:
  - text: What fields does an admin see when creating a product return in the back office?
  - text: /api/return Add new return blade fields Line items columns
  - text: Which statuses does the Status dropdown offer for a New return in the Admin SPA?
  - text: Is Buyer's reference filled for a return created via Returns > Add new return?
  - text: Is the Approved column pre-filled on an admin-created return's Line items blade?
concepts:
  - id: return
  - id: return-status
  - id: admin-order-screen
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /api/return
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-02T06:03:41.372Z
    by: session:0cfc9f97
    who: kutasinaelena
---
A return created in the Admin SPA via Returns > Add new return > Customer orders picker > Line items > Make return opens at once with status New. Its blade shows Return number, Order number, Created date, Modified date, Created by, Customer, Status (dropdown), Buyer's reference (empty for an admin-created return; API customerReference null), Resolution and Reason for decline, plus a Line items widget showing the line count and total. The Line items blade (toolbar Refresh, Save, Approve / decline) has columns Item name, Return reason, SKU, Quantity, Approved, Decline reason, Price; Approved is pre-filled with the requested quantity. Status dropdown for a New return offers New, Completed, Cancelled, Processing; choosing Cancelled + Save persists (PUT /api/return 200).
