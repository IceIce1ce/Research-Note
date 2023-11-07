<h2>1. Abstract</h2>
Given an input, MemAE firstly obtain the encoding from the encoder and then uses it as a query to retrieve the most relevant memory items for reconstruction. At the training stage, the memory contents are updated and are encouraged to represent the prototypical elements of the normal data. At the test stage, the learned memory will be fixed and the reconstruction is obtained from a few selected memory records of the normal data.
<h2>2. Memory-augmented autoencoder</h2>
<h3>2.1 Overview</h3>
MemAE model consists of 3 major components: an encoder, a decoder and a memory module. The encoder first obtains encoding of input. By using encoded representation as a query, the memory module retrieves the most relevant items in memory via attention-based addressing operator which are then delivered to decoder for reconstruction.
<h3>2.2 Encoder and decoder</h3>
Let $f_e(\cdot): \mathbb{X} \rightarrow \mathbb{Z}$ denote the coder and $f_d(\cdot): \mathbb{Z} \rightarrow \mathbb{X}$  denote the decoder. z = $f_e(x, \theta_e), \hat{x} = f_d(\hat{z}, \theta_d)$, where $\theta_e$ and $\theta_d$ denote the parameters of encoder $f_e(\cdot)$ and decoder $f_d(\cdot)$. 
<h3>2.3 Memory module with attention-based sparse addressing</h3>
<h4>2.3.1 Memory-based representation</h3>
The memory is designed as a matrix $\mathrm{M} \in \mathbb{R}^{N \times C}$ containing $N$ real-valued vectors of fixed dimension $C$. Assume $C$ is same to dimension of z and let $\mathbb{Z} = \mathbb{R}^C$. Let the row vector $\mathrm{m}_i, \forall_i \in [N]$ denote the $i$-th row of M, where $[N]$ denotes the set of integers from 1 to $N$. Each m$_i$ denotes a memory item. Given a query z $\in \mathbb{R}^C$, the memory network obtains $\hat{z}$ relying a soft addressing vector $w \in \mathbb{R}^{1 \times N}$ as: $\hat{z} = \mathrm{wM} = \sum_{i = 1}^N w_im_i$, where w is a row vector with non-negative entries that sum to one and $w_i$ denotes the $i$-th entry of w. The weight vector w is computed according to z.
<h4>2.3.2 Attention for memory addressing</h4>
$w_i = \frac{\exp(d(z, m_i))}{\sum_{j = 1}^N \exp(d, z, m_j))}$, where $d(z, m_i) = \frac{zm^T_i}{||z||||m_i||}$.
<h4>2.3.3 Hard shrinkage for sparse addressing</h4>
Considering that all entries in w are non-negative, $\hat{w}_i = \frac{\mathrm{max}(w_i - \lambda, 0) \cdot w_i}{|w_i - \lambda| + \epsilon}$, where max($\cdot$, 0) is also known as ReLU. and $\epsilon$ is a small positive scalar. Afterthat, re-normalize $\hat{w}$ by letting $\hat{w}_i = \hat{w}_i/||\hat{w}||_1, \forall_i$. The latent representation $\hat{z}$ will be obtained via $\hat{z} = \hat{w}\mathrm{M}$. 
<h3>2.4 Training</h3>
$E(\hat{w}^T) = \sum_{i = 1}^T -\hat{w}_i \cdot \log(\hat{w}_i), L(\theta_e, \theta_d, \mathrm{M}) = \frac{1}{T}\sum_{t = 1}^T(R(x^t, \hat{x}^t) + \alpha E(\hat{w}^t))$, where $\alpha$ = 0.0002, $R(x^t, \hat{x}^t) = ||x^t - \hat{x}^t||^2_2$ is the reconstruction error on each sample.
<h2>3. Datasets</h2>
UCSD, Avenue, ShanghaiTech, MNIST, CIFAR-10 and KDDCUP.
<h2>4. Metrics</h2>
AUC, Precision, Recall and F1 score.