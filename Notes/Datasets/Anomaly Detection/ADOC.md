<h2>1. Introduction</h2>
ADOC includes 25 event types, spanning over 721 instances and occurring over a period of 24 hours. This is the largest dataset with localized bounding box annotations that is available to perform anomaly detection. The camera captures video at a resolution of 1080p and a frame-rate of 3 FPS. The video is compressed using H.264 format which is a lossy compression method. The video encapsulates varying illumination conditions, crowded scenarios with background clutter. The data is annotated with a number of events ranging from low to high frequency. The annotations are performed using computer vision annotation tool (CVAT) and are provided in MOT format. Every event is localized using a tight bounding box, a frame id and contains a unique association ID to from trajectories.
<h2>2. Summary</h2>
- Low frequency events:
	- Trajectories: 721.
	- Manually annotated bounding boxes: 13290.
	- Interpolated bounding boxes: 270835.
	- Total bounding boxes: 284125.
- High frequency events:
	- Automatically annotated bounding boxes: 5082993.
	- Estimated number of persons walking: 31675.
- Frame Count:
	- Frames with low frequency events: 97030.
	- Frames with events (low and high): 142962.
	- Total frames: 259123.
<h2>3. Event types</h2>
Riding a bike, walking on grass, driving golfcart, walking with suitcase, having a conversation, riding a skateboard, birds flying, walking with a bike, pushing a cart, person vending, standing on walkway, bending, walking a dog, riding a mobility scooter, running, group of people, cat/dog, crowd gathering, holding a sign, walking with balloons, bag left behind, truck on walkway, person on knee scooter, camera overexposure, person smoking.