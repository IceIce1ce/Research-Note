<h2>1. Abstract</h2>
Proposed a neural network containing three modules for fusing multimodal information: 1) attention module for utilizing weighted features to generate effective features based on the mutual guidance between visual and audio information; 2) fusion module for integrating features by fusing visual and audio information based on the bilinear pooling mechanism; and 3) mutual Learning module for enabling the model to learn visual information from another neural network with a different architecture.
<h2>2. Methods</h2>
<h3>2.1 Attention module</h3>
Denote $X_v \in R^{C \times T}, X_a \in R^{C \times T}$ as visual and audio input features where $T$ is total time steps and $C$ is number of visual and audio feature dimensions. The dependency map $A$ is defined: $A = X^T_vW^T_1W_2X_a$, where $W_1 \in R^{C \times C}, W_2 \in R^{C \times C}$ are trainable parameters implemented by 1D-convolutional layer with kernel size 1 (Conv1D-C1 and Conv1D-C2). The outputs of co-attention are defined: $X_{cv} = \alpha_aW_4X_a\xi(A^T) + X_v; X_{ca} = \alpha_vW_3X_v\xi(A) + X_a$, where $\xi$ is softmax. The final output of attention module is: $X_v' = f_c(X_{cv}, X_{sv}); X_a' = f_c(X_{ca}, X_{sa})$, where $f_c$ is a concatenation operation and $X_{sv} \in R^{C \times T}$ and $X_{sa} \in R^{C \times T}$ are outputs of corresponding self-attention modules.
<h3>2.2 Fusion module</h3>
The fusion module output $X_f$ can be achieved as: $z = W_vX_v' \odot W_aX_a', X_f = sign(z)|z|^{0.5}$, where $W_v, W_a$ are the trainable parameters implemented by 1D-convoultional layer with kernel size 3.
<h3>2.3 Mutual learning module</h3>
It contains only a self-attention module. Feed visual features instead of audio features to the mutual learning module. MSE is utilized instead of KD loss.
<h3>2.4 Training and determination methods</h3>
$K$ instances with the highest scores are employed to calculate the training loss with the MSE function: $L_b(l, Y) = \frac{||l - f_{K-max}(Y)||^2_2}{K}$, where $Y = [y_1,...,y_T]$ is prediction and $l \in R^K$ os ground-truth. The final loss is defined: $L = L_b(l, Y_{mas}) + L_b(l, Y_{ml}) + \gamma L_{mut}(Y_{mas}, Y_{ml})$, where $Y_{mas}, Y_{ml}$ are the predictions for the master branch and mutual learning module. The combination violence score with weight $\beta$ is defined as: $S = (1 - \beta)Y_{mas} + \beta Y_{ml}$.
<h2>3. Datasets</h2>
XD-Violence.
<h2>4. Metrics</h2>
AP.