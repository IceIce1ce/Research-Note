<h2>1. Abstract</h2>
Propose Unbiased MIL to learn unbiased anomaly features that improve WSVAD. At each MIL training iteration, use current detector to divide samples into 2 groups with different context biases: the most confident abnormal/normal snippets and the rest ambiguous ones. Then, by seeking the invariant features across the two sample groups, remove the variant context biases.
<h2>2. Introduction</h2>
At each UMIL training iteration, divide the snippets into two sets using current detector: 1) the confident set with abnormal and normal snippets and 2) ambiguous set with rest snippets. The ambiguous set is grouped into two unsupervised clusters to discover the intrinsic difference between normal and abnormal snippets. Then, seek an invariant binary classifier between two sets that separate the abnormal/normal in the confident set and two clusters in the ambiguous one. The rationale of proposed invariance pursuit is that the snippets in the ambiguous set must have a different context bias from the confident set, otherwise they will be selected into the same set.
<h2>3. Related work</h2>
<h3>3.1 Unsupervised methods</h3>
Include the ones that only use unlabelled training data or directly train and test on testing data.
<h3>3.2 Weakly-supervised methods</h3>
Exploit both normal and abnormal training data with weak annotations only on the video-level.
<h2>4. Method</h2>
In WSVAD, each training video is annotated with a binary anomaly label $y \in$ {0, 1} and partitioned into $m$ snippets. Denote x$_i, i \in \{1,...,m\}$ as the feature of the $i$-th snippet in the video extracted by a backbone parameterized by $\theta$. The goal of WSVAD is to train a snippet-level anomaly classifier $f(\mathrm{x}_i)$ predicting the probability of the snippet being positive.
<h3>4.1 From MIL to unbiased MIL</h3>
In MIL, the backbone $\theta$ is pre-trained on Kinetics400 and frozen in training. It aims to learn $f$ so as to predict the most anomalous snippet in a normal video as normal and otherwise. Specifically, for each video, MIL creates a tuple containing the prediction of $f$ on the most anomalous snippet and video's anomaly label (max{$f(\mathrm{x}_i))^m_{i = 1}, y$). Then MIL aggregates tuple for all videos to construct a labeled confident snippet set $C$ and trains $f$ by minimizing BCE($C$) = $-\mathbb{E}_{(\hat{y}, y) \sim C}[y\log(\hat{y}) + (1 - y)\log(1 - \hat{y})]$, where $\hat{y} = \mathrm{max}\{f(\mathrm{x}_i)\}^m_{i = 1}$. For an abnormal video with $y$ = 1, by maximizing $\mathrm{max}\{f(\mathrm{x}_i)\}^m_{i = 1}$, $f$ is trained to output an even larger probability for most confident abnormal snippet. UMIL includes 4 steps. In step 1, divide snippets into a labeled confident snippet set $C$ and an unlabeled ambiguous snippet set $\mathcal{A}$. In step 2, cluster $\mathcal{A}$ into 2 groups in an unsupervised fashion to distinguish the normal and abnormal snippets. Finally, in step 3, $f$ is supervised by both $C$ and $\mathcal{A}$ to simultaneously predict the binary labels in $C$ and separate the clusters in $\mathcal{A}$.
<h3>4.2 Step 1: divide snippets</h3>
<h4>4.2.1 Constructing C</h4>
During training, track history of last 5 predictions from $f$ for each snippet. Then, at the start of every epoch, select $N$ snippets $\mathrm{x}_1,...,\mathrm{x}_n$ with the least prediction variance and confident set $C$ is given by {$f(\mathrm{x}_i), y_i$}$^N_{i = 1}$.
<h4>4.2.2 Constructing A</h4>
They are collected as the ambiguous set $\mathcal{A} = \{\mathrm{x}_i\}^M_{i = 1}$. Note that $\mathcal{A}$ is a set of features at this point, awaiting the next clustering step.
<h3>4.3 Step 2: clustering ambiguous snippets</h3>
Learn a cluster head $g$ that takes the snippet feature x $\in \mathcal{A}$ as input and outputs softmax-normalized probabilities for being in each of 2 clusters. The head $g$ is trained in a pair-wise manner such that a pair of similar features have similar predictions from $g$ and vice versa. Denote pair-wise form of $\mathcal{A}$ based on cluster prediction from $g$ as: $\mathcal{A}_g = \{g(\mathrm{x}_i)^Tg(\mathrm{x}_j), \mathbb{1}(\mathrm{x}_i \sim \mathrm{x}_j) | \mathrm{x}_i, \mathrm{x}_j \in \mathcal{A}\}$, where dot product is used to measure prediction similarity and $\mathbb{1}(\cdot)$ is an indicator function that returns 1 if cosine similarity between x$_i$, x$_j$ is larger than a threshold $\tau$ and returns 0 otherwise. With the optimized $g$, each feature x$_i$ in $\mathcal{A}$ is assigned a cluster label $y_i$ = argmax$g$(x$_i$) as the cluster with the highest predicted probability. 
<h3>4.4 Step 3: overall objective</h3>
Supervise $f$ with $\mathcal{A}$ using a pair-wise loss: $f$ is trained to produce similar anomaly prediction on feature pairs with same cluster label and push away predictions for those in different clusters. This corresponds to minimizing BCE($\mathcal{A}_f$) with $\mathcal{A}_f$: $\mathcal{A}_f = \{f(\mathrm{x}_i)^Tf(\mathrm{x}_j), \mathbb{1}(\mathrm{y}_i = \mathrm{y}_j) | \mathrm{x}_i, \mathrm{x}_j \in \mathcal{A}\}$, where $f(\mathrm{x}_i)^Tf(\mathrm{x}_j)$ denotes the dot-product similarity of binary probabilities with slight abuse of notation. The overall objective of UMIL is given by: min$_{\theta, f, g}$BCE($C$) + $\alpha$BCE($\mathcal{A_f}$) + $\beta$BCE($\mathcal{A}_g$).
<h4>4.4.1 Training and testing</h4>
Before training, backbone $\theta$ is first pre-trained with MIL and $f, g$ are randomly initialized. In testing, anomalies are labeled on frame level. The model is evaluated with a non-overlapping sliding window of frames to predict anomaly whenever window intersects with anomaly frame.
<h2>5. Datasets</h2>
UCF-Crime and TAD.
<h2>6. Metrics</h2>
AUC.
<h2>7. Code</h2>
https://github.com/ktr-hubrt/UMIL.