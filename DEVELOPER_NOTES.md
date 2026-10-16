# Developer Notes

## Overview

This is a standalone browser-based laminate layout calculator.

- Entry point: [index.html](index.html).
- Local dependencies: `react.development.js`, `react-dom.development.js`, `babel.min.js`.
- JSX is compiled in the browser by the bundled Babel. Keep this launch method; no build system is required.
- No backend, API, or database.
- Plotly is no longer used or present.

## Code Map

Geometry:

- `pointOnSegment`, `segmentsIntersect`, `polygonsIntersect`
- `isPolygonFullyInsidePolygon`
- `buildFittedViewBox`, `buildGridLines`

Shared UI: `PlannerToolbar`, `PlannerCanvas`, `PlannerLayout`.

Shared interaction/controller hook: `usePlannerCore`.

Layout components: `EnglishHerringbonePlanner`, `FrenchClassicPlanner`, `FrenchHerringbonePlanner`, `FrenchBraidPlanner`.

Top-level switcher: `FrenchLayoutsHub`.

## Supported Modes

Dropdown order:

1. English herringbone.
2. French classic.
3. Italian herringbone.
4. Double Italian herringbone.
5. Royal herringbone.
6. French variant 2.

Italian, double Italian, and royal layouts share `FrenchBraidPlanner`.

## Board Parameters

Common French board constants are at the beginning of the script:

- `FRENCH_BOARD_LENGTH = 620` mm.
- `FRENCH_BOARD_WIDTH = 132` mm.
- `FRENCH_BOARD_CUT_ANGLE = 45` degrees.
- `FRENCH_BOARD_DESCRIPTION` generates the shared UI label.

Length and width feed all French layout planners, including Italian module steps and duplicated boards. English boards retain their independent 694 × 132 mm dimensions.

The cut-angle constant describes the supported 45-degree board. Geometry still uses 45-degree constructions (`Math.SQRT2`); changing that constant alone does not support other cut angles. Supporting another angle requires revisiting and validating the layout formulas.

Keep the agreed double Italian geometry numerically equivalent to:

- `pairBOffsetY = 620 - 132 * Math.SQRT2`
- `moduleShiftXOverride = 132 * 2`
- `moduleShiftYOverride = -264 * (Math.SQRT2 + 1)`
- `rowShiftXOverride = 620 / Math.SQRT2`
- `rowShiftYOverride = 620 - 620 / Math.SQRT2`
- `duplicateABoardOffset = { x: 0, y: 132 * Math.SQRT2 }`
- `duplicateBBoardOffset = { x: 132, y: -132 }`

## Geometry And Counting

- A board is present when its polygon intersects the room, including edge crossings with no board vertices inside.
- Existing intersection semantics include boundary-only contact.
- A full board may touch or lie along the room boundary.
- For concave rooms, full-board detection checks edge intervals between boundary intersections; vertices alone are insufficient.
- Manual inclusion/exclusion overrides automatic presence.
- Braid lattice bounds must be transformed back with `inverseTransformPatternPolygon` before computing row/repeat indices. World-coordinate bounds cause missing boards in rotated, elongated rooms.
- French variant 2 also computes coverage and render bounds from the inverse-transformed room. Its previous half-offset padding could omit intersecting boards at large offsets.
- Keep the braid anchor and module vectors unchanged when adjusting coverage.

## Interaction

- X/Y offsets follow screen axes and are limited to ±2000 mm.
- Manual board overrides reset on rotation, offset changes, and layout changes.
- Fit the room on closure and layout changes, but not on rotation.
- Layout changes reset the pointer to measurement mode.
- Zoom is 0.5–3; measurement labels retain their screen size.
- A short click toggles a board. Left-button dragging measures even when it starts over a board.
- Mouse-down records the start only; show the draft ruler after movement.
- Do not stop mouse-down propagation on boards: measurement and panning depend on it.
- SVG click capture consumes the click generated after a measurement drag, including clicks on edges/background.
- Starting a fresh mouse gesture clears any leftover click suppression.
- A window mouse-up finishes a gesture released outside the SVG; losing window focus cancels it.
- Clear/reset paths share `clearDraftMeasurement` to reset all transient ruler state.

## Verification

There is no committed automated test suite. Development checks can compile the inline JSX with the bundled Babel and execute extracted geometry/controller functions in Node.js; hook simulation does not replace a real-browser check.

Before future geometry changes, compare generated boards against an expanded lattice enumeration for all six layouts, including elongated and concave rooms, rotations, and ±2000 mm offsets. Verify the double Italian constants remain equivalent.

Browser regression checklist:

- Draw and close a room; switch through all six layouts.
- Check full/cut counts for boards exactly along walls and across concave recesses.
- Rotate elongated rooms and move patterns to offset limits; check for uncovered regions.
- Toggle a board with a short click; drag a ruler starting over a board.
- End a drag on a board, edge, background, and outside the SVG; verify the next click works.
- Pan and release outside the SVG; return to the canvas and check that panning stopped.
- Clear measurements, reset, and switch layouts during interaction.
- Change zoom and verify label sizes and auto-fit behavior.

## Maintenance

Use component/function names rather than line-number references in this document.
