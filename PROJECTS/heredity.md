# Project 06: Heredity

## Core Concept: Bayesian Networks & Joint Probability Distributions

### 1. Problem Overview
Genetic inheritance of hereditary traits (e.g., hearing impairment associated with mutations in the GJB2 gene) involves uncertainty governed by Mendelian genetics and biological mutation rates. Given partial phenotypic observations (expressed traits) across family pedigree trees, the objective is to compute the posterior marginal probability distribution of gene copies and trait presence for all individuals.

### 2. Bayesian Network Graph Structure
For each person $i$, the joint state is modeled by two random variables:
- $G_i \in \{0, 1, 2\}$: Number of mutated gene copies.
- $T_i \in \{\text{True}, \text{False}\}$: Expression of the physical trait.

The directed acyclic graph (DAG) structure defines conditional dependencies:
- If individual $i$ has no listed parents in the network, $G_i$ depends only on the unconditional population prior:
  $$P(G_i = g)$$
- If individual $i$ has mother $M$ and father $F$, $G_i$ is conditionally independent of ancestors given $\{G_M, G_F\}$:
  $$P(G_i = g \mid G_M, G_F)$$
- Trait expression $T_i$ depends conditionally only on gene copies $G_i$:
  $$P(T_i = t \mid G_i = g)$$

```
[Mother G_M]         [Father G_F]
       \                 /
        v               v
         [Child Gene G_C]
                |
                v
        [Child Trait T_C]
```

### 3. Transition Probabilities with Mutation
A parent with $g \in \{0, 1, 2\}$ copies of the gene passes a mutated allele to their offspring according to:
- $g = 2$: Passes mutated copy with probability $1 - \mu$ (where mutation rate $\mu = 0.01$).
- $g = 1$: Passes mutated copy with probability $0.5$.
- $g = 0$: Passes mutated copy with probability $\mu$ (via spontaneous mutation).

The child inherits one allele from the mother and one from the father independently. The distribution of $G_C \in \{0, 1, 2\}$ is obtained by combining the independent Bernoulli transmission events from both parents.

### 4. Joint Probability Factorization
By the chain rule for Bayesian networks, the joint probability over all variables in the network decomposes as:
$$P(G_1, T_1, \dots, G_n, T_n) = \prod_{i=1}^n P(G_i \mid \text{Parents}(G_i)) \cdot P(T_i \mid G_i)$$

### 5. Inference by Marginalization and Normalization
1. **Enumeration over Unobserved Variables**: Sum joint probabilities over all compatible state configurations consistent with known evidence.
2. **Probability Accumulation**: For each query variable assignment, increment the weighted sum of probability masses.
3. **Normalization**: Divide the accumulated probability mass for each variable by the total partition function:
   $$\alpha = \sum_{v \in \text{Domain}(X)} P(X = v, \mathbf{e})$$
   $$P(X = v \mid \mathbf{e}) = \frac{P(X = v, \mathbf{e})}{\alpha}$$
