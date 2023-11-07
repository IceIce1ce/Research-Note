<h2>1. Abstract</h2>
Present a comprehensive study of deep learning-based methods reported in state-of-the-art to detect video anomalies. Further, discuss the comparative analysis of state-of-the-art methods in terms of datasets, computational infrastructure and performance metrics for both quantitative and qualitative analysis. Finally, outline the challenges and promising directions for further research.
<h2>2. Classification of video anomalies</h2>
<h3>2.1 Local and global anomalies</h3>
Local anomaly significantly deviates from its neighboring spatiotemporal activities. Global anomaly is referred to activities that interact with each other globally in an abnormal, suspicious or unusual manner, even if individual activities may be normal or anomalous in isolation.
<h3>2.2 Contextual or conditional anomalies</h3>
Time and space are considered as contextual features and features used to describe normal behaviors are regarded as behavioral features. Further, contextual anomalies may be classified as spatial and temporal anomalies.
<h2>3. Training and learning frameworks</h2>
Supervised, unsupervised, semi-supervised and active learning.
<h2>4. Approaches for video anomaly detection</h2>
<h3>4.1 Accuracy-oriented approaches</h3>
Some of the important research works are based on generative models, temporal regularity model, spatio-temporal/ predictive models, hybrid models and models using detection of STIPs.
<h3>4.2 Processing-time-oriented approaches</h3>
Few low-level lightweight feature descriptors such as binary and binary pair-based video descriptors are used for video anomaly detection with online performances.
<h2>5. Deep-learning based methods for the video anomaly detection</h2>
<h3>5.1 Trajectory-based methods</h3>
Few important reported works for anomalous activity detection using trajectory analysis based on OC-SVM clustering, snapped trajectory, semantic scene learning as well as tracking, string kernel-based clustering, HMM, deep learning based classifiers, spatio-temporal path search, YOLOv2, SORT, YOLOv3, Deep-SORT for loitering detection. The effectiveness of trajectory-based methods depends on object detection as well as tracking. The trajectory methods as found to be suitable for sparsely crowded environment not for moderately or densely crowded environment. Further, these methods lack contextual information while deciding the abnormality.
<h3>5.2 Global pattern-based methods</h3>
Features such as spatial temporal gradients, kinetic energy, optical flow are widely used in these methods. These methods are helpful in the detection of video anomaly. However, the localization of anomalies using global pattern-based methods is a tedious job.
<h3>5.3 Grid pattern-based methods</h3>
They are used to reduce the processing times by limiting the number of extracted feratures from fixed spatiotemporal regions. 
<h3>5.4 Representation learning models</h3>
The process of extracting useful features or learning good representations of input video data by taking into account crucial prior information of the particular problem is known as representation learning. Further, representation learning is helpful in extracting an effective video descriptor or representation that are generic, compact, efficient and simple. Important methods of video anomaly detection based on representation learning are: sparse coding inspired deep neural networks, reconstruction models and slow feature analysis.
<h3>5.5 Discriminative models</h3>
It try to learn the discriminant features existing among classes. It not widely used for video anomaly detection due to lack of balanced video anomaly datasets and clarity in the definition of anomalous activities as well as anomalous entities.
<h3>5.6 Predictive models</h3>
The objective is to model conditional distribution $P(X_t/(X_{t - 1},...,X_{t - p}))$ for predicting the current frame from the past frame. The predictive models or spatio-temporal models are widely used for video anomaly detection.
<h3>5.7 Deep generative models</h3>
The objective is to learn the joint probability $P(X, Y)$ and subsequently calculate the conditional posterior probability $P(X/y)$.
<h3>5.8 Deep one-class classification models</h3>
OCC, unary classification problem, Deep OCC models.
<h3>5.9 Deep hybrid models</h3>
The hybrid models are comprising multiple models such a way that the overall performance of the end-to-end pipeline for video anomaly detection increases significantly. Here, representation features extracted by DNN-based models are provided as inputs to SVM or RBF.
<h2>6. Datasets</h2>
UCSD Pedestrian, UMN, CUHK Avenue, Subway, PETS 2009, Anomalous Behavior/ York, QMUL Junction, MIT Traffic, Violent flows, Weizmann, CAVIAR, BOSS, i-Lids challenge for detection of bags and vehicles, U-turn, Web, crowd, BEHAVE LV and ShanghaiTech.
<h2>7. Evaluation criteria</h2>
Frame-level, pixel-level, dual-pixel, level, object-level, track-level, region-level.
<h2>8. Performance metrics for quantitative analysis</h2>
Error matrix, ROC, Precision-recall, EER, detection rate, reconstruction error, anomaly score, regularity score and PSNR. 
<h2>9. Research challenges in deep learning approaches for the video anomaly detection</h2>
Need a better datasets, reduction in computational complexity, solving incompleteness of the methodology, finding best evaluation methodologies, need for co-design of hardware and software, trade-off between accuracy and processing time, need to address the environmental challenges.