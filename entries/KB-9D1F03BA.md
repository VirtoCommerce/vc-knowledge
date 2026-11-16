---
id: KB-9D1F03BA
subject: Since vc-frontend 2.62.0 app-runner awaits the design and preset stylesheet chunks before app.mount with no time bound, and the chunk-load recovery reacts only to failed imports, so a stalled style chunk is never recovered
plane: experiential
question: Why does a stalled preset or design stylesheet chunk keep the storefront blank, and since which vc-frontend version?
questions:
  - text: Is there a timeout on loadPresetStyles / loadDesignStyles before the storefront mounts?
  - text: Does chunk-load-recovery reload the storefront when a lazy chunk hangs instead of failing?
  - text: Which vc-frontend commit made the app wait for the preset stylesheet before mount?
  - text: Is the preset-stylesheet boot gate new in vc-frontend-next or inherited from vc-frontend?
status: active
appliesTo:
  - axis: phase
    value: boot
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: client-app/app-runner.ts
  - coordinate: client-app/core/utilities/design/index.ts
evidence:
  - method: source
    deployment: vc-frontend@2.62.0
    defect: suspected
    at: 2026-11-16T14:14:48.740Z
    by: session:4cdfee7c
    who: Lenajava1
---
Read in vc-frontend dev (2.63.0) and vc-frontend-next at 7be3dfdd (PR #23, merge of upstream 2.62.0); core/utilities/design/index.ts is byte-identical in both. app-runner.ts starts loadDesignStyles(design) early and loadPresetStyles(activePresetName) after the preset is added to the theme context, then runs `await Promise.all([designStylesReady, presetStylesReady]); app.mount(appElement);` (upstream lines 260/355/357, Next 265/360/362). loadStyles() wraps `await load()` in try/catch, so a FAILED chunk is swallowed and the app boots. Nothing races or times out the await, so a PENDING chunk holds the mount for as long as it is pending. chunk-load-recovery.ts listens only to vite:preloadError (a failed import) and reloads once if nothing has rendered after BOOT_SETTLE_DELAY=3000. A pending import fires no event, so the recovery never runs for a stall. Introduced upstream by 7d6151e16 (#2577, design styles awaited before mount) and cf7f811ed (#2620, preset stylesheet + paprika.scss added to the same await); first tag containing both is 2.62.0. Only the paprika preset ships a stylesheet (assets/presets/paprika.scss). Upstream fetches it only for paprika stores; vc-frontend-next pins DEMO_THEME_PRESET="paprika" in app-runner.ts, so every Next store has it on the boot path. No unit test covers the slow case (index.load-failure.test.ts covers failure only). Live behaviour of the Next build: KB-E547B0A6.
