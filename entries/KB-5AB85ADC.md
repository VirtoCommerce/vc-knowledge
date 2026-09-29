---
id: KB-5AB85ADC
subject: Sales-rep list share notification carries no organization name; a typed rep message replaces the default body
plane: experiential
question: what does the notification sent when a sales rep shares a list with a customer organization contain
status: active
appliesTo:
  - axis: feature
    value: sales-rep-list-sharing
  - axis: surface
    value: storefront-ui
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
---
On theme 2.59.0-pr-2476, saving the Share dialog with new Customer targets issues Mutation.sendCustomerCommunication per newly added organization with organizationIds=[that org], sendEmail/sendPush true, title 'A new list from your sales representative' and message either the rep's typed note or, when empty, 'Hi! I've just shared the list "<name>" with your organization. Take a look:', followed by a blank line and the /shared-list/<key> URL. Neither title nor message contains the organization name. The recipient's Notifications dropdown shows the message and the link. A whitespace-only note is trimmed client-side and sent/saved as empty.
