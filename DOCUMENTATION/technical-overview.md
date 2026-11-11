# Technical Overview & System Architecture

A comprehensive technical blueprint of the software tools, frameworks, and architecture patterns utilized throughout the Harvard CS50 AI project implementations.

---

## 1. Programming Languages & Runtime Environments
- **Primary Language**: Python 3.10 / 3.11 / 3.12 / 3.14
- **Execution Runtime**: Cross-platform POSIX and Linux WSL environments with strict typing and modern idiomatic paradigms.
- **Package Management**: Virtual environments (`venv`, `pipx`) ensuring isolated dependency trees and reproducible execution.

---

## 2. Core Libraries & Ecosystem Stack

```
+---------------------------------------------------------------------------------+
|                                 APPLICATION LAYER                               |
|        Graph Search  |  Propositional Engines  |  RL Agents  |  Vision & NLP    |
+---------------------------------------------------------------------------------+
|                                 FRAMEWORK LAYER                                 |
|      TensorFlow / Keras     |     HuggingFace Transformers     |      scikit-learn     |
+---------------------------------------------------------------------------------+
|                             SCIENTIFIC & UTILITY STACK                          |
|         NumPy         |         Pandas         |         OpenCV / PIL           |
+---------------------------------------------------------------------------------+
|                           ACADEMIC VERIFICATION TOOLING                         |
|                    check50 (Unit Assertions)  |  submit50 (Git Engine)          |
+---------------------------------------------------------------------------------+
```

### Detailed Library Usage
| Category | Library | Purpose & Applied Context |
| :--- | :--- | :--- |
| **Deep Learning** | `tensorflow` | Building, compiling, training, and evaluating deep neural networks (`Sequential`, `Conv2D`, `MaxPooling2D`, `Dense`, `Dropout`). |
| **NLP & Transformers** | `transformers` | Pre-trained BERT model architectures (`AutoTokenizer`, `TFBertForMaskedLM`), masked token indexing, attention tensor extraction. |
| **Natural Language** | `nltk` | Context-Free Grammars, recursive descent parsing, word tokenization, noun phrase chunking. |
| **Classical ML** | `scikit-learn` | $k$-Nearest Neighbors classification (`KNeighborsClassifier`), dataset train/test splits. |
| **Computer Vision** | `opencv-python` (`cv2`) | Highway signage image loading, resizing, color space standardization, feature normalization. |
| **Imaging & Graphics** | `Pillow` (`PIL`) | Generating self-attention heatmaps, word-grid rendering, and image matrix visualization. |
| **Scientific Computing** | `numpy` | High-performance multi-dimensional array operations, argmax selections, vectorized math. |
| **Data Ingestion** | `pandas` / `csv` | Tabular data parsing, categorical feature encoding, and dataset preprocessing. |

---

## 3. Engineering Quality & Automated Testing
- **Harness Verification**: Every project is assessed using Harvard's automated grading framework (`check50`), validating code structure, imports, deterministic behavior, and edge-case handling against thousands of assertions.
- **Version Control Automation**: Workflows adhere to standard Git branch mechanics and cryptographic commit verification via SSH / PAT authentication.
- **Cross-Platform Compatibility**: Implementations strictly avoid OS-dependent path delimiters, ensuring identical execution across Linux, macOS, and Windows.
