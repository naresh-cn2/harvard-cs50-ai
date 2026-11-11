# Project 12: Attention

## Core Concept: Transformer Architecture, Masked Language Modeling & Self-Attention

### 1. Problem Overview
The Transformer architecture has fundamentally reshaped modern Artificial Intelligence, replacing recurrence with self-attention mechanisms. Using pre-trained bidirectional transformer models (BERT: Bidirectional Encoder Representations from Transformers), the objective is to predict masked words in natural language sentences (`[MASK]`) and analyze how distinct attention heads attend across token sequences by visualizing self-attention heatmaps.

### 2. Masked Language Modeling (MLM)
BERT is trained on the Cloze task: predicting masked tokens based on both left and right context:
$$\text{Text}: \text{"We turned on the [MASK] because it was dark."}$$

1. **Tokenization**: Input sentences are tokenized using WordPiece tokenizers, inserting special tokens `[CLS]` (start of sequence) and `[SEP]` (sequence boundary).
2. **Mask Token Extraction**: `get_mask_token_index(mask_token_id, inputs)` locates the exact index position of `[MASK]` in the tokenized input IDs tensor.
3. **Logit Prediction**: The model computes output logits across the entire vocabulary for the masked index. Applying top-$K$ selection:
   $$\hat{w} = \text{argmax}_{w \in V}^{(K)} \; \text{logits}[\text{mask\_index}, w]$$
   Produces candidate words (e.g., `light`, `lamp`, `lantern`).

### 3. Scaled Dot-Product Self-Attention
For input token embeddings projected into Queries ($Q$), Keys ($K$), and Values ($V$) with dimension $d_k$:
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

The attention weight matrix $A_{i, j} \in [0, 1]$ represents the magnitude of focus that token $i$ places on token $j$:
$$A_{i, j} = \frac{\exp\left(\frac{\mathbf{q}_i \cdot \mathbf{k}_j}{\sqrt{d_k}}\right)}{\sum_{l} \exp\left(\frac{\mathbf{q}_i \cdot \mathbf{k}_l}{\sqrt{d_k}}\right)}$$

```
Query Token (i) -------------\
                              Dot Product / sqrt(d_k) ---> Softmax ---> Attention Weight A_{i, j}
Key Token (j)   -------------/
```

### 4. Attention Visualization Pipeline
To render attention heatmaps across $L$ layers and $H$ heads:
1. **Grayscale Shading Function**:
   `get_color_for_attention_score(attention_score)` maps attention weights $A_{i, j} \in [0.0, 1.0]$ into 8-bit RGB grayscale pixels:
   $$\text{intensity} = \text{round}(\text{score} \times 255)$$
   $$\text{RGB} = (\text{intensity}, \text{intensity}, \text{intensity})$$
   Where $0.0 \rightarrow (0, 0, 0)$ (black, no attention) and $1.0 \rightarrow (255, 255, 255)$ (white, maximum attention).

2. **Diagram Generation**:
   `visualize_attentions(tokens, attentions)` traverses:
   - Every transformer layer $l \in \{1, \dots, L\}$
   - Every multi-head attention slice $h \in \{1, \dots, H\}$
   Drawing an $N \times N$ token grid diagram showing lexical alignment, syntax resolution, and coreference patterns learned by specific attention heads.

### 5. Architectural Takeaways
Visualizing individual attention heads exposes the specialized sub-functions learned during unsupervised pre-training:
- Certain attention heads specialize in attending to immediate neighbors (local syntax).
- Other heads specialize in verb-object dependencies or coreference links (pronoun-noun pairs across sentences).
- Specific heads attend universally to delimiter tokens `[SEP]` and `[CLS]`.
