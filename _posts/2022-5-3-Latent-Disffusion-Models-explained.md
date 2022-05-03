---
layout: post
title: "86: LDMs"
meta_title: "Latent Diffusion Models"
description: "High-Resolution Image Synthesis with Latent Diffusion Models by Robin Rombach, Andreas Blattmann et al."
date: 2022-5-3 16:00:00 -0000
categories: high-res-faster-diffusion-democratizing-diffusion
---

#### High-Resolution Image Synthesis with Latent Diffusion Models by Robin Rombach, Andreas Blattmann et al.

##### ⭐️Paper difficulty: 🌕🌕🌕🌑🌑

***

![LDMs Model](/assets/images/ldms_preview.gif "LDMs Teaser")

##### 🎯 At a glance:

One of the cleanest pitches for a paper I have seen: diffusion models are way too expensive to train in terms of memory, time and compute, therefore let’s make them lighter, faster, and cheaper.

As for the details, let’s dive in, shall we?

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#2b2f3c', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### ⌛️ Prerequisites:

**(Highly recommended reading to understand the core contributions of this paper):**  
1. [Diffusion Models - ADM]({% post_url 2021-12-26-Guided-Diffusion-explained %})  
2. [VQGAN]({% post_url 2021-6-1-VQGAN %})

##### 🚀 Motivation:

Diffusion models (DMs) have a more stable training phase than GANs and less parameters than autoregressive models, yet they are just really resource intensive. The most powerful DMs require up to a 1000 V100 days to train (that’s a lot of $$$ for compute) and about a day per 1000 inference samples. The authors of Latent Diffusion Models (LDMs) pinpoint this problem to the high dimensionality of the pixel space, in which the diffusion process occurs and propose to perform it in a more compact latent space instead. In short, they achieve this feat by pertaining an autoencoder model that learns an efficient compact latent space that is perceptually equivalent to the pixel space. A DM sandwiched between the convolutional encoder-decoder is then trained inside the latent space in a more computationally-efficient way.  

In other words, this is a VQGAN with a DM instead of a transformer (and without a discriminator).

***

##### 🔍 Main Ideas:

**1. Perceptual Image Compression:**  
Authors train an autoencoder that outputs a tensor of latent codes. This latent embedding is regularized with vector quantization within the decoder. This is a slight but important change from VGQAN that means the underlying diffusion model works with continuous latent codes and the quantization happens afterwards.  

**2. Latent Diffusion Models:**  
As the second part of the two-stage training approach a diffusion model is trained inside the learned latent space of the autoencoder. I won’t go into details about how the diffusion itself works as I have covered it before in a previous post. What you need to know here is that the denoising model is a UNet that predicts the noise that was added to the latent codes in the previous step of the diffusion process.  

**3. Conditioning Mechanisms:**  
Authors utilize domain-specific encoders and cross-attention layers to control the generative model with additional information. The conditions of various modalities such as text, are passed through their own encoders. The results get incorporated in the generative process via cross attention with flattened features from the intermediate layers of the UNet.  

##### 📈 Experiment insights / Key takeaways:

- *Baselines:* LSGM, ADM, StyleGAN, ProjectedGAN, DC-VAE  
- *Datasets:* ImageNet, CelebA-HQ, LSUN-Churches, LSUN-Bedrooms, MS-COCO  
- *Metrics:* FID, Perception-Recall  
- *Qualitative:* x4-x8 compression is the sweet point for ImageNet  
- *Quantitative:* LDMS > LSGM, new SOTA FID on CelebA-HQ - 5.11, all scores (with models with 1/2 model size and 1/4 compute) are better (vs other diffusion models) except for LSUN-Bedrooms, where ADM is better  
- *Additional:* the model can get up to 1024x1024, can be used for inpainting, super-resolution, and semantic synthesis. There are a lot of details about the experiments, but that is the 5-minute gist.  

***

##### 🖼️ Paper Poster:

![LDMs poster](/assets/images/ldms.jpg "LDMs Poster")

***

##### 🛠 Possible Improvements:

- LDMs still much slower than GANs  
- Pixel-perfect accuracy is a bottleneck for LDMs in certain tasks (Which ones?).  

##### ✏️My Notes:

- *(Naming: 3.5/5)* The name “LDM” is as straightforward as the problem that the paper that is discussed in the paper. It is an easy-to-pronounce acronym, but not a word and definitely not a meme.  
- *(Reader Experience - RX: 3/5)* Right away, kudos for explicitly listing all of the core contributions of the paper right where they belong - at the end of the introduction. I am going to duck a point for visually-inconsistent figures. They are all over the place. Moreover, the small font size in the tables is very hard to read, especially with how packed the tables appear. Finally, why are the images so tiny? Can you even make out what is on Figure 8? What is the purpose of putting in figures that you can’t read? It would probably be better to cut one or two out to make the rest more readable. Finally, the results table is very hard to read, because different baselines in different order are used for different datasets.  

- I can’t help but draw parallels between Latent Diffusion and StyleNeRF papers - sandwiching an expensive operation (Diffusion & Ray Marching) between a convolutional encoder-decoder to reduce computational costs and memory requirements by performing the operation in spatially-condensed latent space.  
- Let’s think for a second: what other ideas from DNR & styleNeRF could further improve diffusion models ? One idea I can see being useful is the “NeRF path regularization”, which means, in terms of DMs, training a low-resolution DM alongside a high-resolution LDM, and adding a loss that matches subsampled pixels of the LDM to the pixels in the DM  
- It should be possible to interpolate between codes in the learned latent space. Not sure how exactly this could be used, but it is probably worth looking into  

##### 🔗 Links:

[Latent Diffusion Models arxiv](https://arxiv.org/pdf/2112.10752.pdf) / [Latent Diffusion Models GitHub](https://github.com/CompVis/latent-diffusion)

##### 💸 If you enjoy Casual GAN Papers, consider tipping on KoFi:  

<script type='text/javascript' src='https://storage.ko-fi.com/cdn/widget/Widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Tip Casual GAN Papers', '#e02863', 'V7V7BXBHV');kofiwidget2.draw();</script> 

***

##### 🔥 Read More Popular AI Paper Summaries:
- [Mask-conditioned text-to-image - Make-A-Scene Explained]({% post_url 2022-4-12-Make-A-Scene-explained %})
- [How to generate videos from static images - FILM Explained]({% post_url 2022-2-26-FILM-explained %})
- [Improved Text-to-Image Diffusion Models - GLIDE Explained ]({% post_url 2022-4-25-GLIDE-explained %})

##### 👋 Thanks for reading!
*If you found this paper digest useful, **subscribe** and **share** the post with your friends and colleagues to support Casual GAN Papers!*

*[Discuss](https://t.me/casual_gans_chat) the paper*

***

By: [@casual_gan](https://t.me/joinchat/KeutnzlvetRkZGZi)

P.S. Send me paper suggestions for future posts
[@KirillDemochkin](mailto:kdemochkin@gmail.com)!
