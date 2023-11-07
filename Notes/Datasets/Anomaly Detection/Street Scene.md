<h2>1. Introduction</h2>
Street Scene consists of 46 training video sequences and 35 testing video sequences taken from a static USB camera looking down on a scene of a two-lane street with bike lanes and pedestrian sidewalks. Videos were collected from the camera at various times during two consecutive summers. All of the videos were taken during the daytime. The dataset is challenging because of the variety of activity taking place such as cars driving, turning, stopping and parking, pedestrians walking, jogging and pushing strollers and bikers riding in bike lanes. In addition the videos contain changing shadows and moving background such as a flag and trees blowing in the wind. There are a total of 203257 color video frames (56847 for training and 146410 for testing) each of size 1280 $\times$ 720 pixels. The frames were extracted from the original videos at 15 frames per second. The 35 testing sequences have a total of 205 anomalous events consisting of 17 different anomaly types. Ground truth annotations are provided for each testing video in the form of bounding boxes around each anomalous event in each frame. Each bounding box is also labeled with a track number, meaning each anomalous event is labeled as a track of bounding boxes. A single frame can have more than one anomaly labeled.
<h2>2. Summary</h2>
- Total frames: 203257.
- Training frames: 56847.
- Testing frames: 146410.
- Anomalous events: 205.
- Anomaly types: 17. 
- Ground truth: spatial, temporal.
- Resolution: 1280 $\times$ 720.
<h2>3. Anomaly categories</h2>
1. Jaywalking: 61.
2. Biker outside lane: 42.
3. Loitering: 36.
4. Dog on sidewalk: 11.
5. Car outside lane: 10.
6. Worker in bushes: 8.
7. Biker on sidewalk: 7.
8. Pedestrian reverses direction: 6.
9. Car u-turn: 5.
10. Car illegally parked: 5.
11. Person opening trunk: 4.
12. Person exists car on street: 3.
13. Skateboarder in bike lane: 2.
14. Person sitting on bench: 2.
15. Metermaid ticketing car: 1.
16. Car turning from parking space: 1.
17. Motorcycle drives onto sidewalk: 1.