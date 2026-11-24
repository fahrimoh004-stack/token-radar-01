# Token Radar — radar-2

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/fahrimoh004-stack/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/fahrimoh004-stack/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

The best token launch radar: every pre-token protocol in DefiLlama's sealed
snapshot, ranked by what its launch would be worth, with a farm plan for each.

## The model (radar-2)

Implied value is calibrated on the snapshot's own live market, not on a flat
multiple and never on a hardcoded list of names:

- **Category multiple** — the median market-cap / TVL of the live protocols in
  the same category, when at least 3 of them trade, shrunk toward the
  market-wide median and clamped to [0.05, 0.60]. A thinner category uses the
  market median outright, so one live outlier cannot price a whole category.
- **Growth** — `1 + clamp(change_7d, -60, 120) / 100 × 0.5`.
- **Maturity** — `0.6 + min(age_days, 1095) / 1095 × 0.4` (unknown age: 365 days).
- **Breadth** — `1 + 0.015 × min(chains − 1, 10)`.

`implied = round(TVL × category_multiple × growth × maturity × breadth)`

The board ranks every pre-token protocol that is not a wrapped, staked or
bridged product of a token that already trades — the watchlist's exclusion
rule applied to every row, not only to the watchlist. WBTC, Binance staked
ETH, USDT0 and the bridges never top this board: each is a token that
already exists. Live-token protocols are set aside with their market caps.

## The dashboard

A static page, no network: a top-10 spotlight, a sortable and filterable
board (search, category, chain, watchlist-only, 50 rows a page), a detail
view for every protocol with its model breakdown and six-step farm plan,
a live-token view, and the full calibration table under Method.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

`main.ts` (Node 22, standard library only) reads the sealed snapshot,
writes `board.json`, `data.js`, copies `dashboard/` and `screenshots/`,
and writes `dossier.json` with the vendored `vendor/makefun-board.ts`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=htq958hk&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
