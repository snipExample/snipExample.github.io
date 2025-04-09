---
layout: post
title:  "GAN Architectures on the RetinaMNIST Dataset: A Deep Dive"
date:   2025-04-09
---
# Exploring GAN Architectures on the RetinaMNIST Dataset: A Deep Dive

**Posted on April 9, 2025**

---

## Introduction

Generative Adversarial Networks (GANs) have dramatically reshaped the field of generative modeling. By pitting two neural networks, a **generator** and a **discriminator**, against each other in an adversarial contest, GANs have enabled the creation of images, music, and even text that closely resemble real-world data. In this blog post, we take a comprehensive look at three prominent GAN architectures: **Least Squares GAN (LS-GAN)**, **Wasserstein GAN (WGAN)**, and **Wasserstein GAN with Gradient Penalty (WGAN-GP)**. We evaluate their performance on the **RetinaMNIST** dataset, using key metrics such as the **Inception Score (IS)** and **Fréchet Inception Distance (FID)** to highlight their strengths and nuances.

---

## Understanding GANs and Their Uniqueness

### What Are GANs?

At its core, a GAN consists of two competing networks:
- **Generator:** Crafts synthetic images (or other data) from a random noise vector.
- **Discriminator (Critic):** Evaluates images to determine if they are real (from the dataset) or generated.

<div style="text-align: center;">
  <img src="/assets/gans/GANs_architecture.png" alt="GAN Architecture" style="max-width: 600px; height: auto; border-radius: 8px;">
</div>

This setup forms a zero-sum game where each network's improvement forces the other to adapt, leading to increasingly realistic outputs over time.

---

### Why Are GANs Different?

The creativity behind GANs lies in their adversarial training strategy:

- **Dynamic Learning:** Unlike traditional models that simply minimize a loss function, GANs create a dynamic environment where both networks evolve simultaneously.

- **Loss Function Variations:** The choice of loss function greatly influences training dynamics. Different GAN variants use different losses and regularization techniques to improve stability, convergence, and output quality.

- **Diverse Applications:** From generating high-quality images to synthesizing novel art, GANs have diverse applications that benefit from their inherent ability to learn complex data distributions.

---

## The RetinaMNIST Dataset

The **RetinaMNIST** dataset is a subset of the MedMNIST collection and is specifically designed for medical imaging tasks.

- **Image Size:** 28×28 pixels  
- **Preprocessing:** Pixel values are normalized to the range **[-1, 1]**  
- **Image Format:** Grayscale images with a single channel (dimensions 28×28×1)


<div style="text-align: center;">
  <img src="/assets/gans/RetinaMNIST.png" alt="RetinaMNIST Dataset" style="max-width: 600px; height: auto; border-radius: 8px;">
</div>

This dataset provides a challenging yet manageable testbed for comparing GAN architectures, particularly in the context of medical image synthesis.

---

## A Closer Look at GAN Architectures

### 1. Least Squares GAN (LS-GAN)

**LS-GAN** introduces a modification of the traditional GAN loss by using a least squares loss function. This approach offers several advantages:

- **Smoother Gradients:** By reducing the likelihood of vanishing gradients, LS-GAN facilitates a more stable training process.
- **Enhanced Image Quality:** The smoother gradient flow often results in crisper and more detailed images.
- **Architecture:** Typically involves multiple convolutional layers with batch normalization in both the generator and discriminator.

---

### 2. Wasserstein GAN (WGAN)

**WGAN** replaces the conventional binary loss with a loss based on the **Wasserstein distance**, providing a mathematically meaningful measure of the difference between two probability distributions.

- **Wasserstein Loss:** Helps quantify how far the generated data distribution is from the true data distribution.
- **Training Stability:** More stable and less prone to mode collapse.
- **Critic Architecture:** Designed without batch normalization to preserve Lipschitz continuity.

---

### 3. Wasserstein GAN with Gradient Penalty (WGAN-GP)

**WGAN-GP** builds on the WGAN framework by adding a **gradient penalty** term:

- **Enforces Lipschitz Constraint:** Achieves this without harsh weight clipping.
- **Improved Stability:** Results in smoother training dynamics, at the cost of computational complexity.
- **Architecture Similarity:** Similar to WGAN, but with the added gradient penalty in training.

---

## Training Details

A uniform training setup was applied to all models:

- **Epochs:** 50  
- **Batch Size:** 64  
- **Noise Vector Dimension:** 100  
- **Optimizer:** Adam (Learning Rate = 0.0002, β₁ = 0.5)

> For WGAN and WGAN-GP, the critic was updated **five times** for every generator update.

---

## Evaluation Metrics

Evaluating GANs is challenging. Two key metrics were used:

### Inception Score (IS)
- **Purpose:** Measures clarity and variety of generated images.
- **Mechanism:** A pre-trained classifier is used to assess entropy of predictions.
- **Interpretation:** Higher IS = better quality + diversity.

### Fréchet Inception Distance (FID)
- **Purpose:** Compares real vs. generated distributions.
- **Mechanism:** Uses mean and covariance of features from a pre-trained model.
- **Interpretation:** Lower FID = more realistic outputs.

---

## Results and Discussion

Experimental results:

- **LS-GAN:** IS ≈ 1.00, FID ≈ 263.34  
- **WGAN:** IS ≈ 1.00, FID ≈ 263.34  
- **WGAN-GP:** IS ≈ 1.00, FID ≈ 333.25

---

### Loss Curve Visualizations

<div style="text-align: center;">
  <img src="/assets/gans/c-loss.png" alt="C Loss" style="max-width: 600px; height: auto; border-radius: 8px;">
</div>


<div style="text-align: center;">
  <img src="/assets/gans/d-loss.png" alt="Discriminator Loss" style="max-width: 600px; height: auto; border-radius: 8px;">
</div>

<div style="text-align: center;">
  <img src="/assets/gans/g-loss.png" alt="Generator Loss" style="max-width: 600px; height: auto; border-radius: 8px;">
</div>
---

### Insights

- **Inception Score Parity:** All models had similar IS scores, indicating comparable image clarity and diversity.
- **FID Differentiation:** LS-GAN outperformed others, suggesting more realistic generation.
- **Training Dynamics:** LS-GAN’s smoother gradients offered better stability.

---

## Conclusion

**LS-GAN** emerged as a strong performer on RetinaMNIST. While WGAN and WGAN-GP offer theoretical advantages, LS-GAN’s practical edge was evident here.

This study emphasizes the importance of matching architectures and training dynamics to the dataset and application.

---

## Looking Ahead: Future Directions

- **Hybrid Models:** Combine strengths of LS-GAN, WGAN, and others.  
- **Deeper Networks:** Use modern deep learning techniques.  
- **Advanced Regularization:** Explore new techniques for training stability.  
- **Better Metrics:** Especially tailored for medical images.

---

## References

1. Heusel et al. (2017), _GANs Trained by a Two Time-Scale Update Rule_ [[PDF]](https://arxiv.org/pdf/1706.08500)  
2. Gulrajani et al. (2017), _Improved Training of Wasserstein GANs_ [[PDF]](https://arxiv.org/pdf/1704.00028)  
3. Mao et al. (2017), _Least Squares GANs_ [[PDF]](https://arxiv.org/pdf/1611.04076)  
4. MedMNIST v2 Benchmark [[Link]](https://medmnist.com/)  
5. Review of GANs for EHR [[Link]](https://www.researchgate.net/publication/359227500_A_review_of_Generative_Adversarial_Networks_for_Electronic_Health_Records_applications_evaluation_measures_and_data_sources/figures?lo=1)  
6. MedMNIST Data [[Link]](https://medmnist.com/)

---

### **Posted by** — [Rohan Ingle](https://github.com/Rohan-ingle)
