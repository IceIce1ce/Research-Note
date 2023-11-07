<h2>1. Abstract</h2>
Reexamine the design spaces and test limits of what a pure ConvNet can achieve. Gradually modernize a standard ResNet toward design of a vision Transformer and discover several key components that contribute to performance difference along the way. Constructed entirely from standard ConvNet modules ConvNeXts compete favorably with Transformers in terms of accuracy and scalability, achieving 87.8% ImageNet top-1 accuracy.
<h2>2. Introduction</h2>
Investigate architectural distinctions between ConvNets and Transformers and try to identify the confounding variables when comparing network performance. Start with ResNet50 trained with an improved procedure, gradually modernize architecture to the construction of a hierarchical vision Transformer. Propose a family of pure ConvNets dubbed ConvNeXt.
<h2>3. Modernizing a ConvNet: a Roadmap</h2>
Starting point is a ResNet-50 model. First train it with similar training techniques used to train vision Transformers and obtain much improved results compared to ResNet-50. Then, study a series of design decisions which summarized as 1) marco design, 2) ResNeXt, 3) inverted bottleneck, 4) large kernel size and 5) various layer-wise micro designs.
<h3>3.1 Training techniques</h3>
Use a training recipe that is close to DeiT's and Swin Transformer's. Training is extended to 300 epochs for ResNets. Use AdamW, Mixup, Cutmix, RandAugment, Random Erasing and regularization schemes including Stochastic Depth and Label Smoothing.
<h3>3.2 Macro design</h3>
<h4>3.2.1 Changing state compute ratio</h4>
Heavy 'res4' stage was meant to be compatible with downstream tasks like object detection, where a detector head operates on 14 $\times$ 14 feature plane. Adjust the number of blocks in each stage from (3, 4, 6, 3) in ResNet-50 to (3, 3, 9, 3) -> improve from 78.8% to 79.4%.
<h4>3.2.2 Changing stem to 'Patchify'</h4>
A common stem cell will aggressively downsample input images to an appropriate feature map size in both standard ConvNets and vision Transformers. The stem cell in standard ResNet contains a 7 $\times$ 7 convolution layer with stride 2, followed by a max pool, which results in a 4$\times$ downsampling of input images. In vision Transformers, a more aggressive 'patchify' strategy is used as stem cell, which corresponds to a large kernel size and non-overlapping convolution. Swin Transformer uses a similar 'patchify' layer but with a smaller patch size of 4 to accommodate the architecture's multi-stage design. Replace ResNet-style stem cell with a patchify layer implemented using a 4 $\times$ 4, stride 4 convolution layer. Accuracy changed from 79.4% to 79.5%.
<h3>3.3 ResNeXt-ify</h3>
Use depthwise convolution where the number of groups equals the number of channels. Depthwise convolution is similar to weighted sum operation in self-attention which operates on a per-channel basis. Combination of depthwise conv and 1 $\times$ 1 convs leads to a separation of spatial and channel mixing where each operation either mixes information across spatial or channel dimension, but not both. Increase network width to the same number of channels as Swin-T's (from 64 to 96). Accuracy changed from 79.5% to 80.5%.
<h3>3.4 Inverted bottleneck</h3>
Accuracy changed from 80.5% to 80.6%. In the ResNet-200/Swin-B, 81.9% to 82.6%.
<h3>3.5 Large kernel sizes</h3>
<h4>3.5.1 Moving up depthwise conv layer</h4>
MSA block is placed prior to MLP layers. As we have an inverted bottleneck block, the complex/inefficient modules will have fewer channels, while efficient, dense 1 $\times$ 1 layers will do the heavy lifting.
<h4>3.5.2 Increasing the kernel size</h4>
Experimented with several kernel sizes, including 3, 5, 7, 9 and 11. The network's performance increases from 79.9% (3 $\times$ 3) to 80.6% (7 $\times$ 7).
<h3>3.6 Micro design</h3>
<h4>3.6.1 Replacing ReLU with GELU</h4>
Accuracy stays unchanged (80.6%).
<h4>3.6.2 Fewer activation functions</h4>
Eliminate all GELU layers from residual block except for one between two 1 $\times$ 1 layers, replicating style of a Transformer block. Accuracy increased to 81.3%.
<h4>3.6.3 Fewer normalization layers</h4>
Remove two BN layers, leaving only one BN layer before conv 1 $\times$ 1 layers. Accuracy increased to 81.4%.
<h4>3.6.4 Subsituting BN with LN</h4>
Obtain an accuracy of 81.5%.
<h4>3.6.5 Separate downsampling layers</h4>
Use 2 $\times$ 2 conv layers with stride 2 for spatial downsampling. Adding normalization layers wherever spatial resolution is changed can help stablize training. These include several LN layers also used in Swin Transformers: one before each downsampling layer, one after stem and one after final global average pooling. Accuracy increased to 82.0%.
<h2>4. Datasets</h2>
ImageNet-1K, ImageNet-22K, COCO, ADE20K.
<h2>5. Metrics</h2>
Top-1 acc, AP$^{\mathrm{box}}$, AP$^{\mathrm{box}}_{50}$, AP$^{\mathrm{box}}_{75}$, AP$^{\mathrm{mask}}$, AP$^{\mathrm{mask}}_{50}$, AP$^{\mathrm{mask}}_{75}$.
<h2>6. Code</h2>
https://github.com/facebookresearch/ConvNeXt.