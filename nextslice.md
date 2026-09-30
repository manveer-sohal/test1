The previous slice is working.

Now implement the next vertical slice:

Slice 5 — Full Visual/Color Polish Pass

Goal: Once the full 6-stage flow works (Slices 1-4), tune every stage to match the plan's color-language and composition rules consistently.

User-visible result: The whole experience reads as one visual system — consistent pink/blue/green/amber/cyan usage, consistent text-card placement, consistent fog/background, smoother easing.

Included functionality:

- Rose/darker-pink shading in the brain's fold creases rather than flat pink.
- Lighting tuned so all 6 stages are evenly, pleasingly lit.
- Verify/align text-overlay left/right placement per stage; confirm fade timing feels right at normal scroll speed.
- Tune particle counts/speeds and pump glow intensity for legibility (e.g. slow down recalls if too subtle).

Explicitly excluded/deferred: No new geometry/stages; denser brain fold detail still deferred to Slice 6.

Relevant mockup area: The plan's full "Color language" and "Per-stage visual read" sections, treated as the spec to
/plan to preview
Update available! Run: brew upgrade cl…
───────────── ssri-synapse-scrollytelling ─
❯
────────────────regresses in legibility.

How to verify manually: Compare before/after brain screenshots; scroll through the full site once more to confirm nothing else regressed.

How to visually compare with mockup: Side-by-side the brain screenshot against ref-images/side-brain.png's cortex texture density (ignoring color/labels) — judge by comparable organic-ness, not pixel matching.

Dependencies: Requires Slice 2, ideally Slices 1-5 complete.

---

Summary

- Minimum slices for a successful demo: Slices 1-4 — that's the complete brain → pill → zoom → mechanism narrative working end-to-end, even if visually rough.
- Defer first if time is short: Slice 6 (explicitly stretch), then Slice 5 (polish) — a demo can still work with rougher colors/timing. If extremely tight, Slices 2-4 could be dropped and Slice 1 alone shown as a fallback, though that loses the narrative improvement you asked for.
- When to stop adding features: Once Slice 4 works end-to-end without errors (even rough) — stop building new functionality and move fully into Slice 5 polish + verification, touching Slice 6 only if time remains after that.

✻ Baked for 2m 46s · done 2:31 PM · 1 shell still running
copied 13455 chars to clipboard
───────────── ssri-synapse-scrollytelling ─
❯
────────────────match.

Main files/components: /Users/manveersohal/test1/index.html — material/light tuning, CSS tweaks to text overlays.

Required states/interactions: None new — tuning existing parameters only.

Functional acceptance criteria:

- Full scroll-through feels cohesive — no jarring inconsistencies, no stage noticeably harder to read.
- Text legible against the 3D background in every stage.
- No console errors or frame-rate stutter introduced by tuning.

How to verify manually: Full slow scroll-through both directions, screenshotting each stage for side-by-side comparison; read each text card at a glance to check legibility.

How to visually compare with mockup: Compare each stage screenshot against its "Per-stage visual read" bullet and the "Color language" list — confirm every named color/behavior is actually present.

Dependencies: Requires Slices 1-4 fully working.

Keep changes focused and preserve the current working behavior. Run relevant tests/linter after the change. Fix only relevant failures.

After editing, summarize what changed, how to test it, and what remains.
