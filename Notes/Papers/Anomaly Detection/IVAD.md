<h2>1. Abstract</h2>
Represents every object by its velocity and pose. The anomaly scores are computed using a density-based approach.
<h2>2. Methodology</h2>
<h3>2.1 Overview</h3>
IVAD contains three stages: pre-processing, feature extraction, and density estimation. In preprocessing stage, a motion estimator is applied to predict the optical flow of each frame. An object detector is also used to localize and classify the bounding boxes of all objects within a frame. Then use the outputs of both models to extract velocity, pose, and deep representations to create object-level feature descriptors. Finally, the anomaly score of each test frame is computed using density estimation.
<h3>2.2 Pre-processing</h3>
- Optical flow: extract OF for each frame $f \in c$ in every video clip $c$ using OF model. The optical flow map is denoted by $o$.
- Object detection: first detect all objects in each frame using an object detector. Formally, object detection generates a set of $m$ bounding boxes $b_1, b_2..b_m$ for each frame, with corresponding class labels $y_1, y_2..y_m$.
<h3>2.3 Feature extraction</h3>
- Velocity features: cropping the frame-level optical flow map by the bounding box of each object detected by the object detector. Following this step, obtain a set of cropped object flow maps. These flow maps are then rescaled to a fixed size $H_{flow} \times W_{flow}$. Next, propose to use as a feature the average motion for each orientation. The orientation for each flow vector is computed as $\theta = \mathrm{atan}2(y, x)$. The orientations are quantized into $B \in \mathbb{N}$ equi-spaced bins and the final representation consists of the average flow magnitudes of the flow vectors assigned to each bin. Formally, let $o$ be a flow map of an object and let $b \in [B]$. Then all flow vectors $[x, y]^T$ of $o$ with direction $\theta$ in range: $2\pi\frac{b - 1}{B} \leq \theta \leq 2\pi\frac{b}{B}$ will contribute $m = ||x|| + ||y||$ to the sum in bin $b$. Denote velocity feature extractor as: $\phi_{velocity}: H_{flow} \times W_{flow} \rightarrow \mathbb{R}^B$.
- Pose features: obtains pose feature descriptors for each human object $o$ using AlphaPose, denoted by $\hat{\phi}_{pose}(o) \in \mathbb{R}^{2 \times d}$, where $d \in \mathbb{N}$ is the number of keypoints. Perform a simple normalization stage to ensure that the keypoints are invariant to the position and size of the human. First subtract from each landmark, the coordinates of the top-left corner of the object bounding box. Then scale the $x$ and $y$ axes so that the object bounding box has a final size of $H_{pose} \times W_{pose}$. Let $l \in \mathbb{R}^2$ be the top-left corner of the human bounding box. The pose description becomes: $\phi_{pose}(o) = (\frac{H_{pose}}{height(o)}, 0, 0, \frac{W_{pose}}{width(o)})(\hat{\phi}_{pose}(o) - l)$, where $height(o), width(o)$ is the object $o$ bouding box height and width. Finally, flatten $\phi_{pose}$ to obtain final pose feature vector.
- Deep features: use a pretrained CLIP encoder denoted by $\phi_{deep}(\cdot)$ to represent each object bounding box in each frame.
<h3>2.4 Density estimation</h3>
To estimate the density, fit a separate estimator for each feature. For velocity features, use GMM. For pose and deep features, estimate their density using $k$NN. Denote density estimators by $s_{velocity}(\cdot), s_{pose}(\cdot), s_{deep}(\cdot)$.
<h3>2.5 Score calibration</h3>
$\mu_{velocity} = \mathrm{max}_x\{s_{velocity}(\phi_{velocity}(x))\}$, $\mathcal{v}_{velocity} = \mathrm{min}_x\{s_{velocity}(\phi_{velocity}(x))\}$, $\mu_{pose} = \mathrm{max}_x\{s_{pose}(\phi_{pose}(x))\}$, $\mathcal{v}_{pose} = \mathrm{min}_x\{s_{pose}(\phi_{pose}(x))\}$, $\mu_{deep} = \mathrm{max}_x\{s_{deep}(\phi_{deep}(x))\}$, $\mathcal{v}_{deep} = \mathrm{min}_x\{s_{deep}(\phi_{deep}(x))\}$. 
<h3>2.6 Inference</h3>
Each inference clip $c = \{f_1,...,f_n\}$ is fed frame by frame into both the optical flow estimator and the object detector. Then extract three features from each object $o_i$. Train density estimators for each feature. The score for every frame is simply the maximum score across all objects. The final anomaly score is the sum of the individual feature scores normalized by calibration parameters.
<h2>3. Datasets</h2>
UCSD Ped2, CUHK Avenue and ShanghaiTech.
<h2>4. Metrics</h2>
AUROC.
<h2>5. Code</h2>
https://github.com/talreiss/Accurate-Interpretable-VAD.