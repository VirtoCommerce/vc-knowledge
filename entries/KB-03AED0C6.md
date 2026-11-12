---
id: KB-03AED0C6
subject: Admin contact blade 'Organization memberships' grid shows Organization / Roles / Invite status; the lock state is shown only in the membership detail blade as 'Locked state', toggled by Lock/Unlock toolbar buttons with no Save
plane: experiential
question: Where does the Admin SPA show whether a contact's organization membership is locked, and how is it locked or unlocked there?
questions:
  - text: What columns does the Organization memberships widget blade show in Admin?
  - text: Does the Admin memberships grid have a Status column showing Active or Locked?
  - text: Do Lock and Unlock in the Admin membership blade need Save?
status: active
appliesTo:
  - axis: module
    value: customer
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: POST /api/customer/organization-memberships/search
evidence:
  - method: observation
    deployment: vcst_qa
    build: a1bd5cbdd8350608
    conditions: platform=3.1097.0; module:VirtoCommerce.Customer=3.1033.0; role=platform admin
    at: 2026-11-12T09:01:40.549Z
    by: session:9c36bcd2
    who: Aleksandra-Mitricheva
---
Admin as platform administrator, Customers > Contacts, contact with two memberships. Widget 'Organization memberships' (count 2) opens a grid with columns Organization, Roles, Invite status (values 'Approved'); there is no Active/Locked status column. Clicking a row opens the membership detail blade: fields Organization, Roles, Invite status select, and a read-only 'Locked state' line (Unlocked / Locked) plus 'Locked until'. Toolbar: Save, Reset, Lock (when unlocked) or Unlock (when locked). Lock applies immediately (REST membership read shows isLocked=true, isCurrentlyLocked=true, other membership untouched, global account /users/{id}/locked = false); Unlock likewise applies immediately with no Save. The grid's Invite status column stays 'Approved' throughout.
