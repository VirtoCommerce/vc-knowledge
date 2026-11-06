---
id: KB-BBEA0B0D
subject: "POST /api/vcmp/message/unreadcount for an operator with no seller selected returns 200 with the operator group's unread count for the entity; the route is POST-only (GET is 405, Allow: POST)"
plane: experiential
question: What does /api/vcmp/message/unreadcount return for a platform admin (operator) in the Vendor Portal with no vendor selected?
questions:
  - text: Does the Vendor Portal unread message count still return 500 for admin without a vendor?
  - text: Is GET /api/vcmp/message/unreadcount a valid route?
  - text: Whose unread count does unreadcount return when the caller is an operator and no sellerId is sent?
  - text: What does unreadcount return with an empty body?
status: active
appliesTo:
  - axis: app
    value: vendor-portal
  - axis: role
    value: platform-admin-operator
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/vcmp/message/unreadcount
  - coordinate: GET /api/vcmp/message/unreadcount
evidence:
  - method: observation
    deployment: vcmp_dev
    build: 91519219cc049919
    conditions: platform=3.1071.0; module:VirtoCommerce.MarketplaceCommunication=3.1001.0-alpha.220; module:VirtoCommerce.Communication=3.1001.0-alpha.72; vc-shell=2.11.0; vendorSwitcher=none
    at: 2026-11-06T11:57:12.593Z
    by: session:dd3ae5fa
    who: Lenajava1
---
Signed in to the Vendor Portal as the platform admin with no vendor in the Vendor Switcher, POST /api/vcmp/message/unreadcount {entityId, entityType} returned 200 with a bare integer on every call across 4 fresh page loads (offers, an order, seller products). The value equals the unreadMessagesCount that POST /api/vcmp/conversation/search reports for the operator group's communication user (GET /api/vcmp/communicationuser/operator, 'Operator org') for the same entity's conversation: 5 of 5 entities matched (3, 2, 1, 6, 5); entities without a conversation return 0. With sellerId of a vendor in the body it also returns 200 (seller path). An empty body returns 400 with EntityId/EntityType required validation errors. GET on the same route returns 405 with Allow: POST - the controller only declares HttpPost, so a GET was never a valid call. The Vendor Portal UI on this build (vc-shell framework 2.11.0) does not call this endpoint at all; the blade Communication badge uses POST /api/vcmp/message/search with unreadOnly=true instead.
