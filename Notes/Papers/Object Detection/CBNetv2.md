<h2>1. Abstract</h2>
Propose CBNet to construct high-performance detectors using existing open-source pre-trained backbones under the pre-training fine-tuning paradigm. CBNet architecture groups multiple identical backbones, which are connected through composite connections. Specifically, it integrates high and low-level features of multiple identical backbone networks and gradually expands the receptive field to more effectively perform object detection. Also propose a better training strategy with auxiliary supervision for CBNet-based detectors. CB-Swin-L achieves 59.4% box AP and 51.6% mask AP con COCO test-dev while reducing the training time by 6$\times$. With multi-scale testing, 60.1% box AP and 52.3% mask AP without using extra training data.
<h2>2. Introduction</h2>
CBNet groups multiple identical backbones together. Specifically, parallel backbones are connected via composite connections. The output of each stage in an assisting backbone flows to the parallel and lower-level stages of its succeeding sibling. Finally, features of lead backbone are fed to the neck and detection head for bounding box regression and classification. CBNet integrates high and low-level features of multiple backbone networks and expands the receptive field for more effective object detection. Notably, each composed backbone of CBNet is initialized by weights of an existing open-source pre-trained individual backbone. In addition, propose an effective training strategy with supervision for assisting backbones. In particular, propose a pruning strategy to reduce model complexity while not sacrificing accuracy.  Two version of CBNet: CBNetV1 connects only the adjacent stages of parallel backbones, CBNetV2 combines dense higher-level composition strategy, auxiliary supervision and a special pruning strategy.
<h2>3. Related work</h2>
<h3>3.1 Object detection</h3>
- One-stage detectors: YOLO, SSD, RetinaNet, NAS-FPN, EfficientDet.
- Two-stage detectors: Faster R-CNN, FPN, Mask R-CNN, Cascade R-CNN and Libra R-CNN.
<h3>3.2 Backbones for object detection</h3>
VGG, ResNet, DenseNet, ResNeXt, Res2Net, DetNet, FishNet, HRNet, DetNas, Joint-DetNAS, Swin Transformer and PVT.
<h3>3.3 Recurrent convolution neural network</h3>
Incorporate recurrent connections into each convolution layer to enhance the contextual information integration ability of the model. First, the connections between the parallel stages in CBNet are undirectional, while they are bidirectional in RCNN. Second, in RCNN, parallel stages at different time steps share parameter weights, while in the proposed CBNet, the parallel stages of backbones are independent of each other.
<h3>3.4 Model ensemble</h3>
Two key characteristics: model diversity and voting. Model diversity means that models with different architectures or training techniques are trained separately. Most ensemble methods need voting strategies to compare outputs of different models and refine the final predictions.
<h2>4. Proposed method</h2>
<h3>4.1 Architecture of CBNet</h3>
CBNet consists of $K$ identical backbones, It includes two types of backbones: lead backbone $B_K$ and assisting backbones $B_1, B_2,...,B_{K - 1}$. Each backbone comprises $L$ stages (usually $L$ = 5) and each stage consists of several convolutional layers with feature maps of the same size. The $l$-th stage of the backbone implements the non-linear transformation $F^l(\cdot)(l = 1, 2,...,L)$.  $x^l = F^l(x^{l - 1}), l \geq 2$. $x^l_k = F^l_k(x^{l - 1}_k + g^{l - 1}(x_{k - 1})), l \geq 2, k = 2, 3,...,K$, where $g^{l - 1}(\cdot)$ represents composite connection which take features from assisting backbone $B_{k - 1}$ as input and takes features of the same size as $x^{l - 1}_k$ as output. Therefore, the output features of $B_{k - 1}$ are transformed and contribute to input of each stage in $B_k$. For object detection task, only output features of lead backbone are fed into neck and then RPN/detection head, while outputs of assisting backbone are forwarded to its succeeding siblings.
<h3>4.2 Possible Composite Strategies</h3>
For composite connection $g^l(x)$ which takes x = $\{x^i|i = 1,2,...,L\}$ from an assisting backbone as input and outputs a feature of the same size of $x^l$.
- Same Level Composition: $g^l(x) = w(x^l), l \geq 2$, where $w$ represents a 1 $\times$ 1 convolution layer and a batch norm layer.
- Adjacent Higher-Level Composition: $g^l(x) = \mathrm{U}(w(x^{l + 1})), l \geq 1$, where $\mathrm{U}(\cdot)$ is up-sampling operation.
- Adjacent Lower-Level Composition: $g^l(x) = \mathrm{D}(w(x^{l + 1})), l \geq 1$, where $\mathrm{D}(\cdot)$ is down-sample operation.
- Dense Higher-Level Composition: $g^l(x) = \sum_{i = l + 1}^L\mathrm{U}(w_i(x^i)), l \geq 1$. When $K$ = 2, compose features from all higher-level stages in previous backbone and add them to lower-level stages in latter one.
- Full-connected Composition: $g^l(x) = \sum_{i = l + 1}^L\mathrm{I}(w_i(x^i)), l \geq 1$, where $\mathrm{I}(\cdot)$ denotes scale-resizing, $\mathrm{I}(\cdot) = \mathrm{D}(\cdot)$ when $i > l$ and $\mathrm{I}(\cdot) = \mathrm{U}(\cdot)$ when $i < l$.
<h3>4.3 Auxiliary Supervision</h3>
Generate initial results of assisting backbones by supervision with auxiliary neck and detection head to provide additional regularization. Total loss is defined: $\mathcal{L} = \mathcal{L}_{Lead} + \sum_{i = 1}^{K - 1}(\lambda_i \cdot \mathcal{L}^i_{Assist})$. During the inference phase, abandon auxiliary supervision branch and utilize output features of lead backbone.
<h3>4.4 Pruning strategy for CBNet</h3>
When $K = 2$, $s_i$ indicates there are $i$ stages $\{x_j|j \geq 6 - i$ and $j \leq 5, i = 0, 1, 2, 3, 4\}$ in the 2, 3,...,$K$-th backbone and pruned stages are filled by features of same stages in first backbone.
<h3>4.5 Architecture of detection network with CBNet</h3>
CBNetV1 uses only AHLC while CBNetV2 combines DHLC, auxiliary supervision and pruning strategy.
<h2>5. Datasets</h2>
COCO test-dev.
<h2>6. Metrics</h2>
AP$^{\mathrm{box}}$, AP$^{\mathrm{box}}_{50}$, AP$^{\mathrm{box}}_{75}$, AP$^{\mathrm{box}}_{\mathrm{S}}$, AP$^{\mathrm{box}}_{\mathrm{M}}$, AP$^{\mathrm{box}}_{\mathrm{L}}$. 
<h2>7. Code</h2>
https://github.com/VDIGPKU/CBNetV2.
