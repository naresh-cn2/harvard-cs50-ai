# Project 09: Nim

## Core Concept: Reinforcement Learning & Temporal Difference Q-Learning

### 1. Problem Overview
Nim is a mathematical game of strategy where two players take turns removing objects from distinct piles. On each turn, a player must remove at least one object, and all removed objects must come from the same pile. The player forced to take the final object loses (normal/misère play variants). Rather than calculating game-theoretic Nim-sums or using Minimax trees, the AI learns optimal gameplay purely through self-play using Model-Free Reinforcement Learning.

### 2. Markov Decision Process (MDP) Modeling
- **State ($s$)**: A tuple representing object counts across piles, e.g., `(1, 3, 5, 7)`.
- **Action ($a$)**: A tuple `(pile_index, count)` representing removing `count` items from pile `pile_index`.
- **Reward ($r$)**:
  - $r = +1$ for transitioning into a winning terminal state.
  - $r = -1$ for making a move that results in an immediate loss.
  - $r = 0$ for all non-terminal intermediate transitions.

### 3. Q-Learning Algorithm
The expected cumulative discounted future reward of taking action $a$ in state $s$ is encoded in the action-value function $Q(s, a)$.

#### Bellman Temporal Difference Update Rule
Upon taking action $a$ from state $s$, receiving reward $r$, and transitioning to new state $s'$:
$$Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]$$

- $\alpha \in (0, 1]$: Learning rate (governing weight of new information, $\alpha = 0.5$).
- $\gamma \in [0, 1]$: Discount factor (assessing value of future rewards, $\gamma = 1.0$ for episodic games).
- $\text{TD Error} = r + \gamma \max_{a'} Q(s', a') - Q(s, a)$.

```
State (s) --- Action (a) ---> Reward (r) + Next State (s')
    |                                            |
    +---- TD Error = [ r + gamma * max Q(s', a') - Q(s, a) ] <---+
```

### 4. Action Selection: $\epsilon$-Greedy Policy
To balance **exploration** (discovering unknown moves) with **exploitation** (capitalizing on high-value known moves):
$$\pi(a \mid s) = \begin{cases} \text{argmax}_{a \in A(s)} Q(s, a) & \text{with probability } 1 - \epsilon \\ \text{UniformRandom}(A(s)) & \text{with probability } \epsilon \end{cases}$$

During training over 10,000 self-play episodes, $\epsilon$ encourages exploration of diverse game trees. During competitive evaluation, $\epsilon = 0$ forces deterministic greedy execution of the optimal learned policy.

### 5. Convergence & Mastery
Through repeated self-play iterations, the Q-values backpropagate from terminal victories to opening moves. The agent naturally converges to game-theoretic equilibria without any hardcoded rules of Nim arithmetic.
