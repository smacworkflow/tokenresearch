# Token Research Terminal 

Static site for interactive crypto token deep-research reports. Live market data (CoinGecko + DeFiLlama) layered over per-ticker research JSONs. No build step, no backend.

## Deploy (GitHub Pages, ~5 minutes)

1. Create a new public repo (e.g. `tokenresearch`).
2. Upload everything in this folder (`index.html`, `reports/`, `README.md`) — drag the whole folder into GitHub's "upload files" page, or `git push`.
3. Repo → **Settings → Pages** → Source: **Deploy from a branch** → `main` / root.
4. Site goes live at `https://<username>.github.io/tokenresearch/`.

Deep-link a report with `?t=LIT`.

## How it works

- **Analysis layer** — `reports/{ticker}.json` holds a full research run (thesis, tokenomics, catalysts, scenarios, valuation, liquidity depth). Stamped "RESEARCHED {date}".
- **Live layer** — on load, the page fetches current price/mcap/FDV/volume, 365d daily price history (vs. BTC/ETH/SOL), and monthly protocol fees/revenue. Elements that refreshed show a LIVE badge; anything else is the research snapshot. All fetches fail gracefully back to snapshot.
- **Any other ticker** — type it in the search box: you get a live-data-only view (price, caps, chart) with a note that no research run exists yet.

## Adding a new ticker

1. Ask Claude to run the deep-research template on the ticker.
2. Save the produced JSON as `reports/{ticker}.json` (schema: copy `lit.json`). Make sure `live.coingeckoId` and `live.llamaSlug` are set (find them in the CoinGecko URL slug and DeFiLlama protocol URL slug).
3. Add an entry to `reports/index.json`.
4. Commit. The dropdown picks it up automatically.

To refresh an existing report, re-run the research and replace the JSON (bump `researchedAt`).

## API notes

- CoinGecko free tier is rate-limited (~5–15 req/min). The page staggers calls; if you hit limits the badge shows SNAPSHOT — reload after a minute. Optional: get a free demo API key and run `localStorage.setItem('cgKey','YOUR_KEY')` in the browser console once.
- DeFiLlama's API is open and unauthenticated.
- Order-book depth (±2% curve, execution simulator anchor) has no free live API — it's captured per research run. The simulator's ADV updates live from CoinGecko's 24h volume.

## Disclaimer

Research output + model estimates, not investment advice. Liquidity simulator numbers are illustrative, not execution quotes.

