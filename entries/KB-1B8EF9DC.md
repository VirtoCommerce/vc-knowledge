---
id: KB-1B8EF9DC
subject: A Punchout OrderRequest is accepted only for lines of a Returned requisition; SupplierPartAuxiliaryID is "{LineItemId};{SessionId}" and the cart is cleared after the order
plane: experiential
question: Which punchout requisition must a cXML OrderRequest on POST /api/punchout/cxml match, and what happens to the cart afterwards?
questions:
  - text: Can an ERP order cart lines that were never sent in a PunchOutOrderMessage?
  - text: What is the format of SupplierPartAuxiliaryID in a Virto PunchOutOrderMessage?
  - text: Why does an OrderRequest answer 'No punchout requisition was returned for the order.'?
  - text: Do unordered cart lines stay in the cart after a punchout OrderRequest?
status: active
appliesTo:
  - axis: feature
    value: punchout-order-request
  - axis: surface
    value: rest
anchors:
  - coordinate: POST /api/punchout/cxml
  - coordinate: Mutation.createPunchoutRequisition
evidence:
  - method: observation
    deployment: vcptcore_qa1
    conditions: platform=3.1090.0; module:VirtoCommerce.Punchout=3.1000.0-pr-6-a79b; module:VirtoCommerce.XCart=3.1043.0-alpha.300-vcst-5888-punchoutorderrequest
    at: 2026-10-28T21:25:45.503Z
    by: session:54733023
    who: kutasinaelena
    note: "distinct: This is about which requisition/session an OrderRequest must match, the line-id format and the cart clean-up — not about the payment amount KB-884B1A23 records."
---
On vcptcore_qa1 with Punchout 3.1000.0-pr-6-2c6c/a79b: createPunchoutRequisition now puts SupplierPartAuxiliaryID = "<cart line id>;<punchout session id>" on every ItemIn. A cXML OrderRequest is accepted only when its lines belong to the requisition of ONE Returned session of the same mapped user, store and configuration: lines from two sessions → 400 "The order items are from different punchout requisitions."; an Active (never returned) session, an unknown session id, another user's session, or a user with no Returned session on that configuration → 400 "No punchout requisition was returned for the order."; a line not in that requisition (incl. a changed SKU, random or empty line id) → 400 "Line N: item '<sku>' is not in the punchout requisition."; with bare line ids (no ;session) the user's LAST returned session is used, so lines of an older requisition are rejected. After a successful order the buyer cart is cleared completely — unordered lines and the ERP addresses are removed (author: temporary until punchout gets its own cart); named carts are untouched. A duplicated line id was answered "not in the punchout requisition" although the author expected "… is used by another line.".
