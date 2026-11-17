# Mini Google — The Search Revolution

Website - https://mini-google-search-revolution.vercel.app/

An interactive, local-data simulation of document lookup, keyword search, and PageRank. The sample websites and archive documents are fictional. This is an educational model, not Google's complete 1998 ranking system.

## Run locally

Requires Node.js 18+ and npm. From this folder:

```bash
npm install
npm run dev
```

Open the local address printed by Vite. Run `npm test` for algorithm checks and `npm run build` to create a static `dist/` bundle. No backend, account, API key, or database is used.

## Try the simulation

Search for `pizza` to see a low-value page win by repeating a keyword. Switch eras to inspect a small local document archive or compare two rankings of the same fictional web pages. Open **How PageRank Works** to select a page, inspect its incoming and outgoing links, and edit its outgoing links. **Add a webpage** creates a new node with content and chosen links. Changes last for the current session; **Reset demo** restores the original dataset.

## Ranking model

- `src/algorithms/search.js` tokenizes lowercase words and counts exact whole-word query matches. The old ranking sorts by raw count, then ID for deterministic ties.
- `src/algorithms/pagerank.js` constructs a directed graph of valid distinct links. It starts with equal probability on each page, then repeatedly applies `PR(A) = (1-d)/N + d * sum(PR(T)/L(T))` with `d = 0.85`. Dangling nodes distribute their probability equally over all nodes. Iteration ends when the sum of absolute score changes falls below `1e-12` or after 1,000 steps.
- The link-aware ranking first limits results to pages containing at least one query word. Its teaching score is `0.35 × log-scaled keyword count + 0.65 × PageRank / highest PageRank`. The keyword component is divided by the highest log-scaled count among matching pages. This deliberately reduces the payoff for repetition while showing how graph importance changes order. These weights are illustrative, not historical Google weights.
- All calculations derive from the current page data on each edit, including added pages and changed links. Scores are stable for a given dataset and query.

The 1980s mode searches six local text documents and does not use the web graph.

## Project structure

`src/data.js` holds the 15 sample web pages and six archive documents. `src/algorithms/` contains standalone search and PageRank functions and tests. `src/main.jsx` renders the React interface; `src/style.css` includes Tailwind directives and the retro theme. Vite handles local development and production builds.
