<h2>1. Introduction</h2>
More than 24 hours of 800 $\times$ 410 resolution data at 30 FPS captured by the Iowa Department of Transportation (DOT) traffic cameras. It contains 100 videos, each approximate 15 minutes in length. An additional sample set of four videos will include anomaly annotations. The anomaly can be due to car crashes or stalled vehicles. Regular congestion not caused by any traffic incident does not count as an anomaly.
<h2>2. Submission format</h2>
$<video\_id><timestamp><confidence>$
- $<video\_id>$ is the video numeric identifier, starting with 1. It represents the position of video in the list of all track videos, sorted in alphanumeric order.
- $<timestamp>$ is the relative time, in seconds, from the start of the video.
- $<confidence>$ denotes the confidence of the prediction. Should be between 0 and 1.
<h2>3. Evaluation metric</h2>
It is measured by the F1-score and detection time error, measured by RMSE. S2 = F1 * (1 - NRMSE). A multi-car event (one crash followed by another crash, or a stalled car followed by someone else stopping to help) is considered a single anomaly. In particular, if a second event happens within 2 minutes of the first, it should be counted the same anomaly as the first. Compute the detection time error as RMSE of the ground truth anomaly time and predicted anomaly time for all TP predictions. NRMSE is the normalized RMSE score, obtained via min-max normalization.