<h2>1. Abstract</h2>
Assimilate ideas from recent network design, training strategies, testing techniques, quantization and optimization methods. 
<h2>2. Introduction</h2>
Examine post-training quantization (PTQ) and quantization-aware training (QAT) to accommodate them in YOLOv6. Imbue YOLOv6 with a self-distillation strategy, performed both on classification task and regression task. Meanwhile, dynamically adjust knowledge from teacher and labels to help the student model learn knowledge more efficiently during training phase. Reform the quantization scheme for detection with the help of RepOptimizer and channel-wise distillation.
<h2>3. Method</h2>
- Network design: take RepBlock as building block of small networks. For large models -> CSPStackRep Block. The neck adopts PAN topology following YOLOv4 and YOLOv5. Enhance neck with RepBlocks or CSPStackRep blocks to have RepPan. Head -> efficient decoupled head.
- Label assignment: TAL.
- Loss function: VariFocal loss for classifcation loss and SIoU/GIoU loss as regression loss. 
- Industry-handy improvements: self-distillation and more training epochs. 
- Quantization and deployment: train YOLOv6 with RepOptimizer to obtain PTQ-friendly weights. Adopt QAT with channel-wise distillation and graph optimization to pursue extreme performance.
<h3>3.1 Network design</h3>
In YOLOv6, propose two scaled re-parameterizable backbones and necks to accommodate models at different size, as well as an efficient decoupled head with hybrid-channel strategy.
<h4>3.1.1 Backbone</h4>
For small models -> RepBlock. Each RepBlock is converted to stacks of 3 $\times$ 3 convolutional layers with ReLU actiavtion functions during the inference phase. For medium and large networks -> CSPStackRep block. CSPStackRep block is composed of three 1 $\times$ 1 convolution layers and a stack of sub-blocks consisting of two RepVGG blocks or RepConv (at training or inference) with a residual connection. Besides, a CSP connection is adopted to boost performance.
<h4>3.1.2 Neck</4h>
Adopt modified PAN topology from YOLOv4 and YOLOv5. Inaddition, replace CSPBlock used in YOLOv5 with RepBlock (for small models) or CSPStackRep Block (for large models) and adjust width and depth. The neck of YOLOv6 is Rep-PAN.
<h4>3.1.3 Head</h4>
Adopt a hybrid-channel strategy to build a more efficient decoupled head. Specifically, reduce the number of middle 3 $\times$ 3 convolutional layers to only one. The width of the head is jointly scaled by width multiplier for backbone and neck. Adopt anchor point-based paradigm, whose box regression branch predicts the distance from anchor point to four sides of bounding boxes.
<h3>3.2 Label assignment</h3>
Since TAL could bring more performance improvement than SimOTA and stabilize the training, adopt TAL as default label assignment strategy in YOLOv6.
<h3>3.3 Loss functions</h3>
<h4>3.3.1 Classification loss</h4>
VFL Treats positive and negative samples at different degrees of importance, it balances learning signals from both samples -> adopt VFL in YOLOv6.
<h4>3.3.2 Box regression loss</h4>
SIoU is applied to YOLOv6-N and YOLOv6-T while others use GIoU. Probability loss: DFL simplifies the underlying continuous distribution of box locations as a discretized probability distribution. It considers ambiguity and uncertainty in data without introducing any other strong priors -> adopt DFL in YOLOv6-M/L.
<h4>3.3.3 Object loss</h4>
Tried object loss in YOLOv6 but it doesn't bring many positive effects.
<h3>3.4 Industry-handy imporvements</h3>
<h4>3.4.1 More training epochs</h4>
Extend training duration from 300 to 400 epochs.
<h4>3.4.2 Self-distillation</h4>
Apply the classical knowledge distillation technique minimizing KL-divergence between prediction of teacher and student. Limit the teacher to be the student itself but pretrained, call it self-distillation. Knowledge distillation loss: $L_{KD} = KL(p_t^{cls}||p_s^{cls}) + KL(p_t^{reg}||p_s^{reg})$, where $p^{cls}_t$ and $p^{cls}_s$ are class prediction of teacher model and student model, $p^{reg}_t and p_s^{reg}$ are box regression predictions. The overall loss: $L_{total} = L_{det} + \alpha L_{KD}$, where $L_{det}$ is detection loss computed with predictions and labels.
<h4>3.4.3 Gray border of images</h4>
Without gray border, the performance of YOLOv6 deteriorates. The problem is related to the gray borders padding in Mosaic augmentation.
<h3>3.5 Quantization and deployment</h3>
<h4>3.5.1 Reparameterizing optimizer</h4>
RepOptimizer proposes gradient re-parameterization at each optimization step. Reconstruct the re-parameterization blocks of YOLOv6 in this fashion and train it with RepOptimizer to obtain PTQ-friendly weights.
<h4>3.5.2 Quantization-aware training with channel-wise distillation</h4>
In case PTQ is insufficient, propose QAT to boost quantization performance. Besides, channel-wise distillation is adapted within the YOLOv6 framework.
<h2>4. Datasets</h2>
COCO 2017.
<h2>5. Metrics</h2>
AP$^{val}$, AP$^{val}_{50}$, FPS$_{bs = 1}$, FPS$_{bs = 32}$.
<h2>6. Code</h2>
https://github.com/meituan/YOLOv6.