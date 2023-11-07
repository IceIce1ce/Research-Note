<h2>1. Abstract</h2>
The present paper uses better quality transformer-based features named Videoswin Features followed by the attention layer based on dilated convolution and self attention to capture long and short range dependencies in temporal domain.
<h2>2. Proposed method</h2>
<h3>2.1 Stage 1 (feature extraction)</h3>
The first step is to divide the videos into frames. Now the set of these frames (T) makes the video for feature extraction where each video has RGB channels. This gives us the input dimension as N $\times$ C $\times$ T $\times$ H $\times$ W.
<h3>2.2 Stage 2 (attention layer)</h3>
Given an input feature map $F \in \mathbb{R}^{T \times D}$, it produces the output attention feature maps $F' \in \mathbb{R}^{T \times D}$. . It consists of two modules, the one on the left is a short range module, it is used to capture shortterm temporal dependencies and the one on the right is a long range module it is used to compute global temporal context. To calculate the global temporal context, the pairwise temporal self attention is calculated which produces the feature map $M \in \mathbb{R}^{T \times T}$. It first applies the conv1D layer to reduce information to $F^c \in \mathbb{R}^{T \times D/4}$ where $F^c = \mathrm{conv1D}(F)$, then it applies 3 conv1d layers separately. $F^{c1} = \mathbb{conv1D}(F^c), F^{c2} = \mathbb{conv1D}(F^c), F^{c3} = \mathbb{conv1D}(F^c)$. It will combine these 3 conv1D layers with $F^{c4} = \mathrm{conv1D}((F^{c1} * (F^{c2})^T) * F^{c3})$. A residual is added, which gives the final output, $M = F^{c4} + F^c$, where $M \in \mathbb{R}^{T \times T}$. To calculate the short term temporal dependencies it applies the conv1D layer which gives it the output, $K = \mathrm{conv1D}(F)$, where $F \in \mathbb{R}^{T \times D}$. The output M from the long range module is concatenated with the output K from the short range module and a residual connection is added to give us the final output, $F' = \mathrm{concat}(M, K) + F^c$, where $F \in \mathbb{R}^{T \times D}$.
<h3>2.3 Stage 3 (anomaly detection)</h3>
The proposed anomaly detection model uses RTFM model.
<h2>3. Datasets</h2>
ShanghaiTech.
<h2>4. Metrics</h2>
ROC and AUC.