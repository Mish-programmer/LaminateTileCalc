# Developer Notes

## Overview

This folder contains a standalone browser-based laminate layout calculator.

Main entry point:

- [laminate_calculator_all_in_one (2).html](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html)

The application is currently implemented as a single HTML file with React rendered directly in the browser.

There is no backend, no API, and no database layer.

## Runtime Dependencies

Currently used by the HTML:

- [react.development.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/react.development.js)
- [react-dom.development.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/react-dom.development.js)
- [babel.min.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/babel.min.js)

Currently unused:

- [plotly-latest.min.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/plotly-latest.min.js)

`babel.min.js` is used only because the page still contains JSX directly inside the HTML. In a normal frontend project this would usually be handled by the build pipeline instead.

## Code Map

Shared geometry/helpers:

- `pointOnSegment`
- `segmentsIntersect`
- `polygonsIntersect`
- `isPolygonFullyInsidePolygon`
- `buildFittedViewBox`
- `buildGridLines`

Shared UI shell:

- `PlannerToolbar`
- `PlannerCanvas`
- `PlannerLayout`

Shared planner/controller logic:

- `usePlannerCore`

Layout-specific planners:

- `EnglishHerringbonePlanner`
- `FrenchHerringbonePlanner`
- `FrenchClassicPlanner`
- `FrenchBraidPlanner`

Top-level layout switcher:

- `FrenchLayoutsHub`

React bootstrap:

- `ReactDOM.createRoot(...).render(<FrenchLayoutsHub />)`

## Supported Modes

- English herringbone
- French variant 2
- French classic
- Italian herringbone
- Royal herringbone

## Functional Behavior

Common capabilities:

- draw a room polygon manually
- close/reset/undo polygon
- rotate and offset the selected layout
- click boards to force include or exclude them
- measure distances on room edges
- calculate full boards and cut boards

Board counting logic:

- a board counts as present if its polygon intersects the room polygon
- this includes corner cuts and edge crossings
- counting is not limited to cases where a board vertex lands inside the room

Full board logic:

- a board is treated as full only if its polygon is fully inside the room polygon
- otherwise it is treated as a cut board

## Integration Notes

Recommended migration path into a normal web project:

1. Move the inline script logic from the HTML into normal source files.
2. Keep `usePlannerCore` as the shared interaction/controller layer.
3. Keep each planner as a separate layout module or React component.
4. Keep the geometry helpers grouped together as a reusable calculation layer.
5. Replace the in-browser Babel flow with the project build system.
6. Remove `babel.min.js` after the JSX is moved into the normal frontend pipeline.

Low-risk extraction boundary:

- geometry helpers
- `usePlannerCore`
- each layout planner separately
- `FrenchLayoutsHub` as the integration entry point

## Current Project State

- the old simple Plotly-based calculator was removed from the HTML
- `plotly-latest.min.js` is still present in the folder but is no longer used
- the calculator remains prototype-style, but the code structure is now much cleaner than before

## Suggested Cleanup For Handoff

Safe to do later:

- delete [plotly-latest.min.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/plotly-latest.min.js)
- rename [laminate_calculator_all_in_one (2).html](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html) to a cleaner production filename
- move inline styles and inline script into separate files during integration

Not required before handoff:

- no backend work
- no API contract
- no database schema

## Maintenance Note

This document is intentionally written without line-number references.

If the calculator changes in the future, update this file by component/function names and architecture notes rather than by exact line positions. This keeps the document stable across refactors.
