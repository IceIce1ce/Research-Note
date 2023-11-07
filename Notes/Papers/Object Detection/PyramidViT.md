<h2>1. Abstract</h2>
Introduce PVT, which overcomes the difficulties of porting Transformer to various dense prediction tasks. PVT has several merits: 1) PVT not only can be trained on dense partitions of an image to achieve high output resolution, which is important for dense prediction, but also use a progressive shrinking pyramid to reduce computations of large feature maps, 2) PVT inherits the advantages of both CNN and Transformer, making it a unified backbone for various vision tasks without convolution.
<h2>2. Introduction</h2>
PVT overcomes the difficulties of Transformer by 1) taking fine-grained image patches as input to learn high-resolution representation, 2) introducing a progressive shrinking pyramid to reduce sequence length of Transformer as network deepens and 3) adopting a spatial-reduction attention (SRA) layer to reduce the resource consumption.
<h2>3. Related work</h2>
<h3>3.1 CNN backbones</h3>
GoogleLeNet, ResNeXt, DPN, MixNet, SKNet, ResNet and DenseNet.
<h3>3.2 Dense prediction tasks</h3>
<h4>3.2.1 Object detection</h4>
SSD, RetinaNet, FCOS, GFL, PolarMask, OneNet, Faster R-CNN, Mask R-CNN, Cascade R-CNN, Sparse R-CNN, DETR and deformable DETR.
<h4>3.2.2 Semantic segmentation</h4>
UNet, DeepLab.
<h3>3.3 Self-attention and transformer in vision</h3>
Non-local block, Criss-cross, stand-alone self-attention, AANet, LambdaNetworks and DeiT.
<h2>4. Pyramid vision transformer</h2>
<h3>4.1 Overall architecture</h3>
PVT has 4 stages that generate feature maps of different scales. All stages share a similar architecture, which consists of a patch embedding layer and $L_i$ Transformer encoder layers. In the first stage, given an input image of size $H \times W \times 3$, divide it into $\frac{HW}{4^2}$ patches, each of size $4 \times 4 \times 3$. Then, feed the flattened patches to a linear projection and obtain embedded patches of size $\frac{HW}{4^2} \times C_1$. After that, the embedded patches along with a position embedding are passed through a Transformer encoder with $L_1$ layers, and output is reshaped to a feature map $F_1$ of size $\frac{H}{4} \times \frac{W}{4} \times C_1$. In the same way, using the feature map from previous stage as input, obtain the following feature maps: $F_2, F_3$ and $F_4$ whose strides are 8, 16 and 32 pixels.
<h3>4.2 Feature pyramid for Transformer</h3>
PVT uses a progressive shrinking strategy to control the scale of feature maps by patch embedding layers. Denote the patch size of $i$-th stage as $P_i$. At the beginning of stage $i$, divide input feature map $F_{i - 1} \in \mathbb{R}^{H_{i - 1} \times W_{i - 1} \times C_{i - 1}}$ into $\frac{H_{i - 1}W_{i - 1}}{P^2_i}$ patches and then each patch is flatten and then projected to a $C_i$-dimensional embedding. After the linear projection, shape of embedded patches can be viewed as $\frac{H_{i - 1}}{P_i} \times \frac{W_{i - 1}}{P_i} \times C_i$, where height and width are $P_i$ times smaller than input.
<h3>4.3 Transformer encoder</h3>
Transformer encoder in stage $i$ has $L_i$ encoder layers, each of which is composed of an attention layer and a feed-forward layer. Since PVT needs to process high-resolution feature maps, propose SRA layer to replace MHA layer in the encoder. SRA receives a query $Q$, a key $K$ and a value $V$ as input and outputs a refined feature. The difference is that SRA reduces the spatial scale of $K$ and $V$ before attention operation. $\mathrm{SRA}(Q, K, V) = \mathrm{Concat}(\mathrm{head}_0,...,\mathrm{head}_{N_i})W^O, \mathrm{head}_j = \mathrm{Attention}(QW^Q_j, \mathrm{SR}(K)W^K_j, \mathrm{SR}(V)W^V_j)$, where $W^Q_j \in \mathbb{R}^{C_i \times d_{head}}, W^K_j \in \mathbb{R}^{C_i \times d_{head}}, W^V_j \in \mathbb{R}^{C_i \times d_{head}}$ and $W^O \in \mathbb{R}^{C_i \times C_i}$ are linear projection parameters. $N_i$ is head number of attention layer in stage $i, d_{head}$ is equal to $\frac{C_i}{N_i}$. SR($\cdot$) is operation for reducing spatial dimension of input sequence ($K$ or $V$): SR(x) = Norm(Reshape(x, $R_i$)$W^S$). Here, x $\in \mathbb{R}^{(H_iW_i) \times C_i}$ represents a input sequence and $R_i$ denotes reduction ratio of attention layers in stage i. Reshape(x, $R_i$) is an operation of reshaping the input sequence x to a sequence of size $\frac{H_iW_i}{R^2_i} \times (R^2_iC_i)$, $W_S \in \mathbb{R}^{(R^2_iC_i) \times C_i}$ is a linear projection that reduces dimension of input sequence to $C_i$. Attention(q, k, v) = Softmax($\frac{\mathrm{qk^T}}{\sqrt{d_{head}}})\mathrm{v}$.
<h2>5. Datasets</h2>
ImageNet 2012, COCO 2017, ADE20K.
<h2>6. Metrics</h2>
Top-1 Err, AP, AP$_{50}$, AP$_{75}$, AP$_{S}$, AP$_{M}$, AP$_{L}$ and mIoU.
<h2>7. Code</h2>
https://github.com/whai362/PVT.