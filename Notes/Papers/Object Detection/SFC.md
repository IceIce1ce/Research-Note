<h2>1. Abstract</h2>
Train a standalone object-centric context representation to perform the opposite task: seeing what is not there. Given an image, model can predict where objects should exist, even when no object instances are present.
<h2>2. Introduction</h2>
An object can be defined as missing in an image region when:
- An object detector finds nothing.
- A predictor of the object's typical environment indicates high probability of its existence.
<h2>3. Related work</h2>
<h3>3.1 Context in object recognition</h3>
Usually, context is represented by semantic labels around an object. Context Challenge, Visual Memex.
<h3>3.2 Finding missing objects</h3>
General hough transform.
<h3>3.3 Accessibility tasks</h3>
CrossingGuard, Turk. Tohme.
<h2>4. Learning context from explicit object masks</h2>
If context is defined to be everything that surrounds an object except the object itself, this model is learning context literally: every target object instance in training images is masked out. This is a binary classification problem. Positive samples are collected so that each image sample has an object at its center with a black mask covering the object's full extent. The bounding box width to the whole image width ratio is set to 1/4 for the purpose of including a larger contextual area. Negative samples are random crops with similar black masks at their centers. The position of a negative crop is chosen so that the masked region will not cover any groundtruth labeled objects with more than a Jaccard index of 0.2. The sampling strategy is to interleave the positive sampling and negative sampling processes and use the previous positive sample's mask dimension in the next negative sample. Train a CNN model Q with Cross entropy: $\mathcal{L}_c = -Q_y(I_m) + \log\sum_y e^{Q_y(I_m)}$, where $y \in \{1, 2\}$ is he groundtruth label for a masked image $I_m$ (1 for positive, 2 for negative), $Q(I_m)$ is a 2 $\times$ 1 vector representing the output from the network $Q$ while $Q_y(I_m)$ represents its $y$-th element. During test time, a sliding window approach is used to generate a probability heat map for a new image so that each pixel has a context score of how likely it is to contain an object.
<h2>5. A full convolutional model that learns implicit masks</h2>
Minimize a distance loss in addition to the classification loss used in the base network: $\mathcal{L}_d = ||Q(I_m) - Q(I)||_p$, where $Q(I_m)$ is the output vector from the network $Q$ with masked image $I_m$ as the input, $Q(I)$ is the output vector from $Q$ with the unmasked raw image $I$ as the input. One stream of the network computation takes a masked image as input and outputs $Q(I_m)$. In parallel, the other stream of network computation takes the unmasked raw image as input and outputs $Q(I)$. The overall training objective: $\mathcal{L} = \lambda\mathcal{L}_d + \mathcal{L}_c$, where $\lambda$ = 0.5. 
<h2>6. Finding missing object regions pipeline</h2>
- Generate a context heat map using the context network $Q$. 
- Generative object detection results using any object detector. Convert detection boxes into a binary map by assigning 0 to the detected box region, 1 otherwise.
- Perform element-wise multiplication between the context heatmap and the binary map.
- Crop the high scored regions from the image according to the resulting map.
<h2>7. Datasets</h2>
Street view curb ramps.
<h2>8. Metrics</h2>
Recall@K.