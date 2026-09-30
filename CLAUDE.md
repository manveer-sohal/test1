# Project: How SSRIs Treat Depression (scrollytelling)

A single self-contained `index.html` — three.js loaded from a CDN via an importmap, no build step, no backend, no persisted data. Scroll position drives a 7-stage narrative: a pink brain establishes the "why," a pill is introduced as the solution, the camera zooms into a single synapse, and the mechanism (normal signaling → depression → SSRI treatment → restored signaling) plays out there.

## Architecture

All state lives in one JS object, `state = { stageIndex, stageT, pumpActive, pumpBlocked }`, recomputed on every `scroll` event by `onScroll()`:

- `stageCount = 7`, and `#scroll-track` is `700vh` (100vh per stage).
- `stageIndex` = which stage is active (0-6); `stageT` = progress (0-1) within that stage.
- Every per-frame visual (camera position, pill position, brain opacity, pump spin/color, particle behavior) is a pure function of `state`, recomputed inside `animate()`'s `requestAnimationFrame` loop — not of wall-clock time, except for things explicitly meant to idle (brain's slow spin, pump's shimmer).

Stages:

| # | Name | What happens |
|---|------|--------------|
| 0 | The Depressed Brain | Pink brain visible alone (camera `CAMERA_WIDE`), synapse hidden |
| 1 | The Solution | Pill slides in from off-screen right, rests, spins |
| 2 | Delivery | Camera zooms `CAMERA_WIDE → CAMERA_CLOSE`; pill flies to the pump; brain fades out; synapse fades in |
| 3 | Normal Signaling | Particles cross the gap freely, receptors flash |
| 4 | Depression | Pump reabsorbs particles before they cross (`pumpActive`) |
| 5 | Treatment | Pill (already delivered in stage 2) sits at the pump; pump visually jams partway through (`pumpBlocked`) |
| 6 | Restored Signaling | Particles cross freely again, pump stays jammed |

## Gotchas found the hard way this session

- **Membrane-bulge occlusion.** The membrane mesh has a procedural "organic bulge" that peaks at its own center (y=0, z=0). Any object placed near that center (e.g. the pump) needs enough offset to clear the bulge peak (`amplitude + half-width`), or it renders hidden *inside* the membrane's own geometry. Off-center objects (receptors) aren't affected since the bulge is smaller away from center.
- **`brain` is a `Group`, not a `Mesh`.** It contains three parts (cerebrum/cerebellum/brainstem), each with its own material. There is no `brain.material` — use the `setBrainOpacity(o)` helper (loops over `brain.userData.parts`) to fade it. Setting `.material.opacity` directly on the group throws.
- **Off-center object placement must account for aspect ratio.** An object at world-x `X`, at distance `depth` from the camera along its view axis, is only on-screen if `X < depth * Math.tan(fovRadians/2) * aspect`. A position tuned to look right in one browser window can be off-screen (or just barely clipped) in a narrower one. When placing "hero" objects off-axis (like the pill), check this across a plausible aspect range, not just the window you happen to be testing in.
- **Easing applied across a whole stage can make motion invisible for most of that stage.** `easeInOut(t)` starts very slowly (quadratic near 0) — if a slide/fade runs across the *entire* stage's `stageT` range, it can still be >90% un-arrived at 50% scroll progress through that stage. For "arrive early, then rest" motions, compress the eased input (e.g. `easeInOut(Math.min(stageT * 2.5, 1))`) so the motion completes well before the stage ends.
- **This session's Chrome automation tab throttles `requestAnimationFrame` and even `scroll` event dispatch when unfocused/occluded**, and screenshots can show a stale WebGL frame even when the DOM/text has updated correctly. When verifying visual state via that tooling: prefer forcing state + calling `renderer.render()` directly and reading object properties back over trusting a screenshot alone. This is a testing-environment quirk, not something that affects real users with a focused tab.

## Visual conventions

- **Color language:** brain = pink (rose creases/pink ridges via vertex colors); presynaptic membrane = blue; postsynaptic membrane = green; serotonin particles = amber/orange; reuptake pump = amber (active) → gray (blocked); SSRI pill = cyan (deliberately distinct from everything else).
- **Text alignment:** each stage's `.stage-content` card aligns to whichever side is *not* that stage's visual focus (e.g. depression's text is `align-right` because the pump — the focus — is on the left). `.stage` uses `align-items: flex-start` (cards anchor near the top) precisely so text doesn't compete with vertically-centered 3D content on *any* stage.

## Running / verifying

```
python3 -m http.server 8000   # from this directory
```

Open in a normal focused browser tab and scroll. For automated checks, use the `claude-in-chrome` tools, keeping the rAF-throttling gotcha above in mind.
