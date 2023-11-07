<h2>1. Abstract</h2>
Use latent code predictions as our anomaly metric. By decoupling an appearance and a motion model, the model can also process 16 to 45 times more frames per second than related approaches.
<h2>2. Architecture</h2>
<h3>2.1 Learning appearance features</h3>
Use a U-Net type autoencoder that is trained to reconstruct individual input frames. To force the model to focus on the foreground, subtract a background frame from each input frame. This background frame is calculated as a frame with per-pixel mean RGB values over all training data. Add a shortcut connection between the previous frame $T_{k - 1}$ and current frame $T_K$. 
<h3>2.2 Learning motion features</h3>
To predict the latent code of frame $T_t$, extract latent codes for $k$ previous input frames $T_1...T_k$. The extracted latent codes $z_1...z_k$ from the encoder for each of past frames $T_1...T_k$ are then concatenated along the temporal dimension and used as the input for the motion model to predict the latent code $\hat{z}_t$ for a future frame $T_t$. Use 3D convolutional layers in the motion model. Each convolutional block includes a 3D convolutional layer with kernel size $3 \times 3 \times 3$, stride 2 on the temporal dimension and stride 1 on the feature dimension. This is followed by a BatchNormalization and a leaky-relu. Use 3 convolutional blocks in the motion model.
<h3>2.3 Training</h3>
$\mathcal{L} = \lambda_r\sum_{q = 2}^k\sum_{j = 1}^N||\hat{T}_{q, j} - T_{q, j}||^2_2 + \lambda_p\sum_{m - 1}^M||\hat{z}_{t, m} - z_{t, m}||^2_2 + \gamma||W||^2_2$. The first term measures the pixel-wise reconstruction loss where $N$ is the total number of pixels per frame and $k$ is the number of frames in a input sequence. The second term is the MSE between the predicted and the observed latent code where $M$ is the number of elements in the latent code. The last term is an L2 regularization term where $\gamma = 0.001$. For training, $\lambda_r = 1, \lambda_p = 0.001$. For finetune, $\lambda_r = 0.001, \lambda_p = 1.0$.
<h3>2.4 Inference</h3>
$s_t = \frac{\sum_{m = 1}^M||\hat{z}_{t, m} - z_{t, m}||^2}{M}$. After calculating the anomaly score for each frame, normalize the score for each frameset $\mathcal{G}$ to the range of [0, 1]. $s_t = \frac{s_t - min_{j \in \mathcal{G}}(s_j)}{max_{j \in \mathcal{G}}(s_j) - min_{j \in \mathcal{G}}(s_j)}$.
<h2>3. Datasets</h2>
UCSD Pedestrian, CUHK Avenue and ShanghaiTech.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/lyn1874/daml.