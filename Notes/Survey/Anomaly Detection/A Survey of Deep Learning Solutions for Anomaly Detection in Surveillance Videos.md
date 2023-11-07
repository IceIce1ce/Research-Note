<h2>1. Abstract</h2>
This review attempts to provide holistic benchmarking of the published deep learning solutions for video anomaly detection since 2016. The paper identifies, the learning technique, datasets used and overall model accuracy. Reviewed papers were organised into five deep learning methods: autoencoders, continual learning, transfer learning, reinforcement learning and ensemble learning.
<h2>2. Introduction</h2>
This paper examined the deep learning models published since 2016. Only the deep learning solutions in anomaly detection in surveillance videos were considered. 30 papers were reviewed and summarized for better analysis. Datasets used for training and testing, model accuracy, learning techniques and the underlying deep learning algorithms are identified and compared for ranking.
<h2>3. Related works</h2>
Our literature survey provides a comprehensive road map of deep anomaly detection in videos. The works published in this area are clustered according to learning technique. The methodologies used to detect anomalies are analyzed and compared to uncover the rapid changes and their motivations. This paper explores more publications than its peers and aims at establishing the trends and some ranking of the best model.
<h2>4. Deep learning models</h2>
<h3>4.1 Transfer learning</h3>
One strategy of implementing transfer learning is through feature extraction. Pretrained models were used to extract features from labelled video and imagery data. The pre-trained models used mostly in the reviewed models include deep 3-dimensional convolutional networks (C3D) model, Inception V3 module, I3D and YOLOv3.
<h3>4.2 Autoencoders</h3>
Integrate a Conv-AE and Inception module to form a deep autoencoder, integrate convolutional autoencoder and convolutional LSTM (use optical flow to extract features of speed and trajectory from videos), Conv-LSTM and Spatial-Temporal autoencoder.
<h3>4.3 Ensemble learning</h3>
Combine both a 3D convolutional network and FC network, combine conditional generative adversarial networks, R-CNN and SVM.
<h3>4.4 Continual learning</h3>
The continual learning was just a part of model that enabled newly learnt anomalies to be added to knowledge without losing previous knowledge. The anomaly detection part utilized euclidian distance k in KNN to identify anomalies that lie away from nominal manifold.
<h3>4.5 Reinforcement learning</h3>
Use deep Q learning to locate anomalies in videos.
<h2>5. Datasets</h2>
UCF-Crime, UCSD Ped1 & Ped2, Avenue, UMN and ShanghaiTech.
<h2>6. Metrics</h2>
ROC, AUC, ERR and F1 Score.
<h2>7. Trends</h2>
Transfer learning and autoencoders deep learning techniques have taken the biggest part of the reviewed papers. Continual and reinforcement learning is new direction.