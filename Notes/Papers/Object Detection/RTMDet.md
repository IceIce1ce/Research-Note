<h2>1. Abstract</h2>
Explore an architecture that has compatible capacities in the backbone and neck, constructed by a basic building block that consists of large-kernel depth-wise convolutions. Introduce soft labels when calculating matching costs in the dynamic label assignment. 
<h2>2. Introduction</h2>
First exploit large-kernel depth-wise convolutions in the basic building block of the backbone and neck in the model which improves model's capability of capturing global context. Further reduce the number of building blocks to reduce model depth and increasing model width. Putting more params in the neck and making its capacity compatible with the backbone could achieve a better speed-accuracy trade-off. Existing dynamic label assignment can be improved by introducing soft targets instead of hard labels when matching ground truth boxes and model predictions. By simply adding a kernel and a mask feature generation head, RTMDet can perform instance segmentation with only around 10% additional params.
<h2>3. Related work</h2>
<h3>3.1 Efficient neural architecture for object detection</h3>
To improve model efficiency, efficient backbone networks and model scaling strategies and enhancement of multi-scale feature are explored. Recent advances also explore model re-parameterization to improve inference speed.
<h3>3.2 Label assignment for object detection</h3>
IoU, object centers, auxiliary detection heads, dynamic label assignment.
<h3>3.3 Instance segmentation</h3>
Mask classification, top-down and bottom-up approaches, dynamic kernels.
<h3>3.4 Rotated object detection</h3>
Gaussian distribution and convex set. This paper adds an angle prediction branch and replaces GIoU loss by Rotated IoU loss.
<h2>4. Methodology</h2>
<h3>4.1 Model architecture</h3>
<h4>4.1.1 Basic building block</h4>
Introduce 5 $\times$ 5 depth-wise convolutions in the basic building block of CSPDarkNet. Large-kernel depth-wise convolution is a simpler and more effective option for basic building block compared to re-parameterized 3 $\times$ 3 convolution as they require less training cost and cause less error gaps after model quantization.
<h4>4.1.2 Balance of model width and depth</h4>
Reduce the number of blocks in each backbone stage and moderately enlarging the width of the block to increase the parallelization and maintain the model capacity.
<h4>4.1.3 Balance of backbone and neck</h4>
Adopt another strategy that puts more params and computations from backbone to neck by increasing the expansion ratio of basic blocks in the neck to make them have similar capacities, which obtains a better computation-accuracy trade-off.
<h4>4.1.4 Shared detection head</h4>
Choose to share params of heads across scales but incorporate different BN layers to reduce param amount of head while maintaining accuracy.
<h3>4.2 Training strategy</h3>
<h4>4.2.1 Label assignment and losses</h4>
Propose a dynamic soft label assignment strategy based on SimOTA and its cost function is: $C = \lambda_1 C_{cls} + \lambda_2 C_{reg} + \lambda_3 C_{center}$, where $C_{cls}, C_{center}, C_{reg}$ are classification cost, region prior cost and regression cost and $\lambda_1 = 1, \lambda_2 = 3, \lambda_3 = 1, C_{cls} CE(P, Y_{soft} \times (Y_{soft} - P)^2, C_{reg} = -\log(IoU), C_{center} = \alpha^{|x_{pred} - x_{gt}| - \beta}$. Set $\alpha = 10, \beta = 3$ by default.
<h4>4.2.2 Cached mosaic and mixup</h4>
Improve MixUp and Mosaic with caching mechanism. By utilizing cache, time cost of mixing images in the training can be reduced to the level of processing a single image.
<h4>4.2.3 Two-stage training</h4>
Exclude data augmentations and increase number of mixed images to 8 in each training sample in the first training stage of 280 epochs. In the last 20 epochs, switch to large scale jittering (LSJ) allowing for fine-tuning of the model in a domain that is closely aligned with real data distributions.
<h3>4.3 Extending to other tasks</h3>
<h4>4.3.1 Instance segmentation</h4>
The mask feature head comprises 4 convolution layers that extract mask features with 8 channels from multi-level features. The kernel prediction head predicts a 169-dimensional vector for each instance which is decomposed into 3 dynamic convolution kernels to generate instance segmentation masks through interaction with the mask features and coordinate features. To exploit the prior information inherent in the mask annotations, use the mass center of masks when calculating soft region prior in the dynamic label assignment.
<h4>4.3.2 Rotated object detection</h4>
1) Add a 1 $\times$ 1 convolution layer at the regression branch to predict the rotation angle.
2) Modify the bounding box coder to support rotated boxes.
3) Replace GIoU loss with Rotated IoU loss.
<h2>5. Datasets</h2>
COCO 2017.
<h2>6. Metrics</h2>
AP, AP50, Box AP, Mask AP, mAP.
<h2>7. Code</h2>
https://github.com/open-mmlab/mmdetection/tree/3.x/configs/rtmdet.