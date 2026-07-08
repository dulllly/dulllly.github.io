# ContractYear

ContractYear is a React analytics app that measures whether NBA players statistically perform better in the final season of their contract.

## What is the contract year effect?

The "contract year effect" is the popular theory that NBA players elevate their performance in the final season of their contract — the year immediately before they hit free agency — in order to maximize their next payday. ContractYear tests this theory empirically instead of anecdotally: it compares each player's stats in their contract-ending seasons against their own career baseline (their average performance in every other season), so every player acts as their own control. It then aggregates those deltas across the league and checks whether "is this a contract year?" is actually correlated with a scoring bump, using Pearson correlation rather than gut feel.

## Tech stack

- **React 18** — UI and component state
- **Vite** — dev server and production build tooling
- **Axios** — REST client for the ESPN API
- **Recharts** — scatter plot and career stat charts (data visualization)
- **Lodash** — data-shaping utilities
- **Tailwind CSS** — styling
- **PostCSS / Autoprefixer** — CSS processing for Tailwind

## How it works

ContractYear's data pipeline has four stages:

1. **ESPN API** — `src/data/espnApi.js` calls ESPN's public roster and athlete-stats endpoints to fetch team rosters and per-season stat lines (points, rebounds, assists, shooting splits) for every player.
2. **Roster cache** — `src/data/cache.js` + `src/hooks/useRosterCache.js` preload all 30 team rosters on app start and cache the combined player list in `localStorage` for 24 hours, so repeat visits and searches don't re-hit the network.
3. **Analytics engine** — `src/analytics/contractYear.js` and `src/analytics/statistics.js` combine each player's season stats with their manually curated contract history (`src/data/contracts.js`) to compute per-season deltas against a player's non-contract-year baseline, league-wide averages, and the Pearson correlation between "is contract year" and scoring delta.
4. **UI** — the three sidebar pages (Player Lookup, Leaderboard, Correlation Explorer) render that output as searchable player cards, a sortable league-wide table, and an interactive scatter plot.

## Key findings

_Placeholder — replace with real numbers once the full dataset has been reviewed._

> Across 40 players, contract year seasons showed an average PPG delta of +X.X compared to each player's own career baseline, with a Pearson correlation of r = X.XX between contract-year status and scoring change.

## Run locally

```bash
git clone <your-repo-url>
cd contractyear
npm install
npm run dev
```

The app will be available at `http://localhost:5173`.

To create a production build:

```bash
npm run build
npm run preview
```

## Data sources

- **Player and team stats** are pulled live from ESPN's public NBA API (`site.api.espn.com` and `site.web.api.espn.com`). All credit for underlying roster and statistical data goes to ESPN.
- **Contract data** (`src/data/contracts.js`) is **manually curated** for this MVP — contract start/end years were entered by hand from public reporting and are not pulled from a live contracts API. Treat contract-year classifications as best-effort, not authoritative.
