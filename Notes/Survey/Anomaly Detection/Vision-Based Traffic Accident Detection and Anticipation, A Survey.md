<h2>1. Abstract</h2>
Present the first survey on vision-TAD in the deep learning and first-ever survey for vision-TAA. Also provide a critical review of 31 publicly available benchmarks and related evaluation metrics.
<h2>2. Vision-based traffic accident detection</h2>
<h3>2.1 Frame-level deep vision-TAD methods</h3>
Frame feature classification and synthetic frame feature adaptation.
<h3>2.2 Object-centric deep vision-TAD methods</h3>
Deep vision-TAD by trajectory classification, deep vision-TAD by location consistency and deep vision-TAD by scene context consistency.
<h2>3. Vision-based traffic accident anticipation</h2>
<h3>3.1 Vision-TAA with object trajectory prediction</h3>
Early research pipelines in Vision-TAA with object location association borrow the trajectory prediction models and follow the pipeline of detection-tracking-trajectory predictionaccident determination.
<h3>3.2 Vision-TAA with object feature interaction</h3>
The frequent occlusion of road participants makes the aforementioned physical distance measurement infeasible. Therefore, some works begin to explore the interactive relation within the participant group.
<h3>3.3 Vision-TAA with object risk localization</h3>
Traffic accidents certainly involve a safe risk for further movement, and risk localization aims to localize the risk regions or objects for early accident prediction as much as possible.
<h3>3.4 Vision-TAA with driver attention collaboration</h3>
Introduce driver attention which is an active vision clue for safe driving, inspired by the mechanism of human selective attention. Commonly, driver attention also reflects the driving task indirectly because it implies where we want to go or look.
<h3>3.5 Loss functions</h3>
For the accident anticipation loss, most aforementioned works adopt the Exponential Loss to penalize the positive accident video clips for an early accident prediction and a cross-entropy loss for the negative video clips.
<h2>4. Available datasets</h2>
- Vision-TAD datasets: A3D, DVAD, Drive-Anomaly106, RetroTrucks, TNAD, ADV, TAD-1, TaskFix, USDC, TrafficS, DADA-2000, DoTA, TAD-2, DADA-seg, MP-RAD, DRAMA and CTAD.
- Vision-TAA datasets: DAD, Epic Fail, NIDB, CADP, VIENA, GTACrash, CTA, CCD, YouTubeCrash, SUTD-TrafficQA, TRA, DADA-2000, CAP, ROL and DeepAccident.
<h2>5. Evaluation metrics</h2>
<h3>5.1 Metrics for vision-TAD</h3>
AUC, STAUC, Precision, Recall and F1.
<h3>5.2 Metrics for vision-TAA</h3>
AP, TTA and mTTA.
<h2>6. Future insights</h2>
Toward explainable vision-TAD and vision-TAA, scalable and causal vision-TAD and Vision-TAA, label efficient learning for vision-TAD, multi-modal fusion for vision-TAD and vision-TAA, borrowing real-to-sim-to-real framework, vision-TAD and vision-TAA in V2X scenarios, continual reinforcement vision-TAA.