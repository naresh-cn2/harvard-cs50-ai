# Project 02: Tic-Tac-Toe

## Core Concept: Adversarial Search & Minimax with Alpha-Beta Pruning

### 1. Problem Overview
Adversarial search formalizes decision-making in environments where multiple agents have conflicting objectives. In classic two-player zero-sum deterministic games with perfect information like Tic-Tac-Toe, an optimal strategy can be computed using the Minimax decision rule.

### 2. Game Formulation
- **State Space ($S$)**: A $3 \times 3$ grid where each cell is empty (`EMPTY`), occupied by player $X$, or occupied by player $O$.
- **Players ($P$)**: $\text{player}(s) \in \{X, O\}$. $X$ plays on even turns, $O$ plays on odd turns.
- **Actions ($A(s)$)**: Set of coordinates $(i, j)$ where $s[i][j] = \text{EMPTY}$.
- **Transition Model ($\text{result}(s, a)$)**: Returns a state identical to $s$, with $a$ assigned to $\text{player}(s)$.
- **Terminal State ($\text{terminal}(s)$)**: True if either player has won (3 aligned symbols horizontally, vertically, or diagonally) or no empty cells remain.
- **Utility Function ($\text{utility}(s)$)**:
  $$\text{utility}(s) = \begin{cases} +1 & \text{if } X \text{ wins} \\ -1 & \text{if } O \text{ wins} \\ 0 & \text{if game ends in a draw} \end{cases}$$

### 3. Minimax with Alpha-Beta Pruning
To prevent evaluating unpromising subtrees, Alpha-Beta pruning maintains two bounds:
- $\alpha$: The minimum score that the maximizing player ($X$) is assured of.
- $\beta$: The maximum score that the minimizing player ($O$) is assured of.

$$\text{MAX-VALUE}(s, \alpha, \beta):$$
1. If $\text{terminal}(s)$, return $\text{utility}(s)$.
2. $v \leftarrow -\infty$
3. For each $a \in \text{actions}(s)$:
   - $v \leftarrow \max(v, \text{MIN-VALUE}(\text{result}(s, a), \alpha, \beta))$
   - $\alpha \leftarrow \max(\alpha, v)$
   - If $\alpha \ge \beta$: **break** (prune remaining siblings)
4. Return $v$.

$$\text{MIN-VALUE}(s, \alpha, \beta):$$
1. If $\text{terminal}(s)$, return $\text{utility}(s)$.
2. $v \leftarrow +\infty$
3. For each $a \in \text{actions}(s)$:
   - $v \leftarrow \min(v, \text{MAX-VALUE}(\text{result}(s, a), \alpha, \beta))$
   - $\beta \leftarrow \min(\beta, v)$
   - If $\beta \le \alpha$: **break** (prune remaining siblings)
4. Return $v$.

### 4. Complexity & Performance
- **Full Game Tree States**: $9! = 362,880$ leaf nodes without pruning.
- **Pruned Game Tree**: Alpha-Beta pruning reduces the search space down to $\approx 30,000$ state evaluations, achieving an average branching factor reduction from $b$ to $\sqrt{b}$ in optimal move ordering.
- **Guaranteed Outcome**: Against any adversary, the algorithm never loses, always guaranteeing at least a draw or a victory.
