<h2>1. Abstract</h2>
A cross-epoch learning (XEL) strategy associated with a hard instance bank (HIB) is proposed to introduce additional information from previous training epochs. Two new losses are proposed for XEL to achieve a higher detection rate as well as a lower false alarm rate of anomaly events.
<h2>2. Methodology</h2>
$M$ hard negative instances i.e., clip features with the highest anomaly scores in each normal video, are selected to update the HIB ($\Omega \in R^{M \times d}$) with XEL strategy.
- Updating HIB: all the clips from normal videos are re-evaluated after each training epoch. The features of those hard instances with the highest scores are picked out: $h^{(t)}_i = argmax_{h^{(t)}_i \in [1, k_i]}(s^{(t)}_{i, 1},...,s^{(t)}_{i, k_i})$, where $h^{(t)}_i$ is index for highest score in $S^{(t)}_i$. The HIB is updated at the beginning of each training epoch: $\Omega^{(t + 1)} = \{f_{i, h^{(t)}_i}\}^M_{i = 1}$.
- Learning with HIB: at $l$-th iteration in ($t$ + 1)-th epoch, the anomaly score vector $\hat{S}^{(t + 1)}_l$ of features in HIB are calculated in every iteration: $\{\hat{s}^{(t + 1)}_{i, h^{(t)}_i, l}\}^M_{i = 1}$. A validation loss is defined as to penalize the hard negative instances: $L_v = \frac{1}{M}\sum_{i = 1}^M|\hat{s}^{(t + 1)}_{i, h^{(t)}_i, l} - y_{i, h^{(t)}_i}|$. Dynamic margin loss is defined: $L_m = \frac{1}{M}\sum_{i = 1}^Mmax(0, \epsilon - max(S^{(t + 1)}_a) + \hat{s}^{(t + 1)}_{i, h^t_i, l})$.
- Final loss function: $L = L_o + \lambda_1L_v + \lambda_2L_m$, where $L_o$ is loss function of any given WSAD framework.
<h3>3. Datasets</h3>
UCF-Crime and ShanghaiTech.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/sdjsngs/XEL-WSAD.