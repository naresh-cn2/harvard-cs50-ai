# Algorithms & Mathematical Reference

A consolidated reference of the mathematical formulations and computational algorithms implemented throughout Harvard CS50 AI.

---

## 1. Breadth-First Search (BFS)
- **Criterion**: Optimal for uniform-cost unweighted graphs.
- **Frontier**: FIFO Queue $\mathcal{Q}$.
- **Invariant**: Explores all states at path depth $d$ before any state at depth $d + 1$.
- **Time Complexity**: $\mathcal{O}(V + E) = \mathcal{O}(b^d)$.
- **Space Complexity**: $\mathcal{O}(b^d)$ (retains entire frontier in memory).

---

## 2. Minimax with Alpha-Beta Pruning
Evaluates zero-sum adversarial game states with cutoff bounds $[\alpha, \beta]$:

$$\text{Minimax}(s) = \begin{cases} 
\text{Utility}(s) & \text{if Terminal}(s) \\
\max_{a \in A(s)} \text{Minimax}(\text{Result}(s, a)) & \text{if Player}(s) = \text{MAX} \\
\min_{a \in A(s)} \text{Minimax}(\text{Result}(s, a)) & \text{if Player}(s) = \text{MIN} 
\end{cases}$$

- **Pruning Condition**: Terminate sibling exploration when $\alpha \ge \beta$.
- **Best-Case Time Complexity**: $\mathcal{O}(b^{d/2})$ with optimal move ordering.

---

## 3. Propositional Model Checking
Determines if Knowledge Base $KB \models \alpha$ via exhaustive truth-assignment evaluation:

$$\forall m \in \mathcal{M}, \quad \text{Eval}(KB, m) = \text{True} \implies \text{Eval}(\alpha, m) = \text{True}$$

- **Complexity**: $\mathcal{O}(2^n)$ where $n$ is the number of distinct propositional symbols.

---

## 4. Subset-Based Set Calculus (Minesweeper)
Given linear sentences $S_1 = (\mathcal{C}_1, k_1)$ and $S_2 = (\mathcal{C}_2, k_2)$:

$$\mathcal{C}_1 \subset \mathcal{C}_2 \implies (\mathcal{C}_2 \setminus \mathcal{C}_1) = (k_2 - k_1)$$

- **Definite Mines**: $|\mathcal{C}| = k \implies \forall c \in \mathcal{C}, \; c \in \text{Mines}$.
- **Definite Safes**: $k = 0 \implies \forall c \in \mathcal{C}, \; c \in \text{Safes}$.

---

## 5. PageRank (Random Surfer Markov Chain)
Transition distribution over $N$ pages with damping factor $d = 0.85$:

$$PR(p) = \frac{1 - d}{N} + d \sum_{i \in \text{Inbound}(p)} \frac{PR(i)}{|\text{Outbound}(i)|}$$

- **Perron-Frobenius Property**: A unique stationary eigenvector exists satisfying $\mathbf{p} = \mathbf{P} \mathbf{p}$.

---

## 6. Bayesian Network Joint Distribution
Factorization by topological conditioning across directed acyclic graphs:

$$P(X_1 = x_1, \dots, X_n = x_n) = \prod_{i=1}^n P(X_i = x_i \mid \text{Parents}(X_i))$$

- **Posterior Normalization**:
  $$P(X \mid \mathbf{e}) = \alpha \sum_{\mathbf{y}} P(X, \mathbf{e}, \mathbf{y})$$

---

## 7. Arc Consistency (AC-3 Algorithm)
Maintains pairwise consistency across binary constraint networks:

$$\forall x \in D_X, \; \exists y \in D_Y \text{ s.t. } (x, y) \text{ satisfies constraint } C_{XY}$$

- **Time Complexity**: $\mathcal{O}(c \cdot d^3)$ where $c$ is the number of binary constraints and $d$ is maximum domain size.

---

## 8. Q-Learning Temporal Difference Update
Model-free off-policy value approximation:

$$Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]$$

- **Exploration Policy**: $\epsilon$-greedy action selection with parameter decay.

---

## 9. Convolutional Neural Networks (CNNs)
Feature extraction and downsampling operations:

$$S(i, j) = (X * W)(i, j) + b = \sum_{m} \sum_{n} X(i - m, j - n) W(m, n) + b$$

$$\text{ReLU}(z) = \max(0, z), \quad \text{MaxPool}(S) = \max_{(m, n) \in \text{window}} S(2i + m, 2j + n)$$

$$\text{Categorical Cross-Entropy}: \quad \mathcal{L} = -\sum_{k} y_k \log(\hat{y}_k)$$

---

## 10. Scaled Dot-Product Attention (Transformers)
Mapping query, key, and value vectors into weighted representations:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

$$\text{Attention Weight Matrix}: \quad A_{i, j} = \frac{\exp\left(\frac{\mathbf{q}_i \cdot \mathbf{k}_j}{\sqrt{d_k}}\right)}{\sum_l \exp\left(\frac{\mathbf{q}_i \cdot \mathbf{k}_l}{\sqrt{d_k}}\right)}$$
