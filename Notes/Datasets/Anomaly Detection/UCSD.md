<h2>1. Introduction</h2>
UCSD dataset contains two parts: Ped1 and Ped2. Ped1 includes 34 trainng videos and 36 testing ones with 40 irregular events. All of these abnormal cases are about vehicles such as bicycles and cars. Ped2 dataset contains 16 training videos and 12 testing videos with 12 abnormal events. This dataset was acquired with a stationary camera mounted at an elevation, overlooking pedestrian walkways. The crowd density in the walkways was variable, ranging from sparse to very crowded. In the normal setting, the video contains only pedestrians. Abnormal events are due to either 1) the circulation of non pedestrian entities in the walkways or 2) anomalous pedestrian motion patterns. Commonly occurring anomalies include bikers, skaters, small carts and people walking across a walkway or in the grass that surrounds it. A few instances of people in wheelchair were also recorded. All abnormalities are naturally occurring. The first subset of dataset contains groups of people walking towards and away from the camera and some amount of perspective distortion. The second subset contains scenes with pedestrian movement parallel to the camera plane. The video footage recorded from each scene was split into various clips of around 200 frames. For each clip, the groundtruth annotation includes a binary flag per frame, indicating whether an anomaly is present in that frame. In addition, a subset of 10 clips is provided with manually generated pixel-level binary masks, which identify the regions containing anomalies.
<h2>2. Summary</h2>
Ped1:
- Total frames: 14000.
- Training frames: 6800.
- Testing frames: 7200.
- Regularity frames: 9995. 
- Irregularity frames: 4005.
- Abnormal events: 40.
- Scenes: 1.
Ped2:
- Total frames: 4560.
- Training frames: 2550.
- Testing frames: 2010.
- Regularity frames: 2924.
- Irregularity frames: 1636. 
- Abnormal events: 12.
- Scenes: 1.