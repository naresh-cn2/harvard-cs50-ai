# Comprehensive Learning Outcomes

### Harvard CS50's Introduction to Artificial Intelligence with Python

This document details the core theoretical principles, computational paradigms, and engineering competencies acquired across the curriculum.

---

## 1. Classical Search & Optimization
- **Uniform-Cost vs Uninformed Search**: Mastered graph exploration topologies, comparing Breadth-First Search (BFS), Depth-First Search (DFS), and Greedy Best-First Search.
- **Informed Heuristic Search ($A^*$)**: Implemented admissible and consistent heuristic estimators to prove optimality while minimizing explored state space.
- **Game-Theoretic Adversarial Search**: Formulated zero-sum two-player games with Minimax trees and applied Alpha-Beta pruning to achieve branch cutoff factors approaching $\mathcal{O}(\sqrt{b^d})$.

## 2. Knowledge Representation & Formal Logic
- **Propositional Calculus**: Designed knowledge bases using propositional logic connectives ($\land, \lor, \neg, \implies, \iff$).
- **Inference Engines**: Implemented automated model checking and resolution theorem proving to evaluate logical entailment ($KB \models \alpha$).
- **Inference with Partial Information**: Developed subset-based deductive logic engines for constraint reasoning in uncertain environments (e.g., Minesweeper).

## 3. Probabilistic Reasoning & Uncertainty
- **Conditional Probability & Bayes' Rule**: Formulated conditional belief updates under evidence $\mathbf{e}$.
- **Markov Chains & Random Surfers**: Designed stochastic transition matrices and demonstrated empirical convergence to unique stationary distributions via Monte Carlo sampling and iterative algebraic fixed-point algorithms.
- **Bayesian Networks**: Factorized joint probability distributions across directed acyclic graph structures (DAGs) and implemented marginalization and normalization over unobserved variables.

## 4. Constraint Satisfaction Problems (CSPs)
- **Constraint Graphs**: Modeled computational tasks as variable networks with unary, binary, and global constraints.
- **Consistency Enforcement**: Implemented Node Consistency and Arc Consistency via the AC-3 algorithm to prune inconsistent values prior to search.
- **Heuristic Backtracking**: Combined Minimum Remaining Values (MRV), Degree heuristics, and Least-Constraining Value (LCV) to achieve exponential search space reductions.

## 5. Machine Learning & Statistical Pattern Recognition
- **Supervised Learning**: Built end-to-end classification pipelines using non-parametric models ($k$-Nearest Neighbors), feature normalization, and categorical encoding.
- **Class Imbalance Evaluation**: Evaluated classification performance beyond raw accuracy using Sensitivity (True Positive Rate) and Specificity (True Negative Rate).

## 6. Reinforcement Learning
- **Markov Decision Processes (MDPs)**: Formulated state-action-reward loops for episodic decision problems.
- **Model-Free Q-Learning**: Implemented temporal difference updates via the Bellman equation to approximate optimal action-value functions without transition probability models.
- **Exploration vs Exploitation**: Implemented $\epsilon$-greedy action selection policies to balance policy exploration with value exploitation during training.

## 7. Deep Learning & Computer Vision
- **Convolutional Neural Networks**: Engineered multi-layer vision architectures in TensorFlow/Keras combining 2D Convolutions, non-linear activation functions (ReLU), Spatial Max-Pooling, and Dropout regularization.
- **Image Pipeline Engineering**: Built automated computer vision pipelines using OpenCV for image resizing, tensor reshaping, and pixel normalization.

## 8. Natural Language Processing & Transformers
- **Syntactic Parsing**: Formulated Context-Free Grammars (CFGs) with `nltk` to parse ambiguous sentences into hierarchical parse trees and extract noun phrase chunks.
- **Transformer Architectures**: Explored masked language modeling using BERT, implementing query-key-value self-attention analysis and heat-map generation across transformer layers and heads.
