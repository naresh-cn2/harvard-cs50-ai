# Project 01: Degrees of Separation

## Core Concept: Breadth-First Search (BFS) in Large-Scale Graphs

### 1. Problem Overview
The "Six Degrees of Kevin Bacon" is an application of graph theory based on the small-world phenomenon. Given an undirected bipartite graph containing actors and movies, the objective is to determine the shortest path between any two actors, where edges represent shared appearances in films.

```
[Actor A] --- (Movie X) ---> [Actor B] --- (Movie Y) ---> [Actor C]
```

### 2. Algorithmic Formulation
Because the graph edges have uniform weights (each film counts as one transition of cost 1), finding the shortest path corresponds to finding the minimum number of edges between the source and target nodes.

- **Algorithm**: Breadth-First Search (BFS)
- **Frontier Data Structure**: First-In, First-Out Queue (`QueueFrontier`)
- **Explored Set**: Hash set storing visited entity identifiers to eliminate cycles and redundant states.
- **Node Representation**:
  - `state`: Current actor ID
  - `parent`: Reference to the predecessor Node
  - `action`: Movie ID linking the parent actor to the current actor

### 3. State-Space Search Execution
1. Initialize the frontier with the `source` actor as the root node.
2. Initialize an empty `explored` set.
3. While the frontier is not empty:
   - Pop node $n$ from the frontier using FIFO ordering.
   - If $n.\text{state} = \text{target}$, reconstruct and return the path by backtracking through `parent` pointers.
   - Add $n.\text{state}$ to the `explored` set.
   - For each `(movie_id, person_id)` pair accessible from $n.\text{state}$:
     - If `person_id` is neither in `explored` nor already in the frontier:
       - Construct a child node and add it to the frontier.
4. If the frontier is exhausted without reaching the target, no connection exists (return `None`).

### 4. Complexity Analysis
- **Branching Factor ($b$)**: Average number of co-stars per actor across filmography ($\approx 50\text{--}200$).
- **Depth ($d$)**: Shortest path distance (typically $\le 6$).
- **Time Complexity**: $\mathcal{O}(b^d)$ — Guarantees finding the optimal path since all paths of length $k$ are explored before any path of length $k+1$.
- **Space Complexity**: $\mathcal{O}(b^d)$ — Keeps all frontier nodes in memory.

### 5. Key Engineering Insights
- **Early Goal Checking**: Checking whether a neighbor equals `target` at the generation step rather than at expansion step drastically cuts memory consumption and accelerates BFS execution by an entire factor of $b$.
- **Memory Scaling**: On large IMDb dumps (millions of rows), storing node states as lightweight primitive integer IDs rather than strings is crucial to prevent high GC overhead.
