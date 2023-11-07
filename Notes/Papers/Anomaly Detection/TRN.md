<h2>1. Abstract</h2>
Propose TRN to model greater temporal context of each frame. The approach makes use of both accumulated historical evidence and predicted future information to better recognize the action that is currently occurring, and integrates both of these into a unified end-to-end architecture.
<h2>2. Online action detection</h2>
For each frame $I_t$ of an image sequence, a probability distribution $p_t = [p^0_t,...,p^K_t]$ over $K$ possible actions, given only past and current frames {$I_1,...,I_t$}, where $p^0_t$ denotes background probability that no action is occurring.
<h3>2.1 TRN</h3>
The core of the network is TRN cell. At each time $t$ a TRN cell receives a feature vector $x_t$ corresponding to the observation at time $t$, which could include some combination of evidence from the appearance or motion in frame $I_t$ and the hidden state $h_{t - 1}$ from the previous time step. The cell then outputs $p_t$, a probability distribution estimating which action is happening in $I_t$. The hidden state $h_t$ is then updated and used for estimating next time step.
<h3>2.2 TRN cell</h3>
It uses a temporal decoder, a future gate, and a spatiotemporal accumulator (STA). Use LSTMs as the backbone for both e temporal decoder and the STA.
- Temporal decoder: works sequentially to output the estimates of future actions and their corresponding hidden states $\{\overline{h}^0_t,...,\overline{h}^{l_d}_t\}$ for the next $l_d$ time steps, where $h^i_t$ for $i \in [0, l_d]$ indicates hidden state at $i$-th time step after $t$. The input to the decoder at the first time step is all zeros. At other time steps $t$, feed in the predicted action scores $\overline{r}^{i - 1}_t$, embedded by a linear transformer.
- The future gate: takes hidden states from the decoder. Default future gate is an average pooling operator followed by an FC layer. More formally, the future context feature $\overline{x}_t$ is obtained by averaging and embedding the hidden state vector $\overline{h}_t$, gathered from all decoder steps: $\overline{x}_t = RELU(W^T_fAvgPool(\overline{h}_t) + b_f)$,
- The spatiotemporal accumulator: takes the previous hidden state $h_{t - 1}$ as well as concatenation of image feature $x_t$ extracted from $I_t$ and predicted feature $\overline{x}_t$ from future gate and updates its hidden states $h_t$. It then calculates a distribution over candidate actions: $p_t = softmax(W^T_ch_t + b_c)$. Combine the accumulator and decoder losses during training: $\sum_{t}(loss(p_t, l_t) + \alpha\sum_{i = 0}^{l_d}loss(\overline{p}^i_t, l_{t + 1}))$, where $\overline{p}^i_t$ indicates the action probabilities predicted by the decoder for step $i$ after time $t$, $l_t$ represents the ground truth, loss denotes cross-entropy loss and $\alpha$ is a scale factor.
<h2>3. Datasets</h2>
HDD, TVSeries and Thumos'14.
<h2>4. Metrics</h2>
mAP and cAP.
<h2>5. Code</h2>
https://github.com/xumingze0308/TRN.pytorch.