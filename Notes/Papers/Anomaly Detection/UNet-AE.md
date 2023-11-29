<h2>1. Abstract</h2>
Propose a scene-independent robust unsupervised video anomaly detection method based on future frame prediction as a breakthrough and better video anomaly detection technique. This framework can be applied to all cases regardless of the scene environment.
<h2>2. Model</h2>
Model consists of a deep learning model with convolutional layers and four components: the Encoder, Decoder O, Decoder F, and the Discriminator. Encoder and Decoder O are defined as Generator O ($G_O$). Encoder and Decoder F are defined as Generator F ($G_F$). $G_O$ and $G_F$ are trained only on normal video scenes to help detect objects that deviate from normal.
<h3>2.1 Generator O</h3>
$G_O$ is an STNs consisting of Encoder and Decoder O. The Encoder extracts spatio-temporal features from the input video image using Convolutional-LSTM. Decoder O uses the features extracted by the encoder to predict the future optical flow of one frame of the video image using a deconvolution layer. $G_O$ has a skip structure similar to that of Pix2Pix.
<h3>2.2 Generator F</h3>
$G_F$ is an STNs consisting of the Encoder and Decoder F, which predicts the image of one frame in the future. Use U-Net for $G_F$ instead of RNN. Both $G_O$ and $G_F$ incorporate ConvLSTM in their encoder to help model spatio-temporal information.
<h3>2.3 Discriminator</h3>
Use U-Net as the discriminator to identify at the pixel level whether the image is a “true” image sampled from the dataset or a “false” frame image predicted by the generator.
<h3>2.4 Training model</h3>
Spatio-temporal adversarial networks is optimized by alternately minimizing the following two losses: $L_G$ and $L_D$. $L_G = \lambda_f L_{frame} + \lambda_O L_{opt} + L^G_{Denc} + L^G_{D_{dec}}, L_D = L_{D_{enc}} + L_{D_{dec}} + \lambda_C L_{consist}$. Here, $G$ represents generator, $D_{enc}$ and $D_{dec}$ represent Encoder and Decoder modules of Discriminator. $L_{frame}$ and $L_{opt}$ are the prediction loss of the frame image and the optical flow between the ground truth and the prediction result. $L_{frame} = ||\xi_{t + 1} - G_F(x_t)||_1, L_{opt} = ||O_{t + 1} - G_O(x_t)||_1, L^G_{D_{enc}} = -\mathbb{E}_{\xi \sim p\xi}[\log(1 - D_{enc}([\xi_{t + 1}, o_{t + 1}]))] - \mathbb{E}_{x \sim px}[\log(D_{enc}([G_F(x_t), o_{t + 1}]))]$, $L^G_{D_{dec}} = -\mathbb{E}_{\xi \sim p\xi}\sum_{i, j}\log[1 - D_{dec}([\xi_t, o_{t + 1}])]_{i, j}] - \mathbb{E}_{x \sim px}\sum_{i, j}\log[D_{dec}([G_F(x_t), o_{t + 1}])]_{i, j}]$, where input $x_t$ consists of partial time series $\xi_{t - T - 1},...,\xi_t$ with fixed length $T$ at some time $t$. $\xi$ is the image of each frame. $o_{t + 1}$ is the optical flow at time $t + 1$. $[D_{dec}(\xi_{t + 1}, o_{t + 1})]_{i, j}$ and $[D_{dec}(G_F(x_t), o_{t + 1})]_{i, j}$ represent the output result of Discriminator at pixel $(i, j)$. $[\xi_{t + 1}, o_{t + 1}$] means the combination in channel direction.
<h3>2.5 Anomaly detection</h3>
The anomaly degree $a(x_t)$ for input $x_t$ to the model at each frame $t$ is defined: $a(x_t) = |||\xi_{t + 1} - G_F(x_t)| \odot |o_{t + 1} - G_O(x_t)| \odot D_{dec}([\xi_{t + 1}, o_{t + 1}])map||_1$. The final score is defined: $S(x_t) = \frac{a(x_t)}{max(a(x_1),...,a(x_m))'}$, where $m$ is the total number of test data.
<h2>3. Datasets</h2>
UCSD Ped2, Avenue and RetroTrucks.
<h2>4. Metrics</h2>
AUROC.