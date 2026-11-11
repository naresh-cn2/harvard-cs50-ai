# Project 08: Shopping

## Core Concept: Supervised Machine Learning & Classification Metrics

### 1. Problem Overview
E-commerce platforms seek to predict online shopper purchasing intentions based on session telemetry (page visit durations, bounce rates, exit rates, visitor type, operating systems, and calendar month). The goal is to build, train, and evaluate a nearest-neighbors classifier on the Online Shoppers Purchasing Intention dataset, tuning evaluation metrics specifically for class-imbalanced data.

### 2. Feature Pipeline & Data Transformations
The raw dataset contains numerical continuous, ordinal, and categorical features:
- **Continuous numerical**: `Administrative_Duration`, `Informational_Duration`, `ProductRelated_Duration`, `BounceRates`, `ExitRates`, `PageValues`, `SpecialDay`
- **Integer counts**: `Administrative`, `Informational`, `ProductRelated`, `OperatingSystems`, `Browser`, `Region`, `TrafficType`
- **Categorical mapped to numerical**:
  - `Month`: Jan-Dec converted to zero-indexed integers (Jan=0, Feb=1, ..., Dec=11)
  - `VisitorType`: `Returning_Visitor` $\rightarrow 1$, all others $\rightarrow 0$
  - `Weekend`: `TRUE` $\rightarrow 1$, `FALSE` $\rightarrow 0$
- **Target Label**: `Revenue` (`TRUE` $\rightarrow 1$, `FALSE` $\rightarrow 0$)

### 3. Classification Model: $k$-Nearest Neighbors ($k$-NN)
The $k$-NN non-parametric classifier predicts labels based on Euclidean distance in $d$-dimensional normalized feature space:
$$d(\mathbf{x}, \mathbf{x}') = \sqrt{\sum_{j=1}^m (x_j - x_j')^2}$$
For $k = 1$, the decision boundary assigns the class of the single closest training instance.

```
       Feature 2
           ^
           |       o (Class 0: No Purchase)
           |    o     o
           |       [Query x] ----> Nearest Neighbor: * (Class 1)
           |    *        *
           |  *     *       * (Class 1: Purchase)
           +------------------------> Feature 1
```

### 4. Evaluation in Class-Imbalanced Regimes
In e-commerce datasets, non-purchasing sessions vastly outnumber purchasing sessions (often $\approx 85\%$ vs $15\%$). Raw accuracy is misleading because a trivial classifier predicting all negatives achieves $85\%$ accuracy while detecting zero sales.

#### Sensitivity (True Positive Rate / Recall for Positive Class)
Proportion of actual buyers correctly identified:
$$\text{Sensitivity} = \frac{TP}{TP + FN}$$

#### Specificity (True Negative Rate / Recall for Negative Class)
Proportion of non-buyers correctly identified:
$$\text{Specificity} = \frac{TN}{TN + FP}$$

### 5. Empirical Results
Evaluating a $k=1$ nearest neighbors classifier reveals the trade-off:
- Achieving high specificity ($\approx 90\%$) by correctly classifying most non-purchases.
- Maintaining actionable sensitivity ($\approx 40\text{--}50\%$) on identifying high-intent purchasing visits despite severe class imbalance.
