<h2>1. Abstract</h2>
TEVAD utilizes both visual and text features. Specifically, compute text features based on the captions of the videos to capture the semantic meanings of abnormal events.
<h2>2. Method</h2>
Given training videos $\mathcal{V}$, TEVAD first splits each input video $v \in \mathcal{V}$ into $T$ snippets. Afterwards, two separate branches extract visual and text features for each snippet in parallel. The text branch generates dense captions before transforming them to sentence embeddings while the visual branch extracts visual I3D [7] features. Multi-scale temporal networks are included in both branches to better capture multi-scale temporal dependencies. The resulting multi-scale visual $F_{vis} \in \mathbb{R}^{d_{vis}}$ and text features $F_{txt} \in \mathbb{R}^{d_{txt}}$ are fused together and used to calculate the feature magnitude of snippets. Top-K largest feature magnitudes from normal and abnormal videos are passed to train a binary snippet classifier.
<h3>2.1 Generating text features for videos</h3>
<h4>2.1.1 Generating dense captions for videos</h4>
Employ a sliding window strategy and compute the caption for a consecutive 64 frames for every 16 frame. Use SwinBERT to generate the descriptions of video snippets.
<h4>2.1.2 Generating sentence embeddings for videos</h4>
To compute the text features from generated video captions, use SimCSE to generate sentence embeddings.
<h3>2.2 Generating visual features for videos</h3>
Extract ten/five-crop I3D features using a ResNet50 as backbone.
<h3>2.3 Multi-scale temporal feature learning</h3>
Extend MTN to process the text features and then fuse them with visual features. The outputs from the two blocks are concatenated and added to the original features to produce the final output of text MTN denoted as $\overline{F}_{txt} = f_{MTN}(F_{txt}; \theta)$, where $\overline{F}_{txt} \in \mathbb{R}^{d_{txt}}$ and $\theta$ comprises weights for all convolution functions. Both visual and text features go through the similar process thus we have $\overline{F}_{vis} = f_{MTN}(F_{vis}; \theta)$, where $\overline{F}_{vis} \in \mathbb{R}^{d_{vis}}$.
<h3>2.4 Multi-modal feature fusion</h3>
Use three different fusion methods: concatenation, addition and product. Since visual features are five/ten-cropped, the text features are tiled for five/ten times to be consistent with visual features. Overall, use $X = f_{fuse}(\overline{F}_{vis}, \overline{F}_{txt}; \delta)$ to denote the fused features. Three fully connected layers are added to calculate the anomaly scores given by $s = f_{pred}(X; \delta)$. Additionally, $S = \{s_i\}^T_1$ denotes the anomaly scores of snippets in one video $v = \{X_i\}^T_1$.
<h3>2.5 Model training</h3>
Use $l_2$ norm to compute feature magnitude. topK$(v; k)$ is used to denote such a subset which includes $k$ snippets with highest magnitude among $T$ snippets in a video. The feature magnitude of a video $v$ is computed as: $f_{FM}(v; k) = \frac{1}{k}\sum_{X_i \in topK(v; k)}||X_i||_2$. The total training loss of normal and abnormal videos in one batch are denoted as: $\mathcal{L}_{fm} = \sum_{j = 1}^{|\mathcal{V}|}(c - f_{FM}(v_j; k))$, if $y_j = 1$ else $\mathcal{L}_{fm} = \sum_{j = 1}^{|\mathcal{V}|}f_{FM}(v_j; k)$, where $c$ is a pre-defined constant and $|\mathcal{V}|$ is the number of videos in training set. Similarly, the average of the selected k snippets’ anomaly scores is calculated to represent the anomaly score of the whole video as: $f_s(v; k) = \frac{1}{k}\sum_{X_i \in topK(v; k)}f_{pred}(X_i; \delta)$. For the actual anomaly detection, $\mathcal{L}_{bce} = -\frac{1}{|\mathcal{V}|}\sum_{j = 1}^{|\mathcal{V}|}(y_j\log(f_s(v_j; k)) + (1 - y_i)\log(1 - f_s(v_j; k)))$. Overall, the loss function is given as: $\mathcal{L} = \alpha\mathcal{L}_{fm} + \mathcal{L}_{bce}$, where $\alpha = 0.0001$.
<h2>3. Datasets</h2>
UCSD Ped2, ShanghaiTech, UCF-Crime and XD-Violence.
<h2>4. Metrics</h2>
AUC and AP.
<h2>5. Code</h2>
https://github.com/coranholmes/TEVAD.