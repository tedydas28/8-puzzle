# CS152 Assignment 2 — The 8-Puzzle

A* search implementation for the classic sliding tile puzzle. Given a scrambled board, the algorithm finds the optimal (shortest) sequence of moves to reach the goal state.

---

## What it does

The solver takes any n×n sliding puzzle and finds the minimum number of moves to solve it. It uses A* search — which combines the actual cost to reach a state with a heuristic estimate of how far it still is from the goal — so it's guaranteed to find the optimal path without exploring the entire state space.

Three heuristics are implemented, each tighter than the last:

- **h1 — Misplaced tiles:** counts how many tiles are out of place. Simple, but loose.
- **h2 — Manhattan distance:** sums how far each tile needs to travel. Tighter than h1, expands fewer nodes.
- **h3 — Linear conflict:** Manhattan distance plus a penalty for tiles that are in the right row or column but blocking each other. Tightest of the three — dominates h2 in every test case.

---

## How to run it

Open `Assignment2.ipynb` in Jupyter and run the cells in order. The test cells at the bottom verify correctness automatically — if they pass with no errors, everything is working.

```python
# Example
state = [[2,3,7],[1,8,0],[6,5,4]]
steps, expanded, max_frontier, path, err = solvePuzzle(state, h2)
```

Returns the number of steps, nodes expanded, max frontier size, the full solution path, and an error code (0 = solved, -1 = invalid state, -2 = unsolvable).

---

## Extensions implemented

- **Solvability check** — detects unsolvable boards upfront using inversion count parity, instead of letting A* exhaust the full state space before giving up.
- **Linear conflict heuristic (h3)** — dominates Manhattan distance by accounting for tiles that are in the right row/column but physically blocking each other.

---

## Files

```
Assignment2.ipynb   — full implementation and tests
README.md           — this file
```