# Music and feedback · v0.2.2

## Delivered
Original in-project synthesis: 16 bars, 110 BPM, D major with an A/B call-and-response melody, wood keys, plucked accompaniment, bass, shaker/kick/rim percussion and short room reflections. Stereo loop length34.9091156seconds at22050Hz. All sounds are generated locally in src/soundbank.js; no provider API, external sample download or third-party music license is involved. Runtime memory is approximately14MB for cached DSP arrays and Web Audio buffers. No voice or realistic body sound was added.

Independent Music and Effects switches are saved beside the existing campaign record. Legacy master mute initializes both switches off. Existing phone portrait camera and all six maps are preserved.

## Event matrix
| Event | Feedback | Trigger / limit |
|---|---|---|
| First start / new round | short three-note start + music | user gesture; resets music offset |
| Button action | short soft click | actual button action, start cues avoid doubling |
| Spray starts / stops | liquid pop / droplet | transition, not every frame |
| Spray held with ink | watery noise/droplet loop | active play, effect switch on, ink available |
| Gain new ground | small liquid ping |24 actual painted cells,0.8-second cooldown |
| Reclaim ground | ascending double drop |12 stolen cells,0.65-second cooldown |
| Ink empties | low soft dud | charge crosses to zero once |
| Refill completes | two-note chime | charge crosses full once |
| Walk | three alternating dry taps | every0.72 units actually traveled |
| Hit wall | soft thud | movement intent but negligible travel for0.12seconds; latched until released/moving |
| Last10seconds | short ticks | once per second |
| Win / loss / tie | different tonal stingers | result transition; music and water stop |

## Verification before interruption
Actual in-app browser at390×844, native movement and held splash input. Values retained in audio-browser-summary.json are explicitly transcribed from the tool outputs; full raw state history from that interrupted REPL was not retained. Corresponding actual captures: audio-active-mobile.png and audio-settings-mobile.png.

- Before gesture: context locked, no audio source, output RMS0.
- Start: context running, music source active, RMS0.04849.
- Moving/splashing: music and flow active, step/claim/reclaim events triggered, RMS0.04343.
- Effects only: music off, flow on, RMS0.03765.
- Both off: neither loop active, RMS0.
- Music only: music on, flow off even while splash held, RMS0.05120.
- Preference reload retained Music on / Effects off; title remained silent and locked.
- Held ink to empty: charge0, flow off, exactly one empty cue. Released and moved to refill: charge1, exactly one refill cue.

## Verification after recovery
Actual native browser input, read-only inspect diagnostics and Web Audio analyser output. Raw captured states are in audio-browser-states.json.

- At320×568, all pause controls fit without scrolling. Music/Effects buttons are122.5×48px. See audio-settings-320.png. At844×390, pause controls remain scrollable and accessible; see audio-settings-landscape.png.
- Played beyond one complete music loop: game elapsed52.7167seconds, music offset17.7789seconds (after34.9091-second wrap), music start/stop counters unchanged from the start of this run (2/1). Music remained active with analyser RMS0.01939. There was no source restart at the loop boundary. See audio-after-loop-mobile.png.
- Moving into the board boundary produced one latched bump,24 traveled-distance step cues, actual claim/reclaim cues, and one empty cue; holding against the wall/empty button did not repeat bump/empty cues.
- Pause stopped both loops. Once the short UI cue ended, voices0 and output RMS0. Offset stayed17.9922seconds.
- Resume continued at18.0829seconds rather than restarting the music. At the loss result both loops stopped, exactly one loss stinger was active, and output RMS0.06185. Countdown fired10 cues. See audio-result-mobile.png.
- No browser warnings/errors were recorded in this source pass.

## Automated checks
Three audio checks use actual generated PCM plus a minimal Web Audio test double for source lifecycle. They verify finite/bounded samples and silent seam endpoints, duration, distinct footsteps, gesture unlock, interrupted-context resume, single music lifecycle, pause offset, independent mute groups, round reset, real-progress cues, no empty/refill spam and high-refresh frames with no simulation tick. They are distinct from real browser checks above. The15 existing game-rule checks remain part of npm test.

## Audition and limits
WAV previews are synthesized from the exact bank and runtime mix levels: playground-music-preview.wav, feedback-sfx-preview.wav and game-mix-preview.wav in artifacts/audio/. bank-metrics.json records actual PCM peak/RMS and render time. The assistant audio-input audition tool reported that audio input is unsupported, so no subjective listening or speaker-quality approval is claimed. These files allow human audition.

Physical-phone speaker balance, real iOS interruption recovery, two-finger hardware behavior and Loopit hosting are not yet verified. No external service or paid generation was attempted. The music samples are prepared during title idle time; AudioContext creation/playback waits for a player gesture.

## Export verification
All18 checks passed (15 rule +3 audio). Static dist built successfully and standalone embeds11modules including soundbank.js. The embedded standalone was loaded at390×844 and driven with native movement+splash: context running, music+flow active, RMS0.04478, no browser warnings/errors. See audio-standalone-state.json.

The built dist preview at5193 also accepted native movement+splash: musicStarts1, start/spray/claim/step events, RMS0.08367 and no browser warnings/errors. See audio-production-state.json. The final user tab is at http://127.0.0.1:5193/?v=0.2.2 and waits for Start before audio playback. Temporary viewport overrides were reset and QA tabs closed. The production server remains running.
