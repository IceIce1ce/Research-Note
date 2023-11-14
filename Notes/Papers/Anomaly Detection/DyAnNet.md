<h2>1. Abstract</h2>
Use isolation tree-based unsupervised clustering to partition the deep feature space of the video segments. The RGB- stream generates a pseudo anomaly score and the flow stream generates a pseudo dynamicity score of a video segment. These scores are then fused using a majority voting scheme to generate preliminary bags of positive and negative segments. Then use a refinement strategy based on a cross-branch feed-forward network designed using a popular I3D network to refine both scores. The bags are then refined through a segment re-mapping strategy.
<h2>2. Proposed method</h2>
<h3>2.1 Overall architecture</h3>
Firsty, assign pseudo anomaly scores $\hat{y}_s$ and pseudo dynamicity scores $\hat{y}_d$ to video segments $S_i$. These intermediate labels help to form 2 separate bags $\mathcal{A}$ and $\mathcal{N}$. . In the second stage, two separate regressors $\Omega, \psi$ have been trained using these pseudo labels. In the third stage, we have used these trained regressors to refine the contents of the bags. In the next pass, $\Omega, \psi$ are tuned using $\mathcal{A}, \mathcal{N}$.
<h3>2.2 Pseudo label assignment</h3>
The anomaly score can be defined: $d(F) = max_{F \in V}\delta(c, F)$, where $F$ is the feature point, $c$ is center of smallest hypersphere constructed by SVM and $\delta$ is distance function. $d(F) = 2^{\frac{-E(l(F))}{g(|F|)}}$, where $l(F)$ is path length of $F, E(\cdot)$ denotes average path length of $F$ on $n$ isolation trees, and $g(\cdot)$ is expected path length for a given sub-sample. Let $P_k$ represents coordinate of $k^{th}$ pixel of the immediate preceding frame and $M_k$ be the estimated position of the pixel obtained using optical flow in the next frame. The displacement of the pixel $(S_k)$ can be calculated: $S_k = SAD(P_k, M_k)$, where SAD is sum of absolute difference. The frame-level dynamicity score $D_i$ of $i^{th}$ frame is estimated using: $D_i = \frac{1}{m \times n}\sum_{k = 1}^{m \times n}S_k$, where $m, n$ are height and width of frame. Assign an intermediate label $\hat{y}$ to a segment using heuristic: $\hat{y} = f(\hat{y}_s, \hat{y}_d) = 1$ if $\hat{y}_s, \hat{y}_d > \tau$ else 0.
<h3>2.3 Learning of anomaly and dynamicity scores</h3>
$\Theta(S) = f(\Omega(Z_R), \psi(Z_F))$, where $Z_R$ represents RGB frame, $Z_F$ denotes OF of segment $S, \hat{y}_s = (\Omega(Z_R)), \hat{y}_d = (\psi(Z_F))$ and $f(\cdot)$ is label mapping function.
<h3>2.4 Segment re-mapping via iterative learning</h3>
$\hat{y}^{P_{i + 1}}_S = \Omega_{P_i}(Z_R), \hat{y}^{P_{i + 1}}_d = \psi_{P_i}(Z_F)$, where $\hat{y}^{P_{i + 1}}_S, \hat{y}^{P_{i + 1}}_d$ are new scores obtained through sub-optimized regressors.
<h3>2.5 Training and inference</h3>
$y_s = \sum_{i = 1}^k\Omega_i(Z_R), y_d = \sum_{i = 1}^k\psi_i(Z_F)$, where $k$ is number of passes.
<h2>3. Datasets</h2>
UCF-Crime, CCTV-Fights and UBI-Fights.
<h2>4. Metrics</h2>
AUC.