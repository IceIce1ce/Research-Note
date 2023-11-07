<h2>1. Abstract</h2>
Propose an end-to-end Global Information Guided (GIG) anomaly detection framework for anomaly detection using the video-level annotations. First mine the global pattern cues by leveraging the weak labels in a GIG module. Then build a spatial reasoning module to measure the relevance between vectors in spatial domain with the global cue vectors, and select the most related feature vectors for temporal anomaly detection.
<h2>2. The proposed GIG anomaly detection framework</h2>
<h3>2.1 Global information guided module</h3>
Given the extracted high-level semantic feature maps X = $\{x^i\}^T_{i = 1}$ of all segments, obtain global pattern cue by spatio-temporal dimension reduction operation. Apply GlobalMaxPooling - 3D function: g = $\psi(X)$, where $\psi$ denotes the dimension reduction operation, g is the vector of GPC with resolution $\mathbb{R}^{1 \times 1 \times 1 \times d}$. Adopt a simple channel-wise attention operation to imply the GPC vector g as a guidance for representation enhancement, formally: $\hat{X} = \sigma(g) \odot X + X$, where $\hat{X}$ represents enhanced feature maps. employ a fully connected function $\phi_1$ as the anomaly classification head to measure the anomaly status, using the GPC vector as input and outputting scores of 1+ C classes. In detail, take the maximum score among $C$ anomaly classes of the GPC vector as the overall anomaly score $S^*_g$: $S^*_g = max^C_{i = 1}(\sigma(\phi_1(g))_i)$. Then, apply a video-level supervision with BCE loss as: $l^*_g = -(y^*\log S^*_g + (1 - y^*)\log(1 - S^*_g))$.
<h3>2.2 Spatial reasoning module</h3>
At the first step, measure spatial key by calculating relation score between g and $\hat{X}$: $r_{i, j} = R(g, \hat{x}_{i, j})$. Choose the cos simliarity as the relation measurement metric. At the second step, we choose the k spatial vectors with the top-k relation scores and further reduce the spatial dimension to 1 for obtaining the spatial pattern vector $\hat{x}_s$ of each segment by aggregating spatial vectors as: $\hat{x}_s = \frac{1}{k}\sum_{i \in [1, w], j \in [1, h]}top\_k(\hat{x}_{i, j}, r_{i, j})$. Then, apply an anomaly classification head $\phi_2$ for generating pattern vectors of each segment as: $S_s = \sigma(\phi_2(\hat{x}_s))$. Pick up the top $p$ maximum segment-level scores and average them to make a segment-consensus score as: $S^T_s = \frac{1}{p}\sum_{i = 1}^Ttop\_p(S^i_s)$. Apply the Segment−level Supervision on the segment-consensus score. At first, enable a supervision on the overall anomalous status as the GPC vector: $S^*_s = max^C_{i = 1}(S^T_s)_i, l^*_s = -(y^*\log S^*_s + (1 - y^*)\log(1 = S^*_s))$. Further distinguish the anomaly instances by supervising the segment-consensus anomaly score under multi-class anomaly detection function as: $l_s = -(y\log S^T_s + (1 - y)\log(1 - S^T_s))$. The Segment − level Supervision and Video − level Supervision make up our Video-Segment loss: $l_{vs} = l_s + \lambda_1l^*_s + \lambda_2l^*_g$. Also add a sparse constraint as: $l_{sparse} = \sum_{i = 1}^TS^i_s$. The total loss: $l = l_{vs} + \lambda_3l_{sparse} = l_s + \lambda_1l^*_s + \lambda_2l^*_g + \lambda_3l_{sparse}$.
<h2>3. Datasets</h2>
CityScene.
<h2>4. Metrics</h2>
AUC.