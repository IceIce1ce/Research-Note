<h2>1. Abstract</h2>
Propose a background-agnostic framework that learns from training videos containing only normal events. The framework is composed of an object detector, a set of appearance and motion auto-encoders, and a set of classifiers. To overcome the lack of abnormal data during training, propose an adversarial learning strategy for the auto-encoders. Create a scene-agnostic set of out-of-domain pseudo-abnormal examples, which are correctly reconstructed by the auto-encoders before applying gradient ascent on the pseudo-abnormal examples. Further utilize the pseudo-abnormal examples to serve as abnormal examples when training appearance-based and motion-based binary classifiers to discriminate between normal and abnormal latent features and reconstructions. Furthermore, to ensure that the auto-encoders focus only on the main object inside each bounding box image, introduce a branch that learns to segment the main object.
<h2>2. Method</h2>
<h3>2.1 Motivation</h3>
The first approach is to employ a single-shot object detector. The second approach consists of gathering a large and generic pool of pseudo-abnormal examples, substituting the need for abnormal examples during training.
<h3>2.2 Overview</h3>
An object detector is applied on the video. For each object detection, optical flow maps are computed with respect to the previous and next frames. The object detections are given as input to an appearance auto-encoder, while the optical flow maps are given as input to two motion auto-encoders. At the same time, the autoencoders are prevented from learning to reconstruct examples from the pool of pseudo-abnormal training samples passing through the encoder and the adversarial decoder branch. The appearance auto-encoder has a third decoder branch that learns to reconstruct object segments. The segmentation branch helps the model to focus on the foreground object. The absolute differences between the inputs and reconstructions are subsequently used to learn three binary classifiers for discriminating between normal examples and pseudo-abnormal examples. During inference, the average scores provided by the three classifiers represent normality scores associated to the input object detections.
<h3>2.3 Object detection and preprocessing</h3>
YOLOv3 is applied on each frame $t$. To obtain the input for the appearance auto-encoder, crop the objects according to the detected bounding boxes, then, convert the resulting image crops to grayscale. To obtain segmentation maps to be used as ground-truth labels during training time, employ Mask R-CNN. To obtain the motion representation corresponding to an object, use pre-trained version of SelFlow, applying it on each tuple of three consecutive frames.
<h3>2.4 Pseudo-abnormal examples</h3>
To obtain pseudo-abnormal motion patterns, we compute optical flow maps on frame triplets selected at time $t - k$, $t$ and $t + k$ from the normal training video. Choose $k$ = {3, 4, 5, 6}.
<h3>2.5 Architecture</h3>
The input size for the appearance CAE is $64 \times 64$, while the input size for the motion CAEs is $64 \times 64 \times 2$. Each encoder is composed of three convolutional (conv) layers, each followed by a max-pooling layer with a filter size of $2 \times 2$ applied at a stride of 2. The conv layers are formed of $3 \times 3$ filters. Each conv layer is followed by ReLU. The first two conv layers consist of 32 filters, while the third layer consists of 16 filters. The latent representation is composed of 16 activation maps of size $8 \times 8$. Each decoder starts with an upsampling layer, increasing the spatial support of the activation maps by a factor of $2 \times$. After upsampling, apply a conv layer with 16 filters of $3 \times 3$. The first upsampling and conv block is followed by another two upsampling and conv blocks. The last conv layer of an appearance decoder is formed of a single conv filter, while the last conv layer of a motion decoder is formed of two filters. The appearance CAE incorporates one encoder $e$ and three decoder branches. The first decoder $d$ is used to reconstruct the normal objects, the second decoder $d'$ is used to decode the pseudo-abnormal objects and the third decoder $d''$ is used to generate a mask that segments the object and ignores the background of the input image. The motion CAEs incorporate one encoder $\overline{e}_*$ and only two decoder branches, $\overline{d}_*$ to reconstruct the normal objects and $\overline{d}'_*$ to decode the pseudo-abnormal examples, where * can be replaced with values from the set {$b, f$}, where $b$ represents the backward motion and $f$ represents the forward motion. A binary classifier is composed of five conv layers, a fully-connected layer and a Softmax classification layer. Each conv layer in an encoder has skip connections.
<h3>2.6 Training the auto-encoders</h3>
Let $\theta_e, \theta_d, \theta_{d'}, \theta_{d''}$ be the parameters of appearance encoder $e$, the main appearance decoder $d$, the adversarial appearance decoder $d'$ and the segmentation decoder $d''$, $\theta{\overline{e}}, \theta_{\overline{d}_*}, \theta_{\overline{d}'_*}$ be the parameters of motion encoder $\overline{e}_*$, the main decoder $\overline{d}_*$ and the adversarial motion decoder $d'_*$. The motion auto-encoders loss function is: $L_{*mot-rec}(\overline{x}_*, \hat{x}_*) = \frac{1}{h \cdot w \cdot c}\sum_{i = 1}^h\sum_{j=1}^w\sum_{k=1}^c)(\overline{x}_{*ijk} - \hat{x}_{*ijk})^2$. The loss for adversarial branch of motion CAEs: $L_{*mot-adv}(\overline{x}_*, \hat{x}_*) = \frac{1}{h \cdot w \cdot c}\sum_{i = 1}^h\sum_{j=1}^w\sum_{k=1}^c)(\overline{x}_{*ijk} - \hat{x}_{*ijk})^2$. $L_{app-rec}(x, \hat{x}, s, \hat{s}) = \frac{1}{h \cdot w}\sum_{i = 1}^h\sum_{j = 1}^w(x_{ij} - \hat{x}_{ij})^2 + \frac{1}{h \cdot w}\sum_{i = 1}^h\sum_{j = 1}^w-s_{ij} \cdot \log(\hat{s}_{ij}) - (1 - s_{ij}) \cdot \log(1 - \hat{s}_{ij})$, where $x$ is a grayscale image. The loss for the adversarial branch of appearance CAE is: $L_{app-adv}(x, \overline{x}) = \frac{1}{h \cdot w}\sum_{i = 1}^h\sum_{j=1}^w(x_{ij} - \overline{x}_{ij})^2$.
<h3>2.7 Training the binary classifiers</h3>
After training the auto-encoders until convergence, remove the additional decoder branches $d', d'', \overline{d}'_*$, while freezing the parameters $\theta_e, \theta_d, \theta_{\overline{e}_*}$ of the remaining components. Then, pass all the training examples, including the adversarial ones, through the main appearance and motion decoders, obtaining the final reconstructions for the training data. The classifiers are trained with BCE.
<h3>2.8 Inference</h3>
$s(x) = 1 - \mathrm{mean}(\hat{y}^{(i)}), \forall i \in \{1, 2, 3\}$.
<h2>3. Metrics</h2>
AUC, RBDC and TBDC.
<h2>4. Datasets</h2>
Avenue, ShanghaiTech, Subway and UCSD Ped2.
<h2>5. Code</h2>
https://github.com/lilygeorgescu/AED.