<h2>1. Abstract</h2>
Propose a temporal augmented network to learn motion-aware feature. Furthermore, incorporate temporal context into MIL ranking model by using an attention block.
<h2>2. Methodology</h2>
<h3>2.1 Temporal augmented network</h3>
PWCNet is used to compute OF between adjacent frames. Choose 16 frames as a video clip and resize them to a resolution of $112 \times 112$. Then, compute OF on the resized frames. The final input to temporal augmented network is a stack of 15 OF maps with the dimension of $30 \times 112 \times 112$. Design the temporal augmented network to have only 7 layers: 3 encoder layers, 1 bottleneck layer and 3 decoder layers. All layers consist of a 2D convolutional layer followed by a ReLU activation. Use a stride of 2 to halve the feature map resolution instead of pooling. The network can be trained using L1 per-pixel reconstruction loss: $loss_{recond} = |\mathcal{F} - \overline{\mathcal{F}}$. For each video clip with 16 frames, perform a forward pass until the bottleneck layer and conduct a global average pooling operation to derive a $1024 \times 1$ feature.
<h3>2.2 Attention-based temporal MIL ranking model</h3>
$\sum_{i \in \mathcal{B}_a}w_if(\mathcal{V}^i_a) > \sum_{i \in \mathcal{B}_n}w_if(\mathcal{V}^i_n)$, where $w_i$ indicates the learned attention weights. The block consists of three fully-connected layers and two tanh activations in between. The first fully-connected layer has 256 units followed by 64 unit and 1 unit fully-connected layers. $l(\mathcal{B}_a, \mathcal{B}_n) = max(0, 1 - \sum_{i \in \mathcal{B}_a}w_if(\mathcal{V}^i_a) + \sum_{i \in \mathcal{B}_n}w_if(\mathcal{V}^i_n))$. The final loss is: $Loss = l(\mathcal{B}_a, \mathcal{B}_n) + \lambda_1\sum_{i \in \mathcal{B}_a}w_if(\mathcal{V}^i_a)$.
<h2>3. Datasets</h2>
UCF-Crime.
<h2>4. Metrics</h2>
AUC.