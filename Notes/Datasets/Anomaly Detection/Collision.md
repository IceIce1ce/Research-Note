<h2>1. Introduction</h2>
The collision dataset comprised of real-world driving videos. The dataset contains rare collision events from diverse driving scenes, including urban and highway areas in several large cities in the US. These events encompass collision scenarios (i.e., scenarios involving the contact of the dashcam vehicle with a fixed or moving object) and the near-collision scenarios (i.e., scenarios requiring an evasive maneuver to avoid a crash). Such driving scenarios most often contain interactions between two vehicles, or between a vehicle and a bike or pedestrian. The data was collected from a largescale deployment of connected dashcams. Each vehicle is equipped with a dashacam and a companion smartphone app that continuously captures and uploads sensor data such as IMU and gyroscope readings. Overall, the vehicles collected more than 10 million rides, and rare collision events were automatically detected using a triggering algorithm based on the IMU and gyroscope sensor data. The events were then manually validated by human annotators based on visual inspection to identify edge case events of collisions and near-collisions, as well as non-risky driving events. Of all the detected triggers, our subset contains 743 collisions and 60 near-collisions from different drivers. Each video clip contains one such event typically occurring in the middle of the video clip. The clip duration is approximately 40 seconds on average and the frame resolution is 1280 $\times$ 720.
<h2>2. Summary</h2>
- Party type:
	- Vehicle: 85%.
	- Bike: 6%.
	- Pedestrian: 6%.
	- Road object: 1%.
	- Motorcycle: 1%.
- Weather:
	- Clear: 93%.
	- Rain: 5.3%.
	- Snow: 1.7%.
- Lighting:
	- Day: 62%.
	- Night: 38%.