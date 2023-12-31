<h2>1. Abstract</h2>
Present YOLOF, a two key components: dilated encoder and uniform matching. 
<h2>2. Introduction</h2>
This paper studies the influence of FPN's two benefits in one-stage detectors. Design experiments by decoupling the multi-scale feature fusion and divide-and-conquer functionalities with RetinaNet. In detail, consider FPN as a MiMo encoder which encodes multi-scale features from the backbone and provides feature representations for the decoder. Propose YOLOF which only uses one single C5 feature (with a downsample rate of 32) for detection. First design the structure of the encoder properly to extract the multi-scale contexts for objects on various scales, compensating for the lack of multiple-level features. Then, apply a uniform matching mechanism to solve the imbalance problem of positive anchors raised by the sparse anchors in the single feature.
<h2>3. Related works</h2>
<h3>3.1 Multiple-level feature detectors</h3>
Typical approaches to construct multiple features can be categorized into image pyramid methods (DPM) and feature pyramid methods (SSD, FPN).
<h3>3.2 Single-level feature detectors</h3>
R-CNN, R-FCN, YOLO, CornerNet, CenterNet, DETR.
<h2>4. Method</h2>
Replace the complex MiMo encoder with the simple SiSo encoder -> performance drops extensively -> two problems brought by SiSo encoders. The first problem is that the range of scales matching to the C5 feature's receptive field is limited. The second one is the imbalance problem on positive anchors raised by sparse anchors in the single-level feature.
<h3>4.1 Limited scale range</h3>
Combine the original scale range and the enlarged scale range by adding the corresponding features, resulting in an output feature with multiple receptive fields covering all object scales. The above operations can be achieved by constructing residual blocks with dilations on the middle 3 $\times$ 3 convolution layer.
<h4>4.1.1 Dilated encoder</h4>
It contains two main components: projector and residual blocks. The projection layer first applies one 1 $\times$ 1 convolution layer to reduce the channel dimension, then add one 3 $\times$ 3 convolution layer to refine semantic contexts. After that, stack four successive dilated residual blocks with different dilation rates in the 3 $\times$ 3 convolution layers to generate output features with multiple receptive fields, covering all objects' scales.
<h3>4.2 Imbalance problem on positive anchors</h3>
In MiMo encoders, the anchors are pre-defined on multiple levels in a dense paved fashion and the ground-truth boxes generate positive anchors in feature levels corresponding to their scales. Large ground-truth boxes induce more positive anchors than small ground-truth boxes -> cause an imbalance problem.
<h4>4.2.1 Uniform matching</h4>
To solve the imbalance problem, propose an uniform matching strategy: adopting k nearest anchor as positive anchors for each ground-truth box which makes sure that all ground-truth boxes can be matched with the same number of positive anchors uniformly regardless of their sizes. Following Max-IoU matching, set IoU thresholds in uniform matching to ignore large IoU (>0.7) negative anchors and small IoU (<0.15) positive anchors.
<h3>4.3 YOLOF</h3>
<h4>4.3.1 Backbone</h4>
Adopt ResNet and ResNeXt as backbone. The output of backbone is C5 feature map which has 2048 channels and with a downsample rate of 32. All BN layers in the backbone are frozen by default.
<h4>4.3.2 Encoder</h4>
First follow FPN by adding two projection layers (one 1 $\times$ 1 and one 3 $\times$ 3 convolution) after backbone, resulting in a feature map with 512 channels. Then, to enable encoder's output feature cover all objects on various scales, add residual blocks which consist of three consecutive convolutions: the first 1 $\times$ 1 convolution apply channel reduction with a reduction rate of 4, then a 3 $\times$ 3 convolution with dilation is used to enlarge the receptive field. At last, a 1 $\times$ 1 convolution to recover the number of channels.
<h4>4.3.3 Decoder</h4>
Adopt the main design of RetinaNet which consists of two parallel task-specific heads: the classification head and regression head. Only add two minor modifications. The first one is make the number of convolution layers in two heads different. There are four convolutions followed by BN layers and ReLU layers on regression head while only have two on classification head. The second is that follow Autoassign and add an implicit objectness prediction for each anchor on regression head. The final classification scores for all predictions are generated by multiplying the classification output with the corresponding implicit objectness.
<h4>4.3.4 Other details</h4>
Pre=defined anchors in YOLOF are sparse -> add a random shift operation on the image to circumvent this problem. The operation shifts the image randomly with a maximum of 32 pixels in left, right, top and bottom and aims to inject noises into object's position in the image. Moreover, add a restriction that centers' shift for all anchors should smaller than 32 pixels.
<h2>5. Datasets</h2> 
MS COCO.
<h2>6. Metrics</h2>
AP, AP$_{50}$, AP$_{75}$, AP$_{S}$, AP$_{M}$, AP$_{L}$, FPS.
<h2>7. Code</h2>
https://github.com/megvii-model/YOLOF.