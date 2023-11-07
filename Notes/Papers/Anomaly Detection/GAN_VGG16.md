<h2>1. Abstract</h2>
This paper is based on deep learning methods and provides an effective way to detect and locate abnormal events in videos by handling spatio-temporal data. This paper uses generative adversarial networks (GANs) and performs transfer learning algorithms on pre-trained convolutional neural network (CNN) which result in an accurate and efficient model.
<h2>2. Introduction</h2>
Three main parts: preprocessing, training the GAN, and analyzing the optical flow images. The testing phase includes a pre-processing step which is then followed by a spatio-temporal representation. The representation is then calculated and fed to the trained discriminator network as an input. In the final step, abnormal patches of all the frames are determined and an output image is created to show the appearance-motion abnormalities of each frame.
<h2>3. Appearance-motion and motion-only analysis</h2>
<h3>3.1 Pre-processing of input images</h3>
The spatiotemporal representation of frame number t is $D_t = <I_t, I_{t - 2}, I_{t - 4}>$, where $I_t$ is the edge image of frame number $t$.
<h3>3.2 Patch extraction from appearance-motion representation</h3>
Frame patches are determined by adding a grid overlay to the appearance-motion representation of the frames. Among all patches, those that have at lease a minimum amount of foreground pixels are chosen to avoid noises and useless data. These patches are gathered from video frames and inputted to the network for learning and testing processes.
<h3>3.3 Objects movement analysis</h3>
Sorted histograms of direction and speed intensities are calculated in all video frames. Motion abnormalities are then detected based on the sorted histograms. The first 5% of hue/intensity values with the lowest frequencies in the sorted histogram are considered as abnormal. The result of this analysis, together with that of the appearance-motion analysis can effectively detect and locate abnormal patches in the video.
<h2>4. Transfer learning to benefit from vgg16</h2>
In each step, the generator outputs patches and gives them to a discriminator that uses a pre-trained VGG16 network. The discriminator then guesses the originality of its input patches. The final error of the discriminator is calculated based on the accuracy of its detections and returned to both the generator and the discriminator. The first six layers of a pre-trained VGG16 network are used to obviate the need for training more than 10 million learnable parameters and gathering a large amount of various data.
<h2>5. Datasets</h2>
UCSD.
<h2>6. Metrics</h2>
AUC and EER.