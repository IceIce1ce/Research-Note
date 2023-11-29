<h2>1. Abstract</h2>
Propose an online anomaly detection method for surveillance videos using transfer learning and continual learning and provides a mechanism for continually learning from recent data without suffering from catastrophic forgetting.
<h2>2. Proposed method</h2>
<h3>2.1 Feature selection</h3>
Create a feature vector $F^i_t$ for each object $i$ in frame $X_t$ at time $t$, where $F^i_t$ is given by $[w_1F_{motion}, w_2F_{location}, w_3F_{appearance}]$. The weights $w_1, w_2, w_3$ are used to adjust the relative importance of each feature category.
<h3>2.2 Object detection</h3>
Get a bounding box (location) along with the class probabilities (appearance) for each object detected in frame $X_t$. Instead of simply using entire bounding box, monitor the center of the box and its area to obtain location features.
<h3>2.3 Optical flow</h3>
Monitor the contextual motion of different objects in a frame using a pre-trained optical flow model such as Flownet 2. Extract the mean, variance, and the higher order statistics skewness and kurtosis, which represent asymmetry and sharpness of the probability distribution, respectively.
<h3>2.4 Feature vector</h3>
Combining the motion, location, and appearance features, for each object i detected in frame Xt to construct the feature vector.
<h3>2.5 Training</h3>
Given a set of $N$ training videos containing $P$ frames in total, leverage the deep learning module of our proposed detector to extract $M$ feature vectors $\mathcal{F}^M = {F^i}$ for $M$ detected objects in total such that $M \geq P$. These $M$ vectors correspond to $M$ points in the nominal data space. Propose to use kNN distance which captures the local interactions between nominal data points. The training procedure of detector: 
- Randomly partition the nominal dataset $\mathcal{F}^M$ into two sets $\mathcal{F}^{M_1}$ and $\mathcal{F}^{M_2}$ such that $M = M_1 + M_2$. 
- Then, for each point $F^i$ in $\mathcal{F}^{M_1}$, compute the kNN distance $d_i$ with respect to points in set $\mathcal{F}^{M_2}$. 
- For a significance level $\alpha$, the (1 - $\alpha$)th percentile $d_\alpha$ of kNN distance {$d_1,...,d_{M_1}$} is used as a baseline statistic for computing the anomaly evidence of test instances.
<h3>2.6 Testing</h3>
For each object $i$ detected at time $t$, the deep learning module constructs the feature vector $F^i_t$ and computes the kNN distance $d^i_t$ with respect to the training instances in $\mathcal{F}^{M_2}$. The proposed algorithm then computes the instantaneous frame-level anomaly evidence $\delta_t$: $\delta_t = (max_i {d^i_t})^m - d^m_\alpha$, where $m$ is the dimensionality of feature vector $F^i_t$. Finally, following a CUSUM-like procedure, update the running decision statistic $s_t$ as: $s_t = \mathrm{max}\{s_{t - 1} + \delta_t, 0\}, s_0 = 0$. For nominal data, $\delta_t$ gets negative values and otherwise. A video frame is anomalous if the decision statistic $s_t$ exceeds the threshold $h$.
<h3>2.7 Continual learning</h3>
During testing, if the test statistic $s_t$ at time $t$ is zero, the feature vector $F^i_t$ is considered nominal, then feature vector is included in second nominal training set $\mathcal{F}^{M_2}$. When the statistic $s_t$ crosses the threshold $h$, an alarm is raised. . At this point, propose a human-in-the-loop approach in which a human expert labels false alarms from time to time. If it is labeled as a false alarm, all vectors $\{F^i_\tau\}$ between $\tau_{start}$ and $t$ are added to $\mathcal{F}^{M_2}$ so as to prevent similar future false alarms. 
<h2>3. Datasets</h2>
UCSD, Avenue, ShanghaiTech.
<h2>4. Metrics</h2>
AUC.