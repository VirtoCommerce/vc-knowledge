---
id: KB-BEE80DB4
subject: The /connect/authorize consent page names the client, the signed-in account, the grants and the return target; Cancel returns error=access_denied with no code, and a form opened under one account and submitted after a switch to an account with no stored consent shows "This page expired, so nothing was allowed yet"
plane: experiential
question: What does the MCP OAuth consent page at /connect/authorize show, and what do Cancel and a stale form do?
questions:
  - text: What happens when the user clicks Cancel on the MCP consent page?
  - text: When does the consent page say the page expired?
  - text: Is consent remembered after Allow, and removed by Disconnect?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /connect/authorize
  - coordinate: /admin/mcp
evidence:
  - method: observation
    deployment: vcst_qa
    build: 01d892bf697c7bac
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74
    at: 2026-11-10T07:38:15.330Z
    by: session:d6dc0a6d
    who: Lenajava1
---
Authorization code + PKCE for client claude-admin-qa, resource {BACK_URL}/admin/mcp: an unauthenticated user is sent to the platform login with ReturnUrl and then gets the consent page "Claude — Back Office (QA) wants to access your account". It shows the account, the grants (Work with your permissions — everything your account is allowed to do at <host>/admin/mcp; Know who you are; Stay signed in until you revoke access), "After you allow, you go back to localhost:<port>, an app on this computer", and Technical details (Client ID, Resource, Redirect URI, Scopes). The form posts to /connect/authorize with __RequestVerificationToken. Cancel redirects with error=access_denied (OpenIddict ID2015) and no code, and consent is not remembered. Allow issues a code; the token has aud = the endpoint, client_id and sub = the user. Consent is remembered after Allow, so a later authorize issues a code without a page, and Disconnect forgets it. Signing out then Allow on an open form sends the user to login. Re-signing in as the SAME user then Allow succeeds. Signing in as ANOTHER user with no stored consent, then Allow on the first user's form, shows the alert "This page expired, so nothing was allowed yet. Review the access and choose again." with the current account, and Allow then works. Switching to another user who already HAS stored consent, then Allow on the first user's form, issued a code for that other user with no notice, although the page named the first account.
