<h2>1. Abstract</h2>
First analyze the influence of NMS in real-time object detectors on inference speed and establish an end-to-end speed benchmark. Propose RT-DETR, the first real-time end-to-end object detector. Specifically, design an efficient hybrid encoder to efficiently process multi-scale features by decoupling the intra-scale interaction and cross-scale fusion, and propose IoU-aware query selection to improve the initialization of object queries. In addition, proposed detector supports flexibly adjustment of the inference speed by using different decoder layers without the need for retraining.
<h2>2. End-to-end speed of detectors</h2>
<h3>2.1 Analysis of NMS</h3>
The execution time of NMS primarily depends on the number of input prediction boxes and two hyperparameters: score threshold and IoU threshold.
<h3>2.2 End-to-end speed benchmark</h3>
For real-time detectors that require NMS, anchor-free detectors outperform anchor-based detectors with equivalent accuracy because the former takes less post-processing time than the latter. Moreover, anchor-based detectors produce more predicted boxes than anchor-free detectors (three times more).
<h2>3. The real-time DETR</h2>
<h3>3.1 Model overview</h3>
RT-DETR consists of a backbone, a hybrid encoder and a transformer decoder with auxiliary prediction heads. Leverage the output features of the last three stages of the backbone {$S_3, S_4, S_5$} as the input to the encoder. The hybrid encoder transforms multi-scale features into a sequence of image features through intra-scale interaction and cross-scale fusion. Subsequently, the IoU-aware query selection is employed to select a fixed number of image features from the encoder output sequence to serve as initial object queries for the decoder. Finally, the decoder with auxiliary prediction heads iteratively optimizes object queries to generate boxes and confidence scores.
<h3>3.2 Efficient hybrid encoder</h3>
First, remove the multi-scale transformer encoder in DINO-R50 as baseline A. Next, different forms of encoders are inserted to produce a series of variants based on baseline A:
- A -> B: variant B inserts a single-scale transformer encoder which uses one layer of transformer block. The features of each scale share the encoder for intra-scale feature interaction and then concatenate the output multi-scale features.
- B -> C: variant C introduces cross-scale feature fusion based on B and feeds the concatenate multi-scale features into the encoder to perform feature interaction.
- C -> D: variant D decouples the intra-scale interaction and cross-scale fusion of multi-scale features. First, the single-scale transformer encoder is employed to perform intra-scale interaction, then a PANet-like structure is utilized to perform cross-scale fusion.
- D -> E: variant E optimizes the intra-scale interaction and cross-scale fusion of multi-scale features based on D, adopting an efficient hybrid encoder.
<h3>3.3 Hybrid design</h3>
The proposed encoder consists of two modules: the attention-based intra-scale feature interaction (AIFI) module and CNN-based cross-scale feature-fusion module (CCFM). AAFI reduces computational redundancy based on variant D which only performs intra-scale interaction on $S_5$. CCFM is also optimized based on variant D, inserting several fusion blocks composed of convolutional layers into the fusion path. The role of the fusion block is to fuse the adjacent features into a new feature. The fusion block contains $N RepBlocks$ and two-path outputs are fused by element-wise add: Q = K = V = $Flatten(S_5), F_5 = Reshape(Attn(\mathrm{Q, K, V})), Output = CCFM({S_3, S_4, F_5})$, where $Attn$ represents the multi-head self-attention and $Reshape$ represents restoring the shape of the feature to the same as $S_5$ which is the inverse operation of $Flatten$.
<h3>3.4 IoU-aware query selection</h3>
Propose IoU-aware query selection by constraining the model to produce high classification scores for features with high IoU scores and vice-versa during training. The optimization objective of the detector is: $\mathcal{L}(\hat{y}, y) = \mathcal{L}_{box}(\hat{b}, b) + \mathcal{L}_{cls}(\hat{c}, \hat{b}, y, b) = \mathcal{L}_{box}(\hat{b}, b) + \mathcal{L}_{cls}(\hat{c}, c, IoU)$, where $\hat{y}, y$ denote prediction and ground truth, $\hat{y} = \{\hat{c}, \hat{b}\}, y = \{c, b\}, c$ and $b$ represent categories and bounding boxes. Model trained with IoU-aware query selection can produce more high-quality encoder features.
<h3>3.5 Sclaed RT-DETR</h3>
Replace ResNet backbone with HGNetv2. Scale the backbone and hybrid encoder together using a depth multiplier and a width multiplier. Four hybrid encoder, control the depth multiplier and width multiplier by adjusting the number of $RepBlocks$ in CCFM and the embedding dimension of the encoder.
<h2>4. Datasets</h2>
COCO 2017.
<h2>5. Metrics</h2>
FPS$_{bs = 1}$, AP$^{val}$, AP$^{val}_{50}$, AP$^{val}_{75}$, AP$^{val}_{S}$, AP$^{val}_{M}$, AP$^{val}_{L}$.
<h2>6. Code</h2>
https://github.com/lyuwenyu/RT-DETR.