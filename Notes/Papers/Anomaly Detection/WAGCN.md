<h2>1. Introduction</h2>
Propose WAGCN to model the complex contextual relationship among video segments. Firstly, combine the temporal consistency as well as feature similarity of video segments to construct a global graph, which makes full use of the association information among spatial-temporal features of anomalous events in videos. Secondly, propose a graph learning layer in order to break the limitation of setting topology manually, which can extract graph adjacency matrix based on data adaptively and effectively.
<h2>2. Proposed method</h2>
<h3>2.1 Graph construction module</h3>
Combine the similarity of video segments’ spatial-temporal features and the degree of video segments’ temporal proximity to construct a global graph. First use two linear layers followed with a ReLU to embed feature $X$. Then, use dot product to measure similarity as well as adjacency of any two segments in $G^F$. Since an adjacency matrix should be non-negative, bound similarity to (0, 1) with the normalized exponential function. Therefore, the adjacency matrix $A^F$ of $G^F$ is defined: $A^F = softmax(relu(XW_1)relu(W^T_2X^T))$, where $A^F$ is a $T \times T$-size learnable matrix representing adjacency between segments based on similarity.
<h3>2.2 Temporal consistency graph</h3>
The temporal consistency graph $G^T$ is built directly based on temporal structure of video. Its adjacency matrix $A^T \in R^{T \times T}$ depends on temporal position of $i$-th and $j$-th segments: $A^T_{ij} = \exp(-|i - j|)$.
<h3>2.3 Graph convolutional module</h3>
Add residual connections between adjacent layers and use summation for aggregation which is plug-and-play.
<h3>2.4 Training loss of proposed algorithm</h3>
Use $k$-max loss function. Specifically, given the abnormal score vector $s = \{s_i\}^T_{i = 1}$ of video $V$, choose top$-k$ elements in $s$ denoted as $S = \{s_i\}^k_{i = 1}$, where $k = \lfloor \frac{T}{8} + 1 \rfloor$. The final classification is BCE: $L_{k - max} = -\frac{1}{k}\sum_{s_i \in S}[y_i\log(s_i) + (1 - y_i)\log(1 - s_i)]$.
<h2>3. Datasets</h2>
UCF-Crime and ShanghaiTech.
<h2>4. metrics</h2>
AUC.