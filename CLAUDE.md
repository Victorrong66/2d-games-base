# 2d-games-base

This repo is the foundation for small browser games. The first game is **Swarm Arena** (`index.html`), a top-down arena survival shooter. New games reuse its structure and conventions, listed below.

- Repo: `Victorrong66/2d-games-base` (formerly `swarm-arena`)
- Live: https://victorrong66.github.io/2d-games-base/ (GitHub Pages, served from `main` / root)
- License: MIT. No build step, no dependencies, no package manager.

## How a game is built

Each game is **one self-contained HTML file**: inline `<style>` and an inline `<script>` wrapped in an IIFE. Google Fonts is the only external request. You run a game by opening the file in a browser.

Required page head (keep these in every game):

```html
<!doctype html>
<html lang="en">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

### Must-keep fixes

- **`.overlay[hidden] { display: none; }`** must follow any `.overlay { display: grid }` rule. Without it the author `display` overrides the `hidden` attribute, so overlays (start, pause, game over) never hide and on GitHub Pages the "Play again" card swallowed touch input. Any element toggled with `hidden` that also gets a `display` rule needs the same `[hidden]` override.
- Keep the doctype, charset and viewport meta above. Without them, mobile layout and touch break.

### Page structure

- `<canvas id="game">`, fixed and full-bleed, sized in `resize()` with `devicePixelRatio` capped at 2.
- A `.hud` strip on top with `pointer-events: none`. Only the pause button (`#pauseBtn`) takes pointer events.
- Modal screens are `.overlay` > `.card` elements toggled with the `hidden` attribute: `startScreen`, `upgradeScreen`, `pauseScreen`, `overScreen`.
- A `.banner` element for short announcements (`showBanner(txt)`).

### Script layout (sections marked `// ---------- Name ----------`)

1. **Setup**: canvas, `$ = id => document.getElementById(id)`, `reduceMotion` from `prefers-reduced-motion`, color table `C`, world size constants.
2. **Input**: keyboard map `keys{}` (lowercased `e.key`); a **floating touch joystick** where `pointerdown` anywhere on the canvas sets the origin and dragging steers, with pointer capture and a single active pointer id. Touch, dblclick, contextmenu and gesture events are `preventDefault`ed on the canvas so phones don't zoom, select text or show the loupe.
3. **Save file**: `store(k, v)` wraps localStorage in try/catch. One autosave slot with a `SAVE_VERSION`, a whitelist of saved fields, and validation on load (`loadSave` returns null on anything odd). Bump the version when the saved shape changes.
4. **State**: one global `S` object built by `newRun(save)`. `S.state` is `'play' | 'paused' | 'upgrade' | 'over'`; `S = null` means the menu is showing.
5. **Content tables**: data-driven definitions (`TYPES`, `BOSSES`, `UPGRADES` with `apply`, `max`). Add content by adding table entries, not new code paths.
6. **Helpers**: `burst()` particles (capped), `damageEnemy()`, `hurtPlayer()` with i-frames.
7. **Update(dt)**: movement, spawning, firing, collisions, FX, then the stage-clear check.
8. **Draw()**: world drawn under a camera translate centered on the player, with screen shake disabled under reduced motion. Screen-space UI (boss bars, off-screen hints, joystick) is drawn after `ctx.restore()`.
9. **HUD / flow**: `updateHud()`, `gameOver()`, `togglePause()`, `showMenu()`, `begin(save)`.
10. **Loop**: `requestAnimationFrame` with `dt` clamped to 0.05 s. `update` runs only while `S.state === 'play'`; `drawIdle()` runs when `S` is null.

### Behavior every game should keep

- Desktop: WASD/arrows; **P or Esc toggles pause**. Phone: drag anywhere to steer, **II** button to pause.
- Auto-pause on `visibilitychange` when the tab is hidden.
- Pause screen has Resume and "Quit to menu"; the start screen shows Continue when a valid save exists.
- Best score is kept in localStorage under a game-specific key (Swarm Arena uses `swarm-best` and `swarm-save`). New games must use their own key prefix so saves don't collide.
- Focus the primary button when an overlay opens, and give buttons visible `:focus-visible` outlines.
- Honor `prefers-reduced-motion`: no shake, no wobble, no CSS transitions.
- Layout works at phone width, and HUD padding respects `env(safe-area-inset-*)`.

### Visual style

Dark arcade look, with colors as CSS custom properties on `:root` (`--void`, `--floor`, `--grid`, `--ink`, `--muted`, `--hero`, `--heat`, `--gold`, `--violet`), mirrored in the JS `C` table for the canvas. Fonts: **Bungee** for display text, **Chakra Petch** for body text. Canvas shapes glow with `shadowBlur`, so reset it to 0 after use.

## Working in this repo

- Write README text in plain user-facing language. Update the README when gameplay, controls or saves change.
- Verify changes by opening the page in headless Chromium (Playwright is pre-installed in cloud sessions) at both phone and desktop sizes, and check that overlays hide and touch input reaches the canvas.
- Don't put model names in commits, code or docs.
