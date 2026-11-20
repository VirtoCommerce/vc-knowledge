---
id: KB-7BA7FEC6
subject: The OAuth applications form (classic Manager and Admin Shell) has client type, PKCE, consent, grant types, scopes and an MCP resources picker, but no AI-agent template
plane: experiential
question: Can an administrator set up an AI agent OAuth client from the back office OAuth applications screen, and is there an 'Add AI agent' template?
questions:
  - text: Does the OAuth applications form offer a PKCE switch and MCP resources?
  - text: Is there a template for Claude Code or ChatGPT clients in the back office?
  - text: Which grant types and scopes does a new OAuth application get by default?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /api/platform/oauthapps
evidence:
  - method: observation
    deployment: vcst_qa
    build: 957a892645884c54
    conditions: platform=3.1099.0; role=Administrator
    at: 2026-11-20T10:51:18.643Z
    by: session:74ee3083
    who: Lenajava1
---
Signed in to the vcst back office as an administrator, Security > OAuth applications in the classic Manager and in Admin Shell lists the apps with Refresh / Add / Delete only. Add opens a plain "New application" / "New OAuth application" form: Client type (Confidential / Public, fixed after creation), Application type, Consent (Explicit), a Require PKCE switch, Grant types (Authorization code, Refresh token, Client credentials checked; Password unchecked), Scopes (email, profile checked; roles unchecked; openid and offline_access always allowed), a Resources picker listing the platform's MCP endpoints (/admin/mcp, /ucp/mcp) plus other resources, Redirect URIs and Post-logout redirect URIs; it generates a client secret shown once. There is no "Add AI agent" entry or agent template (Claude Code, claude.ai, ChatGPT, custom) in the toolbar or the form in either UI. Nothing was saved.
