<h2>1. Annotation format</h2>
{
video_id: {
		"video_start": int,
		"video_end": int,
		"anomaly_start": int,
		"anomaly_end": int,
		"anomaly_class": str,
		"num_frames": int,
		"subset": "train" or "test"
}
}
<h2>2. Introduction</h2>
Contain 4677 videos (10 FPS) with temporal, spatial and categorical annotations. The largest traffic anomaly dataset to-data and is the first supporting traffic anomaly studies across when-where-what perspectives. DoTA is built by collecting more than 6000 video clips from YouTube channels and selecting diverse dash camera accident videos from different countries under different weather and lighting conditions. Avoid videos with accidents that were not visible or camera fall-off from wind shield, resulting in 4677 videos with 1280 $\times$ 720 resolution. Through the original videos are at 30 fps, extract frames at 10 fps for annotations and experiments.
<h2>3. Anomaly categories</h2>
- ST: Collision with another vehicle which starts, stops or is stationary.
- AH: Collision with another vehicle moving ahead or waiting.
- LA: Collision with another vehicle moving laterally in the same direction.
- OC: Collision with another oncoming vehicle.
- TC: Collision with another vehicle which turns into or crosses a road.
- VP: Collision between vehicle and pedestrian.
- VO: Collision with an obstacle in the roadway.
- OO: Out-of-control and leaving the roadway to the left or right.
- UK: Unknown.