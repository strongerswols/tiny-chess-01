# Transcript — Tiny Chess Engine (make.fun tiny-chess-01)

## Build
- 0x88 board; negamax + alpha-beta, iterative deepening to a fixed
  60,000 nodes/move (deterministic: no clock, no unseeded randomness).
- Transposition table (Zobrist), MVV-LVA ordering, check extensions,
  quiescence (captures; all evasions when in check), null-move pruning.
- Eval: material + piece-square tables, binary middlegame/endgame king
  switch; insufficient-material detection; incremental eval in make/unmake.
- Packed lzma + base85 into one file (4,082 bytes). Python 3 stdlib
  only (json, os, random, urllib.request for the env protocol). No
  model calls, no other files read or imported.

## Verification (2026-11-13)
- Official kit re-downloaded and verified: 31/31 files match kit.json
  sha256. `grade-local --driver process` cannot run a Python bot
  (needs Docker, unavailable here), so trials ran on the kit's own env
  server + sealed scorer, same protocol as grading.
- Practice trial-0: 8 games vs gauntlet depth-2..nodes-2M, 10 s/move,
  200 plies. At 30,000 nodes: 1 win, 1 draw, 6 losses — Elo 989
  (win: depth-4 white; draw: depth-2 white, adjudicated 200 plies).
- Node sweep (loaded VM): 30k nodes 0.4–3.8 s/move; 100k nodes up to
  11.0 s/move (over the limit), so 100k+ was rejected; 60k chosen.
- Same trial-0 at 60,000 nodes: 4 wins, 0 draws, 4 losses — Elo 1396.
  Wins: depth-2, depth-3 (adjudicated), depth-4, nodes-100k. Losses:
  nodes-250k, nodes-500k, nodes-1M (abandoned), nodes-2M (unplayed).
  The last two are wall-time artifacts: the 1700 s harness timeout
  hit on this loaded VM while the house was thinking — no game in
  either trial ended on time.
- Elo 1396 clears the 1,154 final-entry floor and the house baseline's
  practice mean (1,325). Snapshot total under 8,192 bytes, no file
  over 4,096 (main.py 4,082 + LICENSE 1,070 + README.md 1,029).
