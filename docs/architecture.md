# GridStrike — Architecture

## Overview

GridStrike is a single-page application built with vanilla HTML, CSS, and JavaScript. All game logic lives in `script.js`, organized into six cooperating modules using the **Revealing Module Pattern** (IIFEs that return public APIs).

---

## Module Map

```
┌──────────┐   reads/writes   ┌────────────┐
│ Storage  │◄────────────────►│ localStorage│
└────┬─────┘                  └────────────┘
     │ used by
     ▼
┌────────────┐   state queries   ┌────────────┐
│ GameState  │◄─────────────────►│ GameLogic  │
└────┬───────┘                   └─────┬──────┘
     │                                 │
     │         ┌──────┐               │
     │         │  AI  │───────────────┘
     │         └──┬───┘       calls checkWin / getAvailable
     │            │
     ▼            ▼
┌──────────────────────┐
│        Events        │  ← user interactions (clicks, key presses)
└──────────┬───────────┘
           │ calls
           ▼
┌──────────────────────┐
│          UI          │  → DOM rendering, animations, theme
└──────────────────────┘
```

---

## Modules in Detail

### 1. Storage

| API                   | Description                                                                   |
| --------------------- | ----------------------------------------------------------------------------- |
| `save(key, value)`    | JSON-serializes and persists a value to `localStorage`.                       |
| `load(key, fallback)` | Reads and deserializes a stored value; returns `fallback` on miss.            |
| `KEYS`                | Constant map of storage key strings (`gs_mode`, `gs_difficulty`, `gs_theme`). |

### 2. GameState

Holds all mutable game data via getter/setter properties:

- `board` — Array of 9 strings (`""`, `"X"`, or `"O"`).
- `currentPlayer` — `"X"` or `"O"`.
- `gameActive` — Boolean flag; `false` after a win or draw.
- `mode` — `"pvp"` or `"pvcpu"`.
- `difficulty` — `"easy"` or `"smart"`.
- `scores` — `{ X: number, O: number, draws: number }`.
- `reset()` — Clears the board and restores player X.
- `resetScores()` — Zeros all score counters.

Setting `mode` or `difficulty` automatically persists the value through `Storage`.

### 3. GameLogic

Pure functions (no side-effects except `makeMove`):

| Function                  | Returns                                |
| ------------------------- | -------------------------------------- |
| `checkWin(board, player)` | Winning pattern `[i, j, k]` or `null`. |
| `checkDraw(board)`        | `true` if every cell is filled.        |
| `getAvailable(board)`     | Array of empty-cell indices.           |
| `makeMove(index)`         | `true` if move was legal and applied.  |

### 4. AI

Exports a single function:

- `getMove(board)` — Returns the index of the cell the CPU should play.

**Easy mode** picks uniformly at random from available cells.

**Smart mode** follows a priority heuristic:

1. Complete a winning line for the AI.
2. Block the opponent's winning line.
3. Take the center cell (index 4).
4. Take a random open corner (indices 0, 2, 6, 8).
5. Take a random open side (indices 1, 3, 5, 7).

This is not a perfect (minimax) player — it can be beaten — but it plays competently and blocks obvious threats.

### 5. UI

Handles all DOM reads/writes:

| Function                        | Effect                                             |
| ------------------------------- | -------------------------------------------------- |
| `renderBoard()`                 | Rebuilds the 3×3 grid from `GameState.board`.      |
| `updateStatus(msg)`             | Sets the status text.                              |
| `updateScores()`                | Syncs scoreboard counters with `GameState.scores`. |
| `highlightWin(pattern)`         | Adds the `.win` CSS class to winning cells.        |
| `initTheme()` / `toggleTheme()` | Reads/sets `data-theme` on `<html>` and persists.  |

### 6. Events

Wires DOM events to game logic:

- **Cell click/keydown** → `makeMove` → `processAfterMove` → (optional) CPU turn with 350 ms delay.
- **Mode selector** → updates `GameState.mode`, toggles difficulty panel, restarts round.
- **Difficulty selector** → updates `GameState.difficulty`, restarts round.
- **Restart Round** → `GameState.reset()` + re-render.
- **Reset Scores** → `GameState.resetScores()` + `reset()` + re-render.
- **Theme toggle** → `UI.toggleTheme()`.

---

## Data Flow (Single Turn)

```
User clicks cell
  → Events.handleCellClick
    → GameLogic.makeMove (mutates board)
    → Events.processAfterMove
      → UI.renderBoard
      → GameLogic.checkWin  → if win: update scores, highlight, status
      → GameLogic.checkDraw → if draw: update scores, status
      → else: switch player, update status
    → if PvCPU & O's turn: setTimeout(cpuTurn, 350)
      → AI.getMove → GameLogic.makeMove → processAfterMove (same flow)
```
