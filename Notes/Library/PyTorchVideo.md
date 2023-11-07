<h2>1. Abstract</h2>
Introduce PyTorchVideo, an open-source deep learning library that provides a rich set of modular, efficient and reproducible components for a variety of video understanding tasks, including classification, detection, self-supervised learning and low-level processing. The library covers tools including multimodal data loading, transformations and SOTA models. It also supports real-time models on mobile devices.
<h2>2. Introduction</h2>
Present PyTorchVideo which supports: a modular, a full stack, real-time, multiple tasks, reproducible and multiple input modalities.
<h2>3. Library design</h2>
The library follows 4 design principles: modularity, compatibility, customizability and reproducibility.
<h2>4. Library components</h2>
<h3>4.1 Data</h3>
UCF-101, HMDB-51, Kinetics, Charades, Something-Something, egocentric tasks for Epic Kitchen and DomSev as well as video detection in AVA.
<h3>4.2 Transforms</h3>
MixUp, CutMix, RandAugment, AugMix, etc.
<h3>4.3 Models</h3>
C2D, I3D, Slow-only for RGB frames and acoustic ResNet for audio signal, SlowFast, CSN, R2 + 1D, X3D and Audiovisual SlowFast networks.
<h3>4.4 Accelerator</h3>
Model -> Efficient Block Design -> Kernel Optimization -> Graph Optimization -> Quantization -> TorchScript -> deploy. Two benefits: 1) latency of these kernels for floating-point inference is reduced and 2) quantized operation (int8) of these kernels is enabled which is not supported for mobile devices by vanilla PyTorch.
<h2>5. Code</h2>
https://github.com/facebookresearch/pytorchvideo/tree/main.