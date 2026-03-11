# GridStrike

**A modern, polished web implementation of the classic Tic-Tac-Toe game** — built with vanilla HTML, CSS, and JavaScript. No frameworks, no build tools, just clean code you can open in a browser.

![License](https://img.shields.io/badge/license-MIT-blue)

---

## Features

- **Two game modes** — Player vs Player (local) and Player vs CPU
- **Two AI difficulty levels** — Easy (random moves) and Smart (heuristic strategy)
- **Live scoreboard** — tracks X wins, O wins, and draws per session
- **Winning line highlight** with animated pulse effect
- **Light & Dark themes** with one-click toggle, saved to `localStorage`
- **Responsive design** — looks great on desktop and mobile (down to 320 px)
- **Keyboard accessible** — cells are focusable and activatable via Enter/Space
- **Smooth animations** — cell pop on placement, hover effects, win pulse
- **Settings persistence** — game mode, difficulty, and theme remembered across visits
- **Zero dependencies** — pure HTML / CSS / JS

---

## Repository Structure

```
GridStrike/
├── index.html          Main HTML page
├── styles.css          All styling (light/dark themes, animations)
├── script.js           Game logic (modular architecture)
├── assets/
│   └── favicon.svg     Browser tab icon
├── docs/
│   ├── rules.md        Game rules & GridStrike-specific behavior
│   ├── architecture.md JS module map, data flow, AI strategy
│   ├── customization.md How to change colors, fonts, animations
│   └── integration.md  Embed GridStrike in another site
└── README.md           This file
```

---

## Getting Started

No build step required.

### Option A — Open directly

Double-click `index.html` in your file manager, or:

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

### Option B — Local dev server

Any static file server works:

```bash
# Python
python3 -m http.server 8080

# Node (npx)
npx serve .

# VS Code
# Install "Live Server" extension → right-click index.html → "Open with Live Server"
```

Then visit `http://localhost:8080`.

---

## Game Modes

| Mode | Description |
|------|-------------|
| **Player vs Player** | Two humans share the screen, taking turns. |
| **Player vs CPU** | Human plays as X; the computer plays as O. |

In PvCPU mode, choose a difficulty before playing:

| Difficulty | Behavior |
|------------|----------|
| **Easy** | CPU picks a random empty cell — very beatable. |
| **Smart** | CPU uses a heuristic: wins if possible, blocks your wins, prefers center and corners. Challenging but not unbeatable. |

---

## AI Behavior

The Smart AI follows a priority-based heuristic (not minimax):

1. **Win** — complete its own three-in-a-row if one move away.
2. **Block** — prevent the opponent from winning next turn.
3. **Center** — take the center cell if open.
4. **Corner** — take a random open corner.
5. **Side** — take a random open side.

This makes the AI a competent opponent that catches obvious threats, while still leaving room for the player to outmaneuver it.

---

## Customization

GridStrike is designed to be easy to restyle and extend.

- **Colors** — change CSS custom properties in `:root` and `[data-theme="dark"]`.
- **Fonts** — swap the `--font` variable and add the font import.
- **Animations** — modify `@keyframes cellPop` and `winPulse` in `styles.css`.
- **AI tuning** — adjust the `smartMove` heuristic or replace it with minimax.
- **Board size** — update the `.board` width and cell font size.

See [docs/customization.md](docs/customization.md) for detailed instructions.

---

## Future Improvements

- 4×4 and 5×5 board modes
- Online multiplayer (WebSocket / WebRTC)
- Minimax AI for a perfect (unbeatable) difficulty level
- Player name customization
- Sound effects and premium animations
- Global leaderboard / ranking system
- Progressive Web App (PWA) with offline support
- Undo / redo moves
- Game replay / history

---

## License

This project is released under the **MIT License**.

```
MIT License

Copyright (c) 2026 GridStrike Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
