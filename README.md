# tiny-chess-01 — entry (slim build)

[![make.fun tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/c/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/wafiq8/tiny-chess-01/rank.svg)](https://make.fun/c/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/wafiq8/tiny-chess-01/reward.svg)](https://make.fun/c/tiny-chess-01)

A chess engine in a single `main.py` (4,064 bytes): a 0x88-board
negamax search (alpha-beta + transposition table, quiescence over
captures/promotions/check evasions, null-move pruning, LMR) with a
tapered PeSTO-style evaluation (piece-square tables, passed pawns,
king shelter), running on fixed node budgets — never a clock — so
every replay is byte-identical. It plays the gauntlet through the
env at `MAKEFUN_ENV_URL` (POST /game, /move).

The engine source is minified and stored as an lzma payload inside
`main.py`; the file is the whole engine (stdlib only, reads no other
file).

Slim build vs the full-strength development engine (Elo 1364, 5,532 B
packaged — over the cap): PeSTO tables are stored as single-char
quantized values (step 12, max error 6 cp/square); endgame tables for
N/B/R/Q alias the middlegame tables (endgame pawn and king tables are
kept in full); black pieces use a true vertical mirror of the tables;
small eval terms (bishop pair, isolated/doubled pawn penalties) and
several search frills (futility, killers, history, QS delta pruning,
in-tree repetition set) are dropped to fit the 4,096-byte cap.
Movegen is unchanged — perft matches the reference engine exactly.

Build: `slim/` tooling (outside this repo snapshot) — subsS.py +
minifyS.py shrink the development engine, packS.py embeds engine +
env driver. Verification: slim/equivS.py (perft / key / root tests,
ALL PASS) plus two practice gauntlet trials.
