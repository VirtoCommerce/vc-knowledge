---
id: KB-5AD9D8A9
subject: The CyberSource payment module forwards the Flex transient token to the gateway without verifying its signature; its JWT verifier runs only on the capture context
plane: experiential
question: Does the CyberSource module verify the Flex transient token JWT before calling the payment API in Mutation.authorizePayment?
questions:
  - text: Where does VirtoCommerce.CyberSourcePayment validate the Microform token signature?
  - text: Why does authorizePayment accept a forged CyberSource token signature?
  - text: What does CyberSourceJwkValidator validate?
concepts:
  - id: card-payment
  - id: order-payment
status: active
appliesTo:
  - axis: gateway
    value: cybersource
  - axis: layer
    value: module
  - axis: surface
    value: xapi
anchors:
  - coordinate: Mutation.authorizePayment
evidence:
  - method: source
    deployment: VirtoCommerce.CyberSourcePayment@3.1002.0
    defect: suspected
    at: 2026-11-26T05:25:08.864Z
    by: session:779b94fc
    who: kutasinaelena
---
At tag 3.1002.0 of vc-module-cyber-source, CyberSourcePaymentMethod.PostProcessPaymentAsync (Data/Providers/CyberSourcePaymentMethod.cs:79-91) reads the "token" parameter and CyberSourceClient.ProcessPayment (Data/Services/CyberSourceClient.cs:60-74) puts it unchanged into TokenInformation.TransientTokenJwt for PaymentsApi.CreatePaymentAsync. CyberSourceJwkValidator.VerifyJwt (fetches /flex/v2/public-keys/{kid}, validates signature and lifetime) is called only in GenerateCaptureContextInternal (CyberSourceClient.cs:55) on the capture-context JWT, never on the browser-supplied transient token. Gateway statuses AuthorizedPendingReview/PendingReview/Pending/Transmitted/PartialAuthorized map to IsSuccess=true with only payment.OuterId overwritten and no transaction (CyberSourcePaymentMethod.cs:255-260, 326-342). In vc-module-x-order 3.1019.0 PaymentRequestValidator blocks authorizePayment only when the payment is Paid, so a held-for-review or Authorized payment can be submitted again.
