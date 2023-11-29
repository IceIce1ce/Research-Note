<h2>1. Abstract</h2>
This paper develops an enhanced data augmentation method to effectively suppress overfitting during training and design a hybrid random loss function to improve detection accuracy of small objects. Inspired by FCOS, a lighter and more efficient decoupled head is proposed and its inference speed can be improved with little loss of precision. 
<h2>2. Related work</h2>
<h3>2.1 Anchor-free object detector</h2>
SSD, FCOS. Reducing post-processing computation $\rightarrow$ speed-up for edge computing devices. There are two main types of anchor-free detectors: anchor-point-based and keypoint-based.
<h3>2.2 Data augmentation</h3>
Mosaic, Mixup, CopyPaste.
<h3>2.3 Model reduction</h3>
Two categories: lossy reduction and lossless reduction. Lossy reduction usually builds smaller networks by reducing number of network layers and channels. Lossless reduction integrates and couples multiple branch modules to build a more streamlined equivalent module by re-parameterizing techniques.
<h3>2.4 Decoupled regression</h3>
Using a decoupled regression detection head can achieve a better result and accelerate loss convergence. Nonetheless, a decoupled head brings extra inference costs.
<h3>2.5 Small object detecting optimization</h3>
- Small objects are copied and randomy placed in other positions of the image to increase the training data samples of small objects during data augmentation process, which is called replication augmentation.
- Images are zommed and spliced and some larger objects in original image are zommed into small objects.
- Loss function is designed to pay more attention to small objects by increasing proportion of small objects' losses.
<h2>3. Approach</h2>
<h3>3.1 Enhanced-Mosaic & Mixup</h3>
First, use Mosaic method for several groups of images and thus group number can be set according to richness of average number of labels in a single picture in dataset. Then, a last simply processed image is mixed with those Mosaic processed images by Mixup method. In these steps, original image boundary of last image is within boundary of final output image after transformation. 
<h3>3.2 Lite-decoupled head</h3>
Efficient decoupled head add implicit representation layers to all last convolutional layers for better regression performance. With re-parameterizing, implicit representation layers are integrated into convolutional layers for lower inference costs. The last convolutional layers for box and confidence regression are also merged such that model can do inference with high parallel computation.
<h3>3.3 Staged loss function</h3>
Divide training process into three stages. At the first stage, take one of the most common loss function: gIOU loss for IOU loss. Balanced cross entropy loss for classification loss and object loss and regulation loss setting as zero. the training process steps into the second stage at last few data-augmentation-enabled epochs. The loss functions of classification loss and object loss are replaced by hybrid-random loss: hrl($p, t$) = $[4(1 - p)^2r + (1 - r)]t\log(p) + [12p^2r + (1 - r)](1 - t)\log(1 - p)$, where $p$ represents prediction result, $t$ represents ground truth and $r$ is a random number between 0 and 1. For all results in one image, we have: HRL$(P, T) = -\frac{1}{n}\sum_{i = 1}^n\mathrm{hrl}(P(i), T(i))$. When it comes to third stage, close data augmentation and set L1 loss as regulation loss and replace gIOU loss by cIOU loss.
<h2>4. Datasets</h2>
MS COCO2017, VisDrone2019-DET.
<h2>5. Metrics</h2>
AP$_{val}$, AP$_{50}$, AP$_{75}$, AP$_{S}$, AP$_{M}$, AP$_{L}$.
<h2>6. Code</h2>
https://github.com/LSH9832/edgeyolo.