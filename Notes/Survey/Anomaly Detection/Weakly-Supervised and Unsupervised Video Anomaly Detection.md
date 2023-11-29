<h2>1. Abstract</h2>
This article discusses representative unsupervised and weakly-supervised learning methods applied to various data types. In these machine learning methods, GAN, AE, RNN... are broadly adopted for AD. Some reowned and new datasets are reviewed. Furthermore, propose several future directions of research in video anomaly detection.
<h2>2. Introduction</h2>
Deep weakly supervised anomaly detection (WAD) and semi-supervised anomaly detection (SAD) are proposed to detect anomalies on limited labeled data. WAD aims at building the anomaly detection model with data that is partially or incorrectly labeled. SAD aims at working on limited labeled data.
<h2>3. Image data based methods</h2>
<h3>3.1 Unsupervised methods</h3>
AnoGAN, EBGAN, $E^3$ Outlier, MOCCA, SmithNet and CutPaste.
<h3>3.2 Semi-supervised methods</h3>
FenceGAN and OCGAN.
<h2>4. Video data based methods</h2>
<h3>4.1 Weakly supervised learning</h3>
RTFM, MIST, TMSL and WETAS.
<h3>4.2 Unsupervised learning</h3>
HF$^2$-VAD, SSAGAN and ROADMAP.
<h2>5. Real-world datasets</h2>
UCSD Ped 1/2, UMN, CUHK, UCF Crime and OOPS.
<h2>6. Future directions</h2>
<h3>6.1 Better and complex datasets</h3>
UMN has a very limited complexity because the objects in it are only people. The videos in UCSD Ped1 and Ped2 are captured from fixed surveillance cameras and the resolutions are low. There's a need to obtain more complicated datasets like OOPS which is much closer to real-world scenarios.
<h3>6.2 Optimization for computation</h3>
A significant amount of computation on video data must be made to better utilize it.
<h3>6.3 Adaptive methods</h3>
The instances which are previously recognized as anomalies may not be anomalous any more. Therefore, there is a need to find methods to adapt the models to the new definition of anomaly.
<h3>6.4 Loitering anomalies</h3>
Many existing methods based on video data take the motion features into account. However, some anomalies in the real world do not have discriminative motion features compared with normal events. Models may fail to detect this kind of anomaly if no special treatments are done.