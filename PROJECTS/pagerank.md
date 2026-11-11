# Project 05: PageRank

## Core Concept: Markov Chains & Stationary Distributions

### 1. Problem Overview
The PageRank algorithm models the behavior of an idealized web user ("random surfer") navigating a directed graph of hyperlinked web pages. The goal is to estimate the stationary probability distribution representing the likelihood of arriving at any given page over an infinite horizon.

```
[Page A] ----> [Page B] <----> [Page C]
   |                             ^
   +-----------------------------+
```

### 2. Transition Model Formulation
The random surfer model balances hyperlink traversal with random damping:
- With probability $d$ (damping factor, typically $0.85$), the surfer clicks one of the outgoing links from the current page uniformly at random.
- With probability $1 - d$, the surfer teleports to any page in the corpus uniformly at random.

For a current page $p$ with non-empty outgoing links $\text{Links}(p)$ across a corpus of $N$ pages:
$$P(s \mid p) = \frac{1 - d}{N} + \begin{cases} \frac{d}{|\text{Links}(p)|} & \text{if } s \in \text{Links}(p) \\ 0 & \text{otherwise} \end{cases}$$

If page $p$ has no outgoing links ($|\text{Links}(p)| = 0$), it is treated as having links to every page in the entire corpus:
$$P(s \mid p) = \frac{1}{N}$$

### 3. Estimation Methods

#### Method A: Monte Carlo Sampling (`sample_pagerank`)
1. Start at a page chosen uniformly at random.
2. For $i = 1$ to $n$:
   - Generate the next page according to the probability distribution defined by the transition model of the current page.
   - Maintain a frequency counter for each visited page.
3. Normalize counts by total samples $n$ to produce PageRank estimates.

#### Method B: Iterative Convergence (`iterate_pagerank`)
The true PageRank satisfies the recursive linear system:
$$PR(p) = \frac{1 - d}{N} + d \sum_{i \in \text{Inbound}(p)} \frac{PR(i)}{|\text{Outbound}(i)|}$$

1. Initialize $PR(p) = \frac{1}{N}$ for all $p$.
2. Repeatedly recalculate PageRank for each page in parallel.
3. Terminate when the maximum absolute change between iterations across all pages is less than the convergence threshold $\epsilon = 0.001$:
   $$\max_{p} |PR^{(t+1)}(p) - PR^{(t)}(p)| < \epsilon$$
4. Normalize the final distribution so that $\sum_{p} PR(p) = 1$.

### 4. Convergence & Invariance
The Markov chain defined by the random surfer model is ergodic (irreducible and aperiodic due to the damping factor $d < 1$). By the Perron-Frobenius theorem, a unique stationary distribution is guaranteed to exist, and both sampling and iteration converge to this unique fixed-point vector.
