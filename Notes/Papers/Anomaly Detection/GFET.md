<h2>1. Abstract</h2>
This paper reports a study on video anomaly detection, focusing on the analysis of feature embeddings of pre-trained CNNs with the use of novel cross-domain generalization measures that allow to study how source features generalize for different target video domains.
<h2>2. Method</h2>
All datasets are individually mapped to a feature space using the same pre-trained VGG-19. For every domain pair (source A, target B), the training set of A is used to train an anomaly detector model (OC-SVM), which is then employed on the test set of B. Afterwards, evaluate the generalization between A and B and the impact of transfer learning methods on this generalization. Three scenarios were investigated: (i) without any data transformation; (ii) with PCA applied across domains; and (iii) with TCA.
<h2>3. Datasets</h2>
Canoe, Boat-River, Boat-Sea, Ped1/2, Belleview and Train.
<h2>4. Metrics</h2>
AUC and EER.