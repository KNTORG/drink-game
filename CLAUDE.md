# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Thai-language party drinking dice game. Single-file app (`dice-game.html`) — all HTML, CSS, and JavaScript live together with no build tools, frameworks, or dependencies.

## Running

```bash
# Open directly
xdg-open dice-game.html

# Or serve locally
python -m http.server 8000
```

No build step required.

## Architecture

Everything is in `dice-game.html` (~784 lines):
- `<style>` block: CSS with 3D perspective transforms for dice animation, mobile-first layout
- `<script>` block at end of `<body>`: all game logic

Key JS globals and their roles:
- `pipLayouts` — maps face number (1–6) to a 9-element array (pip on/off grid)
- `faceRotations` — maps face number to `{rotateX, rotateY}` for CSS 3D transform
- `fourPlusFourMode` (`'A'`|`'B'`) — settings toggle: mode B makes double-4 trigger "right drinks" instead of "point at someone"
- `isRolling` — guard flag preventing double-rolls during animation
- `history` — in-memory array (max 15 entries), never persisted

Core functions:
- `getResult(sum, d1, d2)` — returns `{type, text}` for the roll outcome; all game rules live here
- `roll()` — orchestrates animation (1200ms), then calls `getResult()` and updates UI
- `setPipLayout(diceFace, number)` — renders pips on a given face element
- `setDiceRotation(dice, number)` — applies CSS 3D transform to show the correct face

## Game Rules (in `getResult`)

| Condition | Result |
|-----------|--------|
| d1 === d2 (not 4+4 mode B) | Gold flash — roller points at someone |
| d1 === 4 && d2 === 4 && mode B | Right player drinks |
| sum === 7 | Left player drinks |
| sum === 8 | Right player drinks |
| sum === 9 | Roller drinks |
| anything else | Nobody drinks |

## Conventions

- 4-space indentation throughout
- `const`/`let`, no `var`; camelCase variables/functions; CSS classes in kebab-case
- UI text is in Thai; comments may be Thai or English
- DOM queries are cached at script start via `getElementById`/`querySelector`
