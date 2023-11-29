<h2>1. Introduction</h2>
Propose DMAD to enhance reconstruction diversity. Design PDM which models diverse normals and measures the severity of anomaly by estimating multi-scale deformation fields from reconstructed reference to original input. Integrated with an information compression module, PDM essentially decouples deformation from prototypical embedding and makes the final anomaly score more reliable.
<h2>2. Diversity-measurable anomaly detection</h2>
<h3>2.1 The framework</h3>
Design information compression module $\phi(\cdot)$ and diversity-aware module $\psi(\cdot)$ under DMAD framework: $L = ||x - g(\phi(f(x), z)) \odot \psi(x)||_2 + \lambda_1R_1(\phi) + \lambda_2R_2(\psi)$m where $\odot$ refers to aggregation operator.
<h3>2.2 Information compression module</h3>
Adapt VQ-Layer as an information compression module to learn $\phi(\cdot)$ given embedding $f(x) \in R^{D \times H' \times W'}$ as a query $z^e = f(x)$ and memory $z \in R^{D \times N}$. Then, quantize $z^e$ into a single-memory feature cube $z^q \in R^{D \times H' \times W'}$ by seeking for memory item with minimum $L2$ distance: $z^q_{h, w} = argmin_{z_n}||z^e_{h, w} -  z_n||_2$, where $z_n$ is $n^{th}$ memory item, $h \in \{1...H'\}, w \in \{1...W'\}$ indicates same location in both $z^q$ and $z^e$. The compression loss $L_{com}$ with stop-gradient operator $SG(\cdot)$ is: $L_{com} = ||SG(z^e) - z^q||_2 + \beta||z^e - SG(z^q)||_2$.
<h3>2.3 Pyramid deformation module</h3>
Categorize the unknown anomalies into the following three types: unseen class (e.g. novel objects), global anomaly (e.g. unexpected movement) and local anomaly (e.g. strange behavior and workpiece damage) of seen class. After feature extraction $\psi(\cdot)$ uses $K$-heads to compute offsets $O = \{O_1...O_K\}$ corresponding to $K$ coarse-to-fine deformations: $\psi(x) = Up(h(PE(x))) = O$, where $PE(\cdot)$ is positional embedding operator, $h: R^{C \times H \times W} \rightarrow R^{2 \times \{H_1 \times W_1...H_K \times W_K\}}$ is the deformation estimator that generates offset vectors, $Up(\cdot)$ is upsampling function that resizes outputs of $K$-heads to same size with original image. Also introduce position embedding operator for the decoder $g(\cdot)$. Then, aggregate $O$ onto reconstructed reference $g(PE(z^q))$ and obtain $\overline{x}_k(k=1...K)$ which is calibrated by $k^{th}$ layer of deformation fields: $\overline{x}_k = g(PE(z^q)) \odot O_1... \odot O_k$, where $\odot$ is grid-sampling function. Add constraint using smoothness loss via gradient operation and strength loss as: $L_{df} = \sum_k||\bigtriangledown O_k||_1 + ||O_k||_2$.
<h3>2.4 Foreground-background selection</h3>
Model background with a learnable template $x_{bg}$ and generate a binary mask to indicate whether a pixel belongs to foreground or background with $f_m(\cdot)$. The final reconstruction $\hat{x}_k$ of $k^{th}$ head is: $\hat{x}_k = f_m(x)\overline{x}_k + (1 - f_m(x))x_{bg}$.
<h3>2.5 Training and inference</h3>
- Training phase: $L_{rec} = \sum_kDis(x, \hat{x}_k)$, where $Dis(\cdot)$ refers to a distance function in sample space. The overall loss: $L_{all} = L_{rec} + \gamma L_{com} + \gamma_2L_{df}$. 
- Inference phase: $A_{rec} = Dis(x, \hat{x}_K), A_{df} = \sum_k||O_k||_2$.
Image-level anomaly score is computed: $Score_I = max(A_{rec} \otimes k*) + \alpha max(A_{df} \otimes k*)$, where $\otimes$ is convolution operator and $k*$ is convolution kernel for anomaly maps.
<h3>2.6 Variant of PDM</h3>
$L = ||x \odot \psi'(x) - g(\phi(f(x \odot \psi'(x)), z))||_2 + \gamma_1R_1(\phi) + \gamma_2R_2(\psi')$. Propose to add inversion of forward deformation, backward deformation $O^T$ based on cycle-consistency principle to maintain diversity of appearance information: $\psi'(x) = Up(h'(PE(x))) = \{O, O^T\}$.
- Training phase: $L_{cyc} = ||x - x \odot O_1... \odot O^T_1||_2, L^+_{df} = \sum_k||\bigtriangledown O_k||_1 + ||\bigtriangledown O^T_k||_1 + ||O_k||_2 + ||O^T_k||_2$. The overall loss: $L^+_{all} = L_{rec} + \gamma_1 L_{com} + \gamma_2L^+_{df} + \gamma_3L_{cyc}$. 
- Inference phase: $A^+_{rec} = Dis(x, \hat{x}_K) \odot O^T_K ... \odot O^T_1, A^+_{df} = \sum_k||O_k ... \odot O_K||_2 + \sum_k||O^T_k ... \odot O^T_1||_2$.
The image-level anomaly score and pixel-level anomaly score are: $Score^+_I = max(A^+_{rec} \otimes k*) + \alpha max(A^+_{df} \otimes k*), Score^+_p = A^+_{rec} + \alpha A^+_{df}$.
<h2>3. Datasets</h2>
Ped2, Avenue, ShanghaiTech, MVTec and MNIST.
<h2>4. Metrics</h2>
AUC.
<h2>5. Code</h2>
https://github.com/FlappyPeggy/DMAD.