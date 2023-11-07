<h2>1. Abstract</h2>
Propose WSAL method focusing on temporally localizing anomalous segments within anomalous videos. A high-order context encoding model is proposed to not only extract semantic representations but also measure the dynamic variations so that the temporal context could be effectively utilized. In addition, in order to fully utilize the spatial context information, the immediate semantics are directly derived from the segment representations. The dynamic variations as well as the immediate semantics, are efficiently aggregated to obtain the final anomaly scores. An enhancement strategy is further proposed to deal with noise interference and the absence of localization guidance in anomaly detection. Also collect a new traffic anomaly (TAD) dataset which specifies in the traffic conditions.
<h2>2. The proposed method</h2>
To predict the state of a video, estimating the anomalous margin among a video, formally: $S(\mathcal{X}) = max_{i, j = 1...m}f(\psi(x_{i - k},...,x_i,...,x_{i + k}), \psi(x_{j - k},...,x_j,x_{j + k}))$, where $\psi$ is a high-order function that encodes an anchored segment as well as its adjacent 2k segments in the temporal context, $f$ is a margin distance metric measuring the anomaly score margin between segment position $i$ and $j$, $S(\cdot)$ is the score of a video that computes maximum relative distance of pairwise positions, max function is chosen to capture the largest score margin, which can represent the extent of abnormalities in a video. In addition, augment training samples to generate 2 types of data: noise data and pseudo-locattion data.
<h3>2.1 High-order context encoding</h3>
The input is the feature vectors $(x_1,...,x_m)$ extracted from consecutive segments. The regression process is formulated as: $\overline{x}_t = \sum_{j = -k,...,k, j \neq 0}W_jx_{t + j} + W_0x_t + b$, where $W_j$ is a projection function on $j$-th segment. The neighbor size $k$ controls temporal context modeled in each local segment $\overline{x}_t$. $\psi^{sem}(\overline{x}_t) = \sigma(w_{sem}\overline{x}_t + b_{sem})$, where $\psi^{sem}(\overline{x}_t)$ represents the semantics score, $w_{sem}$ and $b_{sem}$ are the weight and bias of the fully connected layer. To measure the variation between two adjacent segments, we take the cosine similarity measurement: $cos(\overline{x}_{t - 1}, \overline{x}_t) = \overline{x}^T_{t - 1}\overline{x}/(||\overline{x}_{t - 1}||^2||\overline{x}||^2)$. The corresponding distance metric is $1 - cos(\overline{x}_{t - 1}, \overline{x}_t)$, which has a large value for dramatic variations. Then the second-order discrepancy of local variations is computed as an indicator of anomaly, which becomes: $\psi^{var}(\overline{x}_t) = (2 - cos(\overline{x}_{t - 1}, \overline{x}_t) - cos(\overline{x}_t, \overline{x}_{t + 1})))/4$, where divided by four to normalize scalar into [0, 1]. Then, obtain the singularity of a sequence from the dual context cues, with the margin measurement $f$ as L1-distance: $S^{sem}(\mathcal{X}) = max_{i, j = 1,...,m}|\psi^{sem}(\overline{x}_i) - \psi^{sem}(\overline{x}_j)|, S^{var}(\mathcal{X}) = max_{i, j = 1,...,m}|\psi^{var}(\overline{x}_i) - \psi^{var}(\overline{x}_j)|$. Place a sparsity constraint on the loss function. Added with the sparsity constraint of weigh $\beta$, the margin loss of dual context becomes: $S^O = \zeta^{sem}_1({\mathcal{X}^{(i)}}|^n_{i = 1}) + \zeta^{var}_1({\mathcal{X}^{(i)}}|^n_{i = 1}) + \frac{\beta}{n}\sum_{i = 1}^n\sum_{t = 1}^m(|s^{sem}_t| + |s^{var}_t|)$.
<h3>2.2 Enhanced weak supervision</h3>
Augment the normal video sequences with three kinds of video noise simulations, which are motion blur (kernel size: 5, angle: [−45, 45]), black/blue/purple blocks ([1/4, 1] of raw image size) and random scale (−20 to +20% on x- and y-axis independently)). Randomly choose $m$ segments in a normal video sequence to augment and the augmented data are still treated normal.
<h3>2.3 Hand-crafted anomaly</h3>
First randomly choose a pair of normal and abnormal videos. Then several random segments of the normal video are selected and fused with several segments from the abnormal video. Finally, the obtained segments are combined with the remaining normal video segments to form a pseudo anomalous sequence. To decrease the abrupt in the fused sequence, fused the features of abnormal segments with the normal ones. The coefficient of the anomaly feature ranges in [0.2,0.5] and were randomly generated during the training process.
<h2>3. Dataset</h2>
TAD and UCF-Crime.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/ktr-hubrt/WSAL.