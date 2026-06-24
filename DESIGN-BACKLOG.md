# Design Backlog — toward a multi-game breathwork app

The long-term vision is a single **"super" breathwork app that houses multiple small
games**, each built on the same real breathing techniques (box 4·4·4·4, 4·7·8,
coherent/resonant 5·5) but offering a different *kind* of experience. **The Lantern Tide**
(shipped in [`index.html`](index.html)) is the first game built toward that hub.

These five concepts were generated and scored alongside The Lantern Tide and are kept here
as candidate games. All are designed to be single-file, no-backend, Canvas + procedural
Web Audio, runnable by just opening a file.

## Candidate games

### 1. Tidewriter — *ambient / zen* (top-scored)
A no-fail meditative toy. Your breath is a tide that paints a glowing nocturnal shoreline;
each synced exhale deposits a phosphorescent tide line and bioluminescent plankton, and the
palette blooms midnight-blue → dawn-gold over a long sit. An "honest mirror" — the scene
reflects your steadiness with zero punishment. **Strength:** highest calming fidelity +
visual ceiling. **Watch:** deliberately low-arousal, single-genre loop.

### 2. Ember Tender — *arcade / score-chase*
Keep a single ember glowing through the night by nailing the four phase-transition beats;
clean breaths bank "heat" and a combo multiplier (×2→×5), wind gusts distract, and the tempo
escalates through box → coherent → 4-7-8 tiers (amber → teal → violet). **The design hook:**
the optimal scoring strategy *is* genuinely slow, calm breathing — to win you must actually
relax. **Strength:** strongest replay pull. **Watch:** window/tempo tuning so tension never
fights the calm.

### 3. Tidewright — *rhythm / timing*
"Conduct the tide." Press-and-hold = lungs filling, release = emptying; score **timing**
(transition vs phase boundary) + **steadiness** (smooth hold, no stutter). A "Coherence"
combo *calms* the palette and audio as you do well — a skill-earns-serenity inversion. Each
well-timed exhale breaks a wave on the beat with a consonant chime. **Watch:** lives or dies
on audiovisual polish + window generosity.

### 4. Ember: Keeper of the Long Night — *microphone-controlled*
Tend a campfire with your real breath: the mic reads the smoothed exhale envelope (exhale
only — the audible phase), and a long, even, gentle exhale fuels the fire and pushes back a
freezing dark; survive ~8 cycles to dawn. Rewards evenness/duration, not loudness.
**Strength:** most physiologically honest mic use. **Watch:** `getUserMedia` fragility —
needs calibration + a first-class touch fallback (already prototyped in The Lantern Tide).

### 5. Lumen Garden: The Breath Atlas — *educational / guided program*
A polished wellness-app loop: techniques are unlockable "Seeds," each session has a 20s
"Why" science card + form-coach preview + guided breathing; synchrony banks "light" spent to
grow a persistent bioluminescent garden, with daily streaks in `localStorage`. **Strength:**
most replayable / habit-forming. **Watch:** more app than game.

## Shared-engine note (why Lantern Tide came first)

The Lantern Tide is intentionally structured so its core modules become **shared
infrastructure** for the hub:

- **`Clock`** — drift-free breath-phase state machine (anchored to `performance.now()`),
  any [inhale, hold, exhale, hold] pattern.
- **`Input` + `Mic`** — the 3-tier breath signal (follow-along · tap/hold · optional mic
  with calibration + silent fallback), unified so games never branch on input source.
- **`Audio`** — fully procedural Web Audio (pentatonic pad, breath-tracking ocean noise,
  bell scale, click-free ramps).
- **`Scoring`** — synchrony → calm-meter, streaks, accuracy.

A future hub would keep these and swap the **`World` / `Render` / `Director`** layer per game
(the scene, the art, and the goal/progression). That's the path from one game to the app.

---

# Platform vision — elevating beyond the single-file constraint

Captured from a six-lens design review (3D/graphics, game-feel, audio, art/assets,
wellness/biofeedback, engineering). The single-file `index.html` is deliberate and stays the
shippable artifact today; this section is the roadmap for when the project commits to becoming
a real app.

## The thesis (the one idea that matters most)

Today the game *fakes* two things: visually (orb, lanterns, water, mist are independent glows
that don't know about each other) and mechanically (it rewards pressing **in time**, not
actually calming). The unifying upgrade — which four of the six lenses arrived at
independently — is to make **one breath signal the physics of everything** (light, motion,
sound, reward) **and make that signal reflect the user's real state.** The crane coming home
should be earned evidence the user down-regulated, not a scripted timer.

## Three-tier roadmap (by return-on-effort)

**Tier 1 — retrofit, no new stack. ✅ DONE (implemented in `index.html`, 2026-06-24):**
- `Spring` "BreathConductor" — player ring + a subtle orb "fill and rest" settle; the ring
  wobbles when out of sync and stills as you match (juice = coherence).
- Lantern ignition juice — 3-beat anticipation → white-hot bloom → settle, staggered when two
  light in a cycle so the bay ripples on.
- Hear your own exhale — the mic envelope now drives ocean swell + a surf layer; bells are
  stereo-panned to each lantern's position.
- Binaural pacing — procedural "air on inhale / surf on exhale" breath texture, plus a ~6 Hz
  binaural beat that fades in toward dawn (eyes-closed friendly).
- Safety + science — first-run "why it works" + safety card, and a **Gentle holds** mode that
  caps breath-holds to 4 s (e.g. 4-7-8 → 4-4-8) for pregnancy / cardiovascular / panic-prone users.

**Tier 2 — a build step; biggest visible jump:**
- God-rays cast from the orb, keyed to `Clock.fullness` (inhale = shafts of light sweeping the bay).
- HDR-emissive lanterns + selective bloom (real light that no longer banks at high counts).
- Style-locked AI assets (one SDXL LoRA → crane, village, lanterns, water; provenance ledger).
- GPU-instanced particles (~80 → 50k).
- Adaptive generative score (Tone.js) — voices enter and dissonance resolves as you settle.
- PWA + IndexedDB history (installable, offline, humane streaks; also gives the mic a secure context).

**Tier 3 — transformative; changes the genre:**
- Real HRV biofeedback — a `Bio` module alongside `Mic`: Polar H10 over Web Bluetooth (RR →
  RMSSD + ~0.1 Hz coherence) as gold standard, webcam rPPG (MediaPipe FaceMesh) as the
  no-hardware path. Lanterns light from physiology, not taps. (rPPG: HR trend reliable, individual
  HRV not — be honest in-product; keep raw signals on-device with one-tap export/delete.)
- Resonance-frequency finder — one-time sweep (6.5→4.5 bpm) to set each user's optimal pace.
- True 3D water (reflection/refraction), rigged 3D crane for the homecoming.

## Target architecture (the platform)

The decision that turns "one beautiful `index.html`" into a platform: ship a **headless,
deterministic, event-emitting breath engine** as a package.

- **`@breath/engine`** — lift `Clock` / `Input` / `Mic` / `Scoring` / `Audio` out of the IIFE
  into framework-agnostic ESM TypeScript behind one `BreathEngine` facade that owns the
  fixed-step update order. Inject the clock source (`now()`) so timing is testable in Node.
  Emits a normalized stream: `{ phaseType, fullness, swell, instantSync, cycleIndex, milestones }`.
- **`GameModule` contract** — `{ manifest, createWorld(rng), update, render, onCycle, milestones }`.
  Lantern Tide becomes the first module; the five backburner concepts above become siblings that
  reuse the engine for free. A new game is one folder + a manifest, not an engine fork. Use a
  seeded PRNG so sessions are reproducible and shareable.
- **Shell** — SvelteKit (compile-to-vanilla, tiny runtime — keeps the 60 fps Canvas path clean;
  React's reconciliation near the rAF loop is the wrong default) with routes for the game library,
  programs, settings. Canvas 2D stays the default renderer; **R3F/WebGL is an opt-in renderer**
  for games that want volumetric light.
- **Persistence** — versioned, schema-validated (zod) store over IndexedDB, namespaced per game;
  migrate the current flat `lt.*` keys once. Optional Supabase sync, anonymous-by-default, last-
  write-wins; only derived metrics ever leave the device.
- **Distribution** — PWA first (vite-plugin-pwa + Workbox precache, offline, installable, secure
  context for mic). Capacitor wraps the same build for App Store / Play Store — far less rewrite
  than React Native given the Canvas/Web Audio core.
- **Quality** — Vitest with an injectable clock for drift/pause/pattern-swap timing tests and
  `OfflineAudioContext` snapshots for the bell/ramp math; Playwright smoke tests; Lighthouse-CI
  budget for the 60 fps / installable gates; privacy-first analytics (PostHog, no PII) + Sentry,
  wired to the engine's existing event seams (`session_start`, `movement_complete`,
  `finale_reached`, `mic_calibration_failed`, `lowend_mode_engaged`).

## Honest caveats to carry forward

- 3D water and a fully rigged crane are high-effort and risk pulling a deliberately calm,
  2.5D piece toward "tech demo." Sequence them late.
- Tone.js / Three.js break the open-the-file promise — only adopt once a build step is accepted.
- Wellness features carry responsibility: screen breath-holds (done in Tier 1), keep biofeedback
  on-device, and never turn relaxation into a guilt-driven streak.
