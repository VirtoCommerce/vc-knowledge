---
id: KB-4AC4C5AD
subject: A storefront user whose member is an Employee signs in normally once GetMe returns me.contact null (PR #149 build); on the build without it the whole storefront renders blank after sign-in
plane: experiential
question: What happens on the storefront when a user whose member is an Employee (not a Contact) signs in?
questions:
  - text: Does an Employee-type member with a storefront login break GetMe?
  - text: Why is the storefront blank after an Employee user signs in?
  - text: What does me.contact return for an Employee user?
status: active
appliesTo:
  - axis: member-type
    value: employee
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.me
  - coordinate: /sign-in
  - coordinate: /account/dashboard
  - coordinate: /account/profile
evidence:
  - method: observation
    deployment: vcptcore_qa1
    build: 0361e2adb11cbebb
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0-pr-149-2242
    at: 2026-10-28T23:30:51.123Z
    by: session:faa575c7
    who: kutasinaelena
  - method: observation
    deployment: vcptcore_qa1
    build: 0361e2adb11cbebb
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0-pr-149-2242; theme=2.61.0-alpha.2601; store=B2B-store
    at: 2026-10-28T23:45:35.758Z
    by: session:faa575c7
    bodyHash: 130828bd7de4
    who: kutasinaelena
    note: "Re-observed independently by the frontend lane: NEW signs in (GetMe contact null, no errors[], 147 GraphQL responses in the session HAR with no errors[]), home/dashboard/category/add-to-cart work; OLD vcst_qa blank page with GetMe + GetPageContext each carrying nine NULL_REFERENCE errors."
  - method: observation
    deployment: vcptcore_qa1
    build: 0361e2adb11cbebb
    conditions: module:VirtoCommerce.ProfileExperienceApiModule=3.1022.0-pr-149-2242; theme=2.61.0-alpha.2601; store=B2B-store
    at: 2026-10-29T00:15:17.852Z
    by: session:faa575c7
    bodyHash: 130828bd7de4
    who: kutasinaelena
    note: "Regression re-run: page-load user is read via GetPageContext (pageContext.user.contact null, errors[] empty); dashboard, account menu (email label), Logout, quantity-stepper add (header Cart 1) all work; all /graphql 200."
---
Password sign-in on /sign-in as a user whose member is an Employee with a storefront login (store B2B-store). On ProfileExperienceApi 3.1022.0-pr-149-2242 the GetMe query returns me.contact = null with no errors[]; the storefront lands on the homepage, /account/dashboard, the account menu (shows the email instead of a name), a category page and add-to-cart (quantity stepper) all work; /account/profile shows empty First/Last name and a disabled Update button. On 3.1022.0 (without the change, vcst_qa) the same sign-in leaves a blank white page on every route (/, /account/dashboard after reload): GetMe returns me.contact as an object whose firstName, lastName, fullName, organizationId, defaultLanguage, currencyCode, selectedAddressId, organization, organizations all fail (NULL_REFERENCE), and the console logs an ApolloError listing those fields. Logout is unreachable from the blank page.
