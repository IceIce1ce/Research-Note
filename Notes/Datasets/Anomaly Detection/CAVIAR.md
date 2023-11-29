<h2>1. Introduction</h2>
It includes videos of two different situations. The sequences are ground truth labeled frame-by-frame with bounding boxes and a semantic description of activity in each frame. There are 28 video sequences grouped into 6 different activity scenarios. The sequences are from about 500 to 1400 frames in length, for a total of about 26500 frames. Each individual person was described by a bounding box (id, centre coordinates, width, height, orientation of main axis of individual) plus a description of his/her movement (inactive, active, walking, running). Individuals are only labeled once they start moving, otherwise they are effectively background. Each video frame contains zero or more labeled individual or group boxes. The boxes are labeled with an identifier, which persists as long as the individual is visible. If a person disappears and then later reappears, then the individual obtains a new identity. If the person is obscured/occluded for only a few frames, then the same identity is maintained. Similarly, groups of interacting individuals also are described by bounding boxes (id, centre, coordinates, width, height, orientation of main axis of individual, list of component individual boxes) plus a description of group's movement (inactive, active, moving). Based on the activity interpretation, each group box is usually labeled with a role (meeters, fighters, walkers). 
<h2>2. Summary</h2>
- Walking: 
	- Number of sequences: 3.
	- Number of frames: 3045.
- Browsing:
	- Number of sequences: 6.
	- Number of frames: 6665.
- Collapse:
	- Number of sequences: 4.
	- Number of frames: 4227.
- Leaving object:
	- Number of sequences: 5.
	- Number of frames: 5848.
- Meeting:
	- Number of sequences: 6.
	- Number of frames: 4135.
- Fighting:
	- Number of sequences: 4.
	- Number of frames: 2499.