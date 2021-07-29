---
layout: post
title:  "41: Real-ESRGAN Explained"
meta_title: "Real-ESRGAN Explained"
description:  "Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data by Xintao Wang et al. explained in 5 minutes."
date: 2021-7-30 19:00:00 -0000
categories: synthetic-data-real-world-blind-super-resolution
---

#### Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data by Xintao Wang et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌑🌑🌑

***

![Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data Samples](/assets/images/realesrgan_teaser.jpg "Real-ESRGAN teaser")

##### 🎯 At a glance:

While there are many blind image restoration approaches, few can handle complex real-world degradations. Yet Real-ESRGAN by Xintao Wang and his colleagues from ARC, Tencent PCG, Shenzen Institutes, and University of Chinese Academy of Sciences takes real-world image super-resolution (SR) to the next level! The authors propose a new higher-order image degradation model to better simulate real-world data. This idea together with an improved U-Net discriminator allows Real-ESRGAN to demonstrate superior visual performance than prior works on various real datasets.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [ESRGAN](https://arxiv.org/abs/1809.00219)

***

##### 🚀 Motivation:
Classical degradation model, which consists of blur, downsampling, noise and JPEG compression is not complex enough to model real-world degradations. Models trained on these synthetic samples will easily fail on real-world tests. The goal of this work is to extend blind SR trained on synthetic data to work on real-world images at inference time. Hence, a more sophisticated degradation model called second-order degradation process is introduced.
To compensate for the larger degradation space the VGG-style discriminator is upgraded to a U-Net design. Additionally, spectral normalization (SN) regularization is applied to stabilize training,

***

##### 🔍 Main Ideas:

**1) Classical Degradation Model:** 
Typically the distortion model consists of a gaussian blur followed by a downsampling operation, additive Poisson noise to simulate camera sensor artifacts. Additionally, differentiable JPEG compression is used for lossy compression and introduces unpleasing block artifacts.

**2) Higher-order Degradation Model:**  
A second-order degradation process is ... the classical degradation process applied to an image twice in succession.

**3) Ringing and Overshoot Artifacts:**  
Ringing artifacts often appear as spurious edges near sharp transitions in an image. They visually look like bands or “ghosts” near edges. Overshoot artifacts are usually combined with ringing artifacts, which manifest themselves as an increased jump at the edge transition. The main cause of these artifacts is that the signal is bandlimited without high frequencies. A sinc filter is employed to cut off high frequencies to synthesize ringing and overshoot artifacts for training pairs. It is used in the blurring process as well as the last step of the synthesis, where its order is randomly swapped with JPEG compression.

**4) Networks and Training:**  
The generator is the same as ESRGAN with the addition of pixel-unshuffle to reduce the spatial size and increase channel size before the main ESRGAN network. A U-Net discriminator is used in combination with SN regularization for more discriminative power with more stable training dynamics.

##### 📈 Experiment insights / Key takeaways:

- Real-ESRGAN is trained on 256x256 patches from DIV2K, Flickr2K, and OutdoorSceneTraining datasets
- It is fine-tuned from ESRGAN with a batch size of 48 for 500k iterations with L1, perceptual, and GAN losses
- Considered baseline state-of-the-art methods: ESRGAN, DAN, CDC, RealSR, and BSRGAN
- The baselines are nowhere near Real-ESRGAN in qualitative comparison
- Real-ESRGAN trained with classical first-order degradation model cannot effectively remove noise or blur
- If sinc filters are not employed during training, the restored results will amplify the ringing and overshoot artifacts that existed in the input images, especially around the text and lines
- Most failure cases are supposedly connected with aliasing issues

***

##### 🖼️ Paper Poster:

![Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data paper poster](/assets/images/realesrgan.png "Real-ESRGAN Paper Poster")

***

##### ✏️My Notes:

- Real-ESRGAN is as generic of a name as they get: 2/5
- I am going to bet that there will be a paper that uses ideas from both ESRGAN and [Alias-free GAN]({% post_url 2021-6-25-AliasFreeGAN %}) soon
- What do you think about Real-ESRGAN? Write your response in the comments!

##### 🔗 Links:
[Real-ESRGAN arxiv](https://arxiv.org/pdf/2107.10833v1.pdf) / [Real-ESRGAN github](https://github.com/xinntao/Real-ESRGAN)

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
