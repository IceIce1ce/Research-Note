<h2>1. Abstract</h2>
Firstly, present a structured and comprehensive overview of research methods in deep learning-based anomaly detection. Furthermore, review the adoption of these methods for anomaly across various application domains and assess their effectiveness. Group deep anomaly detection research techniques into different categories based on the underlying assumptions and approach adopted. Besides, for each category, present the advantages and limitations and discuss the computational complexity of the techniques in real application domains. Finally, outline open issues in research and challenges faced.
<h2>2. Different aspects of deep learning-based anomaly detection</h2>
<h3>2.1 Nature of input data</h3>
Input data can be broadly classified into sequential or non-sequential data. Additionally input data depending on the number of features (or attributes) can be further classified into either low or high-dimensional data.
<h3>2.2 Based on availability of labels</h3>
Supervised deep anomaly detection, semi-supervised deep anomaly detection and unsupervised deep anomaly detection.
<h3>2.3 Based on the training objective</h3>
Deep hybrid models, one-class neural networks.
<h3>2.4 Type of anomaly</h3>
- Point anomalies: represent an irregularity or deviation that happens randomly and may have no particular interpretation.
- Contextual anomaly detection: is a data instance that could be considered as anomalous in some specific context. Contextual anomaly is identified by considering both contextual and behavioural features. The contextual features, normally used are time and space. While the behavioral features may be a pattern of spending money, the occurrence of system log events or any feature used to describe the normal behavior.
- Group anomaly: each of the individual points in isolation appears as normal data instances while observed in a group exhibit unusual characteristics.
<h2>3. Applications of deep anomaly detection</h2>
Intrusion detection, fraud detection, malware detection, medical anomaly detection, deep learning for anomaly detection in social networks, log anomaly detection, IoT big data anomaly detection, industrial anomalies detection, anomaly detection in time series, video surveillance.
<h2>4. Deep anomaly detection models</h2>
<h3>4.1 Supervised deep anomaly detection</h3>
- Advantages:
	- Supervised DAD methods are more accurate than semi-supervised and unsupervised models.
	- The testing phase of classification based techniques is fast since each test instance needs to be compared against the precomputed model.
- Disadvantages:
	- Multi-class supervised techniques require accurate labels for various normal classes and anomalous instances which is often not available.
	- Deep supervised techniques fail to separate normal from anomalous data if the feature space is highly complex and non-linear.
<h3>4.2 Semi-supervised deep anomaly detection</h3>
- Advantages:
	- GANs trained in semi-supervised learning mode have shown great promise, even with very few labeled data.
	- Use of labeled data (usually of one class) can produce considerable performance improvement over unsupervised techniques.
- Disadvantages:
	- Applicable even in a deep learning context.
	- The hierarchical features extracted within hidden layers may not be representative of fewer anomalous instances hence are prone to over-fitting problem.
<h3>4.3 Hybrid deep anomaly detection</h3>
- Advantages:
	- Feature extractor significantly reduces the curse of dimensionality, especially in the high dimensional domain.
	- Hybrid models are more scalable and computationally efficient since the linear or nonlinear kernel models operate on reduced input dimension.
- Disadvantages:
	- The hybrid approach is suboptimal because it is unable to influence representational learning within the hidden layers of feature extractor since generic loss functions are employed instead of the customized objective for anomaly detection.
	- The deeper hybrid models tend to perform better if the individual layers are which introduces computational expenditure.
<h3>4.4 One-class neural networks for anomaly detection</h3>
- Advantages:
	- OC-NN models jointly train a deep neural network while optimizing a data-enclosing hypersphere or hyperplane in output space.
	- OC-NN propose an alternating minimization algorithm for learning the parameters of OC-NN model. Subproblem of OC-NN is equivalent to a solving a quantile selection problem which is well defined.
- Disadvantages:
	- Training times and model update time may be longer for high dimensional input data.
	- Model updates would also take longer time, given the challenge in input space.
<h3>4.5 Unsupervised deep anomaly detection</h3>
- Advantages:
	- Learns the inherent data characteristics to separate from an anomalous data point. This techniques identifies commonalities within the data and facilitates outlier detection.
	- Cost effective technique to find anomalies since it does not require annotated data for training the algorithms.
- Disadvantages:
	- Often it is challenging to learn commonalities within data in a complex and high dimensional space.
	- While using autoencoders the choice of right degree of compression is often an hyper-parameter that requires tuning for optimal results. 
<h3>4.6 Miscellaneous techniques</h3>
Transfer learning based anomaly detection, zero shot learning based anomaly detection, ensemble based anomaly detection, clustering based anomaly detection, deep reinforcement learning based anomaly detection, statistical techniques deep anomaly detection.
<h3>4.7 Deep neural network architectures for locating anomalies</h3>
Deep neural networks, spatio temporal networks, sum-product networks, word2vec models, generative models, convolutional neural networks, sequence models, autoencoders.