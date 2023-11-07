<h2>1. Abstract</h2>
YOLOv7 outperforms all known object detectors from 5 FPS to 160 FPS and has the highest accuracy 56.8 AP with 30 FPS on Tesla V100.
<h2>2. Introduction</h2>
Real-time object detectors on CPU: MobileNet, ShuffleNet and GhostNet. Real-time object detectors on GPU: ResNet, DarkNet and DLA. Then, use CSPNet to optimize the architecture. Main contributions: 1) design trainable bag-of-freebies methods to improve detection accuracy without increasing inference cost, 2) found two new issues, how re-parameterized module replaces original module and how dynamic label assignment deals with assignment to different output layers, 3) propose extend and compound scaling methods and 4) reduce 40% parameters and 50% computation.
<h2>3. Related work</h2>
<h3>3.1 Real-time object detectors</h3>
Some characteristics for a SOTA object detector: 1) a faster and stronger network architecture, 2) an effective feature integration method, 3) an accurate detection method, 4) a robust loss function, 5) an efficient label assignment and 6) an efficient training method.
<h3>3.2 Model re-parameterization</h3>
Model re-parameterization techniques merge multiple computation modules into one at inference stage. It has two categories: module-level ensemble and model-level ensemble. For model-level re-parameterization, the first way is to train multiple identical models with different training data and then average the weights of multiple trained models. The other way is to perform a weighted average of the weights of models at different iteration number. For module-level re-parameterization, it splits a module into multiple identical or different module branches during training and integrates multiple branched modules into an equivalent module during inference.
<h3>3.3 Model scaling</h3>
Model scaling uses different scaling factors such as resolution (size of input image), depth (number of layer), width (number of channel) and stage (number of feature pyramid).
<h2>4. Architecture</h2>
<h3>4.1 Extended efficient layer aggregation networks</h3>
E-ELAN uses expand, shuffle, merge cardinality to achieve the ability to continuously enhance the learning ability of network without destroying the original gradient path. E-ELAN only changes the architecture in computational block, while the architecture of transition layer is unchanged. Strategy is to use group convolution to expand the channel and cardinality of computational blocks. Apply the same group parameter and channel multiplier to all the computational blocks of a computational layer. Then, the feature map calculated by each computational block will be shuffled into $g$ groups according to the set group parameter $g$, and then concatenate them together. Finally, add $g$ groups of feature maps to perform merge cardinality. E-ELAN also guide different groups of computational blocks to learn more diverse features.
<h3>4.2 Model scaling for concatenation-based models</h3>
When scale the depth factor of a computational block, also calculate the change of the output of that block. Then, perform width factor scaling with the same amount of change on the transition layers.
<h2>5. Trainable bag-of-freebies</h2>
<h3>5.1 Planned re-parameterized convolution</h3>
Using RepConv in RepVGG without identity connection (RepConvN) to design the architecture of planned re-parameterized convolution. 
<h3>5.2 Coarse for auxilary and fine for lead loss</h3>
Deep supervision adds extra auxiliary head in the middle layers of the network, and the shallow network weights with assistant loss as the guide. Label assigner: considers the network prediction results together with the ground truth and then assigns soft labels. New label assignment: use lead head prediction as guidance to generate coarse-to-fine hierarchical labels, which are used for auxiliary head and lead head learning.
<h4>5.2.1 Lead head guided label assigner</h4>
It is calculated based on the prediction result of lead head and ground truth, and generate soft label through the optimization process. This set of soft labels will be used as the target training model for both auxiliary head and lead head.
<h4>5.2.2 Coarse-to-fine lead head guided label assigner</h4>
It uses the predicted result of the lead head and the ground truth to generate soft label. Generate two sets of soft label: coarse label and fine label, where fine label is the same as soft label generated by lead head guided label assigner, and coarse label is generated by allowing more grids to be treated as positive target by relaxing the contraints of the positive sample assignment process.
<h3>5.3 Other trainable bag-of-freebies</h3>
- Batch norm in conv-bn-activation topology. This part connects batch norm layer directly to convolutional layer. The purporse is to integrate the mean and variance layer at the inference stage.
- Implicit knowledge in YOLOR combined with convolution feature map in addition and multiplication manner.
- EMA model as the final inference model.
<h2>6. Datasets</h2>
MS-COCO 2017.
<h2>7. Metrics</h2>
FPS, AP$_{50}$, AP$_{75}$, AP$_{S}$, AP$_{M}$, AP$_{L}$.
<h2>8. Code</h2>
https://github.com/WongKinYiu/yolov7.