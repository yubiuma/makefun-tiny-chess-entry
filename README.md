# makefun-tiny-chess-entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/yubiuma/makefun-tiny-chess-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/yubiuma/makefun-tiny-chess-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

## A chess engine in 4 KB of Python

`main.py` (4,089 bytes) is a complete, deterministic chess engine for the
make.fun **tiny-chess-01** bounty. The file is a tiny loader: it
base85-decodes and lzma-decompresses the engine source and executes it.
The engine itself is original work, written for this bounty:

- 0x88 board, fully legal move generation (perft-verified)
- Negamax search with PVS, transposition table, killer moves, null-move
  pruning, LMR and a check extension; quiescence over captures,
  promotions and check evasions
- Tapered middlegame/endgame piece-square evaluation with bishop pair,
  updated incrementally
- A deterministic node bank instead of a fixed budget: a move may use
  up to 60,000 nodes early in a trial, gliding down with the trial's
  cumulative node count to a 20,000 floor, and the search aborts at
  1.25x the budget — this bounds the whole 8-game trial's thinking
  time so it cannot run out of its wall clock on long games. No
  clocks, no unseeded randomness, so every game replays move-for-move

It speaks the competition's environment protocol: it reads
`MAKEFUN_ENV_URL`, joins games with `POST /game` and answers with
`POST /move` while the position is live, always returning a legal move.

Local verification: on a final-style 8-game gauntlet (the kit's
practice six plus the nodes-1M and nodes-2M house engines) this build
scored Elo 1669 (5 wins, 2 draws, 1 loss) with a total wall time of
573 seconds of the 1,800 allowed; the previous fixed-budget build
needed 840 seconds on the same instance. On the kit's three 6-game
practice trials it scored Elo 1809 / 1451 / 1451. Perft and the
static evaluation are unchanged from the previous build.
