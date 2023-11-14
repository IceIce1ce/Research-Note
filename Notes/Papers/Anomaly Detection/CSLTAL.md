<h2>1. Proposed method</h2>
<h3>1.1 Model</h3>
Each video is split into segments of 16 frames, with no overlap between consecutive segments.
<h3>1.2 Attention coefficients</h3>
The aim of this module is to assign a weight $\lambda_t \in [0, 1]$ to each element of the input sequence. The module initially performs a masked temporal 1D convolution to allow each feature vector encode information from the past. Such a transformation – which does not alter the number of input feature vectors – is followed by two fully connected layers activated by ReLU functions, except for the last layer where a sigmoid function is employed.
<h3>1.3 Video-level representation</h3>
$x = \sum_{t = 1}^T\lambda_tx_t$. Finally, feed it to a classifier $g(\cdot)$ composed of 2 fully FC layers.
<h3>1.4 Training objective</h3>
The first constraint: $\mathcal{L}_{sp} = ||\lambda||_1$, the second one: $\mathcal{L}_{sm} = \sum_{t = 1}^{T - 1}(\lambda_t - \lambda_{t + 1})^2$.
<h3>1.5 Alignment loss</h3>
Split each sequence ($x_1,...,x_T$) into fixed-size blocks, whose length $L$ is a hyperparameter which is set to 3. Then, randomly choose a feature vector within each block. Once the variants $\mathcal{X}_A, \mathcal{X}_B$ have been created, minimize the following objective function: $\mathcal{L}_a = \sum_{t = 1}^T(\lambda^A_t - \lambda^B_t)^2$, where $\lambda^A, \lambda^B$ are attention coefficients computed by network for $\mathcal{X}_A, \mathcal{X}_B$. 
<h3>1.6 Overall objective</h3>
$\mathcal{L} = \mathcal{L}_{cl} + \alpha\mathcal{L}_{sp} + \beta\mathcal{L}_{sm} + \gamma\mathcal{L}_a$, where $\mathcal{L}_{cl}$ is BCE loss.
<h3>1.7 Temporal proposal</h3>
To generate the proposal, compute a 1-d activation map in temporal domain named T-CAM which indicates the relevance of segment $t$ in the prediction of one of two classes. Each value $a_t$ of such activation map is computed as $a_t = g(x_t)$ if masking the contributions of all time-steps except the $t$-th one. Furthermore, extract weighted T-CAM which combines attention weight and T-CAM activation values ($\psi_t = \lambda_t \cdot a_t$). The anomaly score for each proposal is then computed as: $\sum_{t = t_{start}}^{t_{end}} = \lambda_t \cdot \frac{a_t}{t_{end} - t_{start} - 1}$.
<h2>2. Datasets</h2>
XD-Violence.
<h2>3. Metrics</h2>
AUC and AP.
<h2>4. Code</h2>
https://github.com/aimagelab/CSL-TAL.