# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

```sh
open index.html
# or
python3 -m http.server 8081 && open http://localhost:8081
```

No build step, no dependencies, no tests. The entire game is a single self-contained HTML file.

## Files

- `index.html` — canonical version: authentic arcade style, CRT overlay, pixel-art sprites
- `game.html` — older version with a modern neon aesthetic; not the primary target for changes

## Architecture

Everything lives in the `<script>` block of `index.html`, organized into these sections (marked with `// ── NAME ──` banners):

| Section | What it does |
|---|---|
| AUDIO | Web Audio API helpers: `tone()`, `noise()`, named sound functions. AudioContext must be initialised on the first keydown (`initAC()`). |
| PIXEL ART SPRITES | Sprite data as 2-D bit arrays. `SP = 4` scales art-pixels to canvas pixels. `drawSprite()` and `drawAlienSprite()` iterate the arrays with `fillRect`. |
| PALETTE | Named colour constants (`C_GREEN`, `C_WHITE`, etc.) — use these everywhere instead of raw hex. |
| CONSTANTS | Canvas size (800×700), grid dimensions, speeds, and `BARRIER_SHAPE`. |
| GAME STATE | All mutable state is module-level `let` variables — no state object. |
| INPUT | `keys` object set/cleared by `keydown`/`keyup` listeners; polled each frame. |
| INIT | `startGame()` → `initLevel()` + `initBarriers()`. `calcAlienSpeed()` recomputes alien speed whenever an alien dies. |
| UPDATE | Delta-time game loop. Alien march beat (sound + sprite flip) is a separate timer from continuous horizontal movement. |
| checkCollisions | AABB checks: player bullet vs aliens/UFO/barriers; alien bullets vs barriers/player. |
| DRAW | State-dispatched: MENU → `drawMenu()`, OVER → `drawGameOver()`, otherwise full game scene + `drawCRT()`. |
| LOOP | `requestAnimationFrame` loop; `dt` is clamped to 80 ms to survive tab-blur pauses. |

## Key design rules

- **One bullet at a time** — `playerBullet` is a single object or `null`; no array.
- **Alien speed** — `alienMoveInterval` (march beat, ms) and `alienMoveSpeed` (px/s) are both updated by `calcAlienSpeed()` based on surviving alien count and level. Editing one without the other breaks pacing.
- **Barrier erosion** — each `barriers` entry has `hp: 3`; rendering maps hp → green intensity. `hp === 0` blocks are skipped in draw and collision.
- **AudioContext lifecycle** — Chrome requires a user gesture; `initAC()` is called on the first `keydown`. All sound functions guard with `if (!AC || AC.state !== 'running')`.
- **CRT overlay** — `drawCRT()` must be the last draw call; it uses `globalAlpha` so order matters.
