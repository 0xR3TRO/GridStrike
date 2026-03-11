# GridStrike — Customization

This guide explains how to customize GridStrike's appearance and behavior.

---

## Changing Colors

All colors are defined as CSS custom properties in `styles.css` under `:root` (light theme) and `[data-theme="dark"]` (dark theme).

### Key Variables

| Variable    | Purpose                                             |
| ----------- | --------------------------------------------------- |
| `--accent`  | Primary brand color (buttons, title, active states) |
| `--x-color` | Color of the X mark                                 |
| `--o-color` | Color of the O mark                                 |
| `--win-bg`  | Background color of winning cells                   |
| `--bg-body` | Page background                                     |
| `--bg-card` | Card/panel background                               |
| `--bg-cell` | Cell background                                     |

To change the accent to blue, for example:

```css
:root {
    --accent: #0984e3;
    --accent-hover: #0770c2;
    --accent-light: rgba(9, 132, 227, 0.12);
}
```

### Adding a New Theme

1. Define a new `[data-theme="your-theme"]` block in `styles.css` with all the custom property overrides.
2. In `script.js`, update the `UI.toggleTheme` method (or add a multi-theme switcher) to cycle through your themes.

---

## Changing Board Size

The board width is set with `width: min(340px, 85vw)` on `.board`. Adjust this value and the `gap` / `padding` properties to resize.

Cell font size (`font-size: 2.4rem` on `.cell`) should be adjusted proportionally.

---

## Changing Fonts

Replace the `--font` variable in `:root`:

```css
:root {
    --font: "Inter", sans-serif;
}
```

Don't forget to add the corresponding `<link>` or `@import` for the font in `index.html` or `styles.css`.

---

## Changing Animations

### Cell Pop

The `cellPop` keyframe in `styles.css` controls the animation when a mark is placed:

```css
@keyframes cellPop {
    0% {
        transform: scale(0.5);
        opacity: 0.3;
    }
    60% {
        transform: scale(1.1);
    }
    100% {
        transform: scale(1);
        opacity: 1;
    }
}
```

Adjust timing values or add rotation, color shifts, etc.

### Win Pulse

The `winPulse` keyframe controls the winning-cell animation. Modify it to add bounce, glow, or shake effects.

---

## Adjusting AI Difficulty

### Easy Mode

In `script.js`, the `easyMove` function picks a random open cell. You could bias it (e.g., avoid the center) to make it even easier.

### Smart Mode

The `smartMove` function uses a fixed priority heuristic. To make it harder, you could replace it with a **minimax** algorithm (guaranteed perfect play). To make it more interesting, introduce a random chance to skip the optimal move.

---

## Changing the CPU Delay

The CPU move delay is `350` ms (in `Events.handleCellClick`). Increase it for a more "thoughtful" feel, or set it to `0` for instant responses.
