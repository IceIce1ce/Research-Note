<h2>1. Introduction</h2>
The UBnormal benchmark is generated using Cinema4D which allows us to create scenes using 2D background images and 3D animations. Select a total of 29 natural images that represent street scenes, train stations, office rooms, among others. People, cars or other objects that belong to the foreground were eliminiated. From each natural image, create a virtual 3D scene and generate average 19 videos per scene. For each scene, generate both normal and abnormal videos. The proportion of normal versus abnormal videos is close to 1:1. The anomalies are annotated at the pixel level. For each synthetic object, provide the segmentation mask and object label. All videos are 30 FPS with the minimum height of a frame set to 720 pixels.
<h2>2. Summary</h2>
- Total frames: 236902.
- Training frames: 116087.
- Validation frames: 28175.
- Testing frames: 92640.
- Normal frames: 147887.
- Abnormal frames: 89015.
- Anomalies: 600.
- Scenes: 29.
- Anomaly types: 22.
<h2>3. Anomaly categories</h2>
The following events are considered as normal: walking, talking on the phone, walking while texting, standing, sitting, yelling and talking with others. 22 abnormal event types: running, falling, fighting, sleeping, crawling, having a seizure, laying down, dancing, stealing, rotating 360 degrees, shuffling, walking injured, walking drunk, stumbling walk, people and car accident, car crash, running injured, fire, smoke, jaywalking, driving outside lane and jumping. The test set includes the following anomalies: running, having a seizure, laying down, shuffling, walking drunk, people and car accident, car crash, jumping, fire, smoke, jaywalking and driving outside land. The following anomalies are included in training set: falling, dancing, walking injured, running injured, crawling and stumbling walk. The rest of anomalies are added to validation set. In order to increase the variety of dataset, include multiple object categories such as people, cars, skateboards, bicycles and motorcycles. Moreover, include foggy scenes, night scenes, fire and smoke as abnormal events.