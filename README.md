# 🐾 Animal Cursor Follower

A small, dependency-free web app: pick a favorite animal, then watch it chase your mouse (or finger) around the screen with smooth easing, bobbing, and a wagging tail.

**[Live demo →](#)** *(replace with your GitHub Pages link once deployed — see below)*

## Features

- **Selection screen** — choose from 5 hand-drawn SVG animals: Golden Retriever, Tabby Cat, Red Fox, Cottontail Bunny, and Giant Panda.
- **Smooth cursor chase** — the animal eases toward the pointer (`lerp`-style interpolation) instead of snapping to it, with a subtle bob, lean, and stretch while moving.
- **Direction-aware** — the animal flips to face the direction it's moving in.
- **Touch support** — works with `touchmove` as well as `pointermove`, so it's usable on mobile/tablet.
- **Remembers your pick** — the last animal you chose is saved in `localStorage` (wrapped in `try/catch`, so it degrades gracefully if storage is unavailable).
- **Light/dark aware** — colors are defined as CSS variables and adapt to the visitor's OS theme via `prefers-color-scheme`.
- **Zero dependencies** — pure HTML, CSS, and vanilla JavaScript. No build step, no frameworks, no external assets.

## Project structure

```
animal-cursor-follower/
├── index.html        # Markup + screen structure
├── css/
│   └── styles.css    # All styling, layout, and theme variables
├── js/
│   └── script.js      # Animal artwork data + cursor-follow animation logic
└── README.md
```

## Getting started

No build tools or installs required.

1. Clone or download this repository.
2. Open `index.html` directly in a browser, **or** serve it locally for the best experience (some browsers restrict certain APIs on `file://`):

   ```bash
   # Python 3
   python3 -m http.server 8000

   # or Node.js
   npx serve .
   ```
3. Visit `http://localhost:8000` and pick an animal.

## Deploying to GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to `Deploy from a branch`, choose your default branch (e.g. `main`) and the `/ (root)` folder.
4. Save — your app will be live at `https://<your-username>.github.io/<repo-name>/` within a minute or two.

## How it works

- Each animal is drawn as layered SVG `<path>`/`<ellipse>`/`<rect>` shapes filled with gradients (defined per-animal in `ANIMALS[key].defs()`), which is what gives them soft, shaded, more natural-looking fur/body tone instead of flat cartoon colors.
- `ANIMALS[key].build()` injects that artwork into an SVG `<g>` — once for the small thumbnail on the selection screen, and again (with a unique gradient ID prefix) for the full-size animal on the tracker screen.
- On every animation frame, the animal's position is eased toward the live pointer coordinates, and its `transform` is updated with a translate (position), scale (flip to face direction of travel), and slight rotate (lean into the turn).

## Customizing / adding a new animal

Open `js/script.js` and add a new entry to the `ANIMALS` object with the same shape as the existing ones:

```js
yourAnimal: {
  label: "Display Name",
  defs(id){ return `<!-- gradients, scoped with ${id} -->`; },
  build(g, id){ g.innerHTML = `<!-- SVG shapes using url(#${id}-...) fills -->`; }
}
```

It will automatically appear as a new card on the selection screen — no other changes needed.

## License

Feel free to use, modify, and share this project.
