<h2>1. Abstract</h2>
First release a large-scale and multi-scene dataset named XD-Violence with a total duration of 217 hours, containing 4754 untrimmed videos with audio signals and weak labels. Then propose a neural network containing three parallel branches to capture different relations among video snippets and integrate features, where holistic branch captures long-range dependencies using similarity prior, localized branch captures local positional relation using proximity prior, and score branch dynamically captures the closeness of predicted score. Besides, the method also includes an approximator to meet the needs of online detection.
<h2>2. Methodology</h2>
<h3>2.1 Multimodal fusion</h3>
Use feature extractors $F^V$ and $F^A$ to extract visual and audio feature matrix $X^V$ and $X^A$ using sliding window mechanism where $X^V \in \mathbb{R}^{T' \times d^V}, X^A \in \mathbb{R}^{T' \times d^A}, T'$ is the length of feature matrix, $x^V_i, x^A_i$ are visual and audio features of $i^{th}$ snippet. $X^V$ and $X^A$ are first concatenated in the channels, then the concatenation passes through two FC layers with 512 and 128 nodes. Finally, the output is $X^F$.
<h3>2.2 Holistic and Localized Networks</h3>
- Holistic branch implementation: the holistic relation matrix is defined by feature similarity prior as: $A^H_{ij} = g(f(x_i, x_j))$, where $A^H \in T' \times T', A^H_{ij}$ measures the feature similarity between the $i^{th}$ and $j^{th}$ features, $g$ is the normalization function and $f(x_i, x_j) = \frac{x^T_ix_j}{||x_i||_2 \cdot ||x_j||_2}$. The thresholding operation can be defined as: $f(x_i, x_j) = f(x_i, x_j)$ if $f(x_i, x_j) > \tau$ else 0. After that, adopt softmax as the normalization function $g$ to make sure the sum of each row of $A$ is 1. In order to capture long-range dependencies, design the holistic layer as: $X^H_{l + 1} = Dropout(ReLU(A^HX^H_lW^H_l))$.
- Localized branch implementation: devise the local relation matrix based on proximity prior as: $A^L_{ij} = exp(\frac{-|i - j|^\gamma}{\sigma})$. Likewise, $X^L_{l + 1}$ is the output of $(l + 1)^{th}$ localized layer.
<h3>2.3 Online detection</h3>
HLC approximator, only taking previous video snippets as input, to generate precise predictions guided by HL-Net. Two stacked FC layers followed by ReLU and a 1D causal convolution layer constitute HLC approximator. The 1D causal convolution layer has kernel size 5 in time with stride 1, sliding convolutional filters over time.
<h3>2.4 Score branch implementation</h3>
The relation matrix of score branch is devised as follows: $A^S_{ij} = p(1 - |s(C^S_i) - s(C^S_j)|), p(x) = \frac{1}{1 + \exp(-\frac{x - 0.5}{0.1})}$, where $s$ is sigmoid, $p$ is used to enhance (and weak) the pairwise relation where the closeness of the scores is greater (and less) than 0.5. Analogously, $X^S_{l + 1}$ is the output of $(l + 1)^{th}$ score layer, where $X^S_0 = (X^H_0 = X^L_0) = X^F$.
<h3>2.5 Training based on MIL</h3>
$C^P = (X^H||X^L||X^S)W$, where || denotes the concatenation operation and $C^P \in \mathbb{R}^{T'}$ denotes the violent activations. Use the average of K-max activation $(C^P, C^S)$ over the temporal dimension rather than the whole activations to compute $y^P, y^S$ and K is defined as $\lfloor\frac{T'}{q} + 1\rfloor$. . The instances corresponding to the K-max activation in the positive bag is most likely to be true positive instances (violence). Use the knowledge distillation loss to encourage the output of the HLC approximator to approximate the output of HL-Net: $L_{DISTILL} = \sum_{j = 1}^N(-\sum_is(C^P_i)log(s(C^S_i)))_j$, where N is batch size. Finally the total loss is: $L_{TOTAL} = L_{BCE} + L_{BCE2} + \lambda L_{DISTILL}$.
<h3>2.6 Inference</h3>
Only HLC approximator works, and HL-Net can be removed.
<h2>3. Datasets</h2>
XD-Violence.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/Roc-Ng/XDVioDet.