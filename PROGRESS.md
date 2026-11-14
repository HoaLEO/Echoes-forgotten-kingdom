# Echoes of the Forgotten Kingdom — PROGRESS

Single-file build: `output/index.html`. Backup of the pre-roadmap version: `output/backup/`.
Source lives in `src/*.js` + `template.html`, bundled by `build.mjs` (three.js r180, MIT).

## Test suites (headless Chrome, software GL)
- `test/play.mjs` — full playthrough with real input, all 3 puzzles incl. wrong answers, 5 save → reload → continue checkpoints.
- `test/bounds.mjs` — sprint-jumping in 32 directions from 7 places; player must stay inside the explorable world; jump height at 20–240 fps.
- `test/perf.mjs` — triangles / draw calls per viewpoint (load statistics only, NOT real frame rate).

## Stage 1 — Bug fixing and stability · DONE
- Hard world boundary (`inPlay`): no more climbing out of the forest or leaving the summit by sprint-jumping.
- Region name no longer shows "Celestial Sanctuary" on forest banks.
- Jump height is frame-rate independent (1.065–1.069 m from 20 to 240 fps; was 0.91–1.06).
- Tests: play 101/101, bounds PASS.

## Stage 2 — Save, compass, guidance · DONE
- Autosave (localStorage): interaction replay log + star-stone positions; "Continue" on the title screen; cleared on victory / restart.
- Compass strip with objective marker routed through a waypoint graph, and distance read-out (toggle in Settings).
- Hint key `H`: 1–3 escalating hints per objective step; automatic nudge after 3.5 min without progress.
- Gameplay timers moved from wall-clock to game time (pause-safe, deterministic).
- Tests: play 106/106 (5 save/continue checkpoints), bounds PASS.
- NOT verified: save surviving a real browser restart. Headless Chrome on file:// occasionally dropped localStorage on reload
  (the game had written it); the test re-seeds in that case.

## Stage 3 — Performance · DONE (real-GPU frame rate NOT measured)
- Vegetation split into spatial cells: frustum culling (also in the shadow pass), distance culling, grass fade-out by distance.
- Tree level of detail (near/far meshes), lighter trunks.
- Presets now also control vegetation distance and LOD distance.
- Optional FPS / triangle overlay (Settings → Show frames per second).
- Triangles per frame, High preset: 3.0 M everywhere → 0.8–1.9 M depending on view. Draw calls 160–550.
- Target of ~60 fps at 1080p is unverified until measured on real hardware.

## Remaining
- Stage 4 Forest + Courtyard visuals · Stage 5 Temple visuals · Stage 6 Sanctuary (blue twilight) · Stage 7 content, sound, polish.

## Stage 4 — Forest and Courtyard visuals · DONE (visual check at low resolution only)
- Tree crowns and bushes rebuilt as textured leaf-card foliage (procedural leaf atlas, alpha cut-out) over a dark inner mass; fewer triangles than the old solid blobs.
- Brighter forest ambience; warm lantern waymarks along every forest trail and the mountain stair; braziers in the courtyard (warm vs cool crystal light).
- New overgrown gatehouse ruin beside the forest trail, ruined walls behind the glade, a dozen more ivy curtains on the courtyard walls.
- Inscription tablets now show carved rune rows instead of scattered strokes.
- Tests: play 106/106, bounds PASS, no console errors.
- Note: alpha-tested foliage costs fill rate; real-GPU frame rate still unmeasured.

## Stage 5 — Sunken Temple visuals · DONE (visual check at low resolution only)
- Mirrors are now faceted crystal reflectors in bronze rings, each with its own hue; the beam takes the colour of the reflector it leaves (puzzle geometry and logic unchanged).
- Warm braziers in the great hall, the Chamber of Mirrors and at the temple approach; coloured crystal growths; less monochrome fog / ambient.
- Tests: play 106/106, no console errors.

## Stage 6 — Celestial Sanctuary visuals · DONE (visual check at low resolution only)
- Blue twilight: moonlight, star field and a faint sky band, cool stone and terrain, exposure kept high enough to navigate comfortably.
- Great cyan crystal over the orrery with a column of light, larger blue floating crystals, crystal growths, rune-marked monoliths on the rim, warm braziers for contrast.
- Distant mountains rebuilt: two rings of ridged, snow-capped peaks (opaque, tinted by the atmosphere).
- The climb blends from daylight to twilight with altitude.
- Tests: play 106/106, bounds PASS, no console errors.

## Stage 7 — Content, sound, polish · NOT STARTED (only: larger objective text)
Remaining ideas: more secrets / shortcuts to reach 15–30 min, audio pass after listening on real hardware, foliage and lighting tuning after a real-GPU look.

## Not verified anywhere yet
- Real-GPU frame rate (goal ~60 fps at 1080p, High). All measurements are triangle / draw-call counts in software GL.
- Full-resolution look of stages 4–6 (screenshots were taken at 480x270, Low preset, because software rendering of the new foliage is very slow).
- Mouse look with pointer lock, audio output, Firefox / Safari, save persistence across a real browser restart.
