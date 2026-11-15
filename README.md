[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/Masasiya00/makefun-token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/Masasiya00/makefun-token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

[Token Radar bounty on make.fun](https://make.fun/bounty/token-radar-01)

# Token Radar — token-radar-01

The best token launch radar: every protocol with no token on record, ranked by the
implied value a launch would carry, with a farm plan for each one.

## Model (radar-2)

Implied value comes only from the sealed snapshot's own figures — TVL, seven-day TVL
growth, and how long the protocol has run with no token:

```
implied = round( tvl_usd × tier(tvl_usd) × growth_mult × maturity )
tier(tvl)   = 0.35 at $1B+, 0.30 at $250M+, 0.25 at $50M+, 0.20 below
growth_mult = 1 + clamp(change_7d, -70, 150) / 100 × 0.6 when up, × 1.0 when down
maturity    = 0.55 + 0.45 × min(age_days, 900) / 900  (unknown age: 300 days)
```

No protocol name is special-cased. The board covers all 40 watchlist protocols plus the
160 strongest pre-token protocols below the $50M floor, picked by the model itself.
Protocols whose token already trades are set aside with their market caps shown.

## Farm plans

Each row carries a 7-step farm plan derived from the protocol's own record: its
category picks the action, its first chain picks the gas band, its TVL picks the
capital, its age picks the hold. Every step names its cost and its time.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) reads the snapshot, writes
`board.json` and `data.js`, copies `dashboard/` and `screenshots/`, and writes
`dossier.json` with the vendored make.fun library. The dashboard is a static page
with no network: open `out/dashboard/index.html` in a browser.

## Layout

- `main.ts` — the entrypoint
- `lib/model.ts` — the model, the board and the farm plans
- `vendor/makefun-board.ts` — make.fun's dossier library, as shipped in the kit
- `dashboard/` — the page: `index.html`, `app.css`, `app.js`
- `screenshots/` — the board, a protocol detail view and a filtered view

## License

MIT. See `LICENSE`.
