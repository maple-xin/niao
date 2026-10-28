# Portrait view update · v0.2.1

## Change
Only main.js camera/framing and inspect diagnostics changed, plus version/docs/build artifacts. Six maps, collision, movement/aim input, AI, 60-second rounds, 10-second ink and 1.5-second refill are retained.

The root issue was the portrait follow target clamping to the middle of the board (x±4.8, z−5.8..5). Edge spawns such as map6 z8.6 ended up low and left under the joystick. Portrait now follows to arena edges with a 2-unit interior framing margin (for current boards x±7, z±8), a ground bias aligned with camera azimuth and 1.15-unit velocity look-ahead. The look-ahead eases at rate4; position follows at rate7. Reset clears the lead and settles to spawn framing before play.

Portrait reach18.2 / elevation21, with aspect-based zoom clamped0.82–1.3. Title and landscape keep their existing framing. Read-only camera measurements are included only in ?inspect output; no state-forcing hooks were added.

## Real browser input and captures
Local in-app browser at http://127.0.0.1:5194/ on source, with actual joystick presses and held Space/splash input. This is browser input at phone-sized viewports, not physical-phone two-finger testing.

- Baseline/after: portrait-view-before-390.png and portrait-view-after-390.png, both map6 spawn, 390×844. Old spawn sits by the lower-left joystick; new spawn is centered with feet abouty499 and heady440.
- All six map starts captured at390×844: portrait-map-1-390.png through portrait-map-6-390.png. All initialize in the same safe player frame, including lower-edge spawns.
- Held movement around all four map edges in map6: east, northeast, northwest and southwest. Positions reached approximatelyx±8.51 / z±9.51. Actual player projected feet stayedx102–283 / y372–511 in390×844; the character stayed above the thumb controls. See portrait-east-390.png, portrait-north-east-390.png, portrait-north-west-390.png and portrait-south-west-390.png.
- At320×568, moved to the southwest edge: projected head(81,270), feet(85,319); joystick and splash remain below the character. See portrait-small-320.png.
- At430×932, held movement and splash in Soda Docks: input splash=true, charge0.8133; look-ahead approximately(0.008,−1.141); projected head(201,514), feet(202,579). Next crate and path are visible ahead. See portrait-lookahead-430.png.
- Resized while paused to844×390; remaining round time stayed exactly unchanged; landscape zoom1.12 retained. Resumed and captured, paused again, then returned to390×844 and resumed. Portrait zoom0.9473 restored and input/camera remained valid. See portrait-rotation-landscape.png and portrait-after-rotation-390.png.
- No console warnings/errors recorded.

Read-only measurements: portrait-camera-states.json. All13 portrait samples had feet inside15%–85% of width, head below23% of height and feet above72% of height, leaving the actual thumb controls clear. These are captured samples, not an exhaustive mathematical guarantee for every possible path.

## Checks and delivery
- Node syntax check main.js passed.
- Existing15 rule checks passed after the edit; game rules were not modified.
- Static dist and embedded standalone regenerated from the updated source.
- Embedded standalone loaded at390×844, selected map6, moved3.91units with splash, zoom0.9473, no browser warnings/errors. See portrait-standalone-390.png.
- Built dist preview at5193 verified with simultaneous W+splash, portrait zoom0.9473, visible player atfeet(196,527), no browser warnings/errors. See portrait-production-390.png and portrait-production-state.json.
- Temporary viewport overrides restored and QA tabs closed; old user tabs preserved. New playable tab left at http://127.0.0.1:5193/?v=0.2.1.

## Limits
Physical-device multi-touch, display cutouts, thermal performance and Loopit execution still require target-device checks. This update verifies portrait framing and input continuity at the listed browser viewports, with real native input and screenshots.
