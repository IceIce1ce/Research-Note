<h2>1. Abstract</h2>
Introduce a novel token mixing operator, RepMixer, a building block of FastViT, that uses structural reparameterization to lower the memory access cost by removing skip-connections in the network.
<h2>2. Introduction</h2>
Three key design principles: 1) use of RepMixer block to remove skip connections, 2) use of linear train-time overparameterization to improve accuracy, 3) use of large convolutional kernels to substitute self-attention layers in early stages.
<h2>3. FastViT</h2>
<h3>3.1 Reparameterizing skip connections</h3>
- RepMixer: For an input tensor $X$, the mixing block in the layer was implemented as: $Y = \mathrm{BN}(\sigma(\mathrm{DWConv}(X))) + X$. In RepMixer, rearrange the operations and remove the non-linear activation function as: $Y = \mathrm{DWConv}(\mathrm{BN}(X)) + X$. The main benefit is that is can be reparameterized at inference time to a single depthwise convolutional layer as: $Y = \mathrm{DWConv}(X)$. 
- Positional encodings: use conditional positional encodings that is dynamically generated and conditioned on the local neighborhood of the input tokens.
<h3>3.2 Linear train-time overparameterization</h3>
Replace all dense $k \times k$ convolutions with its factorized version, i.e. $k \times k$ depthwise followed by $1 \times 1$ pointwise convolutions. In order to increase capacity of the factorized layers, perform linear train-time overparameterization as MobileOne. Only overparameterize those layers that replace dense $k \times k$ convolutions with its factorized form. These layers are found in the convolutional stem, patch embedding and projection layers.
<h3>3.3 Large kernel convolutions</h3>
Introduce depthwise large kernel convolutions in FFN and patch embedding layers. FFN block has a structure similar to ConvNeXt block with a few key differences. Use Batch Normalization as opposed to Layer Normalization, as it can be fused with the preceding layer at inference.
<h2>4. Datasets</h2>
ImageNet-1K, FreiHAND, ADE20K and MS COCO.
<h2>5. Metrics</h2>
Top-1 Acc, PJ, PV, F@5, F@15, FPS, mIoU, AP$^b$, AP$^b_{50}$, AP$^b_{75}$, AP$^m$, AP$^m_{50}$ and AP$^m_{75}$.
<h2>6. Code</h2>
https://github.com/apple/ml-fastvit.