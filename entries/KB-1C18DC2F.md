---
id: KB-1C18DC2F
subject: Storefront falls back to the bundled settings_data current preset (not the store's white-labeling preset) when the merged themePresetName does not resolve; mercury and default are bundled presets
plane: experiential
question: What theme does the storefront apply when an organization's white-labeling themePresetName cannot be resolved?
status: active
appliesTo:
  - axis: feature
    value: white-labeling
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: Query.whiteLabelingSettings
evidence:
  - method: source
    deployment: vc-frontend@a6ee5ce
    at: 2026-11-27T11:28:45.173Z
    by: session:d2ee4088
    who: Lenajava1
  - method: source
    deployment: vc-frontend@a6ee5ce36
    at: 2026-11-27T11:43:40.078Z
    by: session:d2ee4088
    bodyHash: 72ab0162775e
    who: Lenajava1
    note: "Re-read useThemeContext.ts (addPresetToThemeContext) and presetNameToFileName on 2026-11-27 in the same checkout: same fallback. New detail: client-app/assets/presets holds black-gold, coffee, default, mercury, paprika, purple-pink, red, watermelon (dark pairs too) - no ocean, no peach, which the Platform user guide's colour-scheme list names; 'paprika' is in no published list."
---
Read in vc-frontend (local checkout a6ee5ce, 2026-11-12; not verified equal to the deployed build): addPresetToThemeContext lowercases the name and replaces spaces with dashes, looks it up in the bundled presets and then at /assets/presets/NAME.json; if absent it retries with client-app/config/settings_data.json "current" (value "default" in that checkout), and only if that also fails throws "Missing preset". The xAPI handler already merged org-over-store per field, so a store's own preset is never consulted as a second fallback; it looks like the store palette only when the store preset equals the default. Bundled presets include default and mercury, so an org preset "Mercury" resolves and is not a non-existent preset. Not observed live by this session (writing an unresolvable preset was out of scope).
