<h2>1. Abstract</h2>
Propose a fully convolutional masked autoencoder framework and a new global response normalization layer that can be added to ConvNeXt architecture to enhance inter-channel feature competition.
<h2>2. Introduction</h2>
Propose to co-design the network architecture and masked autoencoder under the same framework, with the aim of making mask-based self-supervised learning effective for ConvNeXt models and achieving results similar to those obtained using transformers. In designing the masked autoencoder, treat masked input as a set of sparse patches and use sparse convolutions to process only visible parts. To further improve pre-training efficiency, replace transformer decoder with a single ConvNeXt block, making entire design fully convolutional. Identify a ponential issue of feature collapse at MLP layer when training ConvNeXt directly on masked input --> propose adding a global response normalization layer to enhance inter-channel feature competition.
<h2>3. Related work</h2>
<h3>3.1 ConvNets</h3>
UnNAS, ConvNeXt.
<h3>3.2 Masked autoencoders</h3>
Original masked autoencoders are not directly applicable to ConvNets due to their asymmetric encoder-decoder design. 
<h2>4. Fully convolutional masked autoencoder</h2>
The learning signals are generated by randomly masking the raw input visuals with a high masking ratio and letting the model predict the missing parts given the remaining context.
<h3>4.1 Masking</h3>
Use a random masking strategy with a masking ratio of 0.6. As the convolutional model has a hierarchical design, where features are downsampled in different stages, the mask is generated in the last stage and upsampled recursively up to the finest resolution. In practice, randomly remove 60% of 32 $\times$ 32 patches from original input image. Use minimal data augmentation, only including random resized cropping.
<h3>4.2 Encoder design</h3>
Use ConvNext model as encoder. View the masked image from a sparse data perspective which was inspired by learning on sparse point clouds in 3D tasks. During pre-training, propose to convert standard convolution layer in the encoder with submanifold sparse convolution which enables the model to operate only on the visible data points.
<h3>4.3 Decoder design</h3>
Use plain ConvNeXt block as the decoder. This forms an asymmetric encoder-decoder architecture overall, as the encoder is heavier and has a hierarchy. 
<h3>4.4 Reconstruction target</h3>
Compute MSE between reconstructed and target images. Similar to MAE, target is a patch-wise normalized image of original input and loss is applied only on masked patches.
<h2>5. Global response normalization</h2>
<h3>5.1 Feature collapse</h3>
First perform qualitative analysis in the feature space. Visualize the activations of a FCMAE pre-trained ConvNeXt-base model and notice an intriguing feature collapse phenomenon: there are many dead or saturated feature maps and the activation becomes redundant across channels.
<h3>5.2 Feature cosine distance analysis</h3>
Perform a feature cosine distance analysis. Given an activation tensor $X \in R^{H \times W \times C}, X_i \in R^{H \times W}$ is the feature map of $i$-th channel. Reshape it as a $HW$ dimensional vectir and compute average pair-wise cosine distance across the channels by $\frac{1}{C^2}\sum_j^C\frac{1 - \cos(X_i, X_j)}{2}$. A higher distance value indicates more diverse features. Randomly select 1000 images from different classes in ImageNet-1K validation set and extract high-dimensional features from each layer of different models, including FCMAE models, the ConvNeXt model and MAE pre-trained ViT model. Then compute the distance per layer for each image and average values across all images.
<h3>5.3 Approach</h3>
GRN increase the contrast and selectivity of channels. Given an input feature, $X \in R^{H \times W \times C}$, the proposed GRN unit consists of three steps: 1) global feature aggregation, 2) feature normalization and 3) feature calibration. First, aggregate a spatial feature map $X_i$ into a vector $gx$ with a global function $\mathcal{G}(\cdot) := X \in \mathcal{R}^{H \times W \times C} \rightarrow gx \in \mathcal{R}^C$. This gives us a set of aggregated values $\mathcal{G}(X) = gx = \{||X_1||,...,||X_C||\} \in \mathcal{R}^C$, where $\mathcal{G}(X)_i = ||X_i||$ is a scalar that aggregates the statistics of i-th channel. Next, apply a response normalization function $\mathcal{N}(\cdot)$ to the aggregated values. Concretely, use a standard divisive normalization: $\mathcal{N}(||X_i||) := ||X_i|| \in \mathcal{R} \rightarrow \frac{||X_i||}{\sum_{j=1,...,C}||X_j||} \in \mathcal{R}$, where $||X_i||$ is L2-norm of $i$-th channel. Finally, calibrate original input responses using computed feature normalization scores: $X_i = X_i * \mathcal{N}(\mathcal{G}(X)_i) \in \mathcal{R}^{H \times W}$. To ease optimization, add two additional learnable params $\gamma$ and $\beta$ and initialize them to zero. Also add a residual connection between input and output of GRN layer. The resulting final GRN block is $X_i = \gamma * X_i * \mathcal{N}(\mathcal{G}(X)_i) + \beta + X_i$.
<h3>5.4 ConvNeXt V2</h3>
Incorporate GRN layer into original ConvNeXt block. Found that LayerScale in ConvNeXt becomes unnecessary when GRN is applied and can be removed.
<h2>6. Datasets</h2>
ImageNet-1K, ImageNet-22K, COCO and ADEK.
<h2>7. Metrics</h2>
Val acc, FT acc, AP$^{\mathrm{box}}$, AP$^{\mathrm{box}} _{50}$, AP$^{\mathrm{box}}_{75}$, AP$^{\mathrm{mask}}$, AP$^{\mathrm{mask}}_{50}$, AP$^{\mathrm{mask}}_{75}$ and mIoU.
<h2>8. Code</h2>
https://github.com/facebookresearch/ConvNeXt-V2.