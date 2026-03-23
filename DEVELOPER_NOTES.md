# Developer Notes

## Overview

This folder contains a standalone browser-based laminate layout calculator.

Current entry point:

- [laminate_calculator_all_in_one (2).html](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html)

The app is currently implemented as a single HTML file with React rendered directly in the browser.

There is no backend, no API, and no persistence layer.

## Runtime Dependencies

Currently used by the HTML:

- [react.development.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/react.development.js)
- [react-dom.development.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/react-dom.development.js)
- [babel.min.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/babel.min.js)

Currently not used anymore:

- [plotly-latest.min.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/plotly-latest.min.js)

`babel.min.js` is used only because the page still contains JSX inside the HTML. In a normal React/Vite/Webpack project this dependency should not be needed.

## App Structure

Geometry helpers:

- polygon intersection and containment helpers start around [laminate_calculator_all_in_one (2).html:69](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:69)
- full polygon intersection check is at [laminate_calculator_all_in_one (2).html:142](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:142)
- full-board containment check is at [laminate_calculator_all_in_one (2).html:148](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:148)

Shared UI shell:

- toolbar: [laminate_calculator_all_in_one (2).html:246](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:246)
- SVG canvas: [laminate_calculator_all_in_one (2).html:301](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:301)
- page layout wrapper: [laminate_calculator_all_in_one (2).html:455](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:455)

Shared interaction/state layer:

- main reusable planner state hook: [laminate_calculator_all_in_one (2).html:473](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:473)

Layout-specific planners:

- English herringbone: [laminate_calculator_all_in_one (2).html:754](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:754)
- French variant 2: [laminate_calculator_all_in_one (2).html:823](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:823)
- French classic: [laminate_calculator_all_in_one (2).html:908](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:908)
- Royal / Italian braid-based planner: [laminate_calculator_all_in_one (2).html:1004](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:1004)

Mode selection hub:

- [laminate_calculator_all_in_one (2).html:1127](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:1127)

React mount:

- [laminate_calculator_all_in_one (2).html:1246](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html:1246)

## Current Functional Behavior

Supported modes:

- English herringbone
- French variant 2
- French classic
- Italian herringbone
- Royal herringbone

Common capabilities:

- draw a room polygon manually
- close/reset/undo polygon
- rotate and offset the selected layout
- click boards to force include/exclude them
- measure distances on room edges
- calculate full boards and cut boards

Board inclusion logic:

- a board counts as present if its polygon intersects the room polygon
- not only vertex-in-polygon hits are counted
- this was added deliberately to include boards cut by corners or edge crossings

## Integration Notes

Recommended migration path for a web team:

1. Move the logic from the inline Babel script into normal source files.
2. Keep `usePlannerCore` as the natural shared hook/base module.
3. Keep each layout generator as a separate component or geometry module.
4. Replace the current in-browser Babel flow with the project build pipeline.
5. Remove `babel.min.js` after the JSX is moved into a normal frontend build.

Low-risk extraction boundary:

- keep geometry helpers together
- keep `usePlannerCore` as the interaction/controller layer
- keep planners separated by layout type
- keep the `FrenchLayoutsHub` as the integration entry point

## Known Project State

- the old simple Plotly calculator was removed from the HTML
- `plotly-latest.min.js` remains in the folder but is unused
- the app is still prototype-style, but the internal structure is now much cleaner than before

## Suggested Cleanup For Handoff

Safe to do later:

- delete [plotly-latest.min.js](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/plotly-latest.min.js)
- rename [laminate_calculator_all_in_one (2).html](C:/Users/Михаил/Documents/Flooreo/Калькулятор%20укладки%20ламината%20v,2,0_files/laminate_calculator_all_in_one%20(2).html) to a cleaner production name
- move inline styles and script into separate files during integration

Not required before handoff:

- no backend work
- no API contract
- no database schema
