<h2>1. Abstract</h2>
Propose an Anomalous Attention mechanism for weakly-supervised anomaly detection to tackle the aforementioned problems. The approach takes into account snippet-level encoded features without the supervision of pseudo labels. Specifically, the approach first generates snippet-level anomalous attention and then feeds it together with original anomaly scores into a Multi-branch Supervision Module.
<h2>2. Methods</h2>
Three core modules: the Temporal Embedding Unit for feature modeling, the Attention Unit for generating snippet-level anomalous attention, and the Multibranch Supervision Module for learning anomaly completeness and improving localization accuracy.
<h3>2.1 Temporal embedding unit</h3>
Same feature aggregation module of RTFM model.
<h3>2.2 Anomalous attention unit</h3>
After obtaining the enhanced feature, the Temporal Convolution layer TC is first adopted to fully capture channel-wise dependencies and infuse the local context from the neighborhood snippets. A basic attention unit can be formulated as: $F^{(l)}_e = (TC^{(l)}(F^{(l - 1)}_e); LR)$, where $F^{(l - 1)}_e$ indicates feature output from $(l - 1)^{th}$ basic unit. The feature dimension of the final TC layer is 1. Then a sigmoid function is used to obtain normalized anomalous attention.
<h3>2.3 Multi-branch supervision module</h3>
Original abnormal scores are directly obtained from the classifier with 3-layer MLP and the nodes are 512, 128, and 1 respectively. Also, each layer is followed by a ReLU and a dropout function. Denote original anomalous scores as $S^o$. Then, attention-based abnormal scores $S^a = A * S^o$. Then, calculate suppressed original abnormal scores $S^{so}$ and suppressed attention-based abnormal scores $S^{sa}$. $S^{so}_{ij} = S^o_{ij}$ if $A_{ij} < \theta_i$ else 0, where $i \in (1, N)$ denotes $i^{th}$ video and $j \in (1, T)$ denotes $j^{th}$ snippets in current sequence and $\theta_i = [max(A_i) - min(A_i)] * \varepsilon + min(A_i)$, $\varepsilon$ is suppressed rate. Finally, for abnormal scores $S^{sa}$, $S^{sa}_{ij} = S^a_{ij} if A_{ij} < \theta_i$ else 0.
<h3>2.4 Optimizing process</h3>
For negative bags, the guide loss is: $L^{neg}_{guide} = \delta(A^{neg}, \{0...0\})$, where $\delta$ is MSE and $\{0...\}$ denotes a sequence that only consists of 0 and has the same size as $A^{neg}$. For positive bag, the guide loss is: $L^{pos}_{guide} = \delta(A^{pos}, S^{pos}_o)$ if step < $M$ else $\delta(A^{pos}, \{0,1,1...1,0\})$, where $M$ represents the number of training iterations and the sequence can be obtained by 1/0 = 1 if $S^{pos}_o > 0.5$ else 0. Also utilize a normalization loss: $L_{norm} = ||A^{pos}||_1$.
<h3>2.5 Constraints on video-level supervision</h3>
$L_c = \mathcal{n}(S, Y)$, where $n$ is BCE loss and $L^{all}_c = \alpha(L^o_c + L^a_c) + (1 - \alpha)(L^{so}_c + L^{sa}_c)$.
<h3>2.6 Network training and testing</h3>
$L_{sm} = \sum^{T - 1}_{j = 1}(S_j - S_{j + 1})^2, L_{sp} = \sum^T_{j = 1}S_j$, where $L_{sm}, L_{sp}$ are only applied on $S^o, S^a$. The final loss is: $L = L^{all}_c + \gamma L_{norm} + L^{neg}_{guide} + L^{pos}_{guide} + \mu L_{sm} + L_{sp}$. The final predictions are calculated by: $S_{test} = S^a$.
<h2>3. Datasets</h2>
UCF-Crime and XD-Violence.
<h2>4. metrics</h2>
AUC and AP.
<h2>5. Code</h2>
https://github.com/2023-MindSpore-4/Code4/tree/main/WS-VAD-mindspore-main.