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
---
On theme 2.59.0-pr-2476, saving the Share dialog with new Customer targets issues Mutation.sendCustomerCommunication per newly added organization with organizationIds=[that org], sendEmail/sendPush true, title 'A new list from your sales representative' and message either the rep's typed note or, when empty, 'Hi! I've just shared the list "<name>" with your organization. Take a look:', followed by a blank line and the /shared-list/<key> URL. Neither title nor message contains the organization name. The recipient's Notifications dropdown shows the message and the link. A whitespace-only note is trimmed client-side and sent/saved as empty.
