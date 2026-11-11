# Project 03: Knights and Knaves

## Core Concept: Propositional Logic & Knowledge Engineering

### 1. Problem Overview
Raymond Smullyan's classic "Knights and Knaves" logic puzzles feature characters who are either Knights (who exclusively tell the truth) or Knaves (who exclusively lie). The objective is to represent natural language statements as formal propositional logic sentences and determine character identities via automated model checking.

### 2. Knowledge Representation Framework
For characters $A, B, C$, propositional symbols are established:
- $AKnight, AKnave$: $A$ is a Knight / Knave.
- $BKnight, BKnave$: $B$ is a Knight / Knave.
- $CKnight, CKnave$: $C$ is a Knight / Knave.

#### Foundational Axioms (Domain Knowledge)
Each character must be exactly one type:
$$(AKnight \lor AKnave) \land \neg(AKnight \land AKnave)$$
$$(BKnight \lor BKnave) \land \neg(BKnight \land BKnave)$$
$$(CKnight \lor CKnave) \land \neg(CKnight \land CKnave)$$

#### Truth Condition Equivalence
If a character makes an assertion $S$, the statement is true if and only if that character is a Knight:
$$AKnight \iff S$$
Alternatively expressed with implications:
$$(AKnight \implies S) \land (AKnave \implies \neg S)$$

### 3. Puzzle Problem Specifications
- **Puzzle 0**: Character $A$ says: "I am both a knight and a knave."
  $$\text{Sentence } S_0 = AKnight \land AKnave$$
  $$\text{Knowledge}_0 = \text{DomainAxiom}(A) \land (AKnight \iff S_0)$$
  *Inference*: $A$ is a Knave.

- **Puzzle 1**: $A$ says: "We are both knaves." $B$ says nothing.
  $$\text{Sentence } S_1 = AKnave \land BKnave$$
  $$\text{Knowledge}_1 = \text{DomainAxioms}(A, B) \land (AKnight \iff S_1)$$
  *Inference*: $A$ is a Knave, $B$ is a Knight.

- **Puzzle 2**: $A$ says: "We are the same kind." $B$ says: "We are of different kinds."
  $$\text{Sentence } S_{2A} = (AKnight \land BKnight) \lor (AKnave \land BKnave)$$
  $$\text{Sentence } S_{2B} = (AKnight \land BKnave) \lor (AKnave \land BKnight)$$
  *Inference*: $A$ is a Knave, $B$ is a Knight.

- **Puzzle 3**: Multi-agent interaction with compound conditional dialogue between $A, B, C$.
  - $A$ says either "I am a knight" or "I am a knave", but you don't know which.
  - $B$ says "$A$ said 'I am a knave'".
  - $B$ says "$C$ is a knave".
  - $C$ says "$A$ is a knight".
  *Inference*: $A$ is a Knight, $B$ is a Knave, $C$ is a Knight.

### 4. Inference Engine: Model Checking
- **Entailment**: Knowledge Base $KB \models \alpha$ if and only if in every model where $KB$ is True, query $\alpha$ is also True.
- **Algorithm**: `check_all(kb, query, symbols, model)` explores the truth assignment lattice ($2^n$ combinations for $n$ propositional symbols).
- **Correctness**: Sound and complete for propositional logic.
