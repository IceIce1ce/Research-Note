<h2>1. Abstract</h2>
Conduct an in-depth investigation into the images/videos deep learning based AD methods. Discuss current challenges and future research directions thoroughly.
<h2>2. Introduction</h2>
Common weakness that AD algorithms suffer from are: 1) high false positive rate, 2) high computational cost, 3) unavailability of a standard dataset for assessment.
<h2>3. Problem formulation</h2>
There are $\mathcal{U}$ unlabeled images or video frames denoted as $X_\mathcal{N}, (x \in X_{\mathcal{N}} \sim p_{\mathcal{N}})$. AD can be considered as: AD$(\mathcal{F}(y))$ = Normal if $\mathcal{D}(\mathcal{F}(y), p_{\mathcal{N}}) \leq \tau$, otherwise is anomaly, where $\mathcal{D}$ is a metric to compute the distance between given instances and the distribution of normal data, $\mathcal{F}$ is a feature extractor that maps raw data to a set of discriminative features. According to the number of available normal ($\mathcal{N}$), anomalous ($\mathcal{A}$) and unlabeled ($\mathcal{U}$) samples in the training set, proposed methods can be categorized into: unsupervised, semi-supervised and unsupervised.
<h2>4. Supervision</h2>
<h3>4.1 Supervised</h3>
Gathering $\mathcal{N}$ normal and $\mathcal{A}$ anomalous samples for training a binary classifier is straightforward. A CNN can be learned on $\mathcal{N} + \mathcal{A}$ instances in a supervised setting to efficiently make a distinction. Their outcomes are not sufficiently generalized. The performance of deep supervised classifier is sub-optimal owing to the class imbalance.
<h3>4.2 Semi-supervised</h3>
Learn a model on copious unlabeled samples along with a few number of irregular and normal data ($\mathcal{N} + \mathcal{A} << \mathcal{U}$). In this technique, outliers are detected solely based on intrinsic properties of data instances. Unsupervised methods can be considered as a One-Class Classification (OCC) task.
<h2>5. Deep image/video representation</h2>
<h3>5.1 Traditional features</h3>
The first generation of proposed methods for image/video AD was based on either trajectory based features or low level features such as Histogram of Gradient, Histogram Optical Flow and Motion Boundary Histogram. These approaches suffer from several weaknesses such as high computational cost and low performance.
<h3>5.2 Deep features</h3>
<h4>5.2.1 Feature learning</h4>
Early presented methods have utilized auto-encoders as a popular tool for providing a discriminative representation. In fact, they only modified the traditional solutions by using the learned feature set instead of hand-crafted features.
<h4>5.2.2 Pre-trained networks</h4>
Transfer learning is capable of efficiently follow the idea of distilling more domain knowledge into a model.
<h2>6. Deep networks for AD</h2>
<h3>6.1 Self-supervised learning</h3>
A neural network is trained under the specific constraints to learn $p_{\mathcal{N}}$. Not satisfying the desired constraints for a test samples, $X_{test}$ means that the sample does not follow $p_{\mathcal{N}}$ and can be considered as an anomaly. Auxiliary tasks such as minimizing the reconstruction error, forcing the latent representation to be sparse and predicting next frames of videos are well-known and popular self-supervised tasks to learn $p_{\mathcal{N}}$ while $\mathcal{D}$ is a score that shows how the specified constraints are satisfied for detecting the outliers.
<h4>6.1.1 Encoder-Decoder based methods</h4>
L = $\frac{1}{m}\sum||X^i_\mathcal{N} - D(E(X^i_\mathcal{N}))||^2$, where $D(E(X))$ is an encoder-decoder network that learn the distribution of normal data $p_\mathcal{N}$. The params of $D(E(X))$ is optimized to reconstruct normal events not abnormalities.
<h4>6.1.2 CNNs</h4>
An encoder and a decoder for mapping the input to a latent space and then recovering the original input is expensive -> learn a CNN on available normal data for a pre-text task and detect the out-of-distribution data by analyzing the different responses of the neural networks to different types of input data (normal or anomaly).
<h3>6.2 Generative networks</h3>
The absence of samples belonging to outlier/abnormal class is the major obstacle for learning an end-to-end DNN. GANs is a tool to tackle this problem.
<h3>6.3 Anomaly generation</h3>
Utilizing GANs for generating anomalous data instead of directly using it, converts the problem of AD into a binary classification problem. This way of approaching the problem can also be utilized for abnormal data augmentation.
<h2>7. Datasets</h2>
- Image: MNIST, CIFAR-10, CIFAR-100, Caltech-256 and ImageNet.
- Video: UMN, UCSD, CUHK Avenue, UCF-Crime and ShanghaiTech Campus.
<h2>8. Open challenges and future directions</h2>
Detection and false positive rate, fairness, explanation of AD method, Object interaction, safety, adoptable AD, generating outliers, realistic datasets and early detection or prediction.