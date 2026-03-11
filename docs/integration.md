# GridStrike — Integration Guide

Embed GridStrike as a self-contained widget in any web page or application.

---

## Option 1: iframe Embed

The simplest approach — no code changes required.

```html
<iframe
    src="path/to/gridstrike/index.html"
    width="500"
    height="720"
    style="border: none; border-radius: 16px; overflow: hidden;"
    title="GridStrike — Tic-Tac-Toe"
></iframe>
```

### Advantages

- Complete isolation — no CSS or JS conflicts with the host page.
- Works with any framework or CMS.

### Considerations

- The iframe dimensions should match the app's responsive breakpoints. At **500×720** the layout is comfortable; narrower works down to ~320 px wide.
- `localStorage` inside the iframe is scoped to the iframe's origin.

---

## Option 2: Direct Embed

Include GridStrike's files directly in your page for tighter integration.

### Steps

1. **Copy assets** — place `styles.css`, `script.js`, and the `assets/` folder into your project.

2. **Add the CSS** — link the stylesheet in your `<head>`:

    ```html
    <link rel="stylesheet" href="gridstrike/styles.css" />
    ```

3. **Add the HTML** — paste the contents of `<div class="app">…</div>` from `index.html` into your page wherever you want the game to appear.

4. **Add the script** — place the `<script>` tag at the end of your `<body>`:

    ```html
    <script src="gridstrike/script.js"></script>
    ```

### Scoping Styles

To prevent CSS leaks, wrap the widget in a container and scope the styles:

```html
<div class="gridstrike-widget">
    <!-- GridStrike HTML here -->
</div>
```

Then prefix all GridStrike selectors with `.gridstrike-widget`:

```css
.gridstrike-widget .app { ... }
.gridstrike-widget .board { ... }
```

---

## Option 3: Web Component (Advanced)

For maximum encapsulation, wrap GridStrike in a custom element with Shadow DOM:

```js
class GridStrikeGame extends HTMLElement {
    constructor() {
        super();
        const shadow = this.attachShadow({ mode: "open" });
        // Load styles
        const link = document.createElement("link");
        link.rel = "stylesheet";
        link.href = "gridstrike/styles.css";
        shadow.appendChild(link);
        // Load HTML
        fetch("gridstrike/index.html")
            .then((r) => r.text())
            .then((html) => {
                const parser = new DOMParser();
                const doc = parser.parseFromString(html, "text/html");
                const app = doc.querySelector(".app");
                shadow.appendChild(app);
                // Load and execute script within shadow context
                const script = document.createElement("script");
                script.src = "gridstrike/script.js";
                shadow.appendChild(script);
            });
    }
}
customElements.define("gridstrike-game", GridStrikeGame);
```

Usage:

```html
<gridstrike-game></gridstrike-game>
```

> **Note:** The Web Component approach requires adjusting `script.js` to query elements from the shadow root instead of `document`. This is an advanced integration path.

---

## Configuration via URL Parameters

You can extend GridStrike to accept URL search parameters for pre-configuration:

| Parameter    | Values          | Effect                     |
| ------------ | --------------- | -------------------------- |
| `mode`       | `pvp`, `pvcpu`  | Sets the initial game mode |
| `difficulty` | `easy`, `smart` | Sets the CPU difficulty    |
| `theme`      | `light`, `dark` | Sets the color theme       |

Example:

```
gridstrike/index.html?mode=pvcpu&difficulty=smart&theme=dark
```

This requires a small addition to the bootstrap code in `script.js` to read `URLSearchParams` — a straightforward enhancement.
