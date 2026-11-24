# makefun-tiny-chess-entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/yubiuma/makefun-tiny-chess-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/yubiuma/makefun-tiny-chess-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

## A chess engine in 4 KB of Python

`main.py` (4,058 bytes) is a complete, deterministic chess engine for the
make.fun **tiny-chess-01** bounty. The file is a tiny loader: it
base85-decodes and lzma-decompresses the engine source and executes it.
The engine itself is original work, written for this bounty:

- 0x88 board, fully legal move generation (perft-verified)
- Negamax search with PVS, transposition table, killer moves, null-move
  pruning, LMR and a check extension; quiescence over captures,
  promotions and check evasions
- Tapered middlegame/endgame piece-square evaluation with bishop pair,
  updated incrementally
- A soft node governor instead of a fixed budget: moves use the full
  60,000 nodes while the trial's cumulative count is below 18M; past
  that the budget glides down (floor 45,000), and a projection brake
  that estimates the moves left can trim it further (emergency floor
  30,000) — the search aborts at 2x the budget. In ordinary play
  the governor never engages; it only bounds the 8-game trial's
  thinking time when every game runs long. No clocks, no unseeded
  randomness, so every game replays move-for-move

It speaks the competition's environment protocol: it reads
`MAKEFUN_ENV_URL`, joins games with `POST /game` and answers with
`POST /move` while the position is live, always returning a legal move.

Local verification: on a final-style 8-game gauntlet (the kit's
practice six plus the nodes-1M and nodes-2M house engines) this build
scored Elo 1530 (5 wins, 3 losses) with a total wall time of 912
seconds of the 1,800 allowed; the original fixed-budget build needed
840 seconds on the same instance, and its worst case is unbounded,
while this build's worst case is capped by the brake (about 1,135
seconds extrapolated). On the kit's three 6-game practice trials it
scored Elo 1654 / 1809 / 1654, and in a 40-game head-to-head against
the original build it scored 45.0% (14 wins, 8 draws, 18 losses).
Perft and the static evaluation are unchanged from the original
build.
