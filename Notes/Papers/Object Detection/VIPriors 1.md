<h2>1. Abstract</h2>
First edition, prohibit the use of pre-trained models and other transfer learning techniques.
<h2>2. Introduction</h2>
In addition to typical methods used in deep learning challenges such as ensembling, the top competitors in all challenges heavily rely on data augmentation to make their solutions data-efficient.
<h2>3. Challenges</h2>
<h3>3.1 Classification</h3>
<h4>3.1.1 First place</h4>
Propose Dual Selective Kernel network (DSK-net). The DSK-net block has 3 branches, 2 Selective Kernels and 1 skip connection. After each SK, an anti-aliasing block is attached. Team also employs 3 loss functions: positive class loss, center loss and tree supervision loss. Cutmix is applied during training.
<h4>3.1.2 Second place</h4>
- Luo et al. train ResNest-101, TresNet-XL and SEResNeXt-101 with only cross entropy and combination of cross entropy - triplet loss and cross entropy - ArcFace loss. AutoAugment with 24 different policies and Cutmix are utilized to mitigate the overfitting problem.
- Zhao et al. propose a method which consists of two stages: 1) a teacher network is trained with contrastive learning, MOCO v2 to obtain a feature representation and 2) the knowledge of the teacher network is transferred to student network by knowledge distillation. Meanwhile, the student network is also finetuned with labels. To obtain the final result, increase the input size as 448 $\times$ 448 and use ResNeXt101, AutoAugment, label smoothing, ten crops and model ensembling. 
<h4>3.1.3 Third place</h4>
Focus on data augmentation, loss function and ensemble methods to obtain good classification performance. Train efficientNet backbones and Low Significant Bit (LSB) swapping between image pixels is used as a data augmentation method in addition to RandAugment. Propose focal cosine loss, which is a combination of the focal and cosine losses. Besides, Exponential Moving Average, dropout and drop connection are employed during training. Plurality voting ensemble method with 10 models and Test Time Augmentation are applied to obtain the final result.
<h3>3.2 Object Detection</h3>
<h4>3.2.1 First place</h4>
The architecture is a two-stage Cascade-RCNN with multiple backbones. Infuse global context features into each ROI feature to help the network deal with extreme circumstances such as multiple targets and occlusions. Instead of NMS, use WBF to fuse the predictions of the different backbone networks. Data augmentation: bbox-jitter which randomly translates bounding boxes slightly as well as the established methods Grid-mask and Mix-up.
<h4>3.2.2 Second place</h4>
Cascade R-CNN with R50-FPN-DCNv2 backbone. Add many different methods to their solution, including several modules from the Libra R-CNN, Guided Anchoring and Generalized Attention frameworks, the ResNet-D variation on ResNets and TSD. The Albumentations library, the AutoAugment policies and the Stitchers method are used for data augmentation as well as a Mosaics-SC which couples objects from the same supercategory. The final model is an ensemble of different ResNets sharing the described method.
<h4>3.2.3 Third place</h4>
Use a Scratch Mask R-CNN with a ResNet-101 backbone, Soft-NMS and a custom weighting of the components of the loss function to put more emphasis on the classification loss. Augmentations from the Albumentations library are applied to all images in training.
<h3>3.3 Segmentation</h3>
<h4>3.3.1 First place</h4>
Propose a multi-scale version of CutMix to boost the occurence of scare data classes through data augmentation. The method first trains a segmentation model and samples random crops from the training data from the worst performing classes. Then CutMix is used to augment the training data using the sampled crops and finally the model is retrained. Use HRNet with additional scale attention.
<h4>3.3.2 Second place</h4>
Propose to diversify the models, the data and the test samples by using a combination of extensive data augmentation and model ensembling. Firstly, the method extends the Augmix algorithm to semantic segmentation. This is done by maintaining a pool of photometric data augmentation methods, i.e. augmentations that do not alter the spatial characteristics of an image and transform an image through multiple branches of different combinations of augmentations. The branches are then combined both with each other and with the original input image as a weighted sum. Second, the method combines the HRNetv2, Object Contextual Representations, online test time augmentations and Online Hard Example Mining. Finally, multiple models are trained and are combined as a Frequency Weighted ensemble such that low frequency classes have higher weights.
<h4>3.3.3 Third place</h4>
Introduce an edge-preserving loss to force the edge maps of the predictions and grouth-truth labels to overlap. The edge maps are extracted using simple Sobel Filtering and hard-thresholding. Additionally, the pixel occurrence of rare classes is increased by pasting augmented crops from these classes in the training images. Furthermore, HANet architecture is improved by chaning the feature extractor network to ResNeSt.
<h3>3.4 Action Recognition</h3>
<h4>3.4.1 First place</h4>
Assume that motion between frames serves as a good prior. Propose a two-stream configuration (RGB + Flow) in which each stream combines some of the best models for video processing. While the RGB stream ensembles four popular networks I3D, C3D, R3D and R2+1D, the Flow stream combines two I3D with different input clip length. Finally, results of two streams are fused. During training, apply different data augmentation strategies such as random crops, horizontal flipping or frame skipping.
<h4>3.4.2 Second place</h4>
Use a two stream architecture (RGB + Flow) based on a modified C3D network. The enhanced C3D model adds a new term called Temporal Central Difference Convolution (TCDC) to the computation of the vanilla 3D convolution which helps including information from the adjacent components of the 3D receptive field. Utilize a Rank Pooling representation for the RGB stream to avoid noisy details of RGB.
<h4>3.4.3 Thrid place</h4>
Use a more efficient setup: SlowFast model. Also fuse the SlowFast with TSM modules. Apply several data augmentation techniques: center/random crop, horizontal flip and normal/reverse video reproduction.