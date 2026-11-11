# Project 04: Minesweeper

## Core Concept: Knowledge Representation & Logical Inference

### 1. Problem Overview
Minesweeper is a game of incomplete information played on an $H \times W$ grid containing hidden mines. When an agent clicks a safe cell, the game reveals the exact count of neighboring mines (cells adjacent horizontally, vertically, or diagonally). The AI must represent observations as logical constraints and infer safe moves or identify mines with certainty.

### 2. Sentence-Level Logical Calculus
A knowledge state is represented as a collection of linear propositional sentences:
$$\{\text{cell}_1, \text{cell}_2, \dots, \text{cell}_k\} = \text{count}$$
meaning exactly $\text{count}$ of the specified cells are mines.

#### Fundamental Deductions
1. **Definite Mines**:
   If $|\text{cells}| = \text{count}$, then every cell in $\text{cells}$ is guaranteed to be a mine:
   $$\forall c \in \text{cells}, \quad c \in \text{Mines}$$
2. **Definite Safes**:
   If $\text{count} = 0$, then every cell in $\text{cells}$ is guaranteed to be safe:
   $$\forall c \in \text{cells}, \quad c \in \text{Safes}$$

### 3. Subset-Based Inference Principle
Given two sentences $S_1 = (\text{cells}_1 = \text{count}_1)$ and $S_2 = (\text{cells}_2 = \text{count}_2)$ where $\text{cells}_1 \subset \text{cells}_2$:
$$(\text{cells}_2 \setminus \text{cells}_1) = (\text{count}_2 - \text{count}_1)$$

```
Set 2: [ Cell A, Cell B, Cell C, Cell D ] = 2 mines
Set 1: [ Cell A, Cell B ]                 = 1 mine
---------------------------------------------------
Subset: [ Cell C, Cell D ]                = 1 mine
```

This subset difference generates newly derived sentences, frequently cascading into new definite safes and definite mines.

### 4. MinesweeperAI Agent Lifecycle
1. **`add_knowledge(cell, count)`**:
   - Mark `cell` as a visited move.
   - Mark `cell` as safe.
   - Construct a new sentence with unrevealed neighbors and adjusted mine count.
   - Repeatedly infer safe cells and mine cells until no further single-sentence deductions can be made.
   - Compare all pairs of sentences in the knowledge base; apply the subset inference rule to synthesize new sentences.
   - Purge empty or redundant sentences.
2. **`make_safe_move()`**:
   - Return any known safe cell that has not yet been selected.
3. **`make_random_move()`**:
   - If no provably safe moves exist, choose a random unclicked cell that is not marked as a mine.
