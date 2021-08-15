---
layout: post
title: "13: VQ-VAE-2 Explained"
meta_title: "VQ-VAE-2 Explained"
description: "Generating Diverse High-Fidelity Images with VQ-VAE-2 by Razavi et al. explained in 5 minutes."
date: 2021-4-23 19:00:00 -0000
categories: autoregressive-VAE-discrete-latents-image-generation
---

#### Generating Diverse High-Fidelity Images with VQ-VAE-2 by Razavi et al. explained in 5 minutes.

##### ⭐️Paper difficulty: 🌕🌕🌕🌕🌑

***

![Generating Diverse High-Fidelity Images with VQ-VAE-2 Samples](/assets/images/vqvae2_teaser.png "VQ-VAE-2 Samples")

##### 🎯 At a glance:

The authors propose a novel hierarchical encoder-decoder model with discrete latent vectors that uses an autoregressive prior (PixelCNN) to sample diverse high quality samples.

##### ⌛️ Prerequisites:

*(Highly recommended reading to understand the core ideas in this paper):*  
1) [Transformers](https://arxiv.org/abs/1706.03762)  
2) [VQ-VAE](https://arxiv.org/abs/1711.00937)

***

##### 🔍 Main Ideas:

**1) Vector Quantized Variational AutoEncoder:**  
The model is comprise of three parts: an encoder that maps an image to a sequence of latent variables, a shared codebook that is used to quantize these continuous latent vectors to a set of discrete latent variables (each vector is replaced with the nearest vector from the codebook), and the decoder that maps the indices of the vectors from the codebook back to an image.

**2) Learning the codebook:**  
Since the quantization operation is non-differentiable a straight through gradient estimator is used. As scary as that sounds it simply means that the gradient from the first layer of the decoder is directly passed to the last layer of the encoder skipping the codebook altogether.
The codebook itself is updated via exponential moving average of the encoder outputs. The encoder outputs are regularized so that they stay close to the chosen codebook without fluctuating too much.

**3) Hierarchical VQ-VAE:**  
Inspired by the ideas of coarse-to-fine generation the authors propose a hierarchy of vector quantized codes to model large images. The whole architecture resembles a 3 level UNet with concatenating skip connections, except that each feature map is quantized before being passed to the decoder.

**4) Learning priors over the latent codes.**  
In order to sample from the model a separate autoregressive prior (PixelCNN) is learned over the sequences of latent codes at each resolution level. The authors use self attention layers in the top level prior since it has lower resolution, and large conditional stacks coming from the top prior for the bottom prior with higher resolution due to memory constraints. Each prior is trained separately. Sampling from the model requires passing a class label to the trained top level PixelCNN to obtain the top level codes, then passing the class label along with the generated codes to the bottom level to generate the higher resolution code, and then use the decoder to generate an image from the top and bottom level codes.

##### 📈 Experiment insights / Key takeaways:

_ImageNet (Top-1 Classification Accuracy Score - test score of a classifier trained only on samples from the generator)_  
The best is Real Data @ 91.47 (next best is authors' VQ-VAE @ 80.98)

***

##### 🖼️ Paper Poster:

![Generating Diverse High-Fidelity Images with VQ-VAE-2 Paper Poster](/assets/images/vqvae2.png "VQ-VAE-2 Paper Poster")

***

##### ✏️My Notes:

- 3/5 for the name, basic but cool
- Seems to work well for datasets with a ton of classes
- Wonder why this has not dethroned StyleGAN2 as the go-to method for image generation if it is really as good as the authors claim 🤔

##### 🔗 Links:
[VQ-VAE-2 arxiv](https://arxiv.org/abs/1906.00446) / [VQ-VAE-2 Github](https://github.com/rosinality/vq-vae-2-pytorch)

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
