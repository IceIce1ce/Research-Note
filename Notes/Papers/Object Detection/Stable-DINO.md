<h2>1. Abstract</h2>
Integrating positional metrics to DETR's classification loss and matching cost named position-supervised loss and position- modulated cost.
<h2>2. Stable matching</h2>
<h3>2.1 Revisit DETR losses and matching costs</h3>
The final losses in DINO are composed of three parts, a classification loss, a box L1 loss, and a GIOU loss. $L_{cls} = \sum_{i = 1}^{N_{pos}}|1 - p_i|^{\gamma}\mathrm{BCE}(p_i, 1) + \sum_{i = 1}^{N_{neg}}p_i^{\gamma}\mathrm{BCE}(p_i, 0)$, where $N_{pos}$ and $N_{neg}$ are number of positive and negative examples, $p_i$ is the predicted probability of $i^{th}$ example and $\gamma$ is a hyperparameter of focal losses. To assign predictions with ground truths, first calculate a cost matrix $C \in \mathbb{R}^{N_{pred} \times N_{gt}}$ between them. The $N_{pred}$ and $N_{gt}$ are number for predictions and ground truths. Then a Hungarian matching algorithm will perform on cost matrix. The final cost includes three items, a classification cost, a box L1 cost and a GIOU cost. $C_{cls}(i, j) = |1 - p_i|^{\gamma}\mathrm{BCE}(p_i, 1) - p_i^{\gamma}\mathrm{BCE}(1 - p_i, 1)$,
<h3>2.2 Position-supervised loss</h3>
$\mathcal{L}^{(new)}_{cls} = \sum_{i = 1}^{N_{pos}}(|f_1(s_i) - p_i|^{\gamma}\mathrm{BCE}(p_i, f_1(s_i)) + \sum_{i = 1}^{N_{neg}}p_i^{\gamma}\mathrm{BCE}(p_i, 0)$. Use the $s_i$ as a positional metric like IOU between $i^{th}$ ground truth and its corresponding prediction.
<h3>2.3 Position-modulated matching</h3>
$C^{(new)}_{cls}(i, j) = |1 - p_i f_2(s'_i)|^{\gamma}\mathrm{BCE}(p_i f_2(s'_i), 1) - (p_i f_2(s'_i))^{\gamma}\mathrm{BCE}(1 - p_i f_2(s'_i), 1)$, $s'_i$ is another positional metric which use a rescaled GIOU. As GIOU ranges from [-1, 1], shift and rescale it to [0, 1] as a new metric. $f_2$ is another function to tune, $f_2(s'_i) = (s'_i)^{0.5}$.
<h2>3. Datasets</h2> 
COCO 2017.
<h2>4. Metrics</h2>
AP, AP$_{\mathrm{50}}$, AP$_{\mathrm{75}}$, AP$_{S}$, AP$_{M}$, AP$_{L}$, Mask AP, Box AP, AP$^{\mathrm{mask}}_{50}$, AP$^{\mathrm{mask}}_{75}$, AP$^{\mathrm{mask}}_{S}$, AP$^{\mathrm{mask}}_{M}$ and AP$^{\mathrm{mask}}_{L}$.
<h2>5. Code</h2>
https://github.com/IDEA-Research/Stable-DINO.