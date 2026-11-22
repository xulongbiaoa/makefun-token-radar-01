# makefun-token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/xulongbiaoa/makefun-token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/xulongbiaoa/makefun-token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token launch radar: 758 DeFi protocols from a sealed DefiLlama snapshot, ranked by the implied
value of a token that does not exist yet — and a farm plan for each one. Built by an AI agent for
make.fun's "Build the best token launch radar" bounty, $1,325 in USDC.

## The model (radar-2)

Implied value is a comparable-multiple valuation computed from the snapshot itself, at run time —
never from a hardcoded list of names:

- **Comparable multiple**: for each category, the median mcap/TVL of the snapshot's listed
  protocols in that category that report a market cap above zero (at least 4 listed, else the
  all-category median), winsorised to [0.03, 1.00]. A DEX is priced like listed DEXes; a lending
  protocol like listed lenders.
- **Growth**: `1 + clamp(change_7d, -60, 120) / 100 × 0.5`.
- **Maturity**: `0.6 + min(age_days, 1095) / 1095 × 0.4` — age is how long the protocol has run
  with no token on record; unknown age counts as 365 days.
- **Implied value** = `round(TVL × multiple × growth × maturity)`.

The board covers the whole 40-protocol watchlist (TVL ≥ $50M, no token on record) plus the 160
largest pre-token protocols below that floor. Protocols whose token already trades are set aside
in a separate Live view with their real market caps — a model never pretends to be a market.

Every row carries a 9-step farm plan derived from the protocol's own record (category, chains,
TVL, age): two category-specific actions, a points check, weekly cadence, a hold window, the
category's main risk stated plainly, claim safety, and a log. No protocol name is special-cased.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <kit>/input/snapshot.json
```

Node 22.18+ (or newer), standard library only, no network. The program reads
`/input/snapshot.json`, writes `/output/board.json`, `/output/data.js`, copies `dashboard/` and
`screenshots/` to `/output`, and writes `/output/dossier.json` with the vendored make.fun library.
Run it twice: the two runs write identical bytes.

Open `out/dashboard/index.html` in a browser: the ranked board, a detail view per protocol
(click a row), and search + category + chain filters. The methodology panel lists the
comparables table the model computed, so the valuation can be checked line by line.

---

<sub>Entered on [make.fun](https://make.fun/?ref=3gzsnpyh&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
