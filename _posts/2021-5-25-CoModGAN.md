---
layout: post
title:  "23: CoModGAN Explained"
meta_title: "CoModGAN Explained"
description:  "Large Scale Image Completion via Co-Modulated Generative Adversarial Networks by Zhao Shengyu et al explained in 5 minutes."
date:   2021-5-25 19:00:00 -0000
categories: co-modulated-convolutions-multimodal-inpainting
---

#### Large Scale Image Completion via Co-Modulated Generative Adversarial Networks by Zhao Shengyu et al explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Large Scale Image Completion via Co-Modulated Generative Adversarial Networks teaser](/assets/images/comodgan_teaser.gif "CoModGAN teaser")

##### 🎯 At a glance:

Is it true that all existing methods fail to inpaint large-scale missing regions? The authors of CoModGAN claim that it is impossible to complete an object that is missing a large part unless the model is able to generate a completely new object of that kind, and propose a novel GAN architecture that bridges the gap between image-conditional and unconditional generators, which enables it to generate very convincing complete images from inputs with large portions masked out.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [StyleGAN](https://arxiv.org/abs/1812.04948)
2) [StyleGAN2](https://arxiv.org/abs/1912.04958)

***

##### 🔍 Main Ideas:

**1) Modulation approaches:**
On the one hand, there is unconditional modulation from StyleGAN2, where a noise vector is passed through a fully-connected mapping network to obtain a style vector. The style vector is used to generate a modulation vector that multiplies the input feature maps channel-wise. The resulting feature maps are processed by a convolution and demodulated (normalized) to have unit variance.
On the other, there are image-conditional generators that use learned flattened features from an encoder as modulation parameters. Their main shortcoming is the lack of stochastic generative capability. The outputs are simply not diverse enough, when limited input information is available since the output should only be weakly conditioned on the input.

**2) Co-modulation:**
The authors propose to solve the aforementioned issues by combining the two types of style vectors (from encoder and from sampled noise) via a joint affine transformation to produce a single modulation parameter. It is noted that it is not necessary to use a nonlinear mapping to combine the two style vectors, and it is sufficient to assume that the two style vectors can be linearly correlated to improve the generated image quality, especially when a large portion of the input is missing.

**3) Paired/Unpaired Inception Discriminative Score:**
The authors additionally propose a novel metric to measure the linear separability of fake/real samples in a pretrained feature space.
For the paired case the idea is to sample pairs of real and fake images from a joint distribution, extract features from them with a pretrained Inception v3 model, and train a linear SVM on the extracted features. The P-IDS is the probability that a fake sample is considered more realistic than the corresponding real sample.
If there is only unpaired data available the images are sampled independently, and the U-IDS is the misclassification rate of the SVM instead.
The authors point out 3 major advantages of P-IDS/U-IDS: robustness to sampling size, effectiveness of capturing subtle differences, and good correlation to human preferences.

##### 📈 Experiment insights / Key takeaways:

- It is possible to tune the tradeoff between quality and diversity by adjusting the truncation parameter that amplifies the stochastic branch
- CoModGAN outperforms other baselines on inpainting tasks, and especially large-region inpainting
- CoModGAN can be effectively used for various image-to-image tasks such as edges-to-photo and labels-to-photo, and outperforms MUNIT and SPADE on the corresponding tasks
***

##### 🖼️ Paper Poster:

![Large Scale Image Completion via Co-Modulated Generative Adversarial Networks paper poster](/assets/images/comodgan.png "CoModGAN Paper Poster")

***

##### ✏️My Notes:

- 4/5 for the name CoMod GAN sounds very funny in Russian
- The inpainted images look pretty insane
- Really simple yet effective idea
- Awesome that the model is applicable in many various settings
- Interesting whether the images can be composited of different input parts and edited in the latent space

##### 🔗 Links:
[Co-Modulated GAN arxiv](https://openreview.net/pdf?id=sSjqmfsk95O) / [Co-Modulated GAN github](https://github.com/zsyzzsoft/co-mod-gan)

***

##### 👋 Thanks for reading!

<a href="https://www.patreon.com/bePatron?u=53448948" data-patreon-widget-type="become-patron-button">Join Patreon for Exclusive Perks!</a><script async src="https://c6.patreon.com/becomePatronButton.bundle.js"></script>

*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Join the Casual GAN Papers telegram channel](https://t.me/joinchat/KeutnzlvetRkZGZi) to stay up to date with new AI Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
