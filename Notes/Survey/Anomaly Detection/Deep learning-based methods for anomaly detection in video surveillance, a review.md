<h2>1. Abstract</h2>
Review a family of video anomaly detection approaches based on deep learning techniques which are compared in terms of their algorithms and models. Moreover, group methods into different categories based on the approach adopted to differentiate between normal and abnormal events and the underlying assumptions. Furthermore, present publicly available datasets and evaluation metrics used in existing works. Finally, provide a comparison and discussion on the results of various approaches according to different datasets.
<h2>2. Introduction</h2>
Deep learning-based methods for anomaly detection use one of these techniques: reconstruction error to calculate test data divergence from a series of normal training videos, the future frame prediction, the classifiers or scoring methods.
<h2>3. Deep learning-based methods for anomaly detection</h2>
<h3>3.1 Reconstruction error based methods</h3>
Deep learning-based methods typically train a deep neural network using an auto-encoder method and use it to reconstruct normal events with few reconstruction errors. But larger reconstruction errors for anomalous events don't necessarily happen. As a result, it can show that practically many methods based on reconstruction of training data cannot guarantee detection of abnormal events.
<h3>3.2 Scoring based methods</h3>
The main idea of this approach is to generate an anomaly score that may be used to determine whether or not a video segment or frame is abnormal.
<h3>3.3 Future frame-based methods</h3>
The assumption of its use is that normal events are predictable whereas abnormal ones do not match expectations.
<h2>4. Benchmark datasets</h2>
Subway entrance, Subway exit, UMN, UCSD, Ped1, UCSD Ped 2, CUHK avenue, Street scene, ShanghaiTech, UCF-crime.
<h2>5. Evaluation metrics</h2>
- Frame level: a frame is deemed to have detection if it has at least one abnormal pixel. Each frame's ground truth annotation is compared to these detections. The process is carried out several times for different thresholds to create a ROC curve. This assessment does not confirm that the detection corresponds to the actual location of the anomaly. Therefore, some actual positive detections may be the result of fortunate co-occurrences of false positives and abnormal events.
- Pixel level: the accuracy of localization is evaluated by comparing detections to pixel-level ground truth masks, on a collection of ten clips. The frame is demmed as accurately detected if at least 40% of the actually anomalous pixels are found, otherwise it is a false positive.
- ROC curve, AUC and EER.
<h2>6. Comparison and discussion</h2>
Several studies choose to tackle anomaly detection problem using unsupervised learning methods, because do not require labeled video data and can be effectively employed for learning good representations. In addition, it is effective to the complexity and variety of visual behaviors of anomaly in an unconstrained environment. However, they still limited and did not achieve good results. Therefore, other researchers choose to surpass this limit by using semi-supervised learning methods that use data only related to normal class. Some limits: many anomaly detection algorithms work well with very regular scenes, so it is necessary to evaluate how well these methods operate in less structured situations. Moreover, the real time application in unconstrained environment and the time complexity.