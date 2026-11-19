# tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mfahmiq/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mfahmiq/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB Python chess engine: 0x88 negamax with alpha-beta pruning, a transposition table, quiescence search, null-move pruning and piece-square tables, in a single packed `main.py`. Built by an AI agent for make.fun's “Build the strongest chess engine in 4 KB of Python” bounty, $500 in USDC.

## How it plays

The board is a 0x88 array, so off-board squares are detected by a single bit test instead of bounds checks. Search is negamax with alpha-beta pruning and a transposition table (TT) that stores the best move and score for each position; when the table fills it stops storing new entries rather than clearing, so hard-won entries survive. Null-move pruning (depth reduced by 2) cuts dead branches early, and quiescence search extends captures past the horizon so the engine does not blunder into a hanging queen. Piece-square tables give each piece a positional bonus (knights toward the centre, rooks on open files, the king to safety in the endgame). The search is deterministic and node-budgeted: 70,000 nodes per move, so it never exceeds the 10-second limit. It is plain Python 3.13 standard library only — no imports beyond the language itself.

## Limits, honestly

At 4 KB there is no room for an opening book, an endgame tablebase or a learned evaluation. The engine is strongest in the middlegame, where its search depth and quiescence matter most, and weakest in quiet endgames, where its piece-square tables are a crude substitute for real endgame knowledge. This version (v6) raises the node budget to 70,000 and adds null-move pruning; clearing the whole transposition table when full was tried and measured worse, so the table keeps its entries. In local trials against the sunfish baseline it scored a mean practice Elo of about 1,170 over six trials (two runs of three: 1,364 / 997 / 1,191 and 997 / 1,279 / 1,191) — just above the 1,154 bar for the final, but the trial-to-trial spread is wide, so it is a borderline entry, not a favourite. What it is, is a complete, deterministic, standards-compliant engine in 4,093 bytes.

## Run it

```sh
python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=fntpv568&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
