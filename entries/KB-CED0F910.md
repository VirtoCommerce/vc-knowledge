---
id: KB-CED0F910
subject: Platform OpenIddict server keeps the library default code challenge methods (plain + S256) and AuthorizationController.Authorize has no prompt=login branch; neither is an appsettings option
plane: experiential
question: Why does /connect/authorize accept plain PKCE and ignore prompt=login, and can a deployment turn either off by configuration?
questions:
  - text: Is code_challenge_method=plain configurable in the Virto platform OAuth server?
  - text: Where is prompt=login handled in the platform authorize endpoint?
  - text: Can appsettings Authorization options restrict PKCE to S256?
  - text: Does max_age force re-authentication on /connect/authorize?
status: active
appliesTo:
  - axis: platform
    value: 3.1096.0
  - axis: surface
    value: rest
anchors:
  - coordinate: GET /connect/authorize
  - coordinate: GET /.well-known/openid-configuration
  - coordinate: POST /connect/session
evidence:
  - method: source
    deployment: vc-platform@3.1096.0
    defect: suspected
    at: 2026-11-10T15:56:40.834Z
    by: session:40688c0a
    who: Lenajava1
---
Read at vc-platform tag 3.1096.0 (afd70de8). Startup.cs AddServer calls AllowAuthorizationCodeFlow and never touches OpenIddictServerOptions.CodeChallengeMethods; in OpenIddict 7.7.2 that set is initialised to {plain, S256} (OpenIddictServerOptions.cs), and the authorize validation accepts a missing method whenever plain is in the set. Core AuthorizationOptions (Authorization:* appsettings) has no PKCE knob, so restricting to S256 needs code: a module PostConfigure<OpenIddictServerOptions>(o => o.CodeChallengeMethods.Remove("plain")) (the same hook vc-module-ai uses for its grant type) or a platform change. In Controllers/Api/AuthorizationController.cs Authorize() the comment "If prompt=login was specified by the client application," is followed by no code; it has been an orphaned comment since the endpoint was added (VCST-1415, #2809). Only max_age (RequestHasExpired on the cookie IssuedUtc) and prompt=none (login_required / consent_required) are honoured. On a storefront host the platform redirects to the OAuth login path (/oauth/authorize in vc-frontend), whose page never reads prompt and silently mints a 5-minute platform cookie through POST /connect/session, so prompt=login there needs the storefront page to cooperate as well.
