<h2>1. Abstract</h2>
DETR directly transformers queries to unique objects by using one-to-one bipartite matching during training and enables end-to-end object detection. However, effectiveness of one-to-one matching is not fully understood. Conduct a strict comparison between one-to-one Hungarian matching in DETRs and one-to-many label assignments in traditional detectors with NMS. Train Deformable-DETR with traditional IoU-based label assignment achieved 50.2 COCO mAP within 12 epochs with ResNet50 backbone. 
<h2>2. Introduction</h2>
Design a transformer-based object detector that directly assigns positive-negative labels to each query as in traditional detectors and use NMS to remove duplicated predictions, intead of using end-to-end one-to-one matching. Specifically, follow strong DETR framework and replace one-to-one Hungarian matching loss with a one-to-many IoU-based assignment loss in both stages. in the first stage, assign anchors from a transformer encoder to annotations or background using an IoU metric. In the second stage, assign queries similarly. Compared to traditional detectors, DETA features a more expressive head: In the first stage, DETA uses a 6-layer transformer encoder over convolutional backbone, while a tradditional RPN or RetinaNet uses 2 to 4 convolutional layers; in the second stage, DETA uses a 6-layer transformer decoder while FasterRCNN uses 2 linear layers. Compared to end-to-end transformer detectors, DETA only differs in training loss with the same architecture. The effectiveness of training loss in DETA comes from: 1) one-to-many mapping yields more total positive predictions which help speed up model convergence and 2) use an object balancing technique that helps stabilize matching for small objects.
<h2>3. Related work</h2>
<h3>3.1 Traditional NMS-based detectors</h3>
RetinaNet, YOLO, CenterNet, FCOS, ATSS and GFL.
<h3>3.2 End-to-end detectors</h3>
DETR, Deformable-DETR, TSP-RCNN, EfficientDETR, DAB-DETR, DN-DETR, DINO, GroupDETR, $\mathcal{H}$-DETR and FQDet.
<h2>4. Preliminary</h2>
Given an image $I \in \mathbb{R}^{H \times W \times 3}$, a detector produces a bounding box $b_i \in \mathbb{R}^4$ and scores $s_i \in \mathbb{R}^C$ for $C$ categories for each object $i$. 
<h3>4.1 DETR</h3>
DETR decomposes detection into 3 components: a backbone, a transformer encoder and a transformer decoder. Backbone takes image as an input and produces a down-sampled $D$-dimensional feature map $F \in \mathbb{R}^{M \times D}$, where $M$ is the number of features in the feature map, $M = w \times h$ in vanilla DETR or $M = \sum_l w_l \times h_l$ in multi-scale feature resolutions. The transformer encoder refines $F$ using self-attention with positional embeddings and produces updated features $F' \in \mathbb{R}^{M \times D'}$. The transformer decoder transform a set of queries $Q \in \mathbb{R}^{N \times D'}$ into $N$ unique object detections/background $O = \{\hat{b}_i, \hat{s}_i\}^N_{i = 1}$. During this process each query cross-attends to the encoded feature map $F'$.
<h3>4.2 Two-stage DETR</h3>
Object queries $Q$ are learnable network parameters and are fixed for all testing images. Transformer encoder densely predicts ranked proposal boxes $b_i^{prop}$ from fixed initial boxes $b^{init}_i$. Top N proposals form queries for a second stage $Q_i = \mathcal{G}(b^{prop}_i)$. The second stage now takes features from first stage as inputs instead of fixed queries.
<h3>4.3 Hungarian matching loss</h3>
During training, DETR assigns each network output $O = \{\hat{b}_j, \hat{s}_j\}^M_{j = 1}$ to exactly one annotated object in $G = \{b_k, c_k\}^K_{k = 1}$ or background $\emptyset$ where $c_k$ is class id for object $k$. Let $\sigma_i \in \{\emptyset, 1...K\}$ be the assignment of network output $i$ to annotation $\sigma_i$. DETR enforces a one-to-one matching to learn NMS: for $i \neq j$ with $\sigma_i \neq \emptyset$ and $\sigma_j \neq \emptyset$, we have $\sigma_i \neq \sigma_j$. Furthermore, for each annotation $k, \exists i: \sigma_i = k$. DETR optimizes for best matching in the training objective: $\mathcal{L}(O, G) = min_{\sigma}(\sum_{i = 1}^M\mathcal{L}_{cls}(\hat{s}_i, c_{\sigma_i}) + \mathcal{L}_{box}(\hat{b}_i, b_{\sigma_i}))$ (1), where $\mathcal{L}_{cls}$ is a focal-loss based classification loss and $\mathcal{L}_{box}$ is a bounding box loss composed of GIoU and L1 loss. Any unmatched output $\sigma_i = \emptyset$ predicts a background score $s_\emptyset$ and incurs no box loss $\mathcal{L}_{box}(\hat{b}_i, b_\emptyset)$ = 0.
<h2>5. IoU assignment in transformer detectors</h2>
First, relax one-to-one matching constraint and instead allow one-to-many assignments. Allow multiple outputs $i$ and $j$ to be assigned to the same ground truth $\sigma_i = \sigma_j$. Second, switch to a fixed overlap-based assignment strategy instead of loss-based matching in Equation (1).
<h3>5.1 First stage assignments</h3>
Start from fixed initial query features $F$ derived from Deformable-DETR. Each query $i$ is anchored at a specific location ($x_i, y_i$) within the image. Use a constant box size $w_i = 0.1 \times 2^{-l_i} \times W$ and $h_i = 0.1 \times 2^{-l_i} \times H$ where $l_i \in \{0,...,3\}$ is the feature resolution, $W$ and $H$ are width and height of image. This yields an initial box definition $b^{init}_i = (x_i - \frac{w_i}{2}, y_i - \frac{h_i}{2}, x_i + \frac{w_i}{2}, y_i + \frac{h_i}{2})$. Use these initial boxes for both positional embeddings and overlap-based assignment process. Assign each prediction $i$ to highest overlapping ground truth object if overlap is larger than a threshold $\tau$. At the beginning of training, a ground truth object sometimes does not have an overlapping prediction with a threshold $\tau$. Anchor $i$ is closest to object $k$ if the condition $C^{init}_{max}(i, k) = \cap_{j \neq i}\mathcal{1}{IoU(b^{init}_i, b_k) \geq IoU(b^{init}_j, b_k)}$ (2) is met. Then, the assignment procedure is formally: $\sigma^{init}_i = \hat{k} = argmax_k IoU(b^{init}_i, b_k)$ if $IoU(b^{init}_i, b_{\hat{k}}) \geq \tau$ or $C^{init}_{max}(i, \hat{k})$ else $\emptyset$ (3). Use a threshold $\tau$ = 0.7 for the first stage. Train first stage using a class-agnostic version of DETR loss $\mathcal{L}(O, G|\sigma^{init})$ (1) on fixed assignments $\sigma^{init}$ (3). The classification loss $\mathcal{L}_{cls}$ of the first stage is a binary focal loss indicating foreground vs background. Apply NMS on proposal boxes $b^{prop}$. 
<h3>5.2 Second stage assignments</h3>
Instead of using a fixed set of initial boxes $b^{init}$, use outputs of first stage $b^{prop}$ to define an equivalent assignment $\sigma^{prop}_i$. Train this second stage using same loss as DETR $\mathcal{L}(O, G|\sigma^{prop})$. Following FasterRCNN, balance the foreground object ratio using a hyper-parameter $\gamma$. Specifically, when the number of positive queries is larger than $N \times \gamma$, randomly sample $N \times \gamma$ positive queries from them. To encourage a larger number of positive queries, relax overlap criterion for second stage to $\tau$ = 0.6.
<h3>5.3 Object balancing</h3>
Larger objects are naturally more likely to contain many overlapping predictions than smaller objects. Propose a simple object-balancing technique that samples at most $n$ positive assignments for each ground truth. Let $\mu^n_k$ be the $n$-th highest IoU for annotation $k$ and $\tau^n_k = max(\tau, \mu^n_k)$ be new dynamic threshold. Modify the assignment procedure in Equation (3): $\sigma^{prop}_i = \hat{k} = argmax_k IoU(b^{prop}_i, b_k)$ if $IoU(b^{prop}_i, b_{\hat{k}}) \geq \tau^n_k$ or $C^{prop}_{max}(i, \hat{k})$ else $\emptyset$. 
<h2>6. Datasets</h2>
COCO 2017 and LVIS.
<h2>7. Metrics</h2>
$AP, AP_s, AP_m, AP_l, AP_{\mathrm{50}}, AP_{\mathrm{75}}$.
<h2>8. Code</h2>
https://github.com/jozhang97/DETA.