<h2>1. Abstract</h2>
This paper provides a new perspective and insights on the performance of video anomaly detection methods. It reports the results of a benchmark study with state-of-the-art methods using a novel proposed framework for evaluating and comparing the different models. The results of this benchmark demonstrate that using the currently employed set of reference metrics led to the misconception that weakly-supervised methods consistently outperform semi-supervised ones.
<h2>2. Datasets</h2>
CUHK Avenue, ShanghaiTech and UCF-Crime.
<h2>3. Selected methods for evaluation</h2>
FFP, MLEP, Sultani and RTFM.
<h2>4. Metrics</h2>
ROC, TPR, FPR, F1-score, Recall, Precision, Accuracy and PSNR.
<h2>5. Future work</h2>
- The implementation of method-agnostic solutions to reduce punctual false positives could prove essential to improve the performance of current methods without requiring any change in their architecture. One approach could be to apply a filter to the anomaly scores to mitigate instances that are likely to be false positives.
- Additionally, there is an opportunity to improve solutions based on MIL by leveraging temporal relationships within bags. By considering the temporal relationships between instances, the ranking process could achieve better results in selecting the potentially anomalous sequences within the instances contained in the bag. Self-labelling has been shown to be effective and could also be a valuable approach to improve MIL.