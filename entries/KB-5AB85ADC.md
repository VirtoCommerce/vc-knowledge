---
id: KB-5AB85ADC
subject: Sales-rep list share notification carries no organization name; a typed rep message replaces the default body
plane: experiential
question: what does the notification sent when a sales rep shares a list with a customer organization contain
questions:
  - text: What does the message say when my account manager shares a product list with our company?
  - text: Does the list-share notification to a customer organization mention that organization's name?
  - text: If the rep types a note in the share dialog, does it replace the default notification text?
  - text: Is one customer communication mutation sent per newly added organization, with email and push both enabled?
  - text: What happens to a whitespace-only share note - is it sent as-is or trimmed to empty?
concepts:
  - id: customer-communication
  - id: list-sharing
status: active
appliesTo:
  - axis: feature
    value: sales-rep-list-sharing
  - axis: surface
    value: storefront-ui
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.sendCustomerCommunication
  - coordinate: Mutation.changeWishlist
  - coordinate: /account/lists
evidence:
  - method: observation
    deployment: vcptcore_qa
    at: 2026-09-29T12:22:06.233Z
    by: session:p20808
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore_qa
    at: 2026-10-06T01:06:18.607Z
    by: session:b04eb8d9
    who: Aleksandra-Mitricheva
    contradicts: true
    note: On theme 2.59.0-pr-2476-0abb (sales-rep 3.1012.0-pr-21-8964) saving a Specific-customers share with TWO newly added orgs issues a SINGLE Mutation.sendCustomerCommunication with organizationIds=[orgA, orgB], not one call per organization; members of both orgs received the same push (same push id). Title/message/no-org-name parts of the entry still hold.
    resolved: claim-amended
    resolvedAt: 2026-10-12T17:25:50.674Z
    resolvedBy: session:34990ccd
    resolvedIn: judge/list-sharing-20261012
    resolution: One save sends one sendCustomerCommunication with organizationIds = all newly added orgs (observed on vcst and vcptcore; the storefront code has done so since 2026-09-12), not one call per organization.
    amendedFrom: b1466d2d0ed4
  - method: observation
    deployment: vcst
    conditions: theme=2.60.0-pr-2511-81f1; platform=3.1078.0; module:VirtoCommerce.XCart=3.1041.0; module:VirtoCommerce.Cart=3.1012.0; module:VirtoCommerce.SalesRep=3.1013.0-pr-18-d367; store=B2B-store; role=sales-rep
    at: 2026-10-12T17:25:03.511Z
    by: session:34990ccd
    who: Dan-BV
    note: "Two orgs added in one save: ONE SendCustomerCommunication with organizationIds=[both], sendEmail/sendPush true, title 'A new list from your sales representative', message = typed note or the default 'Hi! I've just shared the list ... Take a look:' + blank line + /shared-list URL; no org name. A later single add notifies only that org; removing one sends nothing. Notifications dropdown shows message + link, no title."
    investigation: true
    bodyHash: 491c95939c80
  - method: observation
    deployment: vcptcore
    conditions: theme=2.60.0-pr-2522-6f90; store=B2B-store; role=sales-rep
    at: 2026-10-12T17:25:05.143Z
    by: session:34990ccd
    who: Dan-BV
    note: "Two orgs added in one save: ONE SendCustomerCommunication with organizationIds=[both], sendEmail/sendPush true, title 'A new list from your sales representative', message = typed note or the default 'Hi! I've just shared the list ... Take a look:' + blank line + /shared-list URL; no org name. A later single add notifies only that org; removing one sends nothing. Notifications dropdown shows message + link, no title."
    investigation: true
    bodyHash: 491c95939c80
---
On theme 2.60.0-pr-2511-81f1 (vcst) and 2.60.0-pr-2522-6f90 (vcptcore), 2026-10-12, saving the Share dialog with newly added Customer targets issues ONE Mutation.sendCustomerCommunication per save, with organizationIds = the newly added organizations only (two added at once: one call carrying both ids), sendEmail and sendPush true, title 'A new list from your sales representative', and message either the rep's typed note or, when empty, 'Hi! I've just shared the list "<name>" with your organization. Take a look:', followed by a blank line and the /shared-list/<key> URL. Neither title nor message contains the organization name. Adding one organization later notifies only that organization; removing one, or a scope change other than to Specific customers, sends nothing. The recipient's Notifications dropdown shows the message and the link with a timestamp, without the title. The single organizationIds call has been the storefront rule since 2026-09-12 (vc-frontend wishlist-customer-sharing.vue notifyCustomers(addedIds), design decision 5 of VCST-5707).
