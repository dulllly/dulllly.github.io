# ContractYear

Does an NBA player actually play better in the last year of their contract, or is that just something announcers say? ContractYear pulls real season stats and checks.

## What is the contract year effect?

You've heard the theory: a player's about to hit free agency, so they lock in and put up a career year to cash in on the next deal. It's one of the most repeated storylines in NBA media, and it's almost never actually checked against numbers.

ContractYear checks it. For every player in the dataset, it takes their stats in the final season of each contract and compares them against that same player's average in every *other* season — so nobody's being compared to LeBron, everyone's just being compared to themselves. Those deltas get aggregated across the league, and then run through a Pearson correlation to see if "this is a contract year" is actually related to a scoring bump, or if it's just noise that happens to make a good story.

## Tech stack

- React 18
- Vite
- Axios (talks to the ESPN API)
- Recharts (the career chart and the correlation scatter plot)
- Lodash
- Tailwind CSS + PostCSS/Autoprefixer

## How it works

There's no backend — it's all client-side. Roughly:

**ESPN API → roster cache → analytics engine → UI**

1. `src/data/espnApi.js` hits ESPN's public roster and stats endpoints for every team and player.
2. `src/hooks/useRosterCache.js` preloads all 30 rosters once and stashes them in `localStorage` for a day, so you're not re-fetching 30 teams every time you search for a player.
3. `src/analytics/contractYear.js` and `statistics.js` do the actual math — baseline averages, per-season deltas, and the Pearson correlation.
4. The three sidebar pages (Player Lookup, Leaderboard, Correlation Explorer) just render whatever that math spits out.

## Key findings

_Placeholder until I've gone through the full dataset properly:_

> Across 40 players, contract year seasons showed an average PPG delta of +X.X vs. career baseline, with a Pearson correlation of r = X.XX between contract-year status and scoring change.

## Run locally

```bash
git clone https://github.com/dulllly/contractyear.git
cd contractyear
npm install
npm run dev
```

Opens at `http://localhost:5173`.

Want a production build instead?

```bash
npm run build
npm run preview
```

## Data sources

Stats and rosters come straight from ESPN's public NBA API — full credit to them for the underlying data.

Contract history (`src/data/contracts.js`) is a different story: there's no free, reliable API for historical NBA contract data, so those 40 players' contract years were entered by hand from public reporting for this MVP. Treat it as a solid best-effort, not gospel.
