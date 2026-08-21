# Mobile responsive layout design

## Goal

Make the portfolio readable and balanced on viewports at or below 640px without changing the desktop layout.

## Scope

- Use a 20px side gutter for the hero and portfolio content.
- Recompose the hero vertically: smaller title, illustration below the title, smaller role labels, and a compact scroll cue.
- Turn portfolio tabs into one horizontally scrollable row with smaller labels.
- Stack education cards vertically, keep the school name on one line, and reduce copy and illustration spacing.

## Rules

- The desktop CSS remains the source of truth above 640px.
- Hero title uses a mobile-only clamp of 64px to 76px and does not overlap the illustration.
- Role labels use 10px to 11px text with compact gaps.
- Tabs remain buttons and preserve their selection behavior; overflow scrolls horizontally instead of wrapping or clipping labels.
- Education cards use auto height, text-first order, and a 28px heading that has `white-space: nowrap`.
- Existing reduced-motion behavior and semantic markup remain unchanged.

## Verification

- At a 390px viewport, the hero title, illustration, roles, and scroll cue have no overlap.
- At a 390px viewport, all five navigation labels are reachable by horizontal scrolling and remain legible.
- At a 390px viewport, both education-card titles render on one line and card content does not overflow horizontally.
