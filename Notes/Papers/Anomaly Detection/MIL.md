<h2>1. Abstract</h2>
Learn anomalies by exploiting both normal and anomalous videos. Consider normal and anomalous videos as bags and video segments as instances in MIL and automatically learn a deep anomaly ranking model that predicts high anomaly scores for anomalous video segments. Furthermore, introduce sparsity and temporal smoothness constraints in the ranking loss function to better localize anomaly during training.
<h2>2. Method</h2>
<h3>2.1 Deep MIL ranking model</h3>
Pose anomaly detection as a regression problem. The straightforward approach would be to use a ranking loss such as: $f(\mathcal{V}_a) > f(\mathcal{V}_n)$, where $\mathcal{V}_a$ and $\mathcal{V}_n$ represent anomalous and normal video segments, $f(\mathcal{V}_a)$ and $f(\mathcal{V}_n)$ represent corresponding predicted anomaly scores ranging from 0 to 1. However, in the absence of video segment level annotations, it is not possible to use the above equation. Instead, propose the following multiple instance ranking objective function: max$_{i \in \mathcal{B}_a}f(\mathcal{V}_a^i)$ > max$_{i \in \mathcal{B}_n}f(\mathcal{V}^i_n)$, where max is taken over all video segments in each bag. By incorporating sparsity and smoothness constraints on the instance scores, the loss function becomes: $l(\mathcal{B}_a, \mathcal{B}_n) = \mathrm{max}(0, 1 - \mathrm{max}_{i \in \mathcal{B}_a}f(\mathcal{V}^i_a)) + \mathrm{max}_{i \in \mathcal{B}_n}f(\mathcal{V}^i_n)) + \lambda_1\sum_i^{(n - 1)}(f(\mathrm{V}^i_a) - f(V^{i + 1}_a))^2 + \lambda^2\sum_i^n f(\mathcal{V}^i_a)$. Finally, the complete objective function is: $\mathcal{L}(\mathcal{W}) = l(\mathcal{B}_a, \mathcal{B}_n) + \lambda_3||\mathcal{W}||_F$, where $\mathcal{W}$ represents model weights.
<h3>2.2 Bags formations</h3>
Divide each video into equal number of non-overlapping temporal segments and use these video segments as bag instances. Given each video segment, extract 3D convolution features.
<h3>3. Datasets</h3>
UMN, UCSD, Avenue, Subway and BOSS.
<h2>4. Metrics</h2>
ROC, AUC, EER.
<h2>5. Code</h2>
https://github.com/WaqasSultani/AnomalyDetectionCVPR2018.