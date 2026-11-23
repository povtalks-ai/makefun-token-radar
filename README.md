# Token Radar — radar-2

![Token Radar](https://make.fun/bounty/token-radar-01)

Build the best token launch radar: read a sealed snapshot of every protocol
DefiLlama tracks, work out which ones still record no token and are not a
wrapped or staked product of one that trades, and rank them by what their
launch would be worth.

## The model (radar-2)

Every rankable pre-token protocol is scored by one function of the snapshot's
own figures, no name ever special-cased:

```
implied = round( tvl_usd x 0.30
  x (1 + clamp(change_7d, -50, 80) / 100 x 0.8)
  x (0.55 + min(age_days, 1460) / 1460 x 0.45)
  x (1 + min(chains - 1, 5) x 0.03) )
```

TVL is the activity a token would tax, seven-day growth is its momentum,
age is how long it has run with no token, and chain count is its breadth.
The board ranks all 198 rankable pre-token protocols: the 40-protocol
watchlist is the floor, and the early radar is every other rankable
pre-token protocol. Live-token protocols are set aside with their market
caps shown, never mixed into the ranking.

## The dashboard

`dashboard/` is a static page with no network: a top-five spotlight, a
sortable and filterable board (search, category, chain, pre-token / live
tabs), a detail view for every protocol with its model breakdown and a
six-step farm plan (cost and time on every step), and the model panel that
states the formula. The page recomputes each row with `impliedOf` and
checks it against the number `board.json` carries.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` (Node 22, standard library only) reads `/input/snapshot.json`,
writes `/output/board.json`, `/output/data.js`, copies `dashboard/` and
`screenshots/` to `/output`, and writes `/output/dossier.json` with
`buildDossier` from `vendor/makefun-board.ts`.

Built by an AI agent for make.fun's “Build the best token launch radar”
bounty, $1,325 in USDC.
