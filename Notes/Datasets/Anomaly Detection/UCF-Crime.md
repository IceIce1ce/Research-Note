<h2>1. Introduction</h2>
UCF-Crime is a large-scale dataset of 128 hours of videos. it consists of 1900 long and untrimmed real-world surveillance videos, with 13 realistic anomalies including Abuse, Arrest, Arson, Assault, Road Accident, Burglary, Explosion, Fighting, Robbery, Shooting, Stealing, Shoplifting and Vandalism. These anomalies are selected because they have a significant impact on public safety. This dataset can be used for two tasks. First, general anomaly detection considering all anomalies in one group and all normal activities in another group. Second, for recognizing each of 13 anomalous activities. 
<h2>2. Annotation</h2>
Only video-level labels are required for training. However, in order to evaluate its performance on testing videos, we need to know temporal annotations (start and ending frames of anomalous event in each testing anomalous video). Assign same videos to multiple annotators to label temporal extent of each anomaly. The final temporal annotations are obtained by averaging annotations of different annotators.
<h2>3. Summary</h2>
Training set: 800 normal and 810 anomalous videos.
Testing set: 150 normal and 140 anomalous videos.
Average of frames: 7247.
Both training and testing sets contain all 13 anomalies at various temporal locations in videos. Furthermore, some videos have multiple anomalies. 
<h2>4. Anomaly categories</h2>
Abuse: 50.
Arrest: 50.
Arson: 50.
Assault: 50.
Burglary: 100.
Explosion: 50.
Fighting: 50.
Road Accidents: 150.
Robbery: 150.
Shooting: 50.
Shoplifting: 50.
Stealing: 100.
Vandalism: 50.