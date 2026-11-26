---
id: KB-A0CF2DF3
subject: VcEmptyView renders its empty and error states without role or aria-live, so screen readers are not told when they appear
plane: experiential
question: Is the VcEmptyView empty or error state announced to screen readers (role / aria-live) when it replaces a loading list?
questions:
  - text: Does the storefront empty view have a live region?
  - text: Are empty-list and load-error messages announced by assistive technology on the storefront?
status: active
appliesTo:
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /company/my-customers/{id}/coupons
  - coordinate: /account/coupons
evidence:
  - method: observation
    deployment: mac_local
    conditions: platform=3.1087.0; theme=vc-frontend local build (coupon-item compact branch)
    defect: suspected
    at: 2026-10-21T19:53:06.490Z
    by: session:6eb5c164
---
On the sales-rep coupons widget and the all-coupons page, the "Couldn't load coupons" error state and the "No coupons available" empty state are rendered by the UI-kit VcEmptyView in place of the skeleton after the async load. The rendered element has no role and no aria-live, even for variant="error", and is not inside a live region, so the change is not announced (WCAG 4.1.3). By comparison, the copy-code notification lands in section.notifications-host[aria-live=polite]. Checked by DOM inspection and axe; screen-reader output itself was not tested. The component source (vc-empty-view.vue) also sets no role or aria-live.
