<h2>1. Abstract</h2>
This article reviews the state-of-the-art deep learning based methods for video anomaly detection and categorizes them based on the type of model and criteria of detection. Perform simple studies to understand the different approaches and provide the criteria of evaluation for spatio-temporal anomaly detection.
<h2>2. Introduction</h2>
<h3>2.1 Anomaly detection</h3>
The normal class distribution $\mathcal{D}$ is estimated using training samples $x_i \in X_{train}$, by building a representation $f_\theta: X_{train} \rightarrow \mathbb{R}$ which minimizes model prediction loss error over all training samples, over all $i$: $\theta^* = \mathrm{argmin}_\theta \sum_{x_i \in X_{train}} L_\mathcal{D}(\theta; x_i) = \mathrm{argmin}_\theta \sum_{x_i \in X_{train}} ||f_\theta(x_i) - x_i||^2$. Now the deviation of test samples $x_j \in X_{test}$ under this representation $f_{\theta^*}$ is evaluated as anomaly score, $\alpha(x_j) = ||f_{\theta^*}(x_j) - x_j||_2$ is used as a measure of deviation. Detection is achieved by evaluating a threshold on anomaly score $\alpha_j > T_{thresh}$.
<h3>2.2 Datasets</h3>
UCSD, Avenue, Subway, UMN, Train, Queen Mary University of London U-turn and LV.
<h2>3. Taxonomy</h2>
- Representation learning for reconstruction: PCA or AEs are used to represent the different linear and non-linear transformations to the appearance (image) or motion (flow) that model the normal behavior in surveillance videos.
- Predictive modeling: video frames are viewed as temporal patterns or time series and the goal is to model conditional distribution $P(x_t | (x_{t - 1},...,x_{t - p}))$. The goal is to predict current frame or its encoded representation using past frames.
- Generative models: VAE, GAN or AAE are used for the purpose of modeling likelihood of normal video samples in an end-to-end deep learning framework.
<h2>4. Reconstruction models</h2>
PCA, Autoencoders, Convolutional AutoEncoders, CAEs for video anomaly detection, Contractive AutoEncoders, De-noising AutoEncoders, Stacked DAEs and Deep belief networks. 
<h2>5. Predictive modeling</h2>
Compsite model: reconstruction and prediction, Convolutional LSTM, 3D AutoEncoder and Predictor and Slow Feature Analysis.
<h2>6. Deep generative models</h2>
Generative vs discriminative, VAEs, Anomaly detection using VAE, GANS, GANs for anomaly detection in images, Adversarial discriminators using cross-channel prediction, Adversarial AutoEncoders and controlling reconstruction for anomaly detection.
<h2>7. Metrics</h2>
TPR, Recall, FPR, Precision, AUROC and AUPR.