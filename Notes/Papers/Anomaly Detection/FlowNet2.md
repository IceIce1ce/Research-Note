<h2>1. Abstract</h2>
Three major contributions: first, focus on the training data and show that the schedule of presenting data during training is very important. Second, develop a stacked architecture that includes warping of the second image with intermediate optical flow. Third, elaborate on small displacements by introducing a subnetwork specializing on small motions.
<h2>2. Stacking networks</h2>
<h3>2.1 Stacking two networks for flow refinement</h3>
The first network in the stack always gets the images $I_1$ and $I_2$ as input. Subsequent networks get $I_1, I_2$ and previous flow estimate $w_i = (u_i, v_i)^T$, where $i$ denotes the index of network in the stack. Also optionally warp the second image $I_2(x, y)$ via the flow $w_i$ and bilinear interpolation to $\overline{I}_{2, i}(x, y) = I_2(x + u_i, y + v_i)$. When using warping, additionally provide $\overline{I}_{2, i}$ and the error $e_i = ||\overline{I}_{2, i} - I_1||$ as input to the next network.
<h3>2.2 Stacking multiple diverse networks</h3>
Rather than stacking identical networks, it is possible to stack networks of different type (FlowNetC and FlowNetS).
<h2>3. Small Displacements</h2>
- FlowNet2-CSS-ft-sd: fine-tuned our FlowNet2-CSS network for smaller displacements by further training the whole network stack on a mixture of Things3D and ChairsSDHom and by applying a non-linearity to the error to down weight large displacements. 
- FlowNet2-SD: make the beginning of network deeper by exchanging the 7 $\times$ 7 and 5 $\times$ 5 kernels in the beginning with multiple 3 $\times$ 3 kernels. Because noise tends to be a problem with small displacements, add convolutions between the upconvolutions to obtain smoother estimates.
- FlowNet2: created a small network that fuses FlowNet2-CSS-ft-sd and FlowNet2-SD. The fusion network receives the flows, the flow magnitudes and the errors in brightness after warping as input. It contracts the resolution twice by a factor of 2 and expands again.
<h2>4. Datasets</h2>
Sintel, KITTI, Middlebury.
<h2>5. Metrics</h2>
AEE, F1-all.
<h2>6. Code</h2>
https://github.com/NVIDIA/flownet2-pytorch.