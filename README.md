# The Lantern Tide

A calming, single-file breathwork game. Your breath is the tide that relights a
drowned village's lanterns across a **dusk → night → dawn** arc — and, if you keep
a steady rhythm, a lost crane finds its way home at first light.

There is no failure and no timer pressure: drifting off-pace only slows the tide,
never punishes you.

## Play it

Just open **`index.html`** in any modern browser — no backend, no build step, no
dependencies. Everything (graphics, sound, save data) runs locally in the page.

- **Hold** the spacebar or **press & hold** the screen as you breathe *in*; **release**
  as you breathe *out*. Or simply watch the orb and breathe along — following hands-free
  works fully on its own.
- Pick the **Full Arc** (Coherent 5·5 → Box 4·4·4·4 → Relaxing 4·7·8, ~4–7 min) or a
  single **Free Session** of any one pattern.
- After the finale you can **rest in the tide pool** and keep breathing as long as you like.

The breathing patterns are the real, evidence-informed techniques — coherent/resonant
breathing (~6 breaths/min), box breathing, and the 4-7-8 relaxing breath.

## Using your real breath (microphone)

Tick **"Use my breath (mic)"** on the menu to control the tide with your actual exhale.
It listens only during the breathe-out phase and rewards a long, even, *gentle* exhale —
not loudness. Nothing is recorded or sent anywhere; it runs entirely in your browser.

Microphone access needs a *secure context*. Opening `index.html` straight from disk
(`file://`) works for the touch/follow modes everywhere, but some browsers block the mic
there. To enable the mic, serve the folder locally and open it over `http://localhost`:

```bash
# from this folder
python3 -m http.server 8000
# then visit http://localhost:8000
```

If the mic is denied or unavailable, the game silently falls back to touch — you never
get stuck.

## Accessibility

- **Calm visuals** toggle (also auto-respects your OS "reduce motion" setting) tones
  down particles, grain, and wave motion.
- **Sound** can be muted any time (the ♪ button in-session).
- The game runs entirely offline and stores only your longest "calm streak" locally.

## Notes

`.claude/` contains local preview tooling (a tiny static server + launch config) and is
not part of the game. The game is the single `index.html`.

This is the first game in a planned multi-game breathwork app — see
[`DESIGN-BACKLOG.md`](DESIGN-BACKLOG.md) for the other concepts.
