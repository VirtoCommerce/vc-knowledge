---
id: KB-BABFCC8C
subject: A BuyOnlinePickupInStore addOrUpdateCartShipment that carries a deliveryAddress but no pickupLocationId fails with DB_UPDATE/SQL and leaves the address stored as the user's last pickup location, so later pickup calls without a pickupLocationId fail too until a valid location is sent once
plane: experiential
question: What happens when addOrUpdateCartShipment is called with the BuyOnlinePickupInStore (Pickup) method, a deliveryAddress and no pickupLocationId?
status: active
appliesTo:
  - axis: shipment
    value: buyonlinepickupinstore
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.addOrUpdateCartShipment
  - coordinate: POST /api/customer-preferences/search
evidence:
  - method: observation
    deployment: vcst_qa
    at: 2026-10-08T22:24:43.191Z
    by: session:391bbdbb
    who: yuskithedeveloper
---
On a signed-in organization buyer (password grant with storeId and organization_id), Mutation.addOrUpdateCartShipment with shipmentMethodCode BuyOnlinePickupInStore, option Pickup, a deliveryAddress and no pickupLocationId returned HTTP 200 with errors[].extensions.codes ["DB_UPDATE","SQL"] and the generic message "Error trying to resolve field 'addOrUpdateCartShipment'", not a validation error. Repeated 6/6, on an empty cart and on a cart with a line, and also with the payload {id, deliveryAddress} against an existing pickup shipment. Although the mutation failed, the customer preference CartShipmentLastAddress.<organizationId>.BuyOnlinePickupInStore (read back via POST /api/customer-preferences/search) was rewritten with the address as JSON (~650 chars) instead of a pickup location id (36 chars). From then on, every pickup call by that user in that organization that omits pickupLocationId fails with the same DB_UPDATE/SQL codes, in any cart (3/3, one of them 33 minutes after the original call). FixedRate shipments are not affected. Sending the pickup method once with a valid pickupLocationId succeeds, resets the preference to the location id, and later no-location pickup calls succeed again by reusing that location. Separate, not fully explained: for 1 to 7 minutes after such a failure on a cart that has a line, other mutations on the same cart (FixedRate shipment, valid-location pickup, changeComment) also returned DB_UPDATE/SQL, and the value from a changeComment call that returned an error was later persisted by the next successful save of that cart. Source (vc-module-x-cart AddOrUpdateCartShipmentCommandHandler) saves pickupLocationId ?? the serialized address under a key per shipment method, then loads the saved value into Shipment.PickupLocationId for the pickup method. That column holds at most 128 characters.
