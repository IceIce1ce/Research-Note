<h2>1. Abstract</h2>
ViTPose employs plain and non-hierarchical vision transformers as backbones to extract features for a given person instance and a lightweight decoder for pose estimation. It can be scaled up from 100M to 1B parameters by taking the advantages of the scalable model capacity and high parallelism of transformers, setting a new Pareto front between throughput and performance.
<h2>2. ViTPose</h2>
<h3>2.1 The simplicity of ViTPose</h3>
Structure simplicity: do not adopt skip-connections or cross-attentions in the decoder layers but simple deconvolution layers and a prediction layer. Specifically, given a person instance image $X \in \mathcal{R}^{H \times W \times 3}$ as input, ViTPose first embeds images into tokens via a patch embedding layer, i.e., $F \in \mathcal{R}^{\frac{H}{d} \times \frac{W}{d} \times C}$, where $d$ = 16 is down sampling ratio of patch embedding layer and C is channel dimension. After that, embedded tokens are processed by several transform layers each of which is consisted of a multi-head self-attention (MHSA) layer and a feed-forward network (FFN): $F'_{i + 1} = F_i + \mathrm{MHSA}(\mathrm{LN}(F_i)), F_{i + 1} = F'_{i + 1} + \mathrm{FFN}(\mathrm{LN}(F'_{i + 1}))$, where $i$ represents output of $i$th transform layer and initial feature $F_0$ = PatchEmbed($X$) denotes features after patch embedding layer. Denote the output feature of abckbone network as $F_{out} \in \mathcal{R}^{\frac{H}{d} \times \frac{W}{d} \times C}$. Adopt two kinds of lightweight decoders to process the features extracted from the backbone network and localize the keypoints. The first one is the classic decoder. It is composed of two deconvolution blocks, each of which contains one deconvolution layer followed by batch normalization and ReLU. Each block upsamples the feature maps by 2 times. Then, a convolution layer with the kernel size $1 \times 1$ is utilized to get the localization heatmaps for keypoints: $K = \mathrm{Conv}_{1 \times 1}(\mathrm{Deconv}(\mathrm{Deconv}(F_{out})))$, where $K \in \mathcal{R}^{\frac{H}{4} \times \frac{W}{4} \times N_k}$ denotes the estimated heatmaps (one for each keypoint) and $N_k$ is the number of keypoints to be estimated. Although the classic decoder is simple and lightweight, also try another simpler decoder. Directly upsample the feature maps by 4 times with bilinear interpolation, followed by a ReLU and a convolution layer with the kernel size 3 $\times$ 3 to get heatmaps: $K = \mathrm{Conv}_{3 \times 3}(\mathrm{Bilinear}(\mathrm{ReLU}(F_{out})))$. 
<h3>2.2 The transferability of ViTPose</h3>
Randomly initialize an extra learnable knowledge token t and append it to the visual tokens after the patch embedding layer of the teacher model. Then, freeze the well-trained teacher model and only tune the knowledge token for several epochs to gain the knowledge: $t^* = \mathrm{argmin}_t(\mathrm{MSE}(T(\{t; X\}), K_{gt})$, where $K_{gt}$ is ground truth heatmaps, $X$ is input images, $T(\{t; X\})$ denotes predictions of teacher and $t^*$ represents optimal token that minimizes loss. After that, the knowledge token $t^*$ is frozen and concatenated with the visual tokens in the student network during training to transfer the knowledge from teacher to student networks. Thus, the loss of the student network is: $L^{td}_{t \rightarrow s} = \mathrm{MSE}(S(\{t^*; X\}), K_{gt})$ or $L^{tod}_{t \rightarrow s} = \mathrm{MSE}(S(\{t^*; X\}), K_t) + \mathrm{MSE}(S(\{t^*; X\}), K_{gt})$, where $L^{td}_{t \rightarrow s}$ and $L^{tod}_{t \rightarrow s}$ represent token distillation loss and combination of output distillation loss and token distillation loss.
<h2>3. Datasets</h2>
MS COCO Keypoint.
<h2>4. Metrics</h2>
AP, AP$_{50}$, AP$_{75}$, AR, AR$_{50}$, AR$_{75}$, AP$_M$, AP$_L$ and AP$_L$.
<h2>5. Code</h2>
https://github.com/ViTAE-Transformer/ViTPose.