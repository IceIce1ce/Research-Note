<h2>1. Abstract</h2>
Present a comprehensive overview of deep learning methods to address AD problem. Firstly, different classifications of anomalies are introduced and then DL methods and architectures used for video AD are discussed and analyzed. Moreover, several applications of video AD have been discussed. Finally, challenges and future directions for further research are outlined.
<h2>2. Classifications of anomaly detection</h2>
- Image-based and video-based detection.
- Single-point anomaly and group anomalies.
<h2>3. DL methods of AD</h2>
<h3>3.1 Supervised learning-based AD for video streaming</h3>
VGG16/19, GRUs. Advantage: ability to gather data and produce data output from prior knowledge. Furthermore, they are simpler and have higher performance. Disadvantage: decision boundaries might be overstrained when the training set lacks examples that belong to a class. In addition, they require precise labels for a variety of normal and anomalous classes which are often not available.
<h3>3.2 Semi-supervised learning-based AD for video streaming</h3>
GANs, GRUs and LSTMs.
<h3>3.3 Unsupervised learning-based AD for video streaming</h3>
AEs, Restricted Boltzmann Machines, Deep Belief Networks, GANs, GRUs and LSTM.
<h3>3.4 Transfer learning-based AD for video streaming</h3>
use pre-trained weights from CNNs.
<h3>3.5 Deep active learning-based AD for video streaming</h3>
Active learning is the process by which an algorithm repeatedly asks human annotators for labels to enhance training and improve performance. It reduces the ambiguous nature of anomalies in AD by introducing suitable priors with assistance of a domain expert. 
<h3>3.6 Deep reinforcement learning-based AD</h3>
It learns to strike a balance between taking advantage of its current data model and looking for new classes of anomalies.
<h3>3.7 Deep hybrid models-based AD for video</h3>
Combine multiple models together in such a way as to improve AD for video streaming.
<h2>4. DL architectures for AD</h2>
Two-stream convolutional architecture, 3D convolution architecture, ConvLSTM architecture, using human skeleton data, miscellaneous architectures.
<h2>5. Datasets</h2>
CAVIAR, UMN, UCSD, BEHAVE, Hockey fight, HMDB-51, Violent Flow, UCF-101, ActivityNet and RLVS. 
<h2>6. Metrics</h2>
- Accuracy = $\frac{TP + TN}{TP + TN + FP + FN}$.
- EER = $\frac{FP + FN}{TP + TN + FP + FN}$.
- Recall = $\frac{TP}{TP + FN}$.
- Precision = $\frac{TP}{TP + FP}$.
- Specificity = $\frac{TN}{FP + TN}$. 
- FPR = $\frac{FP}{FP + TN}$.
- FNR = $\frac{FN}{FP + TN}$.
- F1-Score = 2 $\times \frac{\mathrm{Precision} \times \mathrm{Recall}}{\mathrm{Precision} + \mathrm{Recall}}$.
- J Score = Sensitivity + Specificity - 1.
- PWC = 100 $\times \frac{FP + FN}{TP + TN + FP + FN}$.
- ROC and AUC.
<h2>7. Applications of AD for video</h2>
Automated surveillance, autonomous driving, industrial automation, intelligent traffic monitoring and surgical robotics.
<h2>8. Research challenges in DL-based AD approaches</h2>
Anomaly characteristics, anomaly definition, environmental factors, division of dataset, data diversity, data annotation, feature normalization, model generalization, real time systems and lightweight models.
<h2>9. Future directions</h2>
Aerial surveillance, AD from moving cameras, self-supervised learning in video, human-robot collaboration and ensemble approaches.