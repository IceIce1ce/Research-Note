<h2>1. Abstract</h2>
Present a new method that views object detection as a direct set prediction problem. DETR is a set-based global loss that forces unique predictions via bipartite matching and a transformer encoder-decoder architecture. Given a fixed small set of learned object queries, DETR reasons about the relations of objects and global image context to directly output the final set of predictions in parallel.
<h2>2. Introduction</h2>
DETR simplifies that detection pipeline by dropping multiple hand-designed components that encode prior knowledge like spatial anchors or NMS. It also doesn't require any customized layers.
<h2>3. Related work</h2>
<h3>3.1 Set prediction</h3>
The basic set prediction task is multilabel classification. Direct set prediction are postprocessing-free. They need global inference schemes that model interactions between all predicted elements to avoid redundancy. A general approach is to use auto-regressive sequence models such as RNN works. The loss is designed based on Hungarian algorithm to find a bipartite matching between ground-truth and prediction.
<h3>3.2 Transformers and parallel decoding</h3>
One of the advantages of attention-based models is their global computations and perfect memory, which makes them more suitable than RNNs on long sequences. Combine transformers and parallel decoding for trade-off between computational cost and ability to perform global computations required for set prediction.
<h2>4. The DETR model</h2>
<h3>4.1 Object detection set prediction loss</h3>
DETR infers a fixed-size set of $N$ predictions, in a single pass through the decoder where $N$ is set to be larger than the typical number of objects in an image. Let $y$ the ground truth set of objects and $\hat{y} = \{\hat{y}_i\}^N_{i = 1}$ the set of $N$ predictions. Consider $y$ also as a set of size $N$ padded with $\varnothing$ (no object). To find a bipartite matching between these two sets, search for a permutation of $N$ elements $\sigma \in \partial_N$ with lowest cost: $\hat{\sigma} = \mathrm{argmin}_{\sigma \in \partial_N}\sum_i^N \mathcal{L}_{\mathrm{match}}(y_i, \hat{y}_{\sigma(i)})$, where $\mathcal{L}_{\mathrm{match}}(y_i, \hat{y}_{\sigma(i)})$ is a pair-wise matching cost between ground truth $y_i$ and a prediction with index $\sigma(i)$. This optimal assignment is computed with the Hungarian algorithm. Each element $i$ of the ground truth set can be seen as a $y_i = (c_i, b_i)$ where $c_i$ is the target class label and $b_i \in [0, 1]^4$ is a vector that define ground truth box center coordinates and its height and width relative to the image size. For the prediction with index $\sigma(i)$, define probability of class $c_i$ as $\hat{p}_{\sigma(i)}(c_i)$ and the predicted box as $\hat{b}_{\sigma(i)}$. With these notations, define $\mathcal{L}_{\mathrm{match}}(y_i, \hat{y}_{\sigma(i)})$ as $-\mathbb{1}_{\{c_i \neq \varnothing\}}\hat{p}_{\sigma(i)}(c_i) + \mathbb{1}_{\{c_i \neq \varnothing\}}\mathcal{L}_{\mathrm{box}}(b_i, \hat{b}_{\sigma(i)})$. The second step is to compute the Hungarian loss for all pairs matched in the previous step. $\mathcal{L}_{\mathrm{Hungarian}}(y, \hat{y}) = \sum_{i = 1}^N[-\log \hat{p}_{\hat{\sigma}(i)} + \mathbb{1}_{c_i \neq \varnothing}\mathcal{L}_{\mathrm{box}}(b_i, \hat{b}_{\hat{\sigma}}(i))]$. In practice, down-weight the log-probability term when $c_i = \varnothing$ by a factor 10 to account for class imbalance. The box loss is defined as: $\lambda_{\mathrm{iou}}\mathcal{L}_{\mathrm{iou}}(b_i, \hat{b}_{\sigma(i)}) + \lambda_{\mathrm{L1}}||b_i - \hat{b}_{\sigma(i)}||_1$, where $\lambda_{\mathrm{iou}}, \lambda_{\mathrm{L1}} \in \mathbb{R}$ are hyperparameters.
<h3>4.2 DETR architecture</h3>
It contains three main components: a CNN backbone to extract a compact feature representation, an encoder-decoder transformer and a simple feed forward network that makes the final detection prediction.
<h4>4.2.1 Backbone</h4>
A CNN backbone generates a lower-resolution activation map $f \in \mathbb{R}^{C \times H \times W}$. Typical values are $C = 2048$ and $H, W = \frac{H_0}{32}, \frac{W_0}{32}$.
<h4>4.2.2 Transformer encoder</h4>
First, a 1 $\times$ 1 convolution reduces the channel dimension of the high-level activation map $f$ from $C$ to a smaller dimension $d$, creating a new feature map $z_0 \in \mathbb{R}^{d \times H \times W}$. The encoder expects a sequence as input, hence collapse the spatial dimensions of $z_0$ into one dimension, resulting in a $d \times HW$ feature map. Each encoder layer consists of a multi-head self-attention module and a FFN.
<h4>4.2.3 Transformer decoder</h4>
The decoder transforms $N$ embeddings of size $d$ using multi-head self and encoder-decoder attention mechanisms. These input embeddings are learnt positional encodings that refer to as object queries and similarly to the encoder, add them to the input of each attention layer. They are then decoded into box coordinates and class labels by a FFN, resulting $N$ final predictions.
<h4>4.2.4 FFNs</h4>
The final prediction is computed by a 3-layer perceptron with ReLU activation function and hidden dimension $d$ and a linear projection layer.
<h4>4.2.5 Auxiliary decoding losses</h4>
Add prediction FFNs and Hungarian loss after each decoder layer. All predictions FFNs share their params. Use an additional shared layer-norm to normalize the input to the prediction FFNs from different decoder layers.
<h2>5. Datasets</h2>
COCO 2017.
<h2>6. Metrics</h2>
AP, AP$_{50}$, AP$_{75}$, AP$_{\mathrm{S}}$, AP$_{\mathrm{M}}$, AP$_{\mathrm{L}}$.
<h2>7. Code</h2>
https://github.com/facebookresearch/detr.