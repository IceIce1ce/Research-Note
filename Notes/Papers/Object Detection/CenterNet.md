<h2>1. Abstract</h2>
Model an object as a single point - the center point of its bounding box. Detector uses keypoint estimation to find center points and regresses other properties: size, 3D location, orientation and pose. CenterNet achieves 28.1% AP at 142 FPS, 37.4% AP at 52 FPS and 45.1% AP with multi-scale testing at 1.4 FPS on MS-COCO.
<h2>2. Introduction</h2>
Current object detectors represent each object through an axis-aligned bounding box that tightly encompasses the object. Then, reduce object detection to image classification. For each bounding box, classifier determines if image content is an object or background. Feed the input image to a CNN to generate a heatmap. Peaks in this heatmap correspond to object centers. Image features at each peak predict the objects bounding box height and weight. Inference is a single network forward-pass, without NMS for post-processing. For 3D bounding box estimation, regress to the object absolute depth, 3D bounding box dimensions and object orientation. For human pose estimation, consider 2D join locations as offsets from the center and regress to them at the center point location.
<h2>3. Related work</h2>
<h4>3.1 Object detection by region classification</h4>
RCNN, Fast-RCNN.
<h4>3.2 Object detection with implicit anchors</h4>
Faster-RCNN. First, CenterNet assigns the anchor based on location, not box overlap. Second, only have one positive anchor per object and not need NMS. Third, CenterNet uses a larger output resolution (stride of 4) compared to traditional object detectors (stride of 16).
<h4>3.3 Object detection by keypoint estimation</h4>
CornerNet, ExtremeNet.
<h4>3.4 Monocular 3D object detection</h4>
Deep3Dbox, 3D RCNN, Deep Manta.
<h2>4. Preliminary</h2>
$I \in R^{W \times H \times 3}$: an input image, keypoint heatmap $\hat{Y} \in [0, 1]^{\frac{W}{R} \times \frac{H}{R} \times C}$ with $R$ is the output stride and C is the number of keypoint types. Keypoint types include $C$ = 17 human joints in human pose estimation or $C$ = 80 object categories in object detection. $\hat{Y}_{x, y, c} = 1$: detected keypoint and $\hat{Y}_{x, y, c} = 0$: background. Use Hourglass network, ResNet and Deep Layer Aggregation to predict $\hat{Y}$ from image $I$. For each ground truth keypoint $p \in R^2$, compute a low-resolution equivalent $\tilde{p} = \lfloor \frac{P}{R} \rfloor$. Splat all ground truth keypoints onto a heatmap $Y \in [0, 1]^{\frac{W}{R} \times \frac{H}{R} \times C}$ using a Gaussian kernel $Y_{xyc} = exp(-\frac{(x - \tilde{p}_x)^2 + (y - \tilde{p}_y)^2}{2\sigma_p^2})$ with $\sigma_p$ is an object size-adaptive standard deviation. Training objective: if $Y_{xyc} = 1, L_k = \frac{-1}{N}\sum_{xyc}(1 - \hat{Y}_{xyc})^\alpha \log(\hat{Y}_{xyc})$, else $L_k = \frac{-1}{N}\sum_{xyc}(1 - Y_{xyc})^\beta(\hat{Y}_{xyc})^\alpha \log(1 - \hat{Y}_{xyc})$, where $\alpha = 2$ and $\beta = 4$ are hyper-parameters of focal loss and N is the number of keypoints in image $I$. To recover discretization error, predict a local offset $\hat{O} \in \mathcal{R}^{\frac{W}{R} \times \frac{H}{R} \times 2}$ for each center point. Loss of offset use L1: $L_{off} = \frac{1}{N}\sum_p|\hat{O}_\tilde{p} - (\frac{p}{R} - \tilde{p})|$. 
<h2>5. Objects as Points</h2>
$(x_1^{(k)}, y_1^{(k)}, x_2^{(k)}, y_2^{(k)})$: bounding box of object k with category $c_k$. Center point lies at: $p_k = (\frac{x_1^{(k)} + x_2^{(k)}}{2}, \frac{y_1^{(k)} + y_2^{(k)}}{2})$. Object size: $s_k = (x_2^{(k)} - x_1^{(k)}, y_2^{(k)} - y_1^{(k)})$. Use a single size prediction $\hat{S} \in \mathcal{R}^{\frac{W}{R} \times \frac{H}{R} \times 2}$ for all object categories. L1 loss at center point: $L_size = \frac{1}{N}\sum_{k=1}^N|\hat{S}_{pk} - s_k|$. Overall training objective: $L_{det} = L_k + \lambda_{size}L_{size} + \lambda_{off}L_{off}$ with $\lambda_{size} = 0.1$ and $\lambda_{off} = 1$. 
<h4>5.1 From points to bounding boxes</h4>
Extract peaks in heatmap for each category. Then, detect all responses whose value $\geq$ 8-connected neighbors and keep top 100 peaks. $\hat{\mathcal{P}_c}$: set of n detected center points $\hat{P} = {(\hat{x}_i, \hat{y}_i)}^n_{i=1}$. Bounding box at location: $(\hat{x}_i + \delta \hat{x}_i - \hat{w}_i/2, \hat{y}_i + \delta \hat{y}_i - \hat{h}_i/2, \hat{x}_i + \delta \hat{x}_i + \hat{w}_i/2, \hat{y}_i + \delta \hat{y}_i - \hat{h}_i/2)$ with $(\delta \hat{x}_i, \delta \hat{y}_i) = \hat{O}_{\hat{x}_i, \hat{y}_i}$ is the offset prediction and $(\hat{w}_i, \hat{h}_i) = \hat{S}_{\hat{x}_i, \hat{y}_i}$ is size prediction.
<h4>5.2 3D detection</h4>
Three additional attributes: depth, 3D dimension and orientation. $d = 1/\sigma(\hat{d}) - 1$ with $\sigma$ is sigmoid. 
<h4>5.3 Human pose estimation</h4>
Estimate $k$ 2D human joint locations for every human instance in the image ($k$ = 17 for COCO). 
<h2>6. Datasets</h2>
MS COCO, PascalVOC.
<h2>7. Metrics</h2>
$AP, AP_{50}, AP_{75}, AP_S, AP_M, AP_L$, FPS.
<h2>8. Code</h2>
https://github.com/xingyizhou/CenterNet.