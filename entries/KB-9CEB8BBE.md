---
id: KB-9CEB8BBE
subject: Query.carts filter terms (customerIds, customerOrOrganization, organizationId) cannot widen the signed-in buyer's cart scope on the VCST-6251 fixed build
plane: experiential
question: Can a buyer read another customer's or another organization's carts by putting customerIds, organizationId or customerOrOrganization:true in the Query.carts filter?
questions:
  - text: Does the xAPI carts filter argument override the userId/organization scope?
  - text: Does customerOrOrganization:true in Query.carts return colleagues' organization carts?
  - text: Is cart isolation per organization enforced when the carts filter names another org?
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: Query.carts
evidence:
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.XCart=3.1043.0-pr-151-07b3; store=B2B-store; role=storefront buyer, no org role
    at: 2026-11-04T08:15:12.294Z
    by: session:7c4c844c
    who: Lenajava1
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.XCart=3.1043.0-pr-151-07b3; store=B2B-store; role=storefront buyer password grant
    at: 2026-11-04T09:52:48.940Z
    by: session:322b37a0
    bodyHash: 70c61358e01c
    who: Lenajava1
    note: "2026-11-04 independent re-run, 2 runs each: victim cart created in-case for a TechFlow buyer (owner sees it by name filter). AcmeCorp buyer with filter \"customerOrOrganization:true customerIds:<victim>\" and \"customerIds:<victim>\" got only his own 1 cart; a no-organization buyer with \"customerOrOrganization:true organizationId:<TechFlow>\" got only his own 2 carts. name:<own cart name> narrowed to exactly the one cart; a non-matching name returned 0."
  - method: observation
    deployment: vcst_qa
    build: 4fafc097f6a88f6c
    conditions: module:VirtoCommerce.XCart=3.1043.0-pr-151-07b3; store=B2B-store; role=storefront buyer (org-scoped)
    at: 2026-11-04T10:01:45.750Z
    by: session:322b37a0
    bodyHash: 70c61358e01c
    who: Lenajava1
    note: "2026-11-04: caller AcmeCorp buyer, victim = a personal cart (organizationId null) of a no-organization buyer: filters \"customerOrOrganization:true customerIds:<victim>\" and \"customerIds:<victim>\" returned exactly the caller's own total, none of the victim's. Victim = a cart of a distinct AcmeCorp colleague (cart organizationId = caller's): \"customerOrOrganization:true\" and \"customerOrOrganization:true organizationId:<caller org>\" also returned only the caller's own total."
---
On vcst_qa (XCart 3.1043.0-pr-151-07b3, the VCST-6251 fix), buyers signed in with a password-grant storefront token (storeId B2B-store, organization_id = own org) called Query.carts(storeId, userId = self, cultureName, currencyCode, filter). Filters "customerOrOrganization:true customerIds:<other org's buyer>", "customerOrOrganization:true organizationId:<other org>", "customerIds:<other buyer>" and "customerOrOrganization:true" returned only the caller's own carts and wishlists (2 of 2, same as unfiltered); a buyer with no organization using "customerOrOrganization:true organizationId:<other org>" got only his own cart. Repeated 3 times, stable. Legit terms still narrow: name:default -> the default cart only, name:<list name> -> that wishlist only, status:Ordered -> 0. On the pre-fix XCart 3.1044.0 (vcptcore) the same calls leaked: customerOrOrganization:true returned a same-org colleague's cart, and the no-org buyer's organizationId:<other org> filter returned that org's carts.
