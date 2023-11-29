<h2>1. Abstract</h2>
Use Faster R-CNN to identify objects labels and their corresponding location in the video scene as the first step. Then, the optical flow will be utilized to identify adaptive traffic flows in each region of the frame.
<h2>2. Adaptive anomaly detection (AAD)</h2>
<h3>2.1 Optical flow</h3>
The motion patterns is estimated by optical flow using Farneback method. One of main limitations of optical flow is that it fails to extract motion characteristics from static or very slow moving objects. After estimating the pixel-wise dense optical flow, process the data through two pooling stages. First, apply 2 * 2 average pooling with two major objective which are noise and dimensionality reduction that consequently results in improved speed and performance. Then, kept decreasing the dimension by set the maximum change to be the the block representative via a 2 * 2 max pooling stage.
<h3>2.2 Distribution map</h3>
Introduce a 5-tuple that includes pixel-wise mean, max, min, variance, and number of frames of video sequences. Build two distribution maps based on both optical flow and object recognition outputs.Then, the distribution map is used to expose pixel’s values that shows drastic changes compared to velocity distribution or is classified as an unusual object in comparison with object’s history in that area of the frame. The $\mu$ is updated based on incoming data as follows: $\mu_N = \frac{\mu_{N - 1}(N - 1) + x_N}{N}$. Then if we detect an anomaly in the video sequences, we reduce by a half the number of frames, so the weight of the incoming data will be larger. Therefore the updated mean after anomaly detection is calculated in this way: $\mu_N = \frac{\mu_{N - 1}\frac{(N - 1)}{2} + x_N}{\frac{N - 1}{2} + 1}$. Next, calculate the $\sigma^2$ as follows: $\sigma^2_N = \frac{(N - 2)\sigma^2_{N - 1} + (x_N - \overline{x}_N)(x_N - \hat{x}_{N - 1})}{N - 1}$.
<h2>3. Datasets</h2>
PASCAL VOC, UMN and USDC.
<h2>4. Metrics</h2>
ROC.
