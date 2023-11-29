<h2>1. Abstract</h2>
Third edition, focusing on addressing the limitations of data availability in training deep learning models for computer vision tasks. Participants were required to train models from scratch. The objective was to encourage novel approaches that incorporate relevant inductive biases.
<h2>2. Introduction</h2>
Focusing on data efficiency through visual inductive priors. Top competitors heavily relied on model ensembling and data augmentation. Additionally, many of the participants's solutions utilized a limited number of backbones and baseline methods, which seem to possess properties conductive to learning from small data.
<h2>3. Challenges</h2>
- Image classification: using a subset of Imagenet which contains 50 images from 1000 classes for training, validation and testing.
- Object detection: DelftBikes includes 8000 bike images for training and 2000 images for testing. Each image contains 22 different bike parts that are annotated as bounding box, class and object state labels.
- Instance segmentation: main objective is to segment basketball players and the ball on images recorded of a basketball court. The dataset is provided by SynergySports.
- Action recognition: Kinetics400ViPriors which is an adaptation of Kinetics400 dataset. the training set consists of 40k clips, while the validation and test sets contain about 10k and 20k clips.
<h3>3.1 Classification</h3>
<h4>3.1.1 First place</h4>
Using an ensemble of SE + PyramidNet, ResNeSt200e, ReXNet, EfficientNet-B8 and ConvNeXt-XL* models. Optimization tricks: RICP and hard fusion. 
<h4>3.1.2 Second place</h4>
Only use two models in ensemble and improve performance by using cross-decoupled knowledge distillation. Automix and label smoothing are also required.
<h4>3.1.3 Thrid place</h4>
Using 5 different encoder architectures. Applying knowledge distillation from all encoders to all other encoders to train twenty models, which are all ensembled for the final model. CutMix, random erasing, MixUp and AutoAugment.
<h4>3.1.4 Conclusion</h4>
Crucial components: ensembling of many different classification architectures, as well as combining multiple different augmentation policies.
<h3>3.2 Object Detection</h3>
<h4>3.2.1 First place</h4>
Employ an ensemble of various YOLO detectors and CBNetv2. Design two-stage training: 1) pre-training by using weak data augmentation and 2) fine-tuning by using strong data augmentation such as mosaic, mix-up and copy-paste and a weighted training strategy based on image uncertainty. Improving the results by weighted boxes fusion and TTA strategies.
<h4>3.2.2 Second place</h4>
Train Cascade RCNN using Swin Transformer, ConvNext and ResNext as backbone. These backbones are pretrained by using self-supervised methods such as MOCOV2 and MOBY. Use AutoAugment, random flip and multi-scale augmentation methods to improve detection performance. Finally, the non-maximum weighted method, Soft-NMS and model ensemble methods are used on the test set.
<h4>3.2.3 Thrid place</h4>
Train Cascade RCNN detector using ConvNext backbone. Then, they create a synthetic dataset from the training set and obtain pseudo labels on this dataset with the initial trained model. Afterwards, they train the same model only on the pseudo labels with smaller-resolution images. In the end, retrain the pseudo-label pretrained network with the original training set and select some hard classes to improve the detector performance. Augmentation methods: colour jittering and RGB shifting, mix-up and AutoAugment.
<h4>3.2.4 Jury prize</h4>
Two phases: pretraining and adaption phases. In the pretraining phase, utilize mosaic and mix-up on object and image level features and train SimMIM. In the adaption phase, a pretrained encoder of SimMIM is used to initialize the backbone of Cascade RCNN. In coarse detection, the model detects bike objects. In the fine detection phase, the fine detection module runs on the cropped bike object from the previous phase and tries to detect relevant bike parts.
<h3>3.3 Instance Segmentation</h3>
<h4>3.3.1 First place</h4>
TS-DA generates additional training data and TS-IP is applied at test time. TS-DA employs copy-paste with constraints on the absolute locations of the synthetic players and ball objects to ensure all objects are placed inside the court and their relative locations to mimic player-ball interactions. Subsequently, geometric and photometric augmentations are applied to the image to increase the variety in their appearance. During inference, random scaling and cropping is applied to the images, and additional filtering employed to the predictions to ensure only one basketball of reasonable dimensions is present on the court. The segmentation model is based on the Hybrid Task Cascade (HTC) detector and CBSwin-T backbone with CBFPN. Mask Scoring R-CNN is used to improve segmentation quality. Fine-tune using the SWA strategy. 
<h4>3.3.2 Shared second place - A</h4>
Using a Swin Transformer-Large as the backbone and the pipeline is based on CBNetV2. Data augmentations: AutoAugment, ImgAug and Copy-Paste.
<h4>3.3.3 Shared second place - B</h4>
Make use of HTC detector with CBSwin-T backbone with CBFPN using group normalization, Mosaic, test-time augmentations and the Task-Specific Copy-Paste Data Augmentation Method. Moreover, different backbones ResNet, ConvNeXt, Swinv2 and CBNetv2 are trained and combined using Model Soups. Multiple predictions are combined together using mask voting.
<h4>3.3.4 Thrid place</h4>
Employ Location-aware MixUp, RandAugment, GridMask, Random scaling, Copy-paste, Multi-scale agumentation and test-time augmentation. Model used is HTC detector and soft non-maxima suppression is applied on the predicted target boxes. 
<h4>3.3.5 Jury prize</h4>
Using a novel representation of instance activation maps. these maps highlight informative regions for each object which are then used to obtain instance-level features for recognition and segmentation. The method avoids NMS in post-processing by predicting objects in a one-to-one style using bipartite matching.
<h3>3.4 Action Recognition</h3>
<h4>3.4.1 First place</h4>
R(2 + 1)D, SlowFast, CSN, X3D, TANet and Timesformer and apply a model fusion approach by assigning different weights to the models and using the soft voting method to combine their results. In data augmentation, frames were extracted from videos and a subset was selected by choosing every second frame. The videos were resized and noise was added through random flipping. During testing, TenCrop was used as a test-time enhancement. The evaluation involved ten-fold cross-validation where the training dataset was combined with the validation dataset.
<h4>3.4.2 Second place</h4>
Propose a multi-network dynamic fusion model combining a variety of backbones SlowFast, Timesformer, TIN, TPN, Video Swin Transformers, R(2 + 1)D, X3D, DirecFormer. Model predictions are combined as a weighted average by the prediction score of each model. Test-time augmentation with majority voting is used as well as AutoAugment, CutMix and a variety of other spatial, photometric and temporal augmentations during training.
<h4>3.4.3 Thrid place & Jury prize</h4>
Combine self-supervised pre-training of various backbone models, optical flow estimation and model ensembling to train a data efficient video classification model. First, 2D model encoders are pre-trained using MoCO self-supervised representation learning framework on image data using the individual frames of the provided dataset. Next, optical flow features are extracted using TVL-1 method. To correct for camera movement, consecutive image frames are aligned by calculating the transformation matrix based on extracted SIFT features. Finally, a range of models including TSN, TANet, TPN, SlowFast, CSN and Video MAE are trained on the training data and pre-extracted optical flow features. MixUp and CutMix is employed.
<h2>4. Code</h2>
https://github.com/VIPriors/vipriors-challenges-toolkit.