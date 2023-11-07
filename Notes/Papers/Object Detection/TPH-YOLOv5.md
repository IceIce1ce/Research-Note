<h2>1. Abstract</h2>
Based on YOLOv5, add one more prediction head to detect objects with different sizes. Replace the prediction head of YOLOv5 with Transformer Prediction Heads. Combine attention CBAM to gives better prediction in dense regions. Use ensemble and extra classifier.
<h2>2. Introduction</h2>
Three common problems in drones: object scale varies with drone altitude, drone images often have high density that causes occlusion between objects, and drone images often contain strange geographical factors. TPH-YOLOv5 contains four prediction heads separately for 4 target types: tiny, small, medium and large. Through the visualization results, the model has good localization but poor classification ability, using ResNet18 for the image patches cropped from the training data, thus increasing the value by almost 1.0%. AP.
<h2>3. Related Work</h2>
<h4>3.1 Data Augmentation</h4>
For photometric distortion, adjust hue, saturation and value of the images. For geometric disortion, add random scaling, cropping, translation, shearing and rotating. Moreover, there are some other methods such as: MixUp, CutMix and Mosaic.
<h4>3.2 Multi-modal Ensemble Method in Object Detection</h4>
There are three methods to ensemble boxes from object detection models: NMS, Soft-NMS and weighted boxes fusion. In NMS, if IoU > threshold, they are belong to the same object. Box filtering process depends on the choice of IoU threshold, which have a big impact on model performance. Soft-NMS sets an attenuation function for confidence of adjacent bounding boxes based on IoU. WBF merges all boxes to form the final results instead of excluding some boxes.
<h4>3.3 Object Detection</h4>
Backbone: VGG, ResNet, DenseNet, MobileNet, EfficientNet, CSPDarknet53, Swin Transformer. Neck: FPN, PaNet, NAS-FPN, BiFPN, ASFF, SFAM. Head: one-stage object detector and two-stage object detector.
<h2>4. TPH-YOLOv5</h2>
The architecture of TPH-YOLOv5 uses CSPDarknet53 as backbone of YOLOv5 but it has three additional transformer encoder block at final layer. Neck uses CBAM as attention mechanism to focus on tiny objects, module C3 (C3 = 3*Conv + CSP Bottleneck) is still used in some blocks, but other blocks use C3STR (swin transformer), the other components of TPH-YOLOv5 is the same as the original YOLOv5.
<h2>5. Datasets</h2>
VisDrone2021.
<h2>6. Metrics</h2>
$AP, AP_{50}, AP_{75}, AR_1, AR_{10}, AR_{100}, AR_{500}$.
<h2>Code</h2>
https://github.com/cv516Buaa/tph-yolov5.