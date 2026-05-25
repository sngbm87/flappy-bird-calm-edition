# Philosophy & Architecture

**Flappy Bird — Calm Edition** is a solo, vibe-coded browser game built for flow instead of frustration.

Gentler physics. Bigger gaps. A forgiving Feather Shield. Expressive but kind bird emotions. Cozy themes with living procedural music. Full controls on every device. The whole thing fits in a single HTML file you can open from your desktop.

The goal: "I want one more glide."

---

## Core Beliefs

**Zero dependencies is a superpower**

No bundler. No `npm install`. No CDN fonts, no analytics, no runtime assets of any kind. The game is pure HTML + CSS + one `game.js` engine + a tiny service worker for the PWA shell. This means:

- Instant play anywhere (disk, localhost, GitHub Pages, static host)
- Zero supply-chain risk
- The entire engine (~2400 lines) fits comfortably in a modern LLM context window — perfect for AI-native development and for humans who want to understand or modify the whole system in one view

**Procedural everything**

- Visuals: Canvas 2D only (bird, pipes, particles, weather, ground, vignettes)
- Audio: Web Audio oscillators + a procedurally generated reverb impulse
- Music: arpeggios scheduled on the audio clock, different motifs per theme

No image files. No audio files. No external dependencies at runtime. The game carries its entire world inside itself.

**Monolithic by design**

`game.js` is intentionally one file. Splitting it would require imports and a build step, breaking the "double-click index.html and play" promise. The clear 21-section structure with a frozen `CONFIG` block at the top makes navigation and modification easy despite the size.

**Delta-time physics for every display**

All motion, timers, and spawns are multiplied by `state.dt` (normalized to 60 fps) or `state.dtSec` (real seconds). The game feels identical on 60 Hz, 144 Hz, and 240 Hz screens. Lag spikes are capped (`DT_MAX`) so physics never explode.

**Object pooling + sprite caches**

220 pre-allocated particles and 140 weather objects. Offscreen canvases cache sky gradients, clouds, and every combination of theme × bird emotion. No per-frame allocation pressure, crisp on Retina and high-DPI phones via DPR scaling.

**Themes are emotional, not just cosmetic**

Four complete palettes (sunset, midnight, rain, aurora) control bird colors, ground, pipes, sky, particle hues, trail, and the musical arpeggio for that theme. Changing theme instantly transforms the entire mood and soundtrack. Choice persists via guarded localStorage.

**Accessibility is gameplay, not a patch**

- Every important state change goes through `announce()` into an ARIA live region
- Full keyboard support with visible focus
- `prefers-reduced-motion` respected for particles, camera shake, transitions, and music intensity
- `prefers-contrast: more` and `forced-colors` supported
- Large, clearly labeled touch targets for phone players
- Focus trap + inert background in the Zen Customizer drawer

**Mobile players are first-class**

The on-screen Brake / Flap / Dive control bar gives phone users the same expressive depth as desktop keyboard players. Touch events mirror the `heldKeys` model. The playable stage is prioritized in the layout on narrow viewports.

**Local persistence, no drama**

Guarded wrappers around localStorage (try/catch for private mode and quota). Stats, best score, unlocked achievements, settings, and daily-seed progress survive refreshes and PWA installs. No accounts. No cloud. No "your data is gone" stories. Score hacking is accepted — this is an open, hackable, transparent game.

**Tiny, honest PWA**

Real `manifest.webmanifest` + a minimal service worker that only caches the static app shell (HTML, CSS, JS, manifest). Installable on iOS and Android. Works offline after the first visit. No push notifications, no background sync, no analytics, no credential requests.

---

## Verification & Quality Bar

- `npm run check` (syntax + comprehensive smoke tests covering canvas, ARIA, mobile controls, PWA, storage, achievements, reduced-motion paths)
- GitHub Actions runs the identical check on every push and pull request
- Living 250-point solo web-game audit (`AUDIT-250.md`)
- Strict PR checklist: run check, playtest keyboard + mobile + customizer + reduced-motion + accessibility considerations

We treat "works on my machine" as the beginning of quality, not the end.

---

## When Contributing or Extending

1. Read `.memory/security.md` (always, before any change), plus `decisions.md`, `preferences.md`, and `quirks.md`.
2. Group any new magic numbers in the top `CONFIG` block.
3. All time-based logic must use `state.dt` or `state.dtSec`.
4. Screen-reader updates go through `announce()`.
5. Any motion or particles must check `state.reducedMotion`.
6. The customizer drawer must continue to use `inert` when closed and trap focus when open.
7. One focused concern per edit. Test assumptions by reading first.
8. Run `npm run check`, open the game locally, and playtest the changed area (including reduced-motion and mobile).

The constraints are the design. They force creativity, keep the experience light, and make the game a joy to hack on.

---

Built with care for players who loved the original loop and wanted a little more grace in the wings.

---

*This document replaced an earlier, much longer corporate-style blueprint to better match the calm, humble, and joyful spirit of the project.*