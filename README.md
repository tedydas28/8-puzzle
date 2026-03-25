# CS152 Assignment 2 — The 8-Puzzle

A* search implementation for the sliding tile puzzle. Given a scrambled board, finds the optimal sequence of moves to reach the goal state.

---

## Why I found this interesting

The assignment asked for two heuristics. I ended up implementing three, because after getting Manhattan distance working I noticed it was still expanding more nodes than felt necessary.

The issue is that Manhattan distance treats every tile as if it moves independently. But sometimes two tiles are both in the right row, just in the wrong order — meaning one has to leave the row, let the other pass, and come back. That's at least two extra moves per conflicting pair that Manhattan distance never accounts for. So I implemented linear conflict on top of it, which catches exactly that case. It's still admissible, and it consistently dominates h2 across every test case.

---

## Heuristics

- **h1 — Misplaced tiles:** counts tiles not in their goal position. Simple baseline, but loose — it doesn't care how far out of place a tile is.
- **h2 — Manhattan distance:** sums the row and column distance each tile needs to travel. Tighter than h1, expands significantly fewer nodes.
- **h3 — Linear conflict:** Manhattan distance plus a penalty for tiles blocking each other in their goal row or column. Tightest of the three — my addition beyond the base requirements.

---

## Solvability check

Before running A*, the solver checks whether the puzzle is actually solvable. This matters more than it sounds — an unsolvable board would cause A* to silently exhaust the entire state space before giving up, which for larger boards is expensive.

The check uses inversion count parity: if you swap any two tiles in a solved puzzle, the resulting state is permanently unsolvable because every valid move preserves the parity of the board. So you can determine solvability upfront in O(n²) without searching at all. Returns -2 immediately if the board can't be solved.

---

## How to run it

Open `Assignment2.ipynb` in Jupyter and run cells in order. Test cells at the bottom verify everything automatically.

```python
state = [[2,3,7],[1,8,0],[6,5,4]]
steps, expanded, max_frontier, path, err = solvePuzzle(state, h2)
```

Returns steps to solution, nodes expanded, max frontier size, full solution path, and an error code — 0 if solved, -1 if the state is invalid, -2 if unsolvable.

---

## Files

```
Assignment2.ipynb   — implementation and tests
README.md           — this file
```