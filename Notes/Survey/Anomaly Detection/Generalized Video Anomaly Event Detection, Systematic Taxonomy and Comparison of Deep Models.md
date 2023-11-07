<h2>1. Abstract</h2>
This review extends narrow VAED concept from unsupervised video anomaly detection to generalized video anomaly event detection which provides a comprehensive survey that integrates recent works based on different assumptions and learning frameworks into an intuitive taxonomy and coordinates unsupervised, weakly-supervised, fully-supervised and supervised VAED routes.
<h2>2. Datasets</h2>
Subway, UMN, UCSD, CUHK Avenue, ShanghaiTech, UCF-Crime, XD-Violence and UBnormal.
<h2>3. Unsupervised video anomaly detection</h2>
Divide the works into three main groups depending on the type of data. Frame-level methods use whole RGB and flow frames as input and aims to learn global normality. Patch-level methods extract features only from selected regions and ignore invalid information from repetitive regions as well as regional information interactions that do not require attention. The object-level methods consider connection between object and background and perform excellently in the task of detecting anomalous events in complex scenes.
<h3>3.1 Frame-level methods</h3>
Existing methods can be classified into 2 categories according to model structure: single-stream and multi-stream. The former does not distinguish spatial and temporal information. They usually take the original RGB videos as input and learn spatial-temporal patterns by reconstructing input sequence or predicting next frame. Multi-stream networks typically treat appearance and motion as different dimensions of information and attempt to learn spatial and temporal normality using different agent tasks or network architectures. 
<h3>3.2 Patch-level methods</h3>
It takes the video patch (spatial-temporal cube) as input. Compared with frame-level methods, patch-level methods consider finding anomalies from specific spatial-temporal regions rather than analyzing the whole sequence. Patch formation can be divided into three categories: scale equipartition, information equipartition, and foreground object extraction. In scale equipartition, the video sequence is equipartitioned into several spatial-temporal cubes of uniform size along the spatial and temporal dimensions. The subsequent modeling process is similar to frame-level methods. The information equipartition strategy considers that image blocks of same size do not contain same information. Regions close to camera contain less information per unit area than those far away. Before representation, all cubes will be first resized to the same size. The foreground object extraction focuses on modeling regions with information variation to avoid learning cost and disruption of the background. After the sequences are equated into same-scale cubes, those containing only background will be eliminated. 
<h3>3.3 Object-level methods</h3>
It uses a pre-trained object detector to extract objects of interest from video sequence before normality learning. Compared with frame-level and patch-level methods, object-level methods enable model to ignore redundant background information and focus on modeling behavioral interactions of foreground objects. In addition, object-level methods are also considered feasible to investigate scene-adaptive GVAED models.
<h2>4. Weakly-supervised abnormal event detection</h2>
WAED model only works when anomalies can be predefined and enough positive samples can be collected. It is classified into unimodal and multimodal according to input data modalities.
<h3>4.1 Unimodal models</h3>
The unimodal models are similar to UVAD in terms of input data, with the difference that the former computes the anomaly score directly while latter needs first to learn normality and detect anomalies in a downstream task. Specifically, the unimodal WAED model typically slices the unedited video into several fixed-size clips. They consider each clip as an instance and all clips from a video form a bag with same video-level label. And then, pre-trained feature extractors such as C3D, TSN or I3D is used to extract the spatial-temporal features of the examples. Generally, scoring module takes deep representations as input and calculates anomaly score for each instance with supervision of video-level labels.
<h3>4.2 Multimodal models</h3>
Multimodal GVAED aims to mine effective clues related to anomalies from various data such as video, audio and opticla flow. Due to the limitation of datasets, most existing works focused on video and audio information fusion to detect violent behaviors from surveillance videos. Recent work considered RGB frames and optical flow as different modalities.
<h2>5. Fully-unsupervised video anomaly detection</h2>
FVAD does not limit composition of training data and requires no data annotation. In other words, FVAD tries to learn an anomaly detector from random raw data which is a newly emerged technical route in recent years.
<h2>6. Supervised video anomaly detection</h2>
It requires frame-level or pixel-level labels to supervise models to distinguish between normal and anomalies. Therefore, it is often considered a classification task.
<h2>7. Metrics</h2>
AUC, FAR and EER.
<h2>8. Challenges and trends</h2>
<h3>8.1 Research challenges</h3>
Mock anomalies vs real anomalies, single-scene vs multi-scenes, real data vs synthetic data, unimodal vs multimodal, single-view vs multi-view, off-line vs on-line detection.
<h3>8.2 Development trends</h3>
Data level, representation level, deployment level, methodology level.
<h2>9. Code</h2>
https://github.com/fudanyliu/GVAED.