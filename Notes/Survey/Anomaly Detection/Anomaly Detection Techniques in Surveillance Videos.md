<h2>1. Abstract</h2>
This article extensively explain the current improvement made toward video-based human abnormal activity detection and wide applications in various fields range from monitoring in public spaces to personal health rehabilitation.
<h2>2. Classification-based anomaly detection techniques</h2>
It can be generally divided into two prominent categories: one class vs multi-class anomaly detection techniques.
- Support vector machines-based techniques.
- Neural networks-based techniques: for multi-class, it operates in two steps. First, to use the normal training data to train a neural network to learn different normal classes. Second, put each test instance into this neural network, then can judge the acceptability of the network, the one accepted is regarded as normal and vice-versa. Some types of neural network techniques to detect abnormal events: replicator neural networks, multi layered perceptron, auto associative networks, hopfield networks, oscillatory networks.
- Nearest neighbor-based techniques: the most important idea is to calculate the distance or similarity measure between two data instances. There are different ways to compute it such as Euclidean distance for the continuous attributes and simple matching coefficient is mostly used for categorical attributes. The distance or similarity of each attribute can be combined for multivariate data instances. 
<h3>2.1 Advantages</h3>
- Classification-based techniques, especially multi-class techniques can take advantage of robust algorithms to distinguish kinds of instances under different classes.
- Classification-based techniques has fast speed and high accuracy in detecting many categories of known abnormal phenomena in testing phase.
<h3>2.2 Disadvantages</h3>
- Require both accurate labels from normal and anomaly classes, but this is often not possible.
- Potential high false alarm rate - previously legitimate (or unseen) scenarios have the potential to be recognized as anomalies.
<h2>3. Clustering-based detection techniques</h2>
Some limitations: most clustering approaches evaluate a video event as the motion trajectory of one single object, which often cause neglect of important information like spatial and temporal context. For one thing, video anomaly may not correspond to the whole trajectory, only to a part of it. For another thing, an anomaly can arise due to the inappropriate interactions among multiple objects in spite of their individual behaviors are normal. Thus, trajectory clustering-based anomaly methods can cause miss detections.
<h2>4. Other uncommon detection techniques</h2>
Spectral anomaly detection techniques determine if anomaly can be easily identified in such sub-spaces (embedding, projections, etc.). Advantage: it can operate in an unsupervised mode. Disadvantage: It based on the assumption that anomalies and normal instances are distinguishable in the reduced space, so have to project data into a lower dimensional space by the means of PCA.
<h2>5. Future research directions</h2>
- Make these methods more robust to low quality videos, such as occlusion or shaking, faster in dealing with high resolution surveillance videos.
- Develop frameworks that will effectively cope with scenarios of extendibility of video analysis especially in the cluttered real world where environments have many moving objects and activities.
- Develop more advanced deep learning techniques to probe into the hierarchical and temporal relationships for better behavior localization and detection.