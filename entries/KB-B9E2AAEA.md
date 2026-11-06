---
id: KB-B9E2AAEA
subject: VcWidgetSkeleton renders a VcWidget of its size, so its frame equals the loaded widget's to 0px in classic and soft
plane: experiential
question: Does VcWidgetSkeleton have the same size and insets as the loaded VcWidget (classic and soft design)?
questions:
  - text: VcWidgetSkeleton classic border skeleton parity widget size
  - text: Did the classic widget skeleton change radius or border colour in theme 2.62.0?
  - text: Will content jump when a widget skeleton is replaced by the loaded widget?
status: active
appliesTo:
  - axis: component
    value: vcwidgetskeleton
  - axis: surface
    value: storefront-ui
anchors:
  - coordinate: /storybook/components-organisms-vcwidgetskeleton--with-widget
  - coordinate: /storybook/components-organisms-vcwidgetskeleton--header-slot
  - coordinate: /catalog
evidence:
  - method: observation
    deployment: vcst_qa
    build: 80aa52bbcc451dd4
    conditions: theme=2.62.0-pr-2620-b47e; base=dev@37cdaf67
    at: 2026-11-06T12:06:58.081Z
    by: session:213be081
    who: Lenajava1
---
Measured on the hosted Storybook built from vc-frontend PR #2620 (VcWidgetSkeleton 'With Widget' story, sizes xs/sm/md/lg, design classic and soft, preset red light and dark): the skeleton root is section.vc-widget.vc-widget-skeleton and its radius, border width, horizontal padding, header min-height, footer height and total height equal the loaded VcWidget's to 0px; the skeleton renders no title, icon or toggle. On the vcst storefront (classic) the /catalog facet skeletons are 40px collapsed widgets, the same height as the loaded facet headers. Versus the pre-PR base (dev 37cdaf67, Storybook built from source, PIXEL diff) the classic skeleton changed by design: radius 4→8px, border neutral-100→neutral-200, footer divider neutral-300→neutral-200, footer bar 16→20px, and in the header-slot story the header went 56→50px because the skeleton now takes the widget's header min-height. Classic VcWidget itself showed 0 pixel regressions.
