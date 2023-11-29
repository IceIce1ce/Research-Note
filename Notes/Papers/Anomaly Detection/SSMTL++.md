<h2>1. Abstract</h2>
First, study various detection methods. Second, modernize the 3D convolutional backbone by introducing multi-head self-attention modules. Third, study additional self-supervised learning tasks, such as predicting segmentation maps through knowledge distillation, solving jigsaw puzzles, estimating body pose through knowledge distillation, predicting masked regions (inpainting), and adversarial learning with pseudo-anomalies.
<h2>2. Method</h2>
<h3>2.1 Introducing new detection methods</h3>
First update is to consider YOLOv5. propose to detect objects using optical flow or background subtraction, in conjunction with a pretrained object detector.
- OF: on the one hand, consider that a pixel from the optical flow map is part of a moving object if its magnitude is larger than a threshold. Additionally, use the YOLO bounding boxes to blackout regions of already detected objects. The resulting connected components are added to the set of detected objects. To eliminate very small objects created by the noise in the optical flow map, impose a restriction for the width and height of each new object detected through optical flow.
- Background subtraction: on the other hand, we propose to use background subtraction as a faster alternative to optical flow. Apply a threshold to separate the foreground pixels from the background pixels and clean up the result with a morphological closing operation.
<h3>2.2 Introducing new backbones</h3>
extend the original 3D CNN backbone of the SSMTL architecture and introduce a CvT module formed of multiple sequential transformer blocks. For the 2D CvT module, we apply average pooling across time to obtain a 2D input of 8 $\times$ 8 tokens, before introducing the module. For the 3D CvT module, we first apply the module directly on the output of the shared 3D CNN backbone, the input for the CvT module having 7 $\times$ 8 $\times$ 8 tokens. To make it compatible, replace the 2D depthwise convolutions from CvT with 3D depthwise convolutions. The output of the 3D CvT module is passed through a global temporal pooling layer, producing the final output.
<h3>2.3 Introducing new proxy tasks</h3>
<h4>2.3.1 T5: Adversarial reconstruction</h4>
Employ an adversarial training procedure based on reversing the gradients.
<h4>2.3.2 T6: Patch inpainting</h4>
Applying the patch inpainting task at the object level and leveraging the inpainting error to estimate the abnormality level of the input object. The network is tasked with reconstructing a single image of 64 $\times$ 64 pixels, out of which, a random patch is cropped out. At test time, for a given input, this procedure is repeated three times in a row, with the output of the previous step serving as input for the current one. At each step, a different random patch is masked out. Finally, employ the L2 distance between the original image and the final reconstruction to estimate the anomaly level.
<h4>2.3.3 Segmentation</h4>
Attach a decoder to predict the segmentation map. Employ $L_2$ loss between the predicted map and the groundtruth map to train student. Use only the middle crop in the temporal sequence as input.
<h4>2.3.4 T8: Jigsaw</h4>
Split each input image into 4 $\times$ 4 patches of 16 $\times$ 16 pixels each and apply a random shuffle of the patches, out of 100 predefined shuffles. Then task the network to predict the applied shuffle in a multi-way classification setting, using softmax.
<h4>2.3.5 T9: Pose estimation</h4>
For teacher, choose pre-trained UniPose. The student network is tasked with predicting heatmaps for each body joint, being optimized with the $L_2$ loss.
<h3>2.4 Inference</h3>
Start by detecting the objects in each frame, and then, extract the object-centric temporal sequence for each detected object. Pass each object-centric sequence $X$ through the neural model, obtaining the score for each proxy task. For the middle bounding box prediction task, use the mean absolute error between the ground-truth object and the reconstructed object as the anomaly score. For the patch inpainting proxy task $T_6$, consider the mean absolute difference between the final reconstruction and the target object as the anomaly score. For segmentation $T_7$ and pose estimation $T_9$ proxy tasks, interpret the anomaly score as the mean squared error between the teacher’s output and our model’s output. Let $p_{identity}$ be the probability predicted by the jigsaw head for the input image to be identically permuted. The anomaly score for $T_8$ is given by 1 - $p_{identity}$. The final anomaly score for each object is the average of the anomaly scores given by the proxy tasks.
<h2>3. Datasets</h2>
Avenue, ShanghaiTech and UBnormal.
<h2>4. Metrics</h2>
AUC, TBDC and RBDC.