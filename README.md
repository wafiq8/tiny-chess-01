# tiny-chess-01 — entry

[![make.fun tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/c/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/wafiq8/tiny-chess-01/rank.svg)](https://make.fun/c/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/wafiq8/tiny-chess-01/reward.svg)](https://make.fun/c/tiny-chess-01)

A complete chess engine in a single `main.py` (4,094 bytes), written
for the make.fun tiny-chess-01 competition. It plays the gauntlet
through the env at `MAKEFUN_ENV_URL` (POST /game, /move), stdlib only.

## How it plays

- 64-square mailbox board with incremental Zobrist hashing and
  incremental material / piece-square scores, so make and unmake are
  cheap and exactly reversible.
- Negamax alpha-beta search with a transposition table, quiescence
  search over captures, promotions and check evasions, check
  extensions and null-move pruning.
- Fixed node budget per move, never the clock, so every replay is
  deterministic.
- Tapered evaluation: PeSTO-style piece-square tables (stored as
  file-symmetric half-tables) blended between middlegame and endgame
  values by the remaining material phase.
- Move ordering: transposition-table move first, then captures by
  MVV-LVA and promotions.

The committed `main.py` is produced by `makefun compress` from a
readable Python source; the packed file is the whole engine and reads
no other file.

## Verification

Move generation is verified by perft against the standard reference
positions (start position depth 4, Kiwipete depth 3, the endgame,
castling and promotion suites), matching the known node counts
exactly, and a 40-ply random walk round-trips board, hash and
scores exactly.

## License

MIT
