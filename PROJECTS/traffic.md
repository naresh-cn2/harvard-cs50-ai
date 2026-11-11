# Project 10: Traffic

## Core Concept: Computer Vision & Convolutional Neural Networks (CNNs)

### 1. Problem Overview
Autonomous vehicles depend on rapid, high-accuracy classification of roadside traffic signs under varied lighting, rotations, and weather conditions. Using the German Traffic Sign Recognition Benchmark (GTSRB) comprising thousands of color photographs across 43 distinct traffic sign categories, the goal is to design, train, and validate a deep Convolutional Neural Network (CNN) in TensorFlow/Keras.

### 2. Dataset Pipeline & Image Preprocessing
- **Source**: 43 directories representing traffic sign classes (Speed Limit 30, Stop, Yield, No Entry, Turn Right, etc.).
- **Image Standardization**:
  - Color images of arbitrary original resolutions are loaded using OpenCV (`cv2.imread`).
  - Resized to standardized input dimensions: $30 \times 30 \times 3$ RGB tensors (`cv2.resize`).
  - Feature normalization scales pixel intensities from $[0, 255]$ into floating-point range $[0.0, 1.0]$.
  - One-hot encoding transforms integer class labels $y \in [0, 42]$ into 43-dimensional probability targets.

### 3. Neural Network Architecture
The deep vision pipeline incorporates feature extraction stages followed by dense classification layers:

```
[Input: 30x30x3]
       |
       v
[Conv2D: 32 filters, 3x3 kernel, ReLU]
       |
       v
[MaxPooling2D: 2x2 pool size]
       |
       v
[Conv2D: 64 filters, 3x3 kernel, ReLU]
       |
       v
[MaxPooling2D: 2x2 pool size]
       |
       v
[Flatten: 1D feature vector]
       |
       v
[Dense: 128 units, ReLU]
       |
       v
[Dropout: 0.5 regularization]
       |
       v
[Dense: 43 units, Softmax Output]
```

### 4. Mathematical Layer Operations
1. **2D Convolution**:
   Transforms input feature map $X$ using learnable kernel $W$:
   $$S(i, j) = (X * W)(i, j) = \sum_{m} \sum_{n} X(i - m, j - n) W(m, n) + b$$
2. **Rectified Linear Unit (ReLU)**:
   Introduces non-linearity to learn hierarchical visual features:
   $$\text{ReLU}(z) = \max(0, z)$$
3. **Max Pooling**:
   Downsamples spatial dimensions to enforce translation invariance:
   $$P(i, j) = \max_{(m, n) \in \text{window}} S(2i + m, 2j + n)$$
4. **Dropout Regularization**:
   Randomly sets activations to 0 with probability $p = 0.5$ during training, preventing co-adaptation of neurons and mitigating overfitting.
5. **Softmax Cross-Entropy**:
   Computes categorical probability distribution over 43 classes:
   $$\hat{y}_k = \frac{e^{z_k}}{\sum_{j=1}^{43} e^{z_j}}, \quad \mathcal{L} = -\sum_{k=1}^{43} y_k \log(\hat{y}_k)$$

### 5. Training & Evaluation
- **Optimizer**: Adam ($\eta = 0.001$).
- **Test Accuracy**: $> 95\%$ on unseen validation splits, achieving robust identification of road signage across severe visual noise.
