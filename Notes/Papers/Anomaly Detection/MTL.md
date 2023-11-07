<h2>1. Abstract</h2>
First utilize a pre-trained detector to detect objects. Then, train a 3D convolutional neural network to produce discriminative anomaly-specific information by jointly learning multiple proxy tasks: three self-supervised and one based on knowledge distillation. The self-supervised tasks are: (i) discrimination of forward/backward moving objects (arrow of time), (ii) discrimination of objects in consecutive/intermittent frames (motion irregularity) and (iii) reconstruction of object-specific appearance information. The knowledge distillation task takes into account both classification and detection information, generating large prediction discrepancies between teacher and student models when anomalies occur.
<h2>2. Method</h2>
<h3>2.1 Motivation and overview</h3>
First, detect the objects in each frame using a pre-trained YOLOv3, obtaining a list of bounding boxes. For each detected object in the frame $i$, create an object-centric temporal sequence by simply cropping the corresponding bounding box from frames $\{i - t, ..., i - 1, i, i + 1, ..., i + t\}$, resizing each cropped image to $64 \times 64$ pixels. Resulting object-centric sequence is the input of our 3D CNN. During inference, the anomaly score is computed by averaging the scores predicted for each task. For the arrow of time and motion irregularity tasks, we take the probability of the temporal sequence to move backward and the probability of the temporal sequence to be intermittent. For the middle frame prediction task, we consider the mean absolute difference between the ground-truth and the reconstructed object. The last component of the anomaly score is the difference between the class probabilities predicted by YOLOv3 and the corresponding class probabilities predicted by our knowledge distillation branch.
<h3>2.2 Neural architectures</h3>
For each network configuration, the spatial size of the RGB input is $64 \times 64$ pixels. The 3D conv layers use filters of $3 \times 3 \times 3$. Each conv layer is followed by a batch normalization layer and a ReLU activation. Shallow + narrow 3D CNN is formed of three 3D conv layers and three 3D max-pooling layers. Its first 3D conv layer is composed of 16 filters and the next two conv layers are composed of 32 filters each. The padding is set to “same” and the stride is set to 1. The pooling size and the stride are both set to 2. In the shallow + wide configuration, we change the 3D CNN by doubling the number of filters in each conv layer. For the deep + narrow architecture, we increase the number of 3D conv layers from three to six. Finally, in the deep + wide configuration, we double the number of layers as well as the number of filters in each conv layer with respect to the shallow + narrow model. In the middle object prediction head, incorporate a decoder formed of upsampling and 2D conv layers based on $3 \times 3$ filters. The last conv layer in the decoder has only three filters in order to reconstruct the RGB input. The other three prediction heads share the same configuration, having a 2D conv layer with 32 filters and a maxpooling layer with a spatial support of $2 \times 2$. The last layer is a fully-connected layer with either two units to predict the arrow of time and motion irregularity or 1080 units to predict the teachers’ output scores for the 1000 ImageNet classes and 80 MS COCO categories.
<h3>2.3 Proxy task and joint learning</h3>
- Arrow of time: Generate two labeled training samples from each object-centric sequence. The first sample takes the frames in their temporal order, namely $(i - t, ..., i - 1, i, i + 1, ..., i + t)$. The second sample takes the frames in reversed order, namely $(i + t, ..., i + 1, i, i - 1, ..., i - t)$. Let $f$ be the shared 3D CNN and $h_{T_1}$ be the arrow of time head. Let $X^{(T_1)}$ ) be a forward or backward object-centric sequence, $\mathcal{L}_{T_1} = (X^{(T_1)}, Y^{(T_1)}) = -\sum_{k = 1}^2Y_k^{(T_1)}\log(\hat{Y}^{(T_1)}_k)$, where $\hat{Y}^{(T_1)} = softmax(h_{T_1}(f(X^{(T_1)}))$ and $Y^{(T_1)}$ is the one-hot encoding of ground-truth label for $X^{(T_1)}$.
- Motion irregularity: generate two labeled training samples from each object-centric sequence. The first example captures an object in consecutive frames from $i - 1$ to $i + t$. . The intermittent object-centric sequence is created by retaining the frame $i$, then n gradually adding $t$ t randomly chosen previous frames and t randomly chosen succeeding frame. Let $h_{T_2}$ be the irregular motion head and $X^{(T_2)}$ be a regular or irregular object-centric sequence, $\mathcal{L}_{T_2}(X^{(T_2)}, Y^{(T_2)}) = -\sum_{k = 1}^2Y_k^{(T_2)}\log(\hat{Y}^{(T_2)}_k)$, where $\hat{Y}^{(T_2)} = softmax(h_{T_2}(f(X^{(T_2)}))$ and $Y^{(T_2)}$ is the one-hot encoding of ground-truth label for $X^{(T_2)}$.
- Middle bounding box prediction: Let $h_{T_3}$ be the middle bounding box prediction, $\mathcal{L}_{T_3}(X^{(T_3)}, Y^{(T_3)}) = \frac{1}{h \cdot w \cdot c}\sum_{j = 1}^h\sum_{k = 1}^w\sum_{l = 1}^c|Y^{(T_3)}_{jkl} - \hat{Y}^{(T_3)}_{jkl}|$, where $\hat{Y}^{(T_3)} = h_{T_3}(f(X^{(T_3)}))$.
- Model distillation: In order to distill the knowledge from the YOLOv3 and ResNet-50 teachers, student 3D CNN model receives the same input as ResNet-50 and learns to predict the pre-softmax features $Y_{ResNet}^{(T_4)}$ of ResNet-50 and class probabilities $Y^{(T_4)}_{YOLO}$ predicted by YOLOv3. Let $X^{(T_4)}$ X(T4) be the input image comprising a detected object and $h_{T_4}$ be the knowledge distillation, $\mathcal{L}_{T_4}(X^{(T_4)}, Y^{(T_4)}) = \frac{1}{n}\sum_{j = 1}^n|Y_j^{(T_4)} - \hat{Y}_j^{(T_4)}|$, where $\hat{Y}^{(T_4)} = h_{T_4}(f(X^{(T_4)}))$ and $Y^{(T_4)} = Y^{(T_4)}_{ResNet} \oplus Y^{(T_4)}_{YOLO}$ is the is the concatenation of the 1000 ResNet-50 pre-softmax features and the 80 YOLOv3 class probabilities, resulting in a vector of $n$ = 1080 components. 
- Joint loss: $\mathcal{L}_{total} = \mathcal{L}_{T_1} + \mathcal{L}_{T_2} + \mathcal{L}_{T_3} + \lambda \cdot \mathcal{L}_{T_4}$, where $\lambda \in (0, 1]$ (0, 1] is a weight that regulates the importance of the knowledge distillation task.
<h2>3. Datasets</h2>
Avenue, ShanghaiTech and UCSD Ped 2.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/lilygeorgescu/AED-SSMTL.