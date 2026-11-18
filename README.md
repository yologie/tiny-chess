# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/yologie/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/yologie/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB chess engine for make.fun's "Build the strongest chess engine in 4 KB of Python" bounty (tiny-chess-01, $500 in USDC). `main.py` is the whole engine: a 0x88-board negamax search that plays every game through the env.

## How it plays

`main.py` is a fresh 0x88-board engine: negamax alpha-beta with
iterative deepening to depth 8, a transposition table, killer-move and
MVV-LVA ordering, quiescence over captures and promotions, and
late-move reductions that re-search at full depth when a reduced
move looks good. It searches to a fixed node cap of 150,000 nodes
per move, so every trial replays deterministically. The evaluation is
material plus piece-square tables, a bishop-pair bonus, king
centralisation, and a pawn-shield penalty when the king is exposed.
Practice trials: Elo 1,279 on trial-0 and 1,451 on trial-1 (mean 1,365),
with an 8-game wall of about 1,150 seconds against the 1,800-second limit.

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- Every byte counts: comments, whitespace, docstrings.
- Compression is allowed and counted. A `main.py` that unpacks and execs
  a compressed payload (`lzma`, `zlib`) is fine; the payload and the
  unpacker are both part of its 4,096 bytes.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096. MIT, BSD, ISC and 0BSD licences fit.
- Search to a fixed depth or node count, never a clock: a final
  contender's trials are replayed and must play the same moves.

## Grade it locally

You need Docker and the `makefun` CLI. Download the competition's dev
kit, unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.
