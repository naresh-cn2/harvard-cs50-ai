# Project 11: Parser

## Core Concept: Natural Language Processing & Context-Free Grammars (CFGs)

### 1. Problem Overview
Understanding natural language syntax requires formal models capable of determining grammatical structure and extracting semantic components such as noun phrases. The goal is to define a formal Context-Free Grammar (CFG) using Python's Natural Language Toolkit (`nltk`), preprocess raw input sentences, parse them into hierarchical syntactic parse trees, and extract noun phrase chunks (`np_chunk`).

### 2. Preprocessing Pipeline
Raw textual sentences must be cleaned and tokenized prior to syntactic parsing:
1. **Case Normalization**: Convert raw text to lowercase.
2. **Word Tokenization**: Tokenize strings into discrete tokens using `nltk.word_tokenize`.
3. **Punctuation & Noise Filtering**: Retain only tokens that contain at least one alphabetic character (`char.isalpha()`). Pure punctuation marks (e.g., periods, commas, quotation marks) are discarded so they do not interfere with grammar production matching.

### 3. Context-Free Grammar Architecture
A Context-Free Grammar is defined as a 4-tuple $G = (V, \Sigma, R, S)$:
- $V$: Non-terminal symbols (e.g., $S, NP, VP, PP, AP$).
- $\Sigma$: Terminal alphabet (lexicon words).
- $R$: Production rewrite rules.
- $S$: Start symbol ($S$).

#### Non-Terminal Production Hierarchy
- **Sentence Structure ($S$)**:
  $$S \rightarrow NP \; VP \mid S \; Conj \; S \mid S \; Conj \; VP$$
- **Noun Phrase ($NP$)**:
  Noun phrases can consist of simple nouns, determiners, adjectives, compounding nouns, and modifying prepositional phrases:
  $$NP \rightarrow N \mid Det \; N \mid Det \; AP \; N \mid AP \; N \mid NP \; PP$$
- **Verb Phrase ($VP$)**:
  Verb phrases encompass intransitive verbs, transitive verbs with direct objects, adverbial modifiers, and prepositional attachments:
  $$VP \rightarrow V \mid V \; NP \mid V \; NP \; PP \mid V \; PP \mid Adv \; VP \mid VP \; Adv$$
- **Prepositional Phrase ($PP$)**:
  $$PP \rightarrow P \; NP$$
- **Adjective Phrase ($AP$)**:
  Handles recursive adjective stacking (e.g., "the small red house"):
  $$AP \rightarrow Adj \mid Adj \; AP$$

```
                   S
        +----------+----------+
        |                     |
        NP                    VP
   +----+----+           +----+----+
   |         |           |         |
  Det        N           V         PP
   |         |           |     +---+---+
  the     detective     sat    |       |
                               P       NP
                               |   +---+---+
                              on  Det      N
                                   |       |
                                  the    chair
```

### 4. Noun Phrase Chunking (`np_chunk`)
A noun phrase chunk is defined as any subtree with root label `NP` that does **not** contain any other `NP` subtrees inside it (i.e., minimal, flat noun phrases).

```
Tree Traversal Algorithm:
1. For each subtree in parse_tree.subtrees():
2.   If subtree.label() == "NP":
3.     If no child subtree within subtree has label "NP":
4.       Yield subtree as an NP-chunk
```

This isolates fundamental entities (e.g., `the detective`, `the chair`) from higher-order recursive noun phrases.
