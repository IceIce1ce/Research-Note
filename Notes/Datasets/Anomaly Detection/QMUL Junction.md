<h2>1. Introduction</h2>
The dataset features a public road junction controlled by traffic lights and dominated by four traffic flows. The length of the video is approximately 60 minutes (89999 frames) captured with 360 $\times$ 288 frame size at 25 fps. The first 10000 frames of the video is are used for training and the rest are used for testing. Activity modelling and anomaly detection in this scene is challenging due to: 1) complex interactions among vehicles and pedestrians, 2) changing complexity of activity patterns caused by changing traffic volume over time, 3) noisy observations caused by illumination change, shadows and video capturing noise. Ground truth was extracted from the testing set based on manual labelling, which includes atypical activities such as traffic interruptions by emergency vehicles, illegal u-turn, jaywalking, etc. The average duration of anomalies is 4.3 seconds with the shortest interval being 2.64 secs (66 frames) and the longest interval being 8.8 secs (220 frames). Some of the anomalies occurred across several regions.
<h2>2. Summary</h2>
- An ambulance entered the junction using an improper lane of traffic: 88.
- A police vehicle entered the junction using an improper lane of traffic: 95.
- A fire engine entered the junction from the left entrance and caused interruptions to the vertical traffic at both directions: 180.
- A fire engine entered the junction from the right entrance and caused interruptions to the left-right turn traffic: 132.
- Strange driving behavior of a left-turning car: 93.
- A police vehicle entered the junction using an improper lane of traffic: 150.
- Jaywalking: 105.
- Illegal u-turn: 104.