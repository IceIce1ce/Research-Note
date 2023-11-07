<h2>1. Abstract</h2>
Improve the efficiency of optical flow computation with foreground mask and spacial sampling and increase the robustness of optical flow with good feature (TK) points selecting and forward-backward filtering. A foreground channel is also added to the feature vector to help detect static or low speed objects.
<h2>2. Methodology</h2>
<h3>2.1 Robust sparse optical flow</h3>
First, the foreground-masked input frame owns fewer and only moving pixels, which reduce the amount of calculation as well as the chance of matching errors. Second, finding good features makes our optical flow more reliable. Third, the LKT tracker is the most commonly used and stable method for computation of optical flow. LKT tracker is used to compute optical flow only on good feature points, which is both robust and fast. At last, a forward-backward filter inspired by further removes the unreliable matching results and leaves us the robust optical flow. The optical flow is computed in both directions and the distance between the origin of forward flow and the destination of the backward is recorded. Then the worst 50% optical flow is filtered out by a mean filter.
<h3>2.2 Feature extraction and aggregation</h3>
One important limitation of optical flow is that it cannot extract feature from static or slow speed object, even if the object is detected as foreground by motion detection algorithms. A new channel named foreground is added to feature. For each pixel, foreground = 1 for foreground pixel and 0 for background pixel. To further improve robustness, a spacial Gaussian blur is performed on the aggregated feature (on each channel separately), which makes the feature more smooth and stable.
<h3>2.3 Training and detection</h3>
Let vector $A(b, t)$ denote the aggregated feature of block $b$ at frame $t$. The training process is to find a maximum boundary $B(b)$ for each feature channel: $B(b) = \mathrm{max}_tA(b, t)$, where $t$ is a variable that enumerates all frames of training sequences. Let vector $v(b, t)$ denote the aggregated feature extracted from the test video. Then, compute the distance vector $D(b, t)$ as: $D(b, t) = v(b, t) - B(b)$. Finally, whether a block is abnormal is decided by thresholding on each channel of the distance vector: $x(b, t) = 1$ if $D(b, t) > \theta$ and 0 otherwise.
<h3>2.4 Acceleration</h3>
For optical flow, there are two methods for acceleration: 1) foreground is used as a mask to increase both efficiency and robustness and 2) sparse optical flow extracted from fixed grid pixels are sufficient for feature aggregation.
<h2>3. Datasets</h2>
UCSD.
<h2>4. Metrics</h2>
AUC, FPS.
<h2>5. Code</h2>
https://github.com/TomHeaven/AnomalyAnalysisWithOpticalFlow.