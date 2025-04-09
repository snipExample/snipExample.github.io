# Exploring GAN Architectures on the RetinaMNIST Dataset: A Deep Dive

**Posted on April 9, 2025**

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Introduction

Generative Adversarial Networks (GANs) have dramatically reshaped the field of generative modeling. By pitting two neural networks, a **generator** and a **discriminator**, against each other in an adversarial contest, GANs have enabled the creation of images, music, and even text that closely resemble real-world data. In this blog post, we take a comprehensive look at three prominent GAN architectures: **Least Squares GAN (LS-GAN)**, **Wasserstein GAN (WGAN)**, and **Wasserstein GAN with Gradient Penalty (WGAN-GP)**. We evaluate their performance on the **RetinaMNIST** dataset, using key metrics such as the **Inception Score (IS)** and **Fréchet Inception Distance (FID)** to highlight their strengths and nuances.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Understanding GANs and Their Uniqueness

### What Are GANs?

At its core, a GAN consists of two competing networks:
- **Generator:** Crafts synthetic images (or other data) from a random noise vector.
- **Discriminator (Critic):** Evaluates images to determine if they are real (from the dataset) or generated.

<div style="text-align: center;">
    <img src="assets/gans/GANs_architecture.png" alt="GANs_architecture" width="750" />
</div>

This setup forms a zero-sum game where each network's improvement forces the other to adapt, leading to increasingly realistic outputs over time.

### Why Are GANs Different?

The creativity behind GANs lies in their adversarial training strategy:
- **Dynamic Learning:** Unlike traditional models that simply minimize a loss function, GANs create a dynamic environment where both networks evolve simultaneously.
- **Loss Function Variations:** The choice of loss function greatly influences training dynamics. Different GAN variants use different losses and regularization techniques to improve stability, convergence, and output quality.
- **Diverse Applications:** From generating high-quality images to synthesizing novel art, GANs have diverse applications that benefit from their inherent ability to learn complex data distributions.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## The RetinaMNIST Dataset

The **RetinaMNIST** dataset is a subset of the MedMNIST collection and is specifically designed for medical imaging tasks. Key characteristics include:
- **Image Size:** 28×28 pixels.
- **Preprocessing:** Pixel values are normalized to the range **[-1, 1]**.
- **Image Format:** Grayscale images with a single channel (dimensions 28×28×1).

<div style="text-align: center;">
    <img src="assets/gans/RetinaMNIST.png" alt="RetinaMNIST_Dataset" width="750" />
</div>

This dataset provides a challenging yet manageable testbed for comparing GAN architectures, particularly in the context of medical image synthesis.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## A Closer Look at GAN Architectures

### 1. Least Squares GAN (LS-GAN)

**LS-GAN** introduces a modification of the traditional GAN loss by using a least squares loss function. This approach offers several advantages:
- **Smoother Gradients:** By reducing the likelihood of vanishing gradients, LS-GAN facilitates a more stable training process.
- **Enhanced Image Quality:** The smoother gradient flow often results in crisper and more detailed images.
- **Architecture:** Typically involves multiple convolutional layers with batch normalization in both the generator and discriminator, allowing for better feature extraction and synthesis.

### 2. Wasserstein GAN (WGAN)

**WGAN** replaces the conventional binary loss with a loss based on the **Wasserstein distance**, providing a mathematically meaningful measure of the difference between two probability distributions. Key points include:
- **Wasserstein Loss:** This loss function helps in quantifying how far the generated data distribution is from the true data distribution.
- **Training Stability:** Empirical evidence shows that WGAN can be more stable and less prone to issues such as mode collapse.
- **Critic Architecture:** Unlike typical discriminators, the WGAN critic is designed without batch normalization, as preserving the Lipschitz continuity is vital for the Wasserstein formulation.

### 3. Wasserstein GAN with Gradient Penalty (WGAN-GP)

**WGAN-GP** builds on the WGAN framework by adding a **gradient penalty** term. This enhancement:
- **Enforces Lipschitz Constraint:** The gradient penalty helps maintain the required Lipschitz continuity without the need for harsh weight clipping.
- **Improved Stability:** Leads to smoother training dynamics, although it increases the computational complexity during training.
- **Architecture Similarity:** Shares architectural similarities with WGAN, with the primary difference being the incorporation of a gradient penalty in the training loop.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Training Details

For our comparative study, the following training setup was uniformly applied to all models:
- **Epochs:** 50
- **Batch Size:** 64
- **Noise Vector Dimension:** 100
- **Optimizer:** Adam (Learning Rate = 0.0002, β₁ = 0.5)

Additionally, for WGAN and WGAN-GP, the critic was updated **five times** for every generator update. This strategy ensures that the critic remains robust, providing a reliable gradient that guides the generator’s improvements.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Evaluation Metrics

Evaluating the quality of GAN-generated images is challenging. Two metrics commonly used are:

### Inception Score (IS)
- **Purpose:** Quantifies the clarity and variety of the generated images.
- **Mechanism:** A pre-trained classifier predicts class labels for the images, and the score is derived from the confidence and entropy of these predictions.
- **Interpretation:** A higher IS suggests images that are both high quality and diverse, although its reliability can diminish when the classifier isn’t perfectly aligned with the dataset.

### Fréchet Inception Distance (FID)
- **Purpose:** Measures the similarity between the distributions of real and generated images.
- **Mechanism:** Compares the statistics (mean and covariance) of features extracted from a pre-trained network for both real and synthetic images.
- **Interpretation:** Lower FID values indicate that the generated images closely resemble the real ones in both quality and diversity, making this metric a strong indicator of practical performance.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Results and Discussion

The experimental results were as follows:
- **LS-GAN:** IS ≈ 1.00, FID ≈ 263.34
- **WGAN:** IS ≈ 1.00, FID ≈ 263.34
- **WGAN-GP:** IS ≈ 1.00, FID ≈ 333.25

<div style="text-align: center;">
    <img src="assets/gans/c-loss.png" alt="c-loss" width="750" style="margin-bottom: 15px;"/>
    <img src="assets/gans/d-loss.png" alt="d-loss" width="750" style="margin-bottom: 15px;"/>
    <img src="assets/gans/g-loss.png" alt="g-loss" width="750"/>
</div>

### Insights:
- **Inception Score Parity:** All models achieved similar IS values, hinting that the overall image clarity and diversity were comparable.
- **FID Differentiation:** The lower FID of LS-GAN suggests that its images more faithfully capture the distribution of the real retinal images compared to WGAN-GP.
- **Training Dynamics:** The stability observed in LS-GAN can be attributed to its least squares loss, which provides smoother training gradients, whereas the theoretical benefits of WGAN and WGAN-GP are more evident in other contexts or with more complex datasets.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Conclusion

The detailed comparative analysis highlights that **LS-GAN** provides a robust framework for generating retinal images from the RetinaMNIST dataset. Despite the conceptual advantages of **WGAN** and **WGAN-GP**, especially in enforcing a meaningful distributional divergence, the practical performance for this particular application favors LS-GAN, as evidenced by its lower FID. The study underscores the importance of both theoretical innovations and practical training strategies in the development of generative models.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## Looking Ahead: Future Directions

The realm of GANs is rapidly evolving. Future research may explore:
- **Hybrid Architectures:** Combining elements of LS-GAN, WGAN, and other GAN variants to exploit the strengths of each approach.
- **Deeper Network Architectures:** Leveraging advances in deep learning to build more sophisticated models.
- **Advanced Training Techniques:** Investigating alternative regularization and normalization strategies to further enhance stability and performance.
- **Broader Evaluation Metrics:** Incorporating additional metrics tailored to the nuances of medical image synthesis could provide a more holistic view of model performance.

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

## References

1. **Heusel, M., Ramsauer, H., Unterthiner, T., Nessler, B., & Hochreiter, S. (2017).** _GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium_. [Link](https://arxiv.org/pdf/1706.08500)  
2. **Gulrajani, I., Ahmed, F., Arjovsky, M., Dumoulin, V., & Courville, A. (2017).** _Improved Training of Wasserstein GANs_. [Link](https://arxiv.org/pdf/1704.00028)  
3. **Mao, X., Li, Q., Xie, H., Lau, R. Y., Wang, Z., & Paul Smolley, S. (2017).** _Least Squares Generative Adversarial Networks_. [Link](https://arxiv.org/pdf/1611.04076)  
4. **MedMNIST v2:** A Large-Scale Lightweight Benchmark for 2D and 3D Biomedical Image Classification. [Link](https://medmnist.com/)  
5. **A Review of Generative Adversarial Networks for Electronic Health Records: Applications, Evaluation Measures and Data Sources.** [Link](https://www.researchgate.net/publication/359227500_A_review_of_Generative_Adversarial_Networks_for_Electronic_Health_Records_applications_evaluation_measures_and_data_sources/figures?lo=1)  
6. **MedMNIST Data** [link](https://medmnist.com/)

<hr style="margin-top:15px; margin-bottom:15px; border: 1px solid #000;" />

### **Posted by** - [Rohan Ingle](https://github.com/Rohan-ingle)
