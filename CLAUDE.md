# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running

Plain HTML5 Canvas game: no build, no bundler, no dependencies, no tests, no linter. Open `index.html` in a browser, or serve the folder with `npx serve .` (http://localhost:3000). Reload the page to see changes.

## Architecture

- `index.html` only defines an 800×600 `<canvas id="canvas">` and loads `game.js`.
- All game logic lives in the single script `game.js`, with no modules. It is organized in banner-commented sections: Input, Utils, `Bullet`, `Asteroid`, `Ship`, `Particle`, then game state and functions (`initGame`, `nextLevel`, `spawnAsteroids`, `explode`, `killShip`), `update(dt)`, HUD/draw functions, and the `requestAnimationFrame` loop at the bottom.
- Game state is module-level `let` variables (`ship`, `bullets`, `asteroids`, `particles`, `score`, `lives`, `level`) plus a `state` string: `'playing' | 'dead' | 'gameover'`.
- Every entity has `update(dt)` / `draw()` and a `dead` flag. `dt` is in seconds, and speeds are in px/s.
- The world is toroidal: positions use the `wrap(v, max)` helper against `W`/`H`.
- Input: `keys[code]` holds the current held state. `pressed(code)` is a one-shot "just pressed" read that consumes the flag.
- Asteroid size is 1–3 (3 = large). Per-size data is in the index-by-size arrays `RADII`, `SPEEDS` and `POINTS`, whose index 0 is a placeholder. `split()` spawns two asteroids of `size - 1`.

## Notes

- Code comments and the README are in Spanish. Match that when adding comments.
- The README mentions power-ups and a "shooting star" asteroid type, but `game.js` does not currently implement them.
