<h2>1. Abstract</h2>
Two spatio-temporal deep feature extractors have been applied in parallel on the training videos. These streams are then used to train a modified multiple instance learning-based classifier. Finally, a fuzzy aggregation is employed to fuse the anomaly scores. Additionally, two lightweight deep-learning classifiers have been used to substantiate the model’s efficacy for classifying fire and accident events.
<h2>2. Proposed work</h2>
$M_i(S_x) = $S/2$ if $x \leq a$, $S$ if $a < x \leq m$, $S/2$ if $x \geq b$. Here, $M_i$ is MIL score obtained from $i$th stream, $S_x$ is anomaly score of $x$th frame and $S$ is entire segment's anomaly score. The prediction is $F^i_{j, k} = \omega_1 * M_i[j - 1] + \omega_2 * M_i[j] + \omega_3 * M_i[j + 1]$. Finally, all three fused results are added: $D_k = softmax(F(i, k) + F(i + 1, k) + F(i + 2, k))$.
<h2>3. Datasets</h2>
UCF-Crime.
<h2>4. Metrics</h2>
AUC and FAR.