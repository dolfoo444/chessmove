# Best Move Board

A self-contained chess analysis board. Paste a position (FEN) or set the pieces
up by hand, and **Stockfish 19** marks the strongest move with an arrow — the
evaluation, the principal variation, and the top alternatives, all computed
**in your browser**. Nothing is ever sent to a server.

Click pieces to play the moves out and the engine keeps re-analyzing, so you can
walk through a whole game.

![Best move shown with an arrow](docs/preview.png)

## Running it

The page uses an ES module and a Web Worker, which browsers block when you open
an HTML file directly from disk (`file://`). So serve the folder over HTTP — any
static server works:

```bash
# Python (built in on most systems)
python3 -m http.server 8000
# then open http://localhost:8000
```

```bash
# or Node
npx serve .
```

That's it — no build step, no dependencies to install.

## What's in here

| File | What it is |
| --- | --- |
| `index.html` | The whole app — layout, logic, and styling in one file. |
| `sf.js` + `sf.wasm` | Stockfish 19 Lite, single-threaded WebAssembly build. |
| `chess.js` | Move generation, FEN parsing, and SAN notation. |

The Stockfish build is the **single-threaded "lite"** flavour, chosen so it runs
on any origin without needing the cross-origin isolation headers that the
multi-threaded build requires. It's a little slower than a native engine but
still very strong.

## Hosting it as a live site

This repo is private, so GitHub Pages isn't available on a free plan. If you make
the repo public (or upgrade), you can turn on a live link in a minute:

1. Repo **Settings → Pages**.
2. Source: **Deploy from a branch**, branch **main**, folder **/ (root)**.
3. Save. Your site appears at `https://<your-username>.github.io/chess-best-move/`.

Because every file is static, Pages serves it as-is with no configuration.

## How the analysis reads

- **Evaluation** is shown from White's point of view. `+1.20` means White is
  better by about a pawn and a bit; `-0.5` favours Black. `M3` means mate in 3.
- The **green arrow** is the single best move. The two rows beneath the line are
  the next-best alternatives with their own evaluations.
- **Think time** (0.5s–5s) controls how deep the engine looks. Longer is stronger.

## Credits & licenses

- [Stockfish](https://stockfishchess.org/) and
  [stockfish.js](https://github.com/nmrugg/stockfish.js) — **GPLv3**.
- [chess.js](https://github.com/jhlywa/chess.js) — **BSD-2-Clause**.

Because the bundled Stockfish is GPLv3, this project as a whole is distributed
under the **GPLv3**. See `LICENSE`.

This is an analysis tool for study and review.
