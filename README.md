# 🕹️ Jacksan Arcade

A free, single-file browser game arcade with a black-and-orange theme. Ten classic games, a searchable game hub, and per-game best scores that are saved on your device. No downloads, no accounts, no build step: open one HTML file and play.

## Features

- **10 built-in games** across four genres: Arcade, Puzzle, Strategy and Reflex
- **Game hub** with a featured game, genre rows, genre filter buttons and live search
- **Best scores saved locally** for each game (stored in the browser's `localStorage`)
- **Keyboard, mouse and touch support** in every game
- **Responsive layout** that works on desktop, tablet and phone
- **Accessible controls**: focus outlines, ARIA labels on buttons, and Escape to close a game
- **Zero dependencies**: plain HTML, CSS and vanilla JavaScript in one file

## Quick start

1. Download or clone this repository.
2. Open `jacksan-arcade.html` in any modern browser (Chrome, Edge, Firefox, Safari).
3. Pick a game and play.

To serve it locally instead (optional):

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/jacksan-arcade.html
```

To host it online, upload the file to any static host (GitHub Pages, Netlify, Cloudflare Pages, a school web server, etc.).

## Games

| Game | Genre | Controls | Scoring |
|------|-------|----------|---------|
| 🐍 Snake | Arcade | Arrow keys / WASD or on-screen pad · Space to pause | Points |
| 🧱 Breakout | Arcade | ← → or mouse/finger to move paddle · Space to launch | Score |
| 🏓 Pong | Arcade | ↑ ↓ or W / S, or mouse · Space to serve | Your points; first to 7 beats the CPU |
| 🧱 Tetris | Puzzle | ← → move · ↑ rotate · ↓ soft drop · Space hard drop · P pause | Score |
| 🔢 2048 | Puzzle | Arrow keys / WASD or swipe | Score |
| 🃏 Memory Match | Puzzle | Click or tap two cards | Fewer moves is better |
| 💣 Minesweeper | Strategy | Click to reveal · Right-click or long-press to flag | Faster time is better |
| ❌ Tic-Tac-Toe | Strategy | Click or tap a square (you are X and move first) | Win streak |
| 🔨 Whack-a-Mole | Reflex | Click or tap the moles | +10 per hit, −5 per miss, 30-second round |
| 🐤 Flap | Reflex | Space, ↑ or tap/click | Pipes passed |

## Keyboard shortcuts (hub)

| Key | Action |
|-----|--------|
| `/` | Focus the search box |
| `Esc` | Close the current game and return to the hub |

## Project structure

The whole arcade lives in `jacksan-arcade.html`. It is organised as a series of `<script>` blocks:

1. **Shared core**: `ArcadeStorage` (best-score persistence) and `ArcadeUI` (helpers for building HUDs, stats and messages).
2. **One script per game**: each game registers itself on `window.ArcadeGames`.
3. **Hub script**: reads the registry, renders the featured game, genre rows, search and the game overlay.

## Adding a new game

Each game is a small object pushed onto `window.ArcadeGames`. Add a new `<script>` block before the hub script (the last one in the file):

```html
<script>
(function () {
  "use strict";
  window.ArcadeGames = window.ArcadeGames || [];

  window.ArcadeGames.push({
    id: "mygame",              // unique, used as the best-score key
    title: "My Game",
    genre: "Arcade",           // Arcade | Puzzle | Strategy | Reflex (or a new genre)
    icon: "🎮",
    blurb: "One-line description shown on the hub.",
    help: "Controls shown in the game panel.",
    hue: 120,                  // optional: tile colour (0–360)
    // lowerIsBetter: true,    // optional: for time/move-count scores
    // featured: true,         // optional: show on the hub banner

    mount(container, api) {
      // Build your game inside `container`.
      // Call api.report(score) when a round ends to save a best score.
      // Return a cleanup function that stops timers and event listeners.
      return function destroy() {};
    }
  });
})();
</script>
```

**Tips**

- Use `window.ArcadeUI.stat("Score")` and `window.ArcadeUI.setMessage(...)` so your game matches the others.
- Always clear timers, intervals and global listeners in the returned `destroy` function, or they will keep running after the game closes.
- Genres appear in the nav bar automatically.

## Browser support

Any modern evergreen browser with `localStorage` and CSS grid support. If `localStorage` is blocked (for example in private mode), the games still work but best scores will not be saved.

## Known limitations

- Best scores are per-browser and per-device; they don't sync.
- There is no sound.

## Contributing

Bug reports and new game ideas are welcome. Open an issue or pull request with a short description and, for bugs, the browser and steps to reproduce.
