<h2>1. Abstract</h2>
Present the first comprehensive survey of WSAD methods by categorizing them into the above three weak supervision settings across four data modalities (tabular, graph, time-series and image/video data). For each setting, provide formal definitions, key algorithms and potential future directions.
<h2>2. Introduction</h2>
- AD with incomplete supervision: concerns the situation where only part of input data is labeled.
- AD with inexact supervision: happens when the detected labels are not as exact as they need to be.
- AD with inaccurate supervision: refers to the problem where labels are corrupted with noise which may be caused by machine mistakes and annotation errors.
<h2>3. AD with incomplete supervision</h2>
Given anomaly detection task with input data X $\in \mathbb{R}^{n \times d}$ (n samples and d features) and partial labels $y_p \in \mathbb{R}^p$ where p < n, train a detection model M with {X, $y_p$}. Output the anomaly scores on test data $X_{test} \in \mathbb{R}^{m \times d}$, i.e., $O_{test} := M(X_{test}) \in \mathbb{R}^{m \times 1}$. Key algorithms: feature representation learning, anomaly score learning, graph learning, active and reinforcement learning. Open questions and future directions: efficient label acquirement, few-shot/meta learning, balancing weights of non-labeled and labeled data, hyperparameter tuning and model selection.
<h2>4. AD with inexact supervision</h2>
Given an anomaly detection task with input data X $\in \mathbb{R}^{n \times d}$ that can be further grouped into k groups, i.e., {$X^1_G,...,X^k_G$}. With the coarse-grained labels at the group level (i.e., $y_G \in \mathbb{R}^k$ where k $\leq$ n), train a detection model M with {X, $y_G$}, Output the sample level anomaly scores on the test data $X_{test} \in \mathbb{R}^{m \times d}$, i.e., $O_{test} := M(X_{test}) \in \mathbb{R}^{m \times 1}$. Key algorithms: MIL-based, non-MIL based, evaluation metrics & model selection. Open questions and future directions: call for research on other modalities with inexact supervision, cross-modality methodology adaption.
<h2>5. AD with inaccurate supervision</h2>
Given an anomaly detection task with input data X $\in \mathbb{R}^{n \times d}$ and noisy/inaccurate labels $y_w \in \mathbb{R}^n$, train a detection model M with {X, $y_w$}. Output the anomaly scores on the test data $X_{test} \in \mathbb{R}^{m \times d}$, i.e., $O_{test} := M(X_{test}) \in \mathbb{R}^{m \times 1}$. Key algorithms: ensemble learning, denoising network, graph learning. Open questions and future directions: call for research on AD with inaccurate supervision, leveraging self-supervised learning in denoising labels.
<h2>6. Discussions and concluding remarks</h2>
Firstly, these three weak supervision problems often occur simultaneously. For example, inexact labels can also be inaccurate or noisy - new techniques for joint optimization can be useful. Second, different types of weak supervision can be convertible. For example, inexact supervision can be regarded as a special case of inaccurate supervision at the detection level. Also, incomplete supervision may be viewed as inaccurate supervision, with the unlabeled samples being noisy. Thus, one may adapt existing algorithms from one setting to another by considering different formulations.
<h2>7. Code</h2>
https://github.com/yzhao062/WSAD.