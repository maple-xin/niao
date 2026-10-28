# Six-map v0.2.0 verification

## Scope
Five Blox concept images (maps 2–6), six distinct playable arena configurations, map picker, per-map personal bests, local win stamps and next-map result action. Original map 1, 60-second rule and 10-second player charge / 1.5-second refill retained. Runtime art is authored Three.js geometry. Concept images are separate visual direction, not runtime screenshots.

## Automated checks
- Node syntax: main.js, ui.js passed.
- `node --test scripts/logic.test.mjs`: 15/15 passed.
- Each arena has connected free floor and a full-radius navigation graph. Collision, paint exclusion, landing preview, AI and isolated states pass.
- All six maps complete 60-second tactical simulations with finite collision-safe actors and exact ownership counts. See six-map-rules.json. This is pure-rule simulation, not real browser input.
- `node scripts/check-art.mjs`: all six worlds have finite geometry/matrices; each world below 140k base triangles. Both boys checked over 240 animation frames with sources concealed behind opaque front panels. See model-check.json.
- Standalone embeds ten modules including maps.js. Static dist build succeeds.

## Actual browser checks
In-app browser, local root and standalone HTML, no browser warnings/errors recorded.
- Selected each of all six maps; checked map ID/name, fresh round, scene rendering and splash input. See six-map-browser-states.json and map-1/3/4/5/6-browser.png.
- At 390×844, held splash together with movement. Arcade active state: movement input y=-1, splash=true, charge0.863, travel4.50. See arcade-alley-mobile-active.png. Soda Docks also moved and painted; see soda-docks-mobile-play.png.
- At 320×568, picker scrolls and the sixth card can be selected. At 844×390, all six cards fit in a 3-column panel. Initial small-screen score text overlapping the modal was fixed by giving the overlay an explicit stacking layer. See six-map-picker-320.png and six-map-picker-844.png.
- Waited a full rooftop round: loss result and per-map best displayed. Restart remains on map5 with score/reset input. Pause → picker → Back preserves exactly59.6833seconds and returns to pause. Resume returns to play.
- Reload returns to map1. Switching to map5 restores its local best2.6163%; other map bests are independent. Legacy best migration also visible on delivery origin (map1 best45.5%).
- Embedded standalone loads, switches to map2, accepts movement+splash and produces painted ownership. See standalone-six-maps-mobile.png.
- New static build opened at http://127.0.0.1:5193/?v=0.2.0 with six-card picker. Previous user tab was preserved. See six-map-delivery-picker.png.

## Limits
Real-phone multitouch, physical-device performance and Loopit hosting remain unverified. Six complete manual browser victories / next-map win stamps were not claimed: six full win-capable rounds were rule-simulated; browser checks covered map switching, active input, pause, loss, retry and local best persistence. No browser automation injection, fabricated runtime images, or state-forcing test hooks were used. Hidden ?inspect output is read-only diagnostics.

## Visual regression choice
Keep the map-specific browser PNGs and source hashes as review baselines. No new screenshot framework was added to this dependency-free project. Compare actual runtime captures with concepts as different artifact classes.
