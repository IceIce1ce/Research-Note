<h2>1. Abstract</h2>
It comprises a detour fusion module for multimodal fusion. Additionally, contribute two branches of fully hyperbolic graph convolutional networks that excavate feature similarities and temporal relationships among snippets in hyperbolic space.
<h2>2. Method</h2>
<h3>2.1 Multimodal fusion</h3>
Feed visual features into FC layers, ensuring that the visual features have the same dimension as the audio ones. Then, concatenate the visual and audio features to form a joint representation, denoted as $X = f_v(X^V) \oplus X^A$, where $f_v$ is a two-layered FC and $X \in \mathbb{R}^{T \times 2d}$.
<h3>2.2 HFSG branch</h3>
First project fused features $X$ into hyperbolic space by exp map and have $\hat{X} \in \mathbb{L}^{T \times 2d}_K$. Then, define adj matrix $A^\mathbb{L} \in \mathbb{R}^{T \times T}$ via hyperbolic feature similarity: $A^\mathbb{L}_{ij} = softmax(g(\hat{x}_i, \hat{x}_j)), g(\hat{x}_i, \hat{x}_j) = exp(-d^K_\mathbb{L}(\hat{x}_i, \hat{x}_j))$. Before softmax normalization, also employ thresholding: $g(\hat{x}_i, \hat{x}_j) = g(\hat{x}_i, \hat{x}_j)$ if $> \tau$ else 0. Given hyperbolic embedding $\hat{X}$, leverage hyperbolic linear layer $HL(\cdot)$ for feature transformation. The output of this branch is: $\hat{X}^\mathbb{L} = Dropout(LeakyReLU(\hat{X}^{l + 1}))$.
<h3>2.3 HTRG branch</h3>
Construct a temporal relation graph directly based on the temporal structure of a video and learn the temporal relation among snippets in hyperbolic space. Its adj matrix $A^\mathbb{T} \in \mathbb{R}^{T \times T}$ is only dependent on temporal positions of $i$th and $j$th snippets which can be defined as: $A^\mathbb{T}_{ij} = exp(-||i - j||^\gamma)$. The final output is also computed as: $\hat{X}^\mathbb{L} = Dropout(LeakyReLU(\hat{X}^{l + 1}))$.
<h3>2.4 Hyperbolic classifier</h3>
Concatenate the embeddings and input them into a hyperbolic classifier, which can be formalized as: $S = \sigma((\epsilon + \epsilon <\hat{X}^\mathbb{L} \oplus \hat{X}^\mathcal{T}, W>_\mathbb{L}) + b)$.
<h3>2.5 Objective function</h3>
The obj function is: $L_{MIL} = \frac{1}{N}\sum_{i = 1}^N-Y_ilog(\overline{S})$, where $\overline{S}$ is average value of k-max predictions in video bag and $Y_i$ is binary video-level annotation.
<h2>3. Datasets</h2>
XD-Violence.
<h2>4. Metrics</h2>
AP.
<h2>5. Code</h2>
https://github.com/xiaogangpeng/HyperVD.