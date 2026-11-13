# Tiny Chess Engine (4 KB)

A strong chess engine in a single Python file under 4,096 bytes, built for
the make.fun bounty "Build the strongest chess engine in 4 KB of Python".

## Engine

- 0x88 board, negamax + alpha-beta with iterative deepening to a fixed
  node budget (60,000 nodes/move; deterministic: no clock, no unseeded
  randomness).
- Transposition table (Zobrist hashing), MVV-LVA capture ordering, check
  extensions, quiescence search (captures, plus all evasions when in
  check).
- Evaluation: material + piece-square tables (binary middlegame/endgame
  king switch), insufficient-material detection.
- Incremental evaluation maintained in make/unmake.

## Layout

- `main.py` — the engine (packed with lzma + base85, <= 4,096 bytes).
  Python 3 stdlib only; no other files read or imported.
- `LICENSE` — MIT.
- `TRANSCRIPT.md` — build log and test results.

## Run

The engine speaks the make.fun env protocol: it reads `MAKEFUN_ENV_URL`
and plays the games the env serves (`POST /game`, `POST /move`).
