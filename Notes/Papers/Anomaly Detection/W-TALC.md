<h2>1. Abstract</h2>
W-TALC can be divided into two sub-networks, namely the Two-Stream based feature extractor network and a weakly-supervised module, which learn by optimizing two complimentary loss functions.
<h2>2. Methodology</h2>
<h3>2.1 Weakly supervised layer</h3>
- Fully connected layer: introduce a fully connected layer followed by ReLU and Dropout on the extracted features. The operation can be formalized for a video with index $i$ as follows: $X_i = \mathcal{D}(max(0, W_{fc}[X^r_i X^O_i] \oplus b_{fc}), k_p)$, where $\mathcal{D}$ represents Dropout with $k_p$ representing its keep probability, $\oplus$ is addition, $W_{fc} \in \mathbb{R}^{2048 \times 2048}$ and $b \in \mathbb{R}^{2048 \times 1}$, $X_i \in \mathbb{R}^{2048 \times l_i}$.
- Label space projection: project the representations $X_i$ to label space using a FC layer with weight sharing along temporal axis. The class-wise activations after this projection can be represented as: $\mathcal{A}_i = W_aX_i \oplus b_a$, where $W_a \in \mathbb{R}^{n_c \times 2048}, b_a \in \mathbb{R}^{n_c}$ are to be learned and $\mathcal{A}_i \in \mathbb{R}^{n_c \times l_i}$.
<h3>2.2 k-max multiple instance learning</h3>
Set $k$ proportional to number of elements in a bag. Specifically, $k_i = max(1, \lfloor\frac{l_i}{s}\rfloor)$, where $s$ is a design parameter. The MILL is then the CE between predicted pml $p_i$ and ground-truth: $\mathcal{L}_{MILL} = \frac{1}{n}\sum_{i = 1}^n\sum_{j = 1}^{n_c}-y^j_i\log(p^j_i)$.
<h3>2.3 Co-activitiy similarity</h3>
First normalize the per-video class-wise activations scores along the temporal axis using softmax non-linearity as follows: $\hat{\mathcal{A}}_i[j, t] = \frac{\exp(\mathcal{A}_i[j, t])}{\sum_{t'=1}^{l_i}\exp(\mathcal{A}_i[j, t'])}$, where $t$ indicates time instants and $j \in \{1,...,n_c\}$. Use cosine similarity in order to obtain a measure of the degree of similarity between two feature vectors: $d[f_i, f_j] = 1 - \frac{<f_i, f_j>}{<f_i, f_i>^{\frac{1}{2}}<f_j, f_j>^{\frac{1}{2}}}$.
<h3>2.4 Optimization</h3>
The total loss: $\mathcal{L} = \lambda\mathcal{L}_{MILL} + (1 - \lambda)\mathcal{L}_{CASL} + \alpha||W||^2_F$.
<h2>3. Datasets</h2>
ActivityNet 1.2 and Thumos14.
<h2>4. Metrics</h2>
IoU and mAP.
<h2>5. Code</h2>
https://github.com/sujoyp/wtalc-pytorch.