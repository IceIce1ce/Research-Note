<h2>1. Abstract</h2>
Integrate the reconstruction-based functionality into a novel self-supervised predictive architectural building block. The proposed self-supervised block is generic and can easily be incorporated into various state-of-the-art anomaly detection methods. The block starts with a convolutional layer with dilated filters, where the center area of the receptive field is masked. The resulting activation maps are passed through a channel attention module. The block is equipped with a loss that minimizes the reconstruction error with respect to the masked area in the receptive field.
<h2>2. Method</h2>
SSPCAB is composed of a masked convolutional layer activated by Rectified Linear Units (ReLU), followed by a Squeeze-and-Excitation (SE) module.
<h3>2.1 Masked convolution</h3>
The learnable params of masked conv are located in corners of receptive filed, being denoted by sub-kernels $K_i \in \mathbb{R}^{k' \times k' \times c}, \forall i \in \{1, 2, 3, 4\}$, where $k' \in \mathbb{N}^+$ is a hyperparameter defining sub-kernel size and $c$ is number of input channels. Each kernel $K_i$ is located at a distance (dilation rate) $d \in \mathbb{N}^+$ from masked region in center of receptive field which is denoted by $M \in \mathbb{R}^{1 \times 1 \times c}$. To predict a value for every channel in $M$, introduce a number of $c$ masked conv filters, each predicting masked information from a distinct channel. Also add zero-padding of $k' + d$ pixels around input and set stride to 1, such that every pixel in the input is used as masked information. Finally, the output tensor is passed through a ReLU activation. 
<h3>2.2 Channel attention module</h3>
Employ the channel attention module.
<h3>2.3 Reconstruction loss</h3>
Define self-supervised reconstruction loss as MSE: $\mathcal{L}_{SSPCAB}(G, X) = (G(X) - X)^2 = (\hat{X} - X)^2$. $\mathcal{L}_{total} = \mathcal{L}_F + \lambda \cdot \mathcal{L}_{SSPCAB}$.
<h2>3. Datasets</h2>
MVTec AD, Aveue and ShanghaiTech.
<h2>4. Metrics</h2>
AP, AUROC and AUC.
<h2>5. Code</h2>
https://github.com/ristea/sspcab.