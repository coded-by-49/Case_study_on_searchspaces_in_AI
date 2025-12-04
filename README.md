Search Space Case Study: Tic-Tac-Toe vs. Connect 4

Project Overview

This project serves as a comparative case study in Game Theory and Search Algorithms. By implementing AI agents for both Tic-Tac-Toe and Connect 4, this project demonstrates the radical shift in computational prerequisites when the state space of a problem increases.

The core finding of this experiment is the transition from a solvable "perfect" game (Tic-Tac-Toe) to a complex computational challenge (Connect 4) that renders pure Depth First Search (DFS) obsolete.

🧪 The Case Study: The Search Space Problem

During development, a key observation was made regarding the limitations of recursion and memory management.

1. The Control Group: Tic-Tac-Toe

In the Tic-Tac-Toe implementation (players.py), a standard Minimax algorithm (a variant of DFS) was used. The agent explores every possible future game state until the end of the game to determine the perfect move.

Grid: 3 x 3

Approximate State Space: $10^3$ to $10^5$ complexity.

Result: The algorithm runs flawlessly. It calculates the "perfect" game instantly because modern RAM can easily hold the entire game tree in memory.

Visual Representation:

 0 | 1 | 2 
-----------
 3 | X | 5 
-----------
 6 | 7 | O 


Small, manageable branching factor.

2. The Variable: Connect 4

When applying the exact same logic to Connect 4 (players_connect_4.py), the system crashed ("fried the RAM").

Grid: 6 x 7

Approximate State Space: $\approx 4.5 \times 10^{12}$ positions.

Result: Without optimization, the recursion depth is too great. The branching factor (7 possible columns) multiplied by the depth (up to 42 moves) creates a tree so large it causes a Stack Overflow or memory exhaustion before the first move is even chosen.

Visual Representation:

| 00 | 01 | 02 | 03 | 04 | 05 | 06 |
|    |    |    |    |    |    |    |
|    |    |    |    |    |    |    |
|    |    |    | X  |    |    |    |
|    |    | O  | X  |    |    |    |
|    | O  | X  | O  |    |    |    |


Massive branching factor. Even a few moves deep creates millions of possibilities.

The Solution: Heuristic Depth Limiting

To solve the Connect 4 memory crisis, the algorithm was modified in players_connect_4.py:

Depth Limit: The search is capped (e.g., looking only 5 moves ahead) rather than searching until the game ends.

Heuristic Evaluation: Since the AI cannot see the "end" of the game, it uses a scoring system (checking for 2-in-a-row, 3-in-a-row) to guess how good the board looks at the depth limit.

Alpha-Beta Pruning: Optimizes the search by ignoring branches that are mathematically worse than moves already found.

📂 File Structure

Tic-Tac-Toe (Perfect Solver)

game.py: The game engine handling the 3x3 board logic.

players.py: Contains the GeniusComputerPlayer which uses unbridled Minimax to play perfectly.

Connect 4 (Heuristic Solver)

game_connect_4.py: The game engine handling the 6x7 board logic and win checking.

players_connect_4.py: Contains Unbeatable_ai. Note the depth parameter in the minimax function—this is the safeguard against memory overflow.

testing_4.py: A sandbox for testing the heuristic scoring system (awarding points for 3-in-a-row, etc.).

🚀 How to Run

Run Tic-Tac-Toe

Witness the perfect (instant) solver:

python game.py


Run Connect 4

Witness the depth-limited solver (calculating against the search space explosion):

python game_connect_4.py


🧠 Key Takeaways

Search Space Sensitivity: Algorithms that work on small toy problems (Tic-Tac-Toe) often fail catastrophically on larger problems (Connect 4) due to exponential growth.

Optimization Necessity: As game complexity rises, we must trade perfection (searching everywhere) for efficiency (heuristics and depth limits).