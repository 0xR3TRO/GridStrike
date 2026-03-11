# GridStrike — Rules

## Tic-Tac-Toe Basics

Tic-Tac-Toe is a two-player strategy game played on a 3×3 grid. Players take turns marking cells with their symbol — **X** always goes first, followed by **O**.

### Objective

Be the first player to place three of your marks in a horizontal, vertical, or diagonal row.

### How a Round Ends

| Outcome  | Condition                                                              |
| -------- | ---------------------------------------------------------------------- |
| **Win**  | A player completes a row of three (horizontal, vertical, or diagonal). |
| **Draw** | All nine cells are filled and no player has three in a row.            |

---

## GridStrike-Specific Rules

### Game Modes

1. **Player vs Player (PvP)** — Two human players share the same device, alternating turns.
2. **Player vs CPU (PvCPU)** — A human player (X) plays against the computer (O). The CPU difficulty can be set to **Easy** or **Smart**.

### Turn Order

- **X** always moves first at the start of every round.
- In PvCPU mode, the human player is always X.

### Scoring

GridStrike keeps a running tally per session:

- **X wins** — incremented when X completes a winning line.
- **O wins** — incremented when O completes a winning line.
- **Draws** — incremented when the board fills without a winner.

Scores persist until the user clicks **Reset Scores**.

### Post-Round Behavior

- After a win or draw, further cell clicks are ignored.
- The winning line is highlighted on the board.
- Click **Restart Round** to begin a new round with the current scores intact.

### Settings Persistence

The selected game mode, difficulty level, and theme preference are saved to `localStorage` and automatically restored on the next visit.
