<h2>1. Abstract</h2>
Transform the fundamental role of a discriminator from identifying real and fake data to distinguishing between good and bad quality reconstructions. Prepare training examples for the good quality reconstruction by employing the current generator, whereas poor quality examples are obtained by utilizing an old state of the same generator.
<h2>2. Method</h2>
<h3>2.1 Architecture overview</h3>
The generator $\mathcal{G}$, a typical denoising auto-encoder is coupled with the discriminator $\mathcal{D}$ to learn one class data in an unsupervised adversarial fashion. The model built on top of the baseline, makes use of an old frozen generator ($\mathcal{G}^{old}$) to create low quality reconstruction examples. Also propose a pseudo-anomaly module to assist $\mathcal{D}$ in learning the behavior of $\mathcal{G}$ in the case of unusual or anomalous input.
<h3>2.2 Training</h3>
The training is carried out in 2 phases. Phase one is similar to train an adversarial one-class classifier. The loss function is: $\mathcal{L} = \mathcal{L}_{\mathcal{G} + \mathcal{D}} + \lambda\mathcal{L}_R$, where $\mathcal{L}_R = ||X - \mathcal{G}(\overline{X})||^2$ is the reconstruction loss. Phase two of the training is where we make use of the frozen models $\mathcal{G}^{old}$ and $\mathcal{G}$ to update $\mathcal{D}$.
- Pseudo anomaly detection: given 2 arbitrary images $X_i, X_j$ from the training dataset, a pseudo anomaly image $\hat{X}$ is generated as: $\hat{X} = \frac{\mathcal{G}^{old}(X_i) + \mathcal{G}^{old}(X_j)}{2} = \frac{\hat{X}^{low}_i + \hat{X}^{low}_j}{2}$, where $i \neq j$. In order to mimic the behavior of $\mathcal{G}$ when it gets unusual data as input, $\hat{X}$ is then reconstructed using $\mathcal{G}$ to obtain $\hat{X}^{pseudo}$: $\hat{X}^{pseudo} = \mathcal{G}(\hat{X})$.
- Tweaking the objective function: $\mathrm{max}_\mathcal{D}(\alpha\mathbb{E}_X[\log(1 - \mathcal{D}(X))] + (1 - \alpha)\mathbb{E}_{\hat{X}}[\log(1 - \mathcal{D}(\hat{X}))] + \beta\mathbb{E}_{\hat{X}^{low}}[\log(\mathcal{D}(\hat{X}^{low}))] + (1 - \beta)\mathbb{E}_{\hat{X}^{pseudo}}[\log(\mathcal{D}(\hat{X}^{pseudo}))])$, where $\alpha, \beta$ are trade-off hyperparameters.
Quasi ground truth for the discriminator in phase one training is defined as: $GT_{phase\_one} = 0$ if input is $X$ else 1 if input is $\hat{X}$. For phase two training, it takes the form: $GT_{phase\_two} = 0$ if input is $X$ or $\hat{X}$ else 1 if input is $\hat{X}^{low}$ or $\hat{X}^{pseudo}$.
<h3>2.3 Testing</h3>
At test time, only $\mathcal{G}$ and $\mathcal{D}$ are utilized for one-class classification. Final classification decision for an input image $X$ is given as: $OCC$ = normal class if $\mathcal{D}(\mathcal{G}(X))$ < $\tau$ else anomaly class, where $\tau$ is a predefined threshold.
<h2>3. Metrics</h2>
AUC and EER.
<h2>4. Datasets</h2>
Caltech-256, MNIST and UCSD Ped2.
<h2>5. Code</h2>
https://github.com/xaggi/OGNet.