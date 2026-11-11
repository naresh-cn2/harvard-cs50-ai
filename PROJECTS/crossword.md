# Project 07: Crossword Puzzle Generation

## Core Concept: Constraint Satisfaction Problems (CSP) & Arc Consistency

### 1. Problem Overview
Crossword puzzle generation is framed as a classic Constraint Satisfaction Problem (CSP). Given a grid structure of blank word slots and a dictionary vocabulary, the system must assign words to slots such that all word lengths match slot sizes and intersecting slots share identical letters at their points of overlap.

```
       [Slot 1: Across (len=4)]
               |
         +---+---+---+---+
         | C | A | T | S |
         +---+---+---+---+
               |
      [Slot 2: Down (len=3, overlaps at index 1)]
```

### 2. CSP Formalization
- **Variables ($X$)**: Set of variable slots $v = (i, j, \text{direction}, \text{length})$.
- **Domains ($D_v$)**: Set of candidate words from the dictionary whose length matches $v.\text{length}$.
- **Unary Constraints**: Length consistency: $\forall w \in D_v, \; \text{len}(w) = v.\text{length}$.
- **Binary Constraints**:
  1. *Uniqueness Constraint*: $v_i \ne v_j$ for all distinct variables $v_i \ne v_j$.
  2. *Overlap Constraint*: If $v_i$ and $v_j$ overlap at indices $(c_i, c_j)$, then:
     $$w_i[c_i] = w_j[c_j] \quad \forall w_i \in D_{v_i}, w_j \in D_{v_j}$$

### 3. Constraint Propagation Algorithms

#### Node Consistency (`enforce_node_consistency`)
Prunes any word from domain $D_v$ whose length does not equal the variable slot length.

#### Arc Consistency via AC-3 (`ac3`)
An arc $(X, Y)$ is consistent if for every value $x \in D_X$, there exists some value $y \in D_Y$ that satisfies the overlap constraint.

```
function AC-3(csp):
    queue = all arcs (X, Y) in csp
    while queue is not empty:
        (X, Y) = dequeue(queue)
        if REVISE(csp, X, Y):
            if size(D_X) == 0:
                return false
            for each neighbor Z of X (where Z != Y):
                enqueue(queue, (Z, X))
    return true
```

#### Arc Revision (`revise(x, y)`)
Removes $x \in D_X$ if no $y \in D_Y$ satisfies the character agreement constraint at the overlap between $X$ and $Y$.

### 4. Backtracking Search with Heuristics
When arc consistency leaves non-singleton domains, Backtracking Search is applied:
1. **Minimum Remaining Values (MRV)**: Choose unassigned variable with the fewest remaining candidate values in its domain.
2. **Degree Heuristic**: Tie-breaker selecting the variable with the largest number of unassigned overlapping neighbors.
3. **Least-Constraining Value (LCV)**: Order values in domain $D_v$ by the number of choices they eliminate from the domains of neighboring variables.
4. **Interleaved Inference**: Execute AC-3 after each tentative assignment to prune subtrees before recursion.
