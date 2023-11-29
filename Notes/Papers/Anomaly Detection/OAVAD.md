<h2>1. Abstract</h2>
This paper proposes a novel two-stream object-aware VAD method that learns the normal appearance and motion patterns through image translation tasks. The appearance branch translates the input image to the target semantic segmentation map produced by Mask-RCNN, and the motion branch associates each frame with its expected optical flow magnitude.
<h2>2. Proposed method</h2>
<h3>2.1 The appearance branch</h3>
In the appearance branch, we train a U-net with a pretrained resnet34 (trained on the ImageNet) as an encoder. The target semantic segmentation maps of the frames are acquired using Mask-RCNN.
<h3>2.2 The motion branch</h3>
An identical Unet is trained to learn translation from the input image to its corresponding optical flow magnitude map.
<h3>2.3 Masking</h3>
To focus on the motion of the objects, apply the calculated semantic segmentation mask on the calculated target optical flow map. In this way, the background motion is suppressed which helps the network focus and learn the motion of the detected objects.
<h3>2.4 Training</h3>
The optimization follows the following target function: $Argmin_w(diff(T(I_t), F_w(I_t))$, where $T, F_w$ represent the target image for the input I (at time t) and function of the network (Unet). Leverage L1 loss for the optical flow since it does not magnify the effect of the noise. For the appearance network, leveraged the L2 loss, to calculate the difference between the output and the target image. The patchbased loss is calculated for the appearance and motion branches: $L_{ss} = Max(L_{ss1},...,L_{ssk}), L_{of} = Max(L_{of1},...,L_{ofk})$, where $L_{ssi} = ||P_i(f1_w(I_t)) - Pi(SM(I_t))||^2, L_{ofi} = ||Pi(f2_w(I_t)) - Pi(OFM(I_{t - 1}, I_t))||$, where $Pi$ selects $i_{th}$ patch in the frame, $f1_w, f2_w$ denote the networks of the appearance and motion branches.
<h3>2.5 Refinement</h3>
Apply few steps of the erosion and dilation operations respectively on the anomaly map to tackle the mentioned problem of non-invariance to the number of objects.
<h3>2.6 Temporal denoising</h3>
Apply the Savitzky–Golay filter on the anomaly scores of the frames, in order to smooth the anomaly scores and remove the noise.
<h2>3. Datasets</h2>
ShanghaiTech, UCSD-Ped1 and UCSD-Ped2.
<h2>4. Metrics</h2>
AUC.