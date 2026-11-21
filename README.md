# Sokoban solver — make.fun entry for sokoban-solver-01-run-20261121

A push-optimal Sokoban solver in one file, `main.ts` (Node 22, erasable
TypeScript). It answers all 5 instances of a set in one run, writing
`/output/<id>/solution.txt` for each.

## How it works

A level is a hallway with rooms off it, and a box pushed through a door into
the hallway can never come back, so every box stays in its room and the level's
push-optimum is the sum of its rooms' optima. The solver cuts the hallway's
dead squares out of the floor; each part left that holds boxes is a room. It
solves each room with a breadth-first search over pushes (the first solved
state met is push-optimal), pruning dead squares and frozen 2x2 blocks, then
stitches the room solutions together with hallway walks. The search is capped
by state count, never by the clock, so two runs write byte-identical answers.

## Local run

From the kit directory:

```
mkdir -p out
MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Score with the kit's scorer:

```
MAKEFUN_OUTPUT_DIR=out MAKEFUN_REFERENCE_DIR=practice/reference \
  MAKEFUN_SCORES_DIR=scores node program/scorer/score.ts
```

## Practice result

Score **1,000,000** (the maximum): every instance solved in exactly P* pushes
(i001 13, i002 16, i003 29, i004 47, i005 60), in ~1.2 s for the whole set.
The house baseline (plain-astar 1.0.0) scores 200,000 on the same set — it
solves only i001. Three freshly drawn sets (random seeds, kit generator) also
score 1,000,000 each, in under 1 s apiece.

## License

MIT — see LICENSE.
