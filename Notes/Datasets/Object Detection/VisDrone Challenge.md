<h2>1. File format</h2>
$\textbf{<bbox\_left>, <bbox\_top>, <bbox\_width>, <bbox\_height>, <score>, <object\_category>, <truncation>, <occlusion>}$. The object category includes 11 classes: ignored regions (0), pedestrian (1), people (2), bicycle (3), car (4), van (5), truck (6), tricycle (7), awning-tricycle (8), bus (9), motor (10), others (11). The truncation is -1, no truncation = 0 and partial truncation = 1. The occlusion is -1, no occlusion = 0, partial occlusion = 1 and heavy occlusion = 2.
<h2>2. Summary</h2>
- VisDrone2019-DET-train: 6471
- VisDrone2019-DET-val: 548
- VisDrone2019-DET-test-dev: 1610
- VisDrone2019-DET-test-challenge: 1580
<h2>3. Evaluation metrics</h2>
- AP: the average precision over all 10 IoU thresholds of all object categories.
- AP$^{IOU = 0.5}$: the average precision over all object categories when the IoU overlap with ground truth is larger than 0.5.
- AP$^{IOU = 0.75}$: the average precision over all object categories when the IoU overlap with ground truth is larger than 0.75.
- AR$^{max = 1}$: the maximum recall given 1 detection per image.
- AR$^{max = 10}$: the maximum recall given 10 detections per image.
- AR$^{max = 100}$: the maximum recall given 100 detections per image.
- AR$^{max = 500}$: the maximum recall given 500 detections per image.