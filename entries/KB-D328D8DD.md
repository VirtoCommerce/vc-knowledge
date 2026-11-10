---
id: KB-D328D8DD
subject: In the Admin Shell Agent tools screen, a checked switch renders its knob outside the track in right-to-left languages (ar, he)
plane: experiential
question: Do the Admin Shell MCP screens (#/mcp-tools) render the tool switches correctly in right-to-left languages such as Arabic and Hebrew?
status: active
appliesTo:
  - axis: surface
    value: admin-ui
anchors:
  - coordinate: /apps/admin-shell#/mcp-tools
evidence:
  - method: observation
    deployment: vcst_qa
    build: 7983ac1fe08297d5
    conditions: platform=3.1096.0; module:VirtoCommerce.MCP=3.1000.0-pr-1-df74; ui-language=ar,he; role=administrator
    defect: suspected
    at: 2026-11-10T15:47:59.491Z
    by: session:40688c0a
    who: Lenajava1
---
Admin Shell (vc-shell) at /apps/admin-shell/#/mcp-tools, signed in as an administrator, UI language switched to ar and to he (document dir=rtl). The text mirrors correctly, there are no raw locale keys and no horizontal overflow. But every CHECKED switch (the shared VcSwitch, 46 on the Agent tools screen) draws its knob outside the track: track spans x 852–888, knob x 886–902 (the ::before has left:18px plus translateX(16px), which in RTL pushes it past the track end). The knob is white on a white page, so a checked switch reads as a plain blue pill. In LTR (en, fi, fr) the knob sits inside the track (571–587 within 553–589). The unchecked state was not observed (no switch was flipped). Relative dates on the screens are not localized (advisory).
