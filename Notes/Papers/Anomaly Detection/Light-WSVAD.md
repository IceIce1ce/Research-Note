<h2>1. Abstract</h2>
Propose an adaptive instance selection strategy, which is based on the model’s current status to select confident instances, thereby mitigating the uncertainty of weakly labeled data. Design a lightweight multi-level temporal correlation attention module and an hourglass-shaped fully connected layer to construct the model.
<h2>2. Methodology</h2>
<h3>2.1 Multi-level temporal correlation attention</h3>
First use a global average pooling layer to convert the $T$ instance features in a bag into a $T$-dimensional vector representing $T$ channels. Then, evaluate crosschannel interactions via convolution, and ultimately determine the weight of each channel. Use a one-dimensional convolution Conv1D of kernel size $k ( k \geq 3)$ to capture the cross-channel interactions of adjacent instances. In addition, in order to capture the interaction information between adjacent channels at different time spans, jointly use convolution kernels of different sizes. The calculation process is as follows: $T_{attention} = (Conv1D[k], Conv1D[k - 2]...Conv1D[3]) * G(\mathcal{X})$, where $G(\mathcal{X})$ is the feature vector after global pooling. Finally, by feeding $T_{attention}$ into activation layer LeakReLU and performing a concatenation operation with the original input $G(\mathcal{X})$, the attention module outputs $Y_{attention} = \lambda_1Sum(L\_Relu(T_{attention})) \cdot \mathcal{X}$, where $\lambda_1$ = 0.1.
<h3>2.2 Hourglass-shaped FC layer</h3>
HFC has a 2048-64-128 structure, in contrast to the traditional FC’s 2048-128-64.
<h3>2.3 Adaptive instance selection</h3>
- Step 1: $\omega = 1 - \frac{1}{T}\sum_{i = 1}^TS^N_i - \frac{1}{2T - 2}\sum_{i = 1}^{T - 1}(|S^N_{i + 1} - S^N_i| + |S^P_{i + 1} - S^P_i|)$, where $S^P, S^N$ are scores of positive and negative instances.
- Step 2: $K = \omega * \sum_{i = 1}^Tf(S^P_i)$, where $f(\cdot)$ is 1 if $S^P_i \geq 0.9$ otherwise is 0.
- Step 3: $loss_{AIS} = \sum_{\mathcal{X} \in U_{\mathcal{X}_{top-K}}}(ylog(mean(s_\theta(\mathcal{X}))) + (1 - y)log(1 - mean(s_\theta(\mathcal{X})))$, where $s_\theta$ is the feature extraction part of the model, $U$ is the set of video instance (clip) features extracted by I3D and $U_{\mathcal{X}_{top-K}}$ is the set of features of the top-K instances selected from the positive and negative bag.
<h3>2.4 Antagonistic loss function</h3>
Design a new smooth loss as follows: $loss_{smooth} = \frac{1}{T - 1}\sum^{T - 1}_{i = 1}(||S_{i + 1} - S_i||^2)$. $loss_{antagonistic} = [1 - (S^P_{top-1} - S^N_{top-1})] + S^N_{top-1} + (1 - S^P_{top-1})$, where $S^P_{top-1}, S^N_{top-1}$ represent the highest (top-1) abnormality score of instances in the positive and negative bags predicted by the model. The total loss: $loss_{all} = loss_{AIS} + loss_{smooth} + loss_{antagonistic}$.
<h2>3. Datasets</h2>
ShanghaiTech and UCF-Crime.
<h2>4. Metrics</h2>
AUC.