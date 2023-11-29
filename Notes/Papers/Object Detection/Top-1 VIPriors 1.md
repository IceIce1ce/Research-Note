<h2>1. Abstract</h2>
Based on mmdetection of a strong baseline. Firstly, introduce an effective data augmentation method to address lack of data problem, which contains bbox-jitter, grid-mask and mix-up. Secondly, present a robus ROI extraction method to learn more significant ROI features via embedding global context features. Thridly, propose a multi-modal integration strategy to refinement the prediction box, which weighted boxes fusion.
<h2>2. Introduction</h2>
Focus on data augmentation in data preprocess and adapt bbox-jitter, grid-mask and mix-up methods to improve the diversity of object images. Then, to make the detector to learn more prominent features, connect the global context features to the ROI features and each stage. To overcome the lack of data, trained three modertate backbones network of Cascade-RCNN. Finally, adopt to WBF on test dataset. 
<h2>3. Methods</h2>
<h3>3.1 Data Augmentation</h3>
- Bbox-jitter: to increase the fitting ability of model, increase difficulty of the training samples. It randomly shakes the boxes with an absolute amplitude to make the model more sensitive to ROI area.
- Grid-mask: is an information dropping method based on the deletion of regions of input image. Given an input images, the grid-mask algorithm randomly removes a region with a disconnected pixel set.
- Mix-up: uses modeling between different categories to achieve data augmentation. Two samples are randomly selected to improve the diversity of training set from the training samples for simple random weighted summation. At the same time, labels of samples also correspond to weighted summation, and then prediction results and weighted summation of label after loss are calculated, and params are updated in the back propagation.
Other data augmentation methods: random brightness contrast, random scale, pixel padding, random flipping.
<h3>3.2 Capturing Global Context Features</h3>
Use Cascade-RCNN and select multiple backbone networks such as Res2Net-152, SeNet-154 and ResNeSt-152. Scenes' multiple salient targets, foreground interference and chaotic backgrounds are challenging because salient parts of different objects are individuated. There is a global semantic relationship between multiple salient objects. In the usual feature pyramid networks stage, RoI features are extracted from a certain pyramid according to RoI scale. This feature aggregation method of pyramids will be gradually diluted in high-level features passing to the bottom layer. Thus, use the feature map of entire image as a bounding box to achieve ROI convergence to obtain global context features. Then, this global features is tightly connected with the original feature of each ROI in each stage. Finally, the merged features are passed to the classification layer and box regression layer.
<h3>3.3 Weighted Boxes Fusion</h3>
Model fusion methods: voting/Averaging, boosting, bagging and stacking. Select Cascade-RCNN networks of three different backbone for an integrated prediction test dataset. Also do some special operations for Cascade-RCNN of ResNeSt-152, replace all batch normalization layers of detection network with group normalization to make the statistical information of normalization params more accurate. Add switchable atrous convolution (SAC) to backbone network. Moreover, for Cascade-RCNN of three different backbone networks, use bounding box augmentation to randomly crop and flip 6 times for categories with few samples.
<h2>4. Datasets</h2>
A subset of COCO-2017.
<h2>5. Metrics</h2>
AP@0.50:0.95, AP@0.50 and AP@0.75.
<h2>6. Code</h2>
https://github.com/muzishen/VIPriors-Object-Detection-Challenge.