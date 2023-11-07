<h2>1. Abstract</h2>
propose a novel supervised anomaly detection model which explicitly detects open normal events and open abnormal events in the dataset and treats open data and seen data with different classifiers. First, we use the training video to train an imbalanced classifier as the seen data classifier. T hen, d uring the t esting p hase, a n open data filter m odule i s u sed t o d ivide t he t est d ata i nto s een data and open data. Finally, we directly use the seen data classifier to generate anomaly scores for the seen test data. For the open test data, we adopt a domain adaptation method to reduce the distribution difference between it and the training data and train a new classifier to score for it.
<h2>2. Proposed model</h2>
In the training stage, use a pretrained neural network to extract the appearance features of all the training video frames $X_{train}$ and then use $X_{train}$ to train an imbalanced classifier. In the testing phase, the appearance features $X_{test}$ extracted by the pre-trained neural network are put into the open data filter, and $X_{test}$ are divided into open data $X_{test\_open}$ and seen data $X_{test\_seen}$. For the seen data, directly use the seen data classifier to calculate its anomaly scores. For the open data, a domain adaptation module is used to reduce the distribution difference between it and the training data, and construct new feature representations denoted as $X_{train\_new}$ and $X_{open\_new}$. Then, use $X_{train\_new}$ to retrain a new imbalanced classifier, and use it to generate anomaly scores for the open data. Finally, anomaly scores of the open data and seen data are integrated to get the anomaly detection results of the entire testing videos.
<h3>2.1 Feature extraction</h3>
Extract appearance features $X = \{X_1 = \{x^1_1,...x^j_1\},...,X_i = \{x^1_i,...,x^m_i\}\}$ of input videos, where $x^j_i$ is the $j$th frame-level feature representation of $i$th video. 
<h3>2.2 Open data filter</h3>
Use Euclidean distance between the test data and the training data to measure the openness of the test data apply k-means clustering to divide the training data into multiple categories, and obtain clusters representing various types of the training data. After that, calculate the distance between each frame $x^j_i$ in the test video and the cluster centroid $C$ of each type of the training data. For each test frame $x^j_i$ in the test set, , the open probability relative to the training set is defined: $p_{open}(x^j_i, X_{train}) = \mathrm{min}_n||x^j_i - C_n||_2, \forall_n \in \{1,...,k\}$. Use the Mann-Whitney U test to determine the optimal proportion of open data. Assume that the ratio of open data to test data is [0.1,0.5], we can filter out open data $X_{test\_open}$ according to ratio range to obtain a series of seen data $X_{test\_seen}$. Then the mean values of $X_{test\_seen}, X_{train\_nor}$ and $X_{train\_abnor}$ are calculated and denoted as $M_{test\_seen}, M_{nor}$ and $M_{abnor}$. Finally, the U-test method is used to obtain two $P$ values denoted as $P_{nor}$ and $P_{abnor}$, where $P_{nor}$ is ther $P$ value of $M_{test\_seen}$ and $M_{nor}$ and $P_{abnor}$ is that of $M_{test\_seen}$ and $M_{abnor}$. The best open ratio is: argmax$P_{nor}(r) + P_{abnor}(r), \forall r \in [0.1, 0.5]$.
<h3>2.3 Domain adaptation of open data</h3>
Regard the training data as the source domain $X_s$ and open data as target domain $X_t$. Use the Joint Distribution Adaptation (JDA) to simultaneously minimize these two distributions between the source and target domains. In detail, the JDA method minimizes the distribution difference between domains through Maximum Mean Discrepancy distance and get a transformation matrix $A$. The, use $A$ to obtain new feature representations of the training data and open data: $X_{train\_new} = A^TX_s, X_{open\_new} = A^TX_t$. Finally, the new training data representations are used to learn a new imbalanced classifier for the open data.
<h3>2.4 Anomaly score prediction</h3>
First use the Sequential model in Keras to build a neural network. Focal loss is used to optimize the model. Use $g_s(\theta_s)$ and $g_n(\theta_n)$ to represent the classifier trained with the original training data $X_s$ and new source training data $X_{train\_new}$. For the seen data in the test set, directly use $g_s(\theta_s)$ to generate anomaly score of the $j$th frame of $i$th video: $s(x^j_i) = g_s(\theta_s, x^j_i)$. For the open data, use its new feature representation $X_{open\_new}$ to generate anomaly score with the classifier $g_n(\theta_n): s(x^j_{i\_new}) = g_n(\theta_n, x^j_{i\_new})$. Finally, anomaly scores of the seen data and open data are integrated to evaluate the anomaly detection results of the entire testing videos.
<h2>3. Datasets</h2>
Avenue and ShanghaiTech.
<h2>4. Metrics</h2>
AUC.