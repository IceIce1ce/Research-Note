<h2>1. Definition</h2>
The goal of anomaly detection is to temporally or spatially localize anomaly events in video sequences. Temporal localization is referred to as frame-level detection. Spatial localization identifies pixels within each anomaly frame that correspond to anomaly event. It is also referred to as pixel-level detection.
<h2>2. Background</h2>
Early works mainly follow the setting of general anomaly detection which may be better referred to as novelty detection, where all novel events are considered as anomaly -> unsupervised learning, where models are trained with only normal video frames and validated with both normal and anomaly frames -> popular idea: sparse coding and auto encoder. Recent works start to leverage supervision for real-world anomaly detection. There is also a line of research focuses on specific anomaly detection tasks where only one type of anomaly is considered (traffic accident on highway).
<h2>3. Representative approaches</h2>
<h3>3.1 Unsupervised methods</h3>
They are not able to achieve satisfactory performance on complex real-world scenarios and are believed to have better generalization ability on unseen anomaly patterns.
<h4>3.1.1 Classic machine learning</h4>
Adopt classic machine learning techniques with hand-crafted features as well as probability models.
<h4>3.1.2 Deep learning</h4>
Based on deep AE and anomaly events are determined based on the reconstruction error. Another popular direction is to predict future frames based on past frames and assign high anomaly score when real future frame is highly different from predicted one.
<h3>3.2 Weakly supervised methods</h3>
Training videos are labeled with normal or anomalous. However, temporal location of anomaly event in each anomaly video is unknown (weak supervision).
<h3>3.3 Supervised methods</h3>
A popular solution is to leverage geometric prior knowledge and object detection with additional supervision from other public datasets.
<h2>4. Datasets</h2>
- UCSD: It contains two subsets: Ped1 and Ped2 which are captured with different camera poses at two spots in UCSD campus where most pedestrians walk. Training set (34 clips for Ped1 and 16 clips for Ped2) consists of both normal and anomaly frames. Frame-level annotation is provided for all test clips and 10 of them have pixel-level ground-truth. The dataset considers pedestrians walking as normal and non-pedestrian as anomaly instances.
- Subway: It contains two subsets: Subway Entrance and Subway Exit. They contain only one long surveillance video each in subway station.
- Avenue: It contains 15 videos and each video is about 2 minutes long. The total frame number is 35240. 8478 frames from 4 videos are used as training set. Typical unusual events include running and throwing objects.
- UMN: It consists of five videos captured from different angles. The normal pattern is defined as walking and main anomaly activity is running.
- DAD: Normal pattern is vehicles moving around and anomaly events include different traffic accidents. It consists of 678 videos from six cities, 58 videos are used for training. For the rest 620 videos, 620 clips with accidents are sampled as positive clips and 1130 normal clips are sampled as negative clips. They are then randomly split into 2 subsets (455 positive and 829 negative clips for training and 165 positive and 301 negative clips for testing).
- CADP: It focuses on car accident on CCTV. All 1416 videos of CADP contain traffic accidents and 205 of them have temporal as well as spatial annotations. CADP contains videos captured under various camera types, qualities, weather conditions and anomaly events are realistic for real-world applications.
- A3D: It consists of 1500 on-road abnormal event video clips from dashboard cameras. Each video contains an abnormal traffic event and anomaly start and end times are annotated by human annotators. A total of 128175 frames (ranging from 23 to 208 frames) at 10 frames per second are clustered into 18 types of traffic accidents.
- DADA: It is a traffic accident dataset collected for driver attention prediction in accidental scenarios. It has 658476 available frames contained in 2000 videos with resolution of 1584 $\times$ 660. The videos are divided into 54 kinds of categories, such as hitting and out of control, based on ther participants of accidents (pedestrian, vehicle, cyclist, etc). The spatial crash-objects temporal window of occurence of accidents are annotated.
- DoTA: it contains 4677 videos with temporal, spatial and categorical annotations. The video clips are collected from YouTube channels with diverse dash camera accident videos from different countries under different weather and lighting conditions. 
- Iowa DOT Traffic: It consists of 200 videos, each approximately 15 minutes in length, recorded at 30 fps and 800 $\times$ 410 resolution. Training and testing set each contains 100 videos. It does not provide annotation for testing set. Main anomaly patterns are car crashes and stalled vehicles.
- ShanghaiTech: It is collected in ShanghaiTech University under 13 scenes with complex ligh conditions and camera viewpoints. It consists of 437 videos with 726 average frames each. The training set consists of 330 normal videos and testing set contains 107 videos with 130 anomalies. Anomaly events include unusual patterns in campus such as bikers or cars.
- UCF Crime: It consists of 1900 untrimmed videos covering 13 real-world anomaly events. 950 of them are normal videos and the rest videos contain at least one anomaly event for each. The training set contains 800 normal and 810 anomalous videos. The remaining 150 normal and 140 anomalous videos are temporally annotated for validation. Some of the videos may contain multiple anomaly categories. Furthermore, UCF Crime covers different light conditions, image resolutions and camera poses in complex scenarios.
- Street Scene: It focuses on scene anomaly detection and consists 46 training videos and 35 testing videos taken from a static USB camera looking down on a scene of a two-land street with bike lanes and pedestrian sidewalks. There are a total of 203257 color video frames (56847 for training and 146410 for testing) with 1280 $\times$ 720 resolution. The frames were extracted from original videos at 15 frames per second. 17 types of anomaly events/activities are presented in the dataset such as jaywalking, loitering, car illegally parked.
<h2>5. Benchmarks</h2>
The frame-level evaluation criterion uses frame-level ground truth annotations to determine which detected frames are true positives and which are false positives. In pixel-level evaluation, it requires the algorithm to take into account the spatial locations of anomaly objects in frames. A detection is considered to be correct if it covers at least 40% anomaly pixels in the ground truth. The pixel-level evaluation can be conducted only if the pixel-level annotations are available for testing videos.
<h2>6. Metrics</h2>
AUC for unsupervised and weakly supervised setting. For traffic accident detection with supervised setting, F1 score, RMSE of anomaly start time and S3 score are used. S3 = F1(1 - NRMSE), where NRMSE denotes the normalized root mean square error.
<h2>7. Future research directions</h2>
- Learned representations are still not satisfactory for distinguishing complex anomaly activities. Possible better representations include better 3D feature extractor, attention mechanism and causal reasoning.
- It would be promising to explore better setting for practical applications, e.g. better trade-off between generalization ability (unsupervised setting) and performance (weakly supervised setting).
- It is worth exploring effective ways to de-identify the training videos and train anomaly models with de-identified data.
- Close the gap between performance and interpretable AI models.