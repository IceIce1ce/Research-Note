<h2>1. Abstract</h2>
This survey summarizes research trends on anomaly detection in a single scene. Discuss the various problem formulations, publicly available datasets and evaluation criteria. Categorize and situate past research into an intuitive taxonomy and provide a comprehensive comparison of the accuracy of many algorithms on standard test sets. Finally, provide best practices and suggest some possible directions for future research.
<h2>2. Introduction</h2>
This survey focuses on single-scene video anomaly detection because it is the most common use case for video anomaly detection in real-world applications. One important difference is that the single-scene anomaly detection formulation can contain location-dependent anomalies whereas multi-scene cannot. A location-dependent anomaly is an object or activity that is anomalous in some regions of a scene but not in other regions.
<h2>3. Types of video anomalies</h2>
- Appearance-only anomalies: the appearance of unusual objects in a scene.
- Short-term motion-only anomalies: the motion of unusual objects in a scene.
- Long-term trajectory anomalies: the trajectory of unusual object in a scene.
- Group anomalies: the interaction of unusual objects in a scene.
- Time-of-Day anomalies: it is orthogonal to all of the other types.
<h2>4. Overview of single-scene video anomaly detection</h2>
<h3>4.1 Datasets</h3>
Subway, UMN, UCSD, CUHK Avenue, Street Scene, ShanghaiTech and UCF-Crime.
<h3>4.2 Evaluation protocol</h3>
TPR, FPR, Dual Pixel level, Region-based and Track-based.
<h3>4.3 A taxonomy of video anomaly detection approaches</h3>
- Representation: hand-crafted features (spatio-temporal gradients, dynamic textures, histogram of gradients, histogam of flows, flow fields, dense trajectories and foreground masks) and deep learning features.
- Modeling: one-class SVMs, nearest neighbor approaches and HMMs.
- Object detection and tracking approach: creating object trajectories.
- Supervised anomaly detection.
- Video-level weak supervision.
<h2>5. Distance-based approaches</h2>
Using the training data to create a model of normality and measuring deviations from this model to determine anomaly scores. One common approach is to use one-class SVMs. A disadvantage: it is expensive to update the model given new normal training data. An alternative approach is to use a mixture of gaussians to model normal feature vectors and then Mahalanobis distance to measure distance to normality.
<h2>6. Probabilistic approaches</h2>
It aims to admit modeling into a probabilistic framework such as with probabilistic graphical models or high-dimensional mixtures of probability distributions. It coupled with Markov Random Fields and mixtures of Gaussians.
<h2>7. Reconstruction-based approaches</h2>
It aims to represent the input using a high-level or compact representation learned from normal video and then reconstruct the input using only this representation. The assumption is that anomaly inputs are harder to reconstruct using a representation learned from normal videos. Most methods are based on either convolutional auto-encoders or GANs. Disadvantage: auto-encoder or GAN need to be retrained to accomodate new normal training video.
<h3>7.1 Sparse reconstruction approaches</h3>
It imposes an additional constraint on the reconstruction in that it must be performed using a sparse feature set only. Most methods are fast and normality are easy to update in an online fashion. Disadvantage: rely too heavily on memorizing salient normal features, placing a large burden on the normal training set being exhaustive.
<h2>8. Best practices going forward</h2>
Urge researchers in this area to use reliable datasets, new evaluation protocol and participate in reproducible research. A qualitative evaluation of quality of false positives is also important, especially to discover biases in modeling.
<h2>9. Looking ahead</h2>
In existing datasets, loitering anomalies have not been addressed in specific by modeling since they rely heavily on motion cues to ignore processing parts of video. Another challenge for video anomaly detection methods is the ability to handle rare but normal activity. Group, trajectory and time of day anomalies have been unaddressed because benchmark datasets do not exists.