# makefun-tiny-chess-entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/yubiuma/makefun-tiny-chess-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/yubiuma/makefun-tiny-chess-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

## A chess engine in 4 KB of Python

`main.py` (4,031 bytes) is a complete, deterministic chess engine for the
make.fun **tiny-chess-01** bounty. The file is a tiny loader: it
base85-decodes and lzma-decompresses the engine source and executes it.
The engine itself is original work, written for this bounty:

- 0x88 board, fully legal move generation (perft-verified)
- Negamax search with PVS, transposition table, killer moves, null-move
  pruning, LMR and a check extension; quiescence over captures,
  promotions and check evasions
- Tapered middlegame/endgame piece-square evaluation with bishop pair,
  updated incrementally
- Fixed node budget per move (60,000 nodes) — no clocks, no unseeded
  randomness, so every game replays move-for-move

It speaks the competition's environment protocol: it reads
`MAKEFUN_ENV_URL`, joins games with `POST /game` and answers with
`POST /move` while the position is live, always returning a legal move.

Local verification against the kit's practice gauntlet scored Elo 1809
(5 wins, 1 draw) on the practice trial, and in head-to-head play at the
same node budget it scores about 84% against the sunfish 4K baseline
run at its official 50,000 nodes per move.
