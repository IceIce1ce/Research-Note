<h2>1. Abstract</h2>
We present a survey on relevant visual surveillance related researches for anomaly detection in public places, focusing primarily on roads. Firstly, we revisit the surveys done in the last 10 years in this field. Since the underlying building block of a typical anomaly detection is learning, we emphasize more on learning methods applied on video scenes. We then summarize the important contributions made during last six years on anomaly detection primarily focusing on features, underlying techniques, applied scenarios and types of anomalies using single static camera. Finally, we discuss the challenges in the computer vision related anomaly detection techniques and some of the important future possibilities.
<h2>2. Computer vision guided anomaly detection studies</h2>
<h3>2.1 Background and terminologies</h3>
<h4>2.1.1 Anomaly classification</h4>
Anomalies are classified as point anomalies, contextual anomalies and collective anomalies. In the context of visual surveillance, it is common to see anomalies classified as local and global anomalies. Global anomalies can be present in a frame or a segment of the video without specifying where exactly it has happened. Local anomalies usually happen within in a specific area of the scene, but may be missed by global anomaly detection algorithms.
<h4>2.1.2 Challenges and scope of study</h4>
(i) defining a representative normal region, (ii) boundaries between the normal and anomalous regions may not be crisp or well defined, (iii) the notion of anomaly is not same in all application contexts, (iv) limited availability of data for training and validation, (v) data is often noisy due to inaccurate sensing, and (vi) normal behavior evolves over time.
<h3>2.2 Learning methods</h3>
It can be classified as supervised, unsupervised or semi-supervised.
<h3>2.3 Anomaly detection approaches</h3>
<h4>2.3.1 Model-based</h4>
Model-based approaches learn the normal behavior of data by representing them in terms of a set of parameters. Statistical approaches are used in general to learn the parameters of the model as they try to fit the data into a stochastic model. Statistical approaches may be either parametric or non-parametric. Parametric methods assume that the normal data is generated through parametric distribution and probability density function. In nonparametric statistical models, the structure is not defined apriori, instead determined dynamically from the data.
<h4>2.3.2 Proximity-based</h4>
anomalies are decided by how close they are to their neighbors. In distance-based approaches, the assumption is that normal data have dense neighborhood. Density-based approaches compare the density around a point with the density around its local neighbors. The relative density of a point compared to its neighbors is computed as an outlier score.
<h4>2.3.3 Classification-based</h4>
It assumes that a classifier can distinguish between normal and anomalous classes in a given feature space. Class-based anomaly detection techniques can be divided into two categories: one class and multi-class.
<h4>2.3.4 Prediction-based</h4>
It detects anomaly by calculating the variation between predicted and actual spatio-temporal characteristics of the feature descriptor.
<h4>2.3.5 Reconstruction-based</h4>
The assumption is normal data can be embedded into a lower dimensional subspace in which normal instances and anomalies appear differently. Anomaly is measured based on the data reconstruction error.
<h4>2.3.6 Other approaches</h4>
There are two types of clustering approaches. One relies on an assumption that the normal data lie in a cluster, while anomaly data do not get associated with any cluster. The later type is based on an assumption that normal data instances belongs to big and dense clusters, while anomalies either belong to little/small clusters.
<h3>2.4 Applied areas</h3>
QMUL, CAVIAR, UCSD, Bellview, Person, UMN, ARENA, Avenue, Uturn, MIT Trajectory, MIT, MIT parking trajectory, NGSIM, AIRS, PETS2009, Behave, i-LIDS, ShanghaiTech, NVIDIA CITY, BOSS, Car Accident and 1diap.
<h2>3. Critical analysis</h2>
Four key issues: (i) the methods need to be relevant for real-life scenarios and should be applicable to long duration videos, (ii) Secondly, due to the aforementioned trend, very limited amount of research have been carried out for developing generic techniques applicable to a variety of datasets. (iii) There has been hardly any illumination independent research except for accident type anomaly detection, and (iv) Some approaches remove the background and focus on foreground features for anomaly detections.
<h2>4. Challenges and possibilities</h2>
Illumination, pose and perspective, heterogeneous object handling, sparse vs dense, curtailed tracks and lack of real-life datasets.