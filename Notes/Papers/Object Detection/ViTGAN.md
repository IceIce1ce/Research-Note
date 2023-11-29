<h2>1. Abstract</h2>
Integrate the ViT architecture into GAN. Existing regularization methods for GANs interact poorly with self-attention, causing serious instability during training. To resolve this issue, introduce novel regularization techniques for training GANs with ViTs.
<h2>2. Introduction</h2>
In the discriminator, we revisit the Lipschitz property of self-attention and design an improved spectral normalization that enforces Lipschitz continuity.
<h2>3. Method</h2>
<h3>3.1 Enforcing Lipschitzness of transformer discriminator</h3>
Replace the dot product similarity with Euclidean distance and also tie the weights for the projection matrices for query and key in self-attention: Attention$_h(X)$ = softmax$(\frac{d(XW_q, XW_k)}{\sqrt{d_h}})XW_v$, where $W_q = W_k, W_q, W_k$ and $W_v$ are projection matrices for query, key and value, $d(\cdot, \cdot)$ is L2 distance and $\sqrt{d_h}$ is feature dimension for each head.  
<h3>3.2 Improved spectral normalization</h3>
The standard SN uses power iterations to estimate spectral norm of projection matrix for each layer in neural network. Then, it divides the weight matrix with the estimated spectral norm, so Lipschitz constant of the resulting projection matrix equals 1. Concretely, use the following update rule for spectral normalization where $\sigma$ computes standard spectral norm of weight matrices: $\overline{W}_{ISN}(W) := \sigma(W_{init}) \cdot W/\sigma(W)$. 
<h3>3.3 Overlapping image patches</h3>
For each border edge of patch, extend it by $o$ pixels, making the effective patch size (P + 2$o$) $\times$ (P + 2$o$).
<h3>3.4 Generator design</h3>
Propose a novel generator following the design principle of ViT. It consists of two components: a transformer block and an output mapping layer. 
<h3>3.5 Self-modulated LayerNorm</h3>
SLN(h$_l$, w) = SLN(h$_l$, MLP(z)) = $\gamma_l$(w) $\odot \frac{h_l - \mu}{\sigma} + \beta_l$(w), where $\mu$ and $\sigma$ track mean and variance of summed inputs within layer and $\gamma_l$ and $\beta_l$ compute adaptive normalization parameters controlled by latent vector derived from z. $\odot$ is the element-wise dot product.
<h3>3.6 Implicit neural representation from patch generation</h3>
Use an implicit neural representation to learn a continuous mapping from a patch embedding $y^i \in \mathbb{R}^D$ to patch pixel values $x^i_p \in \mathbb{R}^{P^2 \times C}$. $x^i_p = f_\theta(E_{fou}, y^i)$ where $E_{fou} \in \mathbb{R}^{P^2 \cdot D}$ is a fourier encoding of $P \times P$ spatial locations and $f_\theta(\cdot, \cdot)$ is a 2-layer MLP.
<h2>4. Datasets</h2>
CIFAR-10, LSUN bedroom, CelebA.
<h2>5. Metrics</h2>
FID, IS.
<h2>6. Code</h2>
https://github.com/wilile26811249/ViTGAN.