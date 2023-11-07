<h2>1. Abstract</h2>
Provide a supervised learning task under noisy labels. Devise a GCN to correct noisy labels. Based upon feature similarity and temporal consistency, the network propagates supervisory signals from high-confidence snippets to low-confidence ones.
<h2>2. Graph convolutional label noise cleaner</h2>
Adopt an EM-like optimization mechanism. At each training step of the noise cleaner, obtained rough snippet-wise anomaly probabilities from the action classifier, and the target of our noise cleaner is to correct low-confidence anomaly scores via high-confidence ones. Leverage two characteristics of a video to correct the label noise: feature similarity and temporal consistency. Feature similarity means the anomaly snippets share some similar characteristics, while temporal consistency means anomaly snippets probably appear in temporal proximity of each other.
<h3>2.1 Feature similarity graph module</h3>
Features from the action classifier are first compressed with two fully connected layers to mitigate the curse of dimensionality. Model the feature similarity with an attributed graph $F = (V, E, X)$, where $V$ is a video, $E$ describes feature similarity amongst snippets and $X \in \mathbb{R}^{N \times d}$ represents $d$-dimensional feature of these $N$ snippets. The adjacency matrix $A^F \in \mathbb{R}^{N \times N}$ of $F$ is defined: $A^F_{(i, j)} = \exp(X_i \cdot X_j - max(X_i \cdot X))$. Since an adjacency matrix should be non-negative, bound the similarity to the range (0, 1] with a normalized exponential function. The nearby vertexes are driven to have the same anomaly label via graph-Laplacian operations: $\hat{A}^F = \overline{D}^{F^{-\frac{1}{2}}}\overline{A}^F\overline{D}^{F^{-\frac{1}{2}}}$. Finally, the output of a feature similarity graph module layer is computed as: $H^F = \sigma(\hat{A}^FXW)$.
<h3>2.2 Temporal consistency graph module</h3>
Its adjacency matrix $A^T \in \mathbb{R}^{N \times N}$ is only dependent on temporal positions of $i^{th}$ and $j^{th}$ snippets: $A^T_{(i, j)} = k(i, j)$, where $k(i, j) = \exp(-||i - j||)$ is Laplacian kernel. The forward result of this module is: $H^T = \sigma(\hat{A}^TXW)$.
<h3>2.3 Loss function</h3>
$\mathcal{L} = \mathcal{L}_D + \mathcal{L}_I$, where $\mathcal{L}_D = -\frac{1}{|H|}\sum_{i \in H}|\overline{y}_i\ln p_i + (1 - \overline{y}_i)\ln(1 - p_i)]$, where $H$ is the set of high-confidence snippets and $L_{I} = \frac{1}{N}\sum_{i = 1}^N|p_i - \overline{p}_i|$, where $\overline{p}_i$ is the discount-weighted average predictions of noise cleaner over various training epochs.
<h2>3. Datasets</h2>
UCF-Crime, Shanghaitech and UCSD-Peds.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/jx-zhong-for-academic-purpose/GCN-Anomaly-Detection.