<h2>1. Abstract</h2>
Present DINO by using a contrastive way for denoising training, a mixed query selection method for anchor initialization and a look forward twice scheme for box prediction. 
<h2>2. Introduction</h2>
By improving the denoising training, query initialization and box prediction, design a new DETR-like model based on DN-DETR, DAB-DETR and Deformable DETR. DINO contains a backbone, a multi-layer transformer encoder, a multi-layer transformer decoder and multiple prediction heads. Following DAB-DETR, formulate queries in decoder as dynamic anchor boxes and refine them step-by-step across decoder layers. Following DN-DETR, add ground truth labels and boxes with noises into Transformer decoder layers to help stabilize bipartite matching during training. Adopt deformable attention for its computational efficiency. Moreover, propose three new methods: 1) propose a contrastive denoising training by adding both positive and negative samples of same ground truth at same time. After adding two different noises to same ground truth box, mark the box with a smaller noise as positive and other as negative. 2) propose a mixed query selection by selecting initial anchor boxes as positional queries from output of encoder. 3) propose a new look forward twice scheme to correct the updated parameters with gradients from later layers.
<h2>3. Related work</h2>
<h3>3.1 Classical object detectors</h3>
YOLO series, HTC++ and Dyhead.
<h3>3.2 DETR and its variants</h3>
DETR, Deformable DETR, Efficient DETR, DAB-DETR and DN-DETR. 
<h3>3.3 Large-scale pre-training for object detection</h3>
Swin V2 and Florence.
<h2>4. DINO: DETR with improved denoising anchor boxes</h2>
<h3>4.1 Preliminaries</h3>
DINO formulates the positional queries as dynamic anchor boxes and is trained with an extra DN loss. DINO also adopts query selection from Deformable DETR to better initialize the positional queries.
<h3>4.2 Model overview</h3>
Given an image, extract multi-scale features with ResNet/Swin Transformer and then feed them into Transformer encoder with corresponding positional embeddings. After feature enhancement with encoder layers, propose a new mixed query selection strategy to initialize anchors as positional queries for decoder. Use deformable attention to combine features of encoder outputs and update the queries layer-by-layer. The final outputs are formed with refined anchor boxes and classification results predicted by refined content features. Beyond DN method to perform denoising training approach by taking into account hard negative samples.
<h3>4.3 Contrastive denoising traning</h3>
Given $\lambda_1$ and $\lambda_2$, where $\lambda_1 < \lambda_2$. Generate two types of CDN queries: positive queries and negative queries. Positive queries within the inner square have a noise scale smaller than $\lambda_1$ and are expected to reconstruct their corresponding ground truth boxes. Negative queries between inner and outer squares have a noise scale larger than $\lambda_1$ and smaller than $\lambda_2$. They are expected to predict "no object". Similar to DN-DETR, use multiple CDN groups to improve effectiveness of method. The reconstruction losses are $l_1$ and GIOU for box regression and focal loss for classification. The loss to classify negative samples as background is also focal loss. 
<h3>4.4 Mixed query selection</h3>
Only initialize anchor boxes using the position information associated with selected top-K features, but leave the content queries static as before. It helps the model to use better positional information to pool more comprehensive content features from the encoder. 
<h3>4.5 Look forward twice</h3>
Propose look forward twice to perform box update, where parameters of layer-$i$ are influenced by losses of both layer-$i$ and layer-($i$ + 1). For each predicted offset $\Delta b_i$, it is used to update box twice, one for $b'_i$ and another for $b_{i + 1}^{pred}$. The final precision of a predicted box $b^{(pred)}_i$ is determined by two factors: the quality of initial box $b_{i - 1}$ and predicted offset of box $\Delta b_i$. To improve the quality of the first factor, supervising the final box $b'_i$ of layer $i$ with the output of the next layer $\Delta b_{i + 1}$. Hence, use the sum of $b'_i$ and $\Delta b_{i + 1}$ as the predicted box of layer-($i$ + 1). Given an input box $b_{i - 1}$ for the $i$-th layer, obtain the final prediction box $b^{(pred)}_i$ by: $\Delta b_i$ = Layer$_\mathrm{i}$($b_{i - 1}$), $b'_i = \mathrm{Update}(b_{i - 1}, \Delta b_i), b_i = \mathrm{Detach}(b'_i), b_i^{(pred)} = \mathrm{Update}(b'_{i - 1}, \Delta b_i)$, where $b'_i$ is the undetached version of $b_i$. Update($\cdot, \cdot$) is a function that refines the box $b_{i - 1}$ by predicted box offset $\Delta b_i$ as the same way in Deformable DETR.
<h2>5. Datasets</h2>
COCO 2017, Object365.
<h2>6. Metrics</h2>
AP, AP$_{50}$, AP$_{75}$, AP$_{S}$, AP$_{M}$, AP$_{L}$, FPS. 
<h2>7. Code</h2>
https://github.com/IDEA-Research/DINO.