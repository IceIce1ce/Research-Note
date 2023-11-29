<h2>1. Abstract</h2>
ResNeXt is constructed by repeating a building block that aggregates a set of transformations with same topology. This strategy exposes a new dimensions, named cardinality (the size of set of transformations) as an essential factor in addition to the dimensions of depth and width. Increasing cardinality is able to improve classification accuracy.
<h2>2. Introduction</h2>
Present a simple architecture which adopts VGG/ResNets' strategy of repeating layers, while exploiting the split-transform-merge strategy in an easy, extensible way. A module in network performs a set of transformations, each on a low-dimensional embedding, whose outputs are aggregated by summation.
<h2>3. Related work</h2>
<h3>3.1 Multi-branch convolutional networks</h3>
ResNets and Deep neural decision forests.
<h3>3.2 Grouped convolutions</h3>
AlexNet. A special case of grouped convolutions is channel-wise convolutions (number of groups = number of channels). Channel-wise convolutions are part of the separable convolutions.
<h3>3.3 Compressing convolutional networks</h3>
Decomposition spatial and/or channel level.
<h3>3.4 Ensembling</h3>
Averaging a set of independently trained networks.
<h2>4. Method</h2>
<h3>4.1 Template</h3>
ResNeXt consists of a stack of residual blocks. These blocks have the same topology and subject to two simple rules: 1) if producing spatial maps of same size, blocks share same hyper-parameters (width and filter sizes) and 2) each time when spatial map is downsampled by a factor of 2, the width of blocks is multiplied by a factor of 2.
<h3>4.2 Revisiting simple neurons</h3>
Inner product can be though of as a form of aggregating transformation: $\sum_{i = 1}^Dw_ix_i$, where x = $[x_1, x_2,...,x_D]$ is a $D$-channel input vector to neuron and $w_i$ is a filter's weight for $i$-th channel. The above operation can be recast as a combination of splitting, transforming and aggregating.
<h3>4.3 Aggregated transformations</h3>
$\mathcal{F}(\mathrm{x}) = \sum_{i = 1}^C\mathcal{T}_i(\mathrm{x})$, where $\mathcal{T}_i$(x) can be arbitrary function. $C$ is the size of the set of transformations to be aggregated. C is in a position similar to $D$ but need not equal and can be an arbitrary number. Set $\mathcal{T}_i$ to be the bottleneck-shaped architecture. The aggregated transformation serves as the residual function: y = x + $\sum_{i = 1}^C\mathcal{T}_i$(x), where y is the output.
<h2>5. Datasets</h2>
ImageNet-1K, ImageNet-5K, CIFAR-10, CIFAR-100 and COCO.
<h2>6. Metrics</h2>
Top-1 Err, Top-5 Err, AP@0.5 and AP.
<h2>7. Code</h2>
https://github.com/prlz77/ResNeXt.pytorch.