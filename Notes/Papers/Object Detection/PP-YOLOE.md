<h2>1. Abstract</h2>
Optimize PP-YOLOv2 by using anchor-free paradigm, new backbone and neck equipped with CSPRepResStage, ET-head and dynamic label assignment algorithm TAL. It achieves 51.4 mAP on COCO test-dev and 78.1 FPS on Tesla V100 with inference speed 149.2 FPS on TensorRT and FP16-precision.
<h2>2. Introduction</h2>
PP-YOLOE avoids using operators like DCN or Matrix NMS to support various hardware. 
<h2>3. Method</h2>
<h3>3.1 PP-YOLOv2</h3>
Backbone ResNet-vd with DCN, neck of PAN with SPP layer and DropBlock and the lightweight IoU aware head. ReLU is used in backbone while mish is used in neck. PP-YOLOv2 only assigns one anchor box for each ground truth object.
<h3>3.2 PP-YOLOE</h3>
<h4>3.2.1 Anchor-free</h4>
Set upper and lower bounds for three detection heads to assign ground truths to corresponding feature map. Then, the center of bounding box is calculated to select the closest pixel as positive samples.
<h4>3.2.2 Backbone and Neck</h4>
Residual connections introduce shortcut to relieve gradient vanishing problem and can be regarded as a model ensemble approach. Dense connections aggregate intermediate features with diverse receptive fields, showing good performance on object detection task. RepResBlock combines the residual connections and dense connections which is used in backbone and neck. Originating from TreeBlock, replace the concat operation of simplified TreeBlock with element-wise add operation. CSPRepResNet contains one stem composed of three convolution layer and four subsequent stages stacked by RepResBlock. In each stage, cross stage partial connections are used. ESE (Effective Squeeze and Extraction) layer is also used to impose channel attention in each CSPRepResStage. Shortcut in RepResBlock and ESE layer in CSPRepResStage are removed in neck. Width multiplier $\alpha$ and depth multiplier $\beta$ are used to scale the backbone and neck as YOLOv5. Width setting of backbone is [64, 128, 256, 512, 1024], depth setting of backbone is [3, 6, ,6, 3], width setting of neck is [192, 384, 768] and depth setting of neck is 3. 
<h4>3.2.3 Task Alignment Learning</h4>
TAL composes of a dynamic label assignment and task aligned loss. For task aligned loss, use a normalized t, namely $\hat{t}$ to replace the target in loss. BCE for classification: $L_{cls-pos} = \sum_{i = 1}^{N_{pos}}BCE(p_i, \hat{t}_i)$.
<h4>3.2.4 Efficient Task-aligned Head (ET-head)</h4>
Using ESE to replace the layer attention in TOOD and replace the alignment of regression branches with distribution focal loss (DFL) layer. For learning of classification and location tasks, choose varifocal loss (VFL) and distribution focal loss (DFL). For VFL, it uses target score to weight the loss of positive samples while FDL use general distribution to predict bounding box. Loss function of model: $Loss = \frac{\alpha \cdot loss_{VFL} + \beta \cdot loss_{GIoU} + \gamma \cdot loss_{DFL}}{\sum_i^{N_{pos}}\hat{t}}$.
<h2>4. Datasets</h2>
MS COCO-2017.
<h2>5. Metrics</h2>
FPS, AP, AP$_{50}$, AP$_{75}$, AP$_{S}$, AP$_{M}$, AP$_{L}$.
<h2>6. Code</h2>
https://github.com/Nioolek/PPYOLOE_pytorch.